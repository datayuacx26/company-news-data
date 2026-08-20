---
schema_version: "1.0.0"
document_id: "9b5c0b7e8f3a5bda56a9c01f629333ade43b53cba9df55e6d75d9d73206f054e"
company_key: "yc-thndr"
company: "Thndr"
source_id: "yc-thndr-news-import-00d458bb2ee0"
canonical_url: "https://thndr.app/blogpost/introducing-self-managed-kubernetes-at-thndr-a-path-to-modern-deployment/"
published_at: null
first_seen_at: "2026-07-24T04:04:20.660794+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:9919923939b5a4f2ad6b980f0e4f696d9183300f4687e72217bcd414a11c94e4"
---

# Introducing Self-Managed Kubernetes at Thndr: A path to Modern Deployment

At Thndr, 2024 has been nothing but remarkable. With over **12 billion EGP in monthly trading value** processed through our platform, Thndr has solidified its position as Egypt’s **#1 investment platform** in terms of retail value and volume. The numbers speak for themselves: an average of **1.8 million transactions per month** compared to just 91K in 2022, marking one of the most exciting growth stories in the MENA region.


While such exponential growth proves our platform’s success to handle, anyone in the software engineering industry knows the complexities of scaling a system to meet surging demand. Achieving this requires building systems that are scalable, resilient, and high-performing, which demands relentless focus across various workstreams. One of the most critical areas that contributes to this success is infrastructure — the very backbone of our platform.


This article shares our journey of transitioning from a non-Kubernetes setup to a Kubernetes-based infrastructure. We’ll walk you through the challenges we faced, the trade-offs we made, and the solutions we ultimately implemented. Whether you’re an engineering leader, a systems architect, an infrastructure engineer, a DevOps engineer, or simply curious about modern infrastructure practices, we hope you find this story both insightful and inspiring. Get an inside look at the process and discover how you can leverage our learnings for your own tech transformation!


## The On-Premise Infrastructure: Context and Challenges


While cloud providers have and keep making progress in improving compliance, security, and accessibility, financial services in many parts of the MENA region still face strict regulatory requirements. These often mandate that sensitive financial data be stored within the country’s borders, making on-premise infrastructure a necessity for organizations like Thndr.


At Thndr, a significant portion of our infrastructure remains on-premises to meet these regulatory demands. While this setup was sufficient during the early days, the rapid growth in user activity and transaction volume highlighted critical limitations. Upgrading our legacy on-prem infrastructure quickly became a top priority.


Our setup relied on a simple group of virtual machines (VMs), which introduced numerous challenges:


- **Limited development and CI/CD capabilities:** **** – Applications were deployed as single processes without industry-standard development practices or robust CI/CD pipelines.
– Engineers worked on manual and error-prone deployment processes, an approach associated with risks, such as deploying outdated packages.


- **Lack of automated testing:** **** – There were no automated test cases to validate releases before deployment, increasing the likelihood of bugs making it to production.
– Occasionally, deployments occurred during active market hours, compounding the risk of disruptions.


- **Scaling Challenges:** **** – During periods of peak demand, the only option was to manually increase machine resources, a risky and reactive approach that could put platform stability on the line.
– The lack of instance scaling made it impossible to dynamically provision additional instances to handle traffic spikes.


- **Operational Inefficiencies:** **** – Routine maintenance tasks like upgrading machines, installing updates and security patches, or clearing logs to free up disk space, had to be performed manually.


This image was generated using DALL-E


## Discovery


The previously mentioned issues highlighted the pressing need for a scalable, automated, and resilient infrastructure, laying the groundwork for the pivotal decision to transition to a Kubernetes-based solution.


## Why Kubernetes?


Kubernetes is our usual go-to choice for deploying and managing containerized applications since it supports flexible, scalable, reliable, and efficient environments. It makes nearly every aspect of management and deployment easier thanks to features like:


1. Automated rollouts and rollbacks
2. Service discovery
3. Load balancing
4. Storage orchestration
5. Self-healing


When choosing a suitable Kubernetes cluster with limitations such as unavailable cloud solutions, you must work with a group of on-premises machines to create a semi-managed or unmanaged solution. To build Kubernetes on on-premises machines, there are several options available, including k3s, kubeadm, and EKS Anywhere.


## Choosing the Right Kubernetes Solution: Balancing Constraints and Expertise


When setting up a Kubernetes cluster in your system, the chosen approach, tools, and version are determined by your constraints, areas of expertise, and existing toolset. At Thndr, we opted for k3s after extensive research and experimentation. K3s proved to be the optimal solution for Thndr. It required only a few on-premises machines with a lightweight Kubernetes version


## The Way to K3s


## **What is K3s?**


K3s is developed by the Rancher team, and it is a CNCF Sandbox Project. K3s represents a streamlined, simple-to-install, deploy, and oversee iteration of standard Kubernetes (K8s). K3s is an authorized Kubernetes distribution. While K3s is an enhanced variant of Kubernetes (the original version), its fundamental operation remains unaltered.


Reference:[How k3s works](https://k3s.io/)


## Why K3s?


1. Even though the team had limited experience with setting up and configuring k3s, we didn’t encounter significant difficulties deploying k3s in our development and production environments.


1. Rancher reduced the complexity of K3s by removing over 3 billion lines of code from the Kubernetes source. They trimmed down non-CSI storage options, experimental features, and outdated components that were not crucial for fully implementing the Kubernetes API.


1. The single binary file is less than 100MB in size, enhancing speed and reducing resource consumption compared to K8s. Unlike Kubernetes, the master, nodes, and workers do not need to run in multiple instances to boost efficiency.


1. The memory footprint is reduced primarily by running many components inside of a single process. This eliminates significant overhead that would otherwise be duplicated for each component.
2. Certified Kubernetes distribution: Well supported and maintained, expected to remain as such for the foreseeable future.


1. Easier and faster installation and deployment: K3s take seconds to minutes to install and run.


## K3s’s Architecture


1. A server node: Is defined as a host running the k3s server command, with control-plane and datastore components managed by K3s.


1. An agent node: is defined as a host running the k3s agent command without any datastore or control-plane components.


1. Both servers and agents run the kubelet, container runtime, and CNI. See the[Advanced Options](https://docs.k3s.io/advanced#running-agentless-servers-experimental) documentation for more information on running agentless servers.


1. High-Availability: For environments where uptime of the Kubernetes control plane is critical, you can run K3s in an HA configuration. An HA K3s cluster comprises:


1. Embedded DB: An embedded etcd datastore is stored inside each server node.


1. External DB: An external datastore (such as etcd, MariaDB, etc.) is placed outside the cluster in a centralized place.


## Install k3s


In that section, we will focus on the core components we have, starting from how we created the k3s and ending with how we receive the traffic.


## Ansible


We rely on[Ansible](https://www.ansible.com/) for configuration management to set up k3s, with special thanks to[Vincent RABAH](https://github.com/itwars) , who created the Ansible file structure for k3s setup. He is the author of many other valuable resources, so we encourage you to follow his work. Before proceeding, we recommend familiarizing yourself with the core components of Ansible through this[link](https://docs.ansible.com/ansible/latest/network/getting_started/basic_concepts.html) .


1. **What is Ansible?** Ansible is an open-source automation tool for configuration management, application deployment, and task automation. using simple YAML playbooks to define tasks, making infrastructure management more efficient.
2. **Setting Up k3s:** When installing k3s, you’ll have a group of machines. You can easily designate which machines act as masters and which as agents by configuring the *inventory.yml* file.
3. **Default Database Setup:** By default, K3s uses an embedded DB. If you prefer using an external database, this can be done by adding the “-datastore-endpoint” as an extra argument and setting the “use_external_database” flag to true in the *inventory.yml* file.
4. **Upgrading k3s:** To upgrade k3s at any time, simply update the “k3s_version” in the *inventory.yml* file with the desired version and run the provided playbook to upgrade all nodes in the cluster.
5. **Kubeconfig:** After successful bringup, the kubeconfig of the cluster is copied to the control node and merged with *~/.kube/config* under the “k3s-ansible” context.
6. **kubectl:** Start with installing[kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) , you can confirm access to your Kubernetes cluster with the following: “kubectl config use-context k3s-ansible” and then “kubectl get nodes”


## Supporting LoadBalancer Services in On-Prem Kubernetes Setups


On-premises Kubernetes environments lack the cloud-native support for LoadBalancer-type services found in platforms like AWS or GCP. Tools like Cilium, MetalLB, and ServiceLB fill this gap, each with a different approach.


- **ServiceLB:** Built into K3s, ServiceLB exposes services via node IPs. It’s simple but lacks true load-balanced Virtual IP (VIP) support, limiting flexibility for complex setups.
- **MetalLB:** Designed for bare-metal clusters, MetalLB uses either:
- **L2 Mode:** Broadcasts the LoadBalancer IP over the local subnet, offering simplicity but slower failover.
- **BGP Mode:** Advertises LoadBalancer IPs as routes to the network, enabling dynamic routing and scalability but requiring more configuration.
- **Cilium:** An eBPF-based tool focused on networking and security. It provides high-performance load balancing and direct kernel-level routing, though it may be overkill for simple setups.


Each tool offers unique trade-offs. ServiceLB is suited for basic needs, MetalLB balances simplicity and flexibility, and Cilium delivers advanced performance for modern networks.


## Managing Storage in On-Prem Kubernetes with CSI Solutions


Kubernetes, originally designed for stateless applications, now supports StatefulSets to manage stateful workloads with persistent volumes. On cloud platforms, storage is dynamically managed using drivers like AWS EBS, GCE Persistent Disk, or EFS CSI Driver, enabling seamless scaling and reliability. However, in on-prem environments, storage management is more complex due to the lack of native, scalable storage options.


Container Storage Interface (CSI) solutions address this gap by enabling dynamic provisioning, replication, and failover for persistent volumes in on-prem Kubernetes clusters. Key CSI tools include:


- **Longhorn:** Provides lightweight containerized storage by replicating data synchronously across nodes for high availability. It ensures resilience by automatically switching workloads to healthy replicas during node or disk failures.
- **Rook:** Integrates with distributed storage systems like Ceph, offering advanced features like object, block, and file storage. It is ideal for clusters requiring flexible storage types and scalability.
- **OpenEBS:** Focuses on per-application storage, allowing developers to create lightweight volumes with specific requirements. It’s suitable for workloads needing fine-grained control over storage.


Each CSI solution varies in complexity and capabilities, from Longhorn’s simplicity to Rook’s versatility and OpenEBS’s application-centric design. These tools make on-prem storage management more reliable, scalable, and adaptable to diverse workload needs.


Reference:[Kubernetes adoption levels](https://www.statista.com/statistics/1233945/kubernetes-adoption-level-organization/)


## Unlocking New Advantages


- **Modernizing Applications** : Modernizing code involves restructuring and improving software to enhance performance, maintainability, and efficiency, ensuring it remains scalable and adaptable to evolving needs.


- **Streamlined Continuous Integration (CI)** : A robust CI pipeline includes building container images, running test cases, tagging images with semantic versioning, performing security checks using tools like vulnerability scanners, and pushing validated images to a **Container Registry** (e.g., Docker Hub, GitLab, or JFrog).


- **Automated Continuous Deployment (CD)** : GitOps tools streamline deployments by syncing Kubernetes configurations directly from a Git repository. These tools support automation, scheduling, and tracking changes, enabling teams to deploy updates reliably and efficiently.


- **Resilient Service Mesh and Ingress Gateway** : Adopting a service mesh simplifies microservice management by enabling features like rate limiting, circuit breaking, and advanced observability through request tracing and performance monitoring. By injecting a sidecar container into each service, the service mesh provides granular insights and control over inter-service communication.


- **Unified Secrets Management** : Managing sensitive data has improved significantly with tools that integrate with Kubernetes, ensuring secure and stable handling of credentials. Solutions like **Consul** , **Azure Key Vault** , and others offer centralized management, reducing the risks of exposure.


- **Dynamic Auto Scaling** : Kubernetes supports Horizontal Pod Autoscaler (HPA) for scaling applications based on CPU and memory usage. For additional flexibility, tools like **event-driven scaling frameworks** can scale workloads dynamically based on custom metrics or schedules, ensuring readiness for traffic spikes, such as those occurring during peak business hours.


- **Comprehensive Monitoring and Observability** : Monitoring tools enable automated tracking of CPU, memory, storage, latency, and errors. Alerts can be sent to collaboration platforms like Slack or escalation tools, helping teams address issues proactively. Popular tools include: **Prometheus** , **Grafana** , and **log aggregation solutions** .


- **Proactive Incident Alerting** : Incident alerting tools ensure timely responses by escalating alerts through predefined workflows. For instance, alerts may initially notify via messaging platforms and, if unacknowledged, escalate to on-call engineers and managers, ensuring issues are addressed promptly. Tools such as **OpsGenie** and **Spike.sh** support this process.


- **Efficient Application Package Management** : Managing configurations separately from application code ensures consistency across environments (e.g., staging vs. production). Tools like **Helm** enable templating and modular configuration management, simplifying the deployment of applications across clusters with environment-specific details.


## Final Thoughts


The journey from a simple on-premise setup to a robust Kubernetes-based infrastructure has been one of the most impactful and prioritized projects at Thndr. While the transition presented its fair share of challenges, it has empowered us to scale effectively, deliver a more reliable and seamless experience for our users, and adhere to the regulations — all while positioning the platform for long-term growth.


As Thndr continues to expand in Egypt and the broader MENA region, we remain dedicated to evolving our infrastructure. Our goal is not just to meet the demands of today but to anticipate and prepare for the opportunities of tomorrow. This commitment is fueled by our mission to democratize investing, leveraging technology to provide a simple, barrier-free investing experience that is truly user-centric.


#
