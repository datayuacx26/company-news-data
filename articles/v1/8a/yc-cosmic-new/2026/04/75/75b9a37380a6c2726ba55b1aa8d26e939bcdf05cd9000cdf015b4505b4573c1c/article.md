---
schema_version: "1.0.0"
document_id: "75b9a37380a6c2726ba55b1aa8d26e939bcdf05cd9000cdf015b4505b4573c1c"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/event-triggered-agents-your-ai-team-on-autopilot"
published_at: "2026-04-27T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:1b73a1c137219058775f3575d4d4b854613c55554e387b760406c784f772d541"
---

# Event-Triggered Agents: Your AI Team, on Autopilot

Cosmic team agents just learned how to act on their own. Until now, agents responded when you (or Slack, Telegram, WhatsApp) talked to them, plus an optional daily heartbeat. That's powerful, but it makes you the bottleneck. Event-Triggered Agents fix that: any team agent can now wake up the moment something happens in your content (from the dashboard or API) and take action without being asked.


## What's New


- **Trigger on object events.** Configure any team agent to run on , , or , with optional filtering by Object Type. When a matching event fires in the agent's bucket, the agent runs.
- **Same trigger picker as Workflows.** The Object Type and event selectors are the exact ones used by Workflows, so the mental model carries straight over.
- **Trigger Prompt (optional).** Give the agent a focused instruction for triggered runs, separate from its main system prompt. The triggering object's type, title, and metadata are appended automatically, so the agent always knows exactly what fired the run.
- **Post response to channels toggle.** For agents connected to Slack, Telegram, or WhatsApp, choose per-agent whether triggered runs auto-post their reply to the channel (great for "FYI" agents) or stay silent and act only through tools (great for back-office agents).
- **Conversational by default.** Triggered runs flow through the same messaging pipeline as Slack and chat. The agent keeps a dedicated "event-trigger" thread per Object Type, so it builds context across many runs instead of starting from scratch every time.
- **Visible at a glance.** Event-triggered agents show a purple "Event-Triggered" badge in the agents list, with the events and Object Types they're listening to.


## Why This Matters


Reactive agents are great for ad-hoc work. But the real productivity unlock is when your AI team handles the routine stuff before you even notice it happened: a contact gets welcomed the second they sign up, a draft post triggers an editorial review, a deleted asset kicks off a cleanup pass. You stop being the trigger; the content does.


Combine event triggers with the[agent-to-agent delegation](https://www.cosmicjs.com/blog/ai-agents-reorg-project-scoped-teams-multi-bucket-conversations-and-a-chat-first-experience) shipped earlier and you've got a real team: events fire, the right agent picks them up, and it can hand off to specialists when it needs help.


## Examples


- **Welcome flow.** Trigger an agent on for the Object Type with a prompt like "Send a personalized welcome email using Resend, then add the contact to the onboarding sequence." Channel post off, runs silently.
- **Editorial review.** Trigger an agent on for with "If status changed to Published, post a launch announcement in #marketing with the title, summary, and link." Channel post on, the announcement lands in Slack.
- **Cleanup crew.** Trigger an agent on for with "Check if any published Objects still reference this asset and flag them for review." Tools-only, no channel chatter.
- **Cross-team handoff.** Trigger an agent on for with "Summarize the ticket, set priority, and delegate to the right specialist agent based on the category." Pairs perfectly with agent-to-agent delegation.


## How to Set It Up


1. Open any team agent and go to **Settings** .
2. Scroll to **Event Trigger** and toggle it on.
3. Pick your events (, , ) and optionally narrow by Object Types.
4. Add a **Trigger Prompt** for triggered-run-specific instructions (optional).
5. If the agent is connected to Slack, Telegram, or WhatsApp, decide whether to **Post response to channels** .
6. Save. The agent is now listening.


## Polish Shipped Alongside


- **Object Type filter loads everywhere.** The Object Type picker inside the agent form now populates correctly from the agent's bucket, including on project-scoped pages and for agents installed from a template.
- **Delegatable agents endpoint hardened.** Listing delegatable agents no longer errors out when a project contains an agent without a bucket binding.


## Try It


Event-Triggered Agents are live for every Cosmic Workspace.[Open a Project](https://app.cosmicjs.com/projects) , pick a team agent, and turn on a trigger.


New to Cosmic?[Create your free account](https://app.cosmicjs.com/signup) and let your AI team start handling work the moment it arrives.
