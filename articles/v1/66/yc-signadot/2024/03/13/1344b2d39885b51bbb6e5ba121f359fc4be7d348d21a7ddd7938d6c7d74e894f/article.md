---
schema_version: "1.0.0"
document_id: "1344b2d39885b51bbb6e5ba121f359fc4be7d348d21a7ddd7938d6c7d74e894f"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/how-to-be-an-effective-platform-engineering-team/"
published_at: "2024-03-03T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:26:14.489943+00:00"
content_hash: "sha256:a1b8f5454801f51bcfa2c02c39c7ad4ae78c7f63d81454b25cfbb6a438dcac55"
---

# How to Be an Effective Platform Engineering Team

By: Nočnica Mellifera


Platform engineering is a specialized discipline that focuses on creating scalable, reliable and efficient building platforms for developers. Unlike DevOps, which is more about the deployment and operation of applications, platform engineering is about building the underlying infrastructure and tools that developers use. Platform engineers aren’t focused on delivering product improvements, rather they’re the[Hephaestus](https://en.wikipedia.org/wiki/Hephaestus) of the pantheon: making the tools that others need to be the best.


This piece won’t go into depth about why you need a[platform engineering team](https://www.signadot.com/blog/streamlining-microservices-testing-with-platform-engineering/) , or why a developer platform is a good idea, rather I want to go over some lessons learned from others’ journey with platform engineering, the top challenges a platform engineering team will face and some strategies to handle them. Those challenges include:


## Adoption vs. Completeness


### Problem: Our devs can’t wait a year


One of the most pressing challenges for a platform engineering team is finding the right balance between building a comprehensive, feature-rich platform and getting it into the hands of users as quickly as possible. A platform that is too basic may not meet the needs of developers, while one that is overly complex can take too long to build and become a hindrance to early adoption. The key is to prioritize features that offer the most value to the largest number of users and to roll them out in a way that encourages early, yet meaningful, engagement with the platform.


### Solution: Focus on incremental value


Instead of aiming for a fully-featured platform right out of the gate, concentrate on delivering incremental value to your users. Identify the most pressing use cases that your platform can address and build features that solve those specific problems. This approach not only speeds up the development cycle but also encourages early adoption. Users are more likely to engage with a platform that solves their immediate needs, even if it’s not yet feature-complete.


The key challenge here is to start by listening to developers and solving their most pressing needs. A few years ago I was working with a team that, because of some cluster DNS issues, required you to manually copy and paste three different local IP addresses to run the debugger. It was arduous and there wasn’t an easy scripted workaround. The developer platform that we worked on later that same year solved this problem entirely, and the debugger could be activated with a button in the IDE. This pain point was so significant that everyone adopted the tool right away. Even if other components are ramshackle, a basic platform that solves the top two or three problems a team faces will be adopted by all.


## Diverse Backgrounds in Operations


### Problem: Not every dev is a ’Nix nerd


A platform may be used by both novice developers who need a lot of guidance and expert developers who require advanced features and the ability to customize. This diversity necessitates a flexible, adaptable platform design that can cater to different skill levels without becoming either too simplistic or too complicated.


As one basic example, I and others who got started writing code entirely with terminal tools will feel very comfortable if our developer tooling is in the form of a sophisticated CLI. We’re used to paging through command histories, piping outputs to other tools and scanning logs from a terminal. Other developers might use the command line to start services, commit via git and little else. We want to give all engineers power tools without feeling like some of them are being left out.


The challenge lies in creating a platform that is intuitive for beginners but still offers the depth and flexibility that experienced developers expect.


### Solution: Two-layer design


To cater to a diverse user base, consider implementing a two-layer API design. The foundational layer should provide the raw functionality needed for complex use cases, giving experienced developers the flexibility they seek. In the CLI example above, it might make sense to build a web GUI for the most basic tasks like a direct deployment, while enabling more complete config via the command line. Your budget may not allow for this kind of complete dual-layered approach, so consider setting default config values and distributing complete scripts for engineers who are less familiar with operations tools.


## Vendor Lock-In


### Problem: I like this vendor; I don’t “LIKE them like them”


Platform engineering teams often rely on third-party services for certain functionalities, such as package management, source control automation and user account controls. While these services can accelerate development, they also pose the risk of vendor lock-in. This dependency can make it difficult to switch providers or adopt new technologies, limiting the platform’s future adaptability. Worse, if you’re using a closed-source SaaS tool for your developer platform, it’s reasonable to be concerned that you won’t be able to do much if that vendor wants to increase your rates.


### Solution: Vendor abstraction or open source


Admittedly, vendor lock-in is a significant concern, especially when an existing tool solves most of your problems with little engineering work on your part. Note the focus on adoption above, a good solution that’s available now is a lot better than a great one that will roll out in six months. Two possible solutions:


- Abstraction: To mitigate the risks of vendor lock-in, you can employ abstraction layers or wrappers around third-party services. This allows you to switch vendors with minimal impact on the platform or its users. For example, if you’re using a specific cloud provider’s storage service, create an internal API that interacts with that service. If you ever need to switch providers, you only have to update this internal API, rather than making changes throughout your entire codebase.
- Open source: Developer platform[Backstage](https://backstage.io/) solves so many problems, in such a complete way, that once your teams adopt it, you’ll have a lot of trouble migrating off the platform. The good news is that Backstage is open source, so you won’t find your SaaS bill for the platform mysteriously rising by 20% every quarter until it dwarfs your infrastructure costs. Adopting an open source tool as your developer platform helps ensure that time spent on adoption is a good investment.


## Measuring Success


### Problem: It’s hard to prove the benefits of platform engineering


Determining the success of a platform is not straightforward. It can be very difficult to generalize about developer velocity from a single sprint or quarter; after all, it may just be that the challenges faced in that time window were either exceptionally difficult or unusually easy and that explains changes in performance. Improvements in the developer platform can be even more abstruse to measure: With the time taken to adopt a new platform and train your teams, there’s often not a bright line showing “after we adopted this platform, here’s how everything changed.”


Traditional metrics like uptime or latency are important but don’t provide a complete picture. The challenge is to identify the right set of KPIs and to use them to guide ongoing improvements to the platform.


### Solution: Consider DORA


While uptime or latency won’t show you the effectiveness of platform engineering, and certainly won’t show any signal right away, the team at[Google Cloud has suggestions](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) for better metrics to measure how easy it is for your developers to write, test and ship code. DORA metrics come with their own requirements for implementation, but if you need measurable results, it’s work worth doing. See the next section for more detail on measuring success.


## How Can PE Teams Measure Success? DORA metrics in practice


While I recently published[an entire piece on this subject](https://www.signadot.com/blog/how-to-do-dora-metrics-right/) , a brief summary of how you use Dora metrics is to answer four questions about your engineering team:


1. How often do you deploy new code?
2. How long does it take to go from “passing unit tests” to “deployed on production”?
3. Once code is deployed, what’s the likelihood it’ll have to be rolled back?
4. If there’s a bad deploy, failure or other problem, how long does it take to resolve the incident?


There are some specific recommendations about how to measure these values, and these metrics may not currently be available from your source control or observability tools, but looking at these four questions I think it’s easy to see how they can give you a sense of how well we’re[enabling developers to do their job and ship production](https://www.signadot.com/blog/how-to-improve-and-measure-devex-in-your-organization/) features while maintaining reliability.


## A Few Real-World Case Studies


### Stitch Fix


In this article on Medium,[Stefan Krawczyk](https://towardsdatascience.com/what-i-learned-building-platforms-at-stitch-fix-fc5e0ec72c86) describes how his team created a platform for data scientists without them having to “hand off” their models. This philosophical shift toward a team whose sole focus was improving the developer experience was fundamental and changed their entire approach.


At a high level, the platform team operated without product managers and had to come up with platform capabilities to move data scientists forward, who in turn moved the Stitch Fix business forward.


There are a number of lessons in Stefan’s write-up that I’ve used for this article, including the fantastic “focus on adoption, not completeness” when deciding when and how to release tools to the team.


### Uber


In a writeup from 2021,[Gergerly Orosz](https://newsletter.pragmaticengineer.com/p/program-platform-split-uber) wrote about how Uber created a platform engineering team that differed greatly from existing teams, in the main because it didn’t work on a project basis but instead handled ongoing concerns about technical debt and developer enablement. This work presented real challenges in making a business justification, after all, these were engineers specifically not tasked with shipping features. But with careful observation the benefits were plain:


Platform teams improve organizational efficiency. They reduce duplication of work and help with the standardization of approaches, when standardization is a benefit for the organization. For example, setting up a[backend security platform](https://thenewstack.io/securely-scaling-the-myriad-apis-in-real-world-backend-platforms/) team can help with standardizing security vulnerability checks across the company.


Platform teams faced specific challenges at Uber. One that I found fascinating was seniority over-saturation: Since they rarely had to deal with tight deadlines or business stakeholders, senior engineers gravitated toward platform engineering, unbalancing the organization. The whole piece is a fascinating read and talks about how Uber navigated a shift from a pure startup culture to one that needs to protect its enormous technical and business achievements.


### Netflix


[Mike McGarr](https://www.infoq.com/news/2018/08/better-devex-at-netflix/) , manager of developer productivity at Netflix, gave a talk for QCon on how the company moved to polyglot development by creating a developer platform. With containerization this allowed the team at Netflix to move away from a Java shop to one that met engineers where they were and allowed teams to work in the best language for their challenges. Mike has some insights from the very beginning of platform engineering. There’s a great list of lessons learned:


Netflix has learned:


- Polyglot can be expensive
- Containers make for great tool distribution
- Build platforms, not just tools
- Provide native (or native-like) solutions
- Reduce cognitive load


The result is a team that is the model for so many other large engineering organizations and one that faced and overcame the challenges of platform engineering years ago.


## Conclusion


Effective platform engineering is often called a “startup within engineering,” and the observation remains true as we study success stories. Successful PE teams found unfulfilled needs within their larger organization, solved those problems with a product that did one thing very well, and had enough appeal for wide adoption. The result of successful platform engineering is a more connected team that deals with less stress when trying to get its work to market.


Engineers are better supported and can do more of their best work without the arbitrary heavy lifting that is solved with platform tools.


Challenges like lock-in with vendor tools and documenting the benefits are in your future as you pursue platform engineering. If you’d like to join a community of like-minded engineers who are focused on enabling developers, check out the[Signadot Slack](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) to continue the conversation.
