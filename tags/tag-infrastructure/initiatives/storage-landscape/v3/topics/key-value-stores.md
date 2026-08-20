# 9 Key-Value Stores

A key-value store is a storage system designed for storing, retrieving, and managing key-value pairs. Values are identified and accessed via a key, which is similar to a [hash table](https://en.wikipedia.org/wiki/Hash_table). In a key-value store, there is no predefined schema and the value of the data is usually opaque. It is a very flexible data model because the application has complete control over what is stored in the value.

A key-value store system might store its data fully in memory, partially in memory or fully on disk. It might be only locally accessible or remotely accessible. It might only run on a single node or might be distributed and scalable. Many more complex storage systems like databases, block storage, file systems, logging systems are usually built on top of key-value stores or key-value abstraction.

## 9.1 Local Key-Value Stores

A local key-value store is usually accessed by a single application through inter-process communication or direct intra-process API calls. It stores the data in local memory or a local filesystem. The local key-value store is designed for low latency access and the ease of use and operation. Many distributed applications or distributed storage systems use one or more local key-value stores as their basic storage unit for further replication. Berkeley DB, InnoDB, LMDB, and RocksDB are the best examples of this category.

## 9.2 Remote Shared Key-value Stores

A remote shared key-value store is usually accessed by a number of applications through networking protocols (HTTP, RPC, or customized ones). It stores the data in local memory or local filesystem. The shared key-value store is designed for efficiency and flexibility. Some remote key-value stores also provide additional data structures API for the ease of use. A traditional relational database can also be used as a remote key-value store with a simple two columns (key, value) table when reliability and durability are the first priorities.

Redis, and memcached are the best examples of this category.

## 9.3 Distributed Key-value Stores

A distributed key-value store replicates its data to one or more nodes in the system for high availability and durability, and might shard its data to different replication groups for scale out. Some distributed key-value stores trade off latency or scalability for linearizability and serializability [consistency](https://jepsen.io/consistency) guarantees over the entire key-value space to reduce the risk of potential conflict updates. Some provide weaker consistency guarantees (either eventual consistency or stronger consistency within one single partition) but better latency guarantees.

Etcd, ZooKeeper, Consul, etc. provide distributed key-value store API for handling metadata or coordination. They only implement the data replication, but no sharding, to simplify the overall design and improve reliability. These systems provide strong consistency guarantees over the entire key space.

Cassandra, HBase, etc. provide distributed key-value store API for managing massive amounts of data with low latency. They are similar since they are all Wide-Row key value stores. They implement both data replication and sharding. Strong consistency can be achieved for mutations within a row or within a partition, sometimes with limited availability. They do not provide strong consistency guarantees over mutations over different partitions through the entire key space.

Spanner, CockroachDB, TiKV, YugabyteDB, FaunaDB, FoundationDB, etc., provide distributed key-value store API for managing massive amounts of data and strong consistency guarantees. They implement both the data replication and sharding features. Additionally, they implement distributed transactional protocols across multiple shards to support global transactions either through clocks (high accuracy physical clock or [HLC](https://cse.buffalo.edu/tech-reports/2014-04.pdf)) or through a single master ([Calvin](http://cs.yale.edu/homes/thomson/publications/calvin-sigmod12.pdf) or similar protocols). The distributed transaction protocol typically introduces additional latency for cross shard transactions. Even with high accuracy physical clocks, the latency can be as high as [several milliseconds](https://static.googleusercontent.com/media/research.google.com/en//archive/spanner-osdi2012.pdf).

## 9.4 Comparison

| | Local | Remote | Distributed and non-global-transactional | Distributed and global-transactional |
| --- | --- | --- | --- | --- |
| **Availability** | Limited by local components failures | Limited by remote components failures | Partial failures do not affect availability or only limited key-space | Partial failures do not affect availability or only limited key-space |
| **Scalability** | Limited by local resources | Limited by remote resources | Scale out as adding more capacities | Scale out as adding more capacities. API scalability is often limited by a single-master. |
| **Global consistency** | Strong | Strong | Weak | Strong |
| **Durability** | Limited by local storage failure | Limited by remote components failures | Tolerant to partial failures | Tolerant to partial failures |
| **Performance** | Limited by I/O access latency | Limited by I/O access latency and network latency | Limited by I/O access latency and network latency | Limited by I/O access latency, network latency, and usually a single-master. Multiple rounds of network latency for cross shards transactions. |
