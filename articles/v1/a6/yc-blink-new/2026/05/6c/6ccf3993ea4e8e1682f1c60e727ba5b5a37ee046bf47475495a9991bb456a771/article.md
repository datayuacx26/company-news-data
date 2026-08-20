---
schema_version: "1.0.0"
document_id: "6ccf3993ea4e8e1682f1c60e727ba5b5a37ee046bf47475495a9991bb456a771"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/lovable-2-0-whats-new"
published_at: "2026-05-07T00:19:49+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:51:16.827560+00:00"
content_hash: "sha256:f43f7d8039b5fda5075bc861ee46c4d12d76d6ed144aa341bec2197ea0723c95"
---

# Lovable 2.0: What Changed and What It Means for Vibe Coding

Lovable 2.0 shipped in April 2025 with three meaningful changes: team collaboration, an agentic chat mode, and a security scanning layer. Here's what each actually delivers — and where the gaps remain for builders who need production-ready apps.


## What's New in Lovable 2.0


### Multiplayer Workspaces


Lovable now supports team collaboration through workspaces. Pro subscribers ($25/month) get personal workspaces with unlimited project collaborators. Teams subscribers ($30/month) can host up to 20 users in a shared workspace with centralized billing.


One important detail: collaborators use the project owner's credits, not their own. This keeps billing predictable for small teams building together.


### Chat Mode Agent


The chat agent is Lovable's biggest technical change in 2.0. It operates separately from the code editor — no automatic edits, no code changes unless you explicitly ask. Instead, it reasons across multiple steps to:


- Search files and inspect runtime logs
- Query connected databases
- Plan project architecture before touching a line of code
- Debug issues without triggering rewrites


This separation of "thinking through a problem" from "changing the code" prevents the accidental overwrites that frustrated Lovable 1.x users.


### Security Scan


When you publish, Lovable now surfaces security vulnerabilities in your app. **One caveat:** this feature only activates when Supabase is connected. It's a directionally important step toward production-safe vibe coding — but the dependency reveals something about Lovable's architecture.


### Dev Mode and Visual Edits


Dev Mode lets you edit the underlying code directly inside Lovable. Visual Edits lets you click any UI element and adjust styles without writing CSS. Both features shipped before 2.0 and have been refined — they close the gap between "AI-generated prototype" and "polished product."


### Custom Domains Built In


Over 10,000 custom domains have been connected to Lovable apps since the feature launched. Domain purchase and management now lives inside Lovable — no third-party DNS dashboard required.


Lovable landing page


Blink


## What Lovable 2.0 Still Doesn't Solve


Lovable 2.0 is a genuine improvement. But several structural constraints carry over from 1.x:


**Database still requires Supabase.** The security scan itself only works with a Supabase connection — which makes the dependency explicit. Every Lovable app needing persistent storage still requires a separate Supabase account, schema setup, and API key management.


**Auth remains external.** No built-in authentication layer exists in 2.0. You wire in a third-party provider or build it yourself.


**Credit costs accumulate.** The credit system can produce unexpected charges on complex builds. At $25/month base, heavier editing sessions require purchasing additional credits.


**Teams cap at 20 users.** For startups scaling past early-stage, this ceiling arrives faster than expected.
