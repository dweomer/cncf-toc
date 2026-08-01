# 4 Storage stack / layers

Any storage solution is composed of multiple layers of functionality that define how data is stored, retrieved, protected and interacts with an application, orchestrator and/or operating system. Each of these layers has the potential to influence and impact one or more of the attributes of a storage solution including availability, scalability, consistency and durability.

## 4.1 Storage Topology

The storage topology of a storage system defines the different arrangements of storage and compute devices and the data links between them. The topology can influence multiple attributes, including:

- Availability - in terms of the speed of failover and reconvergence following a component failure
- Performance - in terms of both latency and throughput
- Scalability - different topologies are optimised to scale in different directions (e.g. scaling vertically vs horizontally, sometimes referred to as scaling up vs out)
- Consistency and Durability - the topology often defines the consistency delay as well as the data protection options that are possible

### 4.1.1 Centralised

Storage systems that are deployed in a centralised topology tend to be formed of fewer nodes that maintain a tightly coupled state. Often the architecture is dependent on vendor specific hardware technology for intra-controller communication, configuration and data plane activity (such as shared memory, cache synchronisation or shared data buses).

This type of storage is typically accessed by compute nodes via network interfaces where a number of clients consume storage from a small number of centralised nodes. Centralised storage is often characterised by scale-up topologies (or scaling vertically) and is usually more consistent than distributed storage.

As a result of the small number of nodes (often just a single pair), the latency required to maintain data protection and sync consistency is very low and many block based systems use this architecture as a result.

It can be hard to scale such a system horizontally as the requirement for a tightly coupled state limits the number of nodes that can be supported.

### 4.1.2 Distributed

Storage systems that use a distributed topology tend to have a stronger focus on software solutions vs hardware solutions. A software solution will often be implemented with a "shared nothing" architecture where data needs to be synced across more than one node over a standard network connection.

Some distributed solutions are accessed directly in a scale out manner which allows many clients to access many server nodes in parallel. Other distributed storage systems layer other protocols on top to enable compatibility with existing environments or access transports (e.g. NFS or iSCSI) which may limit the overall scalability.

Different distributed architectures have different focuses and make design decisions that may favour performance, scalability, durability, availability or consistency. Distributed topologies typically offer better horizontal scalability as data can be distributed across many more nodes and can support many clients. This can result in systems that are also more complex to deploy and operate and therefore benefit from additional automation.

### 4.1.3 Sharded

Sharding is a process where a dataset or workload is partitioned based on ranges of keys across multiple instances. The shard can be computed by using the key to determine which node to access based on a range, a hash or other algorithms. Sharding is primarily used as a method for scaling database architectures.

Sharded systems provide a way to scale a storage system for both capacity and compute capability. Workload is distributed across the shards in the system allowing workloads to scale horizontally.

Sharded systems can increase operational complexity and care needs to be taken to ensure that the algorithm used to distribute the keys is balanced to the specific workload or dataset. Managing availability can also be more challenging as systems may experience more complex or partial failure modes where only parts of a data set are impacted by individual node or network failures. Although sharding enables scale, the performance of any particular request will be limited to the performance of the specific node that the shard is located on, and it is possible for individual shards to become "hot" or overloaded. Rebalancing shards when scaling a cluster can also be complex.

### 4.1.4 Hyperconverged

Hyperconverged topologies combine application as well as storage workloads onto the same nodes. Multiple nodes can be clustered together creating a common resource pool which is shared for both computer workloads and storage functionality. In hyperconverged topologies, the storage layer is usually implemented as a software component on commodity compute nodes and typically shares the same attributes of a distributed system.

Hyperconverged topologies tend to be selected to maximise flexibility as the storage system can grow with the compute workload.

A reduction in the separation of concerns in hyperconverged systems can have an impact on security and operational complexity as maintenance operations, or any node failure, not only impacts the workload on that node, but also the underlying storage system.

## 4.2 Data Protection

A key function of any storage system is to provide protection of the data that is being persisted in the system or service. This is often implemented as a transparent layer in the system.

### 4.2.1 RAID: Striping, Mirrors & Parity

RAID (Redundant Array of Independent Disks) uses techniques such as striping, mirroring and parity to distribute and provide redundancy for data across a set of disks:

**Striping**: this is a process where data is spread evenly across two or more disks. Striping in itself does not provide redundancy or fault tolerance - in fact, striping on it's own increases the chance of failure as a failure of any of the individual disks will typically result in unavailability of the whole dataset. Instead, striping is used to increase performance of a number of data protection functions by distributing the load across more disks such that a workload is not limited to the performance of a single component.

**Mirrors**: a mirror maintains an identical copy of a dataset across two disks. This configuration enables the availability of data to continue in the event of a disk or component failure. It is also possible to mirror multiple disks for additional redundancy.

**Parity**: when using parity, a data set is distributed across a number of disks that are grouped together. For each unit of data (typically a block, but can be as small as a byte), an algorithm is used to generate an additional set of parity data which is stored alongside the data. In the event of the failure of any individual disk, then the missing data can be regenerated using the remaining data segments and the parity data. The benefit of using parity over mirrors is that parity does not require a full copy of the data set and can therefore implement data protection with less overhead in terms of disks or backend storage capacity. The capacity benefit comes at the expense of performance overhead and using parity for data protection can impact latency and throughput.

There are four main RAID levels in common use today:

**RAID0**: this uses a simple stripe data set and is typically only used when the only consideration is performance, as RAID0 datasets do not have any redundancy.

**RAID1**: a RAID1 dataset consists of a mirror. In a RAID1 dataset, read performance can be increased as the reads can be striped across both sets of the mirror, but writes will only be as fast as an individual disk as the write needs to be written to both disks in parallel. Any data set will also consume double the capacity on the disks as a result.

**RAID5**: this implements a distributed dataset with distributed parity. Each block is distributed across the disks in a RAID5 set together with the additional parity. This method provides a good balance between capacity utilisation and redundancy: the parity ensures that data can be recovered or rebuilt if any single disk fails, but as data is not mirrored, the capacity lost to redundancy is only 1/x (where x is the number of disks in the raid set). Performance of read operations is similar to a striped dataset and can utilise the combined speed of all the disks in the dataset, but write performance has a high penalty: every write or update needs to touch every disk in the RAID set. A RAID5 set can only survive a single disk failure and care must be taken to ensure that a rebuild of the data is completed before a second failure occurs.

**RAID6**: RAID6 is also a distributed dataset with distributed parity with the difference that two sets of parity are generated. This allows for two concurrent disk failures to occur in a RAID6 set without impacting the availability of data. RAID6 has similar disk performance characteristics of RAID5, but imposes a higher CPU workload - this is due to the calculation of the second parity set.

RAID sets can be striped to further spread the data across many more disks for improved performance. This is sometimes referred to as nested RAID, but more often is determined by adding a "0" to each of the RAID levels e.g. RAID10, RAID50 and RAID60 referred to stripes of RAID1, RAID5 and RAID6 respectively.

Using multiple copies of parity (such as RAID6) has become more important as the size of disks continues to grow, as the size of the disk tends to determine the time to rebuild a dataset. As a result, custom additional parity based RAID sets have been defined in some solutions (e.g. RAIDZ in ZFS) to add three or more sets of parity.

Although RAID is typically implemented within the set of disks in a specific node, it is also possible to distribute RAID across a network and implement redundancy across nodes AND disks at the same time. This is a technique used in some distributed storage systems.

### 4.2.2 Erasure Coding

Erasure coding is a method used to protect data where a data set or object is split into multiple fragments that are then encoded and stored with a configurable number of redundant parity sets. As an example a data object might be broken down into 6 data fragments and 4 parity fragments and would be referred to as (6+4) erasure coding. The ability to have many parity fragments enables very high redundancy and very high durability.

Each of the fragments can be distributed across different disks and servers/nodes in multiple locations. Erasure coding typically uses [Reed-Solomon codes](https://en.wikipedia.org/wiki/Erasure_code) (although a variety of algorithms are available with different performance/efficiency characteristics) to perform encoding and is therefore a computationally intensive process. The primary benefit of using erasure codes is the flexibility of a user configurable balance between data distribution, capacity utilisation and redundancy. As a result, erasure coding is utilised in many distributed storage systems and the primary building block for data protection and redundancy for many object stores.

One drawback of erasure coding is that the number of data fragments and the distribution across multiple nodes means that write and read operations on data objects can incur significant latency due to the network overhead as well as the computational overhead. As a result, erasure coding is best applied to large datasets which are optimised for either reducing overall capacity utilisation or improving redundancy and durability.

### 4.2.3 Replicas

Replicas are mirrored data sets that are distributed across multiple servers/nodes. A replica is a full copy of the dataset and therefore the number of replicas for a data set multiplies the capacity needed to store a particular data set. Each individual replica is usable as a standalone copy and therefore rebuild operations are extremely quick as it is both simple and can be implemented as a point to point transaction.

Replicas have a much lower compute and network distribution overhead and are therefore preferred when lower latency is important. Replicas can also be used to provide parallelized read access for some workloads.

## 4.3 Data Services

Storage systems often implement a number of data services which complement the core storage function by providing additional functionality that may be implemented at different layers of the stack.

### 4.3.1 Replication

This service provides the capability to replicate a set of data (e.g. a volume or a bucket) to improve the availability and durability of the data. Note - this data service is often separate from the core data protection function (such as mirrors or replicas) and is generally used to replicate data between independent storage systems often in different locations.

Replication can be performed synchronously where a request to persist data is only acknowledged to the application after the replica target has also acknowledged it. This provides a strongly consistent model with a low time to recover from failure, but can impact latency and performance. Due to the time taken for data to traverse a network, latency increases with distance, and synchronous replication is typically only feasible when the source and target systems are within 100km of each other.

Replication can also be performed asynchronously where data to be replicated is queued and is transferred to the target replica out of band of the actual storage persist operations. This means that asynchronous replication is eventually consistent and has a lower impact to overall performance. Asynchronous replication can support replication over long distances but adequate bandwidth must be available to be able to transfer the deltas that change between the source and target system in an acceptable time frame.

### 4.3.2 Snapshots and Point in Time (PIT) copies

Snapshots or point in time copies of data improve the availability of a dataset and provide the capability to backup and further protect the data. A snapshot is a view of the data set at a given point in time (when the snapshot was taken) and this provides the ability to access this data consistently at that data point.

Snapshots can be implemented in a space efficient manner using techniques such as "copy-on-write" (COW), which provides a virtualisation layer where snapshots only contain the delta between what was written since the snapshot was taken and the original data set. This provides the capability of taking multiple snapshots at different intervals whilst minimising the amount of capacity needed to store the snapshots.

Many storage systems also allow the creation of a point in time copy of the data which includes a full copy of the data set. This is often referred to as a "clone" and utilises additional capacity in the storage system, but creates an independent copy of the data set. This can be useful when the data set is to be utilised in a manner which might impact the performance of the original data set if a snapshot was taken.

Processing snapshots and data copies often means maintaining data structures and metadata which may impact the CPU, memory or disk overhead and performance. Whilst the creation of space efficient snapshots is often a low overhead function, the creation of a clone requires the creation of a full copy of the data set which will impact performance and utilise bandwidth to move the data from the original data set to the copy.

## 4.4 Data Reduction

Storage systems can use a number of techniques to reduce the size of data stored. This improves the capacity efficiency of the underlying physical storage by using data compression and or deduplication. Storage systems implement data reduction with various granularities based on implementation, and data reduction can be applied at a block, file, object, local or global level.

Data compression provides a method to efficiently encode data to remove redundant or repetitive patterns such that the encoded data consumes less space.

Deduplication typically uses a method such as a hash to determine duplicate data and then uses that reference to store a link to the data rather than storing multiple copies of the data.

Many applications can benefit from data reduction techniques, but some types of data that are encrypted or already compressed (e.g. images or videos) do not benefit from data reduction.

Data reduction can impact performance and scalability attributes of a storage system. In general, data reduction will add additional compute overhead which will impact latency and throughput. In some cases, data reduction may improve performance, especially when the limiting factor is the performance of the physical storage or the network.

## 4.5 Encryption

Storage systems can provide methods to ensure that data is protected by encrypting data. Data encryption can be implemented for data in transit or data at rest and can ensure that the encryption function is implemented independently to the application.

Encryption can have an impact on performance as it implies a compute overhead, but acceleration options are available on many systems which can reduce the overhead.

Encryption services can be implemented for data in transit (protecting data in the network) and for data at rest (protecting data on disk). The encryption may be implemented in the storage client or storage server and granularity of the encryption will vary by system (e.g. per volume, per group or global keys).

The encryption function will often depend on integration with a key management system which may add complexity to a storage system.

## 4.6 Physical / Non-Volatile Layer – terminology

Storage systems will ultimately persist data on some form of physical storage layer which is generally non-volatile. The choice of the physical layer impacts the overall performance of the storage system and defines the long term durability of the stored dataset.

Cloud services often use similar terminology for service classes to define the performance characteristics and SLAs of the service.

Some of the most commonly used systems include:

- **Spinning / magnetic disk (e.g. SATA, SAS & SCSI)** - magnetic media are traditional harddisks and are mechanical devices in that they have spinning magnetic disks that are read by a read/write head. Latency is a combination of the rotational latency of the disk, the seek time for the head to move into place to read/write the data and the electronics/bus. SATA, SAS and SCSI are transports used by the operating system to access the device through a host bus adapter (HBA). Latency per operation is measured in a number of milliseconds and throughput is generally under 250MB/sec. Magnetic media generally offers the lowest cost per GB of capacity.
- **SSD (with traditional interfaces such as SATA, SAS or SCSI)** - a solid state disk does not have any moving parts and stores data in non-volatile memory (typically some type of flash). This allows for much lower latency operations - typically small fractions of a millisecond and allows for tens of thousands of I/O operations per second. Throughput is usually limited only to the transport utilised and measured in hundreds of MB per second. Different classes of flash are available which impact the performance as well as the durability - SSD flash wears out and can fail after a given number of cell overwrites. Storage systems that are optimised for SSDs will therefore generally attempt to minimise write amplification to minimise wear.
- **Non Volatile Memory (e.g. SSD/NVMe)** - flash based devices are generally faster than the current generation of transports. NVMe is a faster transport that minimises the protocol overhead by treating the flash more like memory where data can be accessed randomly rather than in block format as defined in disk transport protocols like SCSI. This allows for much lower latency - typically a few tens of microseconds - and much faster throughput - typically measured in GB per second.
