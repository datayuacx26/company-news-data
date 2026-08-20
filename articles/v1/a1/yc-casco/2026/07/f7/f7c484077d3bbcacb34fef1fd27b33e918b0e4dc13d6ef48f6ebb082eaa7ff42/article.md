---
schema_version: "1.0.0"
document_id: "f7c484077d3bbcacb34fef1fd27b33e918b0e4dc13d6ef48f6ebb082eaa7ff42"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/casco-slack-bot"
published_at: "2026-07-30T17:00:00+00:00"
first_seen_at: "2026-07-31T18:50:42.124553+00:00"
fetched_at: "2026-07-31T18:50:43.655795+00:00"
content_hash: "sha256:292e0c619d0885feddc035898e7d1fb9c35de41d32532249b15dbb7f9449296f"
---

# Announcing the Casco Slack bot

# Announcing the Casco Slack bot


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Thu Jul 30 2026


Casco's Slack bot brings your security findings into Slack, where your team already talks. Converse with the agent about a finding, ask follow-up questions for more detail, and pull in the engineer who owns the fix. All in a thread, without inviting anyone into another tool. Enable it in the Casco app under **Settings → Agent integrations → Slack bot** .


## Remediation is a team sport played by people who don't have seats


We know that fixing security issues is an all-hands on deck experience. It's the backend developer who owns the endpoint, the platform engineer who owns the proxy, sometimes a PM who has to decide whether the behavior is even a bug.


None of those people live in your security dashboard. Up until today, you had two options and they both kind of sucked.


**Option one: Invite them into a security dashboard.** Now you're provisioning a seat, walking a backend developer through a security tool they will open exactly once, and waiting while they figure out where the finding lives.


**Option two: copy/paste.** Or worse, screenshot the finding, drop it in a channel, and answer the same three questions ("which endpoint? can you reproduce it? how bad is it?") by alt-tabbing between Slack and the dashboard for the rest of the afternoon.


Both options make the security engineer the middleman. We've spent this whole launch week removing middlemen and to make life easier for you.


## The finding comes to the thread


With the Casco Slack bot enabled, the conversation happens in the shared channel you already have with Casco:


**Findings show up where your team already talks to us.** No new login, no onboarding, no "where do I click."


**Ask the agent follow-up questions.** Need the reproduction details, the affected endpoint, or the severity reasoning? Ask in the thread and the agent answers with the details from Casco.


**Pull in the people who own the fix.** Add the backend developer, the platform engineer, the PM to the shared channel. They get the full context in the thread, and they can ask the agent their own questions instead of asking you.


The security engineer stops being a human API between the dashboard and the people doing the work. That's the feature in a nutshell.


## Chat is where remediation actually happens


There's a pattern behind this launch week. Fixes don't happen in security tools. They happen in the places engineers already work: the coding agent, the terminal, and the thread where three people argue about whether the fix ships this sprint or next.


The[Casco MCP server](https://casco.com/blog/introducing-the-casco-mcp-server) brought findings to your coding agent. The Slack bot brings them to your team. Same thesis: security context should travel to where the work happens, because the reverse, dragging people into security tools, has been failing for as long as security tools have existed.


The thread also does something the dashboard can't: it keeps the decision next to the discussion. Six months later, when someone asks why a finding was deprioritized, the answer is in the thread, with the names attached.


## How to enable it


1. Go to the Casco app.
2. Navigate to **Settings → Agent integrations → Slack bot** .


That's it. The bot joins the shared Slack channel you already have with Casco. There's nothing to install in your workspace, no OAuth flow, and no scopes to grant: the bot lives in the shared channel, not in your Slack.


Get year-round security with autonomous security testing. Get started at[https://casco.com](https://casco.com/)


Quick answers


## Frequently asked questions


What to know before connecting Casco to your coding agent.


###


###


###


###


###


###
