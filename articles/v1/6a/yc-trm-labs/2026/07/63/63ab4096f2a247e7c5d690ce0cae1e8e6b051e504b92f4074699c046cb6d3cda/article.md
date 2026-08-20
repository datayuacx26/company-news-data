---
schema_version: "1.0.0"
document_id: "63ab4096f2a247e7c5d690ce0cae1e8e6b051e504b92f4074699c046cb6d3cda"
company_key: "yc-trm-labs"
company: "TRM Labs"
source_id: "yc-trm-labs-news-import-b34814ebf689"
canonical_url: "https://www.trmlabs.com/resources/blog/meet-the-agent-how-build-me-a-campaign-turned-into-a-reimagining-of-the-b2b-growth-function"
published_at: "2026-07-27T15:00:00+00:00"
first_seen_at: "2026-07-27T21:30:35.528978+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:69f0f39e674af4322328f3d82d5bec50b6bdb0396518965024220dca5c154b63"
---

# Meet the Agent: How "Build Me a Campaign" Turned Into a Reimagining of the B2B Growth Function

Other posts in this series (like[this one](https://www.trmlabs.com/resources/blog/meet-the-agent-how-the-solutions-architecture-team-built-mycroft) ,[this one](https://www.trmlabs.com/resources/blog/meet-the-agents-how-noor-and-vera-are-accelerating-competitive-intelligence-at-trm) , and[this one](https://www.trmlabs.com/resources/blog/meet-the-agent-how-our-deployment-strategy-team-built-mo-to-support-demos) ) introduce agents with unique names and backstories. This one is simply called “Campaign Builder.” Who said marketers were the creative ones?


The Campaign Builder agent does exactly what its name implies. A marketer gives it a strategic prompt, long or short. And about ten minutes of “sautéing onions” later, it delivers a fully fledged campaign brief — complete with background context, quantified objectives (based on real funnel and touchpoint data), audience specs (with real Salesforce lists), messaging, a bill of materials, channel strategy, email sequences, social posts, event strategy, outreach talking points, and content outlines. It also executes the channel plan.


This agent started as an entry to our CEO[Esteban's AI Frontier Challenge](https://www.linkedin.com/pulse/how-53-managers-became-ai-builders-one-week-esteban-casta%C3%B1o-1ffnc/) . And it quickly grew from there.


## The original use case: Campaign planning


A properly validated, data-backed campaign brief used to take days of effort to pull together. The process looked something like this, with each step inching the brief toward completion:


- Dig through various analytics platforms to surface account segments, intent scores, and funnel data
- Query internal documents and Slack threads for context on accounts or segments
- Build the audience lists and run the numbers on expected impact
- Pull messaging information to get the positioning and value prop right
- Build the channel plan
- Draft the content
- Research events
- Plan the emails, social, and outreach
- Set up tracking


Every step lived in a different system and required a different kind of thinking. It was good work, but it was slow. And slow is expensive when you're trying to move on a market signal. This manual process also capped how many strong campaigns we could realistically run as a team with only so many campaign owners.


So the first build was simple: one agent that reads our live systems and writes the brief. We connected the agent to Notion to find messaging and past briefs, Glean for deal context and existing assets, Salesforce for the audience and pipeline data, and built a KPI calculator for projections. We were looking for a ten-minute first draft, which a campaign owner would then review before pushing anything into production.


## The expanded scope: Campaign execution


Once we had built an agent that could plan a campaign from live data, we wanted to see if we could go a step further and challenge the entire campaign motion. If an agent could now *plan* well, could it also *execute* well?


We mapped out a new operating model and a proposed shift in the job to be done. Instead of marketers doing the work, we wanted to explore the potential for marketers to direct a series of agents that do the work (while still keeping the human in the loop to decide what ships).


‍


Before After


One marketer carefully hand-built one campaign at a time; channel owners coordinated each asset. Multi-channel campaigns took **weeks** to get live. Campaigns go live in **days**


.


## How the network works


We now have a small army of agents dedicated to advancing our campaign building motion. Agents hand off their work to each other, consecutively moving the brief closer to market, while always keeping a human in the loop to sign off at the points that matter most.


### Signal


It starts with signal. Agents map the market and watch the outside world, including competitive moves, regulatory shifts, and moments that could be worth acting on. Others watch what’s going on internally. A pipeline health check scores every sales territory against target each week and surfaces where we're soft or where there's an opening. A dashboard shows us how every account is actually engaging and tiers them from cold to hot, powered by (a seriously potent) home brewed account scoring engine.


### Proposed plays


All of that signal turns into proposed plays, which are gated by humans. A marketer approves, tweaks, or discards each play. Nothing gets built just because an agent was feeling a vibe.


### Campaign build


Once a play is approved, Campaign Builder gets to building. It takes the play, reads the live data, and stages the whole thing — the brief, the audience, and the copy already drafted in our voice — for every channel the play needs. It also briefs our design team to create the assets needed for the selected channels.


### Content and creative approval


Then comes a second human gate. A marketer reviews the content and creative before anything goes live.


### Delivery


Only after the marketer has signed off do the delivery agents take over, standing up the ready assets across our channels and launching email, webinar, paid social, and outreach sequences.


### Continuous learning


A final — and often overlooked — agentic step: continuous learning. We took inspiration from the self-learning loops built into Hermes, the self-improving AI agent built by[Nous Research](https://nousresearch.com/) . The agents at each stage of this workflow remember what worked, self-correct so they don’t repeat past mistakes, propose edits to their own instructions from our feedback, and feed results back up so the next round of plays is even sharper. It's composable too; we can run the full chain end to end, or point one agent at a single job.


This is still an early build, with some handholding needed to get things live at the quality we expect. But the core operation runs, we see rapid improvement, and the first campaigns have already gone through the full loop.


## What surprised us along the way


This system was built by the growth marketing team over the course of a few months, and only worked because we committed to continuous testing and iteration. And we learned a ton along the way (including was a repo is and how to use one).


One learning the surprised us was the cross-system synthesis Campaign Builder was capable of.


In one case, it surfaced a document from Google Drive via Glean and wired it straight into the audience messaging. But that wasn’t something we had explicitly asked for it; we’d actually just given it a high-level objective and nothing else. But Campaign Builder autonomously found the link between something we’d learned as an organization and something the brief needed to say. That’s when we started seeing how powerful this whole effort could be in delivering the right message to the right people.


## What we got wrong (and fixed)


An early version of Campaign Builder built its outcome projections on benchmarks instead of our own numbers and data. While the projections looked reasonable, they weren't grounded in how our funnel actually behaves.


The fix was an in-house scoring model. Our in-house model reconstructs every account's engagement from raw touchpoints (weighted by what actually predicts pipeline), tiers every account from cold to hot, and powers the projections and account scoring. It replaced the vendor score we'd previously been leaning on, which was a black box and was never tuned to our funnel.


## What's next


A multi-channel campaign that used to take several weeks can now be pushed live in two days, and our team is spending more time on strategy and judgment rather than gathering context. But Campaign Builder is just the beginning. Now, we’re thinking through how we can build more autonomous agents that are even more connected to each other.


Our vision goes well beyond single campaigns. We’re thinking about a system that spots the reasons to run, runs them, and reports back with results — with humans owning the judgment at each gate and spending their time directing the fleet instead of driving the truck.


‍


{{horizontal-line}}


‍


This post is part of a series where we introduce the AI agents TRMers have built. Each one has a role and a story. If building systems like these that pull from live data and produce real work sounds like your kind of problem,[we're hiring](https://www.trmlabs.com/careers) .
