---
schema_version: "1.0.0"
document_id: "507c734f6b22e2fc56ff20b9646cea7c44e687402a6d623a58ca2266afffea4b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-inbox-zero-email-automation"
published_at: "2026-06-04T12:29:44+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:31.266141+00:00"
content_hash: "sha256:ccf5d80451ea02cfc08daeb5c97d6f50edbf1c64416117fc1bbb9efd7ba200ba"
---

# OpenClaw Inbox Zero: Let Your Agent Handle Email Before You Wake Up

## Writing the SOUL.md instructions for email voice


The agent drafts replies in your voice. The quality of those drafts depends entirely on how well you describe your style in SOUL.md.


Generic instructions produce generic replies. These don't work:


```text
# Bad: too vague
Write professional emails in a friendly tone.
```


Specific instructions produce specific results:


```text
# Good: actionable
## My email style
-   Opening: never "I hope this email finds you well" — go straight to the point
-   Length: under 100 words unless the topic is technical
-   Tone with investors: direct, confident, numbers-first
-   Tone with clients: warm but brief — one compliment maximum
-   Tone with vendors: neutral, no enthusiasm until terms are agreed
-   I never use "synergy", "circle back", "touch base", or "reach out"
-   Signature: [Your Name] — no job title in external emails, title in internal ones
```


The more specific you are, the less editing you'll do on the drafts. After 2 weeks of minor edits, update SOUL.md with corrections you're making repeatedly. The agent learns from what you write in SOUL.md, not from your edits to its drafts.


Gmail inbox showing organized email triage with drafts folder containing AI-prepared replies ready for review and sending


Blink


*Gmail inbox showing organized email triage with drafts folder containing AI-prepared replies ready for review and sending*


## The triage workflow in detail


**Urgent emails** — flagged at the top of the Telegram summary. The agent does not draft a reply unless you configure it to. Urgent emails need your direct judgment — the flag is enough.


**Reply needed** — the agent reads the full thread, checks your memory for project context, writes a reply, saves it to Gmail Drafts. The Telegram summary links to each draft.


**Archive** — moved to Gmail archive. Searchable, not deleted. If a rule miscategorizes something important, you'll find it in search.


**Flag for review** — anything ambiguous lands in a separate Gmail label. You decide.


Honest tradeoff: the agent makes categorization mistakes in the first week. A new vendor hits "flag for review" the first time — the agent doesn't recognize them. After you handle it manually and update SOUL.md, that vendor is handled correctly going forward. Expect 2-3 miscategorizations per day in week one, near zero by week three.


## What you still handle manually


The agent handles volume — the 80% of email that follows predictable patterns. The remaining 20% stays with you.


You still review and send every reply. Drafts are drafts. Legal negotiations, termination notices, complex client disagreements — the agent flags these for your attention, never drafts them.


The[OpenClaw email automation guide](https://blink.new/blog/%5BREDACTED%5D/blog/openclaw-email-automation) covers advanced workflows — email-to-task conversion, meeting scheduling from email, and multi-account setups.


## Running reliably on Blink Claw vs locally


The 7 AM schedule only works if the agent is running at 7 AM. A local OpenClaw agent fails silently if your laptop is in sleep mode — which it usually is at 7 AM.


Blink Claw runs your agent 24/7 — not just when your laptop is on. The HEARTBEAT.md job fires at 7:00 AM, emails get triaged, the Telegram summary arrives — all before you open your laptop.


The[AI agent morning routine guide](https://blink.new/blog/%5BREDACTED%5D/blog/ai-agent-morning-routine) combines the inbox triage with a morning briefing into one morning ritual. The email triage runs first at 7 AM; the briefing follows at 7:05 AM. By 7:10 AM you have both in Telegram.


$22/mo all-in — LLM costs included via 200+ model router. The email triage agent reads 14 emails and drafts 4 replies per morning. That's roughly 8,000-12,000 tokens per run. On a local setup with a separate OpenAI key, that's $0.04-0.08 per morning, about $20/year. On Blink Claw, that cost is included in the flat rate.


No Docker needed, no VPS setup required. Security patches get applied automatically. You message it from Telegram, Discord, or Slack — the same bot that sends you the triage summary can accept commands like` /rerun email_triage` if you want a manual refresh.


The[OpenClaw heartbeat and SOUL config guide](https://blink.new/blog/%5BREDACTED%5D/blog/openclaw-heartbeat-soul-config) covers HEARTBEAT.md scheduling in full — including timezone configuration, which matters if your 7 AM is not UTC+0.


Try Blink free — ship your first app today


Describe what you want to build. Get a working app with database, auth, and hosting in minutes.


[Start free](https://blink.new/)


Yes. Install the Gmail skill twice with different OAuth credentials, or install both Gmail and the microsoft-mail skill. In SOUL.md, label each account — "work Gmail", "personal Gmail", "Outlook" — and give each different triage rules.


The Telegram summary can consolidate both accounts into one message or send separate summaries. Your HEARTBEAT.md instructions control the format.


Delete the draft. The agent doesn't send anything — every reply sits in your Drafts folder until you approve it. A bad draft costs you 5 seconds to delete.


Over time, bad drafts should become rare. Update your SOUL.md with the correction: "When replying to investor emails, never apologize for delays — acknowledge the delay and immediately pivot to what's been done." The more specific your style rules, the better the drafts.


Two sources: the emails it reads during the triage run (it sees your sent mail to understand tone) and your SOUL.md instructions.


On the first run, give the agent 3-5 example sent emails from your own account and ask it to write a style guide for SOUL.md. It will extract your patterns — sentence length, formality level, how you open and close messages — and write them out in a format you can paste directly into SOUL.md.
