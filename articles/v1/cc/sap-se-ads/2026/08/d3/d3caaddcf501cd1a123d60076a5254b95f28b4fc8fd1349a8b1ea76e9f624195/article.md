---
schema_version: "1.0.0"
document_id: "d3caaddcf501cd1a123d60076a5254b95f28b4fc8fd1349a8b1ea76e9f624195"
company_key: "sap-se-ads"
company: "SAP SE"
source_id: "sap-se-ads-rss-5a5b87a458d9"
canonical_url: "https://news.sap.com/2026/08/with-agentic-ai-abap-takes-evolution-to-the-next-level/"
published_at: "2026-08-18T12:15:00+00:00"
first_seen_at: "2026-08-18T12:51:32.796520+00:00"
fetched_at: "2026-08-18T12:51:34.238369+00:00"
content_hash: "sha256:6d223b293fe014295c5cb292accef00210061e9f0d18007384e62cc7c55fa142"
---

# With Agentic AI, ABAP Takes Evolution to the Next Level

AI agents are writing code and translating legacy applications for use in the SAP cloud. Sonja Liénard, head of ABAP platform at SAP, explains what this means for the ABAP programming language and ABAP platform.


In an interview, Liénard talks about the evolution of SAP’s proprietary programming language and ABAP platform, and discusses what the innovations announced at[SAP Sapphire](https://news.sap.com/topics/sap-sapphire/) will mean for customers.


##


Sonja Liénard is an information scientist and business information specialist who joined SAP in 2012. As senior vice president and head of ABAP platform at SAP, she is responsible for ABAP and all matters related to ABAP platform. In this role, she is also the head of ABAP AI and thus globally responsible for the latest developments and innovations in this domain.


## **Q: In our[first conversation](https://news.sap.com/2026/05/agentic-ai-will-change-the-market/) , you explained what ABAP is, what the future looks like, and why agentic AI will change the market. Let’s start from there and then dive deeper. How does ABAP interact with other development technologies in today’s SAP landscapes?**


**Sonja Liénard:** ABAP has been around for over 40 years and has been evolving ever since. With the era of agentic AI, we are now reaching the next stage of that evolution. The modern version of the development model is ABAP Cloud, which has been developed from the outset according to the guiding principles of openness and comprehensive support for the business logic.


We have robust, well-defined APIs that are interfaces to the outside world and offer a full environment with Open Data Protocol (OData) services, modern front ends, and an integration layer. This enables the development of modern web applications. This is in addition to SAP Business Technology Platform (SAP BTP) extensions and an external system like SAP Integration Suite, which can also be integrated.


The ABAP AI strategy empowers developers to add AI capabilities to their custom applications and extensions


[Learn more](https://www.sap.com/documents/2024/11/6c0a3705-e77e-0010-bca6-c68f7e60039b.html)


We currently use Eclipse as a development environment. Starting in Q2 2026, ABAP development tools will also be possible in Visual Studio Code, which is a milestone. The community was quite vocal in requesting this and it was an open door for us. Visual Studio Code is gaining ground and is currently the preferred IDE (integrated development environment) for many developers. Many AI extensions on the market being optimized for it first.


We have redesigned the architecture and are pursuing an open IDE strategy where we can also offer further IDEs for ABAP development. By opening it up, we now have a rich third-party ecosystem that we can leverage for AI, such as GitHub Copilot and Amazon Q.


In addition, we have published an ABAP MCP (Model Context Protocol) server that lets ABAP developers benefit from the entire AI tooling and ecosystem. This is a major, important step and an impressive example of interoperability. Furthermore, we can still rely on the strength of ABAP business logic. With this approach, we are combining the open IDE strategy with a high rate of innovation.


## **You mentioned the ABAP MCP server. What is that exactly?**


****Liénard** :** MCP stands for Model Context Protocol and is an open standard for AI agents to interact with external tools and systems in a structured manner. It is a universal language that AI agents use to ask questions, trigger actions, and retrieve results.


The ABAP MCP server provides ABAP development capabilities based on this protocol. Any agent that supports MCP can interact with ABAP code and our ABAP systems in an intelligent, agent-driven approach. It is a new channel through which we can make our ABAP-specific capabilities available to the outside world.


The ABAP MCP server is an important building block for everything related to agentic AI and an important technical foundation for our new Custom Code Assistant for SAP S/4HANA transformation. The ABAP MCP server for Eclipse is available as of Q2 2026.


## **What are the risks of using AI in SAP enterprise systems and how do you address them?**


**Liénard:** The entire AI market is incredibly dynamic, fast-paced, and characterized by different interests. As such, we need to carefully examine which solutions are durable, robust, and trustworthy enough to run the world’s business processes. We want our solutions to remain secure, compliant, and true to everything SAP stands for.


We must put our core mission at the heart of all decisions. ABAP platform is known for its ability to run large enterprise business. Our customers and partners trust this capability The AI solutions on the market do not qualify for this through their ability to build short-lived solutions, but instead by supporting our core capabilities. That’s why it helps to take a step back, look at the big picture, and make sustainable decisions, but also to remain open to revising past decisions in response to major shifts in the market.


The second point is the accuracy of the code. AI can generate seemingly plausible code that contains subtle errors. To counter this, we rely on a combination of human review and thorough agent testing, keeping humans in the loop at every critical step. AI agents handle the quality checks and validation, with developers making the judgement calls.


Another risk is the loss of business logic during the transformation. We want to help our customers migrate their legacy applications to modern solutions. We are developing custom code management agents for this purpose. The first was released in June. It is crucial for our customers and partners to retain their business logic during the transformation.


Lastly, security and data protection are central topics. AI models always need context to be effective. In the enterprise environment, this context can contain sensitive business data. That’s why we are taking a very careful approach: ABAP AI services only operate within established SAP compliance and trust frameworks, for example. Customers always have control over what is shared and what isn’t.


## **Let’s talk about SAP Sapphire** in 2026 **. What innovations from your area were presented?**


**Liénard:** SAP Sapphire is also a very important conference for[ABAP AI](https://www.sap.com/documents/2024/11/6c0a3705-e77e-0010-bca6-c68f7e60039b.html) . In Q2 2026, we published the first release of the ABAP development tools for Visual Studio Code, initially in the ABAP Cloud scope including SAP Fiori app development and with integration of GitHub Copilot and Amazon Q as AI solutions. Another goal is to support classic ABAP development in Visual Studio Code; additional ABAP object types will follow throughout the year. Also, as of Q2 2026, the ABAP MCP server for Eclipse and Visual Studio Code are generally available to connect third-party solutions, particularly AI-specific third-party tools, to the ABAP system.


We also delivered the first Custom Code Assistant in Q2 2026. The feedback is promising. This enables us to automate and accelerate code migration significantly. In this approach, all communication between legacy migration tools and new agents will take place through an engagement layer. Our overall migration strategy, which we presented at SAP Sapphire, comprises seven different agent families across all phases of a migration project, from planning to execution.


## **Smaller customers and those with older system versions often found it difficult to access AI tools. What is changing here?**


**Liénard:** The barriers to entry are getting much lower. We are switching from users-per-month billing to a usage-based model. Billing will be according to actual usage, based on “AI units” with individual prices. This will help make it easier to use our AI solutions. Customers will be able to better plan how much they want to spend on AI solutions.


In addition, as of Q2 2026, we introduced a side-by-side service that enables the use of all AI solutions, regardless of release. The availability of ABAP AI will be expanded to include SAP S/4HANA Cloud Private Edition for all releases from 2021 and later. This enables us to support most of our customers with AI capabilities, even those that have older releases.


## **How do ABAP and AI agents fit into SAP’s longer-term product vision?**


**Liénard:** We are following the overall SAP strategy, of course. It was apparent at SAP Sapphire that agentic AI is the new theme. In the past, we had Infrastructure as a Service, Platform as a Service, and Software as a Service. Agentic AI solutions are now being added.


I like to use three concentric circles to describe our vision: ABAP Cloud is at the center, as the modern, clean-core variant of our language for long-term stability. The second circle is the ABAP AI layer: developer tools for code explanation, code generation, and ghost texting. The outer circle is comprised of AI agents, a network of specialized agents for complex and multi-step tasks such as transformation, migration, and quality validation. They can also take on development tasks, however, significantly reducing the required effort. This can free up time for decisions regarding business logic and architecture, as well as for verifying quality.


The circles coexist and reinforce each other. The fundamental orientation of the platform and the core of the solutions remain stable. Agents and AI skills complement this in the best possible way.


## **What milestones should customers and partners pay attention to?**


**Liénard:** There are four main milestones. The development environment for Visual Studio Code and the MCP server were released in Q2 2026. This enables the development of AI agents in an open IDE ecosystem. We released the first agent for the custom code migration strategy in Q2 2026, along with the extension of ABAP AI to include SAP S/4HANA Cloud Private Edition release 2021 and later. A second agent for clean core transformation is planned for Q3 2026. And we will continue to develop multi-agent orchestration and add more agents to the portfolio in the course of 2026.


## **Do you have any other takeaways to share?**


**Liénard:** Development should be seen as an opportunity. It’s all about development and collaboration. ABAP and developers remain a strong team. AI will not change anything here. ABAP has a long track record of successful reinvention. AI is just the next chapter, taking over routine tasks and creating freedom for what creates value: knowing your business, making architectural decisions, providing high quality, and meeting security and compliance requirements.


Customers and developers can trust SAP’s AI to be implemented reliably: with a clear road map and high quality, security, and compliance standards that are critical for enterprise systems. We work closely with the community and our customers and partners. That is a key success factor.


---


Subscribe to the SAP News Center for the latest SAP news each week


[Sign up today](https://www.sap.com/campaigns/nl/sap-news-center-newsletter)
