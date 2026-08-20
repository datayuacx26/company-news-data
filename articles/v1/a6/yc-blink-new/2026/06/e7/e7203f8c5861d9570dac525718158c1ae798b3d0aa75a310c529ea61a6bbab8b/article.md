---
schema_version: "1.0.0"
document_id: "e7203f8c5861d9570dac525718158c1ae798b3d0aa75a310c529ea61a6bbab8b"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-inbox-zero-email"
published_at: "2026-06-09T12:21:23+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:71448507772f9e3f6dbfb698582e1fa2bd5606bb5e2d37c797363a48a65c988d"
---

# OpenClaw Inbox Zero: Let Your Agent Handle Email Before You Wake Up

## Configuring the Email Triage Skill


The inbox-triage skill is available on[ClaWHub](https://github.com/openclaw/skills) . Install it in your OpenClaw configuration, then define your rules in two files:` SOUL.md` and` HEARTBEAT.md` .


**Step 1: Install the email skill**


In your OpenClaw directory:


```text
npx   clawctl   skill   install   gzlicanyi/imap-smtp-email
```


Or install via the ClaWHub interface in your agent's configuration panel.


**Step 2: Define your triage rules in SOUL.md**


` SOUL.md` is where you tell the agent what you know about yourself — your context, your rules, what matters and what doesn't. This is the file that makes the difference between a generic email tool and one that actually knows your priorities.


```text
## Email Triage Rules


Urgent (always flag for my review):
-   Any email from [  client-name  ] or [  boss-email  ]
-   Contract or invoice attachments requiring a signature
-   Subject lines containing: "urgent", "ASAP", "time-sensitive"
-   Meeting requests from external parties I haven't met before


Routine (draft a reply and archive):
-   Shipping notifications and order confirmations
-   LinkedIn connection requests and notification digests
-   Internal newsletter and digest emails
-   "Following up" emails from vendors — draft: "Thanks, will review soon"


Archive without reply:
-   Newsletter subscriptions (archive only, do not unsubscribe automatically)
-   GitHub notification digests
-   Automated system alerts that are informational-only


Never do without asking me first:
-   Send any email on my behalf
-   Unsubscribe from any mailing list
-   Delete any email permanently
```


The "Never do without asking me first" section is critical. Your agent surfaces replies for review, not sends autonomously, until you trust the quality of its drafts.


**Step 3: Set up the triage schedule in HEARTBEAT.md**


` HEARTBEAT.md` defines when your agent runs recurring tasks. Add the email triage job:


```text
## Email Triage Schedule


SCHEDULE: daily at 06:00


TASK: Process inbox
1.   Fetch all emails received in the last 12 hours
2.   Apply triage rules from SOUL.md
3.   Archive low-priority emails
4.   Draft replies for routine emails — save as drafts, do not send
5.   Send a Telegram summary: urgent items first, then count of archived/drafted
6.   If any email needs immediate escalation, message me now regardless of schedule
```


The` 06:00` schedule means triage runs while you sleep and is ready when you wake. Adjust to match your timezone.


OpenClaw email triage: the agent sorts, summarizes, and drafts while you sleep — you wake up to a clean inbox with only the emails that genuinely need your attention


Blink


*OpenClaw email triage: the agent sorts, summarizes, and drafts while you sleep — you wake up to a clean inbox with only the emails that genuinely need your attention*


## What Your Agent Can Do (and What It Can't)


**Can:**


- Read and summarize emails from any IMAP-compatible inbox
- Apply labels and archive messages
- Draft replies for routine emails (saved to Drafts — does not send unless you explicitly allow it)
- Send you a Telegram, Discord, or Slack summary with the triage results
- Flag urgent items with a direct notification outside the scheduled summary
- Forward specific emails to a designated address


**Can't (without explicit configuration):**


- Send email on your behalf — this requires you to add` smtp_send: true` specifically in the skill config
- Access calendar data or attachments automatically — those need additional skills installed alongside the email skill
- Handle accounts the skill isn't configured for


The draft-and-review workflow is the right default. After a week of reviewing the agent's drafted replies, you'll know which categories you trust to send automatically.


Start conservative. You can always expand permissions. You can't un-send an email.


## Running This 24/7 on Blink Claw


Your laptop can't be asleep when OpenClaw does its 6 AM email triage.


If you're running OpenClaw locally, the agent only works when your machine is on and awake. Missed the morning triage because your laptop was closed? Your inbox doesn't get processed.


[Blink Claw](https://blink.new/claw) runs OpenClaw 24/7 in the cloud — the agent is always on, whether your laptop is open or not. Your 6 AM triage runs every morning, every day, whether you're home or traveling.


All-in pricing at $22/mo covers the agent hosting, 200+ AI models for reading and summarizing email (no separate API keys), global region hosting, and zero DevOps. No Docker needed. No VPS to patch. No server management.


Setup comparison: local OpenClaw requires Docker installed, configured, and running on your machine. Blink Claw is roughly 10 minutes from signup to your first agent session. Your SOUL.md and HEARTBEAT.md configuration is identical either way — only the hosting is different.


If you've already set up the[OpenClaw morning briefing](https://blink.new/blog/openclaw-morning-briefing) , the email triage skill slots in as an additional task in the same HEARTBEAT.md schedule. Your agent handles weather, calendar, and now email triage in a single 6 AM run.


## Frequently Asked Questions


The email skill connects via IMAP with read access and label permissions. For Gmail, you create an App Password specifically for the agent — not your main account password. If you ever want to revoke access, delete the App Password and the agent loses access immediately. The agent does not send email by default. Drafts go to your Drafts folder for your review. The setup is equivalent in security to any email client you'd install on your phone.


Start with your urgent rules set conservatively — when in doubt, flag it. Review the first week of triage summaries carefully and add rules to SOUL.md where you see gaps. Most people find the agent's triage is accurate within 3-5 days of tuning. Keep the "send email on my behalf" permission off until you've reviewed 50+ draft replies and trust the quality. This isn't set-and-forget on day one — it's a system you tune over the first week.


Yes. Any IMAP-compatible provider works: Gmail, Outlook, Fastmail, ProtonMail (via ProtonMail Bridge), and most business email. The IMAP configuration values differ per provider, but the skill configuration structure is the same. Outlook uses` imap-mail.outlook.com` on port 993. For corporate accounts behind Microsoft Exchange, check with your IT team — some Exchange configurations block external IMAP clients.


With Blink Claw's $22/mo all-in pricing, LLM costs are included — you don't pay extra for model calls. The email triage processes summaries efficiently, so a typical inbox of 20-50 emails per day is very token-light. If you're self-hosting OpenClaw with your own API keys, daily triage costs roughly $0.05-0.15/day with an efficient model like Claude Haiku.


Tools like Superhuman and Inbox Zero are SaaS products with fixed features — you get what they've built. OpenClaw is a configurable AI agent you control: the triage rules in SOUL.md are entirely yours, the schedule in HEARTBEAT.md is yours, and you can add any behavior you can describe in plain language. If you want the agent to cross-reference your calendar before drafting a meeting reply, you can add that. The ceiling is much higher because you're configuring an agent, not using a product.


Yes. Install the IMAP skill once per account with different credentials in each configuration. Run them as separate tasks in HEARTBEAT.md — personal inbox at 06:00, work inbox at 06:05. The agent processes each account independently with whatever rules you define per account in SOUL.md. Many users run triage on their work email only and manage personal email manually.
