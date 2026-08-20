---
schema_version: "1.0.0"
document_id: "3d7ffe2d584764e8af7163575b53e29bbe1978d80b6f809cd2bfa4abad6dfc1d"
company_key: "yc-roark"
company: "Roark"
source_id: "yc-roark-news-import-2870a719ae4c"
canonical_url: "https://roark.ai/blog/roark-v2"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-22T12:05:46.538010+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:d3f9a7856b8ff1123986679c72f2e3294b67c5069220a1f8becf17dac3719cc1"
---

# This is Roark v2

Today we're introducing Roark v2.


Not a redesign. Not a new logo bolted onto the old product. We rebuilt Roark from the first pixel to the last query. Every screen, every report, every interaction, rebuilt from scratch around one question: what does it actually feel like to test and trust a voice agent in production?


The answer is the most significant release in our history. And it is live right now.


Watch the Roark v2 launch film


## Rebuilt from the ground up


We didn't refactor Roark. We rebuilt it.


The entire application is new. New foundation, new navigation, new defaults, all shaped by a year of watching real teams create agents, run simulations, and chase down failing checks. Metrics are the center of gravity now. The AI assistant is one keystroke away from any page. The left nav is organized around the loop you actually run: Monitor, Measure, Simulate, Build.


This is what "rebuilt from scratch" really means. We kept everything you loved, and threw out everything that got in your way.


## A completely new look


Roark v2 is a full rebrand and a full rebuild, at the same time.


The new interface is calmer, faster, and easier to read. Information has room to breathe. The controls you need are where you reach for them. And yes, dark mode is here, designed as a first-class experience, so the people who live in Roark at 2am finally have a home that looks the part. Even the docs are rebuilt, rethemed, and rewritten from the ground up at[docs.roark.ai](https://docs.roark.ai/) .


It is the first time the product looks as considered as the work it does.


## Simulations, built on templates


This is the part I am most excited about.


Testing a voice agent used to mean building a run from a blank page every time. Not anymore. Simulations in Roark v2 start from templates: pick a preset like Flow adherence, Red teaming, or Conversation quality, set your pass and fail thresholds per metric before the run, and go. The template carries the criteria, so the outcome is graded inline the moment the run finishes. Every new workspace ships with system flows and templates out of the box, so there is something real to run on day one.


Starting a simulation run from a template, with Flow adherence, Red teaming, and Conversation quality presets


Templates sit on top of a bigger idea we shipped alongside the rebuild: Customer Flows. Design what a customer actually does once, then run it against any agent.


- **Scripted or improv.** A scripted variant follows a graph you drew. An improv variant follows a plain-text brief and lets the persona ad-lib.
- **Generate flows with Roark.** Paste a transcript, pick a few real calls, or just describe the intent, and the assistant recommends improv vs scripted and builds it for you.
- **Multilingual fan-out.** 34 system personas across 17 languages, plus code-switching variants. Attach a flow once, run it once per language.
- **Test flow in one click.** Finish a flow and the Test flow button drops you straight into a pre-filled run, agent and variants already attached.


And when a run is live, you watch it happen. Live Simulation View opens into a mission-control board with per-call status, elapsed timers, and streaming transcripts, turn by turn, so you can listen in, cut a bad call short, or re-run a single case on the spot.


## Reporting, rebuilt for speed


Reporting was the heart of the old Roark, and it was showing its age. So we tore it down and built it again.


The new reporting engine is dramatically faster, and far more useful. Compare up to five checks or metrics across up to five prior runs on a radar you can actually read. Get a column per metric and per check, not just the first three. Tap any check to filter the calls table. There is even a new` agent_responsive` check, computed on every simulation for free, that catches runs where your agent went silent while the customer was still talking. And load testing now runs ten times the iterations at no extra cost.


Reporting is no longer the thing you wait on. It is the thing you move through.


## Intelligence, everywhere you look


The most powerful part of Roark v2 is what it does on its own.


Roarky, your on-screen guide and analyst. Ask in plain English, "track how often callers ask about pricing," and Roarky builds the whole chain: it finds or creates the metric, starts a collector so it runs on every new call, and drops a chart on your dashboard. Ask "walk me through running my first simulation" and it drives the navigation, spotlighting each control as it goes.


The in-app AI assistant pinned to the right rail


AI-detected issues. Recurring failures across your production calls now cluster themselves into stable, Sentry-style issues automatically. No alert rules to configure. The patterns find you.


Recurring behavioral failures grouped into stable, AI-detected issues


The end-to-end prompt optimizer. After a simulation settles, Roark reads the failing checks, the worst calls, and your current prompt, then proposes targeted edits with anchors and evidence. Fix it, re-run the suite, and get proof it moved.


The prompt optimizer suggesting evidence-grounded edits after a simulation run


Knowledge bases. Give a judge metric the real rubric and a compliance metric the real policy document, so Roark scores against your source of truth instead of an approximation of it.


## Confidence in production


Testing is half the story. Roark v2 also tells you the truth about what is happening live.


Health checks and uptime. Roark now answers the simplest question about your agent: is it answering right now? Pick a cadence, and every monitor renders as a clean status-page card.


Health checks page with status cards and 24-hour probe history


Deterministic or adaptive, your call. A branching-mode toggle lets one flow either fan out into a simulation per path, or collapse into a single call the agent has to navigate live.


Flow editor with the deterministic and adaptive branching-mode toggle


Fine-grained PII redaction lets you choose exactly which categories to redact across both audio and transcript. And in-car detection, a new system metric, flags when a caller is in a vehicle, so you understand the real conditions your agents work through.


## A new plan, and pricing that gets out of the way


We rebuilt the business model too, and gave it a new front door.


Roark v2 introduces a brand new **Pay-as-you-go** plan: fully self-serve, usage-based in plain dollars, and free to start. Sign up, run your first simulation, and pay only for what you actually use, with no sales call and no contract. It sits alongside Team and Enterprise, and there is no feature gating between tiers, so you get the whole platform on every plan.


## This is just the beginning


Roark v2 is the platform we always wanted to build. Faster, smarter, and more beautiful than anything we have shipped before, with simulation templates that get you testing in seconds, an intelligence layer that does the tedious work for you, and a testing loop that earns your confidence before launch.


It is all live today, and it is free to try. Open Roark, switch on dark mode, and see what the whole thing feels like now.


Welcome to Roark v2.
