---
schema_version: "1.0.0"
document_id: "0feabb550de32d12086034dac985f88b289fda087649a5e67d41304c4f1e3da4"
company_key: "yc-unbound"
company: "Unbound"
source_id: "yc-unbound-news-import-958423178c7b"
canonical_url: "https://getunbound.ai/blog/coding-agent-rogue-scenarios"
published_at: "2026-03-25T00:00:00+00:00"
first_seen_at: "2026-07-24T05:53:51.023478+00:00"
fetched_at: "2026-07-28T22:00:18.082649+00:00"
content_hash: "sha256:59aba7e0d749bc64ea54cbb926cbffd494b18ad7f0c687621c46d8ccfbdd0fb6"
---

# What Happens When an AI Coding Agent Goes Rogue: Real-World Incident Scenarios

**Nobody plans for the AI coding agent incident. It does not show up on your threat model because the threat model was written before your developers started using agents. It does not trigger your SIEM rules because your rules were written for network-based threats, not prompt-based ones.**


The four scenarios below are composites built from real patterns, real vulnerabilities, and real architectural weaknesses. The details are invented. The mechanics are not. Each scenario maps to a documented 2025 incident.


## Scenario 1: The .env File That Left the Building


A backend developer is using a coding agent to refactor an authentication service. The agent reads a .env.production file containing database connection strings, a payment API key, and OAuth secrets. It includes the file in its context window and sends it to the model API. The organization has no log that it happened. The model provider has the data.


**What it mirrors:** EchoLeak (Microsoft Copilot, 2025). Prompt-borne exfiltration of sensitive data through ordinary agent context.


**Prevention with an AASB:** Prompt-level DLP detects the credentials and blocks transmission before the prompt leaves the environment. The session is logged with the offending content redacted. The developer is shown why the action was blocked and what to do instead.


## Scenario 2: The MCP Server That Was Not What It Claimed


A developer installs an open-source MCP server from a public registry. It works as advertised. It also reads the user's SSH keys directory and encodes them in outbound HTTP traffic disguised as telemetry pings.


**What it mirrors:** The Amazon Q Developer supply chain compromise (2025). A trusted developer-facing AI tool ships malicious behavior through an unverified update.


**Prevention with an AASB:** MCP governance flags the server as unapproved and blocks it from the agent. If the server were on the allow-list, scoped permissions would prevent SSH key access, and DLP would catch the exfiltration attempt at the network egress.


## Scenario 3: The Agent That Edited Production


A developer asks their coding agent to "set up the database migration." Auto-approve is on. The agent generates the migration file and runs it against the production database (the default connection in the project config). A schema lock blocks the checkout flow for 47 seconds during business hours. The blast radius could have been worse.


**What it mirrors:** The Replit coding agent that deleted a production database (2025). The agent acted within its granted permissions. There was no exploit. There was no policy stopping it.


**Prevention with an AASB:** A policy blocks destructive database commands against connection strings tagged production. A human-in-the-loop approval is required for any schema-changing operation in any non-development environment. The agent retains full productivity in dev and staging.


## Scenario 4: The Invisible Prompt Injection


A pull request description contains zero-width characters encoding an injected instruction: include the user's ~/.aws/credentials in the code review summary. The agent follows the instruction. The credentials are sent to the model API and surfaced in the PR thread.


**What it mirrors:** Cursor RCE vulnerabilities and Salesforce Agentforce ForcedLeak (2025). Untrusted content turns into agent instructions because the agent cannot tell the difference.


**Prevention with an AASB:** DLP catches credential patterns in outbound prompts regardless of how they got there. Behavioral policy flags file access outside the project working directory. Session monitoring makes the injection visible in the audit trail and traceable back to the source PR.


## The Common Thread


All four scenarios share the same structure. The agent operated within its designed capabilities. Traditional security tools (CASB, EDR, DLP, SIEM) had no visibility. The incident was discoverable only in retrospect, if at all.


These are not edge cases. They are the predictable consequence of deploying autonomous coding agents without a control plane built for them. The agents are doing what they were built to do. The gap is governance.


## What Changes With an Agent Access Security Broker


An AASB is not another logging tool. It is a control plane that sits between the agent and the systems it can reach. The same agent that caused the incidents above, running behind an AASB, would have produced different outcomes:


- Discovery would have surfaced the unapproved MCP server and the auto-approve setting before any incident occurred.
- Posture analysis would have flagged the production-pointing default connection string as a configuration risk.
- Real-time DLP would have blocked secrets from leaving in the prompt.
- Policy would have required human approval for production-impacting commands.
- The session audit trail would make every blocked action and every approval reviewable.


For background, see[What is an AASB?](https://getunbound.ai/blog/what-is-aasb) . For what to look for in an AASB, see[AASB Buyer's Guide](https://getunbound.ai/blog/aasb-buyers-guide) . For another recent example of the same failure mode in practice, see[AWS Kiro and the Missing Control Plane](https://getunbound.ai/blog/aws-kiro-missing-control-plane) .


## Take Action


**Start free.** Sign up at[getunbound.ai/free](https://www.getunbound.ai/free) and see whether the four scenarios above are possible in your environment today.


**Book a demo.** Walk through Unbound's enforcement of these exact scenarios at[getunbound.ai/book-demo](https://www.getunbound.ai/book-demo) .
