---
schema_version: "1.0.0"
document_id: "7313b6c8341c13c033cfabafad4fbf375fca8d5a5980d575f9804e30694654ac"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/the-self-maintaining-knowledge-base"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:1b177332800e2e6aef6c117cf032d5f0c4151106fba1e960d9e5c746f8074a2f"
---

# The self-maintaining knowledge base

Meet the all-new Slite: **the first self-maintaining knowledge base** . It pairs a structured, verified knowledge base with an AI agent that detects when documentation has drifted from reality, proposes the fix, and routes every change through human approval before it becomes truth.


### The Slite Agent


**Slite Agent** is now available on the Pro plan. It detects when docs drift from reality across 20+ connected tools, proposes fixes, and routes every change through human review before anything is applied.


- New global floating agent panel (three states: collapsed, popover, expanded) that persists across doc navigation
- Triage view in the sidebar: a dedicated mode showing all pending agent runs with unread badges and per-row indicators
- Agent approval right panel: click an approval card to open a 50/50 split view with the full-document diff, a minimap to jump between change hotspots, and Accept/Dismiss controls
- Redesigned approval cards for all agent action types (edit, create, rename, archive, ownership change)
- Improved diff rendering: inline diffs, correct handling of mentions, code, links, and tables
- Agent thread titles now reflect the document being worked on (e.g. "Fact Check the Reimbursement Policy")
- Slite Agent is callable via Slite UI, Slack, Claude, MCP, API, and the verification popover
- Scheduled automations: set up nightly routines to review entire doc channels and wake up to a triage queue


### Track every edit origin


Slite now tracks whether a doc edit, comment, or action was made from the app, desktop, mobile, public API, MCP, or the Slite Agent. The Activity Log displays it, and three months of history have been backfilled.


### New plans, pricing, and credit system


- Standard → **Basic** ($10/user/month), Knowledge Suite → **Pro** ($20/user/month)
- Pro includes 50 monthly credits per seat for agent actions; credits are pooled at the workspace level
- 14-day free trial (no credit card required) replaces the old free plan
- MCP access now included on all plans, including Basic
- New AI usage screen in settings: see credits spent per user with a custom date range picker


### New slite.com


The Slite website has been completely redesigned with a new homepage hero featuring an animated agent demo, Slite Agent and MCP sections, bento layouts, and full mobile responsiveness.


### Other improvements


- Via attribution badges on comments: a badge next to the avatar indicates when a comment was created, resolved, or archived via MCP or API
- Cmd+Shift+O now opens the Slite Agent directly
- Restored org ownership transfer in the app
- Mobile: collection view sorts now respected; mentioned-user avatars in database columns now resolve correctly
- MCP SliteML format is now MDX-compatible for programmatic parsing
- Onboarding flows updated: better UI, no duplicate steps between Slite and Agent onboarding
- Super Help Center merged into the Slite Help Center


### Bug fixes


- Fixed cited PDF sources from Ask not opening the source document
- Fixed Claude logo not showing on MCP OAuth consent screen
- Fixed incorrect triage URL returned by MCP
- Fixed citations not flowing back from Slite child agent to Super
- Fixed Asana project picker showing inaccessible root nodes
- Fixed long group names overflowing in settings
- Fixed doc insights not loading when a doc has a reaction
- Fixed collaboration crash on missing history (now falls back to current snapshot)
