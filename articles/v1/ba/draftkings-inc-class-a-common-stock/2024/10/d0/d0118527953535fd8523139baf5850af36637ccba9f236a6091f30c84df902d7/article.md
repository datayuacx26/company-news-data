---
schema_version: "1.0.0"
document_id: "d0118527953535fd8523139baf5850af36637ccba9f236a6091f30c84df902d7"
company_key: "draftkings-inc-class-a-common-stock"
company: "DraftKings Inc."
source_id: "draftkings-inc-class-a-common-stock-rss-016c40719db2"
canonical_url: "https://medium.com/draftkings-engineering/migrate-hundreds-of-microservices-to-the-cloud-with-zero-downtime-part-3-075197dea5fb"
published_at: "2024-10-02T06:20:42+00:00"
first_seen_at: "2026-07-20T04:35:13.112015+00:00"
fetched_at: "2026-07-28T20:59:23.866660+00:00"
content_hash: "sha256:b20a6ac7b925dfea8b8c5613eccdb3ce6bf2018a72ce01ab4def682abb9621c9"
---

# Migrate hundreds of microservices to the cloud with zero downtime — Part 3

# Migrate hundreds of microservices to the cloud with zero downtime — Part 3


[Valerii Golovko](https://medium.com/@nevalikne?source=post_page---byline--075197dea5fb---------------------------------------)


5 min read


·


Oct 2, 2024


--


Press enter or click to view image in full size


## HTTP service migration


The migration of services that provide HTTP API is divided into two categories: Internet-facing services and those for internal communication only.


Migration of Internet-facing services depends on the technology stack which is in use for (but not limited by the list below):


- CDN
- Gateway
- Authorization/Authentication
- Service Discovery
- Load Balancing


Every item from the list above should be considered, and a proper solution has to be chosen based on the options provided by the particular toolset.


Some limitations of certain tools are the reason to switch to another technology in order to achieve smooth migration process. For example, if the solution that is in use for the **Gateway** layer is not supported by the target Cloud provider.


Because this migration process is highly specific to certain technologies, it falls outside the scope of this article.


The focus below is dedicated to internal HTTP communication only which includes **Service Discovery** and **Load Balancing** layers.


It’s still specific to particular technologies, however it highlights the way of thinking how to solve such type of tasks.


## Service Discovery and Load Balancing


A system with hundreds of components must provide tools for enhanced observability and discovery, to ease connectivity between them.


In the describing system Fabio and Consul tandem is used for service discovery and load balancing, ensuring efficient distribution and routing of network traffic among the services.


More details about this toolset can be found[here](https://www.consul.io/) and[there](https://fabiolb.net/) .


In the diagram below, you can find a high-level representation of this communication.
In order for **Service B** to be able to call **Service A** following steps happen:


- Service A on startup registers instances in the Consul Service Catalog.
- Fabio is an enhanced Reverse Proxy, that supports listening to Consul Service Catalog updates in real-time.
- It registers itself in Consul with a route like **fabio** .consul.local: **9999 -** Service provides special tags during registration with **prefix** that should be used by Fabio to route traffic to it.
- For example, if prefix is **service-a,** in order to call **Service A** URL should look like: **fabio** .consul.local:9999/ **service-a** /…
- Hence, **Service B** should know the Fabio route and **Service A** prefix in order to start calling it.
- Fabio supports various load-balancing strategies. It automatically re-routes traffic accordingly based on the current health status of service instances.


Press enter or click to view image in full size


## Service migration process


Having described the toolset as a base, let’s take a look over the migration process of a Service providing HTTP API.


The **“Right to Left”** and “ **APIs go last”** approaches described earlier help to support the seamless migration of an HTTP Service.


Therefore, all HTTP consumers should be migrated first. However, during the transition phase, some consumers have already moved to the Cloud, while others are still present On-Prem.


The diagram below presents the initial state of migrating Service A:


Press enter or click to view image in full size


The “Fabio Redirect” approach helps to support such a setup. Fabio Cloud instance registers a manually created route of Service A.


This manual route forwards traffic from Cloud HTTP Consumers to the On-Prem Fabio instance.


Routing table of the Cloud Fabio instance is presented below in such setup:


Press enter or click to view image in full size


### Testing phase


Using the “Fabio Redirect” approach described above gives one wonderful Fabio behavior out of the box, which is quite useful when the time comes to migrate Service A **** to the Cloud.


Once Service A is deployed to the Cloud, it has to be verified before shutting down the On-Prem instance and re-routing all traffic into it.


After deployment of Service A to the Cloud the system will look like on diagram below:


Press enter or click to view image in full size


Having manually created Fabio routes, once Service A **** deployed to the Cloud, it will register its routes in Fabio automatically. However, their weight will be **0%** .


Therefore routing table will look like (assuming we have 2 replicas of the Service A):


Press enter or click to view image in full size


That is exactly what is needed to verify the Cloud version of Service A without Production impact.


### Rollout phase


Once verification is done, Rollout steps are straightforward:


- Remove manually created custom Fabio route
- All the traffic will be rerouted to the Cloud Service automatically
- Start service verification process
- Stop On-Prem Service.
- This step is the last one for a reason. It is described in the Rollback plan below.


Press enter or click to view image in full size


The final routing table will look like:


Press enter or click to view image in full size


### Rollback plan


If there is an issue found during the Service verification phase, mitigation consists of just one step:


- Register back custom Cloud Fabio route pointed to On-Prem address of Service A


That’s why it is better to leave On-Prem Service alive until make sure that the Cloud version is fully functional and keeps up well with production traffic.


Good to mention that such a rollout strategy results in **0** downtime.


The described approach is coupled to a particular toolset; however, with some adjustments, it is possible to adapt it to many other Service Discovery & Load Balancing solutions.


## Conclusion


The scope of the article covers migration strategies of mature distributed systems from one place to another while keeping it running.


The provided examples do not include the full set of challenges that could be encountered during such migration; however, they can give a sense and could be a starting point for engineers who have similar initiative in front of them.


Several key lessons have emerged from the experience at DraftKings migration initiative:


1. **Planning:** The migration initiative should include a proper planning phase with significant time invested to avoid major bumps in the road.
2. **Communication channels overview:** It’s important to revise and prepare solutions for all communications channels in the system upfront for the transition phase of the migration when parts of the system live in both places.
3. **Team Communication:** Proper communication strategies between teams become vital in case of a system with hundreds of services and numerous departments. Regular synchronization, timeline planning and revisions of them have proven to be beneficial.
