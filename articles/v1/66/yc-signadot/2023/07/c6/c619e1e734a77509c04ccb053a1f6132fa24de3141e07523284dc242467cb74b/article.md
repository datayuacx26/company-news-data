---
schema_version: "1.0.0"
document_id: "c619e1e734a77509c04ccb053a1f6132fa24de3141e07523284dc242467cb74b"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-lyft-and-razorpay-share-development-environments-with-hundreds-of-devs/"
published_at: "2023-07-24T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:27a670d0d14838a6a672d0fbc2f56ee8a9222abad065b7af0d0af2063f1d8bc2"
---

# How Lyft and Razorpay share development environments with hundreds of developers

## How Large Teams are Testing on Kubernetes


1. [Why large engineering teams are testing on Kubernetes](https://www.signadot.com/blog/large-engineering-teams-testing-on-k8s/)
2. [Exploring Kubernetes Testing Evolution: Deep-Dive Case Studies from Eventbrite and Prezi](https://www.signadot.com/blog/prezi-eventbrite-teams-testing-on-kubernetes/)
3. [How Uber & DoorDash empower developers to preview every code change in production](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/)
4. How Lyft and Razorpay share development environments with hundreds of developers


Previous articles in this series have covered the way that large organizations let developers test and experiment with new code. This piece talks about how very large developer organizations at Lyft and Razorpay handle a massive scale of deployments, and make testing more than a pale imitation of production. Using request routing, both teams have allowed developers to run code in a shared environment that is consistently kept up to date, and allows components like datastores and third party API’s to get requests without the need for excessive mocking.


## A brief review of the goals


The general design goals of the development tooling are nearly universal:


- Speed - Developers should be able to go from writing code to seeing it work quickly
- Accuracy - Changes should run in an environment that is as close to production as possible. The term we’ve coined for this is[high-fidelity testing](https://www.signadot.com/blog/the-staging-bottleneck-why-your-engineering-team-is-slow-and-how-to-fix-it/) .
- Isolation - Changes implemented by a single developer for the purpose of testing and experimentation shouldn’t affect others’ experience in the shared environment


A final consideration is cost: both in resource costs and the cost of labor to set up and maintain a development and test workflow. While it’s great to try and keep costs minimal, large teams are going to incur costs to run a high fidelity environment that closely replicates production. These goals aren’t mutually exclusive, and both Lyft and Razorpay managed to come up with models that met all three of these goals.


## Lyft


Lyft is committed to running staging like a production service. Staging at Lyft is a first-class environment where breached SLOs will page on-call engineers and developers will raise a SEV if the environment becomes unstable. From their[blog post on the topic:](https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-3-extending-our-envoy-mesh-with-staging-fdaafafca82f)


> *Our staging environment runs the same tech stack as production but with scaled-in resources, fake user data, and a synthetic web traffic generator.*


This commitment to keeping up an accurate staging environment satisfies the accuracy requirement mentioned above, but this system will struggle if a single developer’s commits can break this staging environment. With on-call engineers getting paged every time a new piece of code causes problems.


### Early Validation of Tactics


The team working on the developer experience started with[Onebox](https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-1-a2f5d9a77813#63ab) , a tool to allow single services to be replaced only for the developer working on them The benefit was clear:


*From the Lyft Article: using Onebox to isolate developer experiments*


With a tool to let developers ‘swap out’ a single service, there was no need for heavy process to try out changes on Staging. While Onebox had the right idea, there were scaling problems that caused the Lyft team to move away from the tool. The general concept was correct however, the Lyft team called these ‘Staging Overrides:’ a change that didn’t have to be vetted with the release process.


### The secret ingredient: Request Routing


While[previous pieces](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/) have discussed the idea that developers needed to experiment with high-fidelity environments, the implementation starting with Onebox suggests a way to do this at scale: rather than replicating services into[namespaces](https://www.signadot.com/blog/namespace-based-environments-for-testing-pros-and-cons/) , or managing changes with[feature flags](https://www.signadot.com/blog/pitfalls-of-using-feature-flagging-for-testing-in-shared-staging-environments/) , the technique in use at Lyft is to use request isolation. This has the great advantage of letting only the requests kicked off by a single developer hit the service under experiment.


*From the Lyft Article: How request isolation routes requests for the single developer who is experimenting with a different version of the onboarding service*


While request isolation is a powerful way to allow service-level experimentation, it also offers the ability to test multiple changes: If we can control how requests are routed for tests, our developer can share their changes with others, or add experimental services to their routing.


### Considerations with request isolation


- Routing with Sidecars


You’ll need some kind of tooling on your cluster to read headers and route requests intelligently. In order to allow experimentation everywhere, you’ll need something like a service mesh that’s running across the entire cluster. At Lyft, Envoy filled this role. From their blog:


> *A service calls an upstream service by sending a request to its sidecar Envoy, and Envoy forwards the request to a healthy instance of the upstream. The sidecar Envoy’s configuration is kept up-to-date via our control plane, which uses Kubernetes events to update via xDS APIs.*


- Service discovery: that is, anti-service discovery


Chances are if you’re at the point of implementing tools like a shared experimentation cluster, you’re also doing automated service discovery. This can be a problem since we don’t want our experimental services accepting general requests or cluttering a service map. At Lyft, service discovery is prevented with a pod label:


which can be removed when we’re ready to roll our changes into the main line.


- Override headers and context propagation


Lyft had to include metadata in the requests to direct them to offloaded deployments. This metadata tells the infrastructure when to alter the call flow by specifying the services for which the developer wants to override routing rules and the offloaded deployment to which the cluster should direct traffic. To make it transparent to services and service owners, the team opted to put this metadata inside a request header. From their writeup:


> *An important aspect in any distributed tracing system is*[context propagation](https://opentelemetry.lightstep.com/core-concepts/context-propagation/) *. We need the header metadata to be available throughout the lifetime of a request to ensure that services many calls deep have access to the user-specified overrides. We want to guarantee each service along the way properly forwards the metadata along to services later in the request flow — even if the service individually does not care about the content.*


There isn’t one single solution to the problem of context propagation for the critical routing headers for experimentation, but a strong candidate is OpenTelemetry, which already has an infrastructure for propagating headers.


Notably, Signadot also uses OpenTelemetry under the hood to propagate headers within a cluster.


### Benefits of request isolation: a test process that is faster, safer, ***and*** more accurate


The process of implementing these test services for developers (’offloaded deployments’ as the Lyft team calls them) wasn’t a simple one: it required a change to the deploy process, a propagation system for context, and Envoy filter to control traffic. However the benefits were indisputable:


> *This workflow has drastically improved the process required for E2E confidence. We now have >100 unique services deploying offloaded deployments per month.*


Really it’s *confidence* that is the critical concept here: when engineers can be confident that their code will work as expected in staging and beyond, it allows for faster deployments and overall a lighter process for releasing code from development, to staging, and eventually production.


## Razorpay: how developer velocity can slow exponentially


Razorpay is another very large developer organization with the coordination and synchronization problems common to teams with more than a few hundred developers. At[a KubeCon talk in 2021](https://www.youtube.com/watch?v=f-w5Nh92dt8) , Srinidhi S & Venkatesan Vaidyanathan of the Developer Experience team talked about the challenges they solved with custom tooling. They posted this on the[Razorpay github](https://github.com/razorpay/devstack) :


> *At razorpay, we run all our workloads on kubernetes. Like any other mature organization, we have an involved CI/CD practice with extremely sophisticated pipelines. While this works great for all production and pre-production workloads, we have been noticing over a period of time a bunch of development challenges. In essence, the goal is to Simplify developer workflow and reduce the time taken to rollout features independently.*


This section is based both on Razorpay’s KubeCon talk, their Github repository with a specific solution, and a direct conversation with one of their developer experience leaders.


### Where does ‘testing’ happen?


During their KubeCon talk, Srinidhi showed this table of where testing can occur.


It’s important to think about this question deeply, since we often think of testing as only happening around the CI/CD stage, or even later in automated testing of staging. But the reality is that right when a developer first writes code, in the older model where a monolith was fully emulated on the dev’s laptop, the developer is already checking how well her code is working.


### The Problem: A formal process for pushing to staging


As referenced in part 2 of this series, an early solution is some kind of namespaced duplication of the shared testing environment. This works fine for smaller teams but quickly fails to scale. At Razorpay large teams were all relying on a single staging instance. Since anyone’s commits could break this staging instance for all developers, the process evolved where multiple approvals were required before pushing to staging. Essentially this team had scaled until the same problems with friction in deploying to production were now replicated in staging.


Developers ended up waiting days or even weeks to see their code on staging, and a push from staging to production often required cherry-picking commits, a laborious process.


### Initial solutions: telepresence for isolation


As in the Lyft section above, an ideal solution lets developers isolate only a single component from the cluster, experiment on it, not interfere with a shared environment. While Lyft tried using onebox, the Razorpay team tried Telepresence. With Telepresence, you create global intercepts that intercept all traffic going to a service in your remote cluster and route it to your local environment instead.


The issue with telepresence proved to be the lack of fidelity in this local environment. Also, since telepresence couldn’t host other services like data stores, connectivity with a database had to be configured separately.


These two issues both hurt the relative speed of this solution and, inasmuch as custom database connections are not really identical to what happens after merging to staging, the accuracy of testing was also affected.


### Devstack: a highly opinionated developers tool


The focus of this series is on very large engineering teams. When looking at teams approaching the largest existing scale, it’s common to find the solution was an internal tool that isn’t available to the public. However with Razorpay their tooling has been shared as an open source tool. Meet Devstack


> *Devstack, offers a set of tools to help build and develop code on the individual developer’s laptop, as if they are working on a cloud environment.*


Devstack offers real insight into one solution to the dev isolation problem on a shared cluster. It is, as the team freely admits, highly opinionated about request handling.


### Specific solutions: One big ingress


While request routing remains the key to this solution, the implementation varies a bit from the others listed. Critically, all inter-application communication is handled by ingress, meaning that ingress is the only place that experimental requests need to be detected and routed


*From the Devstack docs, ingress is key to this engineering*


The service routing is configured using Traefik 2.0’s[IngressRoute](https://doc.traefik.io/traefik/v2.0/providers/kubernetes-crd/) . With helm hooks to see when an experimental service is being deployed.


For header propagation, once again we see OpenTelemetry (at the time of the talk still referred to as Opentracing) as an essential tool: the OpenTelemetry “baggage” headers can care arbitrary header information across the stack.


### Results: smoother experimentation


While the Razorpay team hasn’t shared specific statistics from the success of their project, it has been going strong for over two years, with their team reporting that ‘testing deploys is now much smoother and easier than previously’


## Conclusions


As teams evolve, they will need high fidelity testing on a shared environment to accurately reflect Staging and then Production. Conflicts over this testing/development environment are inevitable, so there must be some way to experiment without impacting others. Purely local mocks have their use, but once your stack grows large and complex you’ll end up spending a great deal of effort maintaining these copies , meaning that tests are often fast or accurate, but not both.


With request isolation in a shared cluster, large teams can experiment to their heart’s content, knowing that their changed code can interact with the shared resources without breaking things for anyone else.


While both teams mentioned in this article had the scale and budget to develop their own tools, Signadot can help your team implement the same level of request-isolated experimentation in your cluster at the scale of hundreds of developers. Teams like[DoorDash](https://www.signadot.com/case-studies/how-developers-at-doordash-get-10x-faster-feedback/) have used Signadot to greatly accelerate the developer feedback cycle.


## How Large Teams are Testing on Kubernetes


‍
