---
schema_version: "1.0.0"
document_id: "2f0d4cf54abbf26be6855ef2b5ddeeb9fcb09dcbae8a8da0e346354a6efb03b9"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/a-simpler-ai-surface-ai-studio-is-retiring"
published_at: "2026-06-23T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:10:52.268184+00:00"
content_hash: "sha256:a86b2a08d3902b4ec954453925d00c6e64a6ef7a5d7284aa431ab5896aaf189d"
---

# A Simpler AI Surface: AI Studio Is Retiring

We're simplifying Cosmic AI down to three clear surfaces: **Agents** , **Workflows** , and **Code > Build with AI** . AI Studio is being retired and everything it did now lives in one of those three places. At the same time, "Team Agents" are now just **Agents** , so there's one consistent name for the AI teammates you chat with, schedule, and chain into workflows.


The Agents you've already created keep running exactly as before. Standalone automations (Content, Code, and Computer Use) are being folded into Agents: they stay reachable during a migration window so you can review and move them over, and the old Automations surface is removed on **July 31, 2026** . The result is more simplicity and clarity, with one obvious place for each job and smoother workflows from idea to result.


## What's New


- **One name: Agents.** "Team Agents" are now simply **Agents** across the dashboard, the agent builder, and Workflows. The agent you chat with on Slack, run on a schedule, and drop into a pipeline is the same Agent everywhere.
- **AI Studio folds into three surfaces.** Content modeling and content creation move to your **Cosmic Agent** and the **Agents** surface, building apps moves to **Code > Build with AI** , and multi-step pipelines stay in **Workflows** .
- **Automations are folding into Agents.** An automation is the same record as an Agent (same id), so its detail page is now an Agent page and existing links map one to one. Standalone automations stay reachable from a read-only Automations list during a migration window so you can review and move them into Agents, and that list is removed on **July 31, 2026** .
- **Simpler workflow steps.** When you add a step to a Workflow, you pick an **Agent** and the full toolset it already has, instead of choosing between separate one-off step types.
- **Agents act within your permissions.** In dashboard chat, an Agent now runs each action against your own role, so it can only do what you can do.
- **Cleaner navigation.** **Agents** and **Workflows** are top-level entries with consistent tabs (Agents / Runs and Workflows / Runs), a shared sidebar, and agent avatars in the run history so it's easy to see who did what.


## Why This Matters


AI Studio had grown into a hub that overlapped with Agents, Code, and Workflows, and the "Team Agent" vs "automation" distinction added a layer of naming that didn't earn its keep. Consolidating to Agents, Workflows, and Build with AI means there's exactly one place to do each job, and one word for the thing doing the work.


The result is a clearer mental model: create an Agent, give it capabilities and channels, run it on demand or on a schedule, and chain Agents together in Workflows when you need a pipeline.


## How It Works


- **Agents** live at in your project. Create one, configure its capabilities and channels, and chat with it, schedule it, or trigger it on events.
- **Workflows** live at . Add steps, pick an Agent for each, and outputs flow from one step to the next.
- **Build with AI** lives under , for generating and iterating on applications.


Migration notes:


- Existing automations are reachable as Agents, and old AI Studio links redirect to their new homes.
- While you migrate, a deprecation-flagged **AI Studio** entry stays visible only for projects that still have automations, linking to a read-only Automations list so you can review and move them into Agents before the list is removed on **July 31, 2026** .
- AI Studio "Create with AI" chat history (Content and Automate modes) will not be migrated and will be removed on **July 31, 2026** . Copy anything you want to keep before then. Code and Repository build chats are unaffected.
