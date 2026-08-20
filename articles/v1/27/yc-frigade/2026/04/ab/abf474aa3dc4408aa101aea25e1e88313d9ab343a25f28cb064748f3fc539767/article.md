---
schema_version: "1.0.0"
document_id: "abf474aa3dc4408aa101aea25e1e88313d9ab343a25f28cb064748f3fc539767"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-5962136a45f1"
canonical_url: "https://frigade.com/updates/engage-claude-skill"
published_at: "2026-04-24T00:00:00+00:00"
first_seen_at: "2026-07-21T20:57:52.614663+00:00"
fetched_at: "2026-07-28T21:45:30.754431+00:00"
content_hash: "sha256:b2a892f683cc4f28ba0483e6e02e04f32381f8aa53d4e522e602ce9d5c011d52"
---

# Build Engage Flows from Your Terminal with the Claude Skill

Frigade Engage gives you typed React components for every flow you'd build into your product: announcements, tours, checklists, banners, forms, NPS surveys, cards. They're code. They live in your repo and ship the same way the rest of your app ships.


Today we're shipping a


**Claude skill** that lets your AI coding tool build with those components for you. Describe the onboarding, tour, banner, or survey you want. Claude writes the code against the Engage SDK, wires it into your app, and creates the matching flow in Frigade. No dashboard required.


What it builds:


- **Announcements, tours, and checklists** with the right targeting and copy


- **Forms, surveys, and NPS** with responses flowing back to the dashboard


- **Banners and cards** placed where they belong in your UI


- **Linked flows** so an announcement CTA can launch a tour, or a checklist step can open a form


What you get back is normal Engage code in a normal pull request. Your team reviews it the same way they review anything else and ships it through the same pipeline, so nothing's a black box and nothing's guessed at runtime.


It's safe by default. Dev moves fast with no prompts. Anything destructive in prod requires explicit confirmation. Private keys never enter client code, and every write is logged locally for auditability.


We built it because customers asked for it. The first ask was whether Claude could build out an entire onboarding from the IDE without anyone touching the dashboard in the middle. Now it can.


Available today:


[github.com/FrigadeHQ/frigade-engage-skill](https://github.com/FrigadeHQ/frigade-engage-skill) . Drop the skill into Claude Code, run first-run setup, and tell it what you want to build. We're actively developing support for Cursor, Codex, and other coding tools. Reach out if you have a specific tool in mind.
