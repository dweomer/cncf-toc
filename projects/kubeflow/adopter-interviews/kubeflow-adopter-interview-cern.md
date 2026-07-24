# Kubeflow Adopter Interview - CERN

**Organization**: CERN (European Organization for Nuclear Research)  
**Industry**: Fundamental scientific research / high-energy physics  
**Interview Date**: June 8, 2026  
**Interviewers (TOC)**: Faseela K  
**Interviewees**: Ricardo Rocha, Raulian Chiorescu (CERN)  
**Reference Architecture**: https://architecture.cncf.io/architectures/cern-scientific-computing/  


## Organization Intro

CERN is the European Organization for Nuclear Research, one of the world's largest scientific research centres. It operates the Large Hadron Collider and runs fundamental physics research, employing thousands of scientists and engineers worldwide. CERN runs significant computing infrastructure to process and analyze data from physics experiments, making it a large-scale Kubernetes and cloud-native operator.


## 1. How long has your organization used the project?

CERN began evaluating Kubeflow in 2019-2020. Limited production usage started in 2021, initially focused on notebooks, distributed training and model serving via KServe. Usage expanded in 2024 following an upgrade to Kubeflow 1.9, which added Notebooks and broader KServe adoption to the production workload.


## 2. What were the main motivations to adopt the project and which key features do you use today?

CERN had home-built systems for ML workloads but needed a single solution that covered the majority of use cases. Kubeflow offered that breadth. Key features currently in use:

- **Notebooks**: interactive access to GPUs for data scientists and ML researchers
- **Model Serving (KServe)**: most popular feature at CERN
- **Distributed Training**: large-scale model training workloads
- **Hyperparameter Optimization (Katib)**: still actively used
- **Pipelines**: ML workflow orchestration, and other workflow automation


## 3. Compared with other products and projects in this space (proprietary and open), what drew you to the project?

Kubeflow was selected because it fills a unique gap in the ecosystem: it bridges the cloud-native (Kubernetes) world with the higher-level ML tooling world. CERN evaluated other options including home-built systems, but Kubeflow's breadth of coverage across the ML lifecycle made it the strongest fit. Its Kubernetes-native design meant integration with CERN's existing infrastructure was relatively easy at the infrastructure level.


## 4. What is the current level of usage (pre-production, production) and scale?

**Level**: Production  
**Users**: Hundreds of users on the primary instance; one instance is expected to scale to thousands of users  
**GPU Capacity**: Hundreds of GPUs currently; expected to roughly double by end of year  
**Reference**: https://architecture.cncf.io/architectures/cern-scientific-computing/


## 5. What version of the project is currently in use and what is your update cadence with the project?

Currently running **Kubeflow 1.9.2**. CERN is working toward upgrading to 1.11, though the cadence has slowed as the user base has grown. Earlier in the adoption, upgrades happened faster due to a smaller user footprint. Upgrades are sometimes done at the component level (e.g., upgrading KServe independently) rather than as a full platform upgrade.


## 6. Can you walk me through the experience of adopting or integrating Kubeflow? What challenges did you experience?

**Integration** with existing Kubernetes infrastructure was relatively easy because Kubeflow uses standard Kubernetes primitives.

**Adoption**, however, was not easy at the start:

- **Installation complexity**: Kubeflow uses Kustomize, not a Helm chart. This is very different from other deployments at CERN. The overlay format changed multiple times in the first two years, delaying upgrades. CERN would strongly prefer a Helm chart; Kustomize-based installation is difficult to teach to newcomers and feels like "a bunch of scripts, not declarative."
- **Istio dependency**: Kubeflow's hard dependency on Istio was a significant challenge. CERN does not otherwise use Istio, and it was unclear why Istio was required rather than a standard ingress. This represented a major learning curve and integration effort.
- **Documentation gaps for service managers**: Upstream documentation is better suited for end users than for the administrators and service managers responsible for operating the platform at scale.


## 7. Did you find the information in the repo or the project docs valuable to your implementation?

From an end-user perspective, CERN has developed its own internal documentation. Users increasingly rely on AI tools to generate configurations, which often fails because the tooling doesn't accurately reflect current Kubeflow docs. Upstream documentation could be improved from a service manager and operator perspective; this was identified as one of the areas where investment would have the most impact.


## 8. Did you need to engage with the community members or maintainers? If so, what was the context and outcome?

Yes, CERN actively engages with the community:

- **Communication channels used**: CNCF Slack (`#kubeflow-*` channels), online WG meetings, general community call, mailing list
- **KubeCon presence**: CERN volunteers at the Kubeflow booth, conducts live demos, and presents at conferences:
  - *KubeCon EU Amsterdam 2026*: Kubeflow as the backbone for GPU access / MLOps
    https://kccnceu2026.sched.com/event/3ac486a177945dc278b23b3627edac6b
  - *KubeCon EU Amsterdam 2026*: Kubeflow monitoring stack (being contributed upstream)
    https://kccnceu2026.sched.com/event/2CW6t/collisions-in-the-dark-illuminating-the-95-of-kubeflow-you-cant-see-amine-lahouel-laura-llinares-cern
    (Upstream contribution: https://github.com/kubeflow/manifests/issues/3426)
  - *Cloud Native AI + Kubeflow Day Amsterdam 2026*: Running ML challenges on Kubernetes
    https://colocatedeventseu2026.sched.com/event/2DY6O/running-ml-challenges-on-kubernetes-part-2-hannes-hansen-paulo-guilherme-pinheiro-pereira-cern
  - *Kubeflow Summit London 2025*: Running ML Challenges on Kubeflow
    https://colocatedeventseu2025.sched.com/event/8ece7347102293e529d1c764d9971791

Community is perceived as easy to interact with and diverse.


## 9. Has your implementation of the project provided measurable value?

Yes, specifically:

- **Declarative API for ML workloads**: previously very difficult to achieve at CERN's scale
- **Resource sharing** across multiple users and ML workloads on shared GPU infrastructure
- **HPC integration**: Kubeflow now bridges CERN's HPC workloads and cloud-native ML pipelines
- **GPU utilization and cost savings**: better scheduling and multi-user GPU access reduces idle capacity
- **Access democratization**: enabling interactive GPU access for data scientists and ML researchers with no prior Kubernetes knowledge, via the Kubeflow SDK


## 10. If the project were to be archived, what level of difficulty would your organization experience to remove it?

Kubeflow is deeply embedded in CERN's infrastructure. Difficulty would vary by component:

- **Inference (KServe)**: Relatively easier to migrate; standard OpenAI-compatible APIs mean CERN is not tightly coupled to a specific backend
- **Training, notebooks, and interactive sessions**: Much harder; these are tightly integrated with CERN's GPU allocation model and user workflows
- **Overall**: Given Kubeflow's role as the planned core of CERN's AI strategy for the next 3-5 years, full removal would be a significant multi-year effort. CERN would likely consider stepping into a maintainership and community management role to preserve functionality before accepting full migration to alternatives.


## 11. Is there something that holds the project back from reaching its ultimate potential?

- **Limited vendor ecosystem**: At KubeCon events, CERN primarily sees end users; few vendors or commercial offerings are visibly supporting Kubeflow. Greater vendor visibility would strengthen the project's reputation and adoption by organizations that require commercial support options.
- **Deployment complexity**: Kustomize-based installation adds complexity that can slow down new adopters. CERN has adapted to the existing manifests over time and sees Helm support as the clearest path to lowering the entry barrier. The community is moving in this direction — several subprojects already ship Helm charts, and a GSoC 2026 project is actively working on Helm support for the community distribution. CERN plans to contribute to this effort. Earlier community-driven Helm alternatives (deployKF, treebeard) were also explored but did not reach maturity.
- **Service manager documentation**: Upstream docs are adequate for end users but insufficient for operators managing multi-tenant production deployments.
- **Evolution speed**: Kubeflow has many tightly-coupled components, which means adapting to large AI/ML shifts (e.g., new model paradigms) is inherently slower than for more modular projects.


## 12. In your opinion, what could the project improve?

- **Helm chart for installation**: would dramatically improve adoption by organizations following standard Kubernetes deployment patterns
- **Service manager documentation**: dedicated operator/admin guides are largely missing upstream
- **Security: CVE response across interconnected components**: Kubeflow's interconnected subprojects mean a CVE in a dependency (e.g., KServe) can block the entire platform. A security best practices document for operators would be valuable; the level of isolation between components was poor but is improving over time.
- **Security: inference endpoint multi-tenancy**: currently CERN is affected by an upstream KServe/Knative issue with cross-namespace authorization of inference endpoints:
  https://github.com/knative/serving/issues/12533
  (Not yet tracked as a Kubeflow issue, but documented in the manifests repo under "Cross-namespace Access Control" at https://github.com/kubeflow/manifests/blob/master/common/istio/cluster-local-gateway/overlays/m2m-auth/README.md)
- **Pipeline artifact multi-tenancy**: open issue: https://github.com/kubeflow/pipelines/issues/11760
- **MLMD component maintenance**: The upstream MLMD component is no longer actively maintained and relies on the deprecated `mysql_native_password` authentication method, blocking MySQL backend upgrades at CERN
- **Monitoring stack**: CERN's monitoring stack for Kubeflow is being contributed upstream (https://github.com/kubeflow/manifests/issues/3426)
- **User onboarding via SDK**: The Kubeflow SDK (https://github.com/kubeflow/sdk) is making onboarding easier for data scientists and ML researchers with no Kubernetes background by abstracting Kubernetes concepts into Python; continued investment here would broaden adoption further.


## 13. What are the overall strengths of the project?

- **Ecosystem positioning**: Kubeflow bridges cloud-native Kubernetes and higher-level ML tooling; no direct equivalent covers the same breadth
- **Declarative API for ML workloads**: Enables structured, repeatable ML workflows at scale
- **Multi-user resource sharing**: Enables efficient GPU sharing across teams and user communities
- **Community**: Open, diverse, and accessible; responsive to engagement
- **Breadth of coverage**: Covers the full ML lifecycle from interactive development (Notebooks) through training, HPO (Katib), pipelines, and serving


## 14. Do you have any future plans regarding the project? More involvement, feature requests, expansion, etc.?

- **Current contributions**: 4 CERN team members have contributed upstream, including:
  - https://github.com/kubeflow/manifests/pull/3264
  - Monitoring stack contribution (in progress): https://github.com/kubeflow/manifests/issues/3426
- **Strategic roadmap**: Kubeflow is planned as a core component of CERN's AI strategy for the next 3-5 years, serving as the standard platform for interactive GPU access and ML operations across different scientific communities
- **Scale expansion**: CERN plans to extend Kubeflow-based GPU access beyond the current ML community to thousands of users across other scientific domains
- **Maintainer involvement**: Ricardo and Raulian are open to exploring deeper maintainer involvement in the future as CERN's contribution footprint grows


## Maturity Level Survey

1. **Do you feel you have a good understanding of the meaning of each CNCF maturity level?**

   Yes. Ricardo is a member of the CNCF End User Technical Advisory Board (TAB) and is well-versed in the CNCF project maturity framework, including the criteria and expectations at Sandbox, Incubating, and Graduated levels.

2. **Is there information missing regarding the meaning of each different level?**

   The definitions are generally well understood. The graduation criteria are clearly documented and the DD process provides transparency into what is evaluated. No significant gaps noted.

3. **Do you rely on those levels internally in any way, and if yes how?**

  Yes. CERN uses CNCF maturity levels as a signal of project health and long-term viability when evaluating technologies for adoption. Graduation status is a meaningful indicator for infrastructure decisions at CERN - it reflects that a project has demonstrated production readiness, governance maturity, and a sustainable community, all of which are important to an organization running critical scientific infrastructure.

