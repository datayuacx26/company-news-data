---
schema_version: "1.0.0"
document_id: "76d9e2493ef876a74e7728637f40df83dabcb230992cfaeab4c1658dd0916ed5"
company_key: "yc-webhound"
company: "Webhound"
source_id: "yc-webhound-news-import-9128b8cbaded"
canonical_url: "https://www.webhound.ai/news/back-to-basics"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-24T06:58:24.125345+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:26720d51a7301c1190229df77e6edf7b784eff2de9e0632ad47bc22e49d8e205"
---

# Going back to a composer-first home

Archived product note


This page preserves an April 2026 announcement about controls that have since been retired. Webhound now exposes Hound, a research harness built with DeepSeek V4 Pro and GPT-5.4 across planning, execution, verification, and assembly. Hound is not a selectable model or mode and does not resolve to a single provider backend. You choose the question and dollar budget; the budget controls research effort. See the[current guide](https://www.webhound.ai/guide) .


The short version: a few months ago we put a chat with an agent in front of the home composer. It wasn't better. We're putting the composer back.


## What we tried


Earlier this year we made the home page a chat with the Webhound agent. The pitch we wrote ourselves was: a copilot that helps you plan a run, asks clarifying questions, and chases follow-ups in conversation. We thought some people would want that.


We watched people use it. We talked to people who use Webhound a lot. We read the support thread.


## What we got wrong


For most of you, the chat sat between you and the work. You already knew what you wanted to research. Having to go through a few rounds of agent dialog before a run started was friction, not assistance. The folks who use Webhound the most told us the old direct composer was faster, less in the way, and they missed it.


That's the unflattering part to write but it's the truth. We pushed something we thought would be more useful. It wasn't.


## What's back


The home page is a composer again. A textarea, an **Output** pill (Report, Dataset, Chain, Ask), pills for **Mode** , **Model** , **Deep read** , and **Budget** , and a Send button. Type the brief, set the budget, hit send. Research starts. No middle layer.


The pills give you the things you actually want to control before a run — Plan vs One-Shot, Auto / Pro / Flash, Deep read on or off, dollar budget — without making you ask for them in conversation.


## The agent isn't gone — just internal again


The Webhound agent still does all the work it did before. Inside every session it's a Planner → Executor → Verifier loop, searching the web, reading pages, writing the document, fact-checking each cycle. When you pick Plan mode it still asks the clarifying questions. When you pick Chain it still drafts steps for you in the chain builder.


What changed is that the agent isn't the thing you talk to before research starts. It's the thing that does the research. That's where it always belonged.


## Where to find it


Sign in, hit the home page. The composer is right there. The full walkthrough lives in[the user guide](https://www.webhound.ai/guide) .


Questions, complaints, or stories about how the chat-first version annoyed you? Emailteam@webhound.ai . We read all of it.
