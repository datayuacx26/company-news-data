---
schema_version: "1.0.0"
document_id: "bc820c5fc7589ea9a0ef2f2a699778fc6ac219e4b479af45be39784d5870c41a"
company_key: "workday-inc-class-a-common-stock"
company: "Workday Inc."
source_id: "workday-inc-class-a-common-stock-rss-1edd291cea4c"
canonical_url: "https://medium.com/workday-engineering/building-bridges-unlocking-enterprise-ai-for-the-global-frontline-c299b21a8689"
published_at: "2026-04-09T14:53:20+00:00"
first_seen_at: "2026-07-20T04:35:52.231186+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:d295542aac96e746d8b6a9375a4892c9952c876e83e7183e38227c6599acdbda"
---

# Building Bridges: Unlocking Enterprise AI for the Global Frontline

Featured


[Sam Li](https://medium.com/@samxli?source=post_page---byline--c299b21a8689---------------------------------------)


5 min read


·


Apr 9, 2026


--


**Building Bridges: Unlocking Enterprise AI for the Global Frontline**


*We are introducing an*[open-source bridge](https://github.com/Workday/ai-conversation-bridge) *that links messaging apps your workforce already relies on with regionally hosted AI models, creating an adaptable pathway into Workday. It is a custom-fit architecture that eliminates digital friction and turns everyday frontline chats into instant action.*


By[Sam Li](https://www.linkedin.com/in/samxli/) , with contributions from[Sharon Liang](https://www.linkedin.com/in/leungsharon/) and[Andy Maxwell](https://www.linkedin.com/in/andrewmaxwell/)


> “I just want to get my job done, but every morning starts with a digital scavenger hunt…”


Frontline employees working for a multinational company in Asia face this frustrating sentiment every work day. The friction doesn’t come from their enterprise software, but from a fragmented digital environment split between corporate systems, local messaging apps, and regional AI. This environmental gap leads to delays, uncertainty, and a loss of trust.


These employees need simple, immediate answers: Who is the primary contact for this new sales account? Why isn’t my messaging app syncing with the corporate directory? Can I take time off next week to see the doctor? They deserve a seamless experience that meets them where they already are, allowing them to simply “ask” and “act” without breaking their daily flow of work.


The promise of agentic systems offered a compelling solution to this digital friction. They were designed to cut through complexity and deliver the simple, immediate actions employees desperately need.


However, adoption of agentic systems is being challenged by four core hurdles in markets like China, Japan, and Korea:


- **Super-App Dominance:** Agents struggle to gain traction when employees are anchored in local messaging platforms like WeChat, LINE, and KakaoTalk, where the agents aren’t natively accessible.
- **Regulatory Hurdles:** The utility of globally-hosted LLMs is severely limited in jurisdictions like China due to data governance and cross-border access restrictions.
- **Mobile Access Gap:** Between blocked platforms like Google Play in China and highly localized app store preferences in Korea, deploying a core mobile-based agent across these varied regional environments is incredibly difficult and creates adoption barriers for a mobile-first workforce.
- **Local Jargon:** Standard LLMs often struggle with company-specific terminology and localized HR processes, leading to more friction and incorrect responses.


## Getting closer to the user


If the future of work is a commingling of humans and AI, then in order for human workers to trust and engage with their agentic peers, these agents must prioritize knowing where the users are and meeting them there. This means respecting cultural nuances and habits in how human users interact with other people and systems.


These human interactions converge upon **conversations** and **dialog** . For agents, the hard part is not just generating content and taking action. The hard part is creating a reliable path for that dialog to happen smoothly.


That path shouldn’t be another standalone app, and it shouldn’t force users to abandon the tools they already trust. Instead, it must be a bridge.


## Enter the[AI Conversation Bridge](https://github.com/Workday/ai-conversation-bridge) .


It is a secure connector built to dismantle the roadblocks standing between your Asia workforce and the integrated AI experience they deserve.


[The Bridge](https://github.com/Workday/ai-conversation-bridge) connects the super-apps your teams already live in directly to your Workday core. More importantly, it empowers you to route requests through your own local AI models, along with any custom fine-tunes or RAG capabilities, that inherently follow regional data governance and natively understand your specific business terminology. Go more in depth and watch the introductory video on The Bridge’s[Github repository](https://github.com/Workday/ai-conversation-bridge) .


To see exactly how this architecture works in practice, let’s look at the lifecycle of a common request. Here is how a query travels from a messaging app, through your self-managed AI infrastructure, to your core system, and back:


**The Request Flow**


1. The user sends “` I want to take next Friday off, but I’m assigned to Project Omega. Am I allowed to?` ” in their preferred messaging app.
2. The messaging platform POSTs a webhook to the Chat Connector.
3. The Chat Connector extracts the message and user data, then calls your self-managed orchestration API.
4. The orchestration layer queries your vector store (RAG) to retrieve the latest internal project directives. It extracts a memo: “` All personnel assigned to Project Omega are in a ‘code freeze’ period until March 4th; PTO requests require explicit approval from the project lead.` ”
5. Simultaneously, your LLM recognizes the intent to verify the user actually has the balance to take the time off.
6. The orchestration layer securely calls the Workday MCP tool:` get_current_user_time_off_balance()` .
7. The MCP server retrieves the desired information from the Workday tenant
8. The MCP server returns:` { vacation: { available: 12, used: 3 } }` .
9. The orchestration layer passes both the Workday payload and the RAG directive back to your LLM, which synthesizes the final response: “` You have 12 vacation days available. And because you are assigned to Project Omega the code freeze period is over. You should be able to request the time off without additional approvals. Would you like to proceed?` ”
10. The Chat Connector receives the response and sends it back to the user via the messaging app’s API.


By integrating with your local environments, the Bridge turns everyday frontline conversations into immediate, secure actions.


Press enter or click to view image in full size


Behind the scenes, this architecture is powered by[Flowise](https://flowiseai.com/) . At its core, Flowise is an extensible, open-source, low-code agentic systems development platform. It acts as the dynamic and context-aware orchestration layer, capturing the employee’s request from their chat app, processes it through your preferred local LLM, and securely triggers the transaction via the Workday MCP server. This is an open, flexible architecture built to adapt to your ecosystem, rather than forcing your organization to abandon its preferred tools to fit an unfamiliar standard.


Press enter or click to view image in full size


For the open-source community, this represents a significant step in democratizing AI development and deployment. Moreover, Workday’s commitment to open source elevates our own internal standards. It demands clarity in our approach, incentivizes intentional design, encourages reusability, and accelerates delivery velocity. Ultimately, this open approach makes us better engineers and innovators.


## The bigger opportunity


The AI Conversation Bridge offers a modular, resilient approach to connecting conversational experiences with enterprise systems. It decouples the architecture into four distinct parts: the chat surface, the orchestration engine, the AI layer, and the core business system. Because you aren’t locked into a single UX or a specific model provider, you can swap out components as your needs change.


As the AI landscape evolves with new models and protocols, our engineering response must be to build adaptable bridges, not hardwire current assumptions. We are excited about what this project enables today and what it signals: Workday is building for the open, composable future of enterprise AI.
