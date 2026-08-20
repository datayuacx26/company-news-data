---
schema_version: "1.0.0"
document_id: "da9c49bcaf7ace841094337e012b1fed06e1bc3c8815e4af72d63401f4a8c449"
company_key: "yc-nullstone"
company: "Nullstone"
source_id: "yc-nullstone-news-import-96ce084d16d8"
canonical_url: "https://www.nullstone.io/blog-posts/phase1-startup"
published_at: "2021-05-03T00:00:00+00:00"
first_seen_at: "2026-07-27T04:02:46.708969+00:00"
fetched_at: "2026-08-10T01:11:39.788792+00:00"
content_hash: "sha256:d745b70327450c1a72240a7bdb8a50330124922ea0456db36ecc580cbad389d3"
---

# Application Infrastructure: Launching a Technology Startup

## What does the Startup Phase Look Like?


You are launching a new startup to revolutionize the world; cash-strapped but overflowing with optimism and passion. Or maybe you're part of a digital transformation at a large organization incubating a new idea to transform an existing enterprise into an agile disruptor. While they appear different, these ventures have much the same challenges for the technology team — turning great ideas into reality through iteration and experimentation.


In a stereotypical startup, the founders are often the engineering team while at a burgeoning transformation, the team is seeded with ~8-10 developers from within the organization. Where should the CTO or Architect focus efforts? Should we build things the "right" way or should we build to survive? Should we use a Platform-as-a-service or use a major cloud provider? Should we invest in Kubernetes while launching microservices or just build from a single monolith? As a founder, consultant, and founding engineer, I have repeatedly faced these questions.


At launch, it appears magical when you turn an idea into reality, even if it's just smoke and mirrors. (Heads up when you reach growth phase, many will expect continued magic.) These early days are marked with uncertainty and it's the job of the business leader to validate a business model. As a technology leader, your job is to support those initiatives through prototypes and MVPs.


### Evaluating Providers


When I was faced with the decision of how to host our infrastructure, I routinely opted for a major cloud provider such as Amazon Web Services (AWS) or Google Cloud (GCP) rather than PaaS. At an enterprise, the budget and team is already in place to use cloud resources. At an early-stage startup, I could not deny the appeal of free credits across the major cloud providers. The unfortunate downside is the enormous learning curve for anybody on my team to launch applications on cloud providers. The catalog of services is enormous, each with special nuances that require an infrastructure expert to safely configure. Once we add in Terraform (or any other IaC), my software engineers needed to learn cloud configurations *and* another language/framework.


Personally, PaaS (e.g. Heroku, Firebase) have always resonated with me; a strong developer experience has an enormous \[impact on developer productivity\](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/developer-velocity-how-software-excellence-fuels-business-performance). However, every time I explore one of these options, I am faced with immense uncertainty. What happens if we need to scale rapidly? What happens if we need a service that one of these providers doesn't offer? I know I will have to migrate to a major cloud provider anyways; I don't want to retool my engineering processes when I switch providers.


### Impact on Architecture


When deciding how to architect our software applications, I have tried two different approaches: a simple, single monolith and microservices using a sophisticated orchestrator and service mesh. Regardless of the architecture or approach, we still ran into major problems. Even in the startup phase, our growing team of engineers started to collide with one another in the codebase. Since we were still making major changes to the product, every pull request seemed to cause conflicts with another pull request. Our team found it onerous and distracting to constantly manage merge conflicts. Many times, a single large change that needed to be released first could hold up other critical features and bug fixes.


We decided to break down our monolith into smaller services through the \[strangler pattern\](https://microservices.io/patterns/refactoring/strangler-application.html). Our team found this pattern to be very effective, but microservices alone did not come free. Each time an engineer created a new microservice, we were faced with the initial setup of infrastructure and secrets wired up to our CI/CD process. Then, we had to reconfigure the delivery pipeline for every new environment. This extra effort when creating services and maintaining existing services created another distraction from our main product. Our problems were pronounced when our team wanted to evolve our engineering processes; retooling was a massive initiative for such a small team. This evolution cost us significant time, distracting us from core product development.


### How Can Nullstone Help


At Nullstone, we took these experiences as guidance to create a better foundation so that startups can select their path, monolith or microservices, and easily adapt as the business grows. To help you get started, Nullstone provides rapid launch of standard patterns on major cloud providers in a secure, compliant manner. With a PaaS-like developer experience, it does not take significant time or expertise to launch applications, datastores, and domains. Every application, datastore, or domain is a single click (or command, if you prefer the terminal). Since every business has particular needs that diverge, we made these Terraform modules swappable. As you need to swap out infrastructure, Nullstone provides simple workflows depending on your comfort level and pace: fast and aggressive with acceptable downtime or slow and safe with no downtime. The most important aspect is that your engineering process remains unchanged — no changes to CI/CD, no changes to developer workflow.


When you have hit product-market fit, you're ready to eject from the startup phase into the growth phase. To learn more, continue reading the next post in this series:


\[Application Infrastructure: Navigating the Growth Phase\](/phase2-growth)
