# 7 File Systems

A [file system](https://en.wikipedia.org/wiki/File_system) is a logical persistence layer organized around storing and retrieving data referenced by files. They provide a richer set of primitives than block stores. These primitives include access control, concurrency control and locking, naming and directory structure, sequential file access, and other features. This makes them more suitable for direct use by applications than block stores. The actual persistence function is performed by supporting layers where the file system may translate files to logical block addresses. File systems can be local, remote, or distributed (independent of underlying block store locality). There are numerous [types of file systems](https://en.wikipedia.org/wiki/File_system#Types_of_file_systems) which tend to differentiate to optimize for many characteristics including storage medium, read/write expectations, performance, durability, and access patterns.

*The following categories include examples solely with the intent of providing context to the category being described. Examples are intended to be widely known to the readers.*

## 7.1 Local File Systems

Local file systems are typically built from local, remote, or distributed block stores. They are commonly used by operating systems to store dependent files.

Generally accepted example terms, platforms, and protocols: EXT4, file, inode, XFS

## 7.2 Remote File Systems

Remote file systems are also referred to as the category [network file systems](https://en.wikipedia.org/wiki/Category:Network_file_systems). They consist of a specialized client that presents local data structures and stores data across a network in remote locations. Through separating client from server a remote file system's capabilities expand beyond the limits of the local system.

There are numerous types of remote file systems with their own specializations. For example, remote file systems are not inherently optimized for safe multi-client access. Applications sometimes solve for this by introducing additional locking mechanisms or they embrace [clustered file systems](https://wikivisually.com/wiki/Clustered_file_system).

Generally accepted example terms, platforms, and protocols: CIFS, cluster, file locks, NFS, VMFS

## 7.3 Distributed File Systems

[Distributed file systems](https://en.wikipedia.org/wiki/Category:Distributed_file_systems) are a type of remote file system that provide the ability for clients to seamlessly store and retrieve files across clusters of servers. The scale is elastic because files are stored in a distributed manner and are globally addressable.

Generally accepted example terms, platforms, and protocols: Gluster, HDFS, Lustre, CephFS and CubeFS.

## 7.4 Comparison

Comparing file systems requires also considering the interaction with the underlying storage layers. The following table describes the optimal/neutral/non-optimal combination generally accepted understanding of the interaction of these layers.

| | Local File System on.. | Remote File System on.. | Distributed File System on.. |
| --- | --- | --- | --- |
| **Local Block Store** | Optimal | Optimal | Optimal |
| **Remote Block Store** | Optimal | Neutral | Non-Optimal |
| **Distributed Block Store** | Optimal | Neutral | Non-Optimal |
