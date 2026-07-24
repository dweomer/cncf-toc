# Kubeflow Adopter Interview - Adopter 1

**Organization**: Adopter 1 (anonymized)  
**Industry**: Financial services / banking  
**Interview Date**: June 10, 2026  
**Interviewers (TOC)**: Faseela K  


## Organization Intro

Adopter 1 maintains a machine learning platform for internal data scientists where thousands of data scientists, machine learning engineers, and business analysts train and serve machine learning models. The organization has extremely strict regulatory requirements that shape how it evaluates and adopts open-source technology.


## 1. How long has your organization used the project?

Adopter 1 has been using Kubeflow for approximately **4-4.5 years**, including an initial research/evaluation period before going to production.


## 2. What were the main motivations to adopt the project and which key features do you use today?

Before Kubeflow, Adopter 1 was assembling a collection of otherwise disconnected products and effectively rebuilding the same functionality piece by piece. The team decided to stop reinventing the wheel and adopt Kubeflow as a unified ML platform.

Key features currently in use: **Notebooks, Pipelines, Trainer, Katib** - all components of Kubeflow are used. Adopter 1 is a heavy user of Pipelines specifically, with plans to add ADOPTERS.md entries for other subprojects beyond the existing Pipelines entry.


## 3. Compared with other products and projects in this space (proprietary and open), what drew you to the project?

Two primary differentiators:

1. **Regulatory compliance and cloud agnosticism**: Adopter 1 operates under extremely strict regulatory requirements that preclude many managed cloud services. Kubeflow's open-source, self-hosted, cloud-agnostic nature (with full deployment control) was a key differentiator.
2. **Full ML lifecycle coverage**: While alternative tools exist for individual components (notebook provisioners, workflow orchestrators, distributed compute abstractions), nothing else consolidates the full model development lifecycle into one ecosystem - no comparable alternative covers the full scope.


## 4. What is the current level of usage (pre-production, production) and scale?

**Level**: Production (both experimentation and production workloads)  
**Daily active users**: several thousand (non-prod), with additional production usage  
**Workloads**: large numbers of concurrent workflows; business-critical ML models are trained and served on Kubeflow  
**Scale**: large distributed compute workloads


## 5. What version is used and what is your update cadence with the project?

Adopter 1 tracks the Kubeflow Community Distribution releases closely and updates **every 1-3 months**.

**Upgrade experience**: Adopter 1 experienced regressions during the Kubeflow v2 release cycle and contributed many fixes directly upstream. That period is considered an anomaly; the project is now considered to be in much better hands with improved upgrade processes for v3.


## 6. Can you walk me through the experience of adopting or integrating Kubeflow? What challenges did you experience?

Integration with existing infrastructure was manageable. Adopter 1 is very familiar with Istio, so Kubeflow's Istio dependency was never a blocker.

The primary challenges arose from unique security and regulatory requirements - gaps that upstream Kubeflow did not address out of the box. Addressing those gaps directly led the organization to become significant contributors and maintainers to the project, with multiple notable upstream contributions to Kubeflow Pipelines.


## 7. Did you find the information in the repo or the project docs valuable to your implementation?

Yes. Both the official docs and the generated SDK docs were described as excellent resources:

- Official docs: https://www.kubeflow.org/docs/components/pipelines/
- SDK docs: https://kubeflow-pipelines.readthedocs.io/en/sdk-2.16.1/

Adopter 1 used these extensively as the basis for creating its own internal documentation for its user base. Some upstream docs may be complex for newcomers, but they were reliable and accurate for the team's implementation needs.


## 8. Did you need to engage with the community members or maintainers?

Yes, extensively. Adopter 1 communicates with maintainers primarily through **CNCF Slack**. Maintainers are described as very responsive. The community is characterized as open, friendly, and welcoming.

Adopter 1's level of engagement goes well beyond typical end-user interaction:

- Multiple contributors are active in the project
- Multiple maintainers participate, including at least one in Pipelines
- Active participation in **community calls**
- Presentations at KubeCon + CloudNativeCon and SREcon conferences, including topics covering lessons learned from consumer to contributor, managing upstream forks, and Kubeflow in cloud-native AI
- Adopter 1 has hosted Kubeflow community events


## 9. Has your implementation of the project provided measurable value?

Yes. Some of the organization's highest-value ML models are trained and served via Kubeflow. Specific metrics cannot be shared due to confidentiality, but the team reports:

- Elimination of the fragmented, manually assembled toolchain that preceded Kubeflow
- Significant productivity gains for thousands of data scientists and ML engineers
- Much of the value generated has been generalized and contributed back upstream


## 10. If the project were to be archived, what level of difficulty would your organization experience to remove it?

High difficulty. No single alternative provides the integrated ML lifecycle coverage that Kubeflow does. Individual components could be replaced (e.g., Raytrain for distributed training), but nothing currently consolidates the full ecosystem.

Given the scale (thousands of users, high workflow volume, critical production ML models), full removal would be a significant multi-year effort. The organization has existing maintainer investment in the project and would likely step further into a maintainer/steward role rather than abandon the project.


## 11. Is there something that holds the project back from reaching its ultimate potential?

- **Maintainer depth**: Maintainer coverage for Pipelines has improved under the current shared ownership model. This has worked well for the ecosystem, with materially higher contribution velocity and more new contributors than in earlier periods. Expanding the maintainer base across subprojects remains an active priority as the project continues to grow.
- **Helm support**: Helm chart support for deployment would be a meaningful improvement; the organization uses Helm internally and minimizes forks by upstreaming changes where possible.
- **UX for non-engineer users**: A simplified UX targeting data scientists and business analysts (rather than engineers) would broaden adoption and reduce onboarding friction.


## 12. In your opinion, what could the project improve?

1. **More frequent releases**: the current cadence can be too slow for organizations with aggressive security patching requirements; a regulated environment sometimes requires faster vulnerability response than upstream timelines allow, leading to internal forks
2. **Security response process**: a security best practices document for end users would be valuable beyond existing `SECURITY.md` coverage (Kubeflow is designed to be "secure by default"; that design pattern should be better documented)
3. **Maintainer growth**: actively recruiting maintainers across more subprojects would strengthen the ecosystem


## 13. What are the overall strengths of the project?

- **Ecosystem integration**: Kubeflow brings together an otherwise disconnected set of ML tools in a complex problem domain and provides a mostly unified experience
- **Vendor neutrality**: Truly cloud-agnostic and vendor-neutral, a key factor for regulated industries like financial services
- **Community**: Very open, friendly, well-managed, and inclusive; lots of growth opportunities; transparent governance and maintainer ladder
- **Governance**: Well-organized, diverse, and inclusive; the maintainer ladder is transparent and clearly documented
- **Regulatory fit**: Uniquely suited for organizations with strict compliance requirements due to full deployment control


## 14. Do you have any future plans regarding the project? More involvement, feature requests, expansion, etc.?

- **Maintainer growth**: Plans to onboard more contributors, members, and eventually more maintainers across additional Kubeflow subprojects
- **Future KFP releases**: Looking toward contributing to upcoming KFP releases
- **Reference architecture**: Interest in creating a reference architecture for the banking/financial services sector
- **ADOPTERS.md expansion**: Plans to add entries for Trainer, Katib, Notebooks, Spark Operator, Dashboard, and Model Registry in addition to the existing Pipelines entry


## Maturity Level Survey

1. **Do you feel you have a good understanding of the meaning of each CNCF maturity level?**

   Yes. Both interviewees are well-versed in the CNCF maturity framework and the criteria distinguishing Sandbox, Incubating, and Graduated projects.

2. **Is there information missing regarding the meaning of each different level?**

   The levels are well-understood. The Director noted that Kubeflow's graduation would increase industry credibility, suggesting that the Graduated designation carries meaningful signal for enterprise and regulated-industry adopters evaluating project maturity.

3. **Do you rely on those levels internally in any way, and if yes how?**

   Yes. CNCF maturity level is a factor in the organization's technology evaluation process. Graduation status signals production readiness, governance health, and long-term sustainability - all of which are particularly important for a regulated financial institution that cannot easily change core infrastructure components.

