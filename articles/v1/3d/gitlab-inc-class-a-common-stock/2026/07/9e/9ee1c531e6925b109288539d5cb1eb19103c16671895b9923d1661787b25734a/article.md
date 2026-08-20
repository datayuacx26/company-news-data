---
schema_version: "1.0.0"
document_id: "9ee1c531e6925b109288539d5cb1eb19103c16671895b9923d1661787b25734a"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/ai-agents-for-migrating-rate-limiting-system/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T03:30:07.155656+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:0c4709b741258f4d91dab863b7612cd9928ccf085adfc35d364b3e57b9663d88"
---

# How we used AI agents to migrate GitLab rate limiting

A small team at GitLab spent the past few weeks running an experiment: Could we use AI agents to migrate part of our legacy rate-limiting system without dropping the safety bar?


Short answer: yes. AI agents do work. They can also expose weaknesses in how you usually work. The pod, the loop, and the observability mattered more than the agents. What follows is how we structured the work using GitLab,[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) , and other tools — what worked, what the loop is and where it missed, and how you can copy what we did.


## The setup


GitLab has had two rate-limiting paths in production for years: an application-level` Gitlab::ApplicationRateLimiter` with 121 keys, and a separate Rack-level system. The goal was to unify them on a single implementation in` labkit-ruby` . Observable, testable, and operated the same way everywhere. Every request to the monolith touches it, so its failure modes have to be visible and reversible.


The pod comprised three GitLab team members and a handful of[AI agents](https://about.gitlab.com/topics/agentic-ai/) . Max Woolf, a Staff Backend Engineer on the API Platform team, owned the monolith side and ran most of the rollouts. Bob Van Landuyt, who works on Scalability, owned the gem and shaped the architecture. I held scope and wrote some of the early labkit code. A couple other engineers floated in to absorb context and contribute code and reviews.


Agents read context, drafted specs, implemented bounded changes, wrote tests, and pre-reviewed merge requests. GitLab Duo Code Review kept code quality high on merge requests. Humans owned scope, architecture, rollout, and final review.


We ran a strict loop: read the epic, write the spec, run adversarial review on the spec, implement only after blockers cleared, verify with explicit evidence, run adversarial review on the merge request, escalate to human review, merge. Adversarial review was capped at two resolution rounds before a human had to weigh in. Across the project we shipped 14 numbered specs and somewhere north of 30 merge requests into` labkit-ruby` . In practice, the loop ran tighter or looser depending on the person. Bob often did several spec/review cycles privately before producing a shared artifact.


That loop sounds like a lot. On legacy code, it’s a loop I can trust an agent to execute.


## What worked


Cohort 1 was the high-stakes test: five heavily-trafficked keys including` pipelines_create` ,` notes_create` , and` user_sign_in` . We rolled it 1% → 10% → 50% on May 4, 2026, 100% on May 5, 2026. Bob’s running commentary from that day is the operating model in miniature:


*“All rollouts complete. Up to now, all rate limits from the` applimiter` and the labkit implementation agree. But I suspect this is because there’s not a lot of traffic there. I’m going to see if I can generate some traffic exceeding the limit.”*


That’s what a good rollout looks like, and it’s the kind of judgement no agent should make for you. The new system agreeing with the old isn’t success, it might just mean nothing tripped.


Cohort 2 collapsed the next 95 call sites, 83 in the monolith, and 12 in Enterprise Edition (EE), under a single feature flag pair. Without that consolidation, the rollout would have meant something like 95 individual flag flips and ~190 YAML edits. Agents are very good at this kind of mechanical fan-out across a codebase. Humans are very bad at it.


## Where the loop missed


The loop missed the following things.


One was a shadow-mode miss. Cohort 2 had been running in shadow mode for days, agreeing with the old implementation. Switching it from observe to enforce should have been uneventful. There was a small hiccup.


The new adapter quietly dropped an identifier on one unauthenticated code path. Three String values were being squeezed into two primitive slots, and the wrong value overwrote the identifier. A tiny portion of users saw a generic failure for a short period of time.


Shadow comparison had actually flagged that key as diverging. We just hadn’t built our label set to distinguish a structural collision from a normal disagreement, so the signal sat in the dashboard while we ramped to 100%.


We immediately turned the enforcement flag off. Bob pinned the structural problem in one sentence:


*“I think we should make this better once we clean up this mess and call the` ApplicationLimiter` only with named characteristics, no more array scopes.”*


The immediate fix shipped two days later. The structural cleanup is on the list for the next pass.


It went through every step of the loop: spec, adversarial review, implementation, GitLab Duo Code Review, gradual rollout. The loop both did and didn’t catch it. The lesson wasn’t “agents are dangerous.” It was that we had observability, but not observability that distinguished the failure modes that mattered.


On May 15, Max ran an audit against master, pinged me in Slack, and opened Cohort 6:


*“I’ve added a Cohort 6 to the migration: bits and bobs that got missed (not you, Bob).”*


We had planned five cohorts. We needed six.


The diagnosis came a few days later: Claude had missed a handful of EE-only rate limits:` notification_emails` , some EE registry entries, three webhook keys, three sub-second` partner_*` keys, a few orphaned adapter rows. 17 keys out of 121 had slipped past the earlier cohorts. Each had a reason it didn’t fit cleanly into one of them. None had a reason to be invisible.


We hadn’t asked the agents, or ourselves, to keep a running count against the full key inventory.


Another was Redis. The` redis-cluster-ratelimiting` service runs as a 4-shard cluster. Bob’s read at the start was honest: “There’s headroom, but not enough to double utilization entirely.”


By early May the constraint we’d hit before came back:


*Bob: “The bottleneck we came across before, that wasn’t new for this project, is an actual bottleneck. This means we need to do an infra change to get around that.”*


*Max: “Uh oh.”*


We bumped` maxclients` in stages and halted at 75,000 connections instead of pushing to 100,000, once it became clear that more connections were going to tip the primaries’ CPU into saturation. One primary per shard, one core for command execution. No vertical lever to pull.


## What the agents actually changed


They moved the bottleneck. With agents drafting specs and implementing inside a tight loop, code generation stopped being the slow part. Review capacity, rollout judgement, and operator attention became the slow parts. That’s a much better problem to have, but today it still consumes human capacity.


It also wasn’t always pleasant. Mid-project, Max wrote:


*“Mixed bag, ended up in circles with an agent. Had one of those ‘I could’ve done this faster myself’ moments, which was irritating.”*


A few weeks earlier he’d called the project “one of my steeper learning curves at GitLab, for sure.” Working with agents is a skill, and the cost of building it is days where you’d have made more progress alone.


The other shift was being honest about what “done” meant. Bob’s note at the end of Cohort 1 ( *“the feature flag per rate-limit is overkill, we shouldn’t do these for the next migrations”* ) is a small example of the kind of judgement no agent makes for you. They will happily generate 95 flag flips if you ask. The human judgement was deciding not to.


## Where we are now


By mid-June, all six cohorts are at 100%. All 121 keys in the` ApplicationRateLimiter` run through the new framework, an audit confirmed the legacy path is down to near-zero, and we added a guardrail so no future rate limit can silently bypass it.


That’s the application-level migration done.` RackAttack` is next, the higher-volume layer at roughly 4 billion requests a day. Its shadow-and-enforce middleware is in development; the first merge request is approved and queued for merge.


If you want to copy this, you can use[GitLab Duo Agent Platform](https://about.gitlab.com/gitlab-duo-agent-platform/) to help you write your specs, Duo Developer to implement your issues, and[Duo Code Review](https://docs.gitlab.com/user/gitlab_duo/code_review/) to help you merge your MRs. But that’s the easy part. I’d ask whether you have a Bob. Someone who’ll deliberately try to break the new system at 1% before letting it run at 50%. And whether you have a Max. Someone who’ll run an audit when everyone else thinks the migration is done. The workflow matters; the people more. If you want to try this on your own legacy code,[try it out today](https://about.gitlab.com/free-trial/) .


AI agents work. So does changing how we work alongside them.


##


Is AI achieving its promise at scale?


[Get your AI maturity score](https://about.gitlab.com/assessments/ai-modernization-assessment/)


Quiz will take 5 minutes or less
