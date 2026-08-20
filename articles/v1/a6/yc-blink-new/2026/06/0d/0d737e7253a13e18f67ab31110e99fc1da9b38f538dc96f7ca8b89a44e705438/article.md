---
schema_version: "1.0.0"
document_id: "0d737e7253a13e18f67ab31110e99fc1da9b38f538dc96f7ca8b89a44e705438"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/openclaw-inbox-zero"
published_at: "2026-06-11T00:17:52+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:06.862285+00:00"
content_hash: "sha256:e1efea0a8cea220c970182a749ec195a0f523a7e8f1639c3edb014f20a1767bc"
---

# OpenClaw Inbox Zero: Let Your Agent Handle Email Before You Wake Up

## Setting Up Your Email Agent


1


#### Connect your email account


Add the email skill to your agent in` openclaw.json` :


```text
{
"agents"  : [{
"id"  :   "inbox-agent"  ,
"model"  :   "claude-sonnet-4-5"  ,
"skills"  : [  "email"  ,   "memory"  ,   "telegram"  ]
}]
}
```


For Gmail: create an App Password at` myaccount.google.com/apppasswords` . Requires 2-factor authentication. For Outlook: use Microsoft OAuth — the skill handles the flow. For any IMAP inbox: add host, port, username, and password to the skill credentials.


Run` openclaw skill test email` to confirm the connection. Ninety percent of first-run errors come from App Password misconfiguration — double-check the password and that it is scoped to the correct Google account.


2


#### Write your triage rules in SOUL.md


This step determines classification accuracy. Specific rules produce specific results.


```text
## Email Triage Rules


### HIGH PRIORITY — flag immediately, Telegram alert:
-   Emails from [  @yourcompany.com  ] or named colleagues
-   Subject contains: "urgent", "deadline", "ASAP", "final answer"
-   Replies to any thread I started


### DRAFT FOR REVIEW — write reply, queue for approval:
-   Meeting requests — check my schedule, propose 2 time slots
-   Client questions I can answer in 1-2 sentences
-   Vendor follow-ups on active work


### ARCHIVE — no reply, no notification:
-   Newsletters (list-unsubscribe header present)
-   GitHub, Jira, Linear, Slack digest emails
-   Receipts and order confirmations


### FLAG — show raw email, take no action:
-   Money transfer or payment credential requests
-   Sender domain doesn't match claimed organization
-   Anything marked "attorney-client" or "legal notice"
```


Name actual domains and colleague names — vague rules produce vague sorting. Spend 15 minutes on this file. It directly determines how much review work disappears each day.


3


#### Set your 6am HEARTBEAT schedule


HEARTBEAT.md controls scheduled tasks. Add the inbox runs:


```text
## Email Processing


SCHEDULE: 06:00 UTC
TASK: Morning inbox triage
-   Read all messages since midnight
-   Apply SOUL.md rules to each message
-   Draft replies for needs-review queue
-   Archive newsletters and automated notifications
-   Post morning Telegram digest


SCHEDULE: Every 2 hours, 09:00–18:00 UTC
TASK: Inbox check
-   Read new messages since last run
-   Telegram alert immediately for HIGH PRIORITY only
```


Adjust UTC offsets for your timezone. For EST,` 06:00 UTC` = 1am local — the overnight catch runs before you wake.


4


#### Configure your morning summary format


Add this to SOUL.md to standardize the digest:


```text
## Morning Email Summary Format


📬   **Inbox — [Day, Date]**
Flagged (needs you): [  count  ]
Drafted (awaiting approval): [  count  ]
Archived automatically: [  count  ]


FLAGGED:
• [  From  ] | [  Subject  ] | [One-line summary]


DRAFTS — reply /approve [  id  ] or /edit [  id  ]:
• [  From  ] | [  Subject  ] | [Draft summary]
```


Reply` /draft \[id\]` to read any full draft before approving. Reply` /skip \[id\]` to dismiss without sending.


5


#### Test with a manual run before activating the schedule


Trigger a single run to verify classifications match your rules:


```text
openclaw   agent   run   inbox-agent   --task   "Process inbox: fetch last 10 unread, classify, report classifications."
```


Check each classification against your SOUL.md intent. Fix rules that produce wrong results — one rule edit corrects every future occurrence from that sender or pattern. Your[morning briefing](https://blink.new/blog/ai-agent-morning-routine) setup will feel immediate once this step runs cleanly.


## The 5 Workflows Your Agent Handles Daily


**1. Auto-Reply Templates**


Your agent drafts replies to recurring message types — status update requests, meeting proposals, quick clarifications. Each draft queues for your` /approve` in Telegram. After verifying 15–20 drafts on a specific category, you can mark it for auto-send. The agent learns your reply style from corrections you make to early drafts.


**2. Newsletter Unsubscribe**


Newsletters carry a` List-Unsubscribe` email header. The email skill reads this header and can click the unsubscribe link on your behalf — no browser required. Configure the agent to unsubscribe from any newsletter you have not opened in 30 days. Inbox volume drops measurably in the first week.


**3. Meeting Request Routing**


Meeting requests generate the most cognitive overhead per email. Your agent reads your calendar context from SOUL.md — blocked times, time zones, meeting preferences — and writes a reply proposing two specific slots. It never books anything without your confirmation. You receive the draft, approve it, and the reply sends. Context-switching cost: near zero.


**4. Priority Flagging**


Any message matching your HIGH PRIORITY rules generates an immediate Telegram alert — not queued for the morning summary, but sent the moment the agent's 2-hour check runs. Define high priority narrowly at first: your manager's email address, your three largest client domains. Expand the criteria once you see how it performs.


**5. Daily Digest Summary**


Every morning at 6am, Telegram delivers the full picture: total messages received, archived count, flagged items, and drafts awaiting your approval. This[morning briefing](https://blink.new/blog/ai-agent-morning-routine) replaces the 45-minute inbox audit most people do before they start real work. Your agent runs 24/7 to make that briefing accurate — not just when your laptop is on.


Five email automation workflows an OpenClaw agent handles automatically every day


Blink


## What Your Agent Should Never Do Autonomously


**No auto-send for the first 30 days.** Read every draft. Your agent needs time to learn your voice, your relationship context, and your tone with different senders. Premature auto-send produces technically accurate replies that read as wrong. The 30-day review window calibrates the agent's output to your actual style.


**No action on money or credential requests.** Add money transfer keywords and credential reset language to your FLAG rules — not your DRAFT rules. Social engineering attacks specifically target AI email agents. No agent action means no agent-accelerated breach.


**No delete operations.** Set archiving as the strongest autonomous action available. Classification is excellent but not perfect. Archived email is recoverable; deleted email is not.


**No autonomous processing of legal, compliance, or medical communications.** Add terms like "attorney-client privilege", "HIPAA", "compliance violation", "legal notice", and "cease and desist" to your FLAG rules. These messages require direct human reading regardless of apparent urgency.


Configure these four categories in SOUL.md before activating the agent on your live inbox. Blink Claw handles the infrastructure automatically — the safety boundaries are yours to define.


## Run It 24/7 with Blink Claw


An OpenClaw email agent only processes email while its host is running.


Local OpenClaw on your laptop runs the 6am check — if your laptop is on at 6am. A travel day, a closed lid, or a reboot at midnight means the overnight triage does not happen. You wake up to 47 unread emails again.


Your agent runs 24/7 — not just when your laptop is on — only with always-on hosting. **Blink Claw** is managed OpenClaw hosting from **$22/mo all-in, LLM costs included** . No Docker needed, no VPS setup required. Your agent runs across 30+ data center regions, security patches apply automatically, and the service handles every restart and update.


Blink Claw handles this automatically — you write SOUL.md and HEARTBEAT.md, Blink Claw handles everything else.


Run OpenClaw without the hassle — Blink Claw handles everything from $22/mo →[blink.new/claw](https://blink.new/claw)


Before OpenClaw: 200 unread emails. After: inbox zero, handled by your AI agent overnight


Blink


No — every draft requires your approval by default. Reply` /approve \[id\]` in Telegram to send. After verifying 15–20 drafts on a specific message type, you can enable auto-send for that category only. Auto-send is opt-in, per category, and reversible. The default setting is human-in-the-loop for every outgoing message.


Yes. Gmail supports App Passwords or OAuth. Outlook uses Microsoft OAuth — the skill handles the authorization flow. Any inbox with IMAP/SMTP access works with the IMAP skill variant. This includes Google Workspace, Microsoft 365, Fastmail, ProtonMail, and custom domain email hosted on standard servers.


Fix the SOUL.md rule, not the individual email. If a client email gets archived instead of flagged, add their domain to your HIGH PRIORITY rules. The correction takes 10 seconds and applies to every future message from that sender. Rule-based correction prevents the same misclassification from recurring.


Yes. Configure each inbox as a separate skill instance in` openclaw.json` . You can receive a combined morning digest across all accounts or separate digests per inbox. Start with your primary inbox for one week, verify the rules work correctly, then add secondary accounts.


Local OpenClaw only runs tasks when your machine is on. A closed laptop at 6am means no morning triage. Blink Claw runs 24/7 in the cloud — no Docker needed, no VPS setup. Your agent runs 24/7 whether your laptop is open or not. For email triage that happens overnight, always-on hosting is not optional. Blink Claw provides this from $22/mo all-in, LLM costs included.


Suspicious emails get flagged with an immediate alert — the agent takes no further action. You define what counts as suspicious in SOUL.md: mismatched sender domains, urgent money requests, credential resets. Flagged emails appear in a separate section of your Telegram summary labeled "Review required" with the raw email content displayed. The agent never drafts or sends anything for flagged messages.
