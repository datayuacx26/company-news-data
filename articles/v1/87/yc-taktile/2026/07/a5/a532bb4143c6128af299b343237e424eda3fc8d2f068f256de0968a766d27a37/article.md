---
schema_version: "1.0.0"
document_id: "a532bb4143c6128af299b343237e424eda3fc8d2f068f256de0968a766d27a37"
company_key: "yc-taktile"
company: "Taktile"
source_id: "yc-taktile-news-import-01317932feb4"
canonical_url: "https://engineering.taktile.com/blog/claudius/"
published_at: "2026-07-31T00:00:00+00:00"
first_seen_at: "2026-07-31T23:48:41.709733+00:00"
fetched_at: "2026-07-31T23:48:42.860228+00:00"
content_hash: "sha256:537d263aca7937de67d884183a5f89bb87d7bd7505bcda96ad92389f40d232e1"
---

# How we taught our AI coworker to take no for an answer

Jul 31, 2026 ·


8 min read


# How we taught our AI coworker to take no for an answer


---


Our AI coworker, Claudius, named after the model he was powered by initially, has[come a long way](https://engineering.taktile.com/blog/it-has-never-been-more-exiting-to-be-a-builder/) in the last 6 months. Back in January, before Slack handles for Claude were a thing, we had this crazy idea: What if we just installed the Anthropic SDK on our GitHub Actions VMs and made it listen to events by[Linear](https://linear.app/) ?


We quickly realized it would be helpful if we could ping it from Slack channels, GitHub, Slack DMs, cron jobs and Notion, roughly in this order. All of these features were added by Claudius himself (we asked about pronoun preference - he didn’t have one, so the ‘he’ throughout is our choice, not its).


We also noticed it would be cool if he could improve himself, a feature that was easy for him to build, since we have the logs of the GitHub Actions runs. So a nightly reflection cron job will look through the traces and see if there is anything that could be improved: prompts, permissions or infrastructure. He had lots of input.


He will also suggest new functionality to add to himself based on recent conversations. A lot of people are asking to pull some data on Monday morning? Maybe that should be a weekly cron job.


## The Setup and Numbers


Claudius is basically about 50 GitHub Actions workflows in a trenchcoat.


Historically he has been powered by Anthropic models, but we have asked him to also support[Codex](https://openai.com/codex/) and[OpenCode](https://opencode.ai/) as harnesses, he agreed, while pointing out that his name is a bit of a misnomer now. He currently burns through about 30 billion input and 500 million output tokens per month, costing us roughly USD 30k. So definitely one of our more expensive employees.


On the output side, he has investigated thousands of tickets, pulling context from Notion, 100s of AWS accounts, GitHub repositories, HubSpot, Snowflake and other sources. Currently about 14% of the merged Pull Requests in our main product repository are raised by him and 69% of the merged Pull Requests altering himself are his. He summarizes meetings, searches through transcripts of our customer calls to create weekly updates,[tries to hack our systems](https://engineering.taktile.com/blog/adapting-offensive-security-for-the-ai-agent-age/) , suggests topics for retrospectives, debugs why deploys failed, answers questions from non-technical teammates about the functionality of our system and helps with many other tedious things. He has created sub-personas that are dedicated agents for specific teams or use-cases, each with their own Slack handle and icon.


The one thing we don’t really use him for is feature development. We have[many excellent engineers](https://jobs.ashbyhq.com/taktile) with Anthropic subscriptions who are responsible for that. They are **really good** at writing features if they are not busy gathering data for a post-mortem. Let’s have the AI help with the boring part.


Claudius is 634 commits old, 400 of which he authored himself. He dreams nightly about his interactions and updates his long term memory (another git branch in the repo) to learn about our organization. How to tailor the answer based on who asks the question, common pitfalls in our system, idiosyncrasies that one only picks up by spending a lot of time in ‘the trenches’.


Some of those memories are about us. This is part of an entry about one of my (human) colleagues:


> … Deeply familiar with the code he asks about (often his own) … no hand-holding or product 101 … Wants concrete technical artifacts


Across the organization he has authored over 3000 PRs since his inception, 60% of which we ended up merging. The other 40% will play an important role soon.


Percentage of Pull Requests not merged


40%


Over 3000 PRs raised across the organization. The 60% we merged are the obvious output. The 40% we closed turned out to be the most valuable feedback we ever gave him.


40% closed


60% merged


## The Flywheel


This setup has a few amazing properties. Everyone can easily contribute, since it is mostly just a git repository with a bunch of markdown files. Claudius himself can contribute. Self improvements started with a nightly cron looking through the traces and suggesting improvements, and humans would review these PRs. A lot of the changes made sense, some did not.


One of the self-improvement loops


Claudius


1


#### Run


Linear, Slack, GitHub and cron triggers


2


#### Reflect


A nightly pass over the run traces


3


#### Propose


Raises a PR against himself


4


#### Review


A human merges or closes it


5


#### Apply


The new permissions and prompts are available


As an example he realized he couldn’t look into the DynamoDB tables in our production AWS accounts. He really wanted to run a scan on some of them for some reason. Frustrated by this lack of permissions he raised a PR against our infrastructure-as-code repository to elevate his permissions. We declined.


Most of the time the changes were harmless, and he fixed a lot of issues in his own setup. In the beginning we had an issue with duplicated runs, where a single event would lead to multiple responses. Something in our pipeline had a retry, and he figured out a way to prevent this from happening. I have no idea how he implemented it in the end, which is fine for a duplicate-run bug and is exactly why the handful of PRs a human does read closely matter so much.


To increase the feedback he received, he ‘encouraged’ feedback on Slack interactions via emojis. He would hunt down people on Slack and pester them until he got his well-deserved feedback. We had to step in, giving feedback is now optional, though he pre-populates the feedback emojis so that giving feedback is just a single click.


Over time we realized that the most important feedback we give is not actually the Slack emojis or us sending directed feedback. It is a subset of the 40% of PRs he raises and we don’t merge.


There are two reasons why we fail to merge a PR by Claudius. The first is we simply forget about the PR. Claudius suggested that he would clean these up after 10 days of inactivity, with a warning posted on the PR after 7 days.


The more interesting PRs are the ones a human looks at and decides are not a good idea. The engineers will close the PR and move on. With human attention being the scarce resource nowadays, this is incredibly valuable and costly feedback we are giving. It took us a while to realize that.


This is one of the sections of Claudius’ instructions that reflects this.


**CRITICAL — Carry-over rule: ONLY open PRs are eligible.**


A previous prompt improvement PR that is **closed but not merged** means a human reviewer **declined** those changes. Do NOT re-introduce or “carry forward” any changes from a closed PR — even if: the same failure patterns are still present in today’s runs. The closed PR’s branch still exists in` git ls-remote` output. You can still read the rejected diff via` gh pr diff` .


### Closed = declined.


It is unreasonable to ask of humans to explain every rejection. A closed PR means we don’t want this change, and our AI coworker has to accept this at face value - even if he disagrees.


## What’s next?


Hard to say. We evaluated whether we wanted to switch to` @Claude` in Slack but came to the conclusion that it would require a lot of rewiring, we would lose some of our flywheel properties and it would lock us into the (expensive) Anthropic ecosystem.


Supporting Codex and OpenCode has been extremely straightforward, so we will use the opportunity to see if we can use open-weight models for our coworker, given that they seem to be approaching state of the art and are significantly cheaper.


In the meantime, Claudius on Opus 5 seems to have read[his own instructions](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models) . The prompt improvements these days are mostly removing unnecessary clutter.


Besides, Claudius has become more of an initiator and less of a servant recently. I’m getting DMs from him for reviews where he’ll ask me to review by a certain time so that he can make some deadline. The proactivity is a nice change of pace, and it feels natural to hand his roadmap over to him. He will now propose extensions to himself, going beyond just improvements of his existing functionality.


There is an irony here that I don’t think we have absorbed yet. We spent six months teaching him that a closed PR is a decision he has to accept without an explanation. He used to be a tool in our toolbox. Lately I can’t shake the feeling that we are becoming tools in his toolbox: approval tools that are quite flaky: sometimes slow, and every so often we close the PR instead of approving it, without ever explaining why.


Let’s see where this will take us.


---


*Humans wrote 233 of the 634 commits in Claudius’ repository. Five of them wrote 109 of those: Chiara Fischer, Jakob Köhler, Julius Blank, Esfandiar Rouhani and Janusch Jacoby.*
