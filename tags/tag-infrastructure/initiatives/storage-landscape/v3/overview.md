# CNCF Storage Landscape Whitepaper V3

**Authors (in alphabetical order):**

Alex Chircop, Clinton Kitson, Jakub Scholz, Luis Pabón, Nick Connolly, Patrick McFadin, Quinton Hoole, Raffaele Spazzoli, Sugu Sougoumarne, Tom Prohofsky, Xiang Li, Xing Yang

## 1 Scope of this document

This is version 3 of the CNCF Storage Landscape Whitepaper. It updates and supersedes the [V2 whitepaper](https://bit.ly/cncf-storage-whitepaperV2), continuing to offer clear information on terminology, usage patterns and classes of technology as defined by the goals of the document, and incorporating feedback and understood production use gathered since the earlier versions.

Non-goals continue to be tackled incrementally across versions, on the basis of community feedback, specifically in light of understood production use, and comparisons w.r.t. primary properties.

### 1.1 Goals

1. **Clarify the terminology** currently in use in the storage space, and the relationships between the various terms. Essentially a taxonomy of the storage landscape.
2. This includes anything reasonably within scope of "storage", including block stores, key value stores, databases, object stores, volumes, file systems etc.
3. Provide some general information as to **how these things are currently being used in production** in public or private cloud environments.
4. **Compare and contrast** the various technology areas w.r.t. the primary properties of availability, scalability, consistency, durability, performance, API, etc.

### 1.2 Non-goals

1. Define what's in-scope and out of scope for the CNCF.
2. Provide any recommendations regarding preferred storage approaches or solutions.

## 2 Introduction and document layout

Multiple options were considered when defining how to present the many storage systems and services in the landscape for the document.

In order to simplify the consumption of information in a complex landscape, the document has been structured as follows:

- Definition of the **attributes** of a storage system such that an end-user can understand the appropriate capabilities that might be required by an application or architectural pattern
- Definition of the **layers** in a storage solution (or service) with a focus on **terminology** and how they impact the defined **attributes** - covering the container, orchestrator, transport, topology, virtual/physical, data protection, data services, the non-volatile layers, and **disaster recovery**.
- Definition of the **data access interfaces** in terms of **volume** (including block, file system and shared file system) and **application API** (including object, KV, database, and **streaming/messaging**) as high level groupings
- Separate sections with further detail on **Block** Storage, **File systems**, **Object Storage**, **Key Value** Stores, **Databases**, and **Streaming and Messaging**.
- Definition of the **management interfaces** needed to orchestrate the storage layers to facilitate composability, dynamic provisioning and self service management.

## Sections

- [Attributes of a Storage Interface or System](topics/storage-attributes.md)
- [Storage Stack / Layers](topics/storage-stack-layers.md)
- [Disaster Recovery](topics/disaster-recovery.md)
- [Data Access Interface](topics/data-access-interfaces.md)
- [Block Stores](topics/block-stores.md)
- [File Systems](topics/file-systems.md)
- [Object Stores](topics/object-stores.md)
- [Key-Value Stores](topics/key-value-stores.md)
- [Databases](topics/databases.md)
- [Streaming and Messaging](topics/streaming-and-messaging.md)
- [Orchestration and Management Interfaces](topics/orchestration-and-management.md)
- [Appendix](topics/appendix.md)
