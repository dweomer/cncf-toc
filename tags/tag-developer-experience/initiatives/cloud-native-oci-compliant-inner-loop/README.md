# Cloud Native and OCI Compliant Inner-Loop Tooling & Packaging for AI Engineers

https://github.com/cncf/toc/issues/1740

Integrating the AI developer inner loop into an end-to-end CI/CD process leveraging cloud native technologies and tooling

## Initiative description

Focus on inner loop development which incorporates everything an AI engineer does on a local environment before code or models ever reach CI/CD in a cloud native environment:

* Local container workspaces: Reference inner loop workflow using desktop tooling such as Podman Desktop / Podman AI Lab for root-less, GPU-aware experimentation, including template images for PyTorch/LLM stacks and volume-mounted datasets. ​
* Unified Inner-Loop CLI: Hardening local inference and agentic frameworks via container-based tooling, allowing engineers to spin up inference, RAG, and multi-agent services locally with a single command.
* Standard packaging of artefacts: Drive convergence between various implementations such as ModelKit, ModelCar towards the emerging ModelPack spec to create a single OCI-manifest that can hold model weights, metadata and SBOM.
* Inner-loop supply-chain security: Integrate Notary v2 / model authenticity and transparency via Sigstore, LF AI & Data Model Openness Framework-generated model & data cards, plus SBOM annotations directly into the OCI artefact so that security & openness are “baked in” before CI. ​
* Fast hand-off to outer loop: Provide reference GitOps flows (Flux/Argo) that pull the signed artefact into KServe with ModelPack image-mount optimisation, and register versions in Kubeflow Model Registry.

## Deliverable(s) or exit criteria

* A technical POC showing <10 min “idea-to-inference” path for cloud native agent development on a developer environment.
* Clearly documented standards for OCI artefact standardization across runtimes and registries.
* Specification / procedure to achieve MOF Class III compliant model distributions via any OCI registry.
* Standardised process for leveraging model signing with artefacts-level provenance to support a verified end-to-end CI/CD reference pipeline including outer loop for AI engineering.

## Project Scope & Intent - Cloud Native AI Developer Workflow Interoperability

### Overview and Intent
AI developers today frequently work in fragmented local environments that are disconnected from cloud native operational workflows. While emerging standards like ModelPack and OCI-aligned AI artefact initiatives provide the “packaging” foundations, there is no unified interoperability specification that defines how model artefacts must be described, secured, and verified, and consumed across developer environments, registries, local runtimes, and Kubernetes-based systems..

The goal of this initiative is to define a minimal Interoperability Specification (a “Compliance Profile”) for AI Model Artefacts. 

Rather than redefining the OCI layer structure, media types, digests, or artefact storage mechanics, this initiative defines the **Standardized Metadata Contract** that must exist on top of packaging formats like **ModelPack**. This ensures that any ["Cloud-Native Ready"](https://github.com/cncf/foundation/blob/main/style-guide.md#1-cloud-native-and-open-source) AI artefact contains the mandatory identity, security, openness, provenance, lifecycle, and runtime information needed to support local execution, cross-tool portability, and GitOps-driven delivery.

This initiative intentionally builds on existing OCI-aligned packaging efforts rather than redefining artefact storage or layer mechanics.

Version 1 of this initative focuses on model artefacts only. Future iterations may extend the interoperability profile to additional AI artefact types such as agentic skills, prompts, RAG assets, and workflow definitions.

### Scope Overview
This initiative defines the **Interoperability Layer** for AI artefacts, bridging the gap between raw packaging and operational deployment. 

Within this scope, the initiative will define and validate:
* **An Interoperability Profile Spec:** A set of mandatory annotation conventions and metadata requirements (the “Manifest Contract”) for model artefacts.
* **Compliance & Trust Requirements:** Standards for signing, SBOMs, provenance, lifecycle status, and openness classification.
* **Workflow Reference Patterns:** Validating of the profile through local runtime, registry, GitOps, and Kuberentes integration patterns. 

The initiative is intended to encourage ecosystem alignment and workflow interoperability rather than define new standalone packaging formats or runtime standards.

### In-Scope Areas
#### 1. The Interoperability Specification
Define a structured, minimal specification for AI model artefacts to be considered “Cloud Native Interoperable”. This does not define OCI layering, media types, or packaging mechanics. Instead, it speficies the mandatory metadata and interoperability requirements that model artefacts must expose. This includes:
* **Annotation Conventions:** Standardize keys for runtime frameworks (e.g., vllm), hardware accelerators (e.g., nvidia-gpu), lifecycle status, and compat.
* **Model artefact Identity:** Includes name, version, authorship, and source
* **Runtime Requirements:** Such as supported serving frameworks, accelerator requirements, CUDA compatibility, and memory requirements
* **Lifecycle Status:** Such as experimental, validated, deprecated, or product-ready 
* **Required Trust Metadata:** Including signing, SBOM provenance, and openness classification

Future artefact types, including agentic skills, prompts, RAG contexts, and workflow definitions, are explcitly deferred to later versions.


#### 2. Metadata, Relationships, & MOF Mapping
Define how model artefacts describe themselves and their operational requirements to enable cross-tool discovery, verification, and deployment. This includes:
* **MOF-to-OCI Mapping:** Standardize how the LF AI & Data Model Openness Framework (MOF) classifications (e.g., Class I, II, III) are represented as machine-readable OCI metadata.
* **Lineage & Authorship:** Standardizing metadata for provenance, versioning, and authorship to ensure clear ownership as artefacts move from local environments to registries.
* **Compatibility Metadata:** Declaring software dependencies, runtime requirements, accelerator needs, and infrastructure constraints.
* **Relationship Metadata:** Defining minimal metadata conventions for model-related relationships, such as model to serving configuration or model to deployment target. 
* **Large Binary Asset Optimization:** Documenting best practices and metadata conventions for handling large model weights in OCI registries, while deferring low-level layer mechanics to OCI and packaging efforts such as ModelPack.

This work will build on existing efforts such as, ["ModelPack"] (https://modelpack.org/), ["ModelKit"] (https://kitops.org/docs/modelkit/intro/), and ["ModelCar"] (https://github.com/redhat-ai-services/modelcar-catalog)

#### 3. Supply Chain Security and Transparency
Define the mandatory “Trust Profile” for AI model artefacts to ensure they are verifiable before entering production workflows:
* **Cryptographic Identity:** Standardize artefact signing and verification at the point of creation on a developer's machine, leveraging established frameworks like Sigstore and Notary v2 (Notation), and trsut frameworks based on The Update Framework (TUF), while exploring emerging zero-trust identity protocols such as OpenPubkey and related ecosystem efforts.
* **Transparency Manifests:** Define requirements for attaching transparency and verification artefacts to model artefacts, including SBOM (Software Bill of Materials), provenance documents, and in-toto attestations. These artefacts may be attached directly or via OCI referrer mechanisms and are intended to provide verifiable evidence of artefact composition, provenance, and supply chain integrity.
* **Provenance Metadata:** Requirements for documenting how the artefact was created, modified, signed, and promoted.

The goal is to ensure artefacts are trusted and verifiable before entering CI/CD or GitOps-driven delivery workflows.

#### 4. Developer Inner-Loop & Workflow Interoperability
Define the operational patterns that allow the specification to be utilized in a portable "local environment-to-cluster" journey.
* **Workflow Consistency:** Documenting how existing OCI-aligned tools (ModelPack, ModelKit, ModelCar) can produce artefacts that adhere to this initiative's compliance spec.
* **Local Execution Patterns:** Reference patterns for running compliant model artefacts in local, container-based environments using tools such as Docker, Podman, or other local runtimes.
* **Rapid Iteration Flow:** Validation of the spec through a reference implementation targeting a sub-10-minute "idea-to-inference" experience, excluding large model download time and other network-dependent setup.

#### 5. GitOps and Kubernetes Integration Patterns
Define the "Handoff" patterns for how compliant model artefacts transition into production cloud native systems.
* **GitOps Delivery Patterns:** Reference architectures for pulling compliant artefacts into Flux or Argo CD workflows. This includes standardizing K8s Init Container patterns (leveraging tooling like the ["KitOps init container"] (https://github.com/kitops-ml/kitops)) to pull, cryptographically verify signatures, and unpack specified artefact layers into a shared volume before the main inference or serving runtime boots.
* **Runtime Integration:** Standardized patterns for the seamless deployment of artefacts into serving platforms (e.g., ["KServe"] (https://kserve.github.io/website), [vLLM](https://github.com/vllm-project/vllm), and registration into model registries (e.g., Kubeflow Model Registry).
* **Enterprise Requirements:** Ensuring the promotion spec accounts for air-gapped, regulated, and hybrid-cloud infrastructure constraints.

These patterns are intended as reference integrations, not a full standardization of outer-loop pipelines.

#### 6. Real-World Deployment Considerations
Ensure the approach accounts for:
* Air-gapped
* Enterprise security and compliance requirements
* Regulated environments
* Hybrid and multi-platform infrastructure
* Resource-constrained developer environments
* Other deployment contraints identified through community feedback

This ensures the solution is practical and broadly applicable.

#### 7. Ecosystem Collaboration
This initiative will be developed in collaboration with:
* Related OCI-aligned initiatives
* CNCF projects, like ModelPack
* LF AI & Data communities
* OpenSSF and supply chain security initiatives
* Kubernetes AI and platform engineering communities

The intent is to align efforts across communities rather than define a solution in isolation.

### Out of Scope Areas
* Define new low-level binary compression or OCI layering, media type, or storage mechanics (deferring to OCI/ModelPack)
* Replace or duplicate ModelPack or other OCI-aligned packaging efforts
* Define model architectures or ML training frameworks
* Standardize non-model artefact types in v1, including agentic skills, prompts, RAG assets, or workflow definitions
* Mandate specific vendor-locked developer tools
* Define Kubernetes runtime APIs
* Standardize outer-loop pipelines beyond reference integration patterns

### Definition of Success
* **A Published Interoperability Spec:** A validated specification that existing tools can adopt to ensure cloud native AI model artefacts.
* **Existing Tools:** Can adobt the profile to produce cloud native interoperable model artefacts.
* **Cross-Tool Portability:** Demonstrated ability for an artefact built by one tool to be verified and deployed by a different runtime.
* **The "10-Minute Flow":** A successful reference implementation demonstrating the journey from a local idea to a running inference service on Kubernetes.
* **Model artefacts:** Can be signed, verified, described, and promoted consistently across runtimes, registries, GitOps workflows, and Kubernetes-based systems.
* ** (Stetch Goal) Ecosystem Alignment:** Broad adoption of the "Compliance Profile" metadata across CNCF and LF AI & Data communities.

