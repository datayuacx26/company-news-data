---
schema_version: "1.0.0"
document_id: "ebbd95080bfb511f13cb5da882e2b25e695e115466d879d649605653ccc2218b"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/claude-sonnet-5-on-gitlab/"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:0ec539fc0d02d4d22bd12a6d23b925b9e2986591ee332df84224dbfc1920ff54"
---

# Claude Sonnet 5 on GitLab: More reliable, more efficient

Anthropic’s Claude Sonnet 5 is now available on[GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/) across all tiers and deployment models through GitLab's AI Gateway. Claude Sonnet 5 is built for work that agents assist software teams with every day: multi-step tasks, generating code that holds up under review, and conducting workflows affordably at scale. It’s also the first model in GitLab's evaluation suite to complete all of our benchmark tasks. Sonnet 4.6, its predecessor, completed 93.8% of them. For teams running GitLab Duo agents in production, this translates to tasks that finish with higher-quality code.


> "Claude Sonnet 5 handled the full range of coding tasks we tested it on, while resolving more issues. It's a meaningful improvement to both quality and efficiency. We made it available on GitLab Duo Agent Platform today on all tiers and deployment models."
>
>
> – Manav Khurana, Chief Product and Marketing Officer, GitLab


## Finish every agent run


The most expensive agent failure is often the one that stops halfway. When a run stalls partway through a multi-step task, the cost isn't just the lost work — it's the diagnosis, the re-prompt, and the verification of whatever partial output came back. Reliability is what turns an agent from something you supervise into something you delegate to.


That's the bar Claude Sonnet 5 clears: It's the first model in our evaluation suite to finish every benchmark task. And paired with 8.8% more issues resolved, it means the work that comes back is more likely to be usable, not just present.


For teams using[GitLab Duo Agentic Chat](https://docs.gitlab.com/user/duo_agent_platform/context/#gitlab-duo-agentic-chat) , this is what changes the daily loop of prompt, wait, evaluate. A multi-file refactor produces reviewable output instead of a dead end. Test generation can return coverage you can use. Security investigations can trace further across repository history. Duo foundational agents can handle more of their assigned work without intervention, so your time goes to reviewing results rather than restarting runs.


*Prompting Claude Sonnet 5 to investigate pipeline failures over the past two months using GitLab Orbit*


## Spend less for better results


Efficiency and reliability compound. An agent that finishes more of its work *and* uses fewer resources getting there lowers the real cost of every completed task.


Different models on GitLab Duo Agent Platform consume GitLab Credits at different rates, and the right choice depends on the task. Running a broad set of everyday development work on a model whose cost profile fits that work is how teams keep agent workflows affordable at scale.


For a full list of models and their credit consumption, see the[GitLab Credits documentation](https://docs.gitlab.com/subscriptions/gitlab_credits/#models) .


## Choose the right model for your workflow


The point isn't one model for everything. It's a reliable, cost-efficient default for the broad middle of day-to-day agent work, with heavier models a click away when a task earns them.


Claude Sonnet 5 joins a growing set of[AI models available on GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/model_selection/#supported-models) . Sonnet-class models balance quality, speed, and cost for everyday development work. For complex, long-horizon agentic tasks that demand maximum reasoning depth,[Claude Opus 4.8](https://about.gitlab.com/blog/claude-opus-4-8-on-gitlab/) remains available. You select models per task through[model selection](https://docs.gitlab.com/user/duo_agent_platform/model_selection/) in your GitLab instance.


## Get started today


Claude Sonnet 5 is available now on GitLab Duo Agent Platform through GitLab's AI Gateway. Like other models, it runs on[GitLab Credits](https://docs.gitlab.com/subscriptions/gitlab_credits/#models) .


> [Start a free trial of GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) today, or sign up from the GitLab Free tier by following[a few simple steps](https://docs.gitlab.com/subscriptions/gitlab_credits/#for-the-free-tier-on-gitlabcom) . Existing GitLab Premium or Ultimate subscribers can use the GitLab Credits[included with your subscription](https://docs.gitlab.com/subscriptions/gitlab_credits/#included-credits) .


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less
