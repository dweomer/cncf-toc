# 13 Appendix

## 13.1 Document History

Initially, the document was structured based on classes of storage type which are categorised by the way the storage is consumed e.g. block, file or object. This did not provide a useful way to compare and contrast their attributes and how they are utilised in production as most storage systems have many layers and are formed of multiple components. While the data access interface (like block or file) might affect how the data is consumed and how it might failover between nodes, it does not effectively define attributes such as data protection, consistency, or durability.

As a further complication, many commonly used systems are layered storage systems where, for example, a filesystem may be built on an object store (e.g. CephFS), or a block store may be built on a distributed filesystem (e.g. gluster block storage). This meant that the way the storage is accessed did not usefully define the attributes that an application cared about (such as the durability, data protection or some of the performance characteristics of the overall system), as those attributes are defined at other layers in the stack.

### Version 3 (August 2026)

Version 3 builds on the [V2 whitepaper](https://bit.ly/cncf-storage-whitepaperV2) with the following changes:

- Added a new [Streaming and Messaging](streaming-and-messaging.md) section, covering messaging/streaming APIs and protocols, availability and durability, performance and scalability, and storage.
- Added a paragraph on performance with a reference to the CNCF Performance and Benchmarking whitepaper.
- Added a new [Disaster Recovery](disaster-recovery.md) section, covering the main disaster recovery design archetypes and their RTO/RPO trade-offs.
- Updated the [Orchestration and Management Interfaces](orchestration-and-management.md) section, including updates to the Container Storage Interface (CSI) and Container Object Storage Interface (COSI) sections.
- Refreshed the [Attributes of a Storage Interface or System](storage-attributes.md) section, adding Observability, Elasticity, and Instantiation & Deployment.
- Updated the [Cloud Native Databases](databases.md) section.

## 13.2 Consensus Protocols

[Consensus protocols](https://en.wikipedia.org/wiki/Consensus_\(computer_science\)) provide reliable agreement among a group of potentially faulty distributed processes on a single data value or a replicated log. They are commonly used to decide whether to commit a data change transaction, for leader election, state machine replication, load balancing, clock synchronization and others in distributed systems. The two most popular (families of) consensus algorithms are Multi-Paxos and Raft, both of which have been formally proven correct (for practical uses, with some caveats). Both rely on a single elected leader, and (typically) agreement by a strict majority of participants (e.g. for 5 participants, at least 3 must explicitly agree). Raft is considered simpler to understand and implement than Multi-Paxos. Other ad-hoc attempts at consensus algorithms are notoriously prone to edge case failures.

### 13.2.1 Paxos

[Paxos](https://en.wikipedia.org/wiki/Paxos_\(computer_science\)) is arguably the oldest [formally studied](https://lamport.azurewebsites.net/pubs/lamport-paxos.pdf) family of consensus algorithms. It is considered highly robust when implemented properly, but [challenging to implement correctly for practical uses](https://static.googleusercontent.com/media/research.google.com/en//archive/paxos_made_live.pdf).

### 13.2.2 Raft

[Raft](https://en.wikipedia.org/wiki/Raft_\(computer_science\)) was developed about a decade after Paxos, to [address the issues mentioned above](https://web.stanford.edu/~ouster/cgi-bin/papers/raft-atc14). It has become widely used, and forms the basis of, amongst others, the popular [etcd](https://github.com/etcd-io/etcd) cloud-native key-value store, and [Consul](https://www.consul.io/docs/internals/consensus.html) distributed service mesh.

### 13.2.3 Two-phase Commit ("2PC")

[2PC](https://en.wikipedia.org/wiki/Two-phase_commit_protocol) is a specialized form of consensus protocol used for coordination between participants in a distributed atomic transaction to decide on whether to commit or abort (roll back) the transaction. 2PC is not resilient to all possible failures, and in some cases, outside (e.g. human) intervention is needed to remedy failures. Also, it is a blocking protocol. All participants block between sending in their vote (see below), and receiving the outcome of the transaction from the co-ordinator. If the co-ordinator fails permanently, participants may block indefinitely, without outside intervention. In normal, non-failure cases, the protocol consists of two phases, whence it derives its name:

1. The commit-request phase (or voting phase), in which a coordinator requests all participants to take the necessary steps for either committing or aborting the transaction and to vote, either "Yes" (on success), or "No" (on failure)
2. The commit phase, in which case the coordinator decides whether to commit (if all participants have voted "Yes") or abort, and notifies all participants accordingly.

### 13.2.4 Three-phase Commit ("3PC")

[3PC](https://en.wikipedia.org/wiki/Three-phase_commit_protocol) adds an additional phase to the 2PC protocol to address the indefinite blocking issue mentioned above. But 3PC still cannot recover from network segmentation, and due to the additional phase, requires more network round-trips, resulting in higher transaction latency.

## 13.3 Consistency, Coherence and Isolation

The above three terms are commonly used in various different contexts to mean different things in the fields of data stores and distributed systems. Without going into detail here, suffice to say that, consistency in particular, is a widely misunderstood term, so it's worth thinking twice before assuming that you understand exactly what's meant by a particular use of the term. For example, [ACID](https://en.wikipedia.org/wiki/ACID_\(computer_science\)#Characteristics) (Atomicity, Consistency, Isolation, Durability) properties and the [CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem) (concerning Consistency, Availability and Partition-tolerance) are both widely used terms, and many people assume that they understand what these terms mean. But considerably fewer people realise that "Consistency" means quite different things in those two contexts. For further details, [Wikipedia](https://en.wikipedia.org/wiki/Consistency_\(database_systems\)#As_an_ACID_guarantee) and [Irene Zhang's musing](https://irenezhang.net/research/consistency.html) provide good starting points.

### 13.3.1 ACID

With the above caveats, for data storage systems, Atomicity, Consistency, Isolation and Durability are generally considered to mean:

1. Atomicity: a guarantee that each transaction across multiple data items is treated as a single "unit", which either succeeds completely, or fails completely, even in the case of various failures including machine crashes and network errors.
2. Consistency: Usually understood to mean guarantees about whether a transaction started in the future can necessarily see the effects of all transactions committed in the past. Also sometimes understood to be a guarantee that a transaction can only bring the data from one valid state to another, while maintaining invariants (for example that stock count cannot be less than zero, or that two customers with the same id number cannot exist).
3. Isolation: guarantees that concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially, in some order.
4. Durability: guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure (e.g., power outage or crash). This usually means that completed transactions (or their effects) are recorded in non-volatile memory.

### 13.3.2 The CAP Theorem

[The CAP Theorem](https://en.wikipedia.org/wiki/CAP_theorem) states that it is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:

1. Consistency: Every read receives the most recent write or an error
2. Availability: Every request receives a response that is not an error
3. Partition tolerance: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes

In the absence of network failure both availability and consistency can be satisfied. CAP is frequently misunderstood to mean that one has to choose to abandon one of the three guarantees at all times. In fact, the choice is really between consistency and availability only when a network partition or failure happens; at all other times, no trade-off has to be made.

Database systems designed with traditional ACID guarantees in mind such as RDBMS choose consistency over availability, whereas systems designed around the BASE philosophy, common in the NoSQL movement for example, choose availability over consistency.

The PACELC theorem builds on CAP by stating that even in the absence of partitioning, another trade-off between latency and consistency occurs.
