---
schema_version: "1.0.0"
document_id: "f62c371cf92d247f0d23f558d22c360ed7eaf2e3193806e5b49c745da57e3fba"
company_key: "yc-stagewise"
company: "stagewise"
source_id: "yc-stagewise-news-import-0e327623c986"
canonical_url: "https://stagewise.io/news/release-week-may-11-17"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-22T14:44:14.475585+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:fdc56991740c704e0cb92edb734f0fe97366839e0244fe3567682c2c8f66b516"
---

# Release Week: May 11–17

## What Shipped Last Week


### One sidebar for all your agents


We removed the agent archive, and instead, every agent now lives in a single sidebar, ordered and searchable. No more deciding whether an agent is "active" or "archived" — search for the one you need and continue any previous session.


We also added **agent pinning** . Click the pin icon and that agent stays at the top in its own section, regardless of how many others you spin up. The agents you use every day stay at your fingertips. The rest sit below, still searchable when you need them.


### Permission mode sticks across chats


stagewise now remembers your last used **tool-call permission mode** and applies it to new agent chats automatically. If you work in Smart Approval, new agents start that way. No reconfiguring.


One exception: *Always Allow* won't carry over. Handing an agent unrestricted shell access by default is rarely a good idea. You can still switch to it manually when the task calls for it.


### Thinking blocks are no longer lost


stagewise was dropping earlier thinking content from **Anthropic Claude** and **Google Gemini** models under certain conditions. That's fixed. Claude and Gemini agents should now have an easier time staying consistent instead of circling back to the same issues.


### Slash commands survive context compression


**Slash commands** — like continuing a plan or implementing tasks — now hold their intent after the context window compresses. Agents stay on track instead of drifting when sessions run long.


### A couple of fixes


- The UI no longer flickers when scrolling on **macOS** .
- Closing stagewise used to throw a crash report because LSP servers were not shutting down correctly. That's fixed — the app quits cleanly now.
