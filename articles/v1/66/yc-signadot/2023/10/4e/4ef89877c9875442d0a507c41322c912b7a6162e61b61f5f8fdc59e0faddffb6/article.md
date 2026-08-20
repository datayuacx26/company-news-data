---
schema_version: "1.0.0"
document_id: "4ef89877c9875442d0a507c41322c912b7a6162e61b61f5f8fdc59e0faddffb6"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/scaling-environments-with-opentelemetry-and-service-mesh/"
published_at: "2023-10-17T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:5b9eb4ca6143fe0c9b08ea0e9367f526d8585e374d17152503e53f3793a6a076"
---

# Scaling Environments with OpenTelemetry And Service Mesh

## **Introduction**


With microservices each team is dealing with smaller pieces of the application at a time, modularizing development and operational complexity. On the flip side, however, it has created a need to validate and test that all the pieces are working well together. This need has given rise to many new classes of solutions in the past couple of years - ephemeral environments, on-demand environments, preview environments, etc. They share a common purpose: helping to ensure that functionality works as a whole, as early in the development life cycle as possible.


All these classes of microservice environments have traditionally been set up as fully separate copies of the entire set of microservices. These stacks may, in fact, share infrastructure underneath — like running in the same Kubernetes cluster in different namespaces, or run on single-node clusters, or even (at smaller scale), as Docker containers on some local or remote node. However, this very notion of running stacks of each microservice and all its dependencies separately from one another has some drawbacks:


1. **Cost scaling:** They scale in cost with the number of microservices, and often end up needing workarounds to keep costs in check, both in terms of effort to maintain and infrastructure spend. The cost implications may make developers queue up in some shared environment to accomplish their testing.
2. **Stale dependencies and divergence from production** : Each environment contains its own copy of each dependency, which is difficult to keep in sync, especially as changes are made to each microservice and pushed continuously. Additionally, another form of divergence that occurs is that third-party dependencies and integrations with cloud services may behave differently in these environments from staging or production, increasing the likelihood of a “it worked in test but not in prod” class of issues.
3. **Increased operational overhead** : Operational costs go up even if a person owns just a single microservice in the stack.
4. **Suboptimal developer experience** : It is difficult for a platform team to support each of these environments, often leading to poor developer experience and low usage. The time it takes to set up the environment also affects developer productivity. The more microservices you have, the slower these environments are to bring up.


There have been many workarounds explored to help deal with these in practice, but I want to introduce a different way of thinking about environments that has several benefits over previous approaches.


## **Rethinking Microservice Environments**


When we’re developing microservices, each developer or development team is working on changing a small part of the overall whole. Regardless of how often releases land in production, it is common for each microservice to have its own CI/CD process that sends updates to some higher environment like staging. Given this setup and the desire to test early in the development life cycle, we can think of each microservice dev/preview/test environment as being a combination of what’s changed and the “latest” versions of everything else.


As shown above, we define the latest versions of all the microservices in the stack as the baseline environment. The baseline environment serves as the default version of every microservice dependency for any environment that is set up and is being continuously updated from each CI/CD process. It’s often a single Kubernetes cluster, like staging (or even production). For each new dev/test/preview environment, we only deploy “what changed” (referred to as the sandbox above), which is often a small number of microservices in comparison with the overall number, and share any unchanged dependencies with the baseline environment.


This methodology shares some similarities with canarying in production, but in this case, there is a greater emphasis on isolating microservices sufficiently to create sandboxes that can be used during the development process. In the following section, we’ll look at how such a system of sandbox environments can be built in practice.


## **Request Tenancy**


In the previous section, we looked at the logical construct of a sandbox, which combines things under test with a common set of dependencies from the baseline environment. In practice, such a system relies on two key ideas: request tenancy and routing.


Taking the figure above, we assume that a request can be tagged with a special identifier, something that indicates which tenant is sending the request. As long as this tenancy information is passed along the chain from service to service as the call traverses through the system, we can make a routing decision using that particular tenancy to decide that a particular request should be satisfied by a “sandboxed” service \`svcA\` rather than the latest version of it from the baseline’s version of \`svcA\`. So, we need two components to make this type of flow:


1. A way to tag requests with tenancy using a special identifier as they flow through a network of microservices.
2. A way to make a localized routing decision based on the presence of the identifier specified above.


Thankfully, this notion of passing a piece of request context has become simple in modern microservices, thanks to[OpenTelemetry](https://opentelemetry.io/) . With OpenTelemetry instrumentation in microservices, this functionality is already available. A special[baggage](https://opentelemetry.io/docs/concepts/signals/baggage/) header is automatically forwarded along to the next subsequent microservice. So, as long as OpenTelemetry is used to instrument our microservices, we get this ability to tag a request automatically with no additional effort.


Now, when it comes to actually making the routing decision, the most natural solution is service meshes such as Istio, Linkerd, etc. These meshes enable the creation of rules to make exactly these types of localized routing decisions. Therefore, we end up with something like this:


One of the big wins of using such a system is that testing multiple microservices together becomes extremely simple. Often, features span multiple microservices, which makes them hard to test together till they all have landed into some common shared environment. Here, it’s possible to create a new tenant that is a combination of two other tenants by just controlling the identifier with which we’re tagging the request, which helps introduce new ways of collaboration during the microservice-building process.


## **Data Isolation**


Above we used a simple stateless microservice, where we were using an L7 protocol like HTTP or gRPC, which made request labeling and routing easy. In practice, there are databases, message queues, cloud dependencies, webhooks, etc., for which isolation using request tenancy might not be sufficient.


For example, testing schema changes to a database that a microservice uses might require setting up an ephemeral database instance or logical databases to realize the isolation necessary. In these cases where request tenancy is insufficient, you can use a higher isolation level. Typically, there are two higher levels of isolation that are typically used: logical isolation and infrastructure isolation.


Logical isolation is when you use the same underlying infrastructure (say PostgreSQL database cluster), but set up some unit of tenancy underneath, like a new database or a schema for that particular tenant. Infrastructure isolation is the catch-all, offering dedicated infrastructure for that particular tenant, such as setting up a separate PostgreSQL database cluster. In either case, you can use configuration mechanisms like environment variables / configmaps in Kubernetes to wire the ephemeral logical or physical resource with the rest of the sandbox.


The level of isolation to choose depends on the use case, but there is a clear trade-off: Higher levels increase the operational work involved in setting up and managing infrastructure, while offering lesser interference from other actors in the rest of the system. In practice, in most cases, logical isolation suffices, except where the data store itself lacks such a provision, or in certain performance / load-testing scenarios.


## **Message Queues**


For message queues, it is simplest to build tenancy information into the messages themselves (as is enabled by OpenTelemetry) and make a decision at the consuming microservice whether a particular message is relevant to itself. The key idea here is to enable the consumers to consume messages selectively so that they don’t end up processing messages intended for a different tenant.


In a system like Apache Kafka, this is done by setting up a separate consumer group per tenant, then making application-layer changes to the consumer libraries to implement this kind of logic to consume messages selectively.


## **Async Jobs and Third-Party Dependencies**


In some cases, a microservice may not be participating in request flows, but acting in a completely asynchronous manner, like a cron job that does some operation periodically, or be a point of origin of requests itself. In this case, you can still create a “sandbox” for a new version of it, but the tenancy would be specified for that particular sandboxed instance of the microservice itself. Essentially, our “tenant” in this context becomes an entire microservice, rather than a request.


This same method applies also in the cases where a third-party dependency exists that does not respect tenancy headers, or if you’re using a custom protocol where adding header metadata is not possible. The key idea is to fall back to using configuration for isolation wherever it is not possible to use request tenancy.


## **Conclusion**


The approach of creating environments using request tenancy and tunable isolation solves several drawbacks of the traditional setup of preview, test and dev environments in Kubernetes. Specifically, since we’re deploying as few microservices as needed for each environment, this is highly cost-effective, even at scale, as evidenced by companies that run several hundreds such systems internally like[Uber’s SLATE](https://www.uber.com/blog/simplifying-developer-testing-through-slate/) ,[Lyft’s Staging Overrides](https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-3-extending-our-envoy-mesh-with-staging-fdaafafca82f) and[Doordash](https://doordash.engineering/2022/06/23/fast-feedback-loop-for-kubernetes-product-development-in-a-production-environment/) .


It also ensures high-fidelity testing against the latest dependencies and is quick to set up, bringing wins in terms of developer experience and productivity. There are new ways possible with this approach to collaborate more seamlessly across developers and development teams working on different microservices.


We at Signadot are building a Kubernetes-native solution that makes it easy to create these types of environments and use them for previews, dev, and test environments in Kubernetes. We’re excited to help make this possible and reduce the complexity involved in operationalizing the above. You can read more about Signadot’s approach in our[documentation](https://www.signadot.com/docs/overview) or come talk to us on our[community slack](https://join.slack.com/t/signadotcommunity/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ) channel!
