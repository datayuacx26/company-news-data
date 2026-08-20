---
schema_version: "1.0.0"
document_id: "ca7d2707d6e1661148cb4b329993501ad83de3b782e441d996611db683174162"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/are-we-approaching-infrastructure-as-code-the-wrong-way"
published_at: "2024-10-09T00:00:00+00:00"
first_seen_at: "2026-07-27T21:32:22.131944+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:b5bd1e066ac864b75958e731a443817877c6f647c873402eb8c3d7fa974adbb2"
---

# Are We Approaching Infrastructure as Code the Wrong Way?

Let me start with a bold perfectly reasonable claim: we **are** approaching Infrastructure as Code (IaC) the wrong way.


Yes, this is my Steven P. Jobs "you're holding it wrong" moment.


When it comes to IaC, we often miss the mark. Typically, we fall into one of three approaches:


- We do all the infrastructure work for developers,
- We ask them to write it themselves, or
- We hand them generic modules and say, "Here, run this."


The first two approaches can be problematic, and even if you think you're "not having issues," I encourage you to check in with your team.


The real challenge with IaC isn't about learning tools like Terraform, OpenTofu, or mastering cloud services—though these can be cumbersome. The core difficulty lies in scaling IaC across an organization, managing the resulting knowledge sprawl, and navigating the inevitable cultural shifts.


### Handing the Cloud to Developers—A Misalignment


We've seen this scenario time and again: we hand developers the operational tools and say, “Here you go, figure out which zones you want, how to configure your encryption, or how to optimize network traffic.” But this doesn't align with their core responsibilities.


Let's be real—developers shouldn't have to care about these operational details.


They sure as shit don't when they're using their favorite platform as a service. But now? We've ripped that simplicity from their hands and replaced it with a smaller team and them owning some of the operational burden. Two ingredients for "efficiency," right? This shift may save money on paper, but it comes at the cost of developer productivity and focus. We're asking them to take on tasks that don't align with what they were hired to do—build features and solve business problems.


When we ask developers to configure every aspect of their infrastructure, it's like asking them to become experts in both software development *and* operations. This is a huge cognitive load and distracts from their main focus.


Our role, as ops/platform engineers, should be to reduce this complexity for them, not add to it.


### The Case for Convention Over Configuration


This brings me to the concept of *convention over configuration* —a principle popularized by software frameworks like Ruby on Rails. It's the idea that systems should come with sensible defaults, and only require explicit configuration when absolutely necessary. It's about creating smart conventions that let developers focus on what's unique about their application, not the scaffolding around it.


In software development, we know this saves time and reduces complexity. But when it comes to IaC, we seem to abandon that philosophy.


**Why is IaC so configuration-heavy?**


We give developers an overwhelming amount of choices: which instance type to use, which availability zones to deploy in, how to configure encryption, what security settings are needed. But we don't need to expose developers to all of these operational details.


If we applied *convention over configuration* to IaC, here's what it might look like:


- **Sensible defaults** : Developers don't need to configure every little detail. They should be provided with smart defaults and battle-tested presets that work out of the box.
- **Abstraction of operational complexities** : Instead of making developers learn every intricacy of cloud services, we abstract away what they don't need to know.
- **Interfaces that match their needs** : Just like in software development, we should provide them with interfaces that fit their world. Think of how services like S3 hide immense complexity behind a simple interface—why can't we do the same for IaC?
- **Faster onboarding and better productivity** : Developers should spend more time writing code and solving business problems, not fiddling with operational settings.


Our role as ops engineers is to create the infrastructure so developers can innovate without being bogged down by cloud-specific jargon or nuances.


### Empowerment Through Simplicity


I'm not saying we should take all control away from developers—far from it. This isn't about restricting their options. It's about empowering them by giving them tools that work efficiently with minimal friction.


In many ways, infrastructure should behave more like a well-designed product, where the focus is on usability and developer experience. You wouldn't build a user-facing product and expect people to configure every underlying component, so why do we expect developers to do that with cloud infrastructure?


The more we can abstract operational concerns and focus on providing meaningful, high-level interfaces, the more we allow developers to concentrate on what they do best: build features, ship products, and solve business problems.


### Designing IaC for Developers


When we start designing IaC with *convention over configuration* in mind, we:


- Build modules that align with developers' needs, not our operational concerns.
- Focus on high-level, developer-friendly abstractions that still maintain operational excellence behind the scenes.
- Create defaults and presets that developers can trust, instead of making them configure everything manually.


If you're managing infrastructure today, it's worth asking: Are you enabling your developers to move quickly? Or are you adding unnecessary complexity by handing them operational tools they shouldn't have to deal with?


Infrastructure shouldn't be a configuration nightmare. It should be intuitive, with conventions that free developers from worrying about the details.


### Ready to make Infrastructure as Code (IaC) work for your team, not against them?


Register for our free 4-hour workshop on designing developer-friendly OpenTofu and Terraform modules. In this workshop, we'll dive deep into how you can stop drowning your developers in operational complexity and start embracing convention over configuration for more streamlined, developer-friendly IaC. Learn how to build interfaces that speak their language, abstract away operational details, and make onboarding a breeze - all while maintaining operational excellence.


Don't miss it on **Nov 26 @ 10a PST!**


[Learn more and register here.](https://www.massdriver.cloud/blogs/convention-over-configuration-building-developer-friendly-iac-with-opentofu)
