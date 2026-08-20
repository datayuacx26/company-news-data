---
schema_version: "1.0.0"
document_id: "d9d51eb3f0b2b8a183145d7edb2ba803cdd8075d01e332bc1841f8e0aaec7c58"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/introducing-managed-gateway-api"
published_at: "2025-09-09T21:28:56.259+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:513a7df2ad452c3cbcd9d264fa33a14aedf2ff00ed312058897d8a9f53f72882"
---

# Announcing Gateway API Support for DigitalOcean Kubernetes

Managing traffic into a DigitalOcean Kubernetes cluster has long been the domain of the Ingress. While functional, it comes with limitations in flexibility, role separation, and advanced routing. Today, we’re excited to change that.


We are thrilled to announce that the **Kubernetes Gateway API, as a managed service, is pre-installed in all DigitalOcean Kubernetes (DOKS) clusters and ready to use at no additional cost.**


This next-generation traffic management solution is more expressive, extensible, and powerful than Ingress. Best of all, it’s powered by Cilium’s high-performance eBPF implementation, offering superior performance and advanced routing capabilities without the overhead of traditional proxy-based solutions.


## Key Benefits


-


**Zero Configuration Required** : Gateway API support comes pre-installed via Cilium in all DOKS clusters


-


**Advanced Traffic Management** : Support for header-based routing, traffic splitting, and canary deployments


-


**Superior Performance** : Cilium’s eBPF implementation operates in kernel space, eliminating proxy overhead


-


**Native Load Balancer Integration** : Seamless integration with DigitalOcean Network Load Balancers


-


**Multi-tenant Ready** : Built-in support for cross-namespace resource sharing with secure RBAC


-


**Future-Proof API** : Active development and standardization by the Kubernetes community


### Why Move Beyond Ingress? The Gateway API Advantage


The Gateway API was designed by the Kubernetes community to address the fundamental limitations of the Ingress API. It achieves this through a role-oriented resource model that separates infrastructure concerns from application routing.


-


**Cluster Operators** manage Gateway resources, defining where and how traffic enters the cluster (e.g., provisioning a DigitalOcean Load Balancer).


-


**Application Developers** manage Route resources (like HTTPRoute), defining how traffic is routed to their specific applications.


This separation provides clearer responsibilities and prevents teams from stepping on each other’s toes.


Aspect Ingress Gateway API


Resource Model Single monolithic resource Separated Gateway + Route resources


Team Workflows All teams edit the same resource Clear infrastructure/app separation


Routing Features Basic path/host matching Headers, methods, weights, redirects


Routing Features HTTP/HTTPS only HTTP, HTTPS, TCP, UDP, gRPC


Extensibility Annotation-based Native API fields + custom policies


### The DigitalOcean Difference: Performance Powered by Cilium and eBPF


Our Gateway API implementation isn’t just about better structure; it’s about raw performance. By leveraging Cilium, we process traffic directly in the Linux kernel using **eBPF** , completely bypassing the overhead of traditional proxy-based implementations like NGINX or HAProxy.


What does this mean for your applications?


-


**Lower Latency:** No user-space proxy traversal


-


**Higher Throughput:** Zero-copy packet processing


-


**Less CPU Usage:** Kernel-native operations


-


**Minimal memory footprint** : No proxy pods or sidecars


You get advanced routing capabilities without sacrificing performance or running extra proxy pods.


## Pricing


Gateway API support is included at no additional cost with DOKS. You only pay for:


-


DigitalOcean Load Balancers created by Gateway resources


-


Standard DOKS cluster pricing


## Try the Gateway API on DOKS


We’re here to help you. Please take a look at our in-depth tutorial on getting started with[Managed Gateway API for Kubernetes](https://www.digitalocean.com/community/tutorials/kubernetes-gateway-api-tutorial-cilium-ingress-alternative) . Additionally, take a look at our[product documentation](https://docs.digitalocean.com/products/kubernetes/) and[Release Notes.](https://docs.digitalocean.com/products/kubernetes/#9-september-2025%5Bwww.digitalocean.com%5D(%5Bwww.digitalocean.com%5D(https://www.digitalocean.com))) for further resources.
