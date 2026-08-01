# 11 Streaming and Messaging

Streaming and messaging systems allow clients to communicate with each other by exchanging data through messages or events. The exchange typically happens through central entities called brokers that receive the messages from producers, store them, and later provide them to the consumers.

Streaming and Messaging typically refer to two different patterns of communication. Messaging - or message queueing - is a form of point-to-point communication where each message is delivered to exactly one consumer and is removed after it is consumed. Streaming - or publish-subscribe messaging - is a form of one-to-many communication where the whole stream of messages sent by the producers is delivered to many different consumers in parallel and the messages are retained for some time even after they are consumed. In practice, the line between messaging and streaming is often blurry and many systems support in some form both messaging and streaming patterns.

Streaming and messaging systems are used as remote systems shared by many clients. The clients connect to them remotely over the network using an API / protocol.

## 11.1 APIs / Protocols

The APIs (protocols) used by messaging and streaming systems are typically based on a binary protocol over TCP to achieve maximal efficiency. But in some cases, they might be HTTP-based as well (for example the Amazon AWS SQS API).

Some of the protocols are based on ISO standards such as MQTT or AMQP. Standards allow for easier interoperability between the different implementations and between different servers and clients.

However, in many cases, proprietary protocols that are owned and maintained by a single project are used. Proprietary protocols offer limited interoperability, but usually provide better APIs tailor-made for different features and are easier to use and implement. In some cases, such a proprietary protocol maintained by a single project is adopted by other projects as well. One such example is the Apache Kafka protocol that is implemented and used by many different streaming projects/products. That however has its own challenges as the supported protocol versions or features are not very clear and compatibility cannot be guaranteed.

Some programming languages provide their own set of standard APIs to abstract the wire-level protocol used by the messaging/streaming system (for example the Java-JMS API).

## 11.2 Availability and durability

Streaming and messaging systems use different approaches to achieve availability and reliability:

- **Storage-based**: Relies on the availability and durability of the underlying storage system.
- **Replication-based:** The data are replicated and stored in multiple copies within the messaging/streaming system. Replication allows availability and durability even with local storage which might lead to cost reduction and performance gains. Replication also makes it easy to configure the desired level of availability and durability by controlling the number of replicas.

## 11.3 Performance and Scalability

Streaming systems are often expected to handle very large volumes of data (throughput) with very low latency. So performance and scalability are very important aspects.

To achieve high throughput, horizontal scalability is utilized by sharding the data. The data are split into independent partitions that can be distributed across different nodes. The partitioning of the data can happen based on different criteria. It can be based on a message key to ensure that all messages with the same key end up in the same partition. It can be based on the proximity of the producer and the partition to achieve optimal network flows and reduce costs. Or it can be for example based on a round-robin algorithm to help ensure equal load on all partitions. Adding more partitions or distributing the partitions across more nodes allows an increase in the throughput while maintaining low latency.

## 11.4 Storage

Historically, streaming and messaging systems relied on filesystem-based block or shared file storage to store their data.

As a recent trend, many new streaming systems rely fully on object storage. This solution provides great throughput, scalability, unlimited storage capacity, high availability, and durability at very little cost. However, this comes usually at the cost of higher latency and is therefore suitable only for some use cases.

Older streaming systems usually offer "tiered storage" support. Tiered storage allows offloading selected (typically older) data to object storage while still using regular filesystem-based storage for the rest of the data. Tiered storage offers more flexibility compared to relying only on object storage. That makes it better suited for all use cases. However, it means that the streaming system still uses regular filesystem-based storage as well. That has often a negative impact on scalability and elasticity.
