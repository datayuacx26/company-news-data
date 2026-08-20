---
schema_version: "1.0.0"
document_id: "f1eb201b2bd824c85b2459fb4c40b5cc3082c5811389e11512e35d866ebf16af"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/lovable-creative-engine-update"
published_at: "2026-05-27T12:48:29+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:7858405fdeec2473b76610da3299c50cb2b0f888ac05957724dcd501d88d198e"
---

# Lovable's Creative Engine Update: Multi-Model Routing, Design Previews — and What It Means for Builders

## How Blink Compares on These Dimensions


Multi-model routing is table stakes at[Blink](https://blink.new/blog/%5BREDACTED%5D) . The platform launched with 200+ AI models — Claude, GPT-4o, Gemini, Mistral, and others — selectable per project or workspace. There's no "engine" that needs to be built; the routing has always been user-accessible. This is one area where the Creative Engine is Lovable catching up, not Lovable pulling ahead.


The more important question is what happens *after* the model generates the code.


Lovable's output is frontend-first. When an app needs a real database — user profiles, stored data, relational queries — you add Supabase. That's a separate account, a separate billing relationship, and a config step. When Supabase has an issue, you debug it in Supabase's dashboard, not Lovable's. The same is true for production auth and managed hosting.


Blink includes the database, auth, and hosting in one place. No external accounts. No config. You describe the app, and the full stack — frontend, backend, database, auth — ships together in one conversation. For builders who've spent afternoons troubleshooting Supabase row-level security policies through a Lovable chat interface, this difference is not marginal.


On design previews, Blink uses a different philosophy: iterative refinement in context. Instead of choosing from three upfront variants, you see the output and shape it from there. Both approaches work — the right one depends on whether you prefer to decide before or during the build.


On Skills: Blink workspace skills use the same markdown format as Lovable's implementation.` SKILL.md` , a name, a description, instructions. Teams write a skill once and every project in the workspace picks it up. If you've already written skills for Lovable or Cursor, they transfer directly.


Blink landing page — full-stack AI app builder with 200+ models, database, auth, and hosting included


*Blink landing page — full-stack AI app builder with 200+ models, database, auth, and hosting included from day one*


---


## The One Gap the Creative Engine Didn't Close


The Creative Engine is a front-of-house improvement. The agent is smarter. The design step is better. The model composition is more sophisticated.


But the infrastructure question — where does the database live, who owns the auth, what happens when you need server-side logic — didn't change.


Lovable positions itself as a full-stack builder. In practice, a production Lovable app is a frontend application with Supabase wired in. That's a real stack. But it's not the same as a platform that owns the full layer.


This matters most when something breaks. A Supabase quota issue, a misconfigured auth policy, a row-level security rule blocking queries — all of these get surfaced in Lovable's chat, but fixed in Supabase's console. You end up context-switching between two products for one app.


Blink is a different bet: one product owns the full stack. See[Blink vs Lovable](https://blink.new/blog/%5BREDACTED%5D/blog/blink-vs-lovable) for a more detailed breakdown of how that plays out across different project types.


---


## Pricing


**Lovable** runs on a credit model. Free tier includes limited daily credits. Pro is $25/month per workspace (100 credits/month, with daily top-ups). Business is $50/month. Complex, multi-step builds consume credits faster — a long session refining a dashboard can exhaust a week's allocation. Supabase on top runs $0–$25+/month depending on usage. A builder running Lovable Pro plus Supabase starts at $50/month minimum for a production app.


**Blink** starts free — no credit card required. Paid plans include the full stack: database, auth, hosting, and 200+ models. One bill instead of multiple services. For the same capabilities as Lovable Pro plus Supabase, Blink's all-in pricing typically works out cheaper.


For a broader look at how costs compare across the category,[best AI app builders](https://blink.new/blog/%5BREDACTED%5D/blog/best-ai-app-builders) has the full breakdown.


---


## Who Should Build on Blink Instead?


If you're debugging Supabase issues from inside a Lovable chat and wishing that conversation happened in one place — that's the signal.


If you're running Lovable Pro and a separate Supabase account and it feels like two products sutured together — Blink was built to eliminate that seam.


If you want multi-model routing, you already have it in Blink. If you want workspace Skills, same. What you additionally get is a full stack that ships as a single thing — no outside database, no outside auth, no outside hosting.


[Blink.new](https://blink.new/blog/%5BREDACTED%5D) — database, auth, and hosting included. Free to start.


---


## FAQ


Yes, in two ways. First, routing different steps to different models genuinely improves output quality — a model that's better at layout decisions handles layout, a model that's better at code generation handles code. Second, design previews reduce wasted build cycles for open-ended prompts. Both are real improvements. The caveat:[Blink](https://blink.new/blog/%5BREDACTED%5D) has had 200+ model support from day one. Multi-model isn't a new category capability — it's Lovable adopting a pattern the market has had for a while.


Yes.[Blink](https://blink.new/blog/%5BREDACTED%5D) workspace skills use the same markdown format —` SKILL.md` with a name, description, and instructions. Skills apply automatically when a task matches the description, or invoke manually with` /skillname` . They're workspace-level, so everyone on the team uses the same playbooks. The Blink agent reads and applies them the same way Lovable's does.


Yes.[Blink](https://blink.new/blog/%5BREDACTED%5D) handles full-stack web apps — landing pages, SaaS tools, internal dashboards, user-authenticated apps, payment flows, database-backed applications. The same categories Lovable targets. The structural difference is that Blink includes the database and auth layer in the platform; Lovable requires adding Supabase separately. For simple frontend builds, the difference doesn't surface. For anything with user accounts or persistent data, it matters quickly.


The infrastructure dependency. Lovable's Creative Engine, Skills, and Design Guidance are all improvements to how the agent builds the frontend. But the moment a project needs a real production database or custom auth, you're still setting up Supabase. That hasn't changed. Blink builds the full stack into the platform from the start — no external database account, no separate auth config. For more on this,[Lovable alternatives](https://blink.new/blog/%5BREDACTED%5D/blog/lovable-alternatives) covers the options.


Lovable Pro is $25/month for the workspace. That covers 100 credits/month with daily top-ups. For a production app, add Supabase: free tier has limits, and meaningful usage starts at $25/month. Total: $50+/month. Blink's paid plans cover the full stack in one bill — database, auth, hosting, and 200+ models. For a side-by-side comparison with v0 in the mix,[Lovable vs v0](https://blink.new/blog/%5BREDACTED%5D/blog/lovable-vs-v0) has the detailed breakdown.


Not natively. Lovable connects to Supabase for production database and auth — you set up a Supabase account, wire it into your Lovable project, and manage both. This works, but it means two products, two dashboards, and two billing relationships for one app.[Blink](https://blink.new/blog/%5BREDACTED%5D) includes the database automatically. You don't set up Supabase. You describe your app, and the database is there — same conversation, same product, same bill.


Lovable's design previews generate three directions before you commit — useful when you're not sure which visual path to take.[Blink](https://blink.new/blog/%5BREDACTED%5D) uses iterative refinement: build, see the actual output, shape it in context. Neither is strictly better — the right approach depends on whether you prefer to decide upfront or react to real output. If you've ever spent time choosing between abstract mockups only to find none of them quite worked once built, iterative refinement may save more time overall.


[Blink](https://blink.new/blog/%5BREDACTED%5D) for most first-time builders. Both platforms are designed for non-technical users. The practical difference shows up the moment your app needs user accounts or stored data — almost every real app does. With Lovable, you add Supabase and navigate two products. With Blink, the database and auth are already there. One fewer thing to configure on your first build is a significant reduction in friction.


---


Lovable's May 2026 updates are genuinely good. The Creative Engine makes the agent smarter on complex builds. Design previews cut the feedback loop on visual direction. Skills solve a real repetition problem that every builder who's re-explained their conventions knows well.


What didn't change: Lovable still needs Supabase for production database and auth. The Creative Engine routes prompts to better models, but it doesn't route database queries to a managed backend Lovable owns.


Blink is the full-stack alternative — 200+ models, workspace skills, and the complete infrastructure layer in one place.


Start building at[blink.new](https://blink.new/blog/%5BREDACTED%5D) .
