---
schema_version: "1.0.0"
document_id: "854fc05f0f12b9bbe237be8eef9d94dc6f0795939481d28ab7f48d386a96611f"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-b3e2d0380f72"
canonical_url: "https://www.pagerduty.com/eng/before-ai-ships-code-show-me-the-receipts/"
published_at: "2026-07-08T16:56:11+00:00"
first_seen_at: "2026-07-20T23:19:31.841537+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:6f8a0c40124939a07c7e356ccd351223440428ed73cc8911f8c23ce645b60874"
---

# Before AI Ships Code, Show Me the Receipts by PagerDuty Engineering

Now that AI agents can write our code, when should we let them ship it?


Much of an engineer’s week is consumed by small, routine changes. If agents can handle those changes safely on their own, engineers get that time back to focus on problems that require judgment, creativity, and context.


But letting a change ship without a human looking at it means giving up control. You are deciding that a whole kind of work no longer needs your eyes on it, today or any day after, until something tells you otherwise.


At PagerDuty, reliability is non-negotiable. The bar to ship code is high for humans and higher still for AI. Clearing that bar takes trust, and trust takes evidence. So for the past few months, we have been building the system that gathers the evidence and turns it into a decision: which work an agent has earned the right to ship, and which still needs a person.


This post is about that evidence: how we gather it, how we read it, and what we do when it changes.


## Not all work is the same


We started by defining ten work categories, ordered by the level of risk they tend to carry. To build them, we analyzed a few thousand pull requests merged over the previous 6 months and identified the kind(s) of change they contained. From these, we developed categories ranging from documentation, configuration changes, and dependency updates at the lower-risk end to new features and schema migrations at the higher-risk end. Subsequent PRs are then classified by an LLM and evaluated against human labels, with the classifier’s performance becoming a monitoring signal for our system.


But two pull requests in the same work category can still have very different stakes. So we also give every change a second label: an impact tier from one to five. The work category captures what the change does; the impact tier captures how far that particular change reaches and how easy it would be to undo. A tier-five change might be small, mechanical, and confined to one file. Whereas a tier-one change might span services, cut across system boundaries, or require an architectural decision.


With this classification system in place, every unit of work handled by an agent can be assigned to one of fifty work-and-impact groups. That gives us a consistent way to compare like with like and measure how the agent performs on each kind of work.


Once we could compare similar kinds of work, we wanted to know which of them were worth automating. A task that takes an hour but happens once a year is not worth much attention; a task that takes ten minutes but happens every day adds up to real time.


### Choosing what to automate first


We estimated the active implementation time based on commit activity. We grouped commits made close together into working sessions, added a small allowance for the work before the first commit, and summed those sessions for each pull request. This gives us a conservative estimate of active implementation time. It captures the periods when someone was working and committing code, but not the time spent researching, discussing the problem, or thinking through an approach. The true effort is therefore likely higher than our estimate.


Even with that conservative estimate, the pattern was clear. Around 97% of the pull requests represented less than a full workday of active implementation, and more than half represented less than an hour. Small individual tasks like these are exactly where volume matters: something that takes ten minutes is easy to dismiss on its own, but multiplied across every engineer, every week, it becomes one of the largest pools of toil we have.


Small, bounded changes are also the easiest kind of work to reason about, but size alone does not tell us whether a change is safe to hand off. A one-line change to a billing path can carry more risk than a much larger documentation update. Effort multiplied by frequency tells us where the toil is; the work-and-impact grouping from the previous section tells us where the risk is.


Together, they tell us which groups are worth automating first, and the next question is whether the agent has earned enough trust to act on that.


## The limits of approval


One way to judge whether an agent could handle a category of work autonomously was to look at how often its pull requests were approved without changes. So for each work-and-impact group, we track its changeless approval rate: the proportion of agent-generated pull requests that a human approved without asking for revisions. In principle, that gives us a simple, comparable signal of how reliably the agent performs within each group.


There is, however, a problem with this approach. A pull request approved without changes may have been excellent, or it may not have received meaningful scrutiny. From the metric’s perspective, those outcomes are identical. What we really want to know is not only whether the work was approved, but how carefully it was evaluated. That signal is much harder to collect. Review duration and depth may offer useful clues, while later fixes and production failures (incidents) tell us whether a clean approval was ultimately justified, but none is a perfect measure on its own.


## From evidence to policy


Because approval data is imperfect, we do not treat it as a blanket verdict on what the agent can do. Instead, we apply it within the work-and-impact groups defined earlier, using explicit rules (a policy) to determine when the evidence is sufficient to justify autonomy.


Applying that policy requires knowing, at two separate moments, what kind of work we’re looking at. We use two classifiers for this. The first reads the ticket and the (agent-generated) proposed plan before implementation begins, so we can flag high-risk work for additional oversight by agents and/or humans. The second reads the completed diff once the pull request is open, confirming or correcting the initial assumption. Both assign the work to a category and impact-tier labels described earlier.


With the change now classified, teams can set a simple policy to decide whether a change can be merged or a plan approved, without human oversight. A work-and-impact group becomes eligible for automatic approval only when we have observed enough examples, and its changeless approval rate remains above the threshold defined for that group. Policies do not have to be written separately for all fifty groups; we can apply the same rule across broader sets of work where that makes sense.


Any work not covered by an explicit policy continues to require human review. The evidence informs the decision, but people decide how much risk the organization is prepared to take.


## Autonomy has to be reversible


Granting autonomy changes the evidence available to us. Once a group of changes begins merging automatically, human approval data for that group starts to disappear. The signal that helped justify autonomy becomes less available precisely because we acted on it.


We therefore need other ways to understand what happens after a change ships. Reverts, corrective follow-up changes, failed deployments, rollbacks, and incidents can all provide useful evidence. We also plan to sample a portion of automatically merged changes for human review and feed post-incident findings back into the system.


Those signals should inform policy, but they should not automatically change it. Engineering leadership remains responsible for deciding when to withdraw autonomy. A pattern of poor outcomes may lead to a policy change, but so might a temporary shift in risk tolerance, for example, ahead of a major launch or during a particularly sensitive period.


The important property is not automatic revocation. It is that autonomy exists in policy rather than being embedded in the agent, so it can be narrowed or removed whenever the organization decides the evidence or circumstances no longer justify it.


## What we do not know yet


This system is not yet making merge decisions. The classifiers run on real tickets, plans, and pull requests, and we are collecting the evidence needed to evaluate the policy. For now, the system is descriptive rather than autonomous.


Several important problems remain open.


The first is selection bias. Early adopters are not a representative sample. They are more likely to believe in the system, tolerate its mistakes, and interpret ambiguous results favorably. That can make early performance look better than it will under broader use. One way to test for this is to periodically return a group to full human review and compare its performance under renewed scrutiny.


The second is attribution. Even when agent-generated pull requests receive higher approval rates, we do not yet know why. The code may be better structured, easier to review, or less error-prone. Those explanations have different implications, and approval data alone cannot separate them.


The third is, when we do eventually get enough confidence to merge certain types of changes automatically, how do we handle the merge queue, especially when we anticipate the need for a time buffer between a change being made and it being auto-merged (e.g., to ensure it is done during working hours for the team, to provide a grace period and sufficient room for manual override). How these changes will integrate with and interleave with existing manual deployment work remains an open question.


Finally, the policy cannot remain static. A threshold that is acceptable under one reliability target may be inadequate under another. Each threshold, therefore, needs to be tied to the reliability standard it is intended to support and revisited when that standard changes.


## Why this matters


The goal is not autonomy for its own sake. It is to move faster without letting the system’s speed outrun our ability to understand and manage its risk.


That requires more than a capable agent. It requires understanding an agent’s performance on a *specific type of work* , with explicit, predefined risk thresholds, a way to monitor performance after granting autonomy, and a way to withdraw it if the evidence changes.


We are not there yet. Today, the system classifies work and collects the signals that may eventually support those decisions. The remaining gaps—especially scrutiny, bias, and attribution—are exactly why we are building the evidence before enabling autonomous merges.


The useful question is not whether an agent is trustworthy in the abstract. It is whether this agent system has earned permission to handle this kind of work, at this level of risk, under this policy—and whether the evidence still supports that permission tomorrow.


## About the authors:


José Côrte-Real is a Principal Software Engineer at PagerDuty, where he’s exploring the synergy between distributed systems and machine learning to drive the company’s efforts on automated incident response. He started his career as a researcher in distributed systems at the University of Lisbon and focused his later efforts on defense applications and machine learning for medical devices and vehicular fault prediction.


Ricardo Moreira is an AI Engineer and Full-Stack Developer specializing in LLMs, deep learning, and scalable AI systems. He is a Senior Applied Scientist at PagerDuty, where he built the company’s unified AI agent testing and evaluation framework.


Ralph Bird is a Principal ML Engineer at PagerDuty, focused on AI agents, LLM observability, and production-grade AI systems. Previously an astrophysics researcher at UCLA


and a nuclear safety engineer at Rolls-Royce


, he now builds autonomous AI systems for high-stakes incident response and reliability workflows. Ralph combines deep technical expertise across machine learning, distributed systems, and software architecture with a strong background in research and engineering leadership.
