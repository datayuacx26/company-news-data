---
schema_version: "1.0.0"
document_id: "6d33703c6e6a311ded82ec61b36a749416070ce7be30c70c2034675950985305"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/istios-graduation-with-the-cloud-native-computing-foundation/"
published_at: "2023-08-14T06:56:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:e2061c528a2baf72e8da05c20ed028df2ca8128eb71a708480bcdb31bb87a875"
---

# Istio’s Graduation with the Cloud Native Computing Foundation

It’s a big day for users of the de-facto standard for Service Mesh on Kubernetes as the Cloud Native Computing Foundation (CNCF) has announced the graduation of Istio.


With the graduation of Istio, it has now officially become a part of the elite group of projects that are considered the building blocks of cloud-native computing. This recognition further cements Istio’s importance within the cloud-native ecosystem and validates its impact on the world of microservices and service mesh technology. As Istio continues to be adopted by more Kubernetes Native Platforms, we can expect to see even more powerful tools and integrations being developed to help organizations manage and secure their microservices.


“Today, the Istio project takes its place alongside the projects that enable it and upon which it is built, including Kubernetes, Envoy, Prometheus, and SPIFFE,” said Craig Box, Istio Steering Committee member and[VP of Open Source and Community at ARMO](https://www.cncf.io/announcements/2023/07/12/cloud-native-computing-foundation-reaffirms-istio-maturity-with-project-graduation/) . “On behalf of the project’s leadership, we wish to thank every contributor, both corporate and individual, who have collectively brought us to graduation within the CNCF.”


## What is Istio?


Istio is a popular open-source service mesh that offers a way to manage and secure microservices. Initially developed by Google, IBM, and Lyft, it has since become a part of the CNCF.


At its core, Istio operates by deploying a sidecar proxy alongside each service instance. This proxy intercepts all incoming and outgoing traffic for the service and provides several capabilities, including traffic routing, load balancing, service discovery, security, and observability.


When a service sends a request to another service, the sidecar proxy intercepts the request and applies a set of policies and rules defined in Istio’s configuration. These policies and rules dictate how traffic should be routed, how load should be balanced, and how security should be enforced.


Istio provides an easy way to create a network of deployed services with load balancing, service-to-service authentication, monitoring, and more, without requiring any changes in service code. The sidecar proxy intercepts all network communication between microservices, which can be configured and managed using Istio’s control plane functionality. This year Istio released Ambient Mesh, which allows the power of Istio without the sidecar requirement; more on this below.


## What is the CNCF?


The Cloud Native Computing Foundation (CNCF) is a non-profit organization that aims to advance cloud-native computing and improve the scalability and reliability of containerized applications. The CNCF hosts a number of open-source projects, including Kubernetes, Prometheus, and Istio.


Istio’s graduation with the CNCF is a significant milestone for the project, demonstrating its maturity and impact on the cloud-native ecosystem. This graduation means that Istio has met the CNCF’s rigorous standards for community growth, code development, and adoption, and will continue to receive support and resources from the foundation.


## What This Means for Microservices


As Microservice architecture has evolved, the operational abilities of complex clusters has expanded, but the management and control systems haven’t always kept pace.


For those who can’t use sidecar deployment or are concerned about overhead or management issues, Istio is showing promise with Ambient Mesh, the sidecar-less version. The Istio team managed to[bring ambient mesh to Alpha in Istio 1.18](https://istio.io/latest/news/releases/1.18.x/announcing-1.18/#ambient-mesh) and are continuing to drive it to production readiness. Sidecar deployments remain the recommended method of using Istio, and the[1.19 release](https://github.com/istio/istio/wiki/Istio-Release-1.19) will support a[new sidecar container feature](https://github.com/kubernetes/kubernetes/pull/116429) in Alpha in Kubernetes 1.28.


As microservice architecture continues to evolve, service mesh technology like Istio becomes increasingly important for managing and securing complex clusters. Istio’s graduation with the CNCF validates its impact on the cloud-native ecosystem and demonstrates its maturity as a project. This graduation also means that Istio will continue to receive support and resources from the foundation, which will likely lead to even more powerful tools and integrations being developed for managing microservices.


## Istio and Cilium


Istio’s graduation with the CNCF marks an important milestone for microservices technology. Istio is also useful in tandem with another CNCF project,[Cilium](http://cilium.io/) . Cilium implements critical security and monitoring tools for your cluster, and is quickly becoming the default Container Network Interface (CNI) for Kubernetes. In combination with Istio, it’s possible to implement truly deep control and security. Cilium provides powerful networking and security policies at l3/l4, Istio provides zero trust for applications with defense in depth, traffic control and resiliency.


Cilium just released[version 1.14](https://isovalent.com/blog/post/cilium-release-114/) with mutual authentication, and promises to continue to grow for the project’s users. Notably Cilium cannot operate with Istio ambient mesh.


## What This Means for Kubernetes Native Platforms


Kubernetes Native Platforms such as Red Hat OpenShift, Google Kubernetes Engine (GKE) and Amazon Elastic Kubernetes Service (EKS) have all adopted Istio as their default service mesh.


Istio’s graduation with the CNCF is a further validation of its importance within the cloud-native ecosystem, and a sign of the growing adoption of service mesh technology. As service mesh adoption continues to grow, we can expect to see even more powerful tools and integrations being developed to help organizations manage and secure their microservices.


## Does Istio provide Observability signals?


Istio furnishes a comprehensive telemetry suite for every communication occurring within your service mesh. This telemetry data enhances the visibility into service operations, allowing operators to debug, maintain, and enhance their applications, all without adding any extra workload for service developers. By employing Istio, you can deeply comprehend how services within the monitored cluster interact - be it with other services or with Istio’s own components.


Here’s a snapshot of the telemetry categories Istio produces to elevate the observability of your service mesh:


**Metrics:** Istio crafts a collection of metrics resonating with the fundamental “golden signals” of monitoring, which include latency, traffic, errors, and saturation. Additionally, Istio offers intricate metrics pertaining to the mesh’s control plane. To supplement these metrics, a standard array of mesh monitoring dashboards is also at your disposal.


**Distributed Traces:** For every service, Istio creates distributed trace spans. This equips operators with an in-depth perspective on call pathways and the interdependencies of services within the mesh. Trace information from you service mesh is critical for understanding a distributed system.


**Access Logs:** When traffic is directed towards a service within the mesh, Istio is adept at crafting a comprehensive log for each interaction. This encompasses data on both the origin and the destination. Such granularity in information grants operators the capability to scrutinize service operations right down to the individual workload instance.


This telemetry information is much more focused on operations than business logic, but can be part of an overall Observability system for understanding how users are interacting with your applications.


## What else has graduated from the CNCF?


This year a number of projects have graduated with the CNCF, indicating both their technical maturity and the level of community engagement. Long associated strongly with Kubernetes, the CNCF feels a bit like it’s branching into every part of the Operations landscape. Some recent graduates:


### CRI-O


CRI-O is a lightweight container runtime specifically tailored for Kubernetes. The name “CRI-O” comes from the fact that it is an implementation of the Kubernetes Container Runtime Interface (CRI). The CRI is a plugin interface that allows for different container runtimes to integrate with Kubernetes, and CRI-O is one such implementation. CRI-O is designed to provide a seamless experience for running containers using Kubernetes without necessarily relying on Docker as the container runtime. CRI-O is designed to be compatible with the Open Container Initiative (OCI) standards. This means that it can run any container that adheres to the OCI container image and runtime specifications.


### Flux


On the operation and deployment side, Flux provides a toolset for automating the deployment, scaling, and management of containerized applications. It’s a continuous delivery solution that focuses on the concept of GitOps. Flux constantly monitors the configured Git repository for changes. When it detects a change, such as a new Docker image or updated configuration, it automatically deploys those changes to the cluster.


## Get Started for Free with Signadot


The reason why the Signadot team is such a big fan of Istio is because of the way it enables request isolation for running experimental sandboxes within a shared cluster. Request routing within Kubernetes is implemented by the Signadot Operator, either via integrations with an existing service mesh such as Istio, or by using a system of built-in sidecars.


For more information, check out our blog post on[transforming Kubernetes developer environments](https://www.signadot.com/blog/transforming-kubernetes-developer-environments-the-shift-to-request-level-isolation/) and our documentation on[request routing](https://www.signadot.com/docs/concepts/sandbox#request-routing) .


Signadot is a Kubernetes native platform that allow developers building microservices to safely share a high fidelity environment for rapidly validating code changes. You spin up Sandboxes containing “under-test” versions of services running locally or in Kubernetes and test end-to-end flows using the shared environment to satisfy dependencies. Sharing the remote Kubernetes environment works by isolating requests tagged with unique routing keys associated with Sandboxes.


‍
