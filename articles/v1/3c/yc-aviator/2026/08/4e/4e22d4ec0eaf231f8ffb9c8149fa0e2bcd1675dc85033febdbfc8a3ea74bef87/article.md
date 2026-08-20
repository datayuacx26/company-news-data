---
schema_version: "1.0.0"
document_id: "4e22d4ec0eaf231f8ffb9c8149fa0e2bcd1675dc85033febdbfc8a3ea74bef87"
company_key: "yc-aviator"
company: "Aviator"
source_id: "yc-aviator-rss-35144b27026a"
canonical_url: "https://www.aviator.co/blog/move-code-review-before-the-code/"
published_at: "2026-08-03T17:43:30+00:00"
first_seen_at: "2026-08-03T19:22:17.022672+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:37781b01dad42c34588e29f235a544a09c7f1bb11b7ed3eb7c1de1273dae3260"
---

# Move code review before the code

The pull request as we know it is roughly 20 years old, younger than the careers of many people now defending it as non-negotiable. Code review, too, feels permanent. It is not. Google started reviewing code internally around 2006—plenty of the original Windows software shipped without anything resembling a modern review.


Code review is an approval gate that now no longer matches the shape of software engineering work, and, as we are about to change it again, it helps to remember that we invented the practice in the first place. It solved specific problems: catching defects before integration, teaching junior engineers, and spreading knowledge so no single person held all the context. Somewhere along the way, it also became compliance, gatekeeping, and, in many organizations, theater.


**Now the volume of code has broken the practice** , and the dilemma is not between[not reading code at all](https://www.aviator.co/blog/stop-reading-the-code/) and humans reviewing AI-generated code line by line to prevent slop. It is about what code review was actually for and rebuilding those functions for a world where machines write most of the code.


## Review got stuck at the merge


Ask anyone where code review happens, and the answer is automatic: right before you merge. Not before, not after. One point in the pipeline, and it has become the only point.


At Thoughtworks, trunk-based development, test-driven development, and pair programming are practiced as a kind of religion, which means review is not a ceremony. The benefits arrive continuously, inside the pairing session, not weeks later when someone finally opens a 500-line diff. When you are not sitting on a branch for weeks, and your tests are sufficient, a green pipeline is most of what you need.


The idea that pre-integration should not be the only place we review is not new. What was missing was a forcing function. Now that we have powerful agentic AIs and agentic IDEs helping us produce more code, everyone is starting to think about it again. When an agent can produce a feature’s worth of code in an afternoon, the gaps you want to catch move upstream, to the moment a developer expresses intent to the tool. By the time the diff exists, you are reviewing the consequences of a decision made hours ago, worth thousands of tokens.


[Intent-driven development](https://www.aviator.co/verify?utm_source=tns&utm_medium=content&utm_campaign=q3-2026-tns-verify&utm_term=net-new&utm_content=awareness) at Aviator means capturing intent where it’s made. This can be a brief description of the scope, what’s explicitly out of scope, or a list of[acceptance criteria](https://docs.aviator.co/verify/how-to-guides/writing-effective-acceptance-criteria) . From our experience, intent is best captured directly from prompts, from the decisions the engineer made while working with the agent.


## What are we even reviewing now?


Expressing intent produces artifacts. Dozens of markdown files per feature, specs, clarifying-question logs, the decision tree buried in a prompt conversation. Most teams do not version control any of it. So the idea of what is worth code reviewing has shifted.


Three camps have emerged. Some teams review only the spec and trust the rest. Some still review every artifact, spec, code, and everything in between, fully aware that they are becoming the bottleneck. Some review nothing and test the running system, because the volume leaves them no other option.


The way teams conduct code reviews is also shifting due to the volume of code being created. A diff that spans more than five files already strains a human’s ability to connect intended change to actual change. Multiply that by ten. The reviewer now wants more than a diff and a ticket. They want to see the original intent, the path the agent took, and maybe the code.


## Read the intent, not the implementation


The reviewer doesn’t have to look at hundreds of lines of code trying to figure out if it looks right, but at 10 lines of intent and acceptance criteria with one question in mind: Is this solving the right problem with the right constraints? That is a way better use of a senior engineer’s time.


It also keeps the knowledge-sharing function of review alive. The reviewer who knows there is a date-handling library that has lived in the platform for years can codify that knowledge into the organization’s[AI slop register](https://docs.aviator.co/verify/concepts/invariants?utm_source=tns&utm_medium=content&utm_campaign=q3-2026-tns-verify&utm_term=net-new&utm_content=awareness) , and it scales beautifully. Mine your last 1,000 review comments, cluster them, and generate invariant candidates for human approval. Each invariant you codify is a code review comment that will never have to be written again.


Collective code ownership, the idea that no one person should hold all the context, is finally happening in practice because the context has to leave people’s heads to be useful to an LLM.


## Agents reviewing code don’t need a UI


Most AI code review tools today sit on top of GitHub or GitLab and leave comments. An agent reads the comments, argues back, pushes a change, or defends itself. That is the same theater we had before, now automated on both sides, and it no longer needs an interface.


**Code review, whether done by a human or an agent, has to happen much earlier,** and it doesn’t even have to be a review. It could be rubber ducking or a teaching moment. Pull it all as far left as possible so the amount of work you have to do further down is minimal. An advisory or adversarial agent riding along during generation, catching anti-patterns as they form, is worth far more than one commenting after the commit.


We built a code review agent at Thoughtworks that began as a teaching agent for junior developers, pointed at the team’s known archetype and the conventions doc, and was told to explain where developers were deviating. It evolved into being a review agent.


[Aviator Verify](https://www.aviator.co/verify) spins up servers, sends real traffic, and drives UI interactions to check that the code does what the intent said, not just that it looks right. The goal is to hand the reviewer evidence, so the review becomes a question of whether the evidence and the intent make sense, not a line-by-line diff read.


## Code review does not evolve overnight


None of this happens overnight through an org-wide memo. You cannot coach a team into shedding a decades-old belief; you have to show it in practice.


One Thoughtworks client had a firm rule: anything a consultant produced had to be reviewed. Then a spec-driven development pilot handed them an enormous pile of Markdown and unusually large change sets, and the rule collided with reality. They arrived at the realization on their own that reviewing everything the old-fashioned way would make them the bottleneck.


The AI slop register takes time to build, and in the first month, it will feel like double work: doing code reviews and[building Invariants](https://docs.aviator.co/verify/concepts/invariants?utm_source=tns&utm_medium=content&utm_campaign=q3-2026-tns-verify&utm_term=net-new&utm_content=awareness) . But once codified, a slop register will never need a reviewer again and will keep the same mistake from returning.


## Verification by machines, judgment by humans


The shape changes, but the reasons for review do not. We still need to catch defects, teach the next engineer, and keep decisions visible. What moves is where that work happens. Verification goes to the machines, which are faster and more consistent at it than we are. Judgment and knowledge stay with people because they’re the parts of the review that are important for learning.


So the shift is not from review to no review. It is from reading the code to[reading the intent](https://www.aviator.co/verify) .


*This article was originally published on The New Stack and is authored by **Ankit Jain** , CEO at Aviator and **Vanitha Kumar,** Market Technology Director at Thoughtworks.*
