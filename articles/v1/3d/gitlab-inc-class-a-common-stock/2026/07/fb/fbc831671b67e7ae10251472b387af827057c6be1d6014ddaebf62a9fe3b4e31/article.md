---
schema_version: "1.0.0"
document_id: "fbc831671b67e7ae10251472b387af827057c6be1d6014ddaebf62a9fe3b4e31"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/claude-opus-5-on-gitlab-duo-agent-platform/"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T20:56:02.300564+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:13b2ecd979e0aec4be4ce2cd0729f7951955a20b160a7a01dc66602db251155d"
---

# Claude Opus 5 on GitLab: Reasoning built for the hard tasks

A mistake on a routine task can cost you a minute. A mistake on a large refactor or a debugging trail spanning months of commit history can cost far more, as it compounds silently over hundreds of exchanges. By the time you catch it, every step built on top of it needs unwinding, too. That's the difference between work that rewards speed and high-complexity work that requires getting it right the first time.


Anthropic's newest AI model Claude Opus 5, now available on[GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/) , is built for the tasks that demand the most from an agent. With Opus 5, your engineering team can trust agents with more complex, critical work. In GitLab's internal evaluation, Opus 5 resolved 93.3% of benchmark tasks, a 20.3-point improvement over Opus 4.8's 73.0% resolution rate.


> **"The teams getting the most value from AI agents can hand over their hardest, highest-stakes work and trust the reasoning holds up from first step to last."**
>
>
> — Stuart Moncada, VP, AI Product Management, GitLab


## Reasoning that holds up under complexity


Some teams hesitate when delegating their most challenging work to an agent because the mistakes are costly to unwind. On long-running, high-complexity tasks, an agent's reasoning has to persist from start to finish. With Opus 5’s deeper reasoning, more of your complex tasks, like multi-file features and larger refactors, resolve correctly the first time, helping cut down on the time you spend diagnosing and re-prompting failed runs. Teams running GitLab Duo Agent Platform with Opus 5 can expect to see fewer partial patches, with more of the work coming back ready to merge.


That reliability extends to completeness as well. In GitLab’s internal testing, Opus 5 completed 100% of the tasks it attempted, matching Opus 4.8’s completion rate. The edge is in what they produce: more of Opus 5's solutions are verified correct, putting Opus 5's resolution rate at 93.3%, against Opus 4.8's 73.0%.


One task in GitLab's evaluation called for mockable SSO login support in a CLI authentication flow, a change spanning five files, including new exported types and configuration fields. Several other models tested produced no attempt at a fix. Opus 5 built the full implementation, committed it, and opened a merge request.


You can expect the same precision in code review. Opus 5 flags real bugs and produces few false positives, so your team can stay focused on genuine vulnerabilities and spend less time filtering noise.


If your workload runs several agents at once, you waste less time untangling conflicts between them. Opus 5 keeps subagents coordinated, ensuring they stay out of each other's work. Writer-verifier patterns catch problems between agents before they reach you, with one agent checking another's output before it's accepted. Teams running longer, more autonomous sessions with more agents in parallel see the strongest results.


For cost-sensitive workloads running multiple parallel agents,[GitLab Credits usage caps](https://about.gitlab.com/blog/gitlab-18-11-budget-guardrails-for-gitlab-credits/) let you set a hard limit on spend, so parallel work never runs beyond what you've budgeted.


## Speed keeps pace with depth


On GitLab's hardest benchmark tasks, Opus 5 pairs reliability with speed. At the 95th percentile, the slower tail of its runs, Opus 5 finished 2.2% faster than Opus 4.8 (768 seconds vs. 784.98 seconds) and 21.9% faster than Sonnet 4.6 (768 seconds vs. 982.57 seconds). Reliability and speed move together. For your team, that means more predictable turnaround, even on your longest runs.


## Choose the right model for the task at hand


The right model depends on the task in front of you, not a single org-wide policy. Sonnet-class models handle the bulk of day-to-day development work: fast, affordable, and dependable for what most teams run constantly. Turn to Opus 5 when the work demands deeper reasoning: the hardest debugging, the largest refactors, the decisions you don't want to rework.


You set that choice directly in your GitLab instance through[model selection](https://docs.gitlab.com/user/duo_agent_platform/model_selection/) . Whichever model you choose, it runs inside the same infrastructure: the context layer, policy checks, and audit trail that cover every model on GitLab Duo Agent Platform.


## Put Opus 5 to work


Claude Opus 5 is available now on GitLab Duo Agent Platform and, like other models, runs on[GitLab Credits](https://docs.gitlab.com/subscriptions/gitlab_credits/#models) . New to Duo Agent Platform?[Start a free trial](https://about.gitlab.com/gitlab-duo-agent-platform/) today. Already a GitLab Premium or Ultimate subscriber?[Turn on Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/) and use the[GitLab Credits included with your subscription](https://docs.gitlab.com/subscriptions/gitlab_credits/#included-credits) .


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less
