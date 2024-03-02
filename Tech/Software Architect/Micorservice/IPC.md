## Styles
The various interaction styles can be characterized in two dimensions: one-to-one vs one-to-many and synchronous vs asynchronous.

||one-to-one|one-to-many|
|---|---|---| |Synchronous|Request/response|—|
|Asynchronous|Asynchronous request/response One-way notifications|Publish/subscribe Publish/async responses|

The following are the different types of one-to-one interactions:

- **_Request/response_**—A service client makes a request to a service and waits for a response. The client expects the response to arrive in a timely fashion. It might event block while waiting. This is an interaction style that generally results in services being tightly coupled.
- **_Asynchronous request/response_**—A service client sends a request to a service, which replies asynchronously. The client doesn’t block while waiting, because the service might not send the response for a long time.
- **_One-way notifications_**—A service client sends a request to a service, but no reply is expected or sent.

The following are the different types of one-to-many interactions:

- **_Publish/subscribe_**—A client publishes a notification message, which is consumed by zero or more interested services.
- **_Publish/async responses_**—A client publishes a request message and then waits for a certain amount of time for responses from interested services.

## Message Broker Channel
Each message broker implements the message channel concept in a different way.  JMS message brokers such as ActiveMQ have queues and topics. AMQP-based message brokers such as RabbitMQ have exchanges and queues. Apache Kafka has topics, AWS Kinesis has streams, and AWS SQS has queues. What’s more, some message brokers offer more flexible messaging than the message and channels abstraction described in this chapter.


|Message broker|Point-to-point channel|Publish-subscribe channel|
|---|---|---|
|JMS|Queue|Topic|
|Apache Kafka|Topic|Topic|
|AMQP-based brokers, such as RabbitMQ|Exchange + Queue|Fanout exchange and a queue per consumer|
|AWS Kinesis|Stream|Stream|
|AWS SQS|Queue|—|

## How message queue handling ordering
1. A sharded channel consists of two or more shards, each of which behaves like a channel.
2. The sender specifies a shard key in the message’s header, which is typically an arbitrary string or sequence of bytes. The message broker uses a shard key to assign the message to a particular shard/partition. It might, for example, select the shard by computing the hash of the shard key modulo the number of shards.
3. The messaging broker groups together multiple instances of a receiver and treats them as the same logical receiver. Apache Kafka, for example, uses the term _consumer group_. The message broker assigns each shard to a single receiver. It reassigns shards when receivers start up and shut down.

Scaling consumers while preserving message ordering by using a sharded (partitioned) message channel. The sender includes the shard key in the message. The message broker writes the message to a shard determined by the shard key. The message broker assigns each partition to an instance of the replicated receiver.

![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/03fig11_alt.jpg)

## How to handle duplicate message
There are a couple of different ways to handle duplicate messages:

- Write idempotent message handlers.
- Track messages and discard duplicates.
### WRITING IDEMPOTENT MESSAGE HANDLERS

If the application logic that processes messages is idempotent, then duplicate messages are harmless. Application logic is _idempotent_ if calling it multiple times with the same input values has no additional effect. For instance, cancelling an already-cancelled order is an idempotent operation. So is creating an order with a client-supplied ID. An idempotent message handler can be safely executed multiple times, provided that the message broker preserves ordering when redelivering messages.

Unfortunately, application logic is often not idempotent. Or you may be using a message broker that doesn’t preserve ordering when redelivering messages. Duplicate or out-of-order messages can cause bugs. In this situation, you must write message handlers that track messages and discard duplicate messages.

### TRACKING MESSAGES AND DISCARDING DUPLICATES

A simple solution is for a message consumer to track the messages that it has processed using the `message id` and discard any duplicates


![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/03fig12_alt.jpg)

When a consumer handles a message, it records the `message id` in the database table as part of the transaction that creates and updates business entities. In this example, the consumer inserts a row containing the `message id` into a `PROCESSED_MESSAGES` table. If a message is a duplicate, the `INSERT` will fail and the consumer can discard the message.

Another option is for a message handler to record `message id`s in an application table instead of a dedicated table. This approach is particularly useful when using a NoSQL database that has a limited transaction model, so it doesn’t support updating two tables as part of a database transaction

## Transactional messaging
### USING A DATABASE TABLE AS A MESSAGE QUEUE

Let’s imagine that your application is using a relational database. A straightforward way to reliably publish messages is to apply the Transactional outbox pattern. This pattern uses a database table as a temporary message queue

![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/03fig13_alt.jpg)

The `OUTBOX` table acts a temporary message queue. The `MessageRelay` is a component that reads the `OUTBOX` table and publishes the messages to a message broker.

Pattern: Polling publisher

### PUBLISHING EVENTS BY APPLYING THE TRANSACTION LOG TAILING PATTERN

A sophisticated solution is for `MessageRelay` to _tail_ the database transaction log (also called the commit log). Every committed update made by an application is represented as an entry in the database’s transaction log


![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/03fig14_alt.jpg)

The `Transaction Log Miner` reads the transaction log entries. It converts each relevant log entry corresponding to an inserted message into a message and publishes that message to the message broker. This approach can be used to publish messages written to an `OUTBOX` table in an RDBMS or messages appended to records in a NoSQL database.

## Eliminating synchronous interaction
### USE ASYNCHRONOUS INTERACTION STYLES

Ideally, all interactions should be done using the asynchronous interaction styles  A client creates an order by sending a request message to the `Order Service`.

![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/03fig16_alt.jpg)

The client and the services communicate asynchronously by sending messages via messaging channels. No participant in this interaction is ever blocked waiting for a response.

### REPLICATE DATA

One way to minimize synchronous requests during request processing is to replicate data. A service maintains a replica of the data that it needs when processing requests. It keeps the replica up-to-date by subscribing to events published by the services that own the data.


![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/03fig17_alt.jpg)

`Consumer Service` and `Restaurant Service` publish events whenever their data changes. `Order Service` subscribes to those events and updates its replica.

In some situations, replicating data is a useful approach. One drawback of replication is that it can sometimes require the replication of large amounts of data, which is inefficient. Another drawback of replication is that it doesn’t solve the problem of how a service updates data owned by other services.

### FINISH PROCESSING AFTER RETURNING A RESPONSE

Another way to eliminate synchronous communication during request processing is for a service to handle a request as follows:

1. Validate the request using only the data available locally.
2. Update its database, including inserting messages into the `OUTBOX` table.
3. Return a response to its client.

While handling a request, the service doesn’t synchronously interact with any other services. Instead, it asynchronously sends messages to other services. This approach ensures that the services are loosely coupled. As you’ll learn in the next chapter, this is often implemented using a _saga_.

For example, if `Order Service` uses this approach, it creates an order in a `PENDING` state and then validates the order asynchronously by exchanging messages with other services. 

1. `Order Service` creates an Order in a `PENDING` state.
2. `Order Service` returns a response to its client containing the order ID.
3. `Order Service` sends a `ValidateConsumerInfo` message to `Consumer Service`.
    
    ![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/03fig18_alt.jpg)
    
4. `Order Service` sends a `ValidateOrderDetails` message to `Restaurant Service`.
5. `Consumer Service` receives a `ValidateConsumerInfo` message, verifies the consumer can place an order, and sends a `ConsumerValidated` message to `Order Service`.
6. `Restaurant Service` receives a `ValidateOrderDetails` message, verifies the menu item are valid and that the restaurant can deliver to the order’s delivery address, and sends an `OrderDetailsValidated` message to `Order Service`.
7. `Order Service` receives `ConsumerValidated` and `OrderDetailsValidated` and changes the state of the order to `VALIDATED`.
8. ...

`Order Service` can receive the `ConsumerValidated` and `OrderDetailsValidated` messages in either order. It keeps track of which message it receives first by changing the state of the order. If it receives the `ConsumerValidated` first, it changes the state of the order to `CONSUMER_VALIDATED`, whereas if it receives the `OrderDetailsValidated` message first, it changes its state to `ORDER_DETAILS_VALIDATED`. `Order Service` changes the state of the `Order` to `VALIDATED` when it receives the other message.

After the Order has been validated, `Order Service` completes the rest of the order-creation process, discussed in the next chapter. What’s nice about this approach is that even if `Consumer Service` is down, for example, `Order Service` still creates orders and responds to its clients. Eventually, `Consumer Service` will come back up and process any queued messages, and orders will be validated.

A drawback of a service responding before fully processing a request is that it makes the client more complex