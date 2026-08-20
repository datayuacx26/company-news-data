---
schema_version: "1.0.0"
document_id: "7e5abbfc14024a5d8c012c66183d140b81b94337b694d5fe5d0de3a159497a8f"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/agentmail-vs-gmail-api"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-24T05:21:53.618588+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:4d782dc0850890efd10b036e08e4734a901436bde08a000d02c1f79d0d947212"
---

# AgentMail vs Gmail API

## TL;DR


AgentMail gives AI agents their own email inboxes through a single API. The Gmail API gives applications access to a Gmail user's existing inbox through OAuth. They solve different problems, and the choice depends on whether your agent needs its own address or needs to act on a person's.


The Gmail API is mature, deep, and free if you already have a Google account or a Workspace seat. For an application that acts on a single user's personal Gmail, or for internal automation inside a Workspace tenant your team controls, it is the right tool. The setup involves a GCP project, OAuth consent screen, and restricted-scope verification for production use. The quota system is sized in per-user-per-minute units, with send costing 100 units per call against a 6,000-unit-per-minute cap on new projects (as of May 1, 2026).


AgentMail is built around the inbox as the primitive. Each inbox has its own address, persistent message store, automatic threading, webhooks, WebSockets, multi-tenant pods, and an MCP server. Provisioning is one API call. No GCP project, no OAuth consent screen, no per-mailbox phone verification.


## When the Gmail API is the right call


- The agent acts on a single human user's existing personal Gmail inbox.
- The automation runs inside a Google Workspace tenant your team controls and the same team consents to the OAuth scopes.
- The product needs Gmail-specific features like labels, Workspace admin APIs, or Cloud Pub/Sub push notifications.
- The application is already deep in the GCP ecosystem and another email surface would add a new vendor relationship.


## When AgentMail is the right call


- The agent needs its own email identity, not delegated access to a person's inbox.
- Each agent or tenant needs a scoped identity: own address, own API key, isolated data.
- The product provisions inboxes per customer at signup and needs the operation to be a single API call.
- The workload is autonomous and bursty rather than a human interactively clicking through a Gmail UI.
- The team prefers API-key authentication and a provisioning model without OAuth or app verification.


## What each one is built for


The Gmail API is Google's REST interface to Gmail and Workspace mailboxes. Authentication is OAuth 2.0 against scopes that are classified as restricted, which means apps that go to production need to complete Google's app verification process plus an independent security assessment by a Google-approved lab. Mailboxes are bound to a Google account, so creating a new mailbox programmatically means creating a Google account (with phone verification) or paying for a Workspace seat. Quotas are measured in quota units assessed per user and per project, with daily send caps of 500 emails for consumer Gmail and 2,000 emails for Workspace. Push notifications use Cloud Pub/Sub via the` watch` endpoint, which must be renewed at least every 7 days or notifications stop. Google recommends calling` watch` once per day.


AgentMail is built around the inbox as the API object. Each inbox has its own address on either the shared` @agentmail.to` domain or a custom domain you verify, with its own message store, automatic threading via` In-Reply-To` and` References` headers, webhooks, and WebSocket connections for real-time inbound. Inboxes group into Pods that isolate one tenant's data from another at the infrastructure level. Authentication is an API key. There is no per-mailbox OAuth flow and no restricted-scope verification path because AgentMail mailboxes do not live inside Google.


## The agent loop


Step in the agent loop Gmail API AgentMail


Provision a new mailbox Not supported via API (Google account or Workspace seat required) Native (one API call)


Authentication OAuth 2.0 with restricted scopes; access tokens expire after 1 hour and refresh tokens can be revoked API key


Production verification Google app verification plus restricted-scope security assessment; recertification every 12 months Not required


Send outbound message Build base64url-encoded MIME message; 100 quota units per send Native (plain text or HTML body)


Receive inbound message Cloud Pub/Sub watch, renewed at least every 7 days; or polling` history.list` Native (webhook, WebSocket)


Thread reply against conversation Set` In-Reply-To` and` References` headers in the MIME message Built in (` messages.reply` with message_id)


Default daily send limit 500/day on consumer Gmail; 2,000/day on Workspace Free tier 100/day; Developer and above no daily cap


Rate limit handling Manual backoff on 429; quota units assessed per call Built in


Multi-tenant isolation Per-user OAuth grants in a single GCP project, or separate GCP projects Inbox Pods with scoped API keys


## OAuth, scopes, and the verification path


The Gmail API uses OAuth 2.0 with scopes that determine what an app can read or write. The scopes most agents need (` gmail.send` ,` gmail.readonly` ,` gmail.modify` , and similar) are classified by Google as restricted, which triggers two extra production requirements:


1. **App verification** through Google, which reviews the OAuth consent screen, privacy policy, brand assets, and scope justifications. Internal apps inside a single Workspace organization skip this; external apps don't.
2. **A security assessment** by an independent Google-approved lab, with recertification required every 12 months. Independent assessors set their own pricing for these assessments.


Until verification completes, an app with restricted scopes is capped at 100 users in test mode. Agents that need to provision mailboxes for many customers can't ship to production from test mode.


AgentMail is an API-key product. The agent owns inboxes on AgentMail's infrastructure rather than acting on a Google user, so there is no OAuth consent screen, no scope selection, and no security assessment path.


## Quotas and rate limits


Google measures Gmail API usage in quota units, with limits assessed at multiple levels. As of May 1, 2026, Google updated the published limits. Projects that used the Gmail API between November 2025 and April 2026 keep their previous quotas; projects created on or after May 1, 2026 use the new ones.


- **Per-user, per-minute.** 6,000 quota units on the new schedule. The previous limit was 15,000.
- **Per-project, per-minute.** 1,200,000 quota units shared across all users of the project.
- **Per-user, per-second.** 250 quota units, calculated as a moving average to allow short bursts.


Per-call costs that matter for an agent loop, per Google's Gmail API quota documentation:


- ` history.list` : 2 quota units
- ` messages.list` : 5 quota units (returns message IDs)
- ` messages.get` : 20 quota units (returns a single message body)
- ` threads.get` : 40 quota units
- ` messages.send` : 100 quota units
- ` drafts.send` : 100 quota units


On a new project, the 6,000-per-minute per-user cap translates to a ceiling of 60 sends per user per minute before throttling. Reads are cheaper, but a naive sync that lists message IDs and then calls` messages.get` for each body burns 20 units per fetch.


Google also added an 80,000,000-quota-unit daily threshold per project as of May 1, 2026. The threshold cannot be raised through a quota-increase request. It doesn't trigger billing today; Google has committed to at least 90 days of notice before paid usage begins.


AgentMail's Free tier caps sends at 100 per day. Developer and Startup tiers remove the daily cap and price by inbox count and monthly email volume rather than per-call units.


## Pricing


### Gmail API


Item Cost Notes


Gmail API calls Free today 80M-unit daily threshold added May 2026; not yet billed


Mailbox (consumer Gmail) Free Requires phone verification at account creation


Mailbox (Workspace) $7+/user/month Per seat; pricing varies by Workspace plan


Restricted-scope assessment Set by third-party assessor Annual recertification


### AgentMail


Plan Price Inboxes Emails/month Storage Custom domains


Free $0 3 3,000 3 GB N/A


Developer $20/month 10 10,000 10 GB 10


Startup $200/month 150 150,000 150 GB 150


Enterprise Custom Custom Custom Custom Custom


Dedicated IPs and SOC 2 Type II report on Startup and above. Persistent retention on every plan.


### A note on comparing these numbers


The Gmail API itself is free, so on raw API cost there is no comparison. What's being compared is the operational cost of running an agent on Gmail: per-seat Workspace pricing for each mailbox (or phone verification to create consumer accounts), the engineering cost of OAuth and verification, and the ceiling imposed by per-user quotas and daily send caps. For a multi-tenant SaaS with one mailbox per customer, those costs scale linearly with the customer base.


## Code: the full agent loop


### AgentMail


```text
import os
from dotenv import load_dotenv
from agentmail import AgentMail
from agentmail.inboxes.types import CreateInboxRequest


load_dotenv()
client = AgentMail(api_key=os.getenv("AGENTMAIL_API_KEY"))


# Provision the agent's inbox in one call
inbox = client.inboxes.create(
request=CreateInboxRequest(username="sales-agent")
)


# Send outbound
client.inboxes.messages.send(
inbox_id=inbox.inbox_id,
to="lead@example.com",
subject="Quick question",
text="Hi Alex, saw your post about email infrastructure...",
)


# Reply in-thread when a message.received webhook fires
def on_inbound_email(payload):
message_id = payload["message"]["message_id"]
client.inboxes.messages.reply(
inbox_id=inbox.inbox_id,
message_id=message_id,
text="Thanks for getting back. To answer your question...",
)
```


### Gmail API


The setup happens once: create a GCP project, configure the OAuth consent screen, add the restricted scopes, and complete app verification before going to production. Tokens then need refresh handling, and outbound messages need MIME construction and base64url encoding.


```text
import base64
from email.mime.text import MIMEText
from google.auth.transport.requests import Request
from google.oauth2.credentials import Credentials
from googleapiclient.discovery import build


# Scopes are restricted; production use requires Google verification
SCOPES = [
"https://www.googleapis.com/auth/gmail.send",
"https://www.googleapis.com/auth/gmail.readonly",
"https://www.googleapis.com/auth/gmail.modify",
]


# Access tokens expire after 1 hour; refresh tokens can be revoked
creds = Credentials.from_authorized_user_file("token.json", SCOPES)
if creds.expired and creds.refresh_token:
creds.refresh(Request())


service = build("gmail", "v1", credentials=creds)


# Send: construct MIME, base64url-encode, then send (100 quota units per call)
message = MIMEText("Hi Alex, saw your post about email infrastructure...")
message["to"] = "lead@example.com"
message["subject"] = "Quick question"
raw = base64.urlsafe_b64encode(message.as_bytes()).decode()


service.users().messages().send(
userId="me", body={"raw": raw}
).execute()


# Receive: poll history.list (2 units) or set up Cloud Pub/Sub watch
# Each messages.get costs 20 quota units; threading lives in the MIME headers
```


The Gmail API code works and is well-documented. What it carries with it is the GCP project, the OAuth consent screen, the restricted-scope verification, the token refresh state machine, and the per-user quota accounting your agent has to respect.


## Identity, multi-tenancy, and provisioning


The Gmail API's identity unit is a Google user. Every email operation is scoped to a user's OAuth grant, and that grant requires a real Google account behind it. For a multi-tenant product where each customer needs their own mailbox, this means a Google account per customer, plus an OAuth flow each one has to complete, plus per-user quota that's measured per project. The standard pattern is to either ask each customer to authorize Gmail on signup or to issue a Workspace seat per customer.


AgentMail's identity unit is the inbox. Inboxes group into Pods, and Pods isolate tenants at the infrastructure level. API keys scope to a pod, so a credential issue in one tenant's environment can't reach another's mail. Provisioning a new mailbox at customer signup is one API call with no human in the loop.


## Migrating from the Gmail API to AgentMail


For an agent that's been running on the Gmail API and needs to become a product, the migration replaces three things: the OAuth flow, the per-mailbox provisioning model, and the exposure to Gmail's abuse-detection behavior on Google accounts.


Inboxes provision in AgentMail through` client.inboxes.create()` . Your existing webhook or polling logic for inbound mail becomes AgentMail's` message.received` event or a WebSocket connection. Sending replaces` service.users().messages().send` with` client.inboxes.messages.send` , and threading replaces` In-Reply-To` and` References` header construction with` client.inboxes.messages.reply` .


## Choosing between them


If your agent acts on a person's existing Gmail inbox, the Gmail API is the right tool, and the OAuth and verification work is the cost of doing that job well.


If your agent owns its own address, provisions inboxes per customer, or runs at a scale where per-user quotas and per-seat Workspace pricing don't fit the workload, AgentMail is built for that pattern.


The Free plan provisions real inboxes with full sending and receiving. No credit card required.


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
