---
tags:
  - Architect
  - Microservice
---
## User Story Example
```
Given a consumer
  And a restaurant
  And a delivery address/time that can be served by that restaurant
  And an order total that meets the restaurant's order minimum
When the consumer places an order for the restaurant
	Then consumer's credit card is authorized
  And an order is created in the PENDING_ACCEPTANCE state
  And the order is associated with the consumer
  And the order is associated with the restaurant
```

```
Given an order that is in the PENDING_ACCEPTANCE state
  and a courier that is available to deliver the order
When a restaurant accepts an order with a promise to prepare by a particular
     time
Then the state of the order is changed to ACCEPTED
  And the order's promiseByTime is updated to the promised time
  And the courier is assigned to deliver the order
```

## Overview
![[Pasted image 20231126115903.png]]
## Step 1 Identify system operations
![[Pasted image 20231126115946.png]]
A domain model is created using standard techniques such as analyzing the nouns in the stories and scenarios and talking to the domain experts.
### CREATING A HIGH-LEVEL DOMAIN MODEL

![[Pasted image 20231126120137.png]]
The responsibilities of each class are as follows:

- **`Consumer`**—A consumer who places orders.
- **`Order`**—An order placed by a consumer. It describes the order and tracks its status.
- **`OrderLineItem`**—A line item of an `Order`.
- **`DeliveryInfo`**—The time and place to deliver an order.
- **`Restaurant`**—A restaurant that prepares orders for delivery to consumers.
- **`MenuItem`**—An item on the restaurant’s menu.
- **`Courier`**—A courier who deliver orders to consumers. It tracks the availability of the courier and their current location.
- **`Address`**—The address of a `Consumer` or a `Restaurant`.
- **`Location`**—The latitude and longitude of a `Courier`.

### DEFINING SYSTEM OPERATIONS

A good starting point for identifying system commands is to analyze the verbs in the user stories and scenarios. Consider, for example, the `Place Order` story. It clearly suggests that the system must provide a `Create Order` operation. Many other stories individually map directly to system commands


|Actor|Story|Command|Description|
|---|---|---|---|
|Consumer|Create Order|createOrder()|Creates an order|
|Restaurant|Accept Order|acceptOrder()|Indicates that the restaurant has accepted the order and is committed to preparing it by the indicated time|
|Restaurant|Order Ready for Pickup|noteOrderReadyForPickup()|Indicates that the order is ready for pickup|
|Courier|Update Location|noteUpdatedLocation()|Updates the current location of the courier|
|Courier|Delivery picked up|noteDeliveryPickedUp()|Indicates that the courier has picked up the order|
|Courier|Delivery delivered|noteDeliveryDelivered()|Indicates that the courier has delivered the order|

A command has a specification that defines its parameters, return value, and behavior in terms of the domain model classes. The behavior specification consists of preconditions that must be true when the operation is invoked, and post-conditions that are true after the operation is invoked. Here, for example, is the specification of the `createOrder()` system operation:

|   |   |
|---|---|
|Operation|createOrder (consumer id, payment method, delivery address, delivery time, restaurant id, order line items)|
|Returns|orderId, ...|
|Preconditions|- The consumer exists and can place orders.<br>- The line items correspond to the restaurant’s menu items.<br>- The delivery address and time can be serviced by the restaurant.|
|Post-conditions|- The consumer’s credit card was authorized for the order total.<br>- An order was created in the PENDING_ACCEPTANCE state.|

The preconditions mirror the _givens_ in the `Place Order` user scenario described earlier. The post-conditions mirror the _thens_ from the scenario. When a system operation is invoked it will verify the preconditions and perform the actions required to make the post-conditions true.

Here’s the specification of the `acceptOrder()` system operation:

|   |   |
|---|---|
|Operation|acceptOrder(restaurantId, orderId, readyByTime)|
|Returns|—|
|Preconditions|- The order.status is PENDING_ACCEPTANCE.<br>- A courier is available to deliver the order.|
|Post-conditions|- The order.status was changed to ACCEPTED.<br>- The order.readyByTime was changed to the readyByTime.<br>- The courier was assigned to deliver the order.|
## Step 2 Identify Services

Decompose by subdomain and Decompose by business capability are the two main patterns for defining an application’s microservice architecture

### Business Capability
![[Pasted image 20231126115831.png]]

### Subdomain (Following DDD)
![[Pasted image 20231126115821.png]]

## Step 3 Defining service APIs
The starting point for defining the service APIs is to map each system operation to a service.

After that, we decide whether a service needs to collaborate with others to implement a system operation.

If collaboration is required, we then determine what APIs those other services must provide in order to support the collaboration.

|Service|Operations|
|---|---|
|Consumer Service|createConsumer()|
|Order Service|createOrder()|
|Restaurant Service|findAvailableRestaurants()|
|Kitchen Service|- acceptOrder()<br>- noteOrderReadyForPickup()|
|Delivery Service|- noteUpdatedLocation()<br>- noteDeliveryPickedUp()<br>- noteDeliveryDelivered()|

After having assigned operations to services, the next step is to decide how the services collaborate in order to handle each system operation.

|Service|Operations|Collaborators|
|---|---|---|
|Consumer Service|verifyConsumerDetails()|—|
|Order Service|createOrder()|- Consumer Service verifyConsumerDetails()<br>- Restaurant Service verifyOrderDetails()<br>- Kitchen Service createTicket()<br>- Accounting Service authorizeCard()|
|Restaurant Service|- findAvailableRestaurants()<br>- verifyOrderDetails()|—|
|Kitchen Service|- createTicket()<br>- acceptOrder()<br>- noteOrderReadyForPickup()|- Delivery Service scheduleDelivery()|
|Delivery Service|- scheduleDelivery()<br>- noteUpdatedLocation()<br>- noteDeliveryPickedUp()<br>- noteDeliveryDelivered()|—|
|Accounting Service|- authorizeCard()|—|