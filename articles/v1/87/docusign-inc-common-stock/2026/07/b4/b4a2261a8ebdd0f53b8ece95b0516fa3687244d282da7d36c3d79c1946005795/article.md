---
schema_version: "1.0.0"
document_id: "b4a2261a8ebdd0f53b8ece95b0516fa3687244d282da7d36c3d79c1946005795"
company_key: "docusign-inc-common-stock"
company: "DocuSign Inc."
source_id: "docusign-inc-common-stock-news-import-aebf23be7403"
canonical_url: "https://www.docusign.com/blog/developers/docusign-developers-meetup-bengaluru-2026-recap"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T01:48:46.315991+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:32354600ecdd10d2606bdc6c1202dbce73bbc331c1c22b9153b2dff745546d4f"
---

# From Agreement Data to Agentic Workflows: Five Lessons from Bengaluru

Building an agentic agreement workflow involves more than adding an AI interface to an existing process.


The underlying agreement data has to be usable. APIs need to support agent-driven actions. Business rules need to live in the right place. And the workflow needs clear limits around what an agent can do on its own.


These themes came up throughout the[Docusign Developers Meetup in Bengaluru opens in a new tab](https://luma.com/1xg2bgfa?tk=P26F1t) , where developers, architects, implementation partners, and technology leaders gathered for a keynote, industry panel, and two hands-on workshops.


Here are five ideas from the day that are useful for anyone building agreement workflows with AI.


## **1. Agreement data becomes more valuable when it can drive action**


Most organizations already have large collections of agreements. The harder problem is making the information inside them available when another system, workflow, or person needs it.


That starts with extracting and structuring important terms such as renewal dates, payment terms, obligations, and risk clauses. Once that data is accessible through APIs and agents, it can do more than support search. It can trigger reviews, update other systems, surface risks, and inform the next step in a workflow.


The opening keynote from Apurva Dalal, Group Vice President of Engineering, Builders & Partners at Docusign, and Geetha Rajendran, Director of Engineering, Builders & Partners at Docusign, placed this idea in the broader context of Docusign Intelligent Agreement Management (IAM).


They showed how Docusign IAM connects the create, commit, and manage stages of the agreement lifecycle and how[Iris](https://www.docusign.com/products/platform/ai) , Docusign’s AI engine, helps turn information inside agreements into data that other processes can use.


For developers, the keynote’s central message was the extensibility of the Docusign IAM platform.[Docusign APIs](https://developers.docusign.com/docs/) , the[Docusign MCP Server](https://developers.docusign.com/platform/mcp-server/) ,[Agent Studio](https://www.docusign.com/products/agents) ,[Workflow Builder](https://developers.docusign.com/get-started/agreement-automation-workflow-builder/) ,[Agreement Desk](https://www.docusign.com/products/agreement-desk) , and other platform capabilities give builders several ways to connect agreements with the tools their customers already use.


That could mean starting an agreement from a CRM, bringing agreement insights into an AI assistant, or building a workflow that moves data and actions across multiple systems.


**Dive deeper:** Watch the Bengaluru keynote to see how these capabilities fit together across the agreement lifecycle.


## **2. APIs need to work for agents, not just people**


APIs have traditionally been designed for developers building applications. Agentic systems introduce a different kind of consumer: software that can choose when to call an API, interpret the result, and decide what to do next.


That makes the quality of the interface even more important. Schemas need to be clear. Errors need to be useful. Permissions need to be explicit. Actions need predictable outcomes.


This was one of the main topics in the panel moderated by Kelly Shalk, Director of Developer & Agent Relations at Docusign, with:


-


Vijaya Raman, Partner and Salesforce Practice Leader at IBM


-


Amrit Sanjeev, Staff Developer Relations Engineer at Google


-


Prashanth Rao, Senior Director of Product Management at Docusign


The panel explored how AI-assisted development is changing day-to-day engineering work, along with the design questions that come with building agentic systems: what agents can access, which actions they can take, how failures are handled, and when human review is required.


Enterprise adoption adds another layer. Many companies are starting with bounded use cases where agents can take specific actions within defined limits, while people retain control over exceptions and consequential decisions.


For developers and product teams, the challenge is to support more autonomous behavior without making enterprise systems less predictable, secure, or governable.


**Dive deeper:** Watch the panel for perspectives from Docusign, Google, and IBM on AI development tools, enterprise adoption, and platform design.


## **3. Agreement workflows are moving closer to where work already happens**


Users shouldn’t have to copy information between systems or switch tools repeatedly to move an agreement forward.


The first workshop, Architect AI-Powered Agreement Automation, showed what this can look like in a Salesforce sales process.


Led by Balaji Jayaraman, Senior Programmer Writer at Docusign, and Sangeeta Bora, Senior Product Manager at Docusign, the workshop followed a hypothetical sales team trying to close an enterprise deal at the end of the quarter.


The challenge was to generate a negotiated MSA from Salesforce, populate it with CRM data, route it through review, check it against company policies, and prepare it for signature without relying on manual copying and long email threads.


The workflow started in Salesforce, where a user asked Agentforce to begin agreement preparation using the context already stored in the CRM. The agreement then moved into Docusign for review, collaboration, compliance checks, and preparation for signature.


Attendees also built workflows in Workflow Builder and explored how different parts of the agreement process could be automated.


The important pattern was not limited to Salesforce. The same approach can apply anywhere an agreement process starts in another business system: keep users in the tool they already use, while Docusign handles the agreement steps behind the scenes.


**Dive deeper:** Watch the workshop to see how Salesforce Agentforce, Workflow Builder, and Docusign IAM can support the pre-signature process. You can follow along with[the instructions opens in a new tab](https://github.com/docusign/developers-meetup-blr) .


## **4. The agent is only the visible layer**


A conversational interface may be the part users see, but much of the work required to make an agent useful happens underneath it.


The agreement data needs a consistent structure. Existing documents need to be ingested. Permissions need to be configured. Developers need to define which actions the agent can take and how those actions connect to downstream systems.


The second workshop, Create Agentic Agreement Workflows with Agents and MCP, walked through that implementation layer using a procurement scenario.


Presented by Amrit Prakash, Lead Partner Solutions Architect at Docusign, the workshop followed this sequence:


1.


Model agreement data with the IAM Toolkit and Docusign CLI


2.


Ingest existing agreements at scale


3.


Query that data through an MCP-powered agent


Once the agreements were structured and available, users could ask questions such as which contracts renew in the next 90 days, which vendors have longer payment terms, or where specific risk clauses appear.


For implementation partners and developers, the session showed how much of an agentic workflow depends on the data model, ingestion process, permissions, and available actions, rather than just the model responding to a prompt.


**Dive deeper:** Watch the workshop to see how the IAM Toolkit, Docusign CLI, agreement ingestion, and MCP can work together and follow along with the[workshop instructions opens in a new tab](https://docusign.github.io/developers-meetup-blr/Workshop-2/2_Hands_on_Flow.html) .


## **5. Human oversight works best when it is designed into the workflow**


“Keep a human in the loop” is easy to say. The harder question is where that review should happen and what should trigger it.


A well-designed workflow does not require a person to approve every action. It defines which actions are routine, which exceptions need review, and which decisions should always remain with a person.


That can mean allowing an agent to retrieve agreement data and prepare a draft, but requiring approval before sending it. It can mean automatically routing an agreement that meets policy while escalating one with unusual terms. Or it can mean limiting the systems and records an agent can update.


This pattern appeared across the event: in the panel’s discussion of bounded enterprise use cases, in the use of AI-Assisted Review during the[first workshop opens in a new tab](https://github.com/docusign/developers-meetup-blr) , and in the permissions and action design behind the MCP-powered agent in the second.


The better approach is to automate routine steps and reserve human review for exceptions and higher-risk decisions.


## **What to take away**


Together, the sessions covered both sides of the developer opportunity: improving how agreements are created and managed, and making the data inside completed agreements more actionable.


For developers, the opportunity is to connect these capabilities in ways that make agreement workflows easier to build, extend, and automate.


Watch the full[Docusign Developers Meetup Bengaluru playlist opens in a new tab](https://youtube.com/playlist?list=PLQluyH2yHVAY&si=zK2AGoJ9p-xS2w0X) for the keynote, panel, and both workshops.


*Learn more at the*[Docusign Developer Center](https://developers.docusign.com/) *.*
