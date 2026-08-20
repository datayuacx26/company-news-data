---
schema_version: "1.0.0"
document_id: "a1c8801a5bda965c77c79d12b909c383ab57b3a6887c2337c0b7702046f13ccf"
company_key: "guidewire-software-inc-common-stock"
company: "Guidewire Software Inc."
source_id: "guidewire-software-inc-common-stock-rss-e684718ff8d9"
canonical_url: "https://www.guidewire.com/resources/blog/developers/a-developers-journey-to-guidewire-cloud"
published_at: "2024-01-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:28.205325+00:00"
fetched_at: "2026-07-28T22:26:17.179631+00:00"
content_hash: "sha256:5b159e5a17ceab797a11fda9cd2981f056c884dd597f6440bd2c612f333433c7"
---

# A Developer’s Journey to Guidewire Cloud

- [Home](https://www.guidewire.com/)


- [Resources](https://www.guidewire.com/resources)


[Resources](https://www.guidewire.com/resources)


- [Download Center](https://www.guidewire.com/resources/download-center)
- [Guidewire Conversations](https://www.guidewire.com/resources/guidewire-conversations)
- [Podcasts](https://www.guidewire.com/resources/podcasts)
- [Blog](https://www.guidewire.com/resources/blog)
- [Help and Support](https://www.guidewire.com/resources/help-and-support)
- [Insurance Technology FAQ](https://www.guidewire.com/resources/insurance-technology-faq)


- [Blog](https://www.guidewire.com/resources/blog)


[Blog](https://www.guidewire.com/resources/blog)


- [All Blog Posts](https://www.guidewire.com/resources/blog/all-blog-posts)
- [Best Practices](https://www.guidewire.com/resources/blog/best-practices)
- [Careers](https://www.guidewire.com/resources/blog/careers)
- [Customer Viewpoint](https://www.guidewire.com/resources/blog/customer-viewpoint)
- [Developers](https://www.guidewire.com/resources/blog/developers)
- [General Interest](https://www.guidewire.com/resources/blog/general-interest)
- [Partner Perspective](https://www.guidewire.com/resources/blog/partner-perspective)
- [Technology](https://www.guidewire.com/resources/blog/technology)
- [Trends](https://www.guidewire.com/resources/blog/trends)
- [Industry Trends](https://www.guidewire.com/resources/blog/industry-trends)


- [Developers](https://www.guidewire.com/resources/blog/developers)


- A Developer’s Journey to Guidewire Cloud


*In this post, the first in a series on migrating from a self-managed implementation of Guidewire to the Guidewire Cloud Platform, Rob Kelly shares his perspective on how developers should think about their migration journey, and he offers a tool that self-managed developers can use right now to start that journey.*


## The excitement and challenge of change


*Alien. Finding Nemo. Legally Blonde.*


Three very different movies. And yet, as John Yorke argues in the introduction to *[Into the Woods](https://www.johnyorkestory.com/about/the-book/)* , his book on the structure of archetypal stories, they are more similar than meets the eye. All plunge their characters into a strange new world, he writes, and all involve a quest to find a way out of it, and in every story monsters are vanquished on that quest.


Many self-managed developers are now on a quest to transition to Guidewire Cloud. What is the goal these developers pursue? As with any archetypal story, the goal is a new self. A self that leaves behind long upgrades for fast updates supported by automation. A self that harnesses modern tools and standardized services to efficiently and securely develop quality solutions. A self that is unshackled from the tactical concerns of provisioning and supporting infrastructure and free to collaborate with the business on their shifting strategic priorities to effectively adapt to changes and meet market demands.


But just like the heroes in Yorke’s archetypal stories, developers who set out on this journey will have monsters to face. They will, for example, have to confront technical debt in their implementations, and battle legacy thinking in their architecture and development practices.


As a member of Guidewire’s transition advisory team, I worked with partner and customer development teams as well as with teams in my own organization to help them overcome such obstacles. Prior to this advisory role, I worked in the trenches of transition projects. I’ve learned a lot over the years about what development teams can do to reduce the cost and complexity of their transition journey, and I’ll share some of those lessons in this and future posts.


## The most important tool you can use right now


For now, I offer what I consider the most important tool that self-managed developers can use right now to start their transition journey: the[SurePath Plugin for Guidewire Studio](https://www.google.com/url?q=https://marketplace.guidewire.com/s/product/surepath-plugin/01t3n00000Sgx5tAAB&sa=D&source=docs&ust=1706558174807131&usg=AOvVaw3wW6priRgVIhIBWK3nRAZi) (login required). The SurePath Plugin enhances the developer experience in Guidewire Studio by adding source code inspections to detect and potentially correct problems in code and configuration changes. The most important inspections available with the plugin are those in the Cloud Assurance profile, because these inspections underpin the automated assurance process on Guidewire Cloud.


Automated assurance provides real-time feedback on standards violations on Guidewire Cloud. Nevertheless, self-managed developers should, where possible, address inspection findings detected by the Cloud Assurance profile in their configurations before the transition project begins. Fewer assurance findings at transition time means fewer cloud optimizations to address during the transition project, which in turn means faster time to production on Guidewire Cloud.


## Applying Guidewire Cloud Standards as a best practice


Addressing Cloud Assurance inspection findings before transition time brings a second and equally important benefit to self-managed developers: it presents opportunities to learn and apply[Guidewire Cloud Standards](https://docs.guidewire.com/cloud/standards/latest/Overview/overview.html?_gl=1*8xkqiu*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjI4Nzg0Ny4xNjEuMS4xNzM2MjkwMjE5LjE2LjAuMA..) (login required). Guidewire Cloud Standards are proven principles and practices that ensure quality, predictability, and security for Guidewire Cloud projects. For self-managed customers, these standards are best practices, so developers working on self-managed configurations should look to put Guidewire Cloud Standards into practice on their projects.


Not all standards apply to self-managed configurations, but applying the standards that do will help self-managed development teams to confidently confront technical debt and legacy practices and architectures. And while use of the SurePath Plugin helps self-managed developers to apply Guidewire Cloud Standards, thus conferring key knowledge to help them safely overcome some of the inevitable obstacles they’ll face as they journey to their new home on Guidewire Cloud, the plugin doesn’t cover every standard that self-managed developers can apply. One of those standards is GW-MTH-1084, “[Branching Strategy for Guidewire Implementation Projects](https://docs.guidewire.com/cloud/standards/latest/Methodology/Standard-GW-MTH-1084-BranchingStrategyforGuidewireImplementationProjects.html?_gl=1*172f5na*_gcl_au*ODg2NjU5NjIwLjE3MzMxNTYwMTI.*_ga*MTMxNjc5NjMxNi4xNzIyNjIxMTAz*_ga_LN5WM89V7V*MTczNjI4Nzg0Ny4xNjEuMS4xNzM2MjkwMjcyLjYwLjAuMA..) ” (login required).


More on that next time.


## Stay in the Know


Get updates for Guidewire developers delivered right to your inbox.[Subscribe!](https://www.guidewire.com/developers/developer-resources/developer-newsletter)


[See More Articles](https://www.guidewire.com/resources/blog)


[Subscribe to Our Blog](https://www.guidewire.com/resources/blog/subscribe-to-our-blog)
