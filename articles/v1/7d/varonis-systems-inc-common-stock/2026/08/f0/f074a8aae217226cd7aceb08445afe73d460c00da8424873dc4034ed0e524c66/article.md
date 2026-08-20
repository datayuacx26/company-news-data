---
schema_version: "1.0.0"
document_id: "f074a8aae217226cd7aceb08445afe73d460c00da8424873dc4034ed0e524c66"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/varonis-atlas-claude-inference-hooks-integration"
published_at: "2026-08-07T23:47:27+00:00"
first_seen_at: "2026-08-08T01:00:31.152082+00:00"
fetched_at: "2026-08-08T01:00:32.788098+00:00"
content_hash: "sha256:71db86721fdfafed2097f361aaac376741ccf6fd3dd55d5cd116512e5208c993"
---

# Varonis Atlas Now Integrates with Claude Inference Hooks to Extend Real-Time AI Data Protection

[Varonis Atlas](https://www.varonis.com/platform/ai-security?hsLang=en) now integrates with[Claude Inference hooks,](https://platform.claude.com/docs/en/manage-claude/inference-hooks) routing prompts through an AI security server for real-time allow-or-deny verdicts before inference runs.


That inspection happens ahead of Claude ever seeing the prompt — the request is sent off for review and policy evaluation first. If it violates policy, it never reaches the model.


Support for Inference hooks helps security teams prevent sensitive data exposure, prompt injection attempts, and other risky activity.


As Anthropic continues to expand the Claude ecosystem, AI security platforms have to move quickly to keep data secure.


Just a few weeks ago, we


[brought Atlas coverage](https://www.varonis.com/blog/claude-coverage?hsLang=en) to the entire Claude enterprise suite, including Claude Enterprise, Claude Platform, Claude Code, and Claude Cowork.


Atlas has also enhanced support for Claude Chat and Claude Design.


## How Atlas prevents prompts that fall outside policy


Atlas now sits in the request path itself. When a user submits a prompt, Anthropic sends the conversation transcript to Atlas, which evaluates it against an expansive set of customizable policies, including PII exposure, malicious URL, and prompt injection. Atlas then returns a verdict — allow or deny— before inference proceeds. A denied prompt never reaches Claude at all.


## How Atlas enforces the verdict


Because Atlas understands sensitivity, permissions, and access across an organization's data, prompts aren't evaluated in a vacuum. That context determines the risk and informs an appropriate response. For example, a prompt asking Claude to summarize a spreadsheet depends on whether the spreadsheet contains public marketing copy or unmasked customer PII.


With Inference hooks, that context now drives inline decisions:


Leveraging Claude’s Inference hooks, Varonis Atlas inspects prompts, including attachments, to take action and prevent misuse and data exposure.


AI runtime guardrails.


Atlas inspects the transcript and attachment text on every prompt and takes action in real time, including denying requests that would expose regulated, classified, or sensitive data before a response is generated.


Varonis Atlas enforces consistent policies and guardrails across the Anthropic enterprise suite.


A consistent verdict, everywhere Claude runs.


Atlas enforces consistent policies and guardrails across the entire Claude enterprise suite, including Claude Chat, Claude Design, Claude Enterprise, Claude Platform, Claude Cowork, and Claude Code.


Varonis Atlas created a record of every prompt, response, and tool execution across the entire session.


Complete audit trail.


Atlas creates a record of every prompt and response alongside the actions it took, leveraging Inference hooks to provide an intuitive audit trail for security, governance, and compliance teams.


##


Complete security for the Claude enterprise suite


Most AI security solutions tell you which AI systems exist, not whether data is at risk. Varonis Atlas connects AI risk to data — where the damage happens. That same context now applies to every Claude interaction, from a governed prompt in Claude Chat to a multi-step Claude Cowork task to an agent running in Claude Code.


With Inference hooks, that connected view extends into the request path itself, including posture management and security testing before an AI system goes live, runtime guardrails and inline enforcement, and compliance reporting.


[AI security](https://www.varonis.com/blog/ai-security?hsLang=en) cannot live in silos or point solutions. Atlas support for Claude is one piece of an end-to-end approach to AI security. As organizations scale AI, they also increase exposure. The only way forward is security that understands both how AI behaves and what data it can access.[AI isn’t the risk, uncontrolled AI is.](https://www.varonis.com/blog/securing-ai?hsLang=en)
