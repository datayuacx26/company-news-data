---
schema_version: "1.0.0"
document_id: "5bf42a520af2e33c16dffa4fe9c9f1a4b112a531a78c7b7e2e7791c9a247aa89"
company_key: "yc-weweb-io"
company: "weweb.io"
source_id: "yc-weweb-io-news-import-be394dfb89cc"
canonical_url: "https://www.weweb.io/blog/our-2026-roadmap"
published_at: null
first_seen_at: "2026-08-19T20:00:32.932075+00:00"
fetched_at: "2026-08-19T20:00:35.430470+00:00"
content_hash: "sha256:bd33c7e1e7b8a85c530a296d9dfe654c47ddf35974519bf7b6ebcc414e119e1c"
---

# WeWeb’s 2026 Roadmap: Full-Stack Apps, AI Guardrails & More

💡 TL;DR


- **March:** Full-stack backend goes live with improved & new 3rd-party integrations for email, storage, and more
- **Q2:** Workspace roles & permissions + a better AI with deterministic guardrails
- **Q3–Q4:** Branching + performance/security/compliance audits
- **Ongoing:** Bug fixes and improvements


## Our direction


Before diving into the details of the roadmap, I’d like to restate our vision:


At WeWeb, we are committed to building the best platform for non-coders to build serious web products without worrying about infrastructure, migrations, security, performance, or AI unpredictability.


In other words, WeWeb’s mission is to remove technical complexity without limiting product freedom.


Making complex things feel simple is at the core of everything we do.


## What we’re building toward


We’re systematically removing the hardest parts of building web apps:


✅ **Reliable deployment (frontend + backend)** : One-click deployment of full-stack applications on managed infrastructure, or deployment to your own infra. 2026 goal: release backend in March + continuous improvement.


✅ **Environments & migrations** : Automatic editor/staging/production environments, backups, one-click rollbacks, and automated migrations (blue/green approach with WeWeb tables). 2026 goal: release backend in March + continuous improvement.


⚠️ **Collaboration at scale** : Multiple builders can already work on the same project, but we still need proper branching, workspace roles & permissions, and improved collaboration logs. This is all planned for this year. (More info below)


📅 **Security & compliance** : We’ll introduce a programmatic audit system to detect common security mistakes and enforce configurable compliance rules. Planned for Q4 2026.


📅 **Performance** : We’ll ship an AI performance audit that detects common issues, reports them clearly, and (when possible) offers automatic fixes. Planned for Q4 2026.


✅ **Beautiful, on-brand U** I: Design systems will become more central to the WeWeb experience to ensure consistent UIs at scale. Pixel-perfect manual building, Figma import, and AI-generated UI already work pretty well and will continue to improve. 2026 goal: continuous improvement.


✅ A **dvanced logic (frontend + backend)** : WeWeb is already strong for frontend logic (functions + workflows). We’re now bringing that same building experience to backend logic. 2026 goal: release backend logic in March + continuous improvement.


⚠️ **AI building with real control** : We’re building a first-of-its-kind AI to JSON to App architecture (AJA architecture). It lets users delegate work to AI while keeping the fundamentals deterministic and rule-based (design system, approved APIs, workspace rules, etc.). Planned for Q2 2026.


✅ **Removing the last no-code bottlenecks** : Custom coded components, code export (source or compiled), and keeping the platform open: any API, any SQL database, any storage provider, any auth system. 2026 goal: release new integrations monthly starting March 2026 + continuous improvement.


We have been working on this for many years and we are now very close to having all the pieces in place.


## 2026 roadmap (high-level)


With that in mind, here is the high level roadmap for this year:


- Q1: Release the new full-stack experience, including improved & new integrations for storage, email, and more.
- Q2: Release of the AI to JSON to App agentic system, bringing roles & permissions at workspace level, enabling agentic access to WeWeb (i.e. external AI agents will be able to use WeWeb).
- Q3–Q4: Release branching + audit features (performance, security, compliance).
- Ongoing: Bug fixes and improvements based on feedback.


### A note on SSR and GPT/MCP apps


SSR won’t be part of the 2026 roadmap. We believe it will make sense for WeWeb long-term to unlock more use cases, but we want to stay focused and ship the full-stack + branching + workspace roles & permissions + audit foundation + AJA architecture first.


GPT Apps / MCP Apps are also out of scope for now due to limited demand and bandwidth.


Now let’s dive into the details for this quarter:


### The full-stack experience


We will offer the following out-of-the-box features:


- A scalable database with a scale-to-zero option, automatic database migrations when deploying to production, and a great UI to manage tables, filtered views, and many-to-many relationships
- A scalable server to run any kind of serverless functions (backend logic), with a clean UI to configure backend workflows, define API endpoints, and review your Swagger
- An authentication system
- A storage system (and new third-party integrations)
- An email system (and new third-party integrations)


Users will be able to use these features à la carte. This will unlock powerful combinations, such as using Supabase or Airtable as the database while relying on the WeWeb server for backend logic, authentication (no RLS headaches), and overall security. Or using the WeWeb database and backend together with an external storage provider, and so on.


And of course: all of this is optional. WeWeb will still fully support frontend-only projects.


We originally planned to release the WeWeb backend in January, but we expanded the scope to make it significantly more powerful and useful. Go-live is now planned for March.


### The AI experience: vibe coding under control


We’re working hard on it, and we will release it as soon as we can. We’ve committed to Q2, and our goal is early Q2 (April/May).


The AJA architecture will enable what we call “vibe coding under control” and give you much more granularity and control when building with AI thanks to deterministic guardrails. We are also entirely revamping our agentic system to get (much) better outputs.


The AI will be full-stack, able to build both the UI and the backend.


We hope this roadmap excites you as much as it excites us!


Thank you for being part of our journey. Your feedback shapes everything we build.


Whether you've been with WeWeb from the beginning or you're just discovering us now, we're grateful to have you here.


‍
