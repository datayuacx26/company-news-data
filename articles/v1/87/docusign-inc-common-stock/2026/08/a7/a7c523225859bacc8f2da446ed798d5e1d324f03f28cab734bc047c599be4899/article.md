---
schema_version: "1.0.0"
document_id: "a7c523225859bacc8f2da446ed798d5e1d324f03f28cab734bc047c599be4899"
company_key: "docusign-inc-common-stock"
company: "DocuSign Inc."
source_id: "docusign-inc-common-stock-news-import-aebf23be7403"
canonical_url: "https://www.docusign.com/blog/developers/developers-guide-docusign-iris-agentic-capabilities"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T22:03:43.374746+00:00"
fetched_at: "2026-08-18T22:03:45.457209+00:00"
content_hash: "sha256:44276b28da794ffad26e53d7026731e4c8ea59d93ce6ae52b83aa575f5b27e31"
---

# A Developer’s Guide to Building Agentic Agreement Workflows with Docusign Iris

**Key takeaways**


-


Docusign Iris is the AI engine behind the agentic capabilities, turning agreement context into insights and action.


-


Docusign gives builders several ways to use Iris-powered agreement intelligence: in-platform agents and workflows, portfolio analysis with Worksheets, and external AI workflows through the Docusign MCP Server.


-


These capabilities help teams reduce manual agreement work while keeping people in control of consequential decisions.


Agreement workflows rarely happen in one system. A renewal might need customer data from a CRM, pricing from a CPQ platform, legal approval, the right agreement template, and follow-up after the document is sent.


Each step may be straightforward on its own. The hard part is coordinating them reliably, with the right context, permissions, and human review.


[Docusign Iris](https://www.docusign.com/products/platform/ai) is the AI engine behind Docusign’s agreement intelligence and AI experiences. It uses agreement context, such as terms, obligations, amendments, negotiation history, and renewal provisions, to help teams move from understanding agreements to acting on them.


For developers, that means agreement intelligence can become part of a governed workflow. Builders can use ready-to-deploy agents inside Docusign, create custom agents for organization-specific processes, add agents to structured workflows in Workflow Builder, analyze agreement portfolios with Worksheets, or bring Docusign intelligence into third-party AI applications through the[Docusign MCP Server](https://developers.docusign.com/platform/mcp-server/) .


*Watch this overview to understand how Docusign’s agentic capabilities can support agreement workflows.*


## **Choose the right building block for your workflow**


Docusign provides several ways to use Iris-powered agreement intelligence, depending on where users work and how much reasoning, orchestration, and customization a workflow requires.


### **AI assistant**


The[AI assistant](https://www.docusign.com/products/platform/ai) lets users ask questions about their agreement data and receive responses grounded in the agreements they can access.


A user might ask the assistant to summarize a contract, identify an upcoming renewal, or explain how a clause differs from standard language.


Because the assistant works with agreement context rather than standalone documents, it can provide more relevant answers than a general-purpose model working from isolated files. In many cases, the assistant is also the entry point for calling Docusign agents.


**Where to start:** Review[Docusign Iris](https://www.docusign.com/products/platform/ai) ,[AI at Docusign](https://support.docusign.com/s/document-item?language=en_US&bundleId=fzd1707173174972&topicId=usy1712258503642.html&_LANG=enus) , and[Build with Docusign and AI](https://developers.docusign.com/tools/build-with-docusign-ai/) .


### **Ready-to-deploy agents**


Ready-to-deploy agents handle repeatable, multi-step agreement tasks without requiring a team to design an agent from scratch.


These agents are intended for repeatable workflows such as retrieving agreement information, monitoring status and obligations, applying legal playbooks, and guiding users through standard agreement processes.


Use a ready-to-deploy agent when the task is common, the workflow primarily runs within Docusign, and you need a faster starting point with less customization. Users can invoke agents through the AI assistant or use them as steps in Workflow Builder.


**Where to start:** Explore[Docusign Agents](https://www.docusign.com/products/agents) and the[Docusign Developer Center](https://developers.docusign.com/) for APIs, workflows, and implementation resources.


### **Agent Studio**


Agent Studio lets teams build custom agents for organization-specific processes. A builder describes the desired behavior in natural language and provides the instructions, policies, playbooks, and business context the agent should follow.


Custom agents are useful when a workflow depends on knowledge unique to an organization, such as approval requirements, pricing policies, escalation rules, or standard operating procedures.


After testing and refining the agent, teams can make it available to other users or invoke it from a broader workflow.


#### **Example: Review past agreements before a renewal**


Before a renewal, a legal operations team may need to review prior agreements, amendments, nonstandard terms, total contract value, and upcoming renewal dates.


With Docusign Agent Studio, the team could create a custom agent with instructions such as: *Find the previous agreements for this customer. Summarize the key commercial and legal terms, identify deviations from our standard terms, calculate the total contract value, and list upcoming renewal dates.*


The agent could then:


1.


Retrieve agreements associated with the customer


2.


Extract the relevant terms and dates


3.


Compare the language against the organization’s legal playbook


4.


Flag exceptions that require review


5.


Return a structured renewal summary


The legal team still defines the policies and review requirements. The agent handles the repetitive work of collecting and organizing the information so reviewers can focus on judgment and negotiation.


Once tested, the same agent could be made available to other legal or sales teams or invoked from a broader renewal workflow in Docusign[Workflow Builder](https://www.docusign.com/products/platform/workflow-builder) .


**Where to start:** Explore[Docusign Agents](https://www.docusign.com/products/agents) for custom-agent capabilities and the[Docusign Developer Center](https://developers.docusign.com/) for broader implementation resources.


#### **Use agents in Workflow Builder**


Custom agents become more powerful when they are part of a structured workflow. In Docusign[Workflow Builder](https://support.docusign.com/s/document-item?language=en_US&bundleId=yff1696971835267&topicId=dnx1696972415150.html&_LANG=enus) , agents can be added as steps that combine agent reasoning with rule-based routing, approvals, and downstream actions.


For example, an agent can evaluate an agreement against a playbook and return a structured result. Workflow Builder can then use that result to route an agreement, request approval, escalate an exception, or trigger another downstream action.


When configuring a custom agent for a workflow, define a structured output schema so the agent returns machine-readable results. In Workflow Builder, add the custom AI agent step and use the output in conditional logic.


**Where to start:** Explore[Workflow Builder](https://support.docusign.com/s/document-item?language=en_US&bundleId=yff1696971835267&topicId=dnx1696972415150.html&_LANG=enus) and the[Workflow Builder API](https://developers.docusign.com/docs/workflow-builder-api/) .


### **Worksheets**


[Worksheets in Agreement Manager](https://support.docusign.com/s/document-item?language=en_US&bundleId=map2682ec9e-5c2d-467b-ab4a-aace25e0d24b&topicId=con0114bdd3-186b-4f48-8198-2bad87774b7e.html&_LANG=enus) help teams extract and compare specific data across thousands of agreements at once.


Teams can use Worksheets to identify vendor agreements that allow price increases above a certain threshold, find customer contracts with specific renewal or termination clauses, and surface obligations or caps that deviate from policy.


Worksheets are powered by Iris, but they are not agentic. They help users analyze agreement data and make decisions; they do not independently route agreements, send documents, or execute workflow actions.


Results are grounded in live signed agreements and can include citations, reasoning, and confidence indicators. This helps teams verify the source of each extracted value during a high-volume review.


**Where to start:** Explore[Worksheets](https://support.docusign.com/s/document-item?language=en_US&bundleId=map2682ec9e-5c2d-467b-ab4a-aace25e0d24b&topicId=con0114bdd3-186b-4f48-8198-2bad87774b7e.html&_LANG=enus) ,[Agreement Manager](https://www.docusign.com/products/platform/agreement-manager) , and the[Agreement Manager API](https://developers.docusign.com/docs/agreement-manager-api/) .


### **Docusign MCP Server**


The Docusign[MCP Server](https://developers.docusign.com/platform/mcp-server/) makes Docusign capabilities available in supported third-party AI applications, including environments such as Claude, ChatGPT, Gemini Enterprise, Copilot Studio, and Slack.


This lets developers and business users connect agreement workflows to the tools where teams already work. Depending on the supported environment and configuration, users can create, review, send, track, sign, search, analyze, and manage agreements with natural-language prompts.


The MCP Server is especially useful when an agreement workflow spans multiple systems. An external AI agent might retrieve account data from a CRM, check approved pricing in a CPQ system, confirm approvals, and then use Docusign to prepare an agreement.


#### **Example: Prepare and send an order form from an AI assistant**


Suppose an account executive needs to prepare a renewal order form. The required information may live across a CRM, CPQ system, approval system, and Docusign.


A user could deploy a custom agent in their AI environment and ask: *Prepare an order form for the upcoming renewal.*


The custom agent can then:


1.


Retrieve the customer and opportunity details from the CRM.


2.


Retrieve the approved products, pricing, and discounts from the CPQ system.


3.


Confirm that required approvals have been completed.


4.


Present the proposed agreement details for review.


5.


Use the Docusign MCP Server to select and populate the appropriate template.


6.


Send the agreement for signature after the user confirms.


For consequential actions such as sending an agreement, the agent should present the relevant details and receive explicit approval before proceeding.


**Where to start:** Start with the[MCP Server developer documentation](https://developers.docusign.com/platform/mcp-server/) , then choose a setup path for[Claude](https://developers.docusign.com/platform/mcp-server/anthropic-claude/) ,[ChatGPT](https://developers.docusign.com/platform/mcp-server/openai-chatgpt/) ,[GitHub Copilot](https://developers.docusign.com/platform/mcp-server/github-copilot/) , or other applications.


## **Choose a focused workflow and the right implementation**


When designing your own implementation, start with one agreement process that has clear inputs, repetitive steps, and a defined outcome. Then decide where the workflow should run and what it needs to do:


-


Use Docusign agents or Agent Studio when the work is primarily agreement-focused and benefits from Docusign context, policies, and controls.


-


Use Workflow Builder when the process needs deterministic routing, approvals, conditions, and downstream actions.


-


Use Worksheets when the goal is high-volume portfolio analysis rather than autonomous action.


-


Use the Docusign MCP Server when the workflow needs to run in an external AI ecosystem or coordinate Docusign with other business systems.


The most effective implementations focus on a specific outcome, make agreement context available, define the boundaries of automation, and keep people in control of consequential decisions.


Developers can also combine these capabilities with[Docusign APIs](https://developers.docusign.com/docs/) ,[extension apps](https://developers.docusign.com/extension-apps/) , and[Docusign Workflow Builder](https://support.docusign.com/s/document-item?language=en_US&bundleId=yff1696971835267&topicId=dnx1696972415150.html) for more advanced implementations.


## **Security and control**


Agents are limited to the agreement data the user can access. Permissions configured in Docusign Agreement Manager and other Docusign IAM services determine which agreements and agreement sets that user can retrieve.


When designing an agentic workflow, developers should also decide:


-


Which actions the agent can perform


-


Which actions require user confirmation


-


What data can be sent to external systems or models


-


How the workflow will handle missing or conflicting information


-


What should happen when an approval or policy check fails


-


Which actions and decisions need to be logged


These controls are particularly important for workflows involving confidential agreements, employment documents, regulated data, or financial commitments.


## **Availability**


Availability varies by capability, plan, region, language, and launch phase.


-


[AI assistant, ready-to-deploy agents, and custom agents in Agent Studio](https://www.docusign.com/products/platform/ai) are generally available as of July 2026 and Worksheets in[Agreement Manager](https://www.docusign.com/products/platform/agreement-manager) as of August 2026 for customers hosted in North America data centers with IAM Professional, IAM Enterprise, IAM for Sales, or IAM Platform plans (English only). Using agents as steps in Workflow Builder is supported on IAM Professional plans and above, initially for accounts in North America data centers in English.


-


The Docusign[MCP Server](https://developers.docusign.com/platform/mcp-server/) and connectors are available in global open beta in English, with support for AI environments currently including[Anthropic (Claude)](https://developers.docusign.com/platform/mcp-server/anthropic-claude/) ,[GitHub (Copilot)](https://developers.docusign.com/platform/mcp-server/github-copilot/) ,[OpenAI (ChatGPT)](https://developers.docusign.com/platform/mcp-server/openai-chatgpt/) ,[Google (Gemini-CLI),](https://developers.docusign.com/platform/mcp-server/google-gemini-cli/)[Microsoft Copilot Studio](https://developers.docusign.com/platform/mcp-server/microsoft-copilot/) , and[Slack (Slackbot)](https://developers.docusign.com/platform/mcp-server/slackbot/) . General availability is targeted for a future release.


Because availability and supported capabilities can vary by region, plan, and launch phase, always check the latest product documentation and program requirements before beginning an implementation.


## **Additional Resources**


-


[Build with Docusign and AI](https://developers.docusign.com/tools/build-with-docusign-ai/) to explore the Docusign MCP Server, Agent Studio, Agreement Manager API, and other AI-focused developer capabilities.


-


[Get started with the Docusign MCP Server](https://www.docusign.com/blog/developers/claude-docusign-mcp-connector-guide) to connect Claude to Docusign agreement data and actions.


-


[Explore the Agreement Manager API](https://developers.docusign.com/docs/agreement-manager-api/) to retrieve AI-extracted agreement data for your applications and workflows.


-


[Explore the Workflow Builder API](https://developers.docusign.com/docs/workflow-builder-api/) to create, trigger, and manage agreement workflows programmatically.
