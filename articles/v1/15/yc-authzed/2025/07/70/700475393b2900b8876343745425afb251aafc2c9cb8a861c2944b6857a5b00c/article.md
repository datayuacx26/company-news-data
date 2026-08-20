---
schema_version: "1.0.0"
document_id: "700475393b2900b8876343745425afb251aafc2c9cb8a861c2944b6857a5b00c"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/coding-with-ai-my-personal-experience"
published_at: "2025-07-16T08:21:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:17f4937933beb2beaf3bef56e5004f1cd0c2e3f5e8fea004357a61770df592c5"
---

# Coding with AI: My Personal Experience

I’ve been in tech for over 20 years. I’ve written production code in everything from Fortran to Go, and for the last five of those years, I’ve been a startup founder and CEO. These days, I spend most of my time operating the business, not writing code. But recently, I dipped back in. I needed a new demo built, and fast.


It wasn’t a simple side project. This demo would ideally have multiple applications, all wired into SpiceDB, built with an obscure UI framework, and designed to show off what a real-world, multi-language, permission-aware system looks like. Naturally, I started thinking about who should build it.


Should I ask engineering? Probably not a good idea since I didn’t want to interrupt core product work. What about an intern? Too late in the year for that. Maybe a contractor? I’ve had mixed results there. Skills tend to be oversold, results can fall short, and just finding and vetting someone would take time I didn’t have.


Just prior to this, Anthropic had just released Claude Code and Claude 4. A teammate (with good taste) had good things to say about the development experience, and internet consensus seems to be that (for today at least) Claude is kind for coding models, so I figured I’d give it a try. I’m no novice to working with AI: I have been a paying customer of OpenAI’s since Dall-E and ChatGPT had their first public launches. At AuthZed we also make extensive use of the AI features that are built into some of our most beloved tools, such as: Notion, Zoom, Figma, and GitHub. Many of these features have been helpful, but none felt like a game changer.


## Getting Started


At first, I wasn’t sure how much Claude Code could take on. I didn’t know how to structure my prompts or how detailed I needed to be. I started small: scaffold a project, get a “hello world” working, and set up the build system. It handled all of that cleanly.


Encouraged, I got a little overconfident. My prompts grew larger and fuzzier. The quality of output dropped quickly. I also didn’t have a source control strategy in place, and when Claude Code wandered off track, I lost a lot of work. It’s fantastically bad at undoing what it just did! It was a painful but valuable learning experience.


## Figuring Out a Process


Eventually, I found my rhythm. I started treating Claude Code like a highly capable but inexperienced intern. I wrote prompts as if they were JIRA tickets: specific, structured, and assuming zero context. I broke the work down into small, clear deliverables. I committed complete features as I went. When something didn’t feel right, I aborted early, git reverted, and started fresh.


That approach worked really well.


By the end of the project, Claude Code and I had built three application analogues for tools that exist in the Google Workspace suite, in three different languages! We wrote a Docs-like in Java, a Groups-like in Go, and a Gmail-like in Javascript, and a frontend coded up in a wacky wireframe widget library called[Wired Elements](https://wiredjs.com/) . Each one was connected through SpiceDB, shared a unified view of group relationships, and included features like email permission checks and a share dialog in the documents app. It all ran in Docker with a single command. The entire effort cost me around $75 in API usage.


Check it out for yourself:[https://github.com/authzed/multi-app-demo](https://github.com/authzed/multi-app-demo)


Could I have done this on my own? Sure, in theory. But I’m not a UI expert, and switching between backend languages would have eaten a lot of time. If I’d gone the solo route, I would’ve likely over-engineered the architecture to minimize how much code I had to write, which might have resulted in something more maintainable, but also something unfinished and way late.


## Phenomenal Cosmic Power


This was a different experience than I’d had with GitHub Copilot. Sometimes people describe Copilot as “spicy autocomplete”, and that feels apt. Claude Code felt like having a pair programmer who could actually build features with me.


My buddy Jason Hall from Chainguard[put it best](https://www.linkedin.com/posts/imjasonh_ai-coding-agents-are-like-giving-everyone-activity-7338321865783869440-07AL) in a post on LinkedIn: “AI coding agents are like giving everyone their own mech suit.” and “...if someone drops one off in my driveway I'm going to find a way to use it.”


For the first time in a long while, I felt like I could create again. As a CEO, that felt energizing. It also made me start wondering what else I could personally accelerate.


Of course, I had some doubts. Maybe this only worked because it was greenfield. Maybe I’d regret not being the expert on the codebase. But the feeling of empowerment was real.


## The Next Project: CRM Migration


At the same time, we had a growing need to migrate our sales CRM. We’d built a bespoke system in Notion, modeled loosely after Salesforce. Meanwhile, all of our marketing data already lived in HubSpot. It was time to unify everything.


On paper, this looked straightforward: export from Notion, import into HubSpot. In reality, it was anything but. Traditional CRM migrations are done with flattened CSV files; that wouldn’t play nicely with the highly relational structure we’d built. And with so much existing marketing data in HubSpot, this was more of a merge than a migration.


I’ve been through enough migrations to know better than to try a one-shot cutover. It never goes right the first time, and data is always messier than expected. So I came up with a different plan: build a continuous sync tool.


The idea was to keep both systems aligned while we gradually refined the data. That gave us time to validate everything and flip the switch only when we were ready. Both Notion and HubSpot have rich APIs, so I turned again to Claude Code.


## The Results


Over the course of a week, Claude Code and I wrote about 5,000 lines of JavaScript. The tool matched Notion records to HubSpot objects using a mix of exact matching and fuzzy heuristics. We used[Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) to help with tricky matches caused by accented names or alternate spellings. The tool handled property synchronization and all the API interactions needed to link objects across systems.


The cost came in at around $50 in Claude Code credits.


Could I have done it myself? Technically, yes. But it would have taken me a lot longer. I’m not fluent in JavaScript, and if I had been writing by hand, I would’ve insisted on TypeScript and clean abstractions. That would have been a waste of time for something we were planning to throw away after the migration.


## My Take on AI Coding


Our current generation of coding agents are undeniably powerful. Yes, they’re technically still just a next-token predictor, but that description misses the point. It’s like saying[Bagger 288](https://www.youtube.com/watch?v=azEvfD4C6ow) is “just a big shovel.” Sure, but it’s a shovel that can eat mountains.


I now feel confident taking on software projects again in my limited spare time. That’s not something I expected to feel again as a full-time CEO. And the most exciting part? This is probably the worst that these tools will ever be. From here, the tools only get better. Companies like OpenAI, with Codex, and Superblocks are already riffing on other possible user experiences for coding agents. I’m keen to see where the industry goes.


It also seems clear that AI will play a bigger and bigger role in how code gets written. As an API provider, we’re going to need to design for that reality. In the not-too-distant future, our primary users will likely be coding agents, not just humans.


## Looking Ahead


We’re in the middle of a huge transformation, not just in software, but across the broader economy. The genie is out of the bottle. Even if the tools stopped improving tomorrow (and I don’t think they will) there’s already enough capability to change the way software gets built.


I’ll admit, it’s a little bittersweet. For most of my career, I have self-identified as a computer whisperer: someone who can speak just the right incantations to make computers (or sometimes whole datacenters) do what I need. But like most workplace superpowers, this one also turned out to be a time-limited arbitrage opportunity.


What hasn’t changed is the need for control. As AI gets more capable, the need for clear, enforceable boundaries becomes more important than ever. The answer to “what should this AI be allowed to do?” isn’t “more AI.” It’s strong, principled authorization.


That’s exactly what we’re building at AuthZed. And you’ll be seeing more from us soon about how we’re thinking about AI-first developer experience and AI-native authorization.


Stay tuned.


On this page


- Getting Started
- Figuring Out a Process
- Phenomenal Cosmic Power
- The Next Project: CRM Migration
- The Results
- My Take on AI Coding
- Looking Ahead


## Related


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company AuthZed at EIC 2026: Building the Infrastructure Layer for Enterprise AI Next week, AuthZed attends EIC 2026 in Berlin. Stop by Booth #53, join our dinner with Axalon Partners on Tuesday, May 19th, and hear AuthZed CEO Jake Moshenko speak on Thursday, May 21st about treating authorization as shared infrastructure. May 15, 2026 · 4 min](https://authzed.com/blog/authzed-at-eic-2026)[Company AuthZed at EIC 2026: Building the Infrastructure Layer for Enterprise AI Next week, AuthZed attends EIC 2026 in Berlin. Stop by Booth #53, join our dinner with Axalon Partners on Tuesday, May 19th, and hear AuthZed CEO Jake Moshenko speak on Thursday, May 21st about treating authorization as shared infrastructure. Melissa Smolensky · May 15, 2026 · 4 min](https://authzed.com/blog/authzed-at-eic-2026)
