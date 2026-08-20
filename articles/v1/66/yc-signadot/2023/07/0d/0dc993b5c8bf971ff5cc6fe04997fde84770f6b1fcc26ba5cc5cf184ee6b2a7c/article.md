---
schema_version: "1.0.0"
document_id: "0dc993b5c8bf971ff5cc6fe04997fde84770f6b1fcc26ba5cc5cf184ee6b2a7c"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/leveraging-service-mesh-for-dynamic-traffic-routing-in-shared-kubernetes-environments/"
published_at: "2023-07-09T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:db4b4933312d0bafb348e6d25137d46833d5d9cd248307562d427cf71cb7a05e"
---

# Leveraging Service Mesh for Dynamic Traffic Routing in Shared Kubernetes Environments

Adopting a microservices architecture comes with the promise of scalability, flexibility, and agility. However, it also brings its own set of challenges, particularly when it comes to managing and testing these services. One of the key challenges is how to provide isolated, high-fidelity environments where developers can test their changes in production-like conditions without stepping on each other’s toes.


Kubernetes is the standard for container orchestration, but traditional approaches to creating sample environments like namespace or cluster-based dev environments can lead to infrastructure overhead, longer setup times, and difficulties in managing data and dependencies.


In this post, we will explore how organizations can leverage a service mesh, like Istio or Linkerd, to dynamically route traffic based on request headers, enabling developers to safely share staging Kubernetes environments. The key lies in the concept of “tenantID” that ties a user to the workloads that need to be tested in isolation.


## What is a Service Mesh?


A service mesh is a dedicated infrastructure layer for handling service-to-service communication in a microservices setup. It’s responsible for the reliable delivery of requests through the complex topology of services that comprise a modern, cloud-native application.


Service meshes like Istio and Linkerd provide features such as load balancing, service discovery, traffic management, and detailed monitoring. One of the most compelling features they offer is dynamic traffic routing based on various parameters, including request headers.


## The Power of Request-Level Isolation


Traditional isolation methods work at the infrastructure level, spinning up separate environments for each developer or team. This approach, while effective at a smaller scale, does not scale well as the number of microservices and developers increase.


On the other hand, request-level isolation provides an ingenious solution to this problem. Instead of isolating at the infrastructure level, we isolate at the request level, utilizing the dynamic request routing capability of service meshes like[Istio](https://istio.io/latest/docs/tasks/traffic-management/request-routing/) or[Linkerd](https://linkerd.io/2.13/tasks/configuring-dynamic-request-routing/) .


A unique tenantID tied to each developer or a testing client is embedded in the request headers. This tenantID is then used by the service mesh to route the request to the right version of the microservice, i.e., the one tied to that specific tenantID.


## Implementing Request-Level Isolation using a Service Mesh


To implement request-level isolation, we first need to ensure that each change made by a developer is deployed in its own isolated environment, represented by the tenantID. This can be a specific version of a microservice, or a lightweight ephemeral sandbox containing only the modified services.


Next, we annotate all requests from this tenant to have a request header with the **tenantID** as the value. When using a Web frontend we do this via a browser extension that adds this header on all outgoing requests. Using mobile apps, we update the dev version of the app to include this header on outgoing requests. For command line tools like **curl** or **postman** , we add that the header as a command line argument.


Note that in addition to adding the header to the requests, we need to ensure that these headers are propagated across the inter-service calls. This is typically done using OpenTelemetry libraries that take care of this via context propagation.


Finally, we configure the service mesh to inspect incoming requests and route them based on the tenantID in the headers. For instance, with Istio, we can define a **VirtualService** with match conditions on HTTP headers to route traffic to specific versions of services. Here is an example configuration:


In this example, requests carrying the header **tenantID: dev1** are routed to the **version-dev1** of **myservice** , while other requests are routed to the **main** version.


## Mapping TenantIDs to Developer Sandboxes


Maintaining the association between tenantIDs and the set of “under-test” workloads is critical to managing a multi-tenant Kubernetes environment effectively. While there isn’t a built-in way to handle this directly in Kubernetes, you can leverage a combination of Kubernetes resources and some additional tooling to achieve this goal. Here’s a simple approach:


### Custom Resource Definitions (CRDs)


One way to manage this mapping could be through the use of Custom Resource Definitions (CRDs) in Kubernetes. CRDs allow you to create your own Kubernetes-like resources with custom schemas.


In this scenario, you could create a CRD that represents a “DevelopmentSandbox”, with each instance of this CRD representing a sandbox tied to a specific tenantID. The schema for this CRD could include fields like **tenantID** , **workloads** , and **status** .


When a developer is ready to test changes, they could create a new instance of the DevelopmentSandbox resource, specifying their tenantID and the workloads they’ve modified in the resource’s spec. Here’s an example manifest for such a resource:


You can then use Kubernetes controllers (like operators built with the Operator SDK) to watch for changes to these custom resources and automatically manage the associated resources (deployments, services, etc.) accordingly.


### Using Labels and Annotations


Another approach could be to use Kubernetes’ built-in metadata features: labels and annotations. Labels are key-value pairs that can be attached to Kubernetes objects and are commonly used for identifying and grouping related resources. Annotations are similar, but they’re designed to hold non-identifying metadata, which can be used to store arbitrary data.


In the context of associating tenantIDs with workloads, you could use labels to tag each resource (like a Deployment or a Pod) that belongs to a specific tenantID. Here’s an example:


You can then use Kubernetes’ label selectors when you need to query for all resources associated with a specific tenantID. For instance, you might use a command like **kubectl get pods -l tenantID=dev1** to get all pods associated with the **dev1** tenant.


Remember that using labels in this way requires careful management, especially as the number of tenants and resources grows. You’ll want to establish a consistent labeling strategy and possibly create automation to enforce it.


In conclusion, while Kubernetes does not directly provide a built-in mechanism to handle multi-tenancy at the level of individual developers, its extensibility and flexibility offer several ways to effectively manage the association between tenantIDs and their respective workloads.


## Benefits and Implications


Using service mesh for request-level isolation in Kubernetes offers several benefits:


**Speed and Efficiency** : Developers can test their changes in high-fidelity environments within seconds, without waiting for a complete environment to spin up. The lightweight sandboxes, which contain only the services that have been modified, are spun up swiftly, resulting in faster feedback loops.


**Scalability** : This approach scales beautifully with the number of developers and services. As isolation happens at the request level, hundreds of sandboxes can coexist within the same physical cluster without interfering with each other.


**Resource Optimization** : Because each sandbox only contains the modified services and not the entire microservices setup, the resource utilization is optimized. This leads to significant cost savings as compared to maintaining separate namespaces or clusters for each developer.


**High Fidelity Testing** : Since the sandboxes are within a shared staging environment, they have access to realistic data and real dependencies, just like in production. This leads to more accurate and reliable testing results.


Using a service mesh for request-level isolation provides a powerful and scalable approach to manage developer environments in a Kubernetes context. The innovative use of tenantID tied to individual users and workloads makes it possible for developers to test their changes in isolation, within shared staging environments.


## How Signadot can help


This piece describes how a service mesh can be surprisingly useful for complex routing tasks within your cluster. If you’re interested in using a service mesh for this but *don’t* want to do the technical implementation yourself, Signadot is a platform that provides a turn-key solution to using request-based isolated development environments that scale to 100s of developers and microservices cost effectively. Teams at[DoorDash](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/) and[ShareChat](https://www.signadot.com/blog/how-sharechat-tests-every-release-on-kubernetes/) use Signadot to route requests intelligently and improve the developer experience.


‍
