---
schema_version: "1.0.0"
document_id: "94a931b7fbf08a242cdce8229c9b270160163d0d1e68aabc34f321aebedd5609"
company_key: "yc-minusx"
company: "MinusX"
source_id: "yc-minusx-rss-04cd28ab33fb"
canonical_url: "https://minusx.ai/blog/welcome-to-minusx"
published_at: "2024-08-25T00:00:00+00:00"
first_seen_at: "2026-07-25T14:25:41.183756+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:8d057f2508cde3a1ac9005e20472c71eaf143aa0c4eea9e84bec864d0614d85c"
---

# Welcome to MinusX!

Over the last couple of weeks, you may have seen us launching MinusX on[YC](https://www.ycombinator.com/launches/LXH-minusx-ai-data-scientist-for-jupyter-and-metabase) ,[Hacker News](https://news.ycombinator.com/item?id=41301448) ,[Product Hunt](https://www.producthunt.com/posts/minusx) ,[and a whole](https://www.linkedin.com/feed/update/urn:li:activity:7226708630773936128/)[lot of](https://twitter.com/ycombinator/status/1820937033608495225)[other places](https://www.tryfondo.com/blog/minusx-launches) . Now that there's a bit of calm (read as dealing with only a handful raging fires), I wanted to give a bit more context on the problem we're working on, our take on the solution surface, and why we're excited to be building MinusX.


---


### Where's my data?


Most people start their work everyday by looking at some metric. It may be a cheery plot of app downloads on a dashboard, a dreadful user retention number in a weekly presentation, or a report of a study their lab just completed. Usually, they have follow up questions. Some know whom to turn to for answers. An even fewer number know how to get it themselves. Most forget about it and move on with their day.


The fog-of-war over data plagues all sorts of teams, in organizations of all shapes and sizes. In large orgs, most people don’t know what specific secret filters need to be kept in mind to get clean data. Analysts/Scientists buffer most of these requests over a thousand back and forths. In smaller ones, most data projects end up being one-off efforts, and many die midway. Most of the blame is placed on the tools used. And sometimes on unrealistic expectations of wanting accurate data instantly. "You want a new dashboard? Right now? Are you crazy!?"


Baseline is that if you’re a programmer, you just want answers. If you’re a product manager, you just want answers. If you’re an analyst/scientist, you usually want 10 clones of yourself.


This sounds like a problem that AI should be able to solve. That's exactly what MinusX does. Sreejith, Arpit and I are building MinusX, a data scientist that gives you answers, a clone, and everything in between.


### Sure sure. You're trying to make me download another new shiny analytics app aren't you!? \[Hint: No!\]


A new platform is the most popular solution today. Julius / Hex / Deepnote, a bunch of YC companies etc have platforms that you can move to, that give you an AI colleague out of the box, that answers all your questions. But this is comes with a ton of issues. You are forced to:


- Undergo a painful process of data migration
- Abandon all existing workflows and learn a whole new tool as a side-quest rather than solving actual problems
- Convince the entire team to move before seeing any benefit
- Hope that the new tool has all features you trusty old one did


Imagine how annoyed you'd be if a new human colleague asked you to do all this. Why should an AI be any different?


> An AI Data Scientist is a Scientist, not yet-another-new-fancy-analytics-app


MinusX *just works* . It is a chrome extension that adds a sidechat to your analytics app. Given an instruction, our agent operates the tool - by clicking and typing, just like you would - to analyze data and answer queries. MinusX interoperates with the tools you already use and love, without changing your workflows. The same interface works across all analytics apps. You can[install MinusX](https://minusx.ai/chrome-extension) , and head over to your Jupyter or Metabase apps, and start using it right now!
We'll have a separate post on the nuts and bolts of how it actually works, soon.


### Why will MinusX win?


The bottleneck to doing interesting and complicated data tasks is intelligence, an order of magnitude more than tool sophistication. And intelligence depends on context. The best and most unfiltered form of context is where people already work. If there is one single reason why we will win, it is this.


The philosophy of MinusX mirrors that of[comma.ai](https://comma.ai/) . Just as comma is working on "an AI upgrade for your car", we want to retrofit analytics software with abilities that LLMs have begun to unlock. We also get a kick out of the fact that we use the same APIs humans use (clicking and typing), so we don't really need "permission" from any analytics app (just like comma.ai does not need permission from Mr Toyota Corolla) :)


"Retrofitting" is a weird concept for software, and we've found that it takes a while for people to grasp what this actually implies. We think, with AI, it will be more of a thing. Most software we use will be "upgraded" and not always by the people making the original software.


We're beyond thrilled to be working on MinusX. We think there is an opportunity to set the standard for delightfully co-working with AI agents. We're sprinting towards it, fast. We'd love for you to sprint with us.
❤️
