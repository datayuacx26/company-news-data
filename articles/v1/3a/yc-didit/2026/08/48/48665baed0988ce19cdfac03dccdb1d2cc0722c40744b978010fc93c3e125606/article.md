---
schema_version: "1.0.0"
document_id: "48665baed0988ce19cdfac03dccdb1d2cc0722c40744b978010fc93c3e125606"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/kyc-with-claude/"
published_at: "2026-08-03T18:21:24+00:00"
first_seen_at: "2026-08-04T05:44:56.010101+00:00"
fetched_at: "2026-08-04T09:43:50.004226+00:00"
content_hash: "sha256:0e60923913b2dfcba7ee166b6688e6df8f737d4516ed754522e0b45cd04df711"
---

# KYC with Claude: Running Know Your Customer checks in chat

[Back to blog](https://didit.me/blog/) Blog · August 3, 2026


# KYC with Claude: Running Know Your Customer checks in chat


How compliance analysts use Didit's MCP server in Claude to review KYC sessions, correct extracted data, approve or decline verifications, and manage the review queue — all from chat.


By Didit


·


August 3, 2026 ·


Updated Aug 3, 2026


## **Key takeaways**


- Connect Claude to Didit's Model Context Protocol (MCP) server through OAuth (Open Authorization) 2.1 with PKCE (Proof Key for Code Exchange) — no application programming interface (API) key needed, and your existing Didit role and permissions carry over exactly.
- 115 tools across 11 categories let you search, inspect, correct, review, approve, and decline Know Your Customer (KYC) sessions entirely from the Claude chat window.
- A compliance analyst can work the full In Review queue: inspect decisions, override mis-read fields, leave audit-trail notes, approve or decline, and request partial resubmission — all without opening the Business Console.
- The MCP acts as the signed-in Didit user with that user's exact organization role — an analyst cannot do anything in Claude they cannot do in the console. This directly answers the compliance-officer question: permissions are not bypassed.
- Didit serves 2,000+ companies in production; model inference runs at sub-2s p99. The full KYC bundle is $0.33, and each feature includes 500 free verifications per month.


KYC verification is a daily workflow for compliance teams. Sessions arrive flagged for manual review. Documents get scanned, data gets extracted, and some percentage always falls to a human reviewer to sort out. That reviewer typically spends their day toggling between a Business Console and a queue management tool, clicking through session after session.


There is a faster way. With Didit's Model Context Protocol (MCP) server connected to Claude, a compliance analyst can work the entire review queue from a single chat window. Search for in-review sessions, read why each was flagged, inspect the full decision object, correct a mis-read surname or date of birth, leave a reviewer note, approve or decline with a complete audit trail, and request resubmission of just the failed steps — all without leaving the conversation.


## **How it works**


Connector setup and the OAuth flow are covered in the[Claude installation guide](https://didit.me/blog/install-mcp-server-claude) . For daily review, the important boundary is simple: each tool call runs with the signed-in Didit user's existing organization role, so Claude cannot approve or edit a session when that user lacks the corresponding permission.


[Add the Didit connector to Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Didit&connectorUrl=https%3A%2F%2Fmcp.didit.me%2Fmcp) . The implementation and self-hosting code are available in the public[GitHub repository](https://github.com/didit-protocol/mcp) under the MIT license.


## **The daily KYC review queue in Claude**


The MCP server exposes **115 tools** across 11 categories. For a compliance analyst reviewing KYC sessions, the relevant workflow looks like this:


### **1. Discover your workspace**


Start with` didit_context_get` . This single call returns every organization and application you can access, including which org and app are the default. It replaces the older multi-step discovery pattern and lets you start working immediately.


```text
Call didit_context_get. State the selected organization and application, and do not read or change any session yet.
```


### **2. Find sessions needing review**


Run` didit_session_search` with` status: "In Review"` . This searches across all your apps and organizations in one call and returns sessions tagged with their org and app, newest first. You can filter by date range using` last_n_days` or drill into a specific workflow with` workflow_id` .


```text
Find sessions with status “In Review” using last_n_days: 1. State the date_from and date_to boundaries, then return session_id, workflow, created time, and any observed review reason. Do not call write tools.
```


` last_n_days` is a calendar-date shortcut: it sets` date_from` and` date_to` . It is not a rolling 24-hour filter. Pass explicit` YYYY-MM-DD` dates when your review policy requires a different boundary.


### **3. Inspect the full decision**


For any flagged session, call` didit_session_get_decision` with its session identifier. This returns the full decision and extracted data produced by that session's configured workflow. Depending on the modules in that workflow, the response may include identity-document fields, liveness and face-match results, Anti-Money Laundering (AML) screening output, fraud signals, and review evidence. Do not expect modules that the workflow did not run.


```text
Retrieve the full decision for session SESSION_UUID. Separate observed fields, failed checks, conflicting evidence, and missing data. Do not recommend a status yet.
```


### **4. Correct mis-read data**


OCR (Optical Character Recognition) can mis-read a character — a "0" that should be "O", an accented letter that OCR flattens, a date format mismatch. Use` didit_session_update_data` to override any extracted field: first name, last name, date of birth, document number, issuing state, address, gender, nationality, marital status, and document-specific extra fields. Only send the fields that need correcting; everything else stays as extracted.


```text
For session SESSION_UUID, change only last_name to “Muñoz”. Show the proposed field update and wait for my approval before calling didit_session_update_data.
```


### **5. Leave an audit-trail note**


Use` didit_session_add_review` to attach a reviewer comment to the session's audit trail. You can optionally change the session status as part of the same call — for example, move it to "In Review" if you are actively working it, or to "Approved" if your inspection is complete. Every note and status transition is logged and timestamped.


```text
Add this review comment to session SESSION_UUID without changing its status: “Surname corrected after comparison with the document visual zone.”
```


### **6. Approve, decline, or request resubmission**


When the review is conclusive, use` didit_session_update_status` to set the final status to` Approved` or` Declined` , with an optional comment and email notification. Use a prompt that makes the authorized decision explicit:


```text
Update session SESSION_UUID to Approved with the comment “Manual review completed under policy v4.2.” Do not change extracted identity data.
```


Partial resubmission is stricter than a conversational label.` nodes_to_resubmit` must contain the **exact failed node identifiers** returned for that session. Values such as` liveness` or` blurred document page` are descriptions, not runnable node identifiers. First ask:


```text
From the decision for session SESSION_UUID, list the exact failed node identifiers that are eligible for resubmission. Do not change the session.
```


After reviewing those identifiers, use them verbatim:


```text
Set session SESSION_UUID to Resubmitted and pass these exact nodes_to_resubmit values: ["EXACT_NODE_ID_1", "EXACT_NODE_ID_2"]. Add the comment “Retry only the failed configured steps.”
```


The whole workflow — find, inspect, correct, note, decide — happens inside Claude. Searches and reads do not create reviewer audit entries. The write tools have distinct effects:` didit_session_update_data` applies a correction,` didit_session_add_review` creates a reviewer note, and` didit_session_update_status` records a status change with an optional audit-trail comment.


## **The 10 session statuses and what they mean**


When searching or reviewing sessions, you filter by status. Didit tracks 10 statuses across the session lifecycle:


- **Not Started** — The session was created and the verification link was generated, but the user has not opened it yet.
- **In Progress** — The user opened the verification flow and is actively completing steps.
- **In Review** — The session is currently queued for human review. It may have arrived there through configured workflow logic or an authorized reviewer's manual status change.
- **Approved** — The session's current decision state is approved. That state may come from the configured workflow or from an authorized reviewer's manual status override; it does not prove that every possible check ran or passed.
- **Declined** — The session's current decision state is declined. It may reflect configured workflow logic or an authorized reviewer's manual status override, so read the returned evidence instead of treating the label as a list of failed checks.
- **Expired** — The session's time window elapsed before the user completed verification.
- **Abandoned** — The user started but did not finish the flow.
- **Kyc Expired** — The KYC data itself aged out (e.g., an expired identity document was detected post-verification).
- **Resubmitted** — The analyst requested partial resubmission, and the user was prompted to retake only the failed steps.
- **Awaiting User** — A Know Your Business (KYB) parent session is waiting while required child KYC parties complete their verification.


## **Permissions and security**


The compliance-officer objection to any artificial intelligence (AI)-connected tool is straightforward: can an agent do something in chat that a human reviewer would not be allowed to do in the console? With Didit's MCP server, the answer is no. The MCP authenticates as the signed-in Didit user through OAuth 2.1 with PKCE — every tool call inherits that user's organization role. The Business Console and the MCP server enforce the same privilege checks against the same permission backend in` service-didit-auth` .


An analyst can review sessions, correct data, and approve or decline only within the scope their role already grants. A developer connecting the MCP can create workflows and manage webhooks if their role allows it. The MCP itself does not introduce new permissions. It is a different interface into the same authorization model.


## **Who this is for**


This workflow is designed for compliance analysts who already know how to review a KYC session. The MCP does not automate the reviewer's judgment — it removes the context switching. Instead of opening a browser, logging into the console, finding the right session page, clicking through tabs, and typing into forms, the analyst describes what they want in natural language and Claude executes the sequence of tools.


It is also useful for compliance leads who want to spot-check the review queue from their phone, and for onboarding new analysts who can learn the review process by watching Claude walk through a session's decision data and flag patterns.


## **When to use the console instead**


Some actions remain in the Business Console. The MCP server in Claude handles per-session review and management actions. For bulk operations like installing rule bundles, testing rule changes, or filing a Suspicious Activity Report (SAR), those features live in the console's Transaction Monitoring and Case Management interfaces. The MCP's case-management tools handle triage —` didit_case_manage` supports assign, comment, escalate, reopen, resolve, and update — but SAR filing and rule-engine configuration are console-only workflows.


## **Get started**


Connect Claude to Didit's MCP server from your[Claude connector settings](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Didit&connectorUrl=https%3A%2F%2Fmcp.didit.me%2Fmcp) . The server is free, the hosted endpoint requires no installation, and the full tool reference is on[docs.didit.me](https://docs.didit.me/integration/mcp/tools) .


If you are new to the MCP setup, start with the[installation guide](https://docs.didit.me/integration/mcp/installation) or the[Didit MCP developer page](https://didit.me/developers/mcp) . For an overview of the session lifecycle and what happens when a verification runs, read[KYC with the Didit MCP server](https://didit.me/blog/mcp-kyc-identity-verification) . For the tool catalog, see the[MCP tools reference](https://didit.me/blog/didit-mcp-tools-reference) .


Didit is infrastructure for identity and fraud. 115 MCP tools. $0.33 for a full KYC bundle. 500 free verifications per month for each feature. 2,000+ companies in production. Connect Claude, sign in, and start reviewing.
