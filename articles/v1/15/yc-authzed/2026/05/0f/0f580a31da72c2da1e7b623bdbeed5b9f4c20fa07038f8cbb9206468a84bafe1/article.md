---
schema_version: "1.0.0"
document_id: "0f580a31da72c2da1e7b623bdbeed5b9f4c20fa07038f8cbb9206468a84bafe1"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/calculate-the-authorization-tax"
published_at: "2026-05-18T07:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:15:16.761066+00:00"
content_hash: "sha256:d4aed9fb73d51f4207611f6c347bbf6e2e185ebeea8ebe11ead71677d8fdb5e7"
---

# Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ

Everyone is paying for authorization.


For most companies, it's not a line item that shows up in a budget, but it's real, nonetheless. At AuthZed, we call it the "Authorization Tax"—the collective financial and business burden of building, maintaining, and evolving an in-house authorization system, usually with staff and resources that are supposed to be doing something else.


With even a cursory look at the time your teams spend on authorization-related tasks, the Authorization Tax is a pretty easy concept to grasp, but quantifying the scope of the Tax is a lot harder. Since we sell authorization solutions, we've gotten pretty good at helping potential customers uncover that cost. And just like income taxes, knowing what you owe is a critical component of building a long-term financial strategy.


With that in mind, here are five tips to help you do the math, and two ways to make the results less scary.


## Input 1: Initial development costs


The most visible cost is developer time.


If you're starting from scratch, the sticker price of a home-grown authorization system is steep. Based on discussions with hundreds of customers, a production-ready system can easily take four to six engineers six to eighteen months of development. You know your fully-loaded developer costs better than anyone, but we've heard numerous stories of $500K to over $2M to get a working system through design, implementation, testing, and hardening.


## Input 2: Maintenance costs


As soon as you build an authorization system, you're a software company managing SLAs to your (internal) customers. Maintenance costs kick in immediately.


Some of that cost is what you'd expect—operational management, bug fixes, and so forth. But authorization isn't an application. It's infrastructure that enables your applications, so it needs to always work, for all your use cases. Expect feature development to be an ongoing maintenance burden, whether you're supporting new application functionality, or adapting to new organizational structures or compliance requirements. If you're doing something new, your authorization system will have to adapt, and that normally means more code.


Our customers report that annual maintenance costs often run 50–100% of their original build cost, trending upward over time.


## Input 3: Productivity loss


This is where the math takes more work, but it's worth doing. Beyond the hours and salaries responsible for keeping your authorization engine running, employee productivity through an organization suffers when essential shared infrastructure is a side gig. When authorization logic is embedded directly in applications, Security, HR, Compliance, and other teams with critical needs to modify or audit permissions are now dependent on Engineering. As one customer told us, they were "dead in the water until devs could make time."


There's also an ongoing training burden. When engineers leave, institutional knowledge goes with them. New hires must learn a bespoke system with no external documentation, no community, and no easy answers from AI assistants.


While it's hard to quantify the productivity impact of DIY authorization when that's the only option, many of our customers evaluate our services by establishing baselines with holistic, business-focused metrics like Deployment Frequency. When they evaluate alternative paths such as those discussed below (often on greenfield projects as a Proof of Concept), these benchmarks can shine a light on a lot of waste.


## Input 4: Opportunity cost


The hardest costs of the Authorization Tax to quantify are the things that *didn't happen* .


According to[recent research by Omdia](https://omdia.tech.informa.com/om138097/market-landscape-why-90-of-enterprise-ai-projects-will-fail) , Security is a key reason why 90% of AI projects fail. That's an enormous waste of developer time and potential innovation of revenue, in part because businesses can't trust the system. When you sideline good ideas because your fragile systems are too risky to touch, you're handing your market to the competition. When projects don't ship, take a moment to note whether that might apply.


## Input 5: Replatforming


Some opportunities never even become projects because authorization makes them technically impossible.[Turo wanted to capture a growing market with a co-hosting service](https://authzed.com/customers/turo) , but their authorization service didn't offer the granularity to handle the job. They were faced with a choice: rebuild from the ground up, cede the market to others, or find an alternative.


Your home-grown authorization system was built at a point in time for a specific need. Those needs evolve, whether it's extra scale because of business success, extra granularity for compliance purposes, or completely different authorization paradigms for agentic systems. Eventually, maintaining and upgrading the systems you built will be more expensive than throwing them out and starting again. When this happens, expect expanded impact on all of the costs above. This is frequently the point where we hear from customers who don't want to keep repeating the cycle.


## Two paths to reducing the tax


The first step to breaking that cycle is moving out of the business of developing authorization software. Here are two approaches we've seen work.


### Path 1: Build on open source


There's no point building an authorization engine when there's already a proven, battle-tested system available for free.


[SpiceDB](https://authzed.com/spicedb) is the open-source engine at the core of AuthZed's products, and it's used by thousands of companies, including[Netflix](https://authzed.com/customers/netflix) and[Reddit](https://authzed.com/spicedb/showcase/reddit) . Swapping SpiceDB for custom code can cut your first-year investment by 50 percent, and we've seen customers compress their timelines from months to weeks, as a result.


An OSS engine also reduces maintenance costs and eliminates replatforming, since bug fixes and new features are no longer the company's responsibility. Operational management (monitoring, scaling, upgrades, on-call, etc.) is still up to the organization, as is the architectural choices about how to optimize the engine to run your business, but it's still a huge step up from the waste and risk of DIY. This path can be a fit for teams with strong infrastructure expertise and established support teams with extremely specific deployment requirements.


### Path 2: Consume authorization as a managed infrastructure service


To remove the Authorization Tax completely, businesses can choose a fully managed platform in which infrastructure management, high availability, disaster recovery, upgrades, bug fixes, security patches, scaling, and other hidden costs are all handled. At that point, authorization becomes a utility—infrastructure like compute or storage to be provisioned and utilized, not built and managed.


"For over two years, Turo has used AuthZed's \[Dedicated\] offering where they're responsible for deploying and maintaining all the infrastructure required by the SpiceDB clusters. And that gives us time back to focus on growing our business, which is our primary concern."


—[Andre Sanchez, Turo](https://authzed.com/blog/turos-spicedb-success-story-how-the-leading-car-sharing-platform-transformed-authorization)


In our case, AuthZed's managed services (AuthZed Cloud and AuthZed Dedicated) provide more than just SpiceDB and operational relief. They offer enterprise support, white-glove onboarding and consulting services, and access to commercial capabilities not available elsewhere, including AuthZed Materialize—used by OpenAI to optimize authorization for 37 billion+ documents and 5 million business users.


## Stop paying more than you have to


The Authorization Tax is real, measurable, and usually far more expensive than you think. Whether you reduce it by building on a proven open-source foundation or by offloading it entirely to a managed service, the first step is the same: quantify what you're already paying for authorization.


On this page


- Input 1: Initial development costs
- Input 2: Maintenance costs
- Input 3: Productivity loss
- Input 4: Opportunity cost
- Input 5: Replatforming
- Two paths to reducing the tax
- Path 1: Build on open source
- Path 2: Consume authorization as a managed infrastructure service
- Stop paying more than you have to


## Related


[Company Our SOC2 Renewal and Reflections on Compliance I'm happy to announce that AuthZed recently renewed our SOC2 compliance and our SOC2 Type 2 and SOC3 reports are now available on security.authzed.com. Having just endured the audit process again, I figured it would be a good time to reflect on my personal feelings toward compliance and how my opinion has evolved. Jan 7, 2025 · 5 min](https://authzed.com/blog/2024-soc2-reflection)[Company Our SOC2 Renewal and Reflections on Compliance I'm happy to announce that AuthZed recently renewed our SOC2 compliance and our SOC2 Type 2 and SOC3 reports are now available on security.authzed.com. Having just endured the audit process again, I figured it would be a good time to reflect on my personal feelings toward compliance and how my opinion has evolved. Jimmy Zelinskie · Jan 7, 2025 · 5 min](https://authzed.com/blog/2024-soc2-reflection)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)
