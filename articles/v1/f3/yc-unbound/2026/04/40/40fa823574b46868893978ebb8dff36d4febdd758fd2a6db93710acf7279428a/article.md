---
schema_version: "1.0.0"
document_id: "40fa823574b46868893978ebb8dff36d4febdd758fd2a6db93710acf7279428a"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/governing-claude-across-surfaces"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-24T05:53:51.023478+00:00"
fetched_at: "2026-07-28T21:56:46.217382+00:00"
content_hash: "sha256:e1ef3e5cea1064d4f36c9421c2a184baeebc6ff7ee7d4aeb45485f32100adb5e"
---

# Governing Claude Across Web, Desktop, and Code: What Security Teams Need to Know

**Claude is not one product. It is a family of surfaces, each with a different architecture, a different data flow model, and a different risk profile. Your developers use Claude Code in the terminal with full file system access and MCP connections. Your knowledge workers use Claude through the web app and desktop app to analyze documents and draft content. Your non-technical teams are starting to use Cowork for agentic knowledge work.**


Each of these surfaces creates security exposure. The exposure is different for each one.


Most organizations treat Claude as a single vendor relationship. From a procurement perspective, that is correct. From a security perspective, it is dangerously incomplete. Governing Claude means governing every surface where it operates, with policies that account for what each surface can access and what actions it can take.


## Claude's Four Surfaces


### Claude Code (CLI / Terminal)


Claude Code is the highest-risk surface, and it is where governance matters most.


Claude Code runs as a command-line tool inside the developer's terminal session. This is not a browser tab or a sandboxed application. It is a process that inherits the full permissions of the shell environment it runs in. Whatever the developer's terminal can access, Claude Code can access.


In practice, that means:


**File system access.** Claude Code can read and write any file the developer's user account has permissions for. That includes source code, configuration files, environment variables, SSH keys, cloud credentials stored in ~/.aws, and anything else on the local or mounted file system.


**Shell command execution.** Claude Code can execute arbitrary terminal commands. It can run scripts, interact with databases through CLI clients, trigger deployments, manage cloud infrastructure through the AWS CLI or gcloud, and interact with any service accessible from the command line.


**MCP connections.** Claude Code has native MCP support. It can connect to MCP servers that expose additional tools and data sources: databases, internal APIs, project management tools, CI/CD systems, monitoring stacks, and anything else with an MCP integration. Each MCP connection extends Claude Code's reach beyond the local machine.


**Direct API communication.** Claude Code communicates directly with Anthropic's API. Unlike Cursor or Windsurf (which route through their own proxy infrastructure), there is no intermediary. That is simpler from a trust-chain perspective, but it means there is no third-party proxy layer where controls could be added.


The combination of these capabilities makes Claude Code the most powerful AI coding agent available. It also has the broadest attack surface.


### Claude Web App


The Claude web app (claude.ai) is the most widely used surface and the one with the most familiar risk profile. Users type prompts, paste content, and upload files. Data flows to Anthropic's API and responses come back.


The risks are primarily data leakage through user input: source code pasted into prompts, internal documents uploaded as attachments, credentials included in context. The web app does not have file system access, cannot execute commands, and cannot connect to MCP servers. Its risk is contained relative to Claude Code, but it has the highest interaction volume and broadest user base.


### Claude Desktop App


The desktop app is functionally similar to the web app but runs as a native application. The key difference: the desktop app supports MCP connections. A user can configure the desktop app to connect to MCP servers, giving Claude access to external tools and data sources.


This means the desktop app's risk profile is not identical to the web app's. A desktop app with MCP connections has a broader data access surface. Security teams need to account for this as MCP adoption grows.


### Claude Cowork


Cowork is Anthropic's agentic knowledge work tool for non-developers. It allows users to delegate multi-step tasks to Claude, including research, analysis, document creation, and workflows that span multiple tools and data sources.


Cowork matters for security teams because it extends agentic AI capabilities beyond the engineering organization. When Cowork connects to enterprise tools (through integrations and MCP), it creates the same categories of risk that Claude Code creates for developers: data access across multiple systems, autonomous action execution, and data flows that traditional security tools cannot monitor.


The user profile is different (business analysts, operations teams, project managers), but the governance requirements are the same: visibility, policy enforcement, and data loss prevention.


## Where Traditional Security Tools Lose Visibility


**Your CASB** might detect connections to claude.ai or api.anthropic.com. It cannot tell you what data was included in the prompts, what files were uploaded, what MCP servers are connected, or what actions Claude took.


**Your DLP** watches files, emails, and cloud storage. When Claude Code reads a .env file and sends it in a prompt, no file was uploaded in the traditional sense. When a knowledge worker pastes customer data into the web app, it is text input in a browser, not a file transfer. Neither path is covered.


**Your EDR** sees processes and network calls. It cannot distinguish a benign code suggestion request from one that included production database credentials.


## Governing Claude with Unbound


Unbound is the first Agent Access Security Broker (AASB). It applies a single control plane across all four Claude surfaces so policy is consistent regardless of which surface a user picks up.


### Claude Code Governance


- Real-time monitoring of Claude Code sessions in the terminal, including the prompts sent, files read, and commands executed
- Policy enforcement on file system access, including blocks on credentials, secrets, and PII
- DLP scanning of prompts before they reach Anthropic's API
- MCP connection governance: allow-listing approved servers, per-tool permissions, and data flow inspection
- Shell command policy by environment (warn or block destructive commands against production-adjacent systems)
- Human-in-the-loop approval for high-risk actions like database migrations, infrastructure changes, and mass file modifications
- Full session audit trail for compliance and incident response


### Web and Desktop App Governance


- Visibility into Claude web and desktop app usage across the organization, including unsanctioned personal accounts
- Detection of sensitive data in prompts and file uploads
- Policy enforcement on data sharing, with warn or block actions on classified content
- MCP governance for the desktop app, with the same allow-list and scope controls applied to Claude Code


### Cowork Governance


- Monitoring of Cowork agent activity and multi-step task execution
- Policy enforcement on data access through Cowork integrations
- DLP for data flowing through Cowork workflows
- Audit trail for multi-step agentic tasks tied to user identity


## Why Unified Governance Matters


The risk of governing each surface independently is policy fragmentation. If Claude Code policies are managed by security engineering, web app policies by IT, and Cowork policies by business operations, you end up with inconsistent controls and gaps at the boundaries.


Unified governance means one policy engine that understands all four surfaces. A rule that says "do not transmit production database credentials to any external API" should apply whether those credentials are read by Claude Code from a config file, pasted into the web app by a developer, or accessed by Cowork through an MCP connection to a secrets manager.


## Practical Steps


**Inventory all Claude usage.** Identify who is using Claude Code, the web app, the desktop app, and Cowork. Include shadow usage.


**Map data exposure by surface.** Claude Code's exposure is bounded by shell permissions and MCP connections. The web app by what users choose to input. The desktop app by both manual input and MCP. Cowork by its tool integrations.


**Define per-surface policies.** Not every surface needs the same restrictions. Claude Code in a production-adjacent environment needs strict controls. The web app for general research needs lighter governance.


**Deploy monitoring before enforcement.** Start in observation mode. Understand the baseline before implementing blocking. The audit-to-warn-to-approve-to-block path produces fewer false positives and better developer trust than day-one blocks.


For the evaluation framework, see[AASB Buyer's Guide](https://getunbound.ai/blog/aasb-buyers-guide) .


For foundational concepts, see[What is an Agent Access Security Broker (AASB)?](https://getunbound.ai/blog/what-is-aasb) .


For MCP-specific guidance, see[MCP Server Governance](https://getunbound.ai/blog/mcp-server-governance) .


## Take Action


**Start free.** Sign up at[getunbound.ai/free](https://www.getunbound.ai/free) and see how Unbound governs Claude across every surface in your environment.


**Book a demo.** Walk through Unbound applying one policy across Claude Code, the web and desktop apps, and Cowork at[getunbound.ai/book-demo](https://www.getunbound.ai/book-demo) .
