---
schema_version: "1.0.0"
document_id: "6686e576a96df98b83f976ee9ba48027ca3a6669896bc0589c57f3b3d310e142"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/api-gateway-vs-load-balancer"
published_at: "2023-02-22T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:62305f4737c549133a6e8a929af8258b2ad282dec23919aa87b149e6bd23ccdc"
---

# API Gateway vs Load Balancer

## Introduction


With a massive growth in cloud providers and microservices, new patterns and technologies have emerged to serve various architectures. For example, a load balancer has been around for decades and provides a mechanism to scale workloads to a cluster of servers. On the other hand, an API Gateway provides a means to route workloads to a catalog of services that serve a single application.


Let's gain a deeper understanding of Load Balancers and API Gateways. Equipped with this knowledge, you should be able to make better architectural decisions that provide a robust product to your users while reducing infrastructure costs and overhead.


## Load Balancers


Let's say your service is straining to service requests. You identify that your service needs more resources to handle the load. You could increase your resources on that server/container or add another container/server. If you opt for the latter, you will likely reach for a load balancer. A load balancer distributes incoming requests to many containers or servers. Said another way, a load balancer enables you to scale horizontally.


On the surface, load balancers solve a simple problem: "I want more than one container to serve my site." However, load balancers require the infrastructure and application configurations to work harmoniously. Let's look at some scenarios where load balancers ultimately improve the robustness of your application. I will assume containers to mean any server, container, or resource behind the load balancer.


1. **Resiliency:** If one of your containers freezes or dies, traffic should stop routing to that container. This feature provides resiliency to your product by removing unresponsive containers from interacting with end users.


2. **Security:** Load Balancers must know which port to reach your app. By using a load balancer, your service is implicitly private and does not expose all its ports to the internet — a source of attacks.


3. **SSL Termination:** Load Balancers provide SSL Termination — a technique where HTTPS communication stops at the load balancer and continues as HTTP traffic to the app. Your app doesn't need to manage/configure/renew certificates as part of its delivery process.


## API Gateways


With the advent of microservices, API Gateways have become ubiquitous. Rather than serving n number of containers for one app, an API Gateway exists to serve one container for n apps. Typically, this is done by configuring an API Gateway to route URL paths of a single domain to different services.


Just as with the load balancer, an API Gateway sounds simple. However, API Gateways contain many features to solve second-order problems that emerge from a microservice architecture. For example, imagine an app composed of 5 services.


1. **Authentication:** Do all five services need to identify a user? Do we have to redeploy every service when our authentication code changes?


2. **Authorization:** Once we identify the user, how do all five services codify what actions the user can do? It becomes complex and onerous if every service has to perform authorization.


3. **Rate-Limiting/Throttling:** Many APIs provide rate-limiting or throttling to prevent abuse of an app. All five services need to centralize their requests to enforce these policies. An API Gateway serves as this centralized function that filters traffic before it reaches each service.


4. **Routing:** When a request comes in, which of the five services should handle the request?


## Simple, right? Right?


At first glance, there is little overlap between load balancers and API gateways. API Gateways handle various services, while load balancers handle the volume of services. Unfortunately, we can't claim victory on this mental model.


If we took a composable mindset, we could place load balancers behind API Gateway, but now we have more significant problems. We have an extra hop in our request trail which adds latency to each request. If something goes wrong, we have many load balancers (one for each service) to diagnose with their logs and health metrics. (I know, centralized logging helps, but it's still additional complexity) Most importantly, each ingress device has an enormous amount of configuration.


## **Ingress**


As an infra engineer, you might enjoy mastering a massive and complex system—but the zen of architectural and operational simplicity can be all the more rewarding. So how do we simplify our systems? We add routing features to load balancers and load-balancing features to API gateways, right? (Totally not confusing). Enter \[Ingress\](https://kubernetes.io/docs/concepts/services-networking/ingress/), a term used by Kubernetes to represent a resource that routes external HTTP(S) traffic to services within its cluster. Each cloud provider has their own managed load balancer to add to an extensive list of open-source tools, including \[nginx\](https://nginx.org/), \[Kong\](https://konghq.com/), \[Istio\](https://istio.io/), \[envoy\](https://www.envoyproxy.io/), \[HAProxy\](https://www.haproxy.org/), and many more. If you're in a scenario where you need to scale volume and route to various services, use a single Ingress device instead of chaining an API Gateway to a Load Balancer.


## Conclusion


Now, let's talk about CDNs. Just kidding, we'll save that for another post. Hopefully, with a firm understanding of ingress devices, you can make better choices when architecting a new app, migrating a legacy app, or fiddling with a side project. While choosing the proper Ingress for your app may feel daunting, don't overthink or overengineer. Instead, choose one that is easy to set up and diagnose and provides the features you need today.


‍
