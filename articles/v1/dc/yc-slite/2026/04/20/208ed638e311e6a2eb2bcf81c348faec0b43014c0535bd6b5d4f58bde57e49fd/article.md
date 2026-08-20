---
schema_version: "1.0.0"
document_id: "208ed638e311e6a2eb2bcf81c348faec0b43014c0535bd6b5d4f58bde57e49fd"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/salesforce-as-a-source"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:245a142851ad0a93394e00b2f4525ebac4a618d0a0d8c4c22bcef85d1e86c280"
---

# Salesforce as a source

You can now connect **Salesforce** to Super and ask questions across your CRM in plain language — deal status, account history, pipeline, contacts, and much more. Super queries Salesforce in real-time; no data is stored in Super, so your permissions and audit logs stay exactly where they are.


[Connect Salesforce →](https://slite.slite.page/p/MH_cPxOlPHjbh4/Salesforce)


### Assistant workflow editor improvements


Building and editing assistants just got a lot simpler. Steps are now expanded by default so nothing is hidden, step names are inferred automatically from your instructions so you don't have to name them manually, and the welcome message, kickstart buttons, and context box are now clearly separated — making it much easier to see and configure what your assistant actually does. All existing assistants have been migrated automatically.


### Slack auto-sync


Super now **automatically syncs new public Slack channels** as they're created. For private channels, any team member can invite` @Super` directly, no admin needed.


To enable it: **Super → Data Sources → Slack → ⋯ → Edit Settings → Toggle on Auto-sync**


> Note: Auto-sync applies to new messages going forward. Past messages still need to be imported manually.


### Super member onboarding


New members joining your Super workspace now get a **role-based onboarding** carousel on their first visit. It figures out what they do, Support, Sales, Product, Engineering, shows them the most relevant workflows, and ends with a pre-filled query so they get a real answer immediately. They also get a personalized setup email based on which integrations they pick (Slack, browser extension, Claude MCP, ChatGPT). Making it ridiculously simple for members who join a workspace that's already set up.


### Other improvements


- **Notion source improvements** — Better markdown rendering for Notion content and fixed pagination for large Notion workspaces
- **Linear team listing no longer cached** — Newly created Linear teams now appear immediately in the data source panel


### Bug fixes


- Fixed bot messages (posted via API without a` text` field) not being indexed from Slack channels
- Fixed markdown rendering bug where brackets in source titles were breaking answer display
- Fixed Salesforce not appearing in source filters after being connected
