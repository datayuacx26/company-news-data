---
schema_version: "1.0.0"
document_id: "cb63fd27d7d6716231b566d638b2809f7efeb4e7f0cf67b69e313c6e7b9deec2"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/re-imagining-test-environments-for-microservices/"
published_at: "2022-11-08T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:a1a0a839d266075505663c3a48ae3d7f440a353f8815d005ca6dc9e7be996ade"
---

# Re-imagining Test Environments for Microservices

## The Problem with Pre-Production Testing of Microservices


Organizations that develop cloud-based applications often break complex apps into smaller, more manageable components owned by small, self-contained teams. These components, called microservices, can be developed and released to production independently. Microservices architectures make applications easier to scale and (in theory) make it faster to develop new features.


One problem with microservices, however, has to do with testing. When testing bug fixes or added functionality, teams ideally would have the entire application at their disposal to make sure their changes work as expected and don’t regress existing functionality. It’s difficult to test microservices when you use a distributed application architecture model because each component is a small part of a much bigger puzzle. In other words, there’s no comprehensive pre-production environment with all the services running where developers can *independently* test changes as they apply to the whole.


Here are four common testing phases in the software development lifecycle:


1. Local Testing (development environment)
2. Continuous Integration (CI)
3. Pre-production (Staging)
4. Production


When you have many services, it’s hard to perform high-fidelity software testing in the first two phases, as these environments significantly diverge from production. Nonetheless, developers often perform a minimal level of testing in phases one and two to capture basic feedback.


In fact, it’s increasingly common for independent teams to test their components separately without testing them against the larger environment. However, simply testing one component isn’t sufficient due to the dependencies of other services. All components communicate with each other dynamically, and the behavior of your component depends on interactions with cloud-based resources (e.g., databases and message queues), third-party APIs, and interaction with other services.


Once code is merged, it’s usually deployed into a staging or pre-production environment, where developers learn for the first time if their application works as a whole. They gain higher quality testing feedback because the environment attempts to mimic production as closely as possible.


Teams that develop microservices-based apps tend to rely heavily on testing in the staging environment. Because the staging environment is expensive to set up as a near-replica of production, it’s commonly shared among many teams. Not unexpectedly, different teams test their changes simultaneously and end up stepping on each other’s toes. As everyone continually merges their changes, organizations often incur significant delays in getting the environment to a stable state.


In addition, the feedback cycle is expensive and slow. Troubleshooting becomes a real challenge, as it’s not always clear what caused any problems that occur. For instance, teams often have to roll back a change, fix the problem, reapply the change to the staging environment, and retest. Continual rework of code wastes developers’ time and impacts the speed and frequency with which teams deploy to production. The entire testing process is expensive and slow.


## Existing Solutions Miss the Mark


The staging environment frequently becomes a bottleneck because many teams depend on it at the same time. In response, organizations often implement one of three solutions, each with its own set of problems related to scalability, environment costs, debugging costs, and the opportunity cost of development.


### Reducing Reliance on the Staging Environment


The first solution is to reduce reliance on the staging environment by doing more testing in the local dev environment or in smaller environments in the cloud. This solution may be reasonable for smaller organizations that have a less complex app stack with fewer than 10 microservices. However, as the stack increases in complexity, local testing becomes a problem because these dev environments end up being lightweight versions of the production environment and tend to diverge from production considerably.


The reality is that these types of testing environments may help you get your code up and running, but they don’t provide a high-fidelity testing signal. Further, the cost of providing an environment for each developer naturally increases as the engineering team grows, making scalability a problem.


### Locking, or “Renting” the Staging Environment


Another common solution is to “rent out” the staging environment to one team at a time in order to perform higher fidelity testing at a low environmental cost.


This locked staging environment creates a bottleneck even for a moderately sized team of 25+ engineers. Every team has to wait for the environment to be available so they can test their changes, which means developers can’t develop in parallel. Ultimately, it doesn’t solve the problem of slow and costly debugging that occurs fairly late in the game at the staging/pre-production phase.


### Creating a Pool of Staging Environments


Some organizations use a brute-force approach of scaling environments by cloning multiple copies of an environment. Duplicating the staging environment creates a pool of staging environments that enable developers to develop in parallel. The benefits include higher fidelity testing, shorter feature loops, and moderate scalability.


The downside is that it can take significant time to set up these copies. Initially, you have to describe the entire environment, and only then can you make copies of it. At scale, managing and keeping these environments up to date is a daunting task, especially if you have a large number of cloud resources and microservices. In addition, every staging environment can cost tens of thousands of dollars to spin up and operationally maintain. High infrastructure costs usually make this solution prohibitively expensive to operate and maintain, making it a Band-Aid solution.


## A More Scalable, Effective Microservices Environment Solution


Most solutions in the marketplace don’t provide developers with a pre-production testing environment that:


1. Resembles production as closely as possible to provide high fidelity testing
2. Provides fast feedback to developers
3. Scales affordably


Signadot sandbox environments, are on-demand, short-lived instances created within an existing (pre-production) Kubernetes cluster for testing purposes. The solution enables integration testing, feature testing, end-to-end testing, performance testing, etc. while simulating high-fidelity testing. It introduces the concept of multi-tenancy in a Kubernetes cluster in a scalable and cost effective way. These sandboxes offer a new approach that transforms the way testing is integrated into developer and DevOps teams.


Essentially, sandbox environments achieve multi-tenancy at the application level by using traffic routing and labeling. When you submit test requests to the system, it is dynamically routed to specific versions of service that together constitute the manifestation of an environment. The model is incremental in nature, meaning that you’re able to introduce only the microservices that have changed into the environment versus duplicating the entire set of services and physical resources for the sandbox environment. For example, if you’re only changing 3 of 100 microservices, you can introduce a new environment with just those 3 microservices and reuse a shared pool of services and resources.


This shared pool of services is called the baseline environment, which is typically the main version of services. Test requests to different versions of services can be labeled and dynamically routed. Traffic labeling and routing help to ensure the test requests you send to the system are directed only to your version of the microservice and not to any other version of the service used by other developers.


Sharing a baseline environment and only spinning up components that have changed is a game changer. Unlike traditional environments that incur a sharp increase in costs at scale, these sandboxes have manageable costs due to smart resource sharing. Organizations with hundreds of microservices can use sandbox environments cost effectively because the solution scales. No matter how many services you have, the incremental cost of an ephemeral environment is very low.


## Environments with application level multi-tenancy


Some[leading tech firms](https://eng.lyft.com/scaling-productivity-on-microservices-at-lyft-part-3-extending-our-envoy-mesh-with-staging-fdaafafca82f) use this model of multi-tenancy to cost effectively provide test environments at scale designed specifically to meet the needs of their internal infrastructure and proprietary microservices. For most organizations, though, implementing an effective and reliable solution in house would require a large team of highly skilled engineers and and years of development time. The initial implementation alone could easily cost millions of dollars and require substantial effort, not to mention ongoing maintenance efforts and costs. Understandably, most organizations would rather devote their engineering time and effort to tasks that move their business forward.


### Introducing Signadot Sandboxes


Signadot has a solution with a much wider application in the marketplace than existing bespoke solutions. This innovative solution can be used by anyone running services on Kubernetes, and it’s far less expensive than what it would cost to build it in house.


Signadot is a Kubernetes-native platform that enables organizations to spin up hundreds or thousands of lightweight environments, called *Signadot Sandboxes* . These sandboxes support DevOps teams that are using Kubernetes to power their cloud-native SaaS applications. Unlike traditional environments, Signadot Sandboxes use application-level multi-tenancy to share resources. At the same time, isolation is built into these environments so multiple developers can test their services without getting in each other’s way. This allows testers to create thousands of Sandbox environments within a single Kubernetes cluster without runaway infrastructure costs or operational burden.


Signadot allows teams to leverage their existing pre-production (staging) environment while gaining multiple high-fidelity environments. This allows for a scaled approach to testing by creating one environment for every developer (or pull request) and testing in an environment that resembles production.


### Use Cases


Signadot Sandboxes enable streamlined integration testing. For example, development teams can run tests in a real environment even before merging code by integrating the sandbox in a CI pipeline and running a quick API test using API testing tools like Postman. Beyond that, the sandboxes can accommodate both end-to-end and feature testing, which were formerly possible only in a shared staging environment.


Scalable feature development significantly reduces the bottlenecks that commonly occur in a shared environment. For example, a new feature could involve changes to multiple microservices that span teams and code repositories. Using Sandboxes, developers collaborate on testing such features end-to-end before merging. The ability to do so before merging allows for fast iterations on APIs and a view of the end user experience without impacting other developers.


One common request involves spinning up stateful resources in the context of a sandbox environment. The open source[Resource Plugin Framework](https://www.signadot.com/docs/reference/resource-plugins) allows developers to spin up ephemeral, stateful resources. Organizations can also write custom plugins for various databases, message queues or cloud resources.


## Scaling Environments for Growing Teams


DevOps teams that spin up Signadot Sandboxes to enable *shift-left testing* for microservices will:


- Discover integration issues before merging by testing against real services, data, and third-party dependencies.
- Merge with confidence with faster feedback loops, fewer regressions / rollbacks, and cleaner code.
- Gain a self-serve approach to spinning up environments and greater control for better code quality.
- Promote team collaboration on new features much earlier in the process.


This enables developers to test in a real-world environment and test much earlier.


Signadot uses multi-tenancy and smart sharing of resources in Kubernetes environments, resulting in an efficient, cost-effective model that can scale to hundreds of developers and microservices. To learn more about Signadot Sandboxes,[read the docs](https://www.signadot.com/docs/overview) .


‍
