---
schema_version: "1.0.0"
document_id: "dafbdcc34c087fe8b8144c1053409e389d434988c8db6dabad2447649050ec4f"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/why-we-dont-call-cosmic-an-ai-cms"
published_at: "2026-06-17T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:13:02.492982+00:00"
content_hash: "sha256:1fea8466eea6cfa7218606b2b9bcd11cb63dbb6f62bdb80101a54dfc0a520702"
---

# Why We Don't Call Cosmic an "AI CMS"

A new report from WordPress VIP landed on[Hacker News](https://news.ycombinator.com/item?id=48569278) this week. The headline: 60% of US consumers say seeing 'AI' in brand messaging makes them less likely to trust a product. 61% can't name a single brand that uses AI well in its messaging. And 7 in 10 say the internet feels less human than it did ten years ago.


The comment section was mostly developers nodding along.


We've been thinking about this for a while at Cosmic, and it's a good moment to explain a deliberate choice we made.


## We ship AI. We don't lead with it.


Cosmic has AI agents built into the product. You can create a content agent, give it a schedule, connect it to your bucket, and have it draft, publish, and update content autonomously. You can wire it into Slack, give it approval gates, scope its permissions to specific object types. The AI is real and it's central to what we're building.


But open our homepage. The headline isn't "The AI-Powered CMS." We don't have an AI badge on every feature. We don't use "AI" as an adjective for things that were just always called features.


That's a deliberate product and communications choice, and the WordPress VIP data is a good reason to explain it.


## The trust deficit is real


The research surveyed 2,000 decision-makers and consumers. The findings aren't subtle:


- *60% of consumers* say "AI" in brand messaging makes them less likely to engage
- *61% cannot name a brand* that uses AI well in its messaging
- *74% say the internet feels less human* than it did a decade ago


This tracks with what developers tell us directly. The word "AI" has been applied to so many things, from autocomplete to autonomous agents, that it no longer communicates anything specific. When every SaaS product has an "AI copilot" and every CMS has "AI-powered content," the label becomes noise.


Developers are skeptical by default. They want to know what a tool actually does, how it works, and whether they can trust it with their production stack. "AI-powered" answers none of those questions.


## What we say instead


When we describe Cosmic's agents, we describe what they do:


- A content agent that runs on a schedule, reads your bucket, and drafts posts based on a prompt
- A team agent that lives in Slack, responds to your questions about your content, and can publish on approval
- Scoped execution contexts with explicit permissions so agents can only touch what you've authorized
- Audit logs so you know what ran, when, and what changed


Those are concrete, verifiable behaviors. A developer can evaluate them. A non-technical stakeholder can understand them. Nobody has to take "AI-powered" on faith.


## The Fable 5 lesson


The timing of the WordPress VIP report matters. The Fable 5 and Mythos 5 government suspension hit developers hard this week, with threads on HN running to thousands of comments. The biggest theme in those threads: what do I do if my AI stack gets pulled?


That's a reasonable question, and it's one that "AI-powered" branding makes harder to answer. If your CMS is "AI-powered," what happens when the underlying model changes, gets suspended, or gets too expensive?


Cosmic's answer: the content infrastructure is the stable layer. The model is swappable. We pulled Fable 5 and Mythos 5 from our dashboard and API when the suspension happened and[recommended Opus 4.8 as the replacement](https://www.cosmicjs.com/blog/fable-5-mythos-5-suspended-developer-action-plan) . That migration took minutes because we built the agent layer to be model-agnostic from day one.


That's what "AI" should mean in a CMS context: not a marketing label, but a concrete architectural choice about how AI capabilities connect to your content.


## What this means for how we build


A few principles that follow from this:


*Describe the behavior, not the technology.* "Draft a blog post from this RSS feed on a schedule" is more useful than "AI content generation."


*Make it auditable.* Every agent action in Cosmic creates a log entry. You can see what ran, what it produced, and who approved it. That's the opposite of a black-box "AI feature."


*Keep the model out of the brand promise.* Our product doesn't break when Anthropic changes a model name or when a new open-weights model takes the top spot on the leaderboard. We use to connect content to whatever model is best right now.


*Ship capabilities, not labels.* The question we ask before adding anything to the product: does this make a developer's job concretely easier? If yes, we ship it. Whether we call it "AI" is a secondary question.


## A note to other developers building AI tools


If you're building a product that uses AI, the WordPress VIP data is worth reading in full. The developers in that HN thread weren't anti-AI. They were pro-substance. They want to know what your tool does, what model it uses, what the failure modes are, and what happens when something changes upstream.


Answer those questions clearly and you don't need the label.


---


Cosmic is a headless CMS with built-in AI agents. You can create your first bucket, connect an agent, and have it publishing content in about ten minutes.


[Start free](https://app.cosmicjs.com/signup) — no credit card required. Or[talk to Tony](https://calendly.com/tonyspiro/cosmic-intro) if you want to walk through how agents work in production.
