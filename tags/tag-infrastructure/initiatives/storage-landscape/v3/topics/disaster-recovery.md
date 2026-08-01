# 4.7 Disaster Recovery

Disaster Recovery refers to the behaviour of our systems (not just the storage component of them) when an entire failure domain is lost. Typically in the context of disaster recovery a failure domain is either a data centre or a cloud region.

At a high-level, a disaster recovery provider must ensure two things:

- Client requests are resumed being processed as soon as possible in failure domains that are still available. This metrics is measured by RTO (Recovery Time Objective)
- The least amount of data is lost due to the disaster. This is measured by RPO (recovery point objective).

That being said, the design of disaster recovery procedures focuses mainly on how to replicate the state of stateful workloads across failure domains, such that data is not lost due to a disaster.

The following diagram summarises the main disaster recovery design archetypes, moving from an Active/Passive architectural style (Backup/Restore, Volume Replication, Transaction Replication) to a fully Active/Active style (Distributed Stateful Workload / CNDR), with each successive archetype trading additional architectural complexity for a tighter RTO/RPO:

![Four disaster recovery design archetypes: Back up and restore (Active/Passive, backup to a safe location and restore into a cold failure domain), Volume Replication (Active/Passive, volume replication into a passive failure domain), Transaction Replication (Active/Passive, transaction replication over a network tunnel from a master to a slave database), and Distributed Stateful Workload / CNDR (Active/Active, state sync across a database cluster spanning three active failure domains).](images/disaster-recovery.png)

This table summarises the characteristics of each approach:

| | Backup/Restore | Volume Replication | Transaction Replication | Distributed stateful workloads |
| --- | --- | --- | --- | --- |
| Architectural style | Active/Passive | Active/Passive | Active/Passive | Active/Active |
| Disaster recovery process trigger | Human | Human | Human | Automatic |
| RTO/RPO | RTO: hours | RTO: minutes | RTO: minutes | RTO: seconds |
| | RPO: frequency of backups | RPO: zero for synchronous replication, unbounded for asynchronous replication | RPO: zero for synchronous replication, unbounded for asynchronous replication | RPO: zero |
| Disaster Recovery process ownership | Infrastructure team, particularly storage team | Infrastructure team, particularly storage team | Owner of middleware, typically developer team | Owner of middleware, typically developer team |
| Required Capabilities | Storage: backup and restore<br>Networking: global load balancer | Storage: volume synchronising<br>Networking: global load balancer | Networking: global load balancer, east-west path | Networking: global load balancer, east-west path |

For a more detailed dissertation of this topic, please see the [Disaster Recovery White Paper](https://github.com/cncf/tag-storage/blob/master/cloud-native-disaster-recovery-whitepaper/Cloud%20Native%20Disaster%20Recovery%20V1.pdf).
