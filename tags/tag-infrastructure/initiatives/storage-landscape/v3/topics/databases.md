# 10 Databases

In the past, the term "database" used to be synonymous to a relational database. However, there are now other systems that get categorized as databases even though they don't strictly satisfy the properties of a relational database. In particular, there are many upcoming NewSQL systems, and there are also specialized ones like Graph databases. Similarly, existing relational databases such as PostgreSQL and MySQL have been going in the opposite direction allowing storing data without a fixed schema.

## 10.1 Functionality and Backing Stores

Databases have some advanced functionality over what one would expect of a traditional key-value store. A database typically has some of the following characteristics (but not necessarily all):

- ACID Transactions (Atomicity, Consistency, Isolation, and Durability)
- Secondary indexes
- Relationships across different pieces of data and the ability to join them on-the-fly
- A query language to fetch and (or) mutate the data. The most popular of these is SQL.

We are also aware that the lines are blurring as many key-value systems are starting to support some of the above features.

Many databases allow one to configure their backing store as an external file system or block storage. In such cases, the trade-offs are the same as that of a Key-Value store. Essentially, the comparisons made in [section 9.4](key-value-stores.md#94-comparison) also apply to such systems.

## 10.2 Cloud Native Databases

Not all databases are cloud-native. Therefore, caution must be used before running them in a cloud environment like Kubernetes. The major areas of concern are:

- the life-cycle and mobility of a Kubernetes Pod,
- the ephemeral local storage, and
- the added latency of a remotely mounted volume.

A number of CNCF projects are operators for databases like PostgreSQL or MySQL, and provide advanced capabilities to manage the deployment of the database with complex topology support, as well as handling day 2 operations (such as upgrades), and operational functions such as failovers.

These concerns can typically be addressed with additional tooling like the use of proxies and orchestration systems that can react to events that some databases may not be inherently built to handle. The exact solution will differ based on the extent to which a system is sensitive to the above changes.

On the other hand, systems like [Vitess](http://vitess.io), [TiDB](https://github.com/pingcap/tidb), [YugabyteDB](https://github.com/yugabyte/yugabyte-db), [Cloud Spanner](https://cloud.google.com/spanner) and [CockroachDB](https://github.com/cockroachdb/cockroach) come with built-in proxies and orchestration. These properties make them better suited to run in cloud native environments.

## 10.3 Data Protection

It's also recommended that backups be taken regularly. Even if there is sufficient durability achieved through replication, there are other use cases where a backup comes in handy. For example, if there is a bug in the application that accidentally destroys data, one could go back to an older snapshot to recover the lost data. Some database systems have native support for continuous backup, allowing users to perform finer Point In Time Recovery operations, restoring a consistent snapshot of the database as it was immediately before the incident.

## 10.4 Database Comparison

| Topology | Stand-alone instance | Replicated DB | Sharded | Sharded and Replicated |
| --- | --- | --- | --- | --- |
| **Example** | Individual relational database instance | Master-Replica or Multi-master deployments | Shard a subset of records per instance, behind a front-end router | Cloud Native Databases |
| **Availability** | Limited by the availability of the single node and it's network connection. | Multiple replicas; failover needs to be coordinated | Sharding may lower overall availability - any one unavailable shard may make the DB unavailable. | Availability based on the number of replicas |
| **Scalability** | Requires compute and storage to scale up; capacity limited to the capabilities of a single node | Data is not distributed, but queries can be targeted at replicas; capacity limited to the capabilities of a single node | Horizontal scaling of reads, writes and capacity is possible, but sharding does solve read latency problems without replicas. | Scaling based on the sharding |
| **Consistency** | Strong | Strong | Typically strong, but asynchronous replication and eventual consistency may impact consistency. | Typically strong, but asynchronous replication and eventual consistency may impact consistency. |
| **Durability** | Dependent on capabilities of underlying Volume Storage | Durability is based on the number of replicas. A data loss event requires all n replicas to be lost. | Durability can be comparable to a stand-alone instance due to sharding - although blast radius is minimised as loss of a single shard only results in partial data loss. | Durability is based on the number of replicas. A data loss event requires all n replicas to be lost. |
| **Performance** | Dependent on memory (cache), compute and storage resources | Performance can be negatively impacted by replication overhead, especially is synchronous to facilitate strong consistency. Long running queries can be offloaded to replicas to improve transactions on master | Performance is balanced across a number of nodes. Operational complexity for sharded systems may apply. | May be either increased or decreased due to sharding and replication, depending on query types and replication strategy. |
