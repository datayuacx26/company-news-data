---
schema_version: "1.0.0"
document_id: "a577edfd708056480840e06440a9a0f152dc46b7744722461c12af06c56ee342"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/large-engineering-teams-testing-on-k8s/"
published_at: "2023-05-03T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:98b6dc403a4edeed6247a0f9b3ad7048efd849e026a91448f0aa1fc8e6ff649e"
---

# Why large engineering teams are testing on Kubernetes

## Introduction


In the last few years we’ve seen development teams move away from purely local development, and a more cloud-based developer environment. The goal is increased development velocity, and controlling problems of imperfectly replicated environments.


One of the exciting things about this time is that it is not clear how developers will work on production applications in the near future: Will we improve our tools to replicate the production environment locally, will we write and run all code in a cloud environment, or something in between? The topic seems to be everywhere, just last week at Railsconf we saw a talk from Kedasha Kerr, a Github engineer, titled “[The End of an Era; Kiss your Local Dev Environments Goodbye?](https://railsconf2023.sessionize.com/session/453558) ”


In this piece we take a look at a few enterprise team’s solutions, and try to categorize them.


### The terminology of dev environments


Often when talking about where developers work with their new code, we talk about them ‘testing’ code. This is a stage before any automated unit tests or end to end tests, where the developer is essentially playing around with how their code interacts with other resources. These two phases may not be clearly different, as with test-driven development. In this article we use the term ‘testing’ for brevity, even though testing may also be a separate, later stage of deployment.


## Commonalities in the quest for developer velocity with microservice development


After trying to write separately about four different enterprise engineering teams, I found so many similarities between their stories that I’ve combined all of them into a single narrative. What’s notable is not just that their final conclusions were so similar, but also that each team tried similar solutions before settling on a cloud-based development environment.


The cases I looked at included[Lyft](https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-3-extending-our-envoy-mesh-with-staging-fdaafafca82f) ,[Reddit](https://www.reddit.com/r/RedditEng/comments/12xph52/development_environments_at_reddit/) ,[Doordash](https://doordash.engineering/2022/06/23/fast-feedback-loop-for-kubernetes-product-development-in-a-production-environment/) ,[Eventbrite](https://blimpup.medium.com/why-eventbrite-runs-a-700-node-kube-cluster-just-for-development-e5a0df95a45c) and[Prezi](https://medium.com/swlh/the-3-phases-of-prezis-development-environment-evolution-f3ce3c1e813b) . Not all these teams ended up with using a shared Kubernetes cluster, but they all were trying to solve similar problems with similar methods, and all faced common roadblocks on the path.


## Costs vs. Benefits


When considering a change to the developer environment, a key question to ask is: how do you determine whether it’s worth investing in a significant overhaul to your development workflow before development comes to a standstill?


There’s a reason that a piece on this strategy with the Eventbrite team is entitled: “Why Eventbrite runs a 700 node Kube cluster just for development.” The objection has to be dealt with up front: when running a shared cluster to test you’re code, you’re adding dev time for the tool and cluster, and maintenance costs.


Take a look at this quote from Eventbrite on the benefit of testing on a shared K8s cluster:


> *Have you ever heard a developer say “but it worked for me locally when I ran the tests?” Consistency improves by running in the cloud. The ability to share developer environments proved to be very helpful when trying to understand test failures or work on issues that were hard to reproduce.*


This is the core of the benefit of testing on a shared cluster: the finding of unknown unknowns earlier, and the permanent banishment of the dreaded phrase “It works on my machine.”


I won’t dive too much further into explaining why a consistent, standardized environment is critical to developer velocity. Look for a future article that lays out this argument!


## Phase One: each Developer is an island


Teams like Prezi facilitated machine setup for developers by providing hooks for service installation, testing, and running. The tool also managed inter-service communication. However, the uniqueness of each developer’s machine often resulted in hook script failure due to dependency conflicts. Engineers spent significant time fixing these issues, only to face new ones when revisiting the service months later.


This de-synchronization is an inevitable result of having a large development team without any daily reason to synchronize all dependencies. Often this desynchronization isn’t evident until after hours of debugging.


## Phase Two: Docker Compose


This is what most of us think of when we talk about using containerization tools to share a development environment. By containerizing all dependencies, and orchestrating with Docker Compose, you force developers to synchronize dependencies at the start of the day. The huge advantage of this approach over Phase One is that you’re not waiting for a failure to realize that your environment needs updating.


The great disadvantage, sadly, is that you’ll quickly find yourself turning your macbook into a kiln:


The daily process of grabbing new containers and rebuilding just to try out some development changes can start to drastically affect developer velocity.


A problem with the Docker Compose route is that the costs are often hidden: all that time of your laptop bogging down while it tries to run a complex workload isn’t going to show up as days of development time lost. Rather it will steal 10 minutes from every hour. It won’t show up as a block that stops us from releasing a new feature for a month, but rather as the reason that you’re still struggling to get a feature working as 5pm comes and goes.


You could address the local workstation resource limits issue by providing a VM in the cloud and running docker compose there. However, such systems get hard to manage when the number of services and associated DBs etc grow beyond 15-20.


For the Reddit team, this issue is identified as a[known limitation](https://www.reddit.com/r/RedditEng/comments/12xph52/development_environments_at_reddit/) . They have worked on a custom tool to minimize rebuilds and re-downloads to try to minimize the startup time each day, and have migrated to a kubernetes cluster to share most of the resources needed while developers are experimenting. This does require the maintenance of a custom tool, implementing Tilt, to keep under control. Whether your team has the bandwidth not just to change your dev environment process but to also maintain internal tooling for the sole purpose of cutting into significant morning build times, is up to you.


## Phase Three: Remote environment on Kubernetes


A solution to development environment needs is to build a remote environment on top of Kubernetes. This approach allows for the developer’s code and dependencies to be managed centrally by the DX team, with only a light CLI and a few libraries running on the developer’s laptop.


At Eventbrite, the solution is similar, with a custom tool called **yak** to handle the interactions between the developer’s code and the shared cluster. Another great benefit of **yak** was how it kept things simpler for developers to dip a toe into the Kubernetes experience:


> *We kept it as minimal as possible and the configuration files are plain Kubernetes manifest files. The intent was to feed developer curiosity so they learn more about Kubernetes over time.*


This ‘light-weight’ interaction can slowly build comfort within your team, which is after all the ideal that ‘DevOps’ was supposed to capture!


## How to interact with a shared cluster


The two modes of development: some you want synced perfectly to the sate of work from other teams, and others you want to be able to modify to your design.


*From the Reddit Eng blog, showing how most resources are shared and won’t be* *modified while the developer experiments*


At Prezi, Developers have the freedom to experiment in their own sandbox, a Kubernetes namespace, and can confidently run services in two different modes:


- Dependency mode: In this scenario, the tool is told that a given set of services are needed (and based on the descriptors mentioned above it also figures out transitive dependencies). It then simply deploys — with Helm and Helmfile — pre-built and pre-optimized containers of the dependencies.
- Development mode: This is for the services that are being actively developed. We use Skaffold to build the images and sync changes so developers quickly see the result of their modifications and get feedback.


On a general level, you need to set up services “under test” and the remaining “stable dependencies”. The dependencies can be spun up on-demand, as in the case of namespace based approach or can be satisfied by a shared layer that’s always present and continuously updated via a CI/CD workflow, as in the case of request routing approach.


## What if I don’t want to build my own tool?


Admittedly, all of these case studies have ended with a tool that was maintained by an internal Developer Experience team. These DX costs aren’t anything to be sneezed and, and it does beg the question ‘why isn’t there a SaaS solution for this model?’


Signadot is one way that a SaaS tool can help you share a Kubernetes cluster and only experiment on the services you care about:


*Two developers work simultaneously with a shared dev cluster. The environment requires no mocking and lets the operations team maintain a single Development cluster*


Signadot is by no means the only way to share a cluster for development with open source tools. But Signadot does handle some difficult edge cases like the routing of requests between services, making it much easier for multiple developers to share a development cluster.


Check out our[concept docs](https://www.signadot.com/docs/concepts/introduction) if you’d like to see how it works!


‍
