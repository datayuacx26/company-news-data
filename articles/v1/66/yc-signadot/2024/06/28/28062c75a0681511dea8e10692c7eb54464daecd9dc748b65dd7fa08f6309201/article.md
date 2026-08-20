---
schema_version: "1.0.0"
document_id: "28062c75a0681511dea8e10692c7eb54464daecd9dc748b65dd7fa08f6309201"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-shift-testing-left-4-tactical-models/"
published_at: "2024-06-14T20:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:7c90a98337bce86e56e6b418c96f0e9c7f3a8dc73ded8317648a16d8db2f6e8f"
---

# How to Shift Testing Left: 4 Tactical Models

Originally posted on[The New Stack](https://thenewstack.io/how-to-shift-testing-left-4-tactical-models/) .


I previously wrote about the[strategic reasons for shifting testing left](https://www.signadot.com/blog/why-we-shift-testing-left-a-software-dev-cycle-that-doesnt-scale/) and the[role of QA](https://www.signadot.com/blog/why-shift-testing-left-part-2-qa-does-more-after-devs-run-tests/) when this happens. Now let’s look at four possible models for shifting your testing left. All four models have their advantages and will work for teams at a certain scale and configuration.


These four models are ways many teams try to fix problems with inaccurate tests or having a very long, slow feedback cycle.


## **1. Developers Expand Integration Tests: The “Git Gud” Model**


The reply “git gud” is for someone offering advice to another gamer: rather than changing their strategy, they just need to get better at what they’re already trying. In this case, it’s probably already considered part of developers’ responsibilities to write unit, integration and contract tests. But this model implies that this either isn’t happening or not happening with enough diligence. It is possible that for integration tests or end-to-end tests, it’s a QA team currently writing these tests, but developers of individual services should always have some say in how these tests are written.


### **One Option: Pact Contract Tests**


If you’re looking to improve your testing before services are directly integrated, one way to improve the quality is by adopting[Pact](https://docs.pact.io/) .


Pact testing focuses on the interactions between two separate components, typically known as service consumers and service providers. These tests are designed to verify that the communication between these components is conducted as expected according to a mutually agreed-upon contract. The contract itself specifies the rules for requests that a consumer can make and the responses it expects from the provider.


The core principle of Pact contract testing is to enable development teams to isolate the behavior of their services in a controlled environment. Using simulating requests and responses, teams check whether their service can correctly handle expected interactions without the need to set up and rely on the actual services with which they integrate. The tests are executed such that both the consumer and the provider independently verify adherence to the agreed contract.


Consumer-driven contracts are a distinguishing feature of Pact contract tests. Consumers of services specify the requirements of their interactions, which the providers must then meet. This framework shifts away from “testing for testing’s sake” and moves back to testing as fundamentally concerned with use cases. The consumers of a service become responsible for the testing of service providers.


Pact contract tests are particularly useful in microservices architectures where services are developed and deployed independently. They help ensure that changes in one service do not unexpectedly break another service that depends on it, making continuous integration and deployment processes more reliable.


### **Contract Tests and the Annoying Gap Between Theory and Practice**


When I converse with DevOps and sysadmin professionals to ask how they handle the difficulties with testing microservice architectures, their first answer almost always comes back to this solution: Developers need to write or at least run contract[tests and integration](https://www.signadot.com/blog/the-struggle-for-microservice-integration-testing/) tests; services need to be isolated enough that passing these tests strongly indicates that the service will run successfully with others in the staging environment.


My question to anyone who wants to rely on contract and integration tests with microservices is: Is this working right now? Has it worked in the past? And if not, what are you changing beyond a resolution to do it better?


### **The ROI of Increased Contract Testing**


If you drastically increase the number of contract and other single-service testing, what is the return on investment (ROI) on this effort? While ideally, contract testing would find any integration efforts ahead of time, this rarely aligns with reality. There are inevitable problems when our service interacts with the real versions of other services and dependencies.


Further, there’s a cost involved in the work of producing mocks for other services that add to the implementation costs.


## **2. Leverage Feature Flags**


To allow developers, and then a slowly expanding audience, to try out a feature without showing it to everyone, feature flags can seem like an appealing choice. Feature flags are primarily used to release features in production to a small set of users, then gradually expand to the rest. While feature flags have their benefits, this is a relatively slow process that might not work for experimenting and testing new features


When using feature flags for testing in shared staging environments, there are significant benefits and drawbacks that prevent all but core use cases from delivering value. Let’s take a look at each:


The main advantage of feature flags for testing would be as a “bonus” use case for something you’ve already implemented for another reason. The core use case for feature flags is still for previewing visible features for subsets of users, so testing is just a “nice to have” addition.


### **Feature Flags Aren’t Ideal for Testing**


- **Increased complexity:** Introducing feature flags adds complexity to the codebase and configuration management. As the number of flags grows, maintaining and managing them becomes more challenging, potentially leading to technical debt.
- **Testing overhead:** While feature flags enable more granular testing, managing test cases for different flag configurations can become cumbersome. It requires careful planning and coordination to ensure comprehensive test coverage across all possible scenarios.
- **Fragmented codebase:** Overuse of feature flags can result in a fragmented codebase, with multiple conditional branches and configurations scattered throughout the code. Among the scaling issues, this fragmentation is probably the most significant, as the exact state of a “main” branch can drift as multiple developers and users become used to testing and working on a branch that requires a complex feature flag setting.
- **Poor fit for testing:** How do you test a code rebase with a feature flag? What about an end-to-end performance increase? Or extensive code linting to clear warning messages? Feature flags aren’t really intended for you to test every code change, and in these scenarios, any feature flag solution will feel like a “hack.”
- **Performance overhead:** Feature flags slow both testing and the release of features. Most significantly, requiring release all the way to production to do the first real integration testing means the time between writing a feature and testing how it works is lengthened significantly.


While feature flags offer significant benefits for more accurate shared testing and huge advantages for sharing new features with stakeholders, they come with inherent trade-offs. Feature flags really make more sense for inter-team coordination or showing a new feature to the head of sales, rather than as a way to do your first internal integration tests. We’ve written in the past about the[challenges of using feature flags for testing](https://www.signadot.com/blog/pitfalls-of-using-feature-flagging-for-testing-in-shared-staging-environments/) .


## **3. On-Demand Environments**


When trying to explain the idea behind Signadot, people often ask if we’re just another tool for creating environments on demand. While we are trying to make it easier for developers to work an experiment, the “one cluster per developer” or even “one cluster per team” approach implied by developer environments is explicitly what we’re trying to avoid. Let’s go over the idea along with its[pros and cons](https://www.signadot.com/blog/namespace-based-environments-for-testing-pros-and-cons/) .


### **Separate Developer Clusters: How to Implement**


The idea that you’d run a whole new cluster for every single developer is so impractical that it’s not really worth criticizing. After all, if we’re already bearing the significant expense of running a dev and staging environment cluster, the idea of “what if we multiplied this cost by our number of developers?” is clearly silly. However, often, the general notion of separate dev clusters is sold to us with a few improvements from a naive model.


- **Not per-developer clusters but per team:** A self-evident optimization is to group developers and let them collaborate on a single test cluster. This has the advantage of letting them preview changes along with others’ updates. However, even in a small team, each developer is slowed day to day by waiting for others to complete their tests and/or having to verify that it wasn’t another dev’s changes that broke something.
- **Dev clusters that are only running when needed:** If we optimize our process — and we do generally expect modern environments to support the spinning up and down of complex architectures — we can greatly decrease the infrastructure cost of this option by only running clusters when they’re needed. This option comes with a drawback: we’re either running them most of the time (for example only running them during working hours) or making the developer wait while we start up the cluster each time she needs to experiment.
- **Namespaces instead of separate clusters:** You can use[Kubernetes namespaces](https://www.signadot.com/blog/namespace-based-environments-for-testing-pros-and-cons/) to segregate some services for testing and experimentation. With namespace rules, you can define interactions with other services. This option is appealing and begins to suggest the solution that we’ll explore below: we want our experiments to still be able to rely on some services as a “base layer,” and we want our altered services to not interfere with others’ testing. There are some missing features to the namespace option, namely without a time-to-life expiry date there’s a chance for namespaces to persist long past their useful date.


The concern above about namespaces is really the Achilles heel of all these separate cluster solutions: these alternate clusters slowly falling out of sync with “main.” However we create a separate cluster for a developer’s experiments, we’re creating a separate environment in which no one really owns responsibility for keeping it updated. It only takes a few incidents of a developer testing their changes on` Dev cluster alpha` , then finding that updates made to staging have led to surprise failures for the developer to stop trusting their special cluster. This results in them using the environment less and therefore being less likely to ensure it stays updated.


“But wait,” I hear you say. “Surely this de-synchronization problem can be defeated with software. Couldn’t a master image of the dev cluster exist, updated regularly, that all dev clusters regularly update from?” This leads us to a final possible solution:


- **A “main” dev cluster from which all others draw updates:** A version of this solution has been used in a large number of teams, but the problem is startup time and how it affects developers’ work. In one version, the dev clusters run on devs’ laptops, in others they use shared infrastructure. Either way, at the beginning of the day clusters are updated from a main image with the latest version from staging, so no one is wondering whether their pull request (PR) will successfully run there when their testing is done. However, as the cluster increases in complexity, this morning pull from a main image becomes slower and slower until it can take a few hours to give the developer an environment to test and experiment.


The issues described with development cluster separation are all problems of scale: The right team at the right size will be completely pleased with development clusters. But you should know that in doing so you’re limiting your ability to scale your team past a certain point. For teams with hundreds of developers, it should be clear that trying to stand up separate clusters for each developer to test in is a non-starter due to updating and keeping those clusters in sync.


## **4. Automated Tests Post-Merge**


This fourth option is something of a hybrid. Essentially, run a bunch of automated tests (can be end-to- end, API, integration, etc.) post-merge in a shared environment. These tests are typically specific to a service that are run when PRs for that service are merged. If tests fail, the PR is automatically rolled back.


This approach has the benefit of a shared, high-quality environment to test in, and a relatively large suite of tests has time to run post-merge.


The issue is again with the contention in the shared environment. When there are many parallel PRs merged, the tests don’t have isolation and test failures may be due to issues in other PRs. So debugging is still a challenge.


## **So What Is the Solution to Shifting Left for Microservices?**


A follow-up article will propose a fifth option that may be better for scale challenges. We’ll discuss how request routing and other smart tools can let your developers safely share a single environment for testing. This approach, used by enterprise teams at Uber, Lyft, AirBnB and others, can unlock shift-left testing at large team scales.
