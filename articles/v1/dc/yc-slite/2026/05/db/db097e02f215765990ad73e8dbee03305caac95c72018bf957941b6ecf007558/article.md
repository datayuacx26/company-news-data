---
schema_version: "1.0.0"
document_id: "db097e02f215765990ad73e8dbee03305caac95c72018bf957941b6ecf007558"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/super-is-now-inside-slite"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:95075a4692f90725a5d6c098e0064c2550a0ec373923ede80a1455e9f91a6ab8"
---

# Super is now inside Slite

If your team uses both Slite and Super, you no longer need to switch between two apps. Super is now embedded directly inside the Slite sidebar, so you can ask questions, search across all your connected tools, and get AI-powered answers without ever leaving your docs.


- **For Super-only customers** , the experience stays essentially the same, with only minor UX and naming adjustments.
- **Slite-only customers** won't notice any difference.


Alongside this, Assistants, Digests, and Buttons are now all grouped under **Agent workflows** in the sidebar, aligning with how they're presented on the website. They share a unified list layout, and the Buttons view in particular gets a cleaner look: it now shows usage counts and a tidier editing experience.


### BigQuery as a data source


You can now connect BigQuery to Super and ask questions against your data warehouse in plain language, no SQL required. Super queries your tables live, so answers are always up to date. BigQuery can also be enabled as a source on any Assistant, and is queryable through the Super API. No data is stored in Super; your BigQuery permissions stay exactly where they are.


[Connect BigQuery →](https://slite.slite.page/p/D_wpXQysKZBLSv/BigQuery)


### Other improvements


- History threads now have a **copy link** and **open in new window** option in their context menu, making it easier to share or revisit specific conversations.


### Bug fixes


- Fixed a bug where Asana source setup was failing for members due to incorrect OAuth permissions — connecting Asana as a source now works correctly regardless of the member's role.
- Fixed Linear integration hitting API rate limits during tile syncs and webhook bursts — Linear data now stays reliably up to date without throttling errors.
