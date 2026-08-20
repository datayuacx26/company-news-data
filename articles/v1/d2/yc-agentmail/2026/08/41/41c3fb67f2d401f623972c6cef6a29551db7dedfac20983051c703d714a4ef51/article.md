---
schema_version: "1.0.0"
document_id: "41c3fb67f2d401f623972c6cef6a29551db7dedfac20983051c703d714a4ef51"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/agentmail-official-openclaw-plugin"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-18T21:22:50.659375+00:00"
fetched_at: "2026-08-18T21:22:52.594592+00:00"
content_hash: "sha256:3a5222ff65066326640a430184d1ccd79348f86edfca8ba41b0dc0283bd4c07c"
---

# AgentMail is now an official OpenClaw plugin

TL;DR


- AgentMail is now an official plugin on[ClawHub](https://clawhub.ai/agentmail) , the OpenClaw plugin registry.
- Install it with` openclaw plugins install clawhub:@agentmail/agentmail` .
- It ships two things: a CLI-backed AgentMail skill, and an email **channel** for OpenClaw. Inbound email drives an agent turn, and the agent replies inside the AgentMail thread.
- The channel is reply-only and default-deny. Unknown senders are rejected, replies stay bound to the message that triggered them, and the agent cannot start threads or mail arbitrary recipients.


That was five weeks ago. He asked for a plugin and a channel, and both now exist.


AgentMail is published on ClawHub, the OpenClaw plugin registry, with the Official badge.


```text
openclaw plugins install clawhub:@agentmail/agentmail
```


The fit is structural rather than lucky. OpenClaw's premise is that you reach your agent wherever you already are, and it goes and does the thing. An agent that acts on your behalf needs two things: somewhere to be reached, and something to be recognised by. Email is both, and it is the only address that every person, company and machine already accepts without being asked to install anything first.


Which makes the gap on the shelf odd. ClawHub lists twenty-five channel plugins for OpenClaw. WhatsApp, Matrix, Discord, Slack, Teams, Feishu, LINE, Google Chat, QQ, Zalo, Nostr, Twitch, Urbit, iMessage through BlueBubbles. Twenty-five ways to reach an agent, and until now not one of them was email.


## What is OpenClaw?


OpenClaw is an open source AI agent that runs on your own hardware rather than in someone else's cloud. It reads and writes files, runs shell commands, browses, and keeps long-term memory locally.


The scale is worth stating plainly: 386,000 stars and 81,000 forks on GitHub, from a repository that did not exist before November 2025.


Channels are the part that matters here. OpenClaw does not have a UI so much as it has inboxes. You message it on WhatsApp or Discord or Matrix, it does the work, it messages you back. Every capability it has arrives through a channel, which is why the missing one was worth fixing.


## What is AgentMail?


AgentMail is email infrastructure built for agents instead of people. You create an inbox with an API call and get a real, addressable email address in under a second. No OAuth consent screen, no per-seat billing, no human sitting behind the account.


Inboxes are two-way. Your agent sends, receives, and replies inside real threads with the message history intact, and every operation available in the dashboard is also an API endpoint. Incoming mail can fire a webhook the moment it lands.


## Why does everyone running OpenClaw need this?


Because until now your agent could reach you, and almost nobody else. Chat channels connect an agent to the people already in that app. Email connects it to everyone, including the systems that only speak email: verification codes, invoices, calendar invites, support queues, and other agents.


**Email becomes a channel.** Before this, giving OpenClaw an email address meant wiring it to Gmail through OAuth or IMAP and hoping Google did not flag the account. Now email works the way WhatsApp does. Someone emails the agent's address, that message drives an agent turn, and the reply goes back in the same thread.


In practice that means you can put your own address in` allowFrom` , forward your agent a thread from your phone with "draft me a reply to this," and get the draft back in that thread. No terminal, no session, no laptop open.


**The defaults are narrow on purpose.** Email is the easiest prompt-injection surface anyone will point an agent at, so the channel is locked down before you touch it:


- ` dmPolicy` defaults to` allowlist` , and an empty allowlist denies every sender. You opt senders in, not out.
- Replies are bound to the message that triggered them. The agent cannot open new threads or mail addresses you never approved.
- Every reply re-hydrates and re-authorizes the original sender, so a forged` Reply-To` cannot redirect where the response goes.
- Inbound mail is committed durably before it is acknowledged, so a restart mid-turn does not lose the message.


The failure mode is "nothing happens," not "a stranger drove my agent."


**The skill does not go stale.** Instead of a fixed set of hardcoded tool schemas, the plugin bundles the official AgentMail CLI and teaches the agent to read its help output. When AgentMail ships a new resource, your agent can use it without waiting for a plugin release. It already covers inboxes, messages, threads, drafts, webhooks, domains, pods, and API keys.


## How to get it


Install and enable the plugin:


```text
openclaw plugins install clawhub:@agentmail/agentmail
openclaw plugins enable agentmail
```


Put your credentials where the Gateway can read them, in` ~/.openclaw/.env` . Grab a free API key from the[AgentMail console](https://console.agentmail.to/sign-up) . The webhook secret is optional; without it the channel falls back to WebSocket ingress.


```text
AGENTMAIL_API_KEY=am_...
AGENTMAIL_WEBHOOK_SECRET=whsec_...
```


Point the channel at an inbox and list who is allowed to talk to it, in your OpenClaw config:


```text
{
channels: {
agentmail: {
apiKey: { source: "env", provider: "agentmail", id: "AGENTMAIL_API_KEY" },
inboxId: "agent@agentmail.to",
webhookSecret: { source: "env", provider: "agentmail", id: "AGENTMAIL_WEBHOOK_SECRET" },
dmPolicy: "allowlist",
allowFrom: ["you@example.com"],
},
},
}
```


Restart the Gateway and confirm it loaded:


```text
openclaw gateway restart
openclaw plugins inspect agentmail --runtime
```


The CLI-backed skill is available to the agent immediately, and you can drive it yourself with a passthrough command:


```text
openclaw agentmail -- --format json inboxes list
```


Requirements: OpenClaw 2026.7.2 (beta) or newer, and Node 22.22.3+, 24.15.0+, or 25.9.0+. The published plugin bundles the AgentMail CLI for macOS, Linux, and Windows, so there is nothing else to install.


## Where to go next


- [Email for OpenClaw agents](https://www.agentmail.to/build/openclaw) is the integration page, with every install path and a full config reference.
- [How to give your OpenClaw agent its own email inbox](https://www.agentmail.to/blog/openclaw-agent-email-inbox) is the step-by-step build guide.
- [7 OpenClaw email automation use cases](https://www.agentmail.to/blog/openclaw-email-automation-use-cases) covers support triage, signup verification, invoice processing, and lead follow-up.
- [The best email API for OpenClaw](https://www.agentmail.to/blog/best-email-api-for-openclaw-2026) compares AgentMail against Gmail, Resend, SendGrid, and Mailgun.
- [AgentMail vs Gmail for OpenClaw](https://www.agentmail.to/blog/agentmail-vs-gmail-openclaw) is the head-to-head, including pricing at scale.
- [How to connect OpenClaw to Gmail](https://www.agentmail.to/blog/connect-openclaw-to-gmail) is the guide to read if you want Gmail anyway.


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
