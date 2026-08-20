---
schema_version: "1.0.0"
document_id: "1220887d274a21fc05b2d54fae250811335f0367393fab63720bc639bd7c18ee"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/verify-users-with-claude/"
published_at: "2026-08-03T18:21:24+00:00"
first_seen_at: "2026-08-04T05:44:56.010101+00:00"
fetched_at: "2026-08-04T09:43:50.004226+00:00"
content_hash: "sha256:91c3a544085a13512530349702a3d47a18ebd30f9acf012014cf5f71843e9c46"
---

# How to Verify a User's Identity with Claude

[Back to blog](https://didit.me/blog/) Blog · August 3, 2026


# How to Verify a User's Identity with Claude


Verify a user from inside Claude with natural-language prompts: create the hosted link, run ID, passive liveness, face match, and IP analysis, then read the decision. No code required.


By Didit


·


August 3, 2026 ·


Updated Aug 3, 2026


## **Key takeaways**


- This is an operator playbook: the exact words to type in Claude, what the applicant experiences, how to read the answer, and what to do when a session needs review.
- Didit's Model Context Protocol (MCP) connector lets a signed-in operator use existing workflows and permissions from chat without writing code.
- A workflow controls which Know Your Customer (KYC) checks run. Listing workflows finds the choices; reading the selected workflow reveals its configuration.
- The applicant completes the configured checks on a Didit-hosted page. Claude retrieves and explains Didit's result; it does not inspect the person's document or face itself.
- **In Review** is a handoff for human judgment, not another word for Declined. Ask Claude to separate returned evidence from missing information before anyone changes the status.


You do not need to know an application programming interface (API) schema to help one applicant through identity verification. You need a Didit account, an approved verification workflow, the Didit connector enabled in Claude, and authority under your organization's review policy. The rest can happen in ordinary language.


This guide is deliberately about the person operating the conversation. It does not repeat the create-link-and-poll mechanics already covered in[MCP for KYC](https://didit.me/blog/mcp-kyc-identity-verification) and the[KYC MCP server guide](https://didit.me/blog/kyc-mcp-server-guide) . Keep those references open when you need lifecycle or integration detail. Use this page when the practical question is: “What should I type, what should I tell the applicant, and what do I do with the answer?”


## **Before the applicant: establish the right workspace**


[Add the Didit connector to Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Didit&connectorUrl=https%3A%2F%2Fmcp.didit.me%2Fmcp) and complete the Didit sign-in. The hosted endpoint uses OAuth (Open Authorization) 2.1 with PKCE (Proof Key for Code Exchange), not an API key. Claude acts with the signed-in user's Didit role, so the operator must already have permission for any action they request.


Start every new operating conversation by making the scope visible:


```text
Use Didit to help me verify one applicant. First call didit_context_get. Tell me which organization and application are selected. Do not create or change anything yet.
```


This catches the easiest operational mistake: working in the wrong application when one person can access several. If Claude shows more than one option, name the organization and application you intend to use before moving on.


## **Choose a workflow without guessing its checks**


` didit_workflow_list` lists available workflows. It does **not** return the full workflow graph or configuration. Use it to find the approved workflow name and` workflow_id` , then retrieve the selected workflow explicitly.


```text
List the verification workflows in the selected application with didit_workflow_list. Show only each workflow's name, workflow_id, and status. Do not describe its checks yet.
```


After you select one, ask for the actual configuration:


```text
Retrieve workflow WORKFLOW_UUID with didit_workflow_get. If its steps or branches require graph detail, also call didit_workflow_get_graph with include_config: false. Then explain, in plain English, what the applicant must do. Separate applicant-visible steps from checks that run in the background. Do not create a session.
```


Use` didit_workflow_get` for the full configuration of the selected workflow. Use` didit_workflow_get_graph` when you need its nodes, branches, conditions, or document-processing steps; its default summarized configuration is enough for an operator explanation. This two-step pattern prevents Claude from inferring a bundle merely from a workflow label.


## **Ask for one applicant link**


Once you have confirmed the workspace and workflow, the operator instruction can stay short:


```text
Create one session with didit_session_create using workflow_id WORKFLOW_UUID and vendor_data customer-8421. Return the session_id and url. Do not send the link or change any other record.
```


The only required input to` didit_session_create` is` workflow_id` ;` vendor_data` is an optional customer reference. The response includes a` url` . Copy that hosted link into your approved email, support, or onboarding channel. Creating the session does not itself mean the applicant has been contacted.


That is all the mechanism this operator guide needs. If you are implementing automated delivery, callbacks, webhooks, or polling, use the linked technical guides rather than turning an operator conversation into an integration tutorial.


## **Tell the applicant what will happen**


The applicant opens a Didit-hosted page in their browser; they do not need Claude or an MCP connection. The exact experience follows the selected workflow. A configured full KYC bundle can include identity-document capture, passive liveness, a one-to-one face match, and Internet Protocol (IP) analysis. A different workflow may contain fewer checks, additional checks, or conditional branches.


Ask Claude to draft a message grounded only in the retrieved configuration:


```text
Write a four-bullet message for the applicant explaining what they will see after opening the url. Use only the selected workflow configuration. Mention any document or device preparation it actually requires. Do not promise approval, a completion time, or checks that are not configured.
```


A good operator message explains why the person received the link, which visible steps they will complete, and where to ask for help. It should not expose the internal session token, copy personal data into chat, or describe a background check as an applicant action.


Didit supports 220+ countries and territories, 14,000+ document types, and 48+ languages. Those coverage figures describe the platform; the selected workflow and the applicant's document still determine the actual screens available in that session.


## **Ask for a plain-English result**


When the applicant says they are finished, do not ask Claude whether they “passed.” Ask it to retrieve the recorded decision and keep status separate from evidence:


```text
Call didit_session_get_decision for session SESSION_UUID. Explain the result for a non-technical onboarding operator. Start with the exact current status. Then list only the configured module results and fields actually returned. Separate confirmed evidence, missing evidence, conflicts, and items needing human judgment. Do not change the session.
```


This wording makes hallucination easier to notice. The decision contains the output of modules configured for that workflow, not a universal set of identity-document, liveness, face-match, Anti-Money Laundering (AML), and fraud checks. If a module did not run or a field is absent, the answer should say so rather than fill the gap.


Read the status as the current session state:


- **Not Started** or **In Progress** means the operator should wait or help the applicant complete the hosted flow.
- **In Review** means evidence or workflow logic has routed the session to a human decision.
- **Approved** or **Declined** is the current decision state. Either can reflect configured automation or an authorized reviewer's manual override, so use the accompanying evidence and audit history when policy requires it.
- **Resubmitted** means selected workflow nodes were sent back for another attempt; it is not a fresh, unrelated session.


Model inference runs at sub-2-second p99, but that is not a promise about how long an applicant will take to capture a document, complete the flow, or wait for human review.


## **What to do when the answer is In Review**


Do not translate **In Review** into “failed,” and do not ask Claude to approve or decline in the same prompt that explains the evidence. First request a read-only review packet:


```text
This session is In Review. Call didit_session_get_decision and didit_session_list_reviews. Do not change data or status. Show the returned reason or triggering evidence, the configured module outputs relevant to it, any conflicting or missing information, and prior review or status history. Mark anything not returned as unknown.
```


Then follow your organization's escalation policy. The reviewer may compare extracted identity data with the document evidence, assess a match or screening candidate, request another attempt for exact failed workflow nodes, or make an authorized status decision. Claude can organize the record, but it does not replace the reviewer or the organization's acceptance policy.


If you are authorized to document the review, keep the note separate from the final decision:


```text
Add this comment with didit_session_add_review to session SESSION_UUID: “Escalated for manual review because [observed evidence].” Do not pass new_status and do not modify extracted data.
```


` didit_session_add_review` requires` session_id` , accepts a` comment` , and can optionally change status. Omitting` new_status` makes the intent clear here: record the review note without deciding the case. For corrections, partial resubmission, approval, and decline procedures, use the dedicated[KYC with Claude review-queue guide](https://didit.me/blog/kyc-with-claude) .


## **The operator's final checklist**


- Confirm the organization, application, and applicant reference before creating anything.
- List workflows first, then retrieve the selected workflow's configuration or graph before describing its checks.
- Send only the returned hosted` url` through an approved customer channel.
- Ask Claude to report returned evidence, not to infer absent modules or convert a status into a story.
- Treat In Review as a human handoff. Separate investigation, audit note, data correction, and final status into deliberate steps.
- Keep unnecessary personal data, document images, and internal tokens out of the conversation.


The MCP server itself is free. A configured full KYC bundle costs $0.33 and includes identity-document verification, passive liveness, face match, and IP analysis. Each feature includes 500 free verifications per month. Didit serves 2,000+ companies in production and is infrastructure for identity and fraud.


## **Reference links**


- [MCP overview](https://docs.didit.me/integration/mcp/overview) — hosted endpoint and architecture
- [MCP tools documentation](https://docs.didit.me/integration/mcp/tools) — canonical names and schemas
- [Didit MCP on GitHub](https://github.com/didit-protocol/mcp) — public MIT-licensed source
- [Didit MCP developer page](https://didit.me/developers/mcp) — product overview
- [Connect Didit to Claude](https://didit.me/blog/install-mcp-server-claude) — connector setup
