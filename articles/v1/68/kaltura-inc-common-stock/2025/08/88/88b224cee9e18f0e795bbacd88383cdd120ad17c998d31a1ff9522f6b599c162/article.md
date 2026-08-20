---
schema_version: "1.0.0"
document_id: "88b224cee9e18f0e795bbacd88383cdd120ad17c998d31a1ff9522f6b599c162"
company_key: "kaltura-inc-common-stock"
company: "Kaltura Inc."
source_id: "kaltura-inc-common-stock-rss-8a80d100aa25"
canonical_url: "https://medium.com/kaltura-tech/the-future-is-now-human-ai-collaborative-coding-best-practices-a00307314b96"
published_at: "2025-08-11T15:53:46+00:00"
first_seen_at: "2026-07-20T23:18:51.911067+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:3198ea0247f119a16455ba926814b9a01182bbceaeecfb19db3e605fc67e0527"
---

# The Future is Now: Human-AI Collaborative Coding Best Practices

# The Future is Now: Human-AI Collaborative Coding Best Practices


[Gabi Brontvein](https://medium.com/@gabibron?source=post_page---byline--a00307314b96---------------------------------------)


5 min read


·


Aug 11, 2025


--


Press enter or click to view image in full size


Just a few years ago, before coding agents entered our lives, software engineering was a well-established domain with clear principles: object-oriented programming, test-driven development, design patterns, and SaaS best practices. Over the last 17 years, my teams and I have been fortunate to be part of this journey, establishing best practices in real-world production environments. We have consistently delivered well-designed, high-quality software.


With the advent of coding agents, it became clear that integrating software engineering principles with coding agents could significantly boost development productivity and create truly exceptional engineering organizations.


In this article, I will share my journey as a senior software manager at Kaltura, detailing the transformation from traditional development to a hybrid human-AI mode of development.


## The Transition to AI-Agent-Guided Development


The transition to working with coding agents was not without challenges. Initial concerns included the quality of generated code and tests, as early versions of LLMs were less advanced than today’s models. The code it generated sometimes was not even compiling, and it would take more time to debug it than to write it from scratch.


As LLMs advanced and coding agents were introduced, a new concern arose regarding the value that human developers would bring if coding agents could perform their tasks. Will the coding agents replace human developers?


To address these concerns, we began integrating coding agents into the team’s IDEs and encouraged experimentation. The AI agents were assigned a clearly defined role: as assistants, not replacements. Developers consult these tools and receive guidance on their queries. Of course, all generated content is thoroughly reviewed by developers, approved by QA, and adhere to compliance procedures. Even with these safeguards in place, this approach has begun saving significant time, especially during heavy development phases.


By sharing results and discussing what worked and what didn’t, we successfully incorporated coding agents into our daily workflow.


Another significant concern was the perceived threat to the value of human developers. Managers play a crucial role in this transition. Instead of resisting, I embraced and encouraged the use of coding agents, openly praising the results shared by team members. Over time, this approach led to a consensus that coding agents are valuable allies, not replacements, enhancing our capabilities rather than diminishing them.


## Enhancing Productivity with AI Agents


Today, developers and AI agents collaborate to develop code at scale in a microservices environment, serving millions of concurrent requests in production.


Developers use the AI agents to analyze complex code flows, debug production bugs, design new features with UML agents, and generate code and tests. These capabilities significantly boost productivity and velocity, allowing us to deliver high-quality code faster. We continue to adhere to software engineering best practices, such as TDD, code reviews, and modular design. Every suggestion is reviewed and final decisions are made by the engineering team, ensuring that even under time constraints, we do not skip essential steps.


## Core Best Practices for Working with AI Agents


To ensure that AI agents produce high-quality, production-ready code, it is essential to follow core best practices that we have learned throughout our journey. These practices integrate traditional software engineering methodologies with the capabilities of AI agents.


## 1. Treat the AI Agent as a Co-Developer


Approach the AI agent as a collaborative partner in the development process. This means engaging with the agent in a manner similar to pair programming. Use inclusive language such as “we” instead of “you.” For example, say, “We are developing a C# module that calculates the subscription cost.” This approach helps the AI agent feel more engaged and integrated into the team.


Additionally, instruct the AI agent to ask questions when something is unclear. This ensures that the generated code is consistent with the requirements and reduces the likelihood of misunderstandings. By treating the AI agent as a co-developer, you foster a more interactive and productive development environment.


## 2. Follow a Structured Development Methodology


Of course, pairing with AI doesn’t mean ditching the process. As with traditional development, don’t start coding immediately after receiving product requirements. Begin by creating a design, reviewing it, and planning your user stories, tests, and deployment strategy. Apply the same structured approach to agentic AI-hybrid coding. For QA, plan tests and verify them against the product requirements to ensure the test plan covers all necessary items.


## 3. Document Your Plans


All plans should be thoroughly documented in dedicated Markdown files. For software design, create a design.md file and ask the AI agent to document it there. For the test plan, create a tests.md file and document all required tests.


## 4. Review and Iterate


Brainstorm the design, explore the pros and cons of each direction, and select the best approach that meets the requirements and adheres to the timeline. This iterative process ensures that the final design is robust and aligned with project goals.


## 5. Create a Development Plan


Once the documents are ready, start planning your development. Create a dev_plan.md file, splitting it into epics and user stories. You can also create the entire plan in Jira by using the Jira MCP connection directly from the agent. Follow agile principles, creating the smallest possible user stories that have customer value (internal or external) and are testable.


## 6. Maintain Consistent Development Progress


To maintain consistent development progress according to your plans, instruct the AI agent to follow strictly the design.md document. This can be incorporated as a system instruction. Additionally, at each major milestone, the agent should document its progress.


For this purpose, create and maintain a progress.md document. Add a system instruction that the agent should review and understand its progress on each new coding iteration. Lastly, instruct the agent that if it is unsure about which direction to choose, it should consult with its pair programmer.


## Humans Succeed When They Leverage AI Best Practices


By following these steps, software developed in collaboration with AI will be state-of-the-art, generated according to your specifications, and deliver high-quality, production-ready code with full engineering team approval.


The future of software development is here, and by adopting these new technologies, software professionals can leverage their skills and productivity to become true Humanoid developers.
