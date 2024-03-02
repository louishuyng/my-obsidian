---
tags:
  - Architect
  - Microservice
---
## What is Aggregate

Organize a domain model as a collection of aggregates, each of which is a graph of objects that can be treated as a unit.

shows the `Order` aggregate and its boundary. An `Order` aggregate consists of an `Order` entity, one or more `OrderLineItem` value objects, and other value objects such as a delivery `Address` and `PaymentInformation`.

![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/05fig05_alt.jpg)

Aggregates decompose a domain model into chunks, which are individually easier to understand. They also clarify the scope of operations such as load, update, and delete. These operations act on the entire aggregate rather than on parts of it. An aggregate is often loaded in its entirety from the database, thereby avoiding any complications of lazy loading. Deleting an aggregate removes all of its objects from a database.

## Aggregate rules
### Rule1: Reference only the aggregate root

The previous example illustrated the perils of updating `OrderLineItems` directly. The goal of the first aggregate rule is to eliminate this problem. It requires that the root entity be the only part of an aggregate that can be referenced by classes outside of the aggregate. A client can only update an aggregate by invoking a method on the aggregate root.

A service, for example, uses a repository to load an aggregate from the database and obtain a reference to the aggregate root. It updates an aggregate by invoking a method on the aggregate root. This rule ensures that the aggregate can enforce its invariant.

### Rule 2: Inter-aggregate references must use primary keys

Another rule is that aggregates reference each other by identity (for example, primary key) instead of object references. For example,  an `Order` references its `Consumer` using a `consumerId` rather than a reference to the `Consumer` object. Similarly, an `Order` references a `Restaurant` using a `restaurantId`.

The `Order` aggregate has the IDs of the `Consumer` and `Restaurant` aggregates. Within an aggregate, objects have references to one another.

![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/05fig06_alt.jpg)

This approach is quite different from traditional object modeling, which considers foreign keys in the domain model to be a design smell. 

It has a number of benefits.
- The use of identity rather than object references means that the aggregates are loosely coupled.
- It ensures that the aggregate boundaries between aggregates are well defined and avoids accidentally updating a different aggregate. Also, if an aggregate is part of another service, there isn’t a problem of object references that span services.
- This approach also simplifies persistence since the aggregate is the unit of storage. It makes it easier to store aggregates in a NoSQL database such as MongoDB. 
- It also eliminates the need for transparent lazy loading and its associated problems. Scaling the database by sharding aggregates is relatively straightforward.

### Rule 3: One transaction creates or updates one aggregate
![](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781617294549/files/OEBPS/Images/05fig07_alt.jpg)

In this example, the saga consists of three transactions.
The first transaction updates aggregate `X` in service `A`. The other two transactions are both in service `B`. One transaction updates aggregate `X`, and the other updates aggregate `Y`.

An alternative approach to maintaining consistency across multiple aggregates within a single service is to cheat and update multiple aggregates within a transaction. 
=> For example, service `B` could update aggregates `Y` and `Z` in a single transaction. This is only possible when using a database, such as an RDBMS, that supports a rich transaction model.
=> If you’re using a NoSQL database that only has simple transactions, there’s no other option except to use sagas.

