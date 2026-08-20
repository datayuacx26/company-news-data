---
schema_version: "1.0.0"
document_id: "0916138c29a1f0640cfeb58412babcf5cc6dc610844104d341eb030b6c79da12"
company_key: "yc-verbiflow"
company: "Verbiflow"
source_id: "yc-verbiflow-news-import-392b5e1f9020"
canonical_url: "https://verbiflow.com/blog/drive-verbiflow-from-claude-cursor-codex"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-22T18:39:30.762214+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:0bedb0bee2f1f965a12ddf64e82475a01c9e530612f97790221f420508fe7674"
---

# Why the Verbiflow SDK comes before a Verbiflow agent

Customers keep asking when we’re shipping a Verbiflow agent. Probably at some point. The interface underneath it had to come first. Verbiflow already handles the outbound infrastructure: sequences, connected mailboxes, reply inbox, CRM checks, and reporting sync. The SDK lets Claude Code, Cursor, Codex, and eventually a Verbiflow agent build custom plays on top of that same system instead of reinventing the workflow each time.


## Why the SDK comes first


Without a solid SDK, an agent will build a play one way today and a different way next week. Outbound breaks when the workflow keeps drifting. You end up with sequences that each look slightly different and reports you can’t compare.


The SDK is the repeatable shape. It tells Claude Code, Cursor, Codex, and eventually a Verbiflow agent how to build a play the same way every time: pull data with the right batching and parallelism, use the right connected tools, generate the audience, and stage a sequence draft. If custom logic is needed, Claude Code can modify the play code inside that scaffold. The play is still running through Verbiflow’s infrastructure once it is ready to send.


Data providers should keep doubling down on their data. Apollo on contacts, Crunchbase on company signals, Clay on tables and enrichment workflows. Verbiflow’s job is the orchestration that runs across all of them.


## What Claude Code adds


Claude Code is useful for the work before launch: research, list logic, custom play steps, and copy. With the Verbiflow SDK, it follows the same outbound structure our team uses instead of improvising a fresh workflow on every run.


- **For list building:** pull from connected data providers with the right parallelism, enrich the contacts, and stage the audience.
- **For custom plays:** add or modify the play code when a customer needs a workflow that is not already in the template.
- **For copy:** use the Verbiflow SDK to draft sequence copy that matches the play, the audience, and outbound best practices.


Verbiflow handles the platform work regardless of Claude Code: CRM and sequence checks, connected mailbox health, sending, email and LinkedIn replies, and reporting sync. Claude Code helps you build a better play. Verbiflow runs it.


## What this looks like


A GTM lead should be able to ask:


- “Use connected Crunchbase and Apollo to build a Series A SaaS audience, draft copy for heads of GTM, and stage the sequence in Verbiflow.”
- “Take this Clay export, add the HubSpot suppression rules, improve the copy with the Verbiflow SDK, and prepare the remaining contacts for review.”


The output isn’t another CSV sitting in someone’s downloads folder. It’s a play staged in Verbiflow that the team can review, launch, and track replies on.


## Where the Verbiflow agent fits


We’ll probably ship our own agent at some point. When we do, it should run on the same SDK customers already use with Claude Code, Cursor, and Codex. A play built in Claude Code today should behave the same when it runs inside Verbiflow later.
