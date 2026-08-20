---
schema_version: "1.0.0"
document_id: "30f5dfd7eac6551b4c080bd0f0be1ff0eff69cb0679bea7e39cbb304757e51aa"
company_key: "crowdstrike-holdings-inc-class-a-common-stock"
company: "CrowdStrike Holdings Inc."
source_id: "crowdstrike-holdings-inc-class-a-common-stock-rss-29758b507457"
canonical_url: "https://www.crowdstrike.com/en-us/blog/new-in-falcon-cloud-security-helping-security-teams-move-faster/"
published_at: null
first_seen_at: "2026-07-30T05:04:43.098733+00:00"
fetched_at: "2026-07-30T05:04:45.296663+00:00"
content_hash: "sha256:163dfc3212b377c7706bcf21ed2207b1bed4880a29a38b1d4ce915fc10e5d717"
---

# Falcon Cloud Security July 2026 Release: Helping Security Teams Move Faster in the Cloud

Every change in a cloud environment creates new security decisions.


A new infrastructure as code (IaC) template needs to be validated. Cloud permissions need to be reviewed. An application release introduces new cloud interactions. A Kubernetes cluster needs protection before it goes into production. Individually, these are routine tasks. Together, they create growing operational friction that makes cloud security harder to scale.


This month's CrowdStrike Falcon® Cloud Security innovations reduce that friction. New capabilities strengthen IaC security, streamline cloud identity investigations and remediation, provide deeper application context, expand agentless workload visibility, and simplify Kubernetes deployment. These updates, all of which are generally available, help security teams spend less time managing security operations and more time reducing cloud risk.


## Reduce Friction in Infrastructure Security


Cloud infrastructure is increasingly managed as code. Terraform templates, Kubernetes manifests, IAM policies, and cloud configurations define how cloud environments are built, making IaC one of the earliest opportunities to identify misconfigurations and vulnerabilities before infrastructure is deployed.


Falcon Cloud Security now brings IaC security **directly into Visual Studio Code and IntelliJ** . Teams receive immediate feedback as cloud infrastructure is authored so they can identify misconfigurations and policy violations before changes progress through deployment workflows. This feedback includes remediation guidance so issues can be resolved while infrastructure is built.


Organizations can also extend Falcon Cloud Security with **Custom Rego Rules** for IaC scanning. Security and platform teams can create organization-specific security and compliance policies alongside CrowdStrike's built-in detections. Custom rules integrate seamlessly with the Falcon Cloud Security CLI and surface in the Falcon console alongside native findings. This enables teams to consistently enforce infrastructure security standards and manage findings across cloud environments.


Figure 1. Creating custom Rego rules for IaC security


## Simplify Identity Governance


Cloud permissions continuously evolve as organizations deploy new applications, automate infrastructure, and integrate additional cloud services. Determining who has access to what — and whether that access is appropriate — often requires time-consuming investigation across identities, policies, and cloud resources.


Falcon Cloud Security expands cloud infrastructure entitlement management (CIEM) with capabilities that simplify investigation and remediation. **Permission Querying** extends Graph Explorer beyond existing risk findings so analysts can explore cloud identity relationships even when no security finding exists. Security teams can quickly answer operational questions such as which resources an identity can access, who has access to a particular resource, and how permissions are inherited across an environment.


CrowdStrike also introduces **least privilege remediation** . Rather than requiring administrators to manually create IAM policies, Falcon Cloud Security now generates least-privilege IAM policies based on observed permission usage with a single click. This helps organizations reduce excessive permissions faster while accelerating least-privilege adoption.


Figure 2. Explore identity permissions in Graph Explorer with context


## Understand Application Access to Cloud Resources


Modern applications increasingly interact with cloud infrastructure through privileged identities, cloud APIs, secrets, networking services, and infrastructure automation. Understanding application risk requires understanding how applications are designed to interact with these cloud resources.


Falcon Cloud Security expands application security posture management (ASPM) with capabilities that automatically provide that context. New **Admin Actions** correlate application code analysis with CIEM insights to identify the cloud control plane actions an application is designed to perform, including IAM administration, secrets management, networking, infrastructure provisioning, and access to sensitive data. Comparing intended behavior with granted permissions helps organizations identify excessive application privileges and assign remediation to the teams responsible for the application.


Falcon Cloud Security also introduces the first release of **API Findings** , which automatically discover inbound and outbound APIs directly from application source code. Support includes REST APIs, gRPC services, serverless functions, and message brokers. This gives security teams greater visibility into application communication paths, service dependencies, and API exposure without requiring manual documentation.


Figure 3. Investigating an application performing administrative behavior on cloud infrastructure


## Deploy and Protect Cloud Workloads Faster


Protecting cloud workloads shouldn't introduce additional operational complexity. Falcon Cloud Security now extends **Agentless VM Scanning** to Microsoft Azure, enabling organizations to assess Azure virtual machines for vulnerabilities without deploying agents. This expands existing AWS support while providing the same consistent agentless assessment workflow across cloud providers.


CrowdStrike is also introducing a new **Kubernetes Deployment Wizard** , which guides administrators through deploying Falcon Cloud Security in minutes across Amazon EKS, Azure Kubernetes Service (AKS), Google Kubernetes Engine (GKE), Red Hat OpenShift, and other CNCF-conformant Kubernetes environments.


The guided experience generates a production-ready installation script based on deployment selections. This reduces manual configuration while helping organizations deploy Kubernetes protection more consistently across cloud environments.


**See how to deploy Kubernetes protection in minutes:**


## Reduce Operational Friction Across Cloud Security


Every infrastructure deployment, identity change, application release, and workload rollout creates new security decisions. As cloud environments continue to evolve, the critical challenge is keeping pace with change without increasing operational complexity.


These Falcon Cloud Security enhancements reduce the day-to-day effort required to secure cloud environments. By simplifying infrastructure security, identity governance, application analysis, and workload protection, organizations can spend less time on repetitive operational tasks and more time reducing cloud risk.


#### Additional Resources


- *Watch the Falcon Cloud Security[product demo](https://www.youtube.com/watch?v=1S28SmZ7iag) to see these capabilities in action.*
- *Start an unlimited[15-day free trial](https://www.crowdstrike.com/en-us/products/trials/try-falcon-cloud-security/) of Falcon Cloud Security.*
- *Be part of[Fal.Con 2026](https://www.crowdstrike.com/en-us/events/fal-con/las-vegas/?utm_medium=evt&utm_source=blog&utm_campaign=fal-con-26&utm_term=crwdblogs&utm_language=en-us) and connect with 10,000+ cybersecurity professionals shaping the future of the industry.*
