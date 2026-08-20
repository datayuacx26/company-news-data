---
schema_version: "1.0.0"
document_id: "e1b588f8d11c4bc219ae9f0cb3f3126e71de06bf27d591f4a4f46e8e57581f07"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/cutting-the-high-cost-of-testing-microservices/"
published_at: "2024-08-30T19:52:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:e4b91a521cd6c2e1c3ed01b355d3555963d42203f9350b538cdf970acc21ac79"
---

# Cutting the High Cost of Testing Microservices

*Originally posted on*[The New Stack](https://thenewstack.io/cutting-the-high-cost-of-testing-microservices/) *.*


**While duplicating environments might seem a practical way to ensure consistency, the infrastructure can be costly. A look at alternative strategies.**


A specter is haunting modern development: As our architecture has grown more mature, developer velocity has slowed. A primary cause of lost developer velocity is a decline in testing: in testing speed, accuracy and reliability. Duplicating environments for[microservices](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) has become a common practice in the quest for consistent testing and production setups. However, this approach often incurs significant infrastructure costs that can affect both budget and efficiency.


*Testing services in isolation isn’t usually effective, we want to test these components together.*


## High Costs of Environment Duplication


Duplicating[environments involves replicating entire setups](https://www.signadot.com/blog/environment-replication-doesnt-work-for-microservices/) , including all microservices, databases and external dependencies. This approach has the advantage of being technically quite straightforward, at least at first blush. Starting with something like a namespace, we can use modern container orchestration to replicate services and configuration wholesale.


The problem, however, comes in the actual implementation.


For example, a major FinTech company was reported to have spent over $2 million annually just on cloud costs. The company spun up many environments for previewing changes and for the developers to test them, each mirroring their production setup. The costs included server provisioning, storage and network configurations, all of which added up significantly. Each team needed its own replica, and they expected it to be available most of the time. Further, they didn’t want to wait for long startup times, so in the end, all these environments were running 24/7 and racking up hosting costs the whole time.


*While namespacing seems like a clever solution to environment replication, it really just borrows the same complexity and cost issues from replicating environments wholesale.*


## Synchronization Problems


Problems of synchronization are one issue that rears its head when trying to implement replicated testing environments at scale. Essentially, for all internal services, how certain are we that each replicated environment is running the most updated version of every service? This sounds like an edge case or small concern until we remember the whole point of this setup was to make testing highly accurate. Finding out only when pushing to production that recent updates to Service C have broken my changes to Service B is more than frustrating; it calls into question the whole process.


Again, there seem to be technical solutions to this problem: Why don’t we just grab the most recent version of each service at startup? The issue here is the impact on velocity: If we have to wait for a complete clone to be pulled configured and then started every time we want to test, we’re quickly talking about many minutes or even hours to wait before our supposedly[isolated replica testing environment](https://www.signadot.com/blog/shifting-testing-left-the-request-isolation-solution/) is ready to be used.


*Who is making sure individual resources are synced?*


This issue is, like the others mentioned here, specific to scale: If you have a small cluster that can be cloned and started in two minutes, very little of this article applies to you. But if that’s the case, it’s likely you can sync all your services’ states by sending a quick Slack message to your single two-pizza team. **‍**


Third-party dependencies are another wrinkle with multiple testing environments. Secrets-handling policies often mean that third-party dependencies can’t have all their authentication info on multiple replicated testing environments; as a result, those third-party dependencies can’t be tested at an early stage. This puts pressure back on staging as this is the only point where a real end-to-end test can happen.


## Maintenance Overhead


Managing multiple environments also brings a considerable maintenance burden. Each environment needs to be updated, patched and monitored independently, leading to increased operational complexity. This can strain IT resources, as teams must ensure that each environment remains in sync with the others, further escalating costs.


A notable case involved a large enterprise that found its duplicated environments increasingly challenging to maintain. Testing environments became so divergent from production that it led to significant issues when deploying updates. The company experienced frequent failures because changes tested in one environment did not accurately reflect the state of the production system, leading to costly delays and rework. The result was small teams “going rogue” and pushing their changes straight to staging, and just checking if they worked there. Not only were the replicated environments abandoned, hurting staging’s reliability, it also meant that the platform team was still paying to run environments that no one was using.


## Scalability Challenges


As applications grow, the number of environments may need to increase to accommodate various stages of development, testing and production. Scaling these environments can become prohibitively expensive, especially when dealing with high volumes of microservices. The infrastructure required to support numerous replicated environments can quickly outpace budget constraints, making it challenging to maintain cost-effectiveness.


For instance, a tech company that initially managed its environments by duplicating production setups found that as its service portfolio expanded, the costs associated with scaling these environments became unsustainable. The company faced difficulty in keeping up with the infrastructure demands, leading to a reassessment of its strategy.


## Alternative Strategies


Given the high costs associated with environment duplication, it is worth considering alternative strategies. One approach is to use dynamic environment provisioning, where environments are created on demand and torn down when no longer needed. This method can help optimize resource utilization and reduce costs by avoiding the need for permanently duplicated setups. This can keep costs down but still comes with the trade-off of sending some testing to staging anyway. That’s because there are shortcuts that we must take to spin up these dynamic environments like using mocks for third-party services. This may put us back at square one in terms of testing reliability, that is how well our tests reflect what will happen in production.


At this point, it’s reasonable to consider alternative methods that use[technical fixes](https://www.signadot.com/blog/how-to-fix-flaky-tests/) to make staging and other near-to-production environments easier to test on. One such is[request isolation](https://www.signadot.com/blog/shifting-testing-left-the-request-isolation-solution/) , a model for letting multiple tests occur simultaneously on the same shared environment.


## Conclusion — A Cost That Doesn’t Scale


While duplicating environments might seem like a practical solution for ensuring consistency in microservices, the infrastructure costs involved can be significant. By exploring alternative strategies such as dynamic provisioning and request isolation, organizations can better manage their resources and mitigate the financial impact of maintaining multiple environments. Real-world examples illustrate the challenges and costs associated with traditional duplication methods, underscoring the[need for more efficient approaches](https://www.signadot.com/blog/we-need-a-new-approach-to-testing-microservices/) in modern software development.


At Signadot, we’re committed to letting every developer test their code, a shift left of the QA process that makes testing more reliable with a shorter feedback loop. Our[request isolation and shared test environment](https://www.signadot.com/) solution cuts down significantly on infrastructure costs and has helped teams like[Brex](https://www.signadot.com/case-studies/brex-uses-signadot-to-scale-developer-testing-across-100s-of-engineers/) ,[Yellow](https://www.signadot.com/case-studies/how-yellow-ai-enabled-parallel-feature-development-using-signadot/) and[Earnest](https://www.signadot.com/case-studies/how-earnest-empowers-developers-for-early-testing/) .
