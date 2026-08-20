---
schema_version: "1.0.0"
document_id: "42c7655f1292278f322e6d97f808ab96095c3c65d91a514760232a18c1ea8385"
company_key: "yc-nessie"
company: "Nessie"
source_id: "yc-nessie-news-import-92dbaf0ffbdb"
canonical_url: "https://nessielabs.com/blog/why-we-killed-our-product-twice"
published_at: null
first_seen_at: "2026-08-10T00:59:32.267738+00:00"
fetched_at: "2026-08-10T00:59:34.485287+00:00"
content_hash: "sha256:6e9f37146264736f513de5bd359d25aa871a6dcb54091ccfda0c4a021ddf4dd6"
---

# Why We Killed Our Product Twice

We've rebuilt Nessie from scratch three times in six months. This is the story of how a chat app became a memory layer became something else entirely, and what we learned at each stage.


## Infinite Chat


The first product was born from a personal insight: I hit the one-million-token limit in a Gemini conversation I'd been using as my brain for months. Research, strategy, and half-formed ideas became inaccessible overnight. The context window had moved on without me.


So we built a premium chat for AI power users. You'd download your ChatGPT or Claude data, import the conversations manually into Nessie, and keep chatting with full memory of everything. Our thesis:


**Problem:** AI is brilliant but amnesic. Context dies in siloed chats, forcing you to re-explain yourself constantly.


**Belief:** The future isn't bigger context windows - it's state: user-owned continuity and recall that compounds over time.


**Why now:** Models are great; the state layer isn't. Exports and APIs now exist.


This is what we built in the first two weeks of YC.


## What Broke


YC's guidance was simple: build something you'd actually use. So we did, and the cracks showed right away.


Most AI chats are short and random. The premise that users had long, high-value threads they desperately wanted to continue was narrower than we assumed. Infinite Chat made one conversation longer; what most people actually needed was to manage their knowledge across all conversations. We killed it shortly after.


## The Auto-Organizer


After Infinite Chat, we built something new: a system that captured your AI conversations and auto-organized them into notes and folders.


We had a Chrome extension that let you grab individual chats - clunky, because the DOM on ChatGPT and Claude broke constantly - plus a manual import flow for exported data files. Each conversation got summarized and filed away automatically.


The dream was always bigger: automate the sync entirely, extend it to all major AI platforms, no Chrome extension required. We launched in November. ~1,900 users signed up. 300,000+ conversations imported in one week.


## Where This Hit a Wall


Most conversations don't need to be summarized. Extending auto-organization to four platforms simultaneously was expensive and slow. But the deeper problem was that not every ChatGPT chat is worth turning into a note. That's inefficient for us and not particularly useful for most people.


**The "then what?" problem.** Even with a beautifully organized database of every conversation, users didn't know what to do with it. Honestly, we didn't feel like it was useful for us either. The import was exciting. The daily experience wasn't. We kept asking ourselves: now that everything is organized, what do we actually do with it?


## What We Actually Needed


We were using Nessie ourselves every day, and the actual bottleneck became clear: we'd already done the thinking across our AI tools, but couldn't turn it into anything structured or usable. You think all week in AI conversations. Then Friday comes and someone needs a brief, or a stakeholder wants a decision summary. You spend hours writing something from scratch that covers things you already articulated - just in scattered chats nobody else can see.


And it's not just people you need to brief. Yes, AI model providers now have memory. But memory within one app doesn't help when your thinking is spread across four of them. And none of these platforms give you a structured, portable source of truth you can share with a person or deploy to another tool. You also have limited ways to control what the model providers see and know about you.


What we needed was context extraction - a way to cut through the noise of scattered conversations and produce something you can actually hand to someone, or reference later, or feed to another AI without starting from zero.


## What Nessie Became


This required killing the second product's core assumptions too.


We abolished the word "notes." Everything became contexts - structured, generated artifacts that you can share, deploy, and build on. We stopped trying to be a place where you organize your conversations and started being a system that produces useful outputs from them.


What Nessie became: Chat became the interface for generating, not filing. Every output became something structured you could actually share or deploy, whether it's a brief, a decision doc, or a profile. The library stopped being a folder hierarchy and started being a collection of things you'd actually made.


And this is still just the beginning. We're integrating beyond AI conversations, and the vision is that anything you think through - in any tool - can become part of your context. A glass box you can see into, shape, and control.


## Where We Are Now


We shipped the latest version of Nessie this month. It's live, it's free, and we're using it ourselves every day. We're also still figuring a lot out - what the right contexts look like, how freshness should work, what people actually reach for when their thinking is finally accessible.


We have a bigger thesis about where this goes - about what happens when your thinking actually compounds across everything you use. More on that soon.


We're building this in public and we'd love for you to come along. If any of this resonates,[try Nessie](https://nessielabs.com/) or[book a call with us](https://cal.com/team/nessie/talk-to-nessie-founders) - we genuinely want to hear how you think about this problem.
