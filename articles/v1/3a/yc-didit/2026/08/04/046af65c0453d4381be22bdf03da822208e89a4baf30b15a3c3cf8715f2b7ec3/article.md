---
schema_version: "1.0.0"
document_id: "046af65c0453d4381be22bdf03da822208e89a4baf30b15a3c3cf8715f2b7ec3"
company_key: "yc-didit"
company: "Didit"
source_id: "yc-didit-news-import-d47711ec305c"
canonical_url: "https://didit.me/blog/identity-verification-mcp-claude/"
published_at: "2026-08-03T18:21:24+00:00"
first_seen_at: "2026-08-04T05:44:56.010101+00:00"
fetched_at: "2026-08-04T09:43:50.004226+00:00"
content_hash: "sha256:20116d30f9cf2011720a2d377db50e8e102054f1f66cc3ea10b3afae9c5dd1f4"
---

# The identity verification MCP server for Claude

[Back to blog](https://didit.me/blog/) Blog · August 3, 2026


# The identity verification MCP server for Claude


A security checklist for evaluating Didit’s hosted MCP server for Claude: typed tools, OAuth, role scoping, redaction, and precise action boundaries.


By Didit


·


August 3, 2026 ·


Updated Aug 3, 2026


## **Key takeaways**


- An identity verification Model Context Protocol (MCP) server gives Claude typed tools for real identity and fraud operations; it does not ask the model to invent a verification result.
- Didit’s hosted server exposes 115 tools at` https://mcp.didit.me/mcp` over stateless, POST-only Streamable HTTP (Hypertext Transfer Protocol).
- Access uses OAuth (Open Authorization) 2.1 with PKCE (Proof Key for Code Exchange) and Dynamic Client Registration. There is no application programming interface (API) key mode for the hosted server.
- The server acts as the signed-in user under` didit:management` and` didit:verification` ; existing organization roles continue to define what Claude may do.
- Known credential-bearing responses are redacted and error payloads are sanitized. Tool annotations classify read, write, and destructive behavior; wildcard deletion has a handler-side confirmation check, but the current advertised schema does not expose that confirmation field.
- The MCP connection is free. Usage retains Didit’s published pricing, including a $0.33 full Know Your Customer (KYC) bundle and 500 free verifications per month for each feature.


If you are evaluating an **identity verification MCP server for Claude** , the useful question is not whether Claude can call an endpoint. It is whether the connection gives the artificial intelligence (AI) model enough structured capability to complete real work while preserving authentication, authorization, auditability, and human control. That is where implementations differ.


This guide explains that evaluation model using Didit’s Claude connector as the concrete example. It deliberately does not repeat the click-by-click setup covered in[the Claude installation guide](https://didit.me/blog/install-mcp-server-claude) , the category overview in[the MCP tools reference](https://didit.me/blog/didit-mcp-tools-reference) , or the session sequence in the[KYC session lifecycle guide](https://didit.me/blog/kyc-mcp-server-guide) . The canonical, current schemas live in the documentation and public source.


## **What is an identity verification MCP server?**


MCP is a protocol for giving a model tools. An MCP server publishes named operations with descriptions and typed input schemas. An MCP client such as Claude discovers those operations, lets the model select one, validates its arguments, and carries the result back into the conversation.


An identity verification MCP server applies that pattern to regulated identity and fraud work. Instead of responding from general knowledge when asked to verify a customer, Claude can create a real verification session, retrieve its decision, run a screening check, or inspect an organization’s configured workflows. The returned data comes from the connected service, not from the model’s memory.


That distinction is essential. MCP does not make a language model an identity authority, and it does not transfer compliance responsibility to the model. It gives the model a governed route to the system that performs the checks and records the outcomes. The verification provider remains responsible for the service; the customer remains responsible for policy and review; Claude coordinates the permitted operations.


> **A useful definition:** an identity verification MCP server is an authorization-aware adapter that exposes identity and fraud capabilities as typed tools an AI client can discover and invoke.


## **What Didit’s server enables inside Claude**


Didit is infrastructure for identity and fraud. Its hosted MCP catalog gives Claude 115 tools spanning 19 machine-level domains. The point is not the count alone; it is the range of work Claude can connect across one authenticated session.


A normal interaction starts with` didit_context_get` , which returns the organizations and applications the user can access. Claude can then choose tools that match the task:


- ` didit_session_create` creates a verification session from a configured workflow, while` didit_session_get_decision` retrieves the resulting decision.
- ` didit_verify_id` ,` didit_verify_passive_liveness` , and` didit_verify_face_match` run focused document and biometric checks when their required image files exist on the MCP server's filesystem.
- ` didit_verify_aml` runs Anti-Money Laundering (AML) screening.
- ` didit_verify_kyb_search` and` didit_verify_kyb_select` support Know Your Business (KYB) registry discovery and record selection.
- ` didit_transaction_create` records monitored activity, and` didit_transaction_screen_wallet` performs Know Your Transaction (KYT) wallet screening.
- ` didit_case_create` opens an investigation case, while` didit_case_manage` supports assignment, comments, escalation, reopening, resolution, and field updates.
- ` didit_workflow_create` and` didit_workflow_edit_graph` let authorized users compose verification flows;` didit_webhook_create` connects resulting events to downstream systems.


These are examples, not a substitute for the canonical[MCP tool documentation](https://docs.didit.me/integration/mcp/tools) . The current server exposes tools only: it does not publish MCP resources or prompt templates. Specialized compliance configuration and statutory reporting remain governed workflows in the Business Console rather than autonomous chat actions.


## **The hosted-Claude image boundary**


Five image tools —` didit_verify_id` ,` didit_verify_age` ,` didit_verify_face_match` ,` didit_verify_passive_liveness` , and` didit_lists_entry_upload_face` — accept absolute path inputs that their handlers read from the MCP server's own filesystem.


An image uploaded into hosted Claude is therefore not ordinarily available to these tools: the connector exposes no file-staging tool. Seeing an image in chat is not the same as providing a readable` front_image_path` . The tools are practical in local or self-hosted deployments where files can be placed on the server filesystem.


For a real applicant using hosted Claude, use` didit_session_create` , send the returned` url` , then retrieve the result with` didit_session_get_decision` . The applicant captures the configured evidence in Didit's hosted experience; Claude does not stage the image.


## **The authentication boundary to evaluate**


The hosted endpoint is` https://mcp.didit.me/mcp` , using stateless, POST-only Streamable HTTP. Claude connects through OAuth (Open Authorization) 2.1 with PKCE (Proof Key for Code Exchange) and Dynamic Client Registration; the hosted server has no API-key mode.


The material security property is the resulting identity. Calls run as the signed-in Didit user under` didit:verification` and` didit:management` , while the organization’s backend role still determines which operations succeed. A Reader does not become an administrator because Claude selected a write tool.


For evaluation, confirm that access can be revoked without rotating a production application credential, that actions remain attributable to a user, and that multi-organization context is explicit. The exact discovery, consent, and setup sequence belongs in the[Claude installation guide](https://didit.me/blog/install-mcp-server-claude) and[authentication documentation](https://docs.didit.me/integration/mcp/authentication) .


## **Safety is more than authentication**


Authentication answers who is calling. A production-grade MCP server must also control what the model sees and how risky actions proceed.


Didit marks tools with read-only, write, destructive, idempotent, and open-world annotations. A client can use those signals to group or label operations, but an annotation is descriptive metadata. It does not automatically force the model or server to request confirmation.


The confirmation rule is narrow.` didit_session_delete` permanently deletes one session and requires only` session_id` . Bounded batch deletion with an explicit identifier list also does not require a confirmation field. The handler for a **wildcard deletion** rejects` delete_all: true` unless` confirm: true` is also supplied, but the current advertised batch-delete input schema omits` confirm` . Treat that as a handler-side backstop with a schema gap, not as a complete client-visible approval flow. Teams should add their own human-approval policy for consequential writes instead of assuming an annotation enforces one.


Known credential-bearing outputs are handled deliberately: application credential fields and webhook signing-secret metadata are redacted, while error payloads are deeply sanitized before returning to the client. That is not a promise that every field in every successful business response is globally removed, so teams should still minimize the personal data they ask Claude to retrieve. The credential-reveal operation exists only in the full local/stdio catalogue, requires its own confirmation, and is excluded from the 115-tool hosted OAuth catalogue. The credit top-up operation is excluded from that hosted catalogue as well.


This layered model is preferable to relying on a system prompt that merely tells an agent to “be careful,” but its boundaries must be stated precisely. Backend roles, input validation, targeted redaction, error sanitization, and the wildcard-delete handler check are enforcement. Risk annotations and chat instructions inform behavior, and the current wildcard-confirmation schema gap belongs on an evaluator's checklist.


## **How to evaluate an MCP server for identity work**


Before connecting any identity or fraud service to Claude, verify the following:


- **Transport:** Is there a documented remote endpoint using a current MCP transport?
- **Authentication:** Does access represent a user through OAuth, or does it depend on a broadly privileged credential copied into configuration?
- **Authorization:** Are organization roles enforced by the backend on every call?
- **Schemas:** Do tools define constrained inputs, allowed actions, and useful errors?
- **Risk metadata:** Can the client distinguish reads, writes, destructive operations, and calls that affect external systems?
- **Data handling:** Are secrets and unnecessary personal data redacted from tool results and errors?
- **Boundaries:** Does the provider state what the model cannot do and where human compliance review remains required?
- **Inspectability:** Can your team review the source and a maintained tool reference?


Didit publishes its implementation in the[public MIT-licensed GitHub repository](https://github.com/didit-protocol/mcp) and documents the architecture in the[MCP overview](https://docs.didit.me/integration/mcp/overview) and[authentication guide](https://docs.didit.me/integration/mcp/authentication) . The v5 codebase is marked private for package publication and is not distributed through npm. For Claude, the intended path is the hosted endpoint and its browser-based authorization flow.


## **When the Claude connector is a good fit**


The connector is strongest when a human wants Claude to investigate, coordinate, or execute bounded operations across an existing Didit workspace: review recent decisions, create a hosted verification link, run non-image screenings, inspect a case queue, compare workflows, or summarize activity across applications. It is also useful for developers exploring schemas before implementing a backend integration. Standalone image checks need the server-side file access described above.


It is not a replacement for deterministic production code where your application must trigger the same operation on every request without a conversational user. In that case, use Didit’s Representational State Transfer (REST) APIs and software development kits. MCP and REST serve different callers: one delegates a signed-in person’s work to an AI client; the other connects application logic directly to the service.


The economics are the same whichever interface initiates a check. The MCP server itself is free. A full KYC bundle — identity-document verification, passive liveness, face match, and Internet Protocol (IP) analysis — is $0.33. Each feature includes 500 free verifications per month. Didit supports 2,000+ companies in production across 220+ countries and territories, 14,000+ document types, and 48+ languages.


## **Connect Didit to Claude**


If the authorization and safety model fits your use case,[add the Didit custom connector to Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Didit&connectorUrl=https%3A%2F%2Fmcp.didit.me%2Fmcp) . The connector points to the hosted Streamable HTTP endpoint and starts the Didit sign-in flow.


For exact Claude Desktop and Claude Code steps, use the[dedicated installation guide](https://didit.me/blog/install-mcp-server-claude) . For product context and further examples, visit the[Didit MCP developer page](https://didit.me/developers/mcp) .


The concise verdict is this: an identity verification MCP server is worth using with Claude when it turns identity and fraud operations into typed, permission-aware actions without weakening the controls around them. The tool count makes the connection useful; OAuth, role enforcement, redaction, precise schemas, and narrowly enforced wildcard-delete checks make its risk model inspectable.
