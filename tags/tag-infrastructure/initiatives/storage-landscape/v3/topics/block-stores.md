# 6 Block Stores

Block stores are a persistence target where data is stored in blocks in local, remote, or distributed locations. The blocks are typically numerically addressed using a method called [Logical Block Addressing](https://en.wikipedia.org/wiki/Logical_block_addressing) (LBA) and accessed as a client through a [device](https://en.wikipedia.org/wiki/Device_file#BLOCKDEV) interface provided by a Kernel. The location (local/remote/distributed) is determined by the physical persistence location of the blocks and serves as a method to group and categorize different stores.

It is possible to transparently augment or enhance numerous characteristics of block stores such as availability, scalability, consistency, durability, and performance by adding additional software-based storage layers (ie. RAID) along with physical devices, networking, and nodes. Please refer to the Capacity, Availability, and Partition-tolerance (CAP) theorem overview in the [appendix](appendix.md) for more details.

Virtualization adds another perspective which is important to consider. Operating systems may or may not be aware of the type of block store being used. Virtual machines and machine instances are likely not storing any blocks locally but completely leveraging remote or distributed block stores. In this case, instances provide virtualized hardware that store data remotely and emulate the connectivity and behavior of local physical storage devices. This storage would not be considered a local block store due to non-locality of stored data.

Most applications do not directly store data in block format, but instead interface with file systems supported by block devices (ie. application -> local EXT4 filesystem -> local block device -> local/remote/distributed block store). See the [File Systems](file-systems.md) section for more details.

*The following categories include examples solely with the intent of providing context to the category being described. Examples are intended to be widely known to the readers.*

## 6.1 Local Block Stores

Local block stores are built on [Direct Attached Storage](https://wikivisually.com/wiki/Direct_attached_storage) (DAS) where data is persisted locally on hardware devices. Since all data is stored locally, the scale is limited to the local resource capabilities. The availability of the data is a major consideration when applications are interacting directly with local block stores. [Logical volume management](https://en.wikipedia.org/wiki/Logical_volume_management) (LVM) and similar techniques can be used to augment and concatenate the capabilities that discrete hardware devices provide. These stores tend to be focused on specific use cases where latency is critical or to support other storage services.

Generally accepted example terms, platforms, and protocols: ATA, IDE, logical volumes, LVM, physical volumes, physical storage devices, RAID, SCSI, volume groups

## 6.2 Remote Block Stores

Remote block stores provide storage attached by a network where data is persisted remotely across a network. This is different from local because there is a separation of application from storage. Generally, this has the ability to increase capacity and performance. The availability is also increased since high availability design patterns can be implemented. Without detailed information and assurances and intentional design, service levels are likely to be driven by this category.

Generally accepted example terms, platforms, and protocols: NVMe over Fabrics, AWS EBS, FC, FCoE, iSCSI, SAN

## 6.3 Distributed Block Stores

Distributed block stores are similar to remote block stores but data is persisted across many nodes, possibly in conjunction with the local node, and clients are able to rely on many nodes to provide redundancy and horizontal scalability. When compared with local and remote block stores, distributed block stores require additional control and data access layers to manage data distribution (and often also replication). This added complexity can provide improved scalability, availability, and durability.

Generally accepted example terms, platforms, and protocols: Ceph, DRBD, OpenEBS, Longhorn, hyper-converged
