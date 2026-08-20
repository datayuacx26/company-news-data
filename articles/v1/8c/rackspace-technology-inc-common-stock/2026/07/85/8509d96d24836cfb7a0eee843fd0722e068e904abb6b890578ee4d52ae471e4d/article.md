---
schema_version: "1.0.0"
document_id: "8509d96d24836cfb7a0eee843fd0722e068e904abb6b890578ee4d52ae471e4d"
company_key: "rackspace-technology-inc-common-stock"
company: "Rackspace Technology Inc."
source_id: "rackspace-technology-inc-common-stock-news-import-038771c82b17"
canonical_url: "https://spot.rackspace.com/blog/kubernetes-security"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-22T10:47:01.830546+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:3d727078a73deded30ece56b7a98804fb8b218e8f8b47e7a19400d4643821e98"
---

# Kubernetes Security: Risks, Best Practices, and Common Threats

In March 2025, the Kubernetes Security Response Committee[disclosed a series of critical vulnerabilities affecting ingress-nginx](https://kubernetes.io/blog/2025/03/24/ingress-nginx-cve-2025-1974/) , one of the most widely deployed Ingress controllers for Kubernetes. The vulnerabilities were originally discovered by[Wiz Research](https://www.wiz.io/blog/ingress-nginx-kubernetes-vulnerabilities) , who collectively named the attack chain "IngressNightmare." The most severe vulnerability,[CVE-2025-1974](https://nvd.nist.gov/vuln/detail/CVE-2025-1974) , received a CVSS score of 9.8 and could allow an unauthenticated attacker with network access to execute arbitrary code and potentially compromise an entire Kubernetes cluster. Because ingress-nginx is used across thousands of production environments, the disclosure prompted immediate security advisories from Kubernetes maintainers, cloud providers, and cybersecurity organizations worldwide.


The incident is a reminder that Kubernetes security extends far beyond protecting containers. A Kubernetes cluster consists of interconnected components, including the control plane, worker nodes, APIs, workloads, networking, and identities, each introducing its own[security considerations.](https://kubernetes.io/docs/concepts/security/) While Kubernetes provides powerful built-in[security capabilities](https://kubernetes.io/docs/concepts/security/cloud-native-security/) , they must be configured and maintained correctly to effectively reduce risk.


As Kubernetes continues to power modern cloud-native applications, securing clusters has become a fundamental operational responsibility. According to the[2025 CNCF Annual Survey](https://www.cncf.io/reports/cncf-annual-survey-2025/) , 82% of organizations using containers now run Kubernetes in production, reinforcing its position as the industry's leading container orchestration platform. As adoption grows, so does the importance of protecting clusters from misconfigurations, vulnerable software, unauthorized access, and evolving supply chain threats.


This guide explains what Kubernetes security is, why securing Kubernetes requires a defense-in-depth approach, the most common security risks affecting Kubernetes clusters, and the best practices for protecting workloads throughout the software lifecycle. You'll also learn about the 4 C's of Kubernetes Security and explore recent security enhancements introduced in Kubernetes, helping you build more resilient and secure production environments.


## What Is Kubernetes Security?


[Kubernetes security](https://kubernetes.io/docs/concepts/security/) is the practice of protecting a Kubernetes cluster, its infrastructure, workloads, and data from unauthorized access, misconfigurations, and cyber threats. It combines policies, configurations, and security controls that safeguard every layer of a Kubernetes environment, from the control plane and worker nodes to the containers and applications running inside the cluster.


Effective Kubernetes security spans three core layers:


- **Control plane:** Protects Kubernetes components such as the API server, etcd, scheduler, and controller manager, which coordinate and manage the cluster
- **Worker nodes:** Secures the operating system, kubelet, container runtime, and network configuration on every node
- **Workloads:** Protects pods, containers, service accounts, secrets, and application traffic using controls such as Role-Based Access Control (RBAC), NetworkPolicies, Pod Security Admission, and runtime security


Because these layers work together, leaving one improperly secured can expose the entire cluster.


Managed Kubernetes platforms such as[Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) ,[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/fr-fr/products/kubernetes-service) ,[Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview) , and[Rackspace Spot Managed Kubernetes](https://spot.rackspace.com/) **** simplify cluster operations by managing the Kubernetes control plane, including upgrades, high availability, and security patching.


However, managed Kubernetes does **not** secure everything. You're still responsible for configuring workload security, identity and access management, network policies, secrets management, container image security, and runtime protection inside the cluster. This shared responsibility model is why Kubernetes doesn't become secure simply because it's running on a managed platform.


## Why Does Kubernetes Need Dedicated Security?


The IngressNightmare vulnerabilities demonstrated that even mature Kubernetes environments can be exposed when critical components are left unpatched or misconfigured *‍* . However, incidents like these are rarely caused by a single vulnerability alone. They often result from a combination of software flaws, excessive permissions, insecure configurations, or weak operational practices.


Kubernetes is fundamentally different from traditional infrastructure. Instead of protecting a single operating system or virtual machine, administrators must secure a distributed platform made up of APIs, control plane components, worker nodes, containers, service accounts, secrets, and network communications *‍* . Every component introduces its own attack surface, and a weakness in one layer can create opportunities for attackers to move laterally across the cluster.


This complexity is why Kubernetes security requires a defense-in-depth approach. Rather than relying on a single security control, organizations should apply multiple layers of protection, including strong identity and access management, network segmentation, workload isolation, secure software supply chains, continuous vulnerability management, and runtime monitoring *‍* .


## Kubernetes Isn't Secure by Default


Kubernetes provides a comprehensive set of security features, but most of them require deliberate configuration before they become effective. A newly deployed cluster is functional, not fully hardened. Administrators must configure access controls, network isolation, workload security, and data protection based on their organization's requirements.


Several default behaviors contribute to Kubernetes' attack surface:


- [The Kubernetes API server](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/) is the primary entry point for managing the cluster and one of its most critical components to secure.
- [Role-Based Access Control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) provides fine-grained authorization, but overly permissive roles or excessive privileges can significantly increase security risk.
- **Network communication is unrestricted by default.** Without[NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/) resources, pods can generally communicate with one another unless restricted by the cluster's networking implementation.
- **Containers can run with elevated privileges** unless administrators explicitly configure[Security Contexts](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) ,[Pod Security Admission](https://kubernetes.io/docs/concepts/security/pod-security-admission/) , or other workload security controls.


These defaults don't make Kubernetes insecure. Instead, they provide flexibility for a wide range of workloads and deployment models. Securing a Kubernetes cluster requires administrators to intentionally configure the controls that best match their operational and security requirements.


The sections below explore the most common Kubernetes security risks and the best practices for mitigating them.


## Common Kubernetes Security Issues


Every Kubernetes cluster faces security risks, but the majority of incidents can be traced back to a handful of recurring misconfigurations and operational gaps. Understanding these issues is the first step toward building a more secure Kubernetes environment.


### Unauthorized Access and Privilege Escalation


Misconfigured[Role-Based Access Control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) is one of the most common causes of privilege escalation in Kubernetes. Overly permissive roles, wildcard permissions, and service accounts granted unnecessary administrative privileges can allow an attacker who compromises a single workload to gain broader access across the cluster.


The best defense is the[principle of least privilege](https://kubernetes.io/docs/concepts/security/rbac-good-practices/) , granting users, service accounts, and applications only the permissions they need to perform their intended tasks. RBAC policies should also be reviewed regularly to identify unused roles, excessive permissions, and stale service account bindings.


‍


### Insecure API Server Configuration


The[Kubernetes API server](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/) is the primary entry point for managing a cluster, making it one of the most critical components to secure. Every request, from deploying workloads to retrieving Secrets, passes through the API server.


Protecting the API server involves enforcing TLS for all communications, enabling appropriate[authentication and authorization](https://kubernetes.io/docs/concepts/security/controlling-access/) mechanisms, configuring[admission controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) to validate incoming requests, and restricting API access to trusted networks wherever possible.


‍


### Vulnerable Container Images and Software Supply Chain Risks


Container images are the foundation of Kubernetes workloads. If an image contains known vulnerabilities or originates from an untrusted registry, every workload deployed from that image inherits those risks *‍* .


One of the best-known examples is[XZ Utils (CVE-2024-3094)](https://www.cve.org/CVERecord?id=CVE-2024-3094) . In 2024, researchers discovered a sophisticated backdoor that had been intentionally inserted into the XZ compression library before it reached stable Linux distributions. Had the compromised package been incorporated into container images and deployed to production, every workload built from those images would have inherited the malicious code. The incident demonstrated that modern software supply chain attacks can begin long before an application is deployed to Kubernetes.


Modern Kubernetes security practices combine Software Bills of Materials (SBOMs), image signing, and continuous vulnerability scanning throughout the CI/CD pipeline to identify risks before workloads reach production. Because new vulnerabilities are disclosed every day, images should be scanned continuously rather than only during the initial build process.


‍


### Network Policy Misconfigurations


By default, Kubernetes places no restrictions on pod-to-pod communication unless[NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/) **** resources are defined and supported by the cluster's Container Network Interface (CNI) plugin.


Without network segmentation, a compromised workload may be able to communicate with databases, internal APIs, or workloads running in other namespaces, increasing the potential for lateral movement.


A common best practice is to implement a[default-deny](https://kubernetes.io/docs/concepts/services-networking/network-policies/#default-policies) NetworkPolicy and explicitly allow only the traffic required by each application.


For organizations using Rackspace Spot Managed Kubernetes, clusters can be deployed with **Cilium** as the Container Network Interface (CNI), providing advanced NetworkPolicy enforcement, eBPF-powered networking, and enhanced network observability *‍* .


‍


### Secrets and Etcd Data Exposure


[Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) are Base64-encoded by default, not encrypted. Base64 is an encoding format designed for data representation, not for protecting sensitive information.


To protect credentials and other sensitive data, enable[encryption at rest](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/) for Secrets, restrict direct access to **etcd** , and integrate with external secret management solutions such as cloud key management services or HashiCorp Vault where appropriate.


‍


### Runtime Threats and Visibility Gaps


Preventative controls reduce risk, but they cannot eliminate it entirely. Runtime security focuses on detecting suspicious activity after workloads have been deployed.


Tools such as[Falco](https://falco.org/) detect suspicious runtime behavior by monitoring system calls and other indicators of compromise. Modern eBPF-based tools such as[Tetragon](https://tetragon.io/)) provide deeper runtime visibility by observing process execution, file access, network activity, and security events directly from the Linux kernel with minimal overhead. Meanwhile,[Kubernetes Audit Logs](https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/) record API activity, helping security teams investigate unauthorized actions and maintain an audit trail of changes made to cluster resources.


Maintaining a secure Kubernetes environment also requires keeping cluster components, including the kubelet, container runtime, and CNI plugins, fully patched. Regular vulnerability monitoring and timely updates help reduce exposure to[newly disclosed security vulnerabilities](https://kubernetes.io/docs/reference/issues-security/official-cve-feed/) .


Understanding these common security issues is only the first step. The next section explains the security best practices that should be applied throughout the software lifecycle, from designing applications and building container images to deploying and operating workloads in production.


## Kubernetes Security Best Practices by Phase


Security should be built into every stage of the Kubernetes lifecycle, from designing applications to monitoring workloads in production. Applying security controls early reduces vulnerabilities before they reach production while improving your ability to detect and respond to threats later.


### Design and Development Phase


Design security into your cluster from the start by defining namespace boundaries, RBAC roles, and workload isolation before applications are deployed. Whether you separate workloads by team, environment, or dedicated clusters, establishing these boundaries early is far easier than retrofitting them later. *‍*


‍


### Build Phase


Build workloads from minimal, trusted base images and automatically scan every image before it's pushed to a registry. Tools such as Trivy, Clair, Snyk, and Anchore help identify known vulnerabilities, while image signing with[Sigstore Cosign](https://docs.sigstore.dev/quickstart/quickstart-cosign/) helps verify that images come from trusted build pipelines.


‍


### Deployment Phase


Deployment is the last opportunity to prevent insecure workloads from reaching production. Admission controllers can block privileged containers, unsigned images, or workloads that violate organizational policies, while tools such as[OPA Gatekeeper](https://open-policy-agent.github.io/gatekeeper/website/) and[Kyverno](https://kyverno.io/) help enforce security policies consistently across every deployment. *‍*


‍


### Runtime Phase


Once workloads are running, continuous monitoring becomes essential. Combine runtime detection tools such as Falco and Tetragon with[seccomp](https://kubernetes.io/docs/tutorials/security/seccomp/) , SELinux, or AppArmor to detect suspicious activity, limit container capabilities, and reduce the impact of a compromised workload. Regular patching and vulnerability management should also be part of your runtime security strategy.


## The 4 C's of Kubernetes Security


A widely used framework for understanding Kubernetes security is the **4 C's** : **Cloud, Cluster, Container, and Code** . These four layers represent the different parts of the Kubernetes stack that must be secured, from the underlying infrastructure to the applications running inside containers.


- **Cloud** : The infrastructure and managed services that host your Kubernetes cluster.
- **Cluster** : The Kubernetes control plane, worker nodes, networking, and cluster configuration.
- **Container** : The container runtime, images, and workload configurations.
- **Code** : The application and its dependencies running inside each container.


Together, these layers provide a defense-in-depth approach to Kubernetes security. A vulnerability at any layer can affect the others, so organizations should apply security controls throughout the stack rather than relying on a single line of defense. As Kubernetes continues to evolve, new security features and best practices are regularly introduced, making it important to review and update your security posture over time.


## Recent Kubernetes Security Developments


**Section last updated:** July 2026


Kubernetes security continues to evolve with every release. Recent updates have introduced stronger authentication, improved authorization, better workload isolation, and enhanced API protections. The table below highlights several security features that have recently graduated to stable and the security challenges they address.


Feature Status What it addresses


Bound ServiceAccount Token Improvements Stable Issues short-lived, audience-bound tokens to reduce the impact of leaked service account credentials.


Native Sidecar Containers Stable Introduces a dedicated sidecar container type with predictable startup and shutdown behavior.


Recursive Read-Only Mounts Stable Ensures entire mounted directory trees remain read-only, preventing writes through nested mount points.


Restricted Anonymous API Access Stable Limits unauthenticated access to Kubernetes API endpoints, reducing unnecessary exposure.


Fine-Grained Authorization via Selectors Stable Allows authorization policies to grant access based on resource labels or field selectors, enabling more granular permissions.


Fine-Grained Kubelet API Authorization Stable Replaces broad` nodes/proxy` permissions with more granular authorization for kubelet API endpoints.


Ordered Namespace Deletion Stable Addresses CVE-2024-7598 by ensuring NetworkPolicy resources remain in place until protected workloads are removed.


Understanding these improvements helps teams take advantage of the latest security capabilities, but keeping a Kubernetes environment secure is an ongoing process. The best defense combines current platform features with strong security practices throughout the application lifecycle.


### What's Coming Next


Several Kubernetes security enhancements are still progressing through **Alpha** and **Beta** and are expected to mature in future releases:


- **Kubelet serving certificate hardening** *(Alpha)* – Strengthens validation of kubelet serving certificates to reduce the risk of node impersonation.
- **Constrained impersonation** *(Beta)* – Introduces more granular controls over Kubernetes impersonation, reducing opportunities for privilege escalation.
- **Robust image pull authorization** *(Beta)* – Improves image pull security by ensuring registry credentials are validated before cached private images can be reused.
- **Pod certificates for mTLS** *(Beta)* – Introduces native X.509 certificates for Pods, simplifying workload identity and mutual TLS between services.


Kubernetes releases a new minor version approximately every four months. As new features graduate from Alpha to Beta to Stable, organizations should regularly review and adopt the security capabilities most relevant to their environments.


## Conclusion


Kubernetes security is an ongoing process, not a one-time configuration. By securing your cluster at every stage, from development and deployment to runtime, you can reduce your attack surface and better protect applications from evolving threats.


Start with the fundamentals: review RBAC permissions, implement default-deny network policies, secure Secrets, scan container images, and enable runtime monitoring. These best practices provide a strong foundation for building resilient Kubernetes environments.


## Frequently Asked Questions


### What are the security concerns of Kubernetes?


**‍** Most Kubernetes security incidents trace back to one of five root causes: a control plane left reachable without proper hardening, access roles that grant more than a workload needs, images deployed without a vulnerability check, pod-to-pod traffic left open by default, and credentials stored without encryption at rest. Each one maps to a specific, fixable control, covered in detail earlier in this guide, rather than a vague platform weakness.


‍


### How do you maintain security in Kubernetes?


**‍** Enforce RBAC with least-privilege role bindings, apply default-deny network policies, scan container images continuously through CI/CD, encrypt communication and secrets, and monitor workloads at runtime with tools like Falco. Maintaining security is a continuous practice, not a one-time setup step.


‍


### What are the 4 C's of Kubernetes security?


**‍** The 4 C's are Cloud, Cluster, Container, and Code, four layers that move from the underlying infrastructure inward to the application itself. See the section above for what each layer covers in practice.


‍


### Is Kubernetes secure by default?


**‍** No. Kubernetes ships with security features available, but it does not enable a secure posture automatically. RBAC, network policies, and encryption at rest all require deliberate configuration before they protect anything.


‍


### Are Kubernetes Secrets encrypted?


**‍** No. Secrets are base64-encoded by default, which is an encoding format, not encryption. Encryption at rest for Secrets has to be explicitly enabled at the API server level.


‍


### What is the difference between Kubernetes security and container security?


**‍** Container security concerns the individual image and runtime, things like base image vulnerabilities and runtime isolation. Kubernetes security is the broader discipline covering the cluster, the orchestration layer, and the networking connecting many containers together.
