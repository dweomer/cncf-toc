# 5 Data Access Interface

![Data Access Interface: a Container Orchestrator sits alongside a Workloads (container) layer, which consumes either Volumes (Block, Filesystem, Shared Filesystem) or an API (Object, KV, DB); both are backed by a Storage System, itself split into a Data Plane and a Control Plane that the orchestrator interacts with directly.](images/data-access-interface.png)

The data access interface defines how applications or workloads store or consume data that is persisted by the storage system or service.

The interface is an important factor in the choice of a storage solution as often, different workloads or applications will have a pre-defined or preferred access method.

Different interfaces also influence a number of attributes such as:

- availability – in terms of failover and moving access between nodes
- performance – in terms of latency and throughput
- scalability - in terms of the number of clients that can access a given pool of storage

In addition to the attributes, in practice, the choice of access interface has a large impact on the management interfaces available and therefore the ability of orchestrators to manage and provision storage. In particular, Volume interfaces currently have more mature integrations with orchestrators.

## 5.1 Data Access Interface: Volumes

### 5.1.1 Block

A block device is the fundamental building block of many volumes. A disk device is represented as a block device to an operating system and represents a contiguous set of blocks that are ultimately stored in the disk (or other non-volatile storage). Blocks are typically represented as a 4KiB unit of data to the operating system, although different disk systems may actually store blocks internally in either smaller or larger units. Read and write operations are performed in units of individual blocks.

A block device can be a representation of a local disk but can also be a representation of a virtual or remote disk that is either connected to or provided by a storage system.

Block devices are rarely consumed by applications directly and are often used as a device that underlies a filesystem. Some databases can be configured to consume raw block devices directly in order to improve performance. Permissions and access control of block devices are typically reserved to admin users of the operating system.

Further details are available in the [Block Stores](block-stores.md) section.

### 5.1.2 Filesystem

A filesystem defines how data is persisted and retrieved by the operating system, by structuring the data in terms of files and directories. A filesystem will often use a block device to persist the data to a non-volatile storage medium such as a disk.

Permission attributes in a filesystem can be allocated to both files and directories, allowing granular access to users and groups, as well as defining the type of access (e.g read, write or execute access). Some filesystems support more extended attributes that improve the flexibility and levels of control and access. Filesystems can also support locking semantics that allow an application to mark a file as locked for exclusive use. The supported locking capabilities vary between filesystems and may operate differently when used on a remote or distributed filesystem.

Filesystem code is typically run within the kernel of the operating system to maximise performance, which means that the filesystems available to an application will be dependent on the particular operating system distribution. It is also possible to run filesystems at the user level (FUSE), which are often used to provide a filesystem representation of datasets other than those stored in a native block device.

Further details are available in the [File Systems](file-systems.md) section.

### 5.1.3 Shared Filesystem

Filesystems are typically limited to an individual server or node and can therefore only be accessed by one node at a time. A shared filesystem is a filesystem that can be mounted on more than one node at a time. This provides additional flexibility and supports patterns where applications are distributed between multiple servers and need to access a common set of data.

A shared filesystem can be consumed from a point-to-point service endpoint, where a server node exposes a local filesystem to other servers - this is limited to the performance (and sometimes the availability) of a single node. Alternatively, a shared filesystem can be distributed across multiple nodes and systems in a distributed filesystem - this allows for datasets and scalability beyond what can be supported on a single node.

Clustered filesystems can provide similar functionality to shared filesystems but are rarely utilised in a cloud native context and they use shared block devices which are available on multiple nodes.

## 5.2 Data Access Interface: Application API

### 5.2.1 Object Stores

Object stores use an API to store and retrieve objects or blobs. The API for the most popular object stores utilise a HTTP interface. Object stores are typically based on a distributed architecture that is optimised for capacity, durability and scalability allowing thousands of clients to connect to PB buckets of storage.
The overhead required to commit multiple copies for availability and durability, and the use of a HTTP API tends to lead to a higher latency overhead per operation, but can maintain high levels of throughput through parallel access from multiple clients.

Further details are available in the [Object Stores](object-stores.md) section.

### 5.2.2 Key-Value Stores

A key-value store is accessed by an API and uses a key as an identifier to store and retrieve values from the store. Key-value stores can be implemented in a library, a local system or a distributed system.

Key-value stores are often used to store metadata and configuration and are often implemented with strong consistency. As a result they are often utilised as a method for storing state, configuration and indexes for distributed systems and applications.

Further details are available in the [Key-Value Stores](key-value-stores.md) section.

### 5.2.3 Databases

Databases are typically accessed through an API provided by the project or vendor. Those that offer relational features have the opportunity to build an API that conforms to an industry defined set of standards for access. Examples are Java-JDBC, Python PEP 249, Go's database package, etc.

### 5.2.4 Streaming and Messaging

Streaming and messaging systems allow clients to communicate with each other by exchanging data through messages or events. The exchange typically happens through central entities called brokers that provide the APIs to receive the messages from producers, store them, and later provide them to the consumers. Storing the messages in the brokers is important to decouple the producers from the consumers and provide availability and reliability.

The APIs are typically based on a binary protocol over TCP. But in some cases, they might be HTTP-based as well. They might be based on industry-standard protocols (such as MQTT or AMQP) or be proprietary to the project/product (such as the Apache Kafka protocol).

Further details are available in the [Streaming and Messaging](streaming-and-messaging.md) section.

## 5.3 Orchestrator, host and operating system level interactions

A number of virtualization and access layers are often overlaid or interposed on a Data Access Interface as part of the integration of the storage solution into an orchestrated environment, and can influence Availability, Scalability and Performance of the overall end to end solution.

Often a hypervisor may also be providing access to resources and may be performing a variety of functions including mapping storage resources, pooling multiple resources which are shared between workloads, managing connectivity to resources and handling failover and data protection functions.

### 5.3.1 Volumes

Some interactions that may apply to volume access interfaces include:

- A volume manager (e.g. LVM) which may provide functionality to pool resources, provide data protection and even take an active role in failover and recovery
- Bind mounts and overlay filesystems which provide functionality to layer filesystems and image layers to provide integration with orchestrators and container runtimes.

### 5.3.2 Application API

Some interactions that may apply to application API interfaces include:

- Discovery to provide functionality to identify resources in a cluster or a network
- Meshes, ingress end-points and load balancers that can provide functionality to route requests to store and retrieve data based on content or resource availability

## 5.4 Comparison between Object Stores, File Systems and Block Stores

| Data Access Interface | Most suited | Least suited |
| --- | --- | --- |
| Block | Availability; Low latency performance; Good throughput performance for individual workloads | Capacity scaling; Sharing data with multiple workloads simultaneously |
| Filesystem | Sharing data with multiple workloads simultaneously; Optimised throughput for aggregated workloads | Strong file locking integrity when filesystems are shared |
| Object Store | Availability; Large capacities (PB scale); Durability; Sharing data with multiple workloads simultaneously; Optimised throughput for parallelised workloads | Low Latency performance |

*\*\* The information in this table are generally accepted attributes and measurements for object stores, file systems and block stores.*

## 5.5 Comparison between Local, Remote and Distributed Systems

| | Local | Remote | Distributed |
| --- | --- | --- | --- |
| Availability | Limited by failure of components locally and ability to failover. If a node fails, the local storage is isolated to the local node. | May be limited by single points of failure. Workloads can move to another node and reconnect to the remote storage. | Clients may access numerous nodes, and any storage node failures can be mitigated. The additional complexity of distributed systems may add operational complexity which may in turn affect availability or the ability to recover errors. |
| Scalability | Limited by local architecture (1 node; typically TB) | Limited by monolithic architecture (2-16 nodes; typically 10s-100s of TB) | Scale by adding additional systems. Topology enables scale of both nodes and supported capacities. (3-1000s nodes; often supports PB) |
| Consistency | Yes (storage system implementation is easy) | Yes (storage system implementation is harder with more nodes) | Yes (storage system implementation is hardest) |
| Durability | Limited by local components (less) | Limited by monolithic architecture (more) | Scaling out to additional systems increases durability (most) |
| Performance | Limited by local components, can benefit low-latency applications (100us-5ms, GB/sec) | Similar to local, but additional overhead in network transport (500us-5ms, GB/sec) | Scaling out to additional systems increases performance (500us-5ms, TB/sec) |

*\*\* The information in this table are generally accepted attributes and measurements among local, remote, and distributed storage systems.*
