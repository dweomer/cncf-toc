# Kubeflow Adopter Interview - NVIDIA

**Organization**: NVIDIA (Run:ai team)  
**Industry**: AI/ML platform / GPU computing  
**Interview Date**: June 29, 2026  
**Interviewers (TOC)**: Faseela K  
**Interviewees**: Ron Kahn (Senior Software Engineer), Ekin Karabulut (Developer Advocate)  


## Organization Intro

### Can you give us an overview of your organization and what it does?

NVIDIA is a large technology company. The interviewees are part of the NVIDIA Run:ai team that builds an AI platform, providing a one-stop shop for AI workloads - from notebook-based model development through distributed training to production deployment. Within this platform, Kubeflow Trainer is used as the core infrastructure for distributed training. Other Kubeflow components are used by other internal NVIDIA teams; the interviewees speak specifically from the perspective of their platform team's usage of Kubeflow Trainer.

## Motivation

### Compared with other products in this space (proprietary and open), what drew you to the project?

At the time of evaluation (2023), no comparable mature project existed. Other ML frameworks (PyTorch-native schedulers, MPI wrappers) solved isolated pieces - single-node training or job submission - but none provided the full combination of distributed training orchestration, Kubernetes-native design, and a developer-facing API that abstracted Kubernetes from ML users. Kubeflow Trainer was the clear fit - no other project matched its combination of maturity, features, and Kubernetes-native design. The CNCF backing was a further confidence signal - it indicated broad organizational adoption and long-term viability.

## Usage Scenario

### How long has your organization used the project?

NVIDIA Run:ai began evaluating Kubeflow Trainer in early 2023, focused on solving distributed training at scale. The first production version of their platform using Kubeflow Trainer shipped approximately 3-4 months into 2023. The project has been in continuous production since then.

### What were the main motivations to adopt the project and which key features do you use today?

Two core requirements drove the decision:

1. Kubernetes-native: The platform is built on Kubernetes, so the solution had to fit naturally into that ecosystem.
2. Familiar to ML users: The abstraction had to hide Kubernetes complexity and let users focus on training semantics, not infrastructure.

Currently in use: Kubeflow Trainer, specifically for distributed training with PyTorch and MPI frameworks. Other Kubeflow subprojects are used by other internal NVIDIA teams; the interviewees are not the right contact for those.

### What is the current level of usage (pre-production, production) and scale?

Level: Production.
Scale: The NVIDIA Run:ai platform serves customers running thousands of GPUs on Kubeflow Trainer for distributed training workloads. Customers host their own clusters; NVIDIA Run:ai ships Trainer as a component of their platform offering, installed when the customer wants distributed training.

### What version is used and what is your update cadence with the project?

Currently using Kubeflow Trainer V1 (latest patch version). The NVIDIA Run:ai platform ships monthly, but Kubeflow Trainer itself is updated approximately every 3 months, aligned with the V1 community patch release cycle.

Migration to V2: Planned but blocked on elastic workload support. NVIDIA Run:ai's users rely on elastic training; until V2 reaches full feature parity on this capability, the V1 to V2 migration is on hold.

### Can you walk me through what your experience was in either adopting it outright or integrating it with your existing services and applications? What challenges did you experience with the project?

Integration was smooth overall. The Trainer API was straightforward and the Kubernetes-native design was praised. Three challenges were encountered:

1. Large-scale configuration guidance: When users brought very large workloads, there was no clear documentation on configuring Trainer for high-scale scenarios. A specific configuration parameter was not exposed in the API at the time. NVIDIA contributed the fix upstream and it was merged.

2. Job status visibility for non-Kubernetes users: the NVIDIA Run:ai platform abstracts Kubernetes from end users. At the time, Trainer's status exposure was limited. This has been significantly improved in V2. NVIDIA presented this at KubeCon ~1 year ago; maintainers linked the talk to an upstream tracking issue without being asked.

3. V1-to-V2 feature parity: NVIDIA Run:ai's users depend on elastic workload support, which V2 does not yet fully cover. This is an ongoing operational blocker. There is also no clear documentation showing what V2 features are still missing vs. V1.

### Did you find the information in the repo or the project docs valuable to your implementation? If so, where did you find the information and what specifically was useful?

Generally yes. NVIDIA Run:ai's team primarily uses GitHub examples and the kubeflow.org docs site. The examples are practical and sufficient for most needs. Two gaps noted:

1. V1-to-V2 feature parity documentation is absent - no clear, maintained view of what is still missing in V2 vs. V1.
2. The GitHub repo documentation is sometimes more up-to-date than the official website, creating inconsistency.

### Has your implementation of the project provided measurable value? Such as reducing manual activities, faster integrations, supported federation/multi-cloud, ease of use, cost savings, etc.

Yes, though no quantitative metrics were provided. Value described in qualitative and structural terms:

1. Engineering velocity: Without Kubeflow Trainer, NVIDIA Run:ai would have had to build and maintain a distributed training controller themselves, requiring significantly more engineering investment.
2. Semantic abstraction: Trainer lets users focus on what they are training and why, not how it runs on Kubernetes.
3. Cost avoidance: Reduced engineering spend on infrastructure, reinvested in higher-level platform features.

"Without the project, we would have to do it ourselves. It would probably take us much more time and reinventing the wheel every time instead of using something so valuable from the community."

### Do you have any future plans regarding the project? More involvement, feature requests, expansion, etc.

1. V1-to-V2 migration: Concrete plan once elastic workload parity is delivered in V2.
2. Deeper collaboration: Early-stage ideas for closer collaboration with the Kubeflow community; too early to detail, but the interviewees noted potential for more collaboration that would benefit both teams.
3. Upstream contributions: A few bug fixes contributed historically; the team maintains close relationships with upstream maintainers.

## Perception

### What is your perception in terms of the project's:

- Community openness
- Governance
- Community growth potential
- Maintainer diversity and ladder
- Maintainer response

Community openness: Very positive. Maintainers are open and approachable.

Governance: Community-driven; no single organization appears to dominate direction.

Community growth potential: Strong - NVIDIA sees potential for deeper collaboration.

Maintainer diversity and ladder: NVIDIA has Kubeflow maintainers in other internal teams. The interviewed team stays in regular contact with one upstream maintainer.

Maintainer response: Highly responsive. Maintainers proactively linked a KubeCon talk on Trainer status issues to a tracking issue without being asked.

### How are you participating in the project community?

Primarily as an observer and occasional contributor. The team attends community meetings from time to time, mostly as listeners, and engages via CNCF Slack and direct maintainer contact. A few bug fixes were contributed upstream historically, and NVIDIA presented at KubeCon on Kubeflow Trainer (~1 year ago).

### Did you need to engage with the community members or maintainers? If so, what was the context of the engagement, which communication channels did you use and did it reach an acceptable outcome?

Yes - via CNCF Slack and direct maintainer contact:

- Contributed a configuration fix upstream (for large-scale workloads); merged successfully.
- Engaged with maintainers on the job status issue; feedback incorporated into V2 design.
- NVIDIA presented an issue and requirement at KubeCon; maintainers linked the talk to an upstream tracking issue and implemented.

All engagements reached a positive outcome. "They are always making you feel like they are open for new features, they are open for improvement, there is no ego about it."

### If the project were to be archived, what level of difficulty would your organization experience?

Kubeflow Trainer is a core component of NVIDIA Run:ai's distributed training platform. Removal would require rebuilding that capability from scratch. The interviewees noted that NVIDIA already has Kubeflow maintainers in other internal teams, so the company would likely step up maintainership rather than accept the project being abandoned. Removal from the platform would be a significant engineering effort.

## Project Strengths

### In your opinion, what are the overall strengths of the project?

- Clear API design: Semantic training abstractions that hide Kubernetes complexity are well-executed.
- Community culture: Maintainers are open, actively incorporate feedback, and are considered one of the project's biggest strengths.
- Vendor-neutral governance: The project feels community-driven day-to-day; no single organization dominates direction.
- CNCF backing: Provided early confidence for NVIDIA Run:ai to adopt and build on the project.
- Kubernetes ecosystem integration: No friction with the GPU layer. Separation of concerns between Trainer and the GPU Operator works well in practice.

## Project Improvements

### Is there something you feel that holds the project back from reaching its ultimate potential?

The primary gap for NVIDIA Run:ai is V1-to-V2 feature parity. The V2 redesign introduced important improvements, but the lack of elastic workload support prevents production users like NVIDIA Run:ai from migrating. The absence of a clear, maintained V1-vs-V2 feature comparison makes it difficult for operators to plan the transition.

"It was very challenging, very challenging for us to move to a new, complete way to do it without full feature parity."

No concerns about community direction - the project feels community-driven.

### In your opinion, what can the project do better?

- V1-to-V2 migration documentation: A clear feature parity matrix and migration readiness tracker would significantly help platform operators plan upgrades.
- Large-scale configuration guidance: Dedicated documentation for running Trainer at scale is still sparse.
- Elastic workload support in V2: The specific capability blocking NVIDIA Run:ai's V2 migration.

### Maturity Level Survey

The following questions survey your understanding of CNCF project maturity levels. Please fill in your answers directly in this document.

Q1. Do you feel you have a good understanding of the meaning of each CNCF maturity level?:

Solid understanding of CNCF maturity stages and their criteria.

Q2. Is there information missing regarding the meaning of each different level?

No material documentation gaps identified.

Q3. Do you rely on those levels internally in any way, and if yes how?

CNCF maturity level is used as one validation signal alongside other selection criteria; weighting varies by team.
