---
tags:
  - Microservice
  - Architect
---
## What is Saga pattern?
_Sagas_ are mechanisms to maintain data consistency in a microservice architecture without having to use distributed transactions. 

You define a saga for each system command that needs to update data in multiple services. 

A saga is a sequence of local transactions. Each local transaction updates data within a single service using the familiar ACID transaction frameworks and libraries mentioned earlier.

## An example 
The `Order Service` implements the `createOrder()` operation using this saga. The saga’s first local transaction is initiated by the external request to create an order. The other five local transactions are each triggered by completion of the previous one.

![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/04fig02_alt.jpg)

This saga consists of the following local transactions:
1. **`Order Service`**—Create an `Order` in an `APPROVAL_PENDING` state.
2. **`Consumer Service`**—Verify that the consumer can place an order.
3. **`Kitchen Service`**—Validate order details and create a `Ticket` in the `CREATE_PENDING`.
4. **`Accounting Service`**—Authorize consumer’s credit card.
5. **`Kitchen Service`**—Change the state of the `Ticket` to

## The structure of a saga

The countermeasures paper mentioned in the last section defines a useful model for the structure of a saga.

- **_Compensatable transactions_**—Transactions that can potentially be rolled back using a compensating transaction.
- **_Pivot transaction_**—The go/no-go point in a saga. If the pivot transaction commits, the saga will run until completion. A pivot transaction can be a transaction that’s neither compensatable nor retriable. Alternatively, it can be the last compensatable transaction or the first retriable transaction.
- **_Retriable transactions_**—Transactions that follow the pivot transaction and are guaranteed to succeed.


![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/04fig08_alt.jpg)

In the `Create Order Saga`, the `createOrder()`, `verifyConsumerDetails()`, and `createTicket()` steps are compensatable transactions. 

The `createOrder()` and `createTicket()` transactions have compensating transactions that undo their updates. 

The `verifyConsumerDetails()` transaction is read-only, so doesn’t need a compensating transaction. The `authorizeCreditCard()` transaction is this saga’s pivot transaction. If the consumer’s credit card can be authorized, this saga is guaranteed to complete.

The `approveTicket()` and `approveOrder()` steps are retriable transactions that follow the pivot transaction.

## Countermeasures for handling the lack of isolation
### Semantic Lock
When using the semantic lock countermeasure, a saga’s compensatable transaction sets a flag in any record that it creates or updates.

The flag indicates that the record isn’t _committed_ and could potentially change. The flag can either be a lock that prevents other transactions from accessing the record or a warning that indicates that other transactions should treat that record with suspicion.

It’s cleared by either a retriable transaction—saga is completing successfully—or by a compensating transaction: the saga is rolling back.

The drawback is that the application must manage locks. It must also implement a deadlock detection algorithm that performs a rollback of a saga to break a deadlock and re-execute it. 
### Commutative Updates
One straightforward countermeasure is to design the update operations to be commutative. Operations are _commutative_ if they can be executed in any order. An `Account`’s `debit()` and `credit()` operations are commutative (if you ignore overdraft checks). This countermeasure is useful because it eliminates lost updates.

Consider, for example, a scenario where a saga needs to be rolled back after a compensatable transaction has debited (or credited) an account. The compensating transaction can simply credit (or debit) the account to undo the update. There’s no possibility of overwriting updates made by other sagas.
### Pessimistic view
Another way to deal with the lack of isolation is the _pessimistic view_ countermeasure. It reorders the steps of a saga to minimize business risk due to a dirty read.


To reduce the risk of that happening, this countermeasure would reorder the `Cancel Order Saga`:

1. **`Order Service`**—Change the state of the `Order` to cancelled.
2. **`Delivery Service`**—Cancel the delivery.
3. **`Customer Service`**—Increase the available credit.

Before
1. **`Consumer Service`**—Increase the available credit.
2. **`Order Service`**—Change the state of the `Order` to cancelled.
3. **`Delivery Service`**—Cancel the delivery.

### Reread value
The _reread value_ countermeasure prevents lost updates. A saga that uses this countermeasure rereads a record before updating it, verifies that it’s unchanged, and then updates the record. If the record has changed, the saga aborts and possibly restarts. This countermeasure is a form of the Optimistic Offline Lock pattern ([https://martinfowler.com/eaaCatalog/optimisticOfflineLock.html](https://martinfowler.com/eaaCatalog/optimisticOfflineLock.html)).

The `Create Order Saga` could use this countermeasure to handle the scenario where the `Order` is cancelled while it’s in the process of being approved. The transaction that approves the `Order` verifies that the `Order` is unchanged since it was created earlier in the saga. If it’s unchanged, the transaction approves the `Order`. But if the `Order` has been cancelled, the transaction aborts the saga, which causes its compensating transactions to be executed.
### Version file
The _version file_ countermeasure is so named because it records the operations that are performed on a record so that it can reorder them. It’s a way to turn noncommutative operations into commutative operations. To see how this countermeasure works, consider a scenario where the `Create Order Saga` executes concurrently with a `Cancel Order Saga`. Unless the sagas use the semantic lock countermeasure, it’s possible that the `Cancel Order Saga` cancels the authorization of the consumer’s credit card before the `Create Order Saga` authorizes the card.

One way for the `Accounting Service` to handle these out-of-order requests is for it to record the operations as they arrive and then execute them in the correct order. In this scenario, it would first record the `Cancel Authorization` request. Then, when the `Accounting Service` receives the subsequent `Authorize Card` request, it would notice that it had already received the `Cancel Authorization` request and skip authorizing the credit card.
### By value
The final countermeasure is the _by value_ countermeasure. It’s a strategy for selecting concurrency mechanisms based on business risk. An application that uses this countermeasure uses the properties of each request to decide between using sagas and distributed transactions. It executes low-risk requests using sagas, perhaps applying the countermeasures described in the preceding section. But it executes high-risk requests involving, for example, large amounts of money, using distributed transactions. This strategy enables an application to dynamically make trade-offs about business risk, availability, and scalability.

It’s likely that you’ll need to use one or more of these countermeasures when implementing sagas in your application. Let’s look at the detailed design and implementation of the `Create Order Saga`, which uses the semantic lock countermeasure.