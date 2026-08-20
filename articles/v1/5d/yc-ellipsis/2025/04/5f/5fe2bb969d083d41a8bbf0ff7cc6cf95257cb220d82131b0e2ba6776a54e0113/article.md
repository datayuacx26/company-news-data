---
schema_version: "1.0.0"
document_id: "5fe2bb969d083d41a8bbf0ff7cc6cf95257cb220d82131b0e2ba6776a54e0113"
company_key: "yc-ellipsis"
company: "Ellipsis"
source_id: "yc-ellipsis-news-import-97656c4ed8a9"
canonical_url: "https://www.ellipsis.dev/blog/the-future-of-software-is-the-agent"
published_at: "2025-04-25T00:00:00+00:00"
first_seen_at: "2026-07-24T05:57:47.052865+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:864a6925777c0f0e1a1f3809d11c7e3df0e85e6059a727a4d432e55ac9a1d1df"
---

# The Future of Software Is the Agent

## The shift is from prompts to systems


AI can write code. The larger change is that software work no longer has to begin with a person opening a chat window. A team can define the work once, then run it whenever the right event occurs.


That is an agent. In Ellipsis, an agent is a YAML file in your repository. The file declares its instructions, trigger, repositories, permissions, and budget. When the agent starts, Ellipsis creates a session in an isolated cloud sandbox with the code and tools it needs.


The distinction matters. The agent is the durable definition. A session is the work created from that definition; on a conversational surface, it can span multiple turns and executions. A trigger decides when new work arrives. A sandbox is where each execution happens. Treating these as separate parts turns a useful prompt into infrastructure a team can review and govern.


## Agents belong in the repository


Agent behavior should change through the same process as production code. Put the definition under` agents/` , review it in a pull request, and deploy it by merging to the default branch. The live configuration is always visible in git.


This makes an agent a shared team asset. Engineers can see what work it owns, which repositories it can access, and how much it may spend. Every change has an author and a diff. An invalid edit records a sync error while the last valid configuration stays live.


Infrastructure followed the same path. Teams stopped treating server setup as a sequence of commands and started declaring the desired state in code. Agents apply that model to software work: define the job, deploy the definition, and inspect what each session did.


## Triggers turn intent into routine


A trigger connects an agent to the moment its work becomes useful. Cron triggers start scheduled work. React triggers respond to events from GitHub, Linear, Slack, or Sentry. Mention triggers let a person call an agent from a GitHub, Slack, or Linear conversation. Teams can also start sessions on demand from the dashboard, API, or CLI.


The instructions stay with the agent while the trigger supplies the immediate context. A review agent receives the pull request that changed. A Sentry agent receives the alert that fired. A weekly report agent reads the current repositories at the scheduled time. On conversational surfaces, later events can continue the same thread instead of starting without context.


## Engineers move to the control points


Software engineers do not disappear. Their attention moves. Agents take the repetitive work. Engineers spend their time on decisions.


- **Define the job.** Decide what evidence the agent must read, what output it should produce, and when it should stop.
- **Set the boundary.** Choose its repositories, scope its GitHub token, and cap each session with a budget.
- **Review the consequence.** Let the agent prepare the pull request, diagnosis, report, or review. Keep a person at the checkpoint where judgment changes the outcome.
- **Improve the definition.** Read the session record, find where the instructions were weak, and update the agent in git.


Code review, incident diagnosis, documentation maintenance, and team reports all fit this pattern. The agent handles the repeatable pass. The engineer owns the standard and the consequential decision.


## A cloud platform makes agents safe to scale


One script can perform one job. A team of agents needs a common control plane. Otherwise permissions, execution environments, spend, and history fragment across one-off tools.


- **Isolated sandboxes** give every session its own working tree and toolchain, so parallel work does not contaminate another session or a developer's laptop.
- **Scoped credentials** restrict what the session can read or change. GitHub enforces the repository and permission scope on the token itself.
- **Hard budgets** cap each session and the agent's trailing spend. A session blocked by a trailing limit never creates a sandbox.
- **Full session visibility** records the trigger, transcript, tool calls, output, and cost. A bad result can be inspected and replayed instead of guessed at.


These controls are not extras added after an agent proves useful. They are what let the team trust it with useful work.


## The unit of capacity becomes the session


Agents also change how teams think about capacity. A person can focus on one consequential problem at a time. Cloud sessions can handle many bounded, independent tasks in parallel, each with its own sandbox and budget.


The useful unit is no longer a software seat waiting for someone to use it. It is a session and the resources it consumes. Tokens, CPU, memory, and the platform fee are visible per session, so teams can compare the value of a workflow with its actual usage.


This does not make engineering judgment cheap. It stops spending that judgment on repeatable throughput. Senior engineers should decide how a migration rolls out, not spend the morning reconstructing which pull requests merged yesterday.


## Start with one narrow job


The best first agent has a clear trigger, a bounded output, and an obvious reviewer. It reads a known set of repositories and has only the permissions its job requires.


Write the team's standards into the system instructions and skills. Inspect early session records. Tighten vague instructions, remove unnecessary access, and expand the scope only when the output earns it. The agent improves because its definition improves.


## Engineering at a higher altitude


Every prior abstraction in software followed the same arc. Compilers did not eliminate programmers. They moved programming above assembly. Cloud did not eliminate operations. It moved operations above racking servers. Agents move software engineering above performing every repeatable step by hand.


The future is not an autonomous system replacing the engineering organization. It is an engineering organization that defines agents in code, starts governed sessions when work appears, and keeps people at the decisions that matter.


The future of software is not less engineering. It is engineering at a higher altitude.
