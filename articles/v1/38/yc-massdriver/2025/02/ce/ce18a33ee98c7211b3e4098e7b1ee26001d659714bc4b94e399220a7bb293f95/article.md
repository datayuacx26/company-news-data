---
schema_version: "1.0.0"
document_id: "ce18a33ee98c7211b3e4098e7b1ee26001d659714bc4b94e399220a7bb293f95"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/the-case-for-abstractions-in-iac"
published_at: "2025-02-21T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:7be0bfcfa5eeb6728622e7b8fad2fa32e057a4ff7e80764c6b211d074cef04cc"
---

# The Case for Abstractions in IaC

If you're trying to scale Infrastructure as Code (IaC) adoption and enable developer self-service without chaos, the right abstractions are key. IaC promised automation, consistency, and self-service infrastructure. But in most organizations, it’s just another source of friction. Developers are expected to wrangle raw cloud APIs, ops teams spend their time reviewing infrastructure PRs, and every deployment turns into a game of 20 questions about networking, security, and resource sizing.


The problem isn’t Infrastructure as Code itself—it’s how we use it. **Most teams rely on overly generic modules that expose too many low-level details** , forcing developers to make decisions they shouldn’t have to. Meanwhile, ops teams either act as gatekeepers or find themselves stuck maintaining fragile, one-off configurations.


There’s a better way.


By building Infrastructure as Code modules that encode operational standards and present developers with the right level of abstraction, teams can eliminate unnecessary complexity, reduce misconfigurations, and move faster. Instead of debating instance sizes and IAM policies, developers should be answering questions that make sense for their applications. Instead of reviewing every infrastructure change, ops should be defining the guardrails that make self-service possible.


This post is about fixing that disconnect—making Infrastructure as Code work the way it was meant to—as a tool to ***build*** platforms.


## Cloud APIs Are a Mixed Bag of Concerns


**Every hyperscaler resource API is a messy mix of functional and operational concerns.** A single API call to provision a database might ask about memory settings and extensions (developer concerns) alongside replication, availability zones, and backup schedules (operational concerns). And when Infrastructure as Code tools expose these APIs directly, that confusion spills over into your teams.


Developers end up wading through infrastructure trivia just to ship a feature. They’re not learning useful domain knowledge—they’re learning the quirks of AWS instance types or the difference between synchronous and asynchronous replication. It’s not that they can’t make these decisions; it’s that they shouldn’t have to. They’re forced to step into the ops world for a handful of settings they’ll forget by the next deployment.


Meanwhile, operations teams watch the same mistakes happen again and again. They’ve already established standards for failover strategies, backup policies, and encryption practices—but those standards live in runbooks or someone’s head, not in the infrastructure code itself. Every infrastructure request turns into a support ticket:


- What instance type should I use?
- Does this database need point-in-time recovery?
- How do I make this highly available?


Ops teams end up buried in review cycles, endlessly explaining the same configurations and cleaning up misconfigured infrastructure that never should’ve made it to production.


This isn’t just inefficient; it's organizational drag. **Cloud complexity doesn’t belong in the developer’s workflow.** Infrastructure as Code modules should separate these concerns, letting developers answer simple, application-focused questions—like how much traffic they expect or which extensions they need—while ops encodes the reliability, scaling, and security standards once and for all.


Without that separation, developers remain distracted, operations stay reactive, and infrastructure provisioning never really feels self-service.


## Why Public Modules Don’t Cut It


Most Infrastructure as Code implementations start with the same pattern: use public, off-the-shelf modules to simplify cloud resource provisioning. It feels efficient at first—why write your own PostgreSQL module when there’s already a public one that provisions RDS?


But these generic modules quickly become a source of friction. **They’re designed to work for any organization, *which means they don’t fit yours.*** They leave critical decisions—like backup schedules, availability zones, and security policies—up to the developer.


This flexibility isn’t empowering; it’s paralyzing. Developers are asked to make decisions about settings that have nothing to do with their application logic. What’s the right IOPS setting for a production database? How should read replicas be configured? These aren’t questions developers should have to answer, especially when your organization already has standards for these configurations.


For operations teams, this pattern is even more frustrating. They end up reviewing every infrastructure PR to ensure security settings are correct, encryption is enabled, and backups are configured according to policy. And when something inevitably breaks because a developer made a best-guess decision, it's the ops who’s called in to clean it up.


Worse, generic modules act as a thin abstraction layer that’s easy to bypass. They don’t enforce standards—they merely suggest them. Developers can tweak settings without realizing the impact, introducing inconsistencies across environments. Over time, this flexibility erodes the very standardization that Infrastructure as Code was supposed to bring.


The solution isn’t to ditch Infrastructure as Code—it’s to move beyond these generic modules. Teams need business-specific modules that embed operational standards directly into the infrastructure code. Instead of exposing raw RDS settings, a PostgreSQL module could default to the organization’s approved encryption policies, backup strategies, and instance types.


With the right abstractions, developers don’t need to worry about these details. They configure the database with familiar inputs like “expected traffic” and “required extensions,” while ops ensures the underlying infrastructure is production-ready every time.


Generic modules might get you started, but they won’t scale. To build infrastructure that’s both reliable and easy to manage, you need modules designed for your business, not any business.


But that requires more than just writing custom modules. It requires rethinking how these modules function—not as simple templates, but as a communication layer between development and operations. Let’s talk about what that interface looks like.


## Infrastructure as Code Modules as the Interface Between Developers and Operations


**Infrastructure as Code modules aren’t just about automating infrastructure—they should serve as the interface between developers and operations.** This interface determines how developers request infrastructure, how operational standards are applied, and how these teams collaborate without constant back-and-forth.


A well-designed interface separates operational concerns from application concerns. Ops teams care about reliability, scalability, and security. Developers care about performance, extensions, and functionality. Cloud APIs mix these concerns, leaving developers to make decisions that ops teams have already standardized. Modules should reverse that dynamic by embedding ops decisions directly into the infrastructure code.


**The self-service module interface should focus on the developer’s world.** Instead of exposing infrastructure details, it should ask questions that map to the application’s behavior:


“How much traffic do you expect?” → Ops can translate this into instance sizes and autoscaling configurations.


“What kind of availability does this service need?” → Ops can apply the right replication and failover strategies.


“What database extensions do you need?” → Ops can ensure compatibility and performance using standardized configurations.


These questions anchor the interface in terms developers understand. No more guessing about IOPS, multi-AZ configurations, or backup windows—those decisions are already defined by ops and encoded in the module.


But **these modules aren’t just provisioning tools—they’re documentation.** Every answer a developer provides gets recorded, along with the logic that determines the final infrastructure configuration. Instead of asking, “Why is this a` db.r6g.xlarge` ?” teams can see exactly what led to that decision: expected traffic, required extensions, performance needs, and the instance sizing logic applied.


This approach changes the relationship between teams:


- Ops teams design the interface by codifying standards for reliability, security, and performance.
- Developers interact with that interface, providing inputs that match their application’s needs.
- The infrastructure itself becomes self-documenting, capturing both the decision-making process and the resulting configuration.


With the right interface in place, ops teams don’t need to approve every infrastructure request. They aren’t gatekeepers anymore because the module itself enforces standards. Instead of slowing things down, they move into a more scalable role: enabling developers to build without constant oversight.


## Shifting Ops from Gatekeeping to Enablement


When Infrastructure as Code modules act as the interface between developers and operations, something fundamental changes: **ops teams no longer have to be the gatekeepers of infrastructure.** Instead, they shift into a new role—enablers, building the tools that empower developers to self-serve without sacrificing standards.


Right now, most ops teams are stuck in approval loops, not because they want to be, but because they can’t trust the process.


- Approving every Infrastructure as Code change. Without a reliable enforcement mechanism, every change needs a second set of eyes to ensure best practices are followed. A successful plan or apply doesn’t mean the configuration is good—it just means it’s syntactically valid.
- Using security and compliance tools that flag issues too late. Most security enforcement happens at deployment or in CI, which means catching misconfigurations after they’ve already been written—leading to rework and delays.
- Answering the same questions over and over. “Which instance type should I use?” “Does this database need multi-AZ?” “What’s the right backup policy?” These answers exist, but they aren’t codified anywhere developers can reference.


This happens because ops doesn’t trust the process—because there is no process. Guardrails only exist in runbooks, security scanners, and PR reviews. Developers either guess or ask, and ops steps in as the final filter before things break.


But when operational standards are built into IaC modules—instead of enforced manually—ops doesn’t need to be in the loop for every infrastructure decision.


- **Standards become defaults.** Backup schedules, encryption policies, and network configurations are automatically applied.
- **Security actually shifts left.** Instead of flagging violations after a change is written, the module simply doesn’t allow non-compliant configurations.
- **Infrastructure becomes self-documenting.** Developers don’t need to ask “why is this a` db.r6g.xlarge` ?”—the module’s inputs and decision logic already explain it.


With this shift, ops can finally escape the review queue and focus on higher-impact work:


- Building better abstractions that handle more use cases without customization.
- Standardizing configurations to simplify operations. When infrastructure follows consistent patterns, it’s easier to manage, troubleshoot, and scale.
- Collaborating with developers on application needs without being a bottleneck.


This approach doesn’t remove ops from the equation—it repositions them where they’re most valuable: defining standards once and letting the modules enforce them everywhere. Developers get more autonomy, ops gets more leverage, and the organization gets more consistent infrastructure.


## The 80/20 Rule for Infrastructure as Code Modules


When scaling Infrastructure as Code adoption and enabling developer self-service, one common challenge is designing the right level of abstraction. Teams often struggle with striking a balance between flexibility and standardization. A frequent objection is:


"We tried abstractions, and they didn’t work."


Most teams say this because **they built the wrong kind of abstraction.** They weren’t solving for use cases, they were solving for AWS service APIs—creating thin wrappers around cloud resources instead of designing meaningful developer-facing interfaces.


If your abstraction is just "a nicer way to provision S3," you’re going to run into problems. That’s too generic to be useful. A good abstraction solves for a specific use case—not just the underlying cloud service. It should enforce operational standards while keeping developers focused on application concerns. **Good abstractions don’t just encode opinionated defaults and organizational standards—they intentionally omit capabilities that don’t belong in the use case.** An application-assets S3 module shouldn’t expose logging-specific attributes like write-once policies or long-term archival rules, just as a logging bucket module shouldn’t include public-read ACL options or image processing configurations. A well-designed abstraction doesn’t just define what’s configurable—it defines what should never be a concern in that specific use case.


And here’s the thing: not every developer should find your abstraction useful. That’s a feature, not a failure.


A well-designed abstraction should cover 80% of use cases. If a developer has a need outside of that, they should fork it. Some forks indicate valid edge cases, while others reveal gaps in the abstraction that may need refinement.


- Forking provides valuable feedback. If multiple teams modify a module in the same way, it's a signal that the abstraction may need adjustments to better support those needs.
- Some forks should be merged upstream to improve the abstraction, while others represent distinct use cases that warrant separate modules.
- Forks can rebase or hard fork. If a deviation is minor, teams can continue pulling upstream updates. If it’s fundamentally different, it becomes a new module.


A successful abstraction doesn’t eliminate forking—it makes it an intentional, manageable part of the process, ensuring flexibility without compromising standardization.


### **Bad Assumption: "It’s Just an S3 Bucket"**


Even when an abstraction is named for a specific use case, developers often assume it can be repurposed just because it provides an AWS resource they recognize.


For example, an "application-assets-bucket" module is designed for storing images or static assets, with lifecycle rules, access controls, and storage settings optimized for that use case. But then a developer sees it and thinks:


"Oh, this makes an S3 bucket—I’ll use it for logs."


No. Wrong. Stop.


Logs have different retention policies, compliance requirements, and access patterns. They shouldn’t be dumped into an application asset bucket just because “it’s all S3.” The problem isn’t that the module is too generic—it’s that developers assume any module that provisions an S3 bucket should work for any S3 use case.


A good abstraction defines separate modules for different needs:


- A log storage module with appropriate retention and access controls.
- A media upload module designed for user-generated content.
- A data lake storage module for long-term analytics.


If a use case doesn’t fit? Developers should fork or build a new module—not force an existing one to do something it wasn’t designed for.


### **Balancing Standardization and Flexibility**


Striking the right balance between standardization and flexibility is what allows Infrastructure as Code to scale effectively. Without standardization, every team builds things differently, leading to operational chaos. Without flexibility, teams are forced into rigid, one-size-fits-all solutions that don’t meet their needs. The goal isn’t to eliminate customization but to guide it in a way that keeps infrastructure manageable while allowing for necessary adaptations.


The goal of abstraction isn’t to cover every edge case—it’s to standardize what should be standardized and leave the rest flexible.


- **80% of teams should be able to use the module as-is.**
- **20% should fork, modify, or build something new.**
- Ops teams shouldn’t try to solve every use case upfront. They should watch how teams modify the modules and refine them over time.


This approach removes the fear of "locking teams in" while still ensuring infrastructure remains consistent and maintainable at scale.


## Conclusion


Infrastructure as Code was meant to bring consistency to cloud infrastructure, but exposing raw APIs and relying on generic modules has made it harder for both developers and operations teams.


The solution isn’t to get rid of abstractions—it’s to build better ones.


- **Stop exposing AWS primitives.** Encode operational standards into modules so developers don’t have to make infrastructure decisions.
- **Treat IaC modules as an interface.** Developers should answer questions that align with their applications, while ops ensures reliability, security, and scalability under the hood.
- **Shift ops from approval to enablement.** Define standards once and let modules enforce them instead of manually reviewing every change.
- **Design for the 80%.** A good abstraction doesn’t cover everything—it covers most things well, while allowing flexibility where it matters.


The cloud’s complexity isn’t going away. The question is whether your team will keep exposing that complexity—or start managing it with the right abstractions.
