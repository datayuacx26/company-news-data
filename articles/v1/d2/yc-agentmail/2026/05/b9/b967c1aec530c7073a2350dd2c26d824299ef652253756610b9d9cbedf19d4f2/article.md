---
schema_version: "1.0.0"
document_id: "b967c1aec530c7073a2350dd2c26d824299ef652253756610b9d9cbedf19d4f2"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/cloudflare-vs-agentmail"
published_at: "2026-05-06T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:dc2d015f4bdf7cf17c49dfb9808ed11cb1417c8ba7aac344d94ca16fd0402025"
---

# Cloudflare Email Service vs AgentMail

Cloudflare launched Email Service in April 2026 during Agents Week and positioned it as infrastructure for AI agents. It got attention, and for good reason. Cloudflare has real delivery infrastructure, a developer-friendly platform, and the kind of distribution that makes any launch land. If you're evaluating email options for an agent you're building, it's worth understanding what they shipped and where the two products diverge.


The short version: Cloudflare Email Service is built for sending. AgentMail is an email provider. Inboxes, threads, message history, attachments, and drafts built specifically for agents that carry on conversations over time.


## TL;DR


Cloudflare Email Service is a transactional sending API with programmable inbound routing, a Workers binding, and an open-source reference app for building a managed inbox yourself. It works well if you're already on Cloudflare Workers and need to send email from an agent.


AgentMail is an email provider built for agents. You get a real inbox per agent, its own address, persistent message history, automatic threading, webhooks, drafts, and an MCP server without writing the infrastructure yourself.


## What Cloudflare shipped


The launch has a few distinct pieces.


**Email Sending** is a REST API and a Workers binding (` env.EMAIL.send(...)` ) for outbound email. SPF, DKIM, and DMARC are configured automatically. Pricing is $0.35 per 1,000 emails, and a Workers Paid plan at $5/month minimum is required.


**Email Routing** is Cloudflare's existing free inbound product. It's been around for years. It accepts mail at your domain and forwards it to an address or pipes it into a Worker for processing. There's no storage, no threading, and a 25 MB message size limit.


**Agents SDK email module** is a TypeScript library that lets a Durable Object-backed agent handle inbound mail through an` onEmail()` hook. The routing options include sub-addressing (` agent+id@domain` ) and HMAC-signed reply headers so replies route back to the originating agent instance. This is TypeScript and Workers only.


**Agentic Inbox** is an open-source reference application on GitHub. It's a self-hosted email client with an AI agent, built on Durable Objects, SQLite, and R2. The README is direct about what it is: *"A self-hosted email client with an AI agent, running entirely on Cloudflare Workers."* It shows you how to build a managed inbox from Cloudflare's primitives. You deploy it into your own account.


Notably, Cloudflare requires your sending domain to use Cloudflare DNS. That's not a dealbreaker for teams already on the platform, but it's a hard constraint for everyone else.


## What AgentMail is


AgentMail provisions a real inbox with one API call. That inbox has its own address, stores messages persistently, tracks threads automatically, accepts webhooks and WebSocket connections for real-time inbound, and exposes an MCP server so agents running anywhere can interact with it.


The primitives are different from a delivery API. You create an inbox, messages arrive in it, threads group related messages, and you reply through the thread. Messages don't expire. If a lead replies to your agent's email six weeks later, the context is there.


Inbox Pods group inboxes by tenant. Each pod gets its own API key scope, so a credential issue in one customer's environment can't reach another's mail. That's designed for agent platforms serving multiple customers, not something you bolt on later.


## The agent loop


Step Cloudflare Email Service AgentMail


Send outbound message Native (Workers binding or REST API) Native


Receive inbound message Native (Worker email handler; 25 MB limit) Native (webhooks, WebSocket)


Parse content and attachments Developer handles MIME parsing (postal-mime recommended) Built in


Thread replies against conversation Customer-built; Durable Object SQLite in the reference app Built in


Store conversation history Not included; reference app uses Durable Object SQLite + R2 Persistent, no expiry


Respond in-thread Native send; threading headers set manually Native threaded reply API


Stable identity per agent Sub-addressing on your Cloudflare-DNS domain Inbox per agent, including shared agentmail.to domain


Multi-tenant isolation DIY with Durable Object namespaces and Cloudflare Access Inbox Pods with scoped API keys


## When Cloudflare makes sense


If you're building on Cloudflare Workers and your use case is genuinely transactional, like sending a notification, a magic link, or an OTP. The binding removes credentials from your secret manager, SPF/DKIM/DMARC setup is automatic, and you stay within one billing relationship.


The Agents SDK email module is also a useful primitive if you want agent-per-Durable-Object address mapping and your whole stack lives on Cloudflare. The HMAC-signed reply routing is a thoughtful anti-forgery mechanism.


For teams who want full control and are comfortable writing the inbox layer themselves, the Agentic Inbox reference app is a reasonable starting point.


## When AgentMail makes sense


If your agent receives email and needs to remember what happened across turns, across days, across customers, AgentMail is built for that. The inbox exists as an object. You don't build the storage layer, the thread model, or the attachment handling. You query it.


If your agent runs in Python, in a LiveKit room, in a Google ADK pipeline, or anywhere outside Cloudflare Workers, Cloudflare's inbound system does not reach it. The` onEmail` handler is TypeScript and Workers only. AgentMail's webhooks fire to any URL regardless of where your code runs, and the Python and TypeScript SDKs work without any DNS requirement.


For agent platforms serving multiple customers, Inbox Pods give each tenant their own API key scope and isolated message history, without standing up per-customer infrastructure. And if you need to spin up a fresh inbox at customer signup without DNS configuration, the shared` @agentmail.to` domain lets you do that in one API call.


## Code: what each looks like


### AgentMail


```text
from agentmail import AgentMail
from agentmail.inboxes.types import CreateInboxRequest


client = AgentMail(api_key="your_api_key")


# One call to provision a persistent inbox
inbox = client.inboxes.create(
request=CreateInboxRequest(username="support-agent")
)


client.inboxes.messages.send(
inbox_id=inbox.inbox_id,
to=["customer@example.com"],
subject="Following up on your request",
text="Hi, just checking in on the issue you mentioned last week..."
)


# Inbound message arrives with full content
def on_inbound_email(payload):
message_id = payload["message"]["message_id"]
client.inboxes.messages.reply(
inbox_id=inbox.inbox_id,
message_id=message_id,
text="Thanks for getting back to me. Here's what I found..."
)
```


### Cloudflare Email Service (TypeScript, Workers only)


```text
import { Agent, routeAgentEmail } from "agents";
import { createAddressBasedEmailResolver } from "agents/email";


// Worker entrypoint
export default {
async email(message, env) {
await routeAgentEmail(message, env, {
resolver: createAddressBasedEmailResolver("EmailAgent"),
});
},
};


// Your Agent class
export class EmailAgent extends Agent {
async onEmail(email) {
const raw = await email.getRaw();
// Parse MIME yourself (e.g. with postal-mime)
// Run LLM, update state, decide whether to reply
// Threading state lives in this.state (Durable Object SQLite)
await this.replyToEmail(email, { text: "Got your message..." });
}
}
```


The Cloudflare code works and the routing is elegant. What it requires is writing the MIME parsing, the state schema, the thread model, and the attachment handling yourself. That's the work the Agentic Inbox reference app demonstrates.


## What the difference actually comes down to


Cloudflare's product is built for sending. The inbound side routes messages into a Worker and gives you Durable Object state to store whatever you need. That's powerful if you're already in the Workers ecosystem and want to wire things together yourself.


AgentMail is an email provider. Agents have inboxes. Those inboxes receive email, keep it, and respond to it across time. The use cases are two-way outreach, customer support, recruiting pipelines, tutoring agents, and other workflows where the reply is the point, not an edge case.


The difference shows up in what gets built before the agent does anything useful. Cloudflare's Agentic Inbox reference app on GitHub demonstrates exactly what that work looks like: Durable Objects, SQLite schema, R2 for attachments, MIME parsing, threading logic. That's the infrastructure layer AgentMail provides out of the box.


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
