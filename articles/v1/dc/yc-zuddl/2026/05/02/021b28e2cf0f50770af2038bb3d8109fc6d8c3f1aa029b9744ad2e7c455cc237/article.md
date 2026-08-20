---
schema_version: "1.0.0"
document_id: "021b28e2cf0f50770af2038bb3d8109fc6d8c3f1aa029b9744ad2e7c455cc237"
company_key: "yc-zuddl"
company: "Zuddl"
source_id: "yc-zuddl-rss-52104e166c84"
canonical_url: "https://www.zuddl.com/blog/ems-claude-plugin"
published_at: "2026-05-29T08:58:44+00:00"
first_seen_at: "2026-07-26T06:39:34.720848+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:35fcce805128569e31578ab79069a64791849b7ac7685a7482177a9699b3d669"
---

# The Definitive Claude Code Plugin for Event Marketers | Zuddl Blog

If you run B2B marketing events — customer dinners, prospect happy hours, field events at tradeshows, executive retreats, user conferences, webinars, fireside chats — most of the work isn't the event itself. It's the eight weeks of sourcing, shortlisting, chasing, scheduling, designing, briefing, and reconciling that surround it.


The event-marketing plugin packages that work into ten Claude Code skills you can call from a single conversation. Each skill is opinionated, built around the actual artifacts an event marketer produces (a venue shortlist, a workback, a budget, a stack of LinkedIn promo cards, a post-event brief), and designed to live alongside a per-event folder so the same pinned thread can run the whole program.


## What's inside the plugin


The plugin ships ten skills, grouped by where they sit in the event lifecycle.


### **Sourcing & shortlisting**


**venue-research** — The entry point for sourcing any customer- or prospect-facing event under ~150 people: dinners, happy hours, workshops, mini-conferences, ancillary tradeshow events. Returns 5–8 well-fitted venues with rationale. Handles intake for every event format and delegates to experience-research when the ask is an experience (Sphere tour, F1 suite, helicopter ride, golf with a pro) rather than a fixed-location venue.


**experience-research (internal)** — Sources VIP experiences from hospitality programs, suite brokers, motorsports corporate hospitality, museum buyouts, celebrity-led experiences, and luxury weekend retreats. Invoked automatically by venue-research or directly via /experience-research.


**speaker-research** — Build a ranked, deduplicated, competitor-filtered shortlist of 10–20 target speakers for a specific event — webinars, fireside chats, customer panels, podcast guests, virtual summits, conference sessions. Output is opinionated enough to start outreach to your top 3–5 picks the same day.


**sponsor-research** — Build a ranked shortlist of 15–40 target sponsor companies for a user conference, virtual summit, webinar series, or roadshow, each paired with the specific human inside that company most likely to own the sponsorship decision.


### **Planning & operations**


**workback-schedule** — A reverse-timeline ("T-minus") schedule for a single event with target date, projected date, and on/off-track signal for every prep task. Designed to be invoked repeatedly in a pinned thread: "we pushed X by a week", "mark Y done", "catch up on the thread", "what's left before the event".


**budget** — A line-item budget that lives in the same per-event folder as the workback. Drop in evidence — PDF invoices, image invoices, screenshots of email receipts, even screenshots of a bank/credit-card transaction list — and the skill extracts vendor and total, auto-categorizes each into the right line item, and updates Projected / Actual / Variance. Generates a read-only HTML view styled like a finance tracker.


### **Promotion & attendee experience**


**linkedin-event-promos** — Bulk-renders LinkedIn promotion graphics from a user-supplied HTML template and a CSV of speakers or sponsors. One PNG per row, across the ramp (save-the-date, 1 month out, 2 weeks out, 1 week out, day-of). Built for the moment you have 15 speakers and 8 sponsors to announce and don't want to open Figma 23 times.


**agenda-generator** — Builds and deploys a here.now-hosted conference agenda site: filterable session grid, natural-language "Great fit / Good fit" recommender, personalized agenda builder, shareable link. Also supports a marketer-driven flow where Claude curates a standalone personalized agenda for a specific VIP attendee.


**attendee-chatbot** — Spins up a brand-themed, embeddable chat widget for a single event, powered by a Claude Managed Agent the skill provisions. Answers attendee questions from PDFs you supply (agenda, FAQ, venue map, code of conduct, sponsor list, travel/visa info). Ships as a single-file JS bundle plus an embed snippet.


### **Wrap-up**


**post-event-brief** — Combines registrants-vs-attendees data with optional Salesforce enrichment (top accounts touched, hot leads, pipeline influenced, new contacts created) into a stakeholder-ready markdown brief plus an interactive HTML view. Lives in the same per-event folder as the workback and budget.


## How it actually works in practice


Skills auto-trigger from natural language — you don't need to remember the skill names. The design intent is that one pinned Claude Code thread per event can run workback-schedule, budget, and post-event-brief against the same per-event folder across the full planning window.


Some real prompts you can try right away after you install the skills:


Tool Example prompt


venue-research "We want to host our top 20 CISO customers in NYC for dinner next month."


speaker-research "Build me a lookalike list of 15 speakers similar to Jane Doe for our Q3 fireside series."


budget "Here's a screenshot of my Amex — log these three charges to the dinner budget."


workback-schedule "We pushed the venue contract by a week; what's still on track?"


linkedin-event-promos "Generate the 1-week-out speaker reveal graphics for all 12 speakers."


post-event-brief "Wrap up the Atlanta dinner — show rate, pipeline, top accounts touched."


## How to install it


This is a Claude Code plugin, so it installs through Claude Code's plugin system (not the Anthropic API or[claude.ai](http://claude.ai/) ).


Here's the[plugin URL](https://github.com/zuddl/claude-plugin-event-marketing) .


Open Claude Desktop App > Customize > Plugin > Create Plugin > Add Marketplace > Paste in the plugin URL and sync


Here’s quick help video to install it:
