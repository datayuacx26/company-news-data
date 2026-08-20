---
schema_version: "1.0.0"
document_id: "4ebaeafeece4123edf11f59dc761597923bda1d049327d1b31559f70870bf207"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/multi-step-software-delivery-with-agentic-flows/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:58e31703b5688d1e6affa86ccb5783785c68d449923a27be5b32dd8e831df7fa"
---

# Turn multi-step software delivery into agentic flows you can trust

Knowing what to do next in software development is rarely the hard part. Doing it again in the exact same steps — implement an issue, fix a pipeline, review a merge request — is. Chat that only provides answers still leaves every handoff to you. Homegrown scripts don’t inherit changes in access controls, new triggers, or updated review gates. In both cases, the multi-step paths teams rely on continue to stay stuck as runbooks in someone’s head.


[GitLab 19.2](https://docs.gitlab.com/releases/19/gitlab-19-2-released/) closes that gap as[Custom Flows](https://docs.gitlab.com/user/duo_agent_platform/flows/custom/) reach general availability: AI-powered workflows you define once, trigger from native GitLab events, and run in CI/CD pipeline. A self-healing pipeline pattern like *analyze failed test → generate fix → commit → notify team* becomes something the platform can run end to end.


In addition,[Foundational Flows](https://docs.gitlab.com/releases/19/gitlab-19-2-released/#start-foundational-flows-from-agentic-chat) no longer start only from a button, mention, or assignment. Now, when your request in GitLab Duo Agentic Chat matches specialist work — implement a change, review a merge request, fix a failed pipeline — Duo recommends the flow, you approve the handoff, and you follow it from the conversation.


As a result, agentic software delivery gets past single-turn chat. Teams encode the sequences they already trust, kick them off from events or chat, and keep humans on approval instead of on every intermediate step.


## Why multi-step software delivery stays manual


Agentic demos favor single turns. Real delivery work is a chain: gather context, change code, open a merge request, wait on CI, and respond to review. Without flows, every link in that chain is a person clicking, pasting, or remembering tribal steps.


Building those chains used to feel provisional while custom flows within GitLab Duo Agent Platform were still maturing. Teams delayed encoding the paths they already trust, such as self-healing pipelines, “implement this issue,” and event-driven follow-ups, because production readiness and event coverage weren’t there yet.


## What agentic flows change for engineering teams


- **Automate the sequences you already trust.** Custom flows run multi-step tasks across projects, triggered by GitLab events you already work with: mentions, assignments, pipelines, merge request lifecycle, work item changes, and more. They run under composite identity, so access stays bounded and actions stay attributable.
- **Start specialist work from chat.** Ask Agentic Chat to use the Developer Flow to implement work, the Code Review Flow to review a merge request, or Fix CI/CD Pipeline Flow to diagnose and repair a failed pipeline. Approve the handoff, then keep working while progress shows inline.
- **Keep automatic review intentional.** In GitLab 19.2, exclusion rules let you skip auto-review for bot-authored or branch-pattern merge requests you don’t want burning credits.[Custom review instructions](https://docs.gitlab.com/user/duo_agent_platform/customize/review_instructions/) shape what review looks for, so you’re not only choosing *which* merge requests get reviewed, but *how* .


**See Duo agentic flows in action:**


## How do they work?


**Custom flows.** Create one from a project or the[AI Catalog](https://docs.gitlab.com/user/duo_agent_platform/ai_catalog/) , choose visibility, enable it where you need it, and attach a[trigger](https://docs.gitlab.com/user/duo_agent_platform/triggers/) so the right GitLab events start it. You can add human-in-the-loop checkpoints at sensitive steps. In 19.2, custom flows also pick up a "work item status changed" trigger and bulk enablement for public flows across up to 100 projects. Looking ahead, a Flow Creation Agent is on the roadmap so teams can describe a flow in plain language and get a runnable definition, without hand-writing the full schema first.


**Foundational Flows in Agentic Chat.** When your request matches specialist work, it can be handed to a foundational flow, you approve before anything runs, and you stay in the conversation while it works. That’s the difference from chat that only answers: The multi-step part leaves your hands without leaving GitLab.


**Updated Automation for Code Review Flow.** Exclusion rules keep bot-driven or out-of-scope merge requests from burning code review cycles you didn’t mean to spend. And,[custom review instructions](https://docs.gitlab.com/user/duo_agent_platform/customize/review_instructions/) shape what “good” looks like for your team, so that expanding automation doesn’t mean reviewing everything the same way.


## Start encoding the paths you already run


Custom Flows are now generally available, and Foundational Flows can now start from Agentic Chat which routes your request to the right specialist flow as you describe what you need — translating the agentic software delivery sequences your team already trusts to predictable automation, beyond tribal knowledge.


Ready to learn more? Start by reviewing our documentation for[Custom Flows](https://docs.gitlab.com/user/duo_agent_platform/flows/custom/) and[Foundational Flows](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/) . One thing to plan for: Event-driven flows consume credits based on the work they do. Try them on a few projects before you turn them loose across a large group.


Similar to other capabilities of Duo Agent Platform, you can get access to agentic flows with a[free trial of GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) . On the Free tier, you can[sign up in a few simple steps](https://docs.gitlab.com/subscriptions/gitlab_credits/#for-the-free-tier-on-gitlabcom) .


Already on GitLab Premium or Ultimate? Start by[turning on Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/) and use the[GitLab Credits included with your subscription](https://docs.gitlab.com/subscriptions/gitlab_credits/#included-credits/) .


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less
