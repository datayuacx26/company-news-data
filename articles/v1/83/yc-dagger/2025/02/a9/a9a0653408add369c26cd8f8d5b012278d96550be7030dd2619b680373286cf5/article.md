---
schema_version: "1.0.0"
document_id: "a9a0653408add369c26cd8f8d5b012278d96550be7030dd2619b680373286cf5"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/puzzle-case-study/"
published_at: "2025-02-24T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:29e444561d7c41386d5500b8636aa98b8529fbe5450b3e41dfa4eba8871ec4ef"
---

# Streamlining CI Infrastructure at Puzzle with Dagger, KubeVirt, and ArgoCD

We’re always interested in hearing how our community is using Dagger – their use cases, their challenges, and their experiences with deploying Dagger in different environments. Our Discord is a great place to find these stories, and to benefit from the knowledge and experience of the Dagger community.


In this blog post, we’ll share the story of Daggernaut Yannik Dällenbach (aka @ioboi. on Discord). Yannik is a Platform Engineer at[Puzzle ITC](https://www.puzzle.ch/) , a consulting firm focusing on the automation of IT infrastructure using Kubernetes. By running the Dagger Engine in virtual machines (VMs, yes VMs!) on Kubernetes, Yannik and his team were able to streamline Puzzle’s CI infrastructure and make it easier to test, upgrade, and schedule jobs on it.


> “By hosting the Dagger Engine on ephemeral VMs managed by KubeVirt, we have achieved better scalability and isolation while integrating seamlessly with GitLab and ArgoCD. Dagger’s flexibility was key to improving our existing pipelines without requiring a complete overhaul.” - Yannik Dällenbach, Platform Engineer, Puzzle


#### Lack of Direct Control over CI Infrastructure


Puzzle uses GitLab CI/CD on OpenShift with Kubernetes runners for CI, and ArgoCD as the GitOps continuous delivery tool. The Dagger Engine is deployed on OpenShift as a DaemonSet, via Dagger’s official Helm chart.


OpenShift uses SELinux under the hood, enabling greater control over system access and security. However, within Puzzle, the CI/CD team does not have administrative privileges for the OpenShift cluster. Therefore, any modifications to the Dagger Engine Daemonset or the underlying nodes must be routed to a separate Operations team.


For example, the Dagger Engine expects to run as a privileged container. For this, the CI/CD team has to request additional Security Context Constraints (SCCs) for the Dagger Engine nodes in the cluster. Similarly, if the Dagger Engine cache fills up too quickly due to heavy workloads, the CI/CD team has to ask the Operations team to perform a manual disk cleanup on the affected nodes. These requests add unnecessary time and overhead to the CI/CD team’s day-to-day work.


> “A big problem for us as the CI/CD team is that we don’t have admin permissions for the OpenShift cluster. So we rely on the operations team that is responsible for our OpenShift clusters. Typically, we have to write a ticket or chat with the operations team to help us, which makes the overall management a bit harder for us.”


#### Switching to VM-Based Workloads


To make things easier, Puzzle’s CI/CD team decided to investigate other options to self-manage the Dagger Engine deployment. After investigating confidential containers (rejected as they also required SCCs) and dedicated external virtual machines (rejected as they felt like a step backwards), the team finally settled on a KubeVirt-based implementation.


[KubeVirt](https://kubevirt.io/) is a CNCF project which allows teams to run and manage virtual machines (VMs) in a Kubernetes cluster. VMs run in Pods and can be fully managed via the Kubernetes API. Under the hood, KubeVirt provides a controller which takes care of running each VM’s QEMU process and integrating with the Kernel Virtual Machine (KVM) hypervisor on each node. KubeVirt also supports the use of cloud-init for VM initialization, and Kubernetes Persistent Volumes (PVs) for data storage; the latter are exposed to the VM as disks.


Here’s how Puzzle’s new KubeVirt-based architecture works:


1. KubeVirt manages a pool of ephemeral Ubuntu-based VMs. When a new VM is initialized, a cloud-init script installs Docker and Dagger on the VM.
2. The cloud-init script also mounts two persistent volumes (PVs), to store Docker’s containers and volumes and Dagger’s work directory. This ensures that even though the VMs are ephemeral, new VMs still have access to the data of older ones, such as the Dagger cache on the local Kubernetes node (see the planned “shared caching” improvement below).
3. A health check is performed to confirm when the Dagger Engine inside the VM is ready to accept connections.
4. GitLab’s Kubernetes executor schedules incoming jobs as Pods on Kubernetes. These Pods connect to the Dagger Engine via a Kubernetes service using TCP. Network policies are in place to ensure that it is only possible to connect to Dagger Engines in the same namespace.
5. The Dagger Engine executes the Dagger Functions in the job.


#### Faster Configuration, 100% Control


With this new setup, the CI/CD team has complete control over the Dagger Engine VMs. For example:


- Via KubeVirt, the team can scale up the number of VMs when workloads increase, making more Dagger Engines available to handle the increased demand.
- The team can upgrade Dagger to the latest version simply by updating the Helm chart for the deployment, and Kubernetes will then recreate the VMs with the new version.
- Testing the CI infrastructure with new Dagger versions is much simpler, as the team can spin up a new VM in a few minutes and target runners to the new Dagger Engine.


The new setup has also benefited Puzzle’s development teams, by significantly simplifying CI configuration for their projects.


- Previously, configuring a CI workflow involved using a pre-defined template, which took care of downloading a customized Dagger container image, setting Dagger Engine variables, and so on.
- Now, workflow configuration is no longer template-dependent: developers simply include the` dagger` tag in their workflow and call Dagger Functions as required.


> “By running the Dagger Engine in ephemeral VMs, we’re able to integrate our CI infrastructure into our existing GitOps setup. We don’t need cluster admin permissions, we don’t need to deal with privileged pods, and we can manage the entire VM ourselves. Because of this, upgrading and testing is also now much faster - our velocity has increased significantly!”


To see the new CI infrastructure at Puzzle in action, check out the video below:


#### Future Improvements: VM Optimizations, Shared Caching, and Custom Operators


Going forward, Yannik and his team have a number of improvements in mind:


- They are currently stress-testing the system to identify the optimal configuration (RAM/CPU) for each VM, and they also intend to evaluate the performance of the TCP connection versus a UNIX socket connection.
- They currently run the VMs on Ubuntu, but plan to switch to a specialized Linux distribution like Flatcar Container Linux.
- They plan to implement a shared cache for Dagger, which can be used by all the Dagger Engines in the pool.
- They plan to create a Kubernetes Operator to abstract away even more of the operational tasks.


> “Dagger has allowed us to level up our current CI/CD platform. It has given us a way to encapsulate our platform engineering expertise into tools and standards that our developers can use, both locally and in CI.”


Do you have a Dagger story you’d like us to feature? Tell us all about it in[Discord](https://discord.gg/dagger-io) .
