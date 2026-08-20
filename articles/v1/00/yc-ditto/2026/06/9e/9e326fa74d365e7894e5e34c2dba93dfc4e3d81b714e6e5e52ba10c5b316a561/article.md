---
schema_version: "1.0.0"
document_id: "9e326fa74d365e7894e5e34c2dba93dfc4e3d81b714e6e5e52ba10c5b316a561"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/now-in-beta-automate-your-content-system-setup-from-whats-live-in-your-production-codebase"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:e15fb89a94e91f6fa8b7c890802b485714d6905841e32e343890e58196c95611"
---

# Ditto Blog | Now in beta: Automate your content system setup, from what’s live in your production codebase.

Systemization — structure, guardrails, and governance — is crucial to generating and shipping better product copy. But the system itself is just a means to the end. It’s your single source of truth that works alongside the other tools you’re building in, as you generate, edit, and ship copy. We want teams to spend their time focused on outcomes (like higher quality copy, shipped faster)—not focused on building the “perfect system.”


The good news is: **the foundation to your team’s perfect system already exists** . It's sitting in your codebase, being served to your users right now. Today, we're shipping a way to take that source of truth — your production codebase — and turn it into a working content system in minutes.


## What we built


Ditto now takes you step-by-step through your content system setup. Ditto scans your codebase, identifies your user-facing strings, and sets up your content system for you — automatically, based on what's already in production.


Run a new Ditto CLI command in your codebase. Continue setup in the Ditto web app. In moments, Ditto has extracted strings, metadata, translations, and content style standards based on what made it to production. You can review these extractions to assemble a working V1 of your content system: a component library, style guides, translation management, and the metadata infrastructure to keep it all connected.


Here's what gets created:


**A V1 reusable component library.** Every user-facing string Ditto identifies becomes an entry in your reusable component library immediately. Repeat strings across the codebase are surfaced so you can start consolidating duplication right away.


Ditto will also build out the data and context around your strings based on what's already live in code: pluralization, variables, tags, existing string keys as developer IDs (or its own if none exist), and folder organization based on your product's structure.


**V1 translation management.** If localization already exists in your codebase, Ditto will detect it, set up locale variants, and wire everything in — so your existing translations are part of the system from day one.


**V1 style guide rules.** Ditto infers a first set of style rules directly from the strings it finds — not from a generic template. The focus is high-level, cross-cutting content: voice and tone, style and mechanics (punctuation, date formats, abbreviation conventions), and a terminology list of the nouns, verbs, and brand names that show up consistently in your product.


Once you have the foundations in place, the system only compounds over time. Ditto learns as you use it, surfaces recommendations, and refines over time, to generate higher quality copy every iteration and get you to final faster.


## What you unlock immediately


The moment your system is approved and set up, everything that depends on it starts working.


Style guides can now be applied in every workflow: copy generated anywhere can be drafted and review against standards in Ditto, with Magic Edit suggestions. Ditto’s GitHub Review Bot can start checking pull requests against your style guide before anything ships. You're not configuring these tools one by one — they inherit the system.


Your component library is usable right away. Repeat strings are set up as components with their own unique developer IDs, which means teams can start reusing them immediately.


## How it works


**Run the new CLI command.** Run Ditto's CLI command within your codebase and it scans all the strings — user-facing copy, localization files, variables, string keys, and more.


**Continue in the web app.** The setup guide in Ditto takes you from there. It'll ask a few basic questions about your product and anything that should inform how things get organized — surface-level context that makes the output significantly more accurate.


**Review an initial pass.** Ditto surfaces what it's inferred and asks for your feedback before building anything out.


- For the component and metadata decisions, you'll see what it's proposing and can flag strings you'd rather exclude.
- For style guide rules, you'll see a preview of the *sections and examples* of rules that will be created — not the comprehensive style guide, because you're not in fine-tuning mode yet.


The goal at this stage is getting the shape right, not perfecting every rule. You can give feedback and Ditto will do a second pass before you approve.


**Approve, and it's ready.** Once you're satisfied with the direction, Ditto processes everything and sets up your library, style guides, and infrastructure automatically. From that point forward, the system is live.


## Your live product is the starting point


This goes beyond time savings: a content system built from production strings is accurate by default. It reflects exactly what users are seeing, not what someone remembers was in the codebase. String keys match what's deployed. Localized variants reflect what's already translated. Copy components reflect the copy decisions your team has already agreed on.


Most content systems start from scratch and spend months catching up to production. Ditto’s starts from production and works forward from there.


## Join the beta


If you've been wanting to bring structure and trust to your product copy, you can have a working system in minutes. This feature is currently in beta — reach out to get started.


[Book a beta kickoff call →](https://calendly.com/d/ds55-452-gwg/ai-content-systems-beta-intro-call)


We'll walk through your setup, run the CLI together, and make sure the system reflects your product accurately before you ship it.
