---
tags: Kafka, Backend
---
# Broker

When you deploy a Kafka Broker, you don’t just deploy a single machine. You deployed multiple machines, which consist of clusters. With all those machines, Kafka uses a distributed config system, Zookeeper, to manage and coordinate its node and cluster. ZooKeeper will notify the leader in the cluster to ensure that it is not dead. If the leader dies, ZooKeeper will help elect a new leader to ensure the broker is highly available.

![](https://miro.medium.com/v2/resize:fit:1400/1*2W_uVKQ-gOptjPPNxSXVWw.png)

Big Picture of Broker

I often got a question from a system design interview when designing applications with Kafka, “ _If everything needs to go through Kafka, isn’t Kafka a single point of failure?”_ The answer is No, because, in practice, you will need to spin up multiple replicas on a broker.

# Consumer

Consumers in a Kafka read data and consumer messages from Topics they subscribed to. The topic is a way to separate each record into categories. Multiple consumers can consume from that category. Each topic will have its partition, which helps distribute the load and increase availability and throughput.

Consumers keep track of their offsets in a special topic called `__consumer_offsets`. Thus, record offset helps the consumer to pick up where they left off if they die. Thus, it doesn't need to consume the record from the beginning of the offset. Kafka employs an approach of a dumb pipeline, smart clients meaning that Kafka brokers don't know anything about consumer offsets.

![](https://miro.medium.com/v2/resize:fit:1400/1*St7S__3LA7TGZ65iKa8P8w.png)

High-Level Illustration of Consumer And offset topic

# Producer

Kafka producers message directly to the corresponding topic partitions. It also serializes, compresses, and loads balance data among brokers through partitioning.

# What is the Consumer Group in Kafka?

When you have a system that consists of many producers to many consumers, you will require a way to have a topic consumed by multiple consumers without stepping on each other’s toes. Kafka achieved that by separating the consumers into its group. Kafka uses group id to identify all the consumers in the same group. When a consumer is in the same group, every record from the topic will only be delivered to that consumer.

![](https://miro.medium.com/v2/resize:fit:1400/1*glrSa7CSPVKoj7Nx_DXu6w.png)

An Illustration of Consumer Group

# Consumer Group Gotchas

Remember that topic consists of one or more partitions? Aside from the purpose of increasing throughput and availability, Kafka also ensures that only one consumer consumes each partition in that group.

When a consumer is started, it will join a consumer group (this happens under the hood).

![](https://miro.medium.com/v2/resize:fit:1400/1*FKb-_1FwDgjiHNBULCT7WA.png)

One Consumer is assigned to two partitions

Now, what happens if you increase the consumer to two?

Each consumer will be consuming one partition.

![](https://miro.medium.com/v2/resize:fit:1400/1*ArV8KDj-oPSPH2p0ijQkKA.png)

Two Consumers are assigned to different partitions. Kafka automatically distributes the partition to each consumer evenly.

Again, what happens if you have more consumers in the group than the partition provided by the broker?

![](https://miro.medium.com/v2/resize:fit:1400/1*961p9_eIWh9PHFYaRT2hjw.png)

Illustration of what happen to the third consumer

That’s right! You guessed it. That one consumer will be sitting there idle.

If you want to increase the consumer count, you will need to plan accordingly by also increasing the number of partitions. Thus, the partition is a unit of parallelism from the consumers’ perspective. Why?

You know the reason now. 😏

Now that you know briefly what a consumer group is, let’s go into some real-life examples of using a consumer group to achieve some non-functional requirements from business ask.

# Example and Use Cases of Changing Consumer Group

# “My message needs to be Processed in Order.”

We have order events, `ORDER_RECEIVED, ORDER_CREATED, ORDER _CANCELED`. Kafka Producer will publish the event into a single Order Topic distributed across the partitions. On the other end, multiple consumers will poll messages from the topic in parallel and process each one of the events. We want the message to be processed in order with such a system. An `ORDER_CANCELED` event on an ordered 123 can only be processed after `ORDER_CREATED` since it makes no sense.

How can we guarantee message ordering in Kafka?

Remember that each partition can only be consumed by one consumer? That is key! Because partition and consumer are 1:1 relationships, putting the same message into the same partition will consume it by one consumer.

The message partition key determines a message partition. In the above scenario, to ensure the message is processed in order, we can create a hash as the partition key of the producer so that each time the producer sends a record, it will go to the same partition. The partition key can be based on the order since the producer will produce different event types based on a single order Id. Therefore, we ensure that the same order will always be at the same partition to preserve message ordering.

# “I want also to Process multiple things in this Event, not just XYZ.”

The business use case above is that we want to broadcast the event to multiple consumers. One example is that we want to notify the user that the payment has been authorized and update the order table to mention that the order has been paid. We will require multiple consumers to receive the same event topic. We can do this by putting order and notification consumers in 2 different groups. This way, the order and notification consumers will receive the same amount of message records and won’t cause any skipped messages.

![](https://miro.medium.com/v2/resize:fit:1400/1*YVITh6OqFBCEFLlLV0wIyA.png)

Illustration of order and notification consumer group processing payment event topic

# “My App often received multiple duplicate notifications.”

The problem is the side effect of trying to have multiple consumer instances consume multiple records and process duplicate events.

For example, we don’t want multiple instances of notification service to process the payment record and send multiple “Your payment has been received” emails to the customer.

![](https://miro.medium.com/v2/resize:fit:1070/1*7nQtT-RCeJZ322KtCPwiTA.png)

Illustration on how duplicate event will be consumer in multiple consumer environment

We want exactly one of the instances to process each record. We would put all the notification service instances in one consumer group to do this. This way, we can ensure that one record will always go to only one instance within that group.

Another duplication problem occurs when the consumer re-processed the event again because of client failure while committing its record offset. Kafka lets you configure how consumers want to commit their records. You can set auto-commit to true, and your consumer will automatically commit the message once received. In this case, it is At-most-once. One of the disadvantages of at-most-once is that if the consumer dies before processing the data, that data can be gone forever.