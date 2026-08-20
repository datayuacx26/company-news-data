---
schema_version: "1.0.0"
document_id: "6e3af632d362b606075971da31cbc7237e4ce88cc385c75377095ea82452a59d"
company_key: "yc-joindex"
company: "Dex"
source_id: "yc-joindex-news-import-5787fcdf0962"
canonical_url: "https://www.joindex.com/blog/automations"
published_at: null
first_seen_at: "2026-07-21T16:11:25.745021+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:636be29727a7a1d92526d43147ddd9e3bf5d811a45d99c86e22d6c4400ee427b"
---

# Introducing Automations

Automations are agents that run in the background. You describe what you want done, set a condition, and Dex will run it automatically.


## How it works


Write a description of what you want in the Dex sidebar. Dex interprets it and creates a condition and an action for you to review. Once you confirm, the automation is live.


Conditions can be time-based (every morning, end of day, weekly) or event-based (a meeting ending, a new email, a contact being added). When the condition is met, the action runs as a regular Dex prompt, which means it can do anything Dex can normally do: browse the web, read and send emails, use tools across your stack, and more.


Active automations show up in the[Control Panel](https://www.joindex.com/blog/meet-taskos#control-panel) . You can configure and manage them from the[Automations Dashboard](https://app.joindex.com/dashboard/automations) .


## What people are building


- **Pre-call briefs:** Before every meeting scheduled in Google Calendar, get a report on who you're talking to, recent interactions, notes, and relevant context.


- **Post-meeting follow-ups:** When a meeting on your Google Calendar ends, draft a follow-up email to each attendee in your voice and post a summary to a Slack channel.


- **Inbox triage:** Every morning at 8am, go through your Gmail inbox. Flag anything that needs a reply, draft responses for routine messages, and archive the rest.


- **CRM sync:** At 6pm, pull every email, call, and meeting from the day and log them as activities in HubSpot.


These are just what we've seen so far. Automations are general-purpose, so if you can describe it, Dex can run it.


## Permissioning


Each automation has an "always confirm" setting. When on, the automation will pause before taking any action and wait for your approval. You can find pending actions in the chat log and confirm or reject them there. When off, the automation carries out actions on its own without waiting.


## Rollout


Automations are now in beta for Pro users. If your extension isn't updating, head tochrome://extensions and click the update button in the top-left corner.


Event-based conditions currently support Gmail and Google Calendar, with more apps on the way.


Join our[Discord](https://discord.gg/TYzCjwsTnT) to share feedback and shape what we build next!
