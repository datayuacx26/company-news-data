---
schema_version: "1.0.0"
document_id: "14a203f4503f88980df7e5a29737572c1ecc56f279734d409d7533a72c75522f"
company_key: "yc-spott"
company: "Spott"
source_id: "yc-spott-news-import-75f63b0df54a"
canonical_url: "https://spott.io/resources/building-modern-ats-from-scratch"
published_at: "2026-07-13T21:20:37.065+00:00"
first_seen_at: "2026-07-24T01:59:40.161266+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:ad09321874d899168ae469422ec1d6c85c00b4bf94bfa0349b2b3ec756498e58"
---

# Building a Modern ATS From Scratch: A Conversation With Spott's Lead Frontend Engineer

## TL;DR


Recruiting software has a reputation for being clunky, ugly, and exhausting to use. We built Spott from scratch to change that. In this conversation, Melanie, our lead frontend developer, explains what it takes to build a modern ATS from the ground up: making dense recruiter data feel clean without hiding a single critical detail, giving every recruiter deep customization so the tool adapts to them, and shipping fixes the same day a recruiter reports a problem. The short version of her philosophy: we don't stop iterating until the most complex tasks feel simple for the people using them. And because Spott has an AI-native foundation rather than fifteen years of patched-over code, it can do things legacy tools structurally can't, like contextual matching that reads the "vibe" of a candidate straight from a call note.


Recruiting software is famously hard to love. Most of it was built for a different era and has been patched ever since, which is why so many recruiters describe their ATS as something they fight rather than something that helps.


We think that's a design failure, not an inevitability. So we sat down with **Melanie, lead frontend developer at Spott** , who joined as one of our first engineers and has spent the last year and a half building our ATS from the ground up. We talked about what "intuitive" actually means when a recruiter is staring at thousands of candidates, the feature that looked simple and nearly broke her team, and why starting from a blank page is an advantage you can't retrofit.


*Melanie, Lead Frontend Developer at Spott.*


## Who are you, and what do you build at Spott?


I'm Melanie, lead frontend developer at Spott. I joined about a year and a half ago as one of the first developers, and together with a group of genuinely talented engineers I've been building our ATS from the ground up.


I'm really proud that we're building the next generation of ATS. We're moving beyond the clunky, outdated systems recruiters have put up with for years and creating something that actually feels intuitive and powerful to use.


## Recruiting software is famously clunky and ugly. What are you obsessed with getting right that others don't bother with?


Recruiters deal with a massive amount of data. Thousands of candidates, endless notes, and complex pipelines. Most legacy tools just dump all of that into a messy, spreadsheet-style view that's exhausting to look at.


My non-negotiable is making that data look clean and concise without leaving out a single critical detail. The recruiter should find exactly what they need in seconds.


We get there through deep customization across the whole platform. Whether it's custom attributes and views or AI-prompted columns, we built Spott so every recruiter can set up their workspace to show exactly what matters to them. It's about giving people a tool that adapts to their workflow, instead of forcing them to adapt to the software.


## Walk us through something in Spott that looks simple but was actually really hard to build.


A recent one that was genuinely hard is our new Search V2. The goal was to overhaul the candidate search experience to be as intuitive as possible, and that led us to build a system that now auto-suggests pre-filters and ranking criteria based on the context of the search.


We also prioritized flexibility. A recruiter can start a search from a job, from a raw job description, or from scratch. Making that level of complexity feel effortless is a big UX challenge, but it's a clear example of how we work: we don't stop iterating until the most complex tasks feel simple for the person using them.


## You're building a brand-new platform instead of patching a 15-year-old one. What does that let you do that legacy tools can't?


Comparing Spott to a legacy ATS is like the difference between running a technical mountain trail in a pair of heavy old leather hiking boots versus modern, carbon-plated trail runners.


Those old boots were built for a different era. They're clunky, they're heavy, and they're full of patches. You can add new laces, but the foundation is still outdated.


Because we built Spott from scratch, we're not fighting legacy code from 2010. We built an[AI-native foundation](https://spott.io/resources/what-is-ai-native-ats) from day one, and that lets us do things legacy tools just can't. One example is contextual matching that actually understands a candidate's "vibe" from a call note, not just the keywords on their CV. We're built for speed and agility on modern recruiting terrain, not just trying to survive it.


## How do you actually know the product is easy to use, and that a recruiter never feels lost?


We don't guess what recruiters want. We iterate based on reality.


My team and I spend a lot of time watching session recordings, seeing exactly where a user's mouse pauses or where they get lost in a menu. We also keep a very tight feedback loop with our users. As an engineer, I can hear a piece of feedback in the morning and have a fix or an improvement live by the afternoon.


We're constantly talking to recruiters, watching how they actually move through the platform, and refining the experience from there.


## What's coming that you're most excited about, without spoiling too much?


The feature I'm most excited about is our general chatbot. We're essentially folding our entire interface into a single, conversational one.


It's not a side-chat bolted onto the corner of the screen. It's a new way to interact with every part of Spott. I can't wait for customers to see how much faster it makes their day-to-day.


## The throughline


Read Melanie's answers back to back and one idea keeps surfacing: the software should bend to the recruiter, never the other way around. Clean views that hide nothing important. Customization deep enough that two recruiters can run the same platform and see two different workspaces. A search that meets you wherever you start. Fixes that ship the same day you flag them. And underneath all of it, an[AI-native foundation](https://spott.io/resources/2-billion-tokens-ai-at-the-core) that reads context instead of just storing fields.


That last part is the piece a legacy ATS can't copy by adding a feature. It's the difference between AI[bolted onto old architecture](https://spott.io/why-spott) and AI built into the data path from the first commit. It's also why the "simple" things in Spott, like[matching a candidate by context](https://spott.io/matching) rather than keywords, take the most engineering to get right.


## See it for yourself


The best way to understand what a modern, recruiter-first ATS feels like is to put your own messy role in front of it.[Book a demo](https://spott.io/start) and bring the workflow your current tool makes hardest.
