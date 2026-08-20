---
schema_version: "1.0.0"
document_id: "49d0d94260ec6ab2163e2552a50408617cced28fb2fceb5b7dafea8ab7772bb0"
company_key: "mcdonald-s-corporation-common-stock"
company: "McDonald's Corporation"
source_id: "mcdonald-s-corporation-common-stock-rss-e3f7e88d5cc9"
canonical_url: "https://medium.com/mcdonalds-technical-blog/what-happened-when-we-treated-ai-like-an-engineering-teammate-4745e9a54a59"
published_at: "2026-04-28T13:23:51+00:00"
first_seen_at: "2026-07-22T17:27:13.648832+00:00"
fetched_at: "2026-08-20T02:26:39.473331+00:00"
content_hash: "sha256:d53e98fa831991335cb4d3b198f656ff8fb5293a4d4d97c18f3479c278d275ec"
---

# What Happened When We Treated AI Like an Engineering Teammate

How an AI engineering assistant boosted sprint velocity by 20% and modernized services up to 10x faster.


*by: Sumedha Shenoy, Director, Engineering Tech Lead, and Kamal Jackson, Sr Manager, Engineering Tech Lead*


Quick Bytes:


- A growing backlog of essential but repetitive “glue work” was slowing the Restaurant Topology Management (RTM) team’s ability to modernize and deliver new features
- By treating an AI engineering assistant as an autonomous teammate — not a chatbot — the team offloaded end‑to‑end maintenance tasks at scale
- The result was 20% faster sprint velocity, security updates completed twice as fast, and modernization work delivered up to 10× quicker without sacrificing quality


In fast‑moving software environments, critical but repetitive “glue work” like configuration updates, security patches, and documentation often compete with time that could be spent delivering new features. For the Restaurant Topology Management (RTM) team — who build and maintain the cloud‑based platform that enables markets to create and deploy consistent restaurant device configurations — this maintenance workload had grown quietly over time and began to limit the team’s ability to innovate. To regain momentum and address mounting operational debt, the team turned to an AI‑powered, agent-based engineering assistant embedded into our standard development workflows, capable of handling complex, repetitive tasks.


**The challenge: The “un-fun” backlog** Before piloting the AI assistant, the RTM team was managing maintenance tasks that had persisted across multiple sprints:


- **Security updates:** Enhancing automated scanning workflows to maintain a strong security posture
- **Framework modernization:** Updating services to align with the team’s current application framework
- **Test coverage improvements:** Strengthening unit test consistency across services
- **Documentation updates:** Refreshing internal service documentation to support smoother onboarding and troubleshooting


This maintenance work was important but time‑consuming. It continually diverted attention away from delivering new capabilities.


**The pilot: A new way to scale engineering capacity** When the RTM Development team was asked to participate in an AI engineering pilot, senior developers William Kolluri and Terrel Dodd identified how it could serve as the perfect solution. Instead of positioning the tool as a chatbot, they treated it as an autonomous teammate capable of handling heavy-duty, end‑to‑end tasks.


This perspective shift opened the door to offloading work that had historically bottlenecked modernization efforts to empower developers to focus on higher-value work.


**The results: Putting the AI assistant to work** The team granted the AI assistant access to repositories, assigned tickets, and had senior engineers review its completed work.


Granting the AI assistant access to the code repositories enabled it to fully understand the overall architecture, naming conventions, and interdependencies that are not apparent from isolated code snippets. This context empowered the AI Assistant to perform a complex, multi-step task efficiently. Providing this level of access was crucial in accelerating the completion of the task.


Impact followed quickly across multiple engineering areas:


**1. Accelerating security updates through automated scanning integration** Integrating an automated security‑scanning solution involved modifying complex YAML files across the team’s source control and continuous‑integration environment.


- **Impact:** The AI assistant analyzed the existing pipelines, read the documentation, and generated the necessary configuration changes.
- **Result:** The integration was completed in **one week** instead of the estimated **two** , a **50%** reduction in effort.


**2. Reducing tech debt through framework modernizing** Upgrading frameworks are notoriously brittle because things often break unexpectedly.


- **Impact:** The AI assistant was tasked with migrating services to the team’s modern application framework.
- **Result:** A **single** developer completed the task across all ten services in a **single** sprint. Previously, the team could only complete one or two services per sprint to ensure stability.


**3. Strengthening quality through expanded test coverage** Improving unit test coverage is essential for code quality, but it is often resource intensive.


- **Impact:** The AI assistant identified gaps in unit tests and wrote new cases for uncovered edge cases.
- **Result:** Test coverage **increased to over 90%** across more than ten RTM services, completed by a single developer in one sprint. In the past, this would have required three developers working across three sprints.


**4. Streamlining documentation updates** This was an unexpected win for the team, providing context that was previously unavailable.


- **Impact:** The AI assistant generated high-quality markdown documentation for the internal wiki.
- **Result:** The process was reduced from an estimated 1–4 days per microservice **to just minutes.**


**The metric that mattered** The impact of the pilot was immediate and measurable. By offloading maintenance “toil” to the AI assistant, the team’s sprint velocity increased by roughly 20%. This increase represents actual features shipped to customers that would have otherwise been displaced by maintenance work.


**Empowering the engineer** The AI-powered engineering assistant made our engineers more effective. By taking on the “glue work” that had built up across sprints, the assistant gave the team the space to refocus on meaningful engineering problems and feature delivery.


While not every task will see the same level of improvement, the pilot showed that the right automation can be a strategic asset for high‑impact, repetitive work. By delegating the glue, the team’s focus shifted back to what matters most: building.


---


[What Happened When We Treated AI Like an Engineering Teammate](https://medium.com/mcdonalds-technical-blog/what-happened-when-we-treated-ai-like-an-engineering-teammate-4745e9a54a59) was originally published in[McDonald’s Technical Blog](https://medium.com/mcdonalds-technical-blog) on Medium, where people are continuing the conversation by highlighting and responding to this story.
