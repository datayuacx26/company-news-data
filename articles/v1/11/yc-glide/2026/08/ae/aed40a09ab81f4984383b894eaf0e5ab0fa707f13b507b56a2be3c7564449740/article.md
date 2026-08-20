---
schema_version: "1.0.0"
document_id: "aed40a09ab81f4984383b894eaf0e5ab0fa707f13b507b56a2be3c7564449740"
company_key: "yc-glide"
company: "Glide"
source_id: "yc-glide-news-import-b50025bbdd3a"
canonical_url: "https://www.glideapps.com/blog/creating-slipstream"
published_at: "2026-08-06T23:37:44+00:00"
first_seen_at: "2026-08-07T05:12:27.980721+00:00"
fetched_at: "2026-08-07T05:12:30.124855+00:00"
content_hash: "sha256:a398f4d1d9919cfded42321bc50f77af7a9e5575bbc9a555fdd5ed0a2f84f05b"
---

# Creating Slipstream: How we rebuilt our software engineering process for AI agents

***TL;DR:*** *GlideOS is Glide's platform for building AI-powered apps from natural language. Slipstream is the agentic engineering system we built to ship it. Our first agentic platform took 8 months to build. Once Slipstream was in place, the team reached parity in 90 days. We didn't change the model or the tooling. To get there, we bet that agents, carefully trusted and designed for, could outpace any human-shaped process. This is a look at that process, including what changed, what it cost us to get there, and what we're still not sure about.*


Both times we were solving the same problem. We aimed to let our users generate intuitive, AI-powered apps with nothing but natural language. The first time, we built agentic capability on top of an architecture and a set of processes designed for *humans* . It worked. But it was slow. And the slowness wasn't incidental. It was the human-shaped process showing through.


We began again with a new design.


This time, we believed that if agents had the freedom to operate naturally, they would do better than the human process we added on. It worked better than we could have imagined. We couldn't have built it that way on our first attempt. It took living inside the slow version to trust the fast one.


## Why our human-centered engineering process broke down


The impact of centering our engineering process on humans showed up everywhere. Deployments still waited on individuals. Rollbacks relied on an engineer or, worse, our customers to spot issues. It wasn’t necessarily a bad process. It was just entirely dependent on a human to notice, approve, and review every step.


### The Problem: Pull requests became a time-consuming


We felt it first and hardest in pull requests.


Pull request review used to be one of the more valuable parts of an engineer's day. As agent-generated code increased in volume, it became the opposite. A queue we had to clear, and again tomorrow. And again the next day.


The code was good. This was the problem. Good enough, consistent enough, that review stopped being a judgment. And started being an approval stamp. Engineers asked to reduce their review commitments. They felt the code needed less scrutiny, not more.


### Slipstream: catching problems without a human in the loop


But getting out of the way created a new problem. If agents are building and shipping without a person checking every step, something still has to catch it when it's wrong.


We needed to create another system. This system would detect when something breaks and fix it automatically, without waiting for someone to notice. We ended up with two systems in one: a factory and a recall line running behind it.


Once we built the system, building our team’s trust in it didn't happen by default. It took months to tighten the agent guardrails. This included linters, prompts, constraints, automated checks, and evals.


None of this was fixable at the process layer alone. The old architecture only worked because humans were the ones operating it. This meant the fix had to go all the way down to the architecture itself. Agents took over as the operator, rethinking key decisions. This included everything from the repository structure to deployment locations.


Here are the key changes we made:


1. We replaced our distributed, polyrepo setup with a single monorepo. This way, an agent could see the entire system, not just its specific part.
2. We moved to Cloudflare. This change lets agents set up infrastructure quickly, instead of relying on engineers.
3. We leaned into feature flags as a default. This meant shipping and exposing could stay two separate decisions. Skipping a staging environment and going straight to production became safe instead of reckless.
4. Finally, we moved to a merge queue for higher throughput with less friction.


None of the changes we made was the system itself. It was the foundations that had to be true before the system could exist.


## Slipstream: Our AI agent software delivery pipeline


We call this delivery pipeline Slipstream. It’s the system every agent, station, and feature runs through, from spec to production. The name is meant literally: a slipstream is borrowed speed, tucking into the wake of something ahead of you and moving faster than you could alone. Agents absorb the drag, the repetitive, mechanical parts of shipping software, so a feature travels the line faster than a person could push it through alone.


Slipstream consists of three stations, each run by an agent:


- **Build** : An agent builds the feature from a spec. It follows conventions, linting, and architecture rules right from the start. Written cleanly the first time, not cleaned up afterward
- **Gate:** A second agent validates the change against the rules that have to be held. Evals and end-to-end tests, not manual approval. A human still signs off, but on the feature, not the code. What a human cares about is whether the change works. Code-level quality is Slipstream's job now
- **Ship:** The change clears the merge queue and goes live behind a flag. Shipping and exposing stay two separate decisions. A rollout watcher follows every deploy and flag switch, checking for degradation


Below you can see a screenshot of our Slipstream Slack agent that takes a user request and puts it through the build, gate, and ship pipeline.


## How AI agents detect and fix production issues without human intervention


When a build or gate agent misses something and a bad change reaches production, Slipstream doesn't wait for someone to notice. It runs a separate recall pattern to catch it: detect, notify, diagnose and repair, and return to service. The recall loop finds the fault and opens a fix, which then travels back through the Ship station like any other change. It only activates when something's actually gone wrong. Most changes never touch it. It's the same logic a car factory uses: cars leave the line and mostly stay fine, and when they don't, the industry doesn't add a fourth assembly station, it issues a recall.


- **Detect** : Telemetry flags the fault. This could be a runtime error, security finding, anomaly in traffic, or a drop in code quality.
- **Notify** : Notify the affected units. They are identified and sent to a GitHub issue. The owning team is assigned to handle it.
- **Diagnose & repair** : An agent picks up the issue, finds the root cause, and opens a fix as a pull request against it
- **Return to service** : Once a human approves the fix, it ships through the same path as any other change: Ship


Rolling anything forward (a feature or a fix) still ends with a person. However, the two sign-offs ask different questions:


- At the gate after Build, a human reviews the feature: does it do what we asked?
- After Diagnose & repair, a human reviews the code: is this fix safe to ship?


Neither is a return to line-by-line review. The system holds the code-quality bar. But these are the two judgment calls we've deliberately kept human. Rolling back, on the other hand, doesn't wait for a person. The rollout watcher alerts someone first, then reverts automatically if there's no response. From our internal Slack, the day it first happened:


> > “ *Big win over in agentic engineering land! I think this is one of the first instances of a 'self-healing' platform. Our rollout watcher detected a platform service regression, and in this case, we'd empowered it to make an autonomous decision to roll back the change to protect customers. In a past world, this might have taken an hour or more of an engineer's time.”*
> > *- Shay Frendt, Glide CTO*


Once we trust this pattern and the guardrails below it like we trust the build agent’s output, we expect to automate the approval step next. Security fixes, runtime errors, and reliability issues will fix themselves from start to finish.


## Results: Shipping software with AI agents


We're publishing this mid-rollout, not after the fact. Some of the numbers below are real results. Some are the metrics we've committed to tracking as the system matures. We think that's more useful to you than a polished after-the-fact case study would be.


**Metric** **Before (G2)** **Goal** **Status** **Notes**


**Deployment frequency** Daily, human-gated release trains Continuous, automated 15 mins a deploy happens Merge → production median: 14 min, vs 23 h on G2. Gate is a passing eval/e2e bar, not manual approval


**PR cycle time** 21 h (p75, reviewed PRs) < 6 h 5.4 h (p75) Full cycle including human sign-off.


**Agent-review threshold** 0% (all human review) 100% 90% % of changes agent-reviewed without a human involved.


**Rollback trigger** Manual, reactive Autonomous on user-breaking behavior 100% of deploys Every deploy monitored; auto-rollback on degradation — moved from theoretical to reality


**Non-engineers shipping to production** 0% (only engineers) 100% of the org empowered to ship 25% Engineers no longer gatekeep solving problems. 10 people have shipped 37 production PRs from specs alone


What is most remarkable is the trend across all of these numbers. They are all moving in one direction: away from humans. The system now enforces the bar itself. Humans focus on reviewing outcomes instead of mechanics.


One of the most exciting parts of this shift is the way it unlocked engineering abilities for even non-technical team members. **Everyone in a non-engineering role can now ship to production.**


A quarter of Glide, people once blocked behind an engineering queue, already have. Engineering is roughly 30% of the company. Opening the line to everyone else nearly doubled the number of people who can turn customer feedback into a fix on the same day.


The gap between 100% unlocked and 25% shipping is the part we haven't solved. The technology stopped being the barrier months ago. What's left is confidence. People who've spent their careers being told production is someone else's job don't start shipping just because the door is open. This isn't a Slipstream problem, it's a human one, and it's the next thing we have to earn, the same way we earned the engineers' trust the first time around.


## What AI-first engineering means for your team


Build, gate, ship, recall. You already have these stations.


Every quality engineering team runs some version of build, gate, ship, and recall, whatever they call it. The real question is who those stations are built for. Slipstream removes the grunt work from our engineers' responsibilities. The maintenance, the toil, the constant small interruptions that used to eat the week are now the system's job, freeing people to do the thing only people can do: ship net-new value to our users. The next step is extending Slipstream's guarantees to our customers: the same reliability and quality bar it holds internally becomes the bar we hold for what we ship them.


We're writing this down because we don't get to hold our customers to a standard we won't hold ourselves to. Slipstream isn't just an internal tool, it's the same pattern we want our customers' operations running on. Every lesson we learned the hard way, through eight months of work, guardrails, and false starts, is one our customers won't need to learn. This article is the record of proving that pattern on ourselves first, before asking anyone else to trust it.


Code was never the hard part. It's cheap to generate now. Software engineering was always the harder discipline: maintenance, security, reliability. What changed is who, or what, does that work.


Create a Glide app in five minutes, for free.


[Sign Up](https://os.glideapps.dev/?utm_source=blog&utm_campaign=Creating+Slipstream%3A+How+we+rebuilt+our+software+engineering+process+for+AI+agents)


[Simon Fletcher](https://www.glideapps.com/blog/author/simon-fletcher)


Principal Software Engineer at Glide


### Related Articles


[The ultimate guide to custom AI apps for your e-commerce operations](https://www.glideapps.com/blog/ai-apps-for-ecommerce)[Shivani Shah](https://www.glideapps.com/blog/author/shivani-shah) 6 Aug 2026


[GlideOS launch recap](https://www.glideapps.com/blog/glideos-launch-recap)[Glide](https://www.glideapps.com/blog/author/glide) 4 Aug 2026


[AI engineering is the next wave after no-code: Here is how business leaders can get started](https://www.glideapps.com/blog/no-code-to-ai)[Gideon Lahav](https://www.glideapps.com/blog/author/gideon-lahav) 28 Jul 2026
