---
schema_version: "1.0.0"
document_id: "a6c556c2a547f20e2cbad135c6094af55e96afb8bf40d4b0cc5037e8cd015c48"
company_key: "yc-gauge"
company: "Gauge"
source_id: "yc-gauge-news-import-b2f5831b5413"
canonical_url: "https://www.withgauge.com/blog/agent-led-growth/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T02:01:39.875048+00:00"
fetched_at: "2026-08-06T02:01:42.018836+00:00"
content_hash: "sha256:be92def9a534cc8bc4858536e5c384b59ea5775e19c623708a79130112f14665"
---

# What is Agent Led Growth? How Coding Agents Choose Software

A developer delegated a real vendor decision to an agent this morning, and the whole thing was over before anyone finished making coffee. For sixty years, marketing meant standing where humans already are: billboards, then telemarketing, then the browser tab, then the feed. Every channel was a new room to walk into and start talking. This one is not a room. There is a new gatekeeper between your product and the people who would use it, it does not watch ads, and it has never heard of you unless you made yourself legible to it. The biggest decisions your customers make are now made by coding agents in the terminal. We call this Agent Led Growth, and this piece breaks down how it works and how to win.


Here is the whole decision. The developer asks for email sending and names no vendor. The agent thinks for thirty seconds, matches one provider on a three-line setup, runs a single install, and reports back that it is wired up. No other vendor is named, no page is visited by a human, and nothing about it lands in your analytics.


## **What Agent Led Growth Actually Is**


Agent Led Growth (ALG) is a new marketing channel built around which tools coding agents recommend and how they implement and install those tools. It's about optimizing so your tool is the one that gets recommended, and then making sure it is built to be implemented correctly.


This new channel is comparable to organic search channels like SEO (Search Engine Optimization) and[AEO (Answer Engine Optimization)](https://www.withgauge.com/aeo) , except instead of Google and chat interfaces, it happens in the terminal.


## **The Two Sides to Agent Led Growth**


There are two aspects to Agent Led Growth. They are separate problems and they fail in different ways.


**ARO (Agent Recommendation Optimization). Get recommended.**


ARO is about influencing what tools and brands coding agents recommend and build with. The prompt sets the goal, the session context narrows it, and the agent researches from there and picks one. That pick is the whole decision. The developer sees what won and never learns what lost.


Two things move it:


- **Shape model priors.** Live in the training data of the next models.
- **Win agent web search.** SEO and AEO for the queries agents actually run.


**AX (Agent Experience). Get implemented properly.**


AX is how well an agent can actually use your product once it picks you. Everything the agent touches counts: your docs, your setup, your API, your errors, your CLI. If it can get from install to working code without a human stepping in, your AX is good. If it guesses wrong, gets stuck, or gives up, it is not.


Three things move it:


- **Agent-readable docs.** Examples and recipes, not prose.
- **Frictionless onboarding.** Seamless API key setup and installation.
- **Predictable API.** Types, SDKs, and errors it can self-correct from.


Fail ARO and you are never in the running. Fail AX and you get picked, break during the install, and get replaced before the developer has finished reading the diff. ARO gets you into the file. AX keeps you there.


## **How Agents Actually Pick (ARO)**


Recommendation is not one moment. It is a pipeline, and you can get dropped at any stage.


#### **Stage 1: The Model's Knowledge**


What the model believes before anything is typed. Baked in at training.


- **Slowest to move, most valuable to own.** Hard to influence, durable once you are in, re-rolled every release.
- **A prior, not a lookup.** The model is recalling, not searching.
- **Different per model.** Claude, GPT, and Gemini hold different defaults for the same category.


#### **Stage 2: The Context**


Everything in front of the model when it decides. Set per session. Narrows the field before a single search runs, and often settles it.


- **The prompt.** A named tool ends the decision before it starts. An unnamed one hands it to the agent for its recommendation.
- **The codebase.** Whatever is already wired in becomes the default, already-embedded tools guide next steps.
- **Skills and rules files.** CLAUDE.md and AGENTS.md are standing orders, applied on every run without being asked for.
- **User memory and preferences.** Where the agent learns the developer's taste. Standing preferences on style, language, and coding patterns.


Each stage runs on a different clock, and the clock decides how you invest against it. Model knowledge is the hardest to influence and gets re-rolled every release, but the effect lasts longest once you are in. Context is instant and resets every session. Research is the fastest surface to move, and it runs live on every query.


#### **Stage 3: Research**


If the agent decides to do web research, this is where strong SEO and AEO/GEO pay off, just against a different surface. Live, per query.


- **Rank for the model's terms, not yours.** The agent writes the queries. They are longer, more literal, and task-shaped.
- **Multiple searches, run in parallel.** Several angles at once, not one query.
- **Product documentation.** Ranking gets the agent to the page. The page has to explain how to implement, concretely enough to write working code from.
- **Your docs are the fetch.** Across 500 runs, docs took 55% of all page fetches. Source code 18%, package registries 11%, third party content 5%. The blogs and listicles you have been buying are rounding errors here.
- **Setup pages do the heavy lifting.** Of the docs fetched: setup 26%, README 18%, quickstart 15%, API reference 13%, package registry 11%, dependencies 1%. Setup, README, and quickstart together are nearly 60%, which is the first ten minutes of using your product.


## **How to Optimize for Agent Led Growth**


**Track.** Understand who coding agents recommend, broken out by agent and model, and where they go to research. Your share of voice against competitors, which sources get searched and fetched, and where your tool gets implemented.


**Analyze.** You need to be able to go inside the runs. Every session leaves a trace, and that trace is the only place the agent experience is visible. You should know what the agent searched, which sources it opened and which it skipped, what it read from your docs, and whether it named you, a competitor, or nothing. Then the build. What it installed, which version of your API it wrote against, whether the integration worked, what it guessed wrong, where it got stuck, and whether it recovered or gave up on you.


**Act.** Fix what the runs showed you, then re-run the same prompts to see if it worked. Some fixes are documentation: the setup step agents keep missing, the version the model thinks is current, the quickstart that skips what they needed. Some fixes are the product: config that should not need a decision, errors that leave the agent stuck, an install that takes six steps instead of one. Either way you make the change, run the same prompts again, and see if agents behave differently.


**Example KPIs to Track:**


- **Share of voice.** How often agents name you versus competitors.
- **Fetch rate.** Whether your docs get opened, and which pages.
- **Install rate.** How often a session ends with you in the project.
- **Mention to install conversion.** Where you leak between getting named and getting used.
- **Integration success rate.** How often the agent ships something that works without human correction.


## **How Gauge Helps With ALG (Agent Led Growth)**


[Gauge](https://www.withgauge.com/agents) is a platform purpose built for Agent Led Growth. It runs real coding agents against real prompts and repositories in isolated sandboxes and captures the full trace of every session: every search, every page fetched, every package installed, every file written. That becomes the numbers above, sliced by agent, model, language, framework, greenfield versus existing codebase, and branded versus unbranded prompts. The splits matter more than the averages, because you can dominate on Claude in TypeScript and be invisible to Codex in Python.


You can open any run and read what the agent did, from the first search to the last line, including the moment it chose a competitor and what it had just read. Failures get grouped across sessions, so one missing setup step breaking fourteen runs surfaces as a single ranked item with fourteen pieces of evidence. That ranking tells you whether a fix is worth a sprint.


Then you act and verify. Gauge writes and publishes the documentation fixes and hands the product fixes to your team with the sessions attached. Re-run the same prompts and see if the number moved. You can also replay one session with a single variable changed, forking at the point the agent fetched your page and swapping the content, same repo and model, which proves your edit caused the result instead of correlating with it.


It runs on a schedule, because your numbers come from model weights that change several times a year. A new release shifts defaults, research habits, and what the model believes about your category. On a standing cadence a regression shows up as a trend break in days instead of a quarter of churn you cannot explain.


## **Conclusion**


- **A new channel exists, and it runs in the terminal.** Coding agents now choose and install the tools developers build with. That is a marketing channel, whether or not anyone at your company owns it.
- **It works like organic search, but the buyer is a machine.** SEO won Google, AEO won chat, ALG wins the agent. Same discipline, new surface.
- **Winning means two things, not one.** Get recommended, then get implemented correctly. Most teams are only thinking about the first, and the second is where wins turn into losses.
- **Your docs are the product now.** Agents read to execute, not to be persuaded. Setup, README, and quickstart carry more weight than anything you publish for humans.
- **You cannot manage what you cannot see.** None of this shows up in your analytics. The only way to know where you stand is to run the agents and read what they did, which is what[Gauge](https://www.withgauge.com/agents) is built to do.
- **It is a loop, not a project.** Track, analyze, fix, re-run, and do it again when the models change.
- **The window is now.** Every codebase an agent fills with a competitor is one you probably do not win later.
