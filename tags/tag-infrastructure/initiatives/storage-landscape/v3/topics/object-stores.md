# 8 Object Stores

Unlike file systems and block stores, where there is a general understanding of the implementation behind the interface, object stores are quite heterogeneous in their implementations. In general an object store system is an *atomic* key-value store, where the key and value are defined by the implementer of the storage system. An atomic key value store guarantees that a request to *set* or *get* a value is either fully committed or not at all.

There are many examples of object stores. From how an internet browser gets HTML content and sends, or posts, data back to the web server, to how an operating system gets and sets data from an LBA on a block store.

## 8.1 HTTP Based Object Storage

Due to the large range of object store implementations, this paper will be focusing on HTTP based object stores as defined by [Amazon Web Services S3](https://aws.amazon.com/s3/), [Google Cloud](https://cloud.google.com/storage/docs/concepts), and [OpenStack Swift](https://docs.openstack.org/swift/latest/). Typically, these types of object stores are used for large opaque values, as a result, they are often used to store images, videos, and data backups.

These types of object store systems have defined a set of methods based on the HTTP protocol where the key is a [URL](https://en.wikipedia.org/wiki/URL) and the value is a set of data. This interface makes it simple to access content since there is no need to mount or attach an object store. Due to the nature of this model, these type of object store systems are always remote to the requester node.

An HTTP based object store access model is largely constructed of an *account* in which it has a set of *buckets* as they are called by S3 or *containers* as they are called by OpenStack Swift. Each of these buckets or containers can then contain objects. The *key* is based on the combination of these values loosely based on the format:

```
http(s)://<server/account/container>/[...object]
```

where the *object* is a unique identifier reference to an object. For example:

```
https://server.io/v1/admin/pictures/path/to/the/picture.jpg
```

shows the object is `path/to/the/picture.jpg`.

One of the advantages of these HTTP based systems over simple object stores is the metadata management. These storage systems make it possible for the requester to attach custom metadata to the objects, which can then be used to list, fetch, or group objects. Another advantage is in access control, where access can be placed on anything from a set of containers or buckets to a single object.

## 8.2 Scalability, Availability, Durability, Performance

HTTP object stores are designed for scalability and durability, but not for low latency performance as compared to block or file based storage systems. Instead they are designed for supporting extremely large amounts of data spread over not only a single data center, but over many regions all over the world. HTTP object stores are also designed for durability, supporting many methods of maintaining the data integrity of their objects. They may maintain multiple copies of their objects or use [erasure coding](https://en.wikipedia.org/wiki/Erasure_code) to maintain object durability. These methods provide an unprecedented object durability service level agreement. As an example, Amazon Web Services S3 claims object store service is designed for a durability of [99.999999999%](https://aws.amazon.com/s3/storage-classes/).

Due to their nature, HTTP based object store systems are not suited for latency sensitive applications. On the other hand, unlike block and file, an object store system can provide data to clients on behalf of an application! For example, when a web browser requests data from an application which stores data in an object store, instead of having the application return the data itself, it can send the web browser pointers to the data on the object store. The object store system then returns the data *directly* to the client from a region closest to it, reducing the network IO requirements of the application.
