---
tags:
  - Architect
  - Microservice
---
## Benefits

### Reliably publishes domain events

A major benefit of event sourcing is that it reliably publishes events whenever the state of an aggregate changes. That’s a good foundation for an event-driven microservice architecture. Also, because each event can store the identity of the user who made the change, event sourcing provides an audit log that’s guaranteed to be accurate. The stream of events can be used for a variety of other purposes, including notifying users, application integration, analytics, and monitoring.

### Preserves the history of aggregates

Another benefit of event sourcing is that it stores the entire history of each aggregate. You can easily implement temporal queries that retrieve the past state of an aggregate. To determine the state of an aggregate at a given point in time, you fold the events that occurred up until that point. It’s straightforward, for example, to calculate the available credit of a customer at some point in the past.

### Mostly avoids the O/R impedance mismatch problem

Event sourcing persists events rather than aggregating them. Events typically have a simple, easily serializable structure. As mentioned earlier, a service can snapshot a complex aggregate by serializing a memento of its state, which adds a level of indirection between an aggregate and its serialized representation.

### Provides developers with a time machine

Event sourcing stores a history of everything that’s happened in the lifetime of an application. Imagine that the FTGO developers need to implement a new requirement to customers who added an item to their shopping cart and then removed it. A traditional application wouldn’t preserve this information, so could only market to customers who add and remove items after the feature is implemented. In contrast, an event sourcing-based application can immediately market to customers who have done this in the past. It’s as if event sourcing provides developers with a time machine for traveling to the past and implementing unanticipated requirements.

## Drawbacks
- It has a different programming model that has a learning curve.
- It has the complexity of a messaging-based application.
- Evolving events can be tricky.
- Deleting data is tricky.
- Querying the event store is challenging.****