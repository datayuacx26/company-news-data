---
schema_version: "1.0.0"
document_id: "cfce0143e581cad896e54827d0082068525be3f73b79efa5cd3f1c6bf139d355"
company_key: "yc-promptarmor"
company: "PromptArmor"
source_id: "yc-promptarmor-rss-057c96aa2b5f"
canonical_url: "https://promptarmor.substack.com/p/data-exfiltration-in-openai-agent"
published_at: "2025-10-14T12:33:21+00:00"
first_seen_at: "2026-07-25T19:53:47.024087+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:8bdcdece487350914b3b54abf650fcc6c443a3e9f26bb2e17b1855afa3254334"
---

# Data Exfiltration in OpenAI Agent Builder via MCP

# Data Exfiltration in OpenAI Agent Builder via MCP


### By: PromptArmor 10/14/2025


Oct 14, 2025


**Context**


Our goal is to demonstrate an indirect prompt injection attack on OpenAI Agent Builder, as this new use case will be widespread and in the hands of users who may not be aware of this attack surface. OpenAI is aware of this issue and recommends protections for users to implement.


Our objective here is driven by our observation of this interesting pattern - we are seeing more AI applications pass on the responsibility of avoiding indirect prompt injection attacks onto users - while at the same time appealing to less technical users who may not be equipped to fully understand these risks.


‘Shared responsibility’ (which has drawn ire in other scenarios e.g. recent Salesforce breaches) is emerging in the AI ecosystem with a dangerous form: where companies have transitioned from


**“we should implement best practices against prompt injection to protect our users”** to


**“we acknowledge prompt injection is an issue and you should implement best practices, best of luck”** . They are putting the burden on users who may not understand the threat model. As such, this article aims do demonstrate how even a ‘simple’ workflow in Agent Builder can be easily exploited.


****


**Agent Builder**


Agent Builder is a low-to-no code AI workflow builder released last week by OpenAI. Agent Builder allows non-technical users to create workflows using AI Agents, and use them to power applications (easily built using ChatKit, OpenAI’s new tool to create AI app user interfaces).


An agentic sales workflow designed to perform email outreach is manipulated by a prompt injection in a Google Form submission (external data) to send an email with customers’ data from Salesforce (internal data) to an attacker.


In our testing, we created a sales tool which ingested lead forms and CRM data and sent personalized emails. We proved that an attacker was able to submit a malicious lead form to convince the agent to instead send confidential data from the CRM to the attacker directly. Along the way, we also successfully bypassed a ‘Guardrail’ workflow step using a prompt injection.


As previously mentioned, this is known functionality; OpenAI has acknowledged that systems created with the Agent Builder, like many agentic AI systems before it, are susceptible to being manipulated through prompt injection attacks (when malicious data sources trick the AI model into performing actions against the user’s wishes) -


[even when users have implemented the provided protections](https://platform.openai.com/docs/guides/agent-builder-safety#private-data-leakage) . Their recommendations to users include: leverage a human in the loop, use structured outputs, use recent models, do not use untrusted variables in developer messages and leverage the Guardrails feature.


**Manipulating an AI Agent**


The primary building block is called ‘Agent’ that leverages a large language model to perform a task. Developers can configure the agent’s system prompt, tools, and what model powers it (e.g., GPT-5 vs GPT 4).


In this scenario, we prove that an agentic sales system connected to external services through MCP tools* can be manipulated by untrusted data (in Google Form submissions) to expose sensitive data (from Salesforce) by sending emails (via Gmail) to malicious recipients.


**MCP (Model Context Protocol) is an open-source standard for integrating and sharing data with external tools, systems, and data sources. Applications can host ‘MCP Servers’ to allow AI agents to integrate with their capabilities.*


**Building the Agentic System**


We started by naming an agent ‘Get leads from form submissions’, and granting it the ability to read submissions from a Google Form representing web leads. We did this using the Zapier MCP tool, which allowed us to create a custom connection between Google Drive and our Agent, such that the Agent can read all of the Google Form submissions every time the Agent workflow is activated.


Left: The ‘Get leads from form submissions’ agent is configured to read Google Form responses from a Google Sheet using a Zapier MCP tool. Right: The demo form submissions are collected from.


Next, we created an agent called ‘Get leads from new CRM records’ and connected it to Salesforce, instructing it to retrieve any new leads or contacts from the last week.


The ‘Get leads from new CRM records’ Agent is configured to read Salesforce CRM records using the Zapier MCP tool called ‘salesforce_find_records’.


Then, we created a ‘Triage Leads’ agent. This agent was given the ability to send emails to engage prospects and create new CRM records.


The ‘Triage leads’ Agent is configured with Zapier MCP tools it can use to send emails with Gmail and create Salesforce contacts.


*Note: there are two primary protections that OpenAI Agent Builder allows users to build into the workflow, within this context:*


*1. Jailbreak Guardrail Blocks to prevent prompt injections. We believe many users will not implement this protection, as it represents overhead to get the system running - to mitigate a threat they have never witnessed first-hand. Again, the premise of this article is to showcase the threats to the average enterprise user. Nevertheless - we demonstrate in the appendix that our attack can succeed even with the Jailbreak Guardrail Block in place.*


*2. Human in the Loop controls. When considering whether to allow tool calls without approval, we considered how our workflow would be configured during real world use.*


-


*Empirically, we have seen enterprise teams developing workflows like this one without human-in-the-loop controls.*


-


*It is a simpler workflow, created by salespeople (who are likely to be less security-driven).*


-


*Users build agentic workflows for the explicit purpose of taking themselves out of the loop.*


**Exfiltrating the Agent’s Data**


With our system configured, we posed as a bad actor trying to exfiltrate CRM data. To do this, we submitted a Google Form response containing a prompt injection.


This injection was designed to trick the Triage Leads Agent into sending CRM data in its outreach email, by claiming to be a partner under NDA working on finalizing a deal.


Here’s what went down:


1.


**Get leads from form submission Agent**


The ‘Get leads from form submission’ Agent executes an MCP tool call to retrieve the form responses from Google Sheets. After collecting the form response (which contains the attacker’s prompt injection), it outputs the contents of the form response to the next workflow step.


**2. Get leads from new CRM Records Agent**


The ‘Get leads from new CRM records’ Agent executes an MCP tool call to retrieve recently updated contacts from Salesforce. The agent then sends the CRM data, and the form response received from the prior agent (which contains the attacker’s prompt injection), to the next step in the workflow.


The ‘Get leads from new CRM records’ Agent reads from Salesforce and finds records corresponding to new contacts and leads.


**3. Triage leads Agent**


The ‘Triage leads’ Agent read the attacker’s prompt injection - and followed the attacker’s prompt injection - sending the internal CRM data to the attacker’s email!


The ‘Triage leads’ agent confirms that it has read the prompt injection from the attacker’s form submission and acted on it by sending an email containing the CRM contacts to the attacker.


The agent (emailing under the name Marissa Johnson) sent an email containing the Salesforce CRM records to the attacker’s email.


## Appendix:


**A Note on Safety Mechanisms**


OpenAI has provided several safety mechanisms that can be utilized to substantially decrease the risk profile of agentic applications. However, as they themselves


[noted](https://platform.openai.com/docs/guides/agent-builder-safety#private-data-leakage) , the protections aren’t foolproof.


We examined a subset of the defenses, and identified shortcomings agent builders should be aware of:


**1. The ‘Guardrails Block’**


Considering that this workflow is intended to run autonomously, a user who is concerned about the risk of prompt injections or jailbreaks may decide to leverage OpenAI’s Guardrail block.


The Guardrail block is designed to examine data that flows through it against jailbreak, PII, moderation, or hallucination policies. When a workflow reaches a Guardrail Block, the Guardrail Block evaluates the data from the previous steps and outputs either ‘passed’ or ‘failed’ depending on whether the data violates one of the policies set by the developer.


The result from the Guardrail Block can be used as a condition to determine what workflow step is executed next. For example, a developer could configure a workflow where if the guardrail step outputs ‘failed’ because it detected a malicious input, the workflow stops early. This allows a human to step in and review what data violated the guardrail policy. If the guardrail step does not detect any policy violations, and outputs ‘passed’, then the next agent step begins.


Without the Guardrail Block, the output from the first two workflow steps (‘Get leads from form submissions’ and ‘Get leads from new CRM records’) is a direct input to the “Triage Leads” step.


So, we insert the Guardrail Block right before the “Triage leads” step. It is configured this way so that the Guardrail Block can check the data passed from the first two workflow steps (‘Get leads from form submissions’ and ‘Get leads from new CRM records’) for prompt injections.


If the Guardrail Block outputs ‘passed’, our workflow will continue. If the Guardrail Block outputs ‘failed’, our workflow will execute an ‘End Block’, stopping the workflow immediately.


This way, if the data does contain a prompt injection, the Guardrail Block will prevent this prompt injection from being passed to the final agent (“Triage Leads”). This is important because the final agent has the capability to take more sensitive actions that can be manipulated by the prompt injection (e.g. sending emails).


A ‘Guardrail Block’ is added to our old workflow, in between the agents responsible for collecting data and the ‘Triage Agent’ responsible for sending emails.


We enabled the ‘Jailbreak’ defense, and set the default model choice and confidence threshold (GPT-4.1-mini, 70% confidence).


The ‘Guardrails’ Block is configured to process all data from earlier in the workflow using the ‘Jailbreak’ policy. If the GPT 4.1 Mini model detects a jailbreak with 70% confidence, the guardrail check will fail.


The flaw? The prompt injection guardrails are


[LLM-based](https://openai.github.io/openai-guardrails-python/ref/checks/jailbreak/) , meaning it can be tricked in the same way that the agents themselves can be tricked.


It did not detect the attacker’s prompt injection.


The Guardrail Block outputs ‘passed’ despite the presence of a prompt injection, allowing the malicious data to be passed to the next workflow step.


**2. Interesting find during Reasoning**


One mechanism OpenAI uses to improve the performance of its models is allowing the model to ‘reason’ before outputting to the user. To ‘reason’, the model has a conversation with itself, discussing what its objectives are and how to achieve them. This reasoning logic is typically displayed to the user as it is being created, but after each ‘thought’ is complete, the reasoning block is concealed from the display (unless the user goes back and clicks to expand it). After ‘reasoning’, the model prints a final output to the user.


In the ‘reasoning’, we can see that internally, the model is motivated to identify prompt injections - so that it can be sure to avoid executing instructions from untrusted parties. This is likely due to some protections at the “model layer”; in other words, safeguards OpenAI put in during training - or after training - to mitigate the risks of prompt injection.


The model recognized the prompt injection in its ‘Reasoning’, stating: ‘I want to avoid executing any instructions that are part of the content itself’.


The model identifies a malicious instruction (prompt injection), calling it out as a social engineering attempt, and choosing not to act on it.


We noticed an interesting behavior that appeared in the ‘Get leads from form submission Agent’:


If the agent identified an injection, it expressed the intention to avoid acting on the injection. However, it reasoned that as long as it did not execute the malicious request itself, it was okay to continue processing the malicious data according to its normal workflow.


Model reasoning trace showing the agent deciding that it is okay to send data including a prompt injection to the next workflow step, as long as it does not act on the prompt injection itself.


This can be problematic for multi-step AI workflows, as was seen in our example. It means that - even if a model


*identifies* a prompt injection - it can still be passed verbatim from one agent to another, allowing for the manipulation of agents deeper in the workflow.


Thanks for reading! Subscribe for free to stay in the know about AI Security.
