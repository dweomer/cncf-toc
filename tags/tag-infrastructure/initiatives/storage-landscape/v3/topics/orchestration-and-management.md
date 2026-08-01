# 12 Orchestration and Management Interfaces

This section defines how Container Orchestration Systems (CO) interact with the Storage Systems to associate workloads with Data from the Storage Systems. Depending on the Data Access Interfaces, different layers may be involved.

## 12.1 Volumes - block stores and filesystems

![Workloads sit above Container Orchestrators, which talk to Control-Plane Interfaces. Those control-plane interfaces connect either to Frameworks and Tools (arrow B) or directly to a storage system's Control Plane (arrow A), while Workloads consume storage directly from a storage system's Data Plane (arrow C). Frameworks and Tools can also interact directly with a storage system's Control Plane (arrow B).](images/container-orchestration-system.png)

A Container Orchestration System (CO) such as Kubernetes can support multiple interfaces to interact with the Storage System.

The Storage System can:

- **(A)** support control plane interface API directly and interact directly with the orchestrator or
- **(B)** interact with the orchestrator via an API Framework layer or other Tools.

The orchestrator can use the control plane interfaces **(A)** or **(B)** to support the request for a volume and may also be able to use the interface to dynamically provision a volume.

Workloads consume **(C)** storage from storage systems over various data access interfaces.

The underlying storage infrastructure layer can be software-based commodity storage, cloud storage, or enterprise storage. The management layer provides an abstraction over the complexity of various storage systems.

Whether to use **(A)** or **(B)** depends on user requirements and capabilities supported by the storage system. **(A)** has been primarily focusing on dynamically provisioning storage (or pre-provisioning storage) for workloads, although more advanced functionality may be added in the future. **(B)** may also support discovery, automation, and other data services such as data protection, data migration, or data replication in addition to provisioning.

There are on-going discussions in Kubernetes to provide more advanced functionality such as data protection. At the time of this writing, a Data Protection Working Group was formed with collaboration between Kubernetes SIG-Storage and SIG-Apps to promote this objective ([https://github.com/kubernetes/community/blob/master/wg-data-protection/charter.md](https://github.com/kubernetes/community/blob/master/wg-data-protection/charter.md)).

### 12.1.1 Control Plane Interfaces

*"Control-Plane Interfaces"* refers to storage interfaces for CO. It includes Native Interfaces such as Kubernetes Native Drivers and Docker Volume Driver Interface as well as External Interfaces such as Container Storage Interface.

#### 12.1.1.1 Container Storage Interface

The Container Storage Interface (CSI) is the industry-standard specification for containerized storage. It enables storage vendors to write a single plugin that works across any compliant Container Orchestration (CO) system, such as Kubernetes, effectively replacing legacy "in-tree" storage drivers and decoupling storage lifecycle management from core platform releases.

##### 12.1.1.1.1 Core CSI Architecture

CSI follows a gRPC-based architecture, modularizing functionality into three primary services:

- **Identity Service:** Allows the Container Orchestrator to query the plugin for its capabilities and metadata.
- **Controller Service:** Manages the volume lifecycle, including provisioning (creating/deleting), attaching/detaching, snapshotting, cloning, and volume modification.
- **Node Service:** Operates on the individual worker node, managing the attachment, formatting, and mounting of volumes for container access.

##### 12.1.1.1.2 Advanced Capabilities

CSI has evolved into a mature specification that supports several advanced operational workflows:

- **Volume Expansion & Modification:** CSI supports the dynamic resizing of volumes via `ControllerExpandVolume` and the modification of volume attributes (such as performance tiers, IOPS, or throughput) via `ControllerModifyVolume`. In modern Kubernetes environments, `ControllerModifyVolume` is surfaced through `VolumeAttributesClass` (VAC), allowing for non-disruptive, declarative updates to storage performance without unmounting or recreating the volume.
- **Snapshots:** Supports Snapshot Metadata and Volume Group Snapshots, enabling complex data protection and recovery scenarios.
- **Block vs. Mounted Volumes:** Supports both **Block Volumes** (raw devices for applications that manage their own file systems) and **Mounted Volumes** (filesystems mounted directly as directories inside the container).

##### 12.1.1.1.3 Status and Resources

CSI is the foundational mechanism for storage in modern Kubernetes.

- **Official Spec:** [https://github.com/container-storage-interface/spec](https://github.com/container-storage-interface/spec)

#### 12.1.1.2 Kubernetes Native Drivers

This refers to Kubernetes in-tree volume drivers that extend Kubernetes volume interfaces to support block and file storage systems. Kubernetes has the following concepts for storage:

- Persistent Volume (PV) is a piece of storage provisioned by an administrator on the storage system.
- Persistent Volume Claim (PVC) is the storage requested by a user. Kubernetes cluster will try to find a matching PV that matches the PVC request.
- PV can be pre-provisioned or dynamically provisioned. Dynamic provisioning is done using a Storage Class created by an administrator. Storage Class defines different levels of services that a storage system can provide. Kubernetes manages the life cycle of PVs and PVCs. Data on a volume can persist beyond the lifetime of a pod that consumes the volume.

Kubernetes in-tree volume drivers can support the following functionalities: create and delete volume, attach and detach volume, mount and unmount volume, and expand volume.

Kubernetes SIG Storage has completed the migration of in-tree volume drivers to CSI. A [design spec](https://github.com/kubernetes/enhancements/tree/master/keps/sig-storage/625-csi-migration) enabled a seamless migration path by allowing CSI drivers to handle volume provisioning requests as a proxy for in-tree drivers. Cloud provider volume drivers including AWS EBS, GCE PD, OpenStack Cinder, Azure, and vSphere have all been migrated to out-of-tree CSI drivers.

#### 12.1.1.3 Docker Volume Driver Interface

Docker volumes can be used to persist data in Docker. Docker provides a mechanism for storage vendors to write a volume driver so that remote storage systems such as Amazon EBS can be used to provide volumes for a docker container. This allows data volumes to persist beyond the lifetime of a single Docker host. If a plugin registers itself as a VolumeDriver when activated, it must provide the Docker Daemon with writable paths on the host filesystem. The Docker daemon provides these paths to containers to consume. The Docker daemon makes the volumes available by bind-mounting the provided paths into the containers.

Supported Docker volume driver interfaces include Create, Remove, Mount, Unmount, Path, Get, List, and Capabilities.

- [https://docs.docker.com/storage/](https://docs.docker.com/storage/)
- [https://docs.docker.com/engine/extend/plugins_volume/](https://docs.docker.com/engine/extend/plugins_volume/)

### 12.1.2 Frameworks and other tools

*"Frameworks and other tools"* are extensions of CO's *"Control-Plane Interfaces"*. In addition to provisioning and managing storage, this extended control plane can also support discovery, automation, data protection, data migration, disaster recovery, monitoring, analytics, performance tuning, and data lifecycle management, etc.

Some examples of frameworks and other tools described in this section include [Rook](https://rook.io) and [Velero](https://github.com/velero-io/velero).

## 12.2 Container Object Storage Interface

File and block storage are treated as first class citizens in the Kubernetes ecosystem via CSI. Workloads using CSI volumes enjoy the benefits of portability across vendors and across Kubernetes clusters without the need to change application manifests.

Object storage has been rising in popularity in recent years as an alternative form of storage to filesystems and block devices. Object storage paradigm promotes disaggregation of compute and storage.

[Container Object Storage Interface (COSI)](https://github.com/kubernetes-sigs/container-object-storage-interface) aims to make object storage the first class citizen in Kubernetes. COSI provides Kubernetes APIs to provision object buckets and allow the buckets to be consumed by the pods. It also introduces gRPC interfaces for object storage providers to write drivers to provision buckets.

As of August 2026, the project is actively developing its `v1alpha2` API specification, moving toward a more robust and idiomatic Kubernetes resource model.

### 12.2.1 COSI Components & APIs

The architecture remains centered on the separation of provisioning and access, but with refined controller logic to handle the new condition-based status reporting.

**Core APIs:**

- **BucketClass, BucketClaim, Bucket:** Standardizes the storage backend definition (`BucketClass`), the end-user request (`BucketClaim`), and the provisioned bucket object (`Bucket`).
- **BucketAccessClass & BucketAccess:** Handles the security plane. `BucketAccess` requests result in the generation of credentials (typically stored in a Kubernetes `Secret`), now communicated through cleaner condition-based status updates in the `v1alpha2` spec.

**Architecture:**

1. **COSI Controller Manager:** Orchestrates the lifecycle of the COSI resources. It monitors for new claims and updates their status conditions as they move through the provisioning or access-granting workflow.
2. **COSI Sidecar:** Acts as the communication bridge. It watches the COSI API objects and facilitates gRPC calls to the driver, reporting the results back to the controller to update the resource status conditions.
3. **COSI Driver:** The vendor-specific implementation. It translates the COSI gRPC requests into concrete operations on the object storage backend (e.g., creating an S3 bucket or updating an IAM policy).

## 12.3 Application API

Currently the Control-Plane Interfaces, the storage interfaces supported by COs, do not include key value stores and databases. Some Frameworks and Tools have support for key value stores and databases. Some examples will be given in the following section.

### 12.3.1 Key-Value Stores

It is possible for a management interface to provide a way to deploy and manage key value stores, similar to how databases can be deployed and managed by the management interface.

### 12.3.2 Databases

Management interface can provide a way to deploy and manage databases. For example, Rook provides an operator to deploy and manage CockroachDB and [YugabyteDB](https://docs.yugabyte.com/latest/deploy/kubernetes/single-zone/oss/rook-operator/) clusters. Another CNCF project Vitess ([https://vitess.io](https://vitess.io)) also provides an operator to manage MySQL clusters.
