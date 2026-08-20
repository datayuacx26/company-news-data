---
schema_version: "1.0.0"
document_id: "b6c927a45d0966954633402306f021bd7a742757422ab4485c5aab99b5c9285f"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/how-we-built-an-ai-oncall-engineer"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-21T11:29:48.523536+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:f51e1eb4f84317ee68ee0ee18907f842d6bca82e791e82c9075b5adb08f00c8d"
---

# How we built an AI oncall engineer at Brex

## **We encoded our oncall playbook into an agent. Here's what happened.**


A ticket came in about an export failure on a customer's accounting integration. Before the oncall engineer finished reading the alert, the agent had already posted to Slack: root cause, affected customer, Datadog log evidence, the relevant code path, a suggested next step. The engineer confirmed the findings and acted. Thirty to forty-five minutes of context-gathering took about three.


We'd been working on this for a while. That ticket was when we decided to invest in it seriously.


Most oncall is an engineer reading a ticket, searching Datadog, querying Snowflake, checking the docs, and assembling a timeline of what broke and when. Experienced engineers follow roughly the same sequence every time. We decided to encode it.


## **The agent**


It lives in Slack. When a ticket arrives through an automated workflow or a direct @mention, it launches an investigation using the same tools a human would: Datadog for logs and metrics, Linear for ticket context, Glean for internal documentation, Snowflake for data queries, the codebase for code paths. Each connection goes through MCP, so it's calling the same APIs an engineer would, running the same searches.


The investigation runs inside a Claude SDK session that handles tool orchestration and conversation state. When it finishes, the agent produces a structured report covering severity, root cause, evidence with citations, open assumptions, and a suggested next step. The report goes to Slack and gets saved as a Linear document on the ticket.


By default it's read-only. The agent can search, query, and read, but it can't modify files, push commits, or change ticket states. We enforce this at multiple layers: a user allowlist, a tool allowlist, and an explicit blocklist for destructive operations. If a follow-up question implies write intent ("can you open a PR for this?"), the system detects it and unlocks write tools for that specific reply. The default surface stays small on purpose.


## **The actual hard part**


The engineering problem was giving the agent enough domain knowledge to investigate a ticket it had never seen before.


A general-purpose model knows nothing about our accounting export pipeline, or which Snowflake tables hold reimbursement transaction states, or that when a customer reports a "missing journal entry" the first thing to check is whether the export job actually ran. That knowledge lives in engineers' heads, spread across old Slack threads, runbooks, and the kind of institutional memory that is not always written down.


We needed to make it available to the agent in a form that was structured enough to be useful, and maintainable enough that teams would actually keep it current.


## **The knowledge layer**


We started with an accounting skill: a set of markdown files encoding the accounting team's investigation knowledge, organized into three tiers.


A **routing table** maps symptoms and keywords to runbooks. When a ticket about "export failure" comes in, the routing table points the agent to the right runbook before it does anything else.


A **runbook** covers a scenario, such as export failures, payment processing errors, or balance discrepancies. Each runbook walks through diagnostic checks to narrow the problem, what to try when those are ambiguous, remediation steps with guardrails, and an escalation path that specifies the exact evidence the next team needs to pick it up.


**Reference material** sits at the third tier: Snowflake table schemas, Datadog service names, dashboard links, domain terminology. Runbooks pull it in as needed.


When the agent follows a runbook, it’s running the exact diagnostic steps an experienced engineer would take. When there is no matching runbook, the agent still investigates, but we force it to label its reasoning as assumptions rather than facts. If it cannot find direct evidence, it has to say so. This prevents the agent from filling gaps with confident-sounding guesses and keeps the engineer from acting on information that was never actually verified.


We audited the accounting skill against every ticket from 2025. The agent independently **matched the correct root cause and mitigation on 91% of them** , with seven partial matches and three that needed follow-up. The skill hadn't been tuned against those tickets ahead of time. That hit rate came from encoding the right procedures.


## **The learning loop is a flywheel**


Every report gets saved as a Linear document on the original ticket. This turned out to be more important than we expected.


**Reports are searchable.** When the agent starts a new investigation, one of its first steps is looking for past tickets with similar symptoms. If a previous investigation already found the root cause, the agent picks it up. Institutional knowledge compounds over time instead of evaporating when engineers leave.


**Reports are reviewable.** Engineers see exactly what the agent found, what evidence it cited, what it flagged as assumption versus confirmed fact. The agent shows its work. That's how trust gets built, incrementally.


**Reports are scorable.** We grade them on a rubric: did it follow the runbook, cite specific evidence, distinguish facts from assumptions, identify the right escalation path? Scores feed back into skill improvements.


Every investigation that hits an undocumented scenario is a signal that the skill needs work. Every improved runbook makes future investigations faster. Over time, the Linear reports become a dataset. We review them, spot which scenarios keep coming up without a matching runbook, find the gaps, write new runbooks. The next time that scenario shows up, the agent handles it better.


None of this was planned. Saving reports to Linear was a practical choice that turned into a feedback loop.


## **What we got wrong**


Upgrading the model improved investigation quality less than writing better runbooks. The model is good at following procedures and synthesizing evidence; it's bad at inventing the right investigation strategy from nothing. The knowledge layer is what makes the difference.


We also underestimated structured output. Requiring the agent to produce a YAML report with specific fields (severity, root cause, facts with evidence, assumptions with confirmation steps, unknowns with blocking reasons) sounds rigid. In practice it forces precision and makes the report faster to act on.


The goal was never to be right every time, either. We wanted to hand the engineer a 70% complete investigation instead of a blank page. Even when the root cause hypothesis is wrong, the evidence the agent gathered — log excerpts, query results, code pointers — is usually correct and saves real work. Engineers are used to working with incomplete information. What they don't want is to gather it themselves.


## **What changed**


Oncall still requires an engineer. That didn't change.


What changed is where their time goes. Before, most of a shift was spent gathering context: reading tickets, searching logs, running queries, assembling a picture of what happened. Now the engineer starts with a report that's often correct and always has the raw evidence attached. Follow-up questions go into the thread and the agent picks them up with full context without re-explaining the situation.


The work shifted from figuring out what's going on to deciding what to do about it. Three minutes instead of thirty.
