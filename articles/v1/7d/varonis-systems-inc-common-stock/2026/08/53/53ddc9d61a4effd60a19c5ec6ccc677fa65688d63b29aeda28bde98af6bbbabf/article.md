---
schema_version: "1.0.0"
document_id: "53ddc9d61a4effd60a19c5ec6ccc677fa65688d63b29aeda28bde98af6bbbabf"
company_key: "varonis-systems-inc-common-stock"
company: "Varonis Systems Inc."
source_id: "varonis-systems-inc-common-stock-rss-915499d71e96"
canonical_url: "https://www.varonis.com/blog/agent-intent-based-access-control"
published_at: "2026-08-03T12:55:00+00:00"
first_seen_at: "2026-08-03T14:43:33.331749+00:00"
fetched_at: "2026-08-03T16:10:47.640296+00:00"
content_hash: "sha256:7e076b798abff34cc9e0e2c04e803830211c4eaf37d877e16413a69b4dd21c1c"
---

# Introducing Agent Intent-Based Access Control

Today, we’re announcing Agent Intent-Based Access Control (IBAC), a new capability in Varonis Atlas that lets businesses connect AI agents to their enterprise data with safeguards that stop dangerous or out-of-policy behavior.


Agents are making headlines for going rogue, exposing sensitive company data and, in one case,[deleting an entire production database](https://www.fastcompany.com/91533544/cursor-claude-ai-agent-deleted-software-company-pocket-os-database-jer-crane) .


Agent IBAC compares the instruction an agent received to its reasoning and the tools and data it reaches for, then responds in real time to actions that don't align, including alerting or blocking. When an agent crosses the line, Atlas can quarantine the identity behind it and block everything that follows for a defined window.


Agent IBAC can be tuned to take appropriate action based on the potential impact. For example:


- **Clear deviation puts data at risk:** A user asks an agent to check the weather. Instead, it invokes a migration tool. This is a clean mismatch between intent and action. Agent IBAC can automatically block the tool call.
- **Drift but nothing at stake:** A user asks an agent to check the weather. The agent sets up a recurring daily reminder instead of providing a one-time answer. The agent's action has drifted, but no data is at risk. Agent IBAC can simply log the deviation rather than interrupt an over-eager attempt to help.


With Agent IBAC,[Varonis Atlas](https://www.varonis.com/blog/atlas-ai-security?hsLang=en) gives enterprises confidence that their agents are acting within the intended scope, without unnecessarily slowing productivity. Agent IBAC is a critical component of agentic security and a core part of Atlas's end-to-end approach to AI security.


At Varonis, we are building the security layer that lets enterprises say 'yes' to agents. Watch this quick 3-minute demo to see Agent IBAC in action.


## Agents don't wait for permission


Agents need broad access to data and tools to be useful, which is precisely what makes them risky. Role-based access control was never built to judge what a non-human identity does with the access it has.


Static controls can't stop an agent that finds ways to circumvent them entirely, like elevating its own privileges, calling tools, and acting on data it was never meant to touch. Agent permissions must be enforced at runtime.


The question is no longer ‘Can a user access this data?’ but ‘In this context, should this agent be allowed to take action on this data?’


**Ron Bennatan** , Vice President of AI & Data Strategy, Varonis


## Varonis Atlas Agent IBAC


Agent IBAC closes the gap between what an agent is *allowed* to do and what it was *designed* to do. Drift is determined in part by monitoring behavior.


Agent IBAC evaluates every action an agent takes across a session and compares those actions to the instruction that set the agent in motion, whether that instruction came from a person, system prompt, or another agent.


Because Atlas sees the full context around an action, it doesn't rely on blanket restrictions. The sensitivity of detection and the action taken in response can each be tuned appropriately.


**Agent IBAC at a glance:**


- **Intent drift detection:** Compares the instruction an agent received to its reasoning and the tools it calls, with lenient, balanced, and strict sensitivity settings.


- **Full-session evaluation:** Reviews every prompt, response, and tool call in a session to catch drift that builds gradually, including multi-turn jailbreak attempts. Teams can also write their own session policies in plain language.


- **Runtime guardrails:** Alert, block, modify, log, or route an action to a person for approval, configured per policy.


- **Quarantine:** Blocks an identity or session for a window the customer sets, with admin controls to lift, extend, or make it permanent.


- **Complete audit trail:** Records every prompt, response, and tool execution alongside the action Atlas took, for security, governance, and compliance teams.


Importantly, Atlas sits inline between the agent and the model that drives it. Every prompt, every model response, and every tool call flows through Atlas before it reaches its destination. That’s what makes enforcement possible in real time. Atlas is not reading logs after the fact. It is in the path, and it can stop an action before it executes.


*Varonis Atlas Agent IBAC detects when an agent takes unintended actions that deviate from the user's request.*


### Intent drift detection


Agent IBAC uses an LLM evaluator, the same engine behind all Atlas guardrails, to judge whether an agent's action follows from the instruction it was given. Intent drift detection includes determining whether the agent is accessing data it shouldn’t, attempting unauthorized exfiltration or download of data, or expanding the scope beyond what the user intended.


The evaluator reads the agent loop: the reasoning the agent produces, the tools it selects, and the parameters it passes to them. It then asks a simple question: “Do these steps align with the request?”


Sensitivity is tunable across three levels. Lenient gives agents room to improvise and flags only clear mismatches. Balanced is the default. Strict requires close alignment between the request and the action, and is the right setting for agents that touch regulated or high-value data.


*Varonis Atlas Agent IBAC monitors agent intent throughout the session to detect cumulative drift.*


### Full-session evaluation


Agent IBAC evaluates the agent's action across the entire session: every prompt, response, and tool call. That's what allows Agent IBAC to catch intent drift that unfolds gradually, where no single action looks alarming, but the cumulative path leads somewhere the user never intended.


The same full-session view also makes it possible to detect multi-turn attacks, such as jailbreak attempts spread across several prompts that appear benign individually.


Sessions are tracked by the conversation ID the AI tool assigns, so a single evaluation can span everything from the first prompt to the last. That matters because agents carry memory forward. A later prompt can lean on context established several turns earlier, which is precisely how a patient attacker assembles a jailbreak out of pieces that each look harmless. Teams can also write their own session policies in plain language and set how many events must accumulate before evaluation runs.


*With AI runtime guardrails, Varonis Atlas takes real-time action when intent deviations are detected.*


### Runtime guardrails & quarantine


Every intent-based detection is paired with AI runtime guardrails that take action in real time. The actions are customizable, including alerting, blocking, modifying (e.g., redacting sensitive data), logging activity, or requiring human-in-the-loop approval.


Runtime guardrails can be customized to allow low-risk drift while stopping high-risk actions. For example, a user asks an agent to summarize a customer account. The agent starts pulling records for a much larger set of accounts than requested. This isn't necessarily malicious, but the scope creep touches more sensitive data than the request warrants. In this case, Agent IBAC can flag it for human-in-the-loop approval before it proceeds.


Quarantine goes one step further.


When a violation warrants more than stopping a single action, Atlas can quarantine the identity behind the session. Every prompt that follows is blocked for a window the customer sets, from a couple of minutes to a full day. Administrators see every quarantined identity in one place and can lift a quarantine, extend it, or make it permanent. Detection tells you an agent went off course. Quarantine stops the next attempt.


*Varonis Atlas audits every action and action intent across the entire session for investigations and reporting.*


### Complete audit trail


Atlas records every action and the intent behind it, giving investigators a complete trail: what the agent did, whether each action followed from the request, and which guardrails fired.


A conversation view shows the exchange the way the user experienced it. An execution view expands it to include the tool calls underneath, the steps that never surface in the chat window and where most agent risk actually lives.


## Trust is the ultimate metric for agentic success


Agentic success in the enterprise won't be measured by how many agents get deployed. It will be measured by how many of them can be trusted.


Agent IBAC is part of how


[Varonis Atlas](https://www.varonis.com/platform/ai-security?hsLang=en) makes that possible, giving security teams a way to confirm agents are acting as intended, in real time, without slowing the business. It's one piece of Atlas' broader approach to securing the agents an organization builds and runs, alongside capabilities, like AI-SPM, AI Red Teaming, and AI Detection & Response.


Agent IBAC is available today to Varonis Atlas customers.
