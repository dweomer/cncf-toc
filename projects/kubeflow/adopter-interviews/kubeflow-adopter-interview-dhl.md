# Kubeflow Adopter Interview - DHL Data & AI

**Organization**: DHL Data & AI  
**Industry**: Logistics / enterprise AI platform  
**Interview Date**: June 30, 2026  
**Interviewers (TOC)**: Faseela K, Brandt Keller  
**Interviewees**: Julius Von Kohout (Principal Platform Engineer), Dr. Gezim Sejdiu (Chief Data Engineer)  


## Organization Introduction 

### Can you give us an overview of your organization and what it does?

DHL Data & AI is the central Data & AI organization within the DHL Group. It provides a portfolio of services including Data Analytics, Operations Research, Machine Learning, AI Platforms & Solutions, Data Engineering, and MLOps Engineering. Its mission is to transform raw data into business value by delivering actionable insights and scalable AI solutions across the DHL Group.

The team supports the full lifecycle from ideation and experimentation to deployment and operations, and drives group-wide Data & AI governance, technology incubation, enablement, training, and community-building. Solutions span demand forecasting, ETA prediction, network optimization, staff scheduling, customer analytics, finance analytics, GenAI/NLP applications, geocoding, and customs product classification.

## Motivation

### Compared with other products in this space (proprietary and open), what drew you to the project?

Kubeflow stood out for providing a cloud-agnostic ML platform built on Kubernetes. Multiple alternatives were evaluated, including MetaFlow, Databricks, and MLFlow. Kubeflow offered the strongest balance between flexibility, extensibility, scalability, and vendor independence - particularly for multi-cloud deployments where no comparable alternative exists.

Key factors in the decision:
- Open-source ecosystem and avoidance of vendor lock-in
- Native Kubernetes integration
- Strong support for multi-cloud and hybrid deployments
- Rich ecosystem for experimentation, pipelines, training, and model lifecycle management
- Ability to customize and extend components for enterprise requirements
- Strong alignment with long-term MLOps and platform engineering strategy

## Usage Scenario

### How long has your organization used the project?

DHL Data & AI has been using Kubeflow since 2021 as the foundation of its enterprise Data Science platform. Over this period, the platform evolved from supporting pilot projects to a strategic ML/AI platform serving hundreds of users across the DHL Group.

### What were the main motivations to adopt the project and which key features do you use today?

Primary motivations:
1. Standardization of machine learning workflows
2. Scalable model development and deployment
3. Enterprise-grade MLOps capabilities
4. Support for hybrid and multi-cloud architectures
5. Efficient collaboration between data scientists, platform engineers, and business stakeholders

Currently in active use:
- Kubeflow Notebooks (Notebooks V2 in evaluation)
- Kubeflow Pipelines (KFP V1/V2 in production)
- Kubeflow Dashboard (multi-tenancy workspace management and isolation)
- Kubeflow Spark Operator (managing Apache Spark application lifecycle on Kubernetes)
- GPU-enabled workloads
- Containerized model development
- Experiment tracking and reproducible workflows
- CI/CD integration and MLOps automation
- Resource governance and cost management

Subprojects on the near-term roadmap: Katib, Trainer, and Hub.

### What is the current level of usage (pre-production, production) and scale?

Level: Production

Scale:
- Several hundred registered platform members and active users per month
- Several hundred onboarded projects and use cases
- Probably several hundred thousand active business end users (drivers, delivery personnel, sorting operations, finance etc.) relying on ML solutions powered by the platform
- Multiple business-critical ML solutions running in production
- Deployments across multi-cloud and hybrid environments

### What version is used and what is your update cadence with the project?

Always using the latest Kubeflow Community Distribution. DHL Data & AI uses the community distribution directly. Julius von Kohout is a Community Distribution release maintainer and actively works on the release cadence - in some cases shipping updates the same day as upstream releases.

### Can you walk me through what your experience was in either adopting it outright or integrating it with your existing services and applications? What challenges did you experience with the project?

The adoption journey was generally successful. Integration involved existing enterprise identity management systems, security and compliance requirements, multi-cloud infrastructure, monitoring and observability tooling, CI/CD ecosystems, and internal chargeback and governance processes.

Key challenges from early adoption (with current status):
- **Operational complexity**: Large Kubernetes-based platforms require platform engineering expertise. Has improved substantially with platform maturation.
- **Spark Operator integration (2024)**: Integrating Spark Operator into the community distribution required dedicated work ([community-distribution#2889](https://github.com/kubeflow/community-distribution/pull/2889)). Now resolved and in active production use.
- **Enterprise authentication and authorization (2024)**: OIDC to OAuth2-proxy migration. DHL contributed the solution upstream together with Roche Pharmaceutical, now available to the broader community.
- **Multi-tenancy and enterprise isolation**: Object storage isolation, PSS baseline/restricted by default, network policies, RBAC hardening.
- **Managing upgrades**: Previously complex; now improved with the dedicated [upgrading and extending section](https://github.com/kubeflow/community-distribution#upgrading-and-extending) in the community distribution.

Current state: operational complexity has improved significantly. The platform is considered security-hardened with the exception of a few remaining multi-tenancy items.

### Did you find the information in the repository or the project documentation valuable to your implementation?

Yes. Official documentation, GitHub repositories, community discussions, and implementation examples provided valuable guidance. Documentation has improved notably, with documentation automation now in place.

A key differentiator of the Community Distribution is that it goes beyond documentation: it provides automation and test-driven development so users can copy verified CI/CD pipelines rather than relying purely on written documentation.

Particularly useful resources:
- Installation and deployment documentation
- Component architecture overviews
- Kubeflow Pipelines documentation
- Community issue discussions and GitHub Discussions
- Upgrade and migration documentation ([community distribution upgrading section](https://github.com/kubeflow/community-distribution#upgrading-and-extending))

### Has your implementation of the project provided measurable value?

Yes. Key benefits:
- Reduced platform onboarding effort for new projects
- Faster development-to-production cycles
- Improved reproducibility of machine learning workflows
- Standardization of ML development practices across DHL Data & AI and business units
- Multi-cloud deployment flexibility
- Better governance of machine learning solutions
- Reduced operational overhead through automation

Kubeflow has become a core capability enabling AI and ML at scale across the DHL Group.

### Do you have any future plans regarding the project?

- **Hub**: Planned to be exposed to DHL Data & AI customers from the next release onwards. Currently evaluating MLFlow vs Kubeflow's model registry.
- **Katib and Trainer**: On the near-term roadmap for LLM fine-tuning evaluation.
- **Notebooks V2**: Alpha already under analysis.
- **KFP V2 init containers**: KFP V2 currently lacks init container support - an active open item to resolve upstream or contribute.
- **GenAI workloads**: Expanding platform support including LLM-related workloads.
- **KubeCon presentations**: DHL Data & AI teams are already presenting at KubeCon, strengthening community visibility.
- Continued upstream contributions, community engagement, and GSoC mentorship.

## Perception

### What is your perception in terms of the project's community and governance?

- **Community openness**: Very positive. The project welcomes participation from a large set of organizations, especially after it was donated by Google to the CNCF. The community now feels more heterogeneous and healthier than it did 3 years ago, particularly with the Community Distribution initiative.
- **Governance**: Transparent and aligned with the expectations of a mature CNCF ecosystem project. Vendor neutrality is actively enforced - no single-vendor push has been experienced.
- **Community growth potential**: Strong and growing. Market demand for scalable MLOps is increasing. The adopter base has grown and the outlook is confident.
- **Maintainer diversity and ladder**: Contributions from multiple companies and organizations. Kubeflow is active in Google Summer of Code (GSoC), with some interns becoming ongoing contributors.
- **Maintainer response**: Generally positive. Active community meeting participation.

### How are you participating in the project community?

DHL Data & AI's participation is deep and structural:
- Julius Von Kohout: Kubeflow Steering Committee (KSC) member, Dashboard repository owner, Pipelines manifests owner, KCD release maintainer, security response team member.
- DHL Data & AI team members are part of the Kubeflow security response team.
- DHL Data & AI runs internal security scanning and contributes fixes upstream.
- Contributed OIDC-to-OAuth2 proxy support (with Roche Pharmaceutical), Spark Operator work, multi-tenancy and security best practices, KFP bugfixes, and architectural guidance for contributors from Google, IBM, RedHat, AWS, and Capital One.
- Active in community meetings and GSoC mentorship.

### Did you need to engage with the community members or maintainers?

Yes, and DHL Data & AI's engagement goes beyond typical adopter participation.

Key engagement examples:
- Contributed OIDC-to-OAuth2 proxy support (with Roche Pharmaceutical) and Spark Operator improvements; both are now available to the broader community.
- Internal security scanning surfaces CVEs; findings are fed upstream and fixed in the community where applicable.
- Active participation in community meetings.
- One area where collaboration could have been more structured: the interactive Spark Operator feature, which could have benefited from earlier community coordination. Raised as a constructive example of where collaboration processes could improve.
- All community interactions described as professional and productive.

### If the project were to be archived, what level of difficulty would your organization experience?

DHL Data & AI is a strong Kubeflow maintainer. If other organizations stepped back, DHL Data & AI would be well-positioned to step up. The governance model ensures vendor neutrality, so no single organization's departure would put the project at risk. There is no meaningful multi-cloud alternative to Kubeflow, particularly for regulated sectors.

## Project Strengths

### In your opinion, what are the overall strengths of the project?

- Open-source and vendor-neutral foundation with actively enforced governance
- Strong Kubernetes-native architecture
- MLOps capabilities spanning the full ML lifecycle
- Flexibility and extensibility for enterprise customization
- Multi-cloud and hybrid-cloud support - no comparable alternative exists for multi-cloud deployments
- Large and growing ecosystem
- Ability to scale from experimentation to enterprise production
- Community Distribution initiative strengthening the release and adoption story
- Strong relevance in regulated sectors where vendor neutrality and auditability matter

## Project Improvements

### Is there something you feel that holds the project back from reaching its ultimate potential?

The primary challenge is operational complexity, though it has improved significantly over the years. Deploying and operating the platform at enterprise scale still requires substantial Kubernetes expertise. Reducing this barrier would lower adoption for many organizations.

Two specific structural gaps:
- **Helm support**: Lack of official Helm chart support adds friction for organizations that have standardized on Helm. Work in progress via GSoC.
- **Standardized user management pipeline**: Each organization currently builds its own user management and onboarding automation. A reusable pattern would reduce duplicated effort across adopters.

### In your opinion, what could the project improve?

- Simplified installation and upgrades, specifically Helm support
- More streamlined lifecycle management and release automation
- Stronger observability and monitoring capabilities out-of-the-box
- Standardized user management pipeline

## Maturity Level Survey

Q1. Do you feel you have a good understanding of the meaning of each CNCF maturity level?

Good understanding of CNCF maturity levels and their significance.

Q2. Is there information missing regarding the meaning of each different level?

No significant gaps identified. CNCF Graduation is seen as a clear signal of governance maturity and project quality.

Q3. Do you rely on those levels internally in any way, and if yes how?

Yes. Graduation provides more confidence in governance and maturity, and is viewed as a signal that a project is a safe long-term investment. The maturity level carries weight when evaluating and advocating for the project internally and externally.
