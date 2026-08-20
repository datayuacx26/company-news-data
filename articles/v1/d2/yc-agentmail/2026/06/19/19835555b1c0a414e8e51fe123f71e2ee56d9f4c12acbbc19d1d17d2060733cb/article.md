---
schema_version: "1.0.0"
document_id: "19835555b1c0a414e8e51fe123f71e2ee56d9f4c12acbbc19d1d17d2060733cb"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/agentmail-now-supports-imap"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:5424343fd82b2f54f0f85923c39ebe64c27976c208ce2dc8766c161435b8742d"
---

# AgentMail now supports IMAP

AgentMail now supports IMAP. Every inbox you provision can be read by any standard email client or library that speaks the protocol: Outlook, Apple Mail, Thunderbird, anything that talks to a real mail server. SMTP for sending has been there since launch. With IMAP shipped, AgentMail covers both halves of the standard email protocol surface alongside the API.


This is short. Here is what IMAP is, why it matters for agent inboxes, and how to point a client at yours.


## What IMAP is


IMAP (Internet Message Access Protocol) is the standard protocol for reading and managing email on a mail server. Every time you open Apple Mail, Outlook, or Thunderbird and your inbox appears, you are using IMAP under the hood. Messages live on the server, the client syncs them, and changes (read state, deletes, moves) reflect across every device pointed at the same account.


It has been the open standard for reading email since the 1990s. The companion protocol for sending is SMTP, which AgentMail has supported since launch.


## Why IMAP matters for agent inboxes


An agent's inbox is not only useful when an agent is reading it. The same inbox often needs to be reachable by humans, ops teams, debugging tools, and legacy systems that only know how to talk IMAP and SMTP.


**Humans in the loop.** Support inboxes, sales inboxes, and approval inboxes are agent-driven on the write side, but humans want to read them in their own mail client. Connecting Apple Mail or Outlook to an AgentMail inbox over IMAP means a teammate can monitor what the agent is doing without ever touching the API.


**Debugging and observability.** Sometimes the fastest way to figure out why an agent loop stalled is to open the inbox and look at the messages. IMAP makes that as simple as adding the account to whatever mail client you already have open.


**Portability.** IMAP is an open standard. The inbox you provision today on AgentMail can be read by software written 25 years ago. If you ever want to move data out, the protocol is right there.


**Familiar tooling during development.** If you already know` imaplib` or` nodemailer` , you can prototype an agent against an AgentMail inbox without learning the SDK first. The SDK is faster and more capable for production, but IMAP and SMTP cover the basics without learning anything new.


## How to use it


You will need two things from the[AgentMail Console](https://console.agentmail.to/) :


1. Your **inbox email address** (this is your IMAP username), from Dashboard then Inboxes
2. An **API key** (this is your IMAP password), from Dashboard then API Keys


Connection settings:


```text
Host:     imap.agentmail.to
Port:     993
Username: your inbox email (e.g., myinbox@agentmail.to)
Password: your AgentMail API key
SSL/TLS:  required


```


That is it. Point any IMAP client at those settings and it connects. Full setup, including a Python` imaplib` example and a Node` imap` example, is in the[IMAP and SMTP docs](https://docs.agentmail.to/imap-smtp) .


For Apple Mail, Outlook, or Thunderbird, add an account with those settings the same way you would add any other IMAP account. The inbox shows up.


For a quick programmatic test in Python:


```text
import imaplib
import os


inbox_email = "myinbox@agentmail.to"
api_key = os.getenv("AGENTMAIL_API_KEY")


imap = imaplib.IMAP4_SSL("imap.agentmail.to", 993)
imap.login(inbox_email, api_key)
imap.select("INBOX")


status, message_ids = imap.search(None, "ALL")
for msg_id in message_ids[0].split():
status, msg_data = imap.fetch(msg_id, "(RFC822)")
# parse msg_data[0][1] with the standard library `email` module


imap.logout()
```


One thing to know: IMAP currently exposes the **INBOX** folder. Other folders (Sent, Drafts, Trash) and label-based filtering are available through the[AgentMail API](https://docs.agentmail.to/quickstart) , not IMAP. If you need full mailbox structure or real-time inbound notifications, the API and[webhooks](https://docs.agentmail.to/webhooks-overview) are the right call.


## SMTP, briefly


SMTP for sending has been live since launch. The server is` smtp.agentmail.to` on port 465 with SSL required, and the password is the same API key. Per-send limits: 50 recipients per message, 10 MB max message size. The` From` address must match the inbox you are authenticating as. Full credentials and Python and TypeScript examples are in the[IMAP and SMTP docs](https://docs.agentmail.to/imap-smtp) .


## When to use IMAP and SMTP vs the API


Use case Use


Connect Outlook, Apple Mail, or Thunderbird IMAP/SMTP


Simple programmatic send from any language SMTP


Read mail from a backup or archiving tool IMAP


Full inbox management, drafts, labels, threads API


Real-time inbound notifications API webhooks or WebSockets


Access all folders, not just INBOX API


Bulk operations across many inboxes API


The API is still the right surface for everything an agent does at runtime. IMAP and SMTP are there so the inbox behaves like a real email account when you need it to.


## Wrapping up


The inbox you provision through AgentMail is a real, addressable mailbox. With IMAP shipped, it is also reachable by every email client and library that speaks the standard protocol. That is the bar a real email provider has to meet, and it is the bar AgentMail meets now.


If you already have an inbox, your existing API key works as the IMAP and SMTP password. If you do not, the Free plan gives you three real inboxes with no credit card required. The[Quickstart](https://docs.agentmail.to/quickstart) gets you one in under a minute.


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
