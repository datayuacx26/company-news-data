---
schema_version: "1.0.0"
document_id: "c3a3a865d533194c045fd5e75c9bb676bbd6eea04c92bad92d715912db0a9d11"
company_key: "yc-brainboard"
company: "Brainboard"
source_id: "yc-brainboard-news-import-c1cb64b4ba70"
canonical_url: "https://www.brainboard.co/blog/complete-guide-on-kubernetes-network-policy"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-23T04:03:09.323194+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:179a5ee6c2eb62984eb2edbd0df8b98035750fc231f9596f257d80c91f5078f9"
---

# Complete Guide on Kubernetes Network Policy

By default, Kubernetes is open: any pod in the cluster can communicate freely with any other pod. This is convenient for development, but in production, it poses a security risk. Kubernetes network policy is a mechanism that allows you to control which traffic is permitted between pods and external services. Want to know how K8s network policy works? How to create rules? How to apply them securely in real-world environments? Read on, because the Brainboard team is about to answer all your questions.


## What Is a Kubernetes Network Policy


Kubernetes network policy is a set of rules that determines how pods can communicate with each other and with external services. Essentially, it’s a pod-level firewall: you specify which traffic is allowed, and everything else is blocked.


Rules are applied declaratively via YAML manifests and operate based on labels. The policy selects pods via a podSelector and defines the allowed incoming and outgoing connections for them. Kubernetes networking becomes manageable and predictable.


### How Default Kubernetes Networking Works


Without Kubernetes network policies, all pods in the cluster communicate freely. This simplifies development and debugging: you don’t need to think about rules; everything works.


The problem arises in production and multi-tenant environments. If a single service is compromised, an attacker gains direct access to the entire cluster - to databases, internal APIs, and other applications. Without traffic restrictions, “hacking one means hacking everything.”


### Where Network Policies Fit in the Security Model


Kubernetes network policy is one of the three key security layers in Kubernetes. RBAC controls who can interact with the Kubernetes API. Pod Security Standards define the privileges granted to containers at launch. Network Policy controls what traffic flows between workloads.


All three layers complement each other. Configuring only RBAC and considering the system secure is insufficient: traffic between pods remains unprotected.


## How Network Policies Work


The logic of Kubernetes networkpolicy is simple: a policy is applied to a group of pods via a label selector and explicitly describes which traffic is allowed. Anything not explicitly allowed is blocked. An important nuance: policies operate on a whitelist basis, not a blacklist.


If at least one policy is applied to a pod, all unauthorized traffic is automatically blocked. If there are no policies, the pod is open to all connections.


### Ingress and Egress Rules


Ingress rules control incoming traffic: who can connect to the pod. Egress rules control outgoing traffic: where a pod can connect. Both types can be combined in a single policy, giving you full control over communication paths. For example: a database accepts traffic only from backend pods (ingress) and does not connect anywhere itself (egress is completely blocked).


### Pod Selectors, Namespace Selectors, and IP Blocks


Selectors define the sources and destinations of traffic. podSelector selects pods by labels within the same namespace. namespaceSelector allows you to permit traffic from an entire namespace - useful for isolating environments. ipBlock works with specific IP ranges - used to control external traffic or integrate with on-premise systems. These tools can be combined to create precise and flexible rules.


### CNI Plugin Requirements (Calico, Cilium, Flannel)


An important point that is often overlooked: Kubernetes itself does not enforce network policies. They are enforced by a CNI (Container Network Interface) plugin. Calico and Cilium support the full set of network policy features. Flannel does not come in its default configuration. Before configuring policies, make sure your CNI supports them; otherwise, the manifests will be applied, but the rules will not work.


## Creating a Network Policy (YAML Examples)


The basic structure of a manifest includes several key fields. podSelector - which pods the policy applies to. policyTypes - a list of types: Ingress, Egress, or both. Next are the ingress and egress sections with rules. Kubernetes network policy examples below show the most common patterns.


### Deny All Ingress Traffic


The first step in securing a namespace is to block all incoming traffic by default. A policy with an empty podSelector (selects all pods) and an empty ingressRules array blocks all incoming connections. After that, you explicitly allow only what is truly necessary. This is the classic “default deny” approach - the foundation of zero-trust in Kubernetes.


### Allow Traffic from Specific Pods


A typical scenario: a database should only accept connections from a backend service. The policy applies to pods labeled app:database and allows ingress only from pods labeled app:backend. Any other pod - frontend, monitoring, third-party service - will not be able to connect directly to the database.


### Allow Traffic from a Namespace


namespaceSelector allows you to permit traffic between namespaces. For example, allow the staging namespace to access a shared service, but block direct access from dev to production. This is especially important in multi-tenant clusters where different teams work in isolated namespaces.


### Restrict Egress to Specific IPs


Egress policies restrict pods’ outgoing connections. Practical example: a microservice should access an external API only via a specific IP address. Using ‘ipBlock’, we specify the allowed CIDR range - everything else is blocked. This is critical for meeting security requirements when you need to ensure that data only goes to authorized destinations.


## Common Use Cases


Network policy management in real production systems addresses several common tasks: database isolation, environment separation, external traffic control, and the implementation of zero-trust architecture. Properly configured policies are not a one-time setup but an ongoing process.[Brainboard](https://www.brainboard.co/) lets you visually design network architecture and manage infrastructure-as-code, making this process much clearer.


### Isolate a Database to Backend Pods Only


The most common scenario. The database is the most sensitive component of the system, and only backend services should have direct access to it. A policy with an ingress rule based on a podSelector with the label ‘role: backend’ blocks all other paths - frontend, external requests, and neighboring services are left out.


### Block Cross-Namespace Communication


By default, pods from different namespaces communicate freely. A policy without a namespaceSelector in the ingress rules blocks all cross-namespace traffic. This isolates teams and environments from one another: dev cannot reach staging, and staging cannot reach production.


### Allow DNS While Blocking Everything Else


Completely blocking egress breaks DNS resolution, rendering the cluster inoperable. The correct approach is to block all egress traffic but explicitly allow UDP traffic on port 53 to the cluster’s DNS server (usually kube-dns in the kube-system namespace). This preserves cluster functionality while maintaining maximum isolation.


## Kubernetes vs Calico Network Policies


Standard Kubernetes network policies operate at the namespace level and below. Calico extends these capabilities: GlobalNetworkPolicy applies to the entire cluster without being tied to a namespace, supports DNS names in rules, provides more detailed traffic logging, and allows setting policy priorities. Standard policies are sufficient for basic tasks. For complex enterprise environments with audit and global isolation requirements, Calico is the obvious choice.


## Limitations of Network Policies


Network policies operate at the IP and port levels - they do not analyze traffic content (Layer 7). If control at the HTTP method or header level is required, a service mesh such as Istio is needed. Additionally, debugging policies without specialized tools is difficult: Kubernetes does not log blocked connections by default. Performance depends entirely on the CNI plugin.


## Best Practices for Production


Kubernetes network policy best practices boil down to a few principles:


- Start with a default-deny policy for all namespaces - this is the baseline from which you add permissions.
- Use labels consistently and thoughtfully: the accuracy of selectors depends on the quality of the labels.
- Test policies in staging before deploying them to production.
- Document the logic of each policy directly in YAML using annotations.
- Monitor traffic - tools like Hubble (for Cilium) provide a clear picture of traffic flows.


If you’re building infrastructure with Kubernetes and want to manage the entire lifecycle - from architecture design to deployment - in one place,[try Brainboard](https://www.brainboard.co/contact-us) .


## FAQ


### **Do network policies apply to all namespaces by default?**


No. Policies apply only to the namespace in which they are created and only to selected pods.


### **What happens if no network policy Kubernetes exists for a pod?**


The pod is fully open - all incoming and outgoing traffic is allowed without restrictions.


### **Can I use network policies without Calico?**


Yes, if the CNI plugin supports network policies. Cilium is a good alternative with advanced features.


### **Do network policies affect traffic between containers in the same pod?**


No. Policies operate at the pod level; traffic between containers within a pod is not affected.


### **How do I test if a network policy is working?**


Launch a temporary pod and try connecting to the target service using curl or nc.
