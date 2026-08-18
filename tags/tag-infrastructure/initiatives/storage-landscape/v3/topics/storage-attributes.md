# 3 Attributes of a storage interface or system

Storage systems and services have a variety of interfaces which are suitable for different use cases and tend to be composed of multiple layers which each impact different attributes of the system.

When choosing an overall storage solution, the different attributes of the desired solution need to be considered.

It is important to note that different storage systems are built with different design objectives, and may be architected to optimise for one or more storage attributes which may in turn impact another storage attribute.

## 3.1 Availability

Availability of a storage system defines the ability to access the data during failure conditions. The failures may be due to failures in the storage media, transport, controller or any other component in the system.

Availability defines how access to the data continues during a failure condition and also how access to the data is re-routed (or failed-over) to another access node in the event that the node that is accessing the data is unavailable.

The availability attribute can sometimes be referred to as a Recovery Time Objective (RTO) after a failure i.e. the time between a failure occurring and service being recovered.

Availability can be measured in Uptime as a % of availability (e.g. 99.9% uptime) as well as MTTF (mean time to failure) or MTTR (mean time to repair) which are measured in units of time.

## 3.2 Scalability

Scalability of a storage system can be measured by a number of criteria. Different criteria may be important for different use cases and each defines a set of architectural patterns that will need to be implemented in a storage system.

Criteria used to measure scalability include:

A. the ability to scale the number of clients that can access the storage system
B. the ability to scale throughput (e.g. MB/sec) or number of operations (e.g. per second) of a single interface
C. the ability to scale the capacity, in terms of data stored, of a single deployment of the storage system/service. This could be with respect to storage volume (GB/TB/PB) and/or number of individual items.
D. ability to scale the number of components in a storage system to facilitate (a), (b), or (c)

## 3.3 Performance

Similar to scalability, the performance of a storage system can be measured against different criteria, the relative importance of each depending on the use case.

Performance of a storage system is typically measured in terms of one or more of:

- Average and worst-case latency to perform storage operations to files, disk blocks or objects.
- the number of storage operations that are possible per second
- the throughput of data that can be stored or retrieved per second
- The steady state and burst performance of the metrics above.

Storage performance varies greatly from the combination of workloads, storage software layers, topology, and storage systems. Benchmarks and datasheets offer some guidance but experiments with real datasets and data processing unique to a workload produces a better prediction of deployed performance.

## 3.4 Consistency

Consistency attributes of a storage system refer to the ability to access newly created data or updates to the same after it has been committed and applies to both:

- "read" operations returning the correct data after a "write", "update" or "delete" - with or without a delay.
- any delays that occur between performing the data storage operation and the data getting committed to a non-volatile store or being fully protected.

Systems that have delays between read operations returning up-to-date data, and/or delays before all data is protected after getting committed are defined as being "eventually consistent". If there are no delays, the system is defined as being "strongly consistent". Consistency is discussed in further detail in the [Appendix](appendix.md).

The consistency attribute can sometimes be referred to as a Recovery Point Objective (RPO) after a failure i.e. the amount of tolerated data loss (usually measured in time, based on the consistency delay) when a component or service in the storage system has suffered a failure.

## 3.5 Durability

Durability covers the attributes of a storage system that impact the ability for a data set to endure as opposed to just being accessible. Multiple factors can impact the durability of a storage system, including:

- the data protection layers, such as how many copies of the data are available
- the levels of redundancy of the system
- the endurance characteristics of the storage media that is holding the data (e.g. SSD vs spinning disks vs tape)
- the ability to detect corruption of data (e.g. due to component failure or wear/usage) and the ability to use data protection functions to rebuild or recover the corrupted data (sometimes referred to as "bit-rot")

## 3.6 Observability

Effective monitoring of IT infrastructure is essential for ensuring optimal performance and minimizing downtime. In modern computing environments, especially in distributed applications, monitoring every component is critical to maintaining high service levels. Failure to monitor any service or system, even for a short period, can result in significant consequences for the entire system, making monitoring a critical aspect of building and maintaining cloud-native applications. The interdependencies between different services in a distributed environment can create complex failure scenarios, making it even more important to have comprehensive observability capabilities.

In data systems, monitoring the flow and status of infrastructure is vital due to the large volumes of data that are commonplace. Efficient monitoring systems that provide early warning for potential issues can help operators prevent costly downtime and ensure the continuous flow of information. Building robust infrastructure observability frameworks that incorporate monitoring, logging, and tracing capabilities is critical to gain visibility into every aspect of the infrastructure. This enables the identification of potential bottlenecks, failures, and errors that can impact the performance of the entire system. In conclusion, observability is an essential aspect of modern IT infrastructure, and implementing effective monitoring systems is a crucial step towards ensuring optimal performance, reliability, and availability of cloud-native applications and data systems.

## 3.7 Elasticity

In modern computing environments, elasticity is a critical aspect of infrastructure management. While scalability focuses on adding resources to meet demand, elasticity goes a step further by enabling the release of resources when they are no longer required. This feature allows organizations to optimize their resource utilization, ensuring that they only pay for the services and resources they need. The ability to scale up and down quickly is a defining characteristic of elastic infrastructure, which is also referred to as on-demand infrastructure. In environments with limited resources, such as private data centers, elasticity is essential for efficient resource allocation and utilization.

In data management, elasticity means having the ability to optimize storage space usage by reclaiming storage when no longer needed. This can involve moving older data to less expensive storage tiers or archiving it to long-term storage to free up space for more critical data. Cloud infrastructure, which charges for every resource used, can benefit greatly from this feature, as it helps to prevent unnecessary costs from running services that are no longer required. An elastic infrastructure that can rapidly respond to changes in demand and adjust resource allocation accordingly is a vital component of modern IT systems, enabling organizations to maintain optimal performance, scalability, and cost-effectiveness.

*The Observability and Elasticity attributes above are adapted from the [Data on Kubernetes Whitepaper - Database Patterns](https://github.com/cncf/tag-storage/blob/master/data-on-kubernetes-whitepaper/data-on-kubernetes-whitepaper-databases.md).*

## 3.8 Instantiation & Deployment

A storage system can be deployed or instantiated on-premises or in a cloud environment in a variety of ways which defines where the storage solution or service can be deployed and/or consumed:

- **Hardware**: deployed as hardware solutions in a datacenter. This limits the portability of the application and generally means that such systems cannot be deployed in a public cloud environment
- **Software**: deployed as software components on commodity hardware, appliances or cloud instances. Software solutions tend to be more platform agnostic and can be installed both on-premises as well as cloud environments. Some software defined storage systems can also be deployed as a container and deployment can be automated by an orchestrator.
- **Cloud Services**: consumed from public cloud providers. Cloud services provide storage services in cloud environments.
