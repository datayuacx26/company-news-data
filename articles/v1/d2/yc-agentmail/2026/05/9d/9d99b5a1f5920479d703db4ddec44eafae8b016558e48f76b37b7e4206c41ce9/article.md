---
schema_version: "1.0.0"
document_id: "9d99b5a1f5920479d703db4ddec44eafae8b016558e48f76b37b7e4206c41ce9"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/build-email-agents-complete-guide"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:9b14be674ad807a49ec82023c3a56cd72d03b29a9fcead6d1b6cae58e3057ace"
---

# Everything you need to know to build Email Agents

*Author's note: Friends, foes, builders, customers, investors. Agents are now officially a thing and I wanted to make an all-in-one guide about why email is important, what we do, and how to build with us or email. I hope this is helpful :)*


## Contents


**Part 1: Why Email**


- Email as the universal communication protocol
- Email as identity, memory, and audit trail


**Part 2: Technical Foundations**


- Agent email infrastructure (how it differs from human email)
- Inbox creation and lifecycle management
- Sending: deliverability, formatting, threading
- Receiving: webhooks, WebSockets, and when to poll
- Thread management and conversation state
- Labels as a state machine
- Drafts and scheduled sending
- Attachments and rich content
- Allow and block lists


**Part 3: Architecture Patterns**


- One inbox per agent vs shared inboxes
- Multi-tenant email with Pods
- Agent-to-agent communication over email
- Human-in-the-loop patterns
- Error handling and retry strategies


**Part 4: Production Deployment**


- Deliverability for non-human senders
- Rate limiting and warm-up
- Monitoring and observability
- Security: SPF, DKIM, DMARC for agents


**Part 5: Use Cases**


- Use cases in production


## Chapter 1: Email as the Universal Communication Protocol


Email is the oldest and most universal digital communication protocol still in active use. Every business has it. Every person has it. Every platform supports it. No other protocol comes close to email's reach.


For AI agents moving from research demos to production deployments, this universality matters. An agent that can email can communicate with anyone, human or machine, without requiring the other party to install software, create an account, or adopt a new protocol.


### What makes email unique for agents


**Asynchronous by design.** Email does not require both parties to be online simultaneously. An agent can send an email and process the reply hours or days later. This matches the natural rhythm of business communication.


**Identity built in.** An email address is an identity. When you give an agent an email address, you give it a way to be contacted, a way to be identified, and a persistent communication channel. No OAuth tokens to expire. No API keys to rotate on the communication side.


**Audit trail by default.** Every email is a record. The thread history is a complete log of the conversation. For regulated industries (legal, finance, healthcare), this is not a nice-to-have but a requirement.


**Interoperable.** Email works across every platform, every device, every organization. Your agent can email a Gmail user, an Outlook user, and a self-hosted mail server with the same code.


### Why existing email services fail agents


1. **No programmatic inbox creation.** You cannot create a new email address with one API call. Each address requires domain verification, DNS configuration, or a human Google account.
2. **Authentication designed for humans.** OAuth 2.0 flows require browser-based consent. Agents do not have browsers (or rather, when they do, the OAuth flow is a hack, not a feature).
3. **Rate limits for human patterns.** Gmail allows 250 emails per day. That works for a human. An agent managing 50 customer support conversations hits that limit before lunch.
4. **Terms of service violations.** Google explicitly prohibits automated access to Gmail accounts. Agents using Gmail API are one audit away from a ban.
5. **No conversation management.** Sending is easy through APIs like SendGrid, Resend, Mailgun, etc. Managing the reply, the follow-up, the escalation, the state of the conversation? That is where existing services offer nothing.


### Enter agent-native email infrastructure


Agent-native email infrastructure is built from the ground up for non-human senders and receivers. The core primitives:


- **Inbox creation via API.** One call creates an address. No DNS, no verification, no human in the loop.
- **Send, receive, reply.** Full bidirectional email, not just outbound.
- **Thread management.** Conversations tracked automatically. Query by thread, not just by message.
- **Labels as state.** Tag messages with arbitrary strings. Use them as a lightweight state machine.
- **Multi-tenancy.** Pods isolate groups of inboxes by customer, workspace, or use case.


## Chapter 2: Email as Identity, Memory, and Audit Trail


An email address is more than a communication channel. It is three things at once.


### Identity


When you create an agent inbox, you create an identity. The email address` support-agent@agentmail.to` is how the outside world knows this agent. Customers reply to it. Systems send notifications to it. Other agents address messages to it.


Unlike API keys (which are secrets), email addresses are designed to be shared. You put them on websites, in signatures, on business cards. This public identity is a feature for agents that need to be reachable.


### Memory


Every email thread is a record of a conversation. The agent does not need a separate database to remember what was discussed with a customer. The thread history is the memory.


This is especially powerful for long-running workflows:


- A recruiting agent's thread with a candidate contains every message exchanged over weeks
- A sales agent's thread with a prospect contains the full negotiation history
- A support agent's thread with a customer contains the complete ticket history


Labels add structured metadata on top of this unstructured memory. A message labeled` \["billing", "escalated", "priority-high"\]` encodes workflow state directly on the communication record.


### Audit trail


In regulated industries, communication records are not optional. Healthcare (HIPAA), finance (SEC/FINRA), and legal (attorney-client privilege) all require complete, tamper-resistant communication logs.


Email provides this by default. Every message has a timestamp, sender, recipient, subject, and body. Thread history shows the full sequence of communication. Labels show how the message was processed. This audit trail exists without any additional infrastructure.


## Chapter 3: Agent Email Infrastructure (How It Differs from Human Email)


Human email infrastructure (Gmail, Outlook) is designed for a person sitting at a computer, reading messages, and typing replies. Agent email infrastructure is designed for software that creates inboxes programmatically, sends messages at scale, and processes replies automatically.


### Key differences


**Ephemeral vs persistent inboxes.** Human email addresses are permanent. You keep your Gmail address for years. Agent inboxes can be either:


- *Persistent* : A support agent that receives customer emails needs a stable address. Customers bookmark it, save it in contacts, reply to old threads.
- *Ephemeral* : An agent that needs to receive a single verification code creates a temporary inbox, receives the email, extracts the code, and deletes the inbox. The address exists for seconds.


**Programmatic lifecycle.** Agents create and delete inboxes based on business logic:


- One inbox per customer (isolate conversations)
- One inbox per campaign (track engagement)
- One inbox per job opening (separate recruiting pipelines)
- Temporary inboxes for one-off tasks (verification, password reset)


This is fundamentally different from human email, where the address is tied to a person, not a workflow.


## Chapter 4: Inbox Creation and Lifecycle Management


### Creating inboxes


The simplest call:


```text
# Creates a new inbox with a default agentmail.to domain
new_inbox = client.inboxes.create()
print(f"Created Inbox: {new_inbox.inbox_id}")
```


This creates an inbox with an auto-generated email address like` my-agent-x7k@agentmail.to` .


### Custom usernames


For branded or predictable addresses:


```text
inbox = client.inboxes.create(
username="webhook-demo",
client_id="webhook-demo-inbox"  # Ensures idempotency
)
```


### Idempotent creation


Use` client_id` to make creation safe to retry:


```text
# The first time this code is run, it creates a new inbox.
inbox = client.inboxes.create(
username="idempotent-test",
client_id="user-123-inbox-primary"
)
print(f"Created inbox: {inbox.id}")


# If you run this exact same code again, it will NOT create a second
# inbox. It will return the same inbox object from the first call.
inbox_again = client.inboxes.create(
username="idempotent-test",
client_id="user-123-inbox-primary"
)
print(f"Retrieved same inbox: {inbox_again.id}")
# The inbox.id will be identical in both calls.
```


This is critical for agents that restart, redeploy, or run in distributed environments.


### Listing and retrieving


```text
# Lists all inboxes in your organization
all_inboxes = client.inboxes.list()
print(f"Total Inboxes: {all_inboxes.count}")


# Get a specific inbox by its ID
retrieved_inbox = client.inboxes.get(inbox_id='my_name@domain.com')
print(f"Retrieved Inbox: {retrieved_inbox.inbox_id}")
```


### Deleting inboxes


Deleting an inbox removes the email address and all associated messages. Use this for ephemeral inboxes after the task is complete.


```text
await client.inboxes.delete(inbox.inbox_id);
```


### Inbox API keys


Each inbox can have its own scoped API keys for fine-grained access control:


```text
# Create a key scoped to one inbox
key = client.inboxes.api_keys.create(
new_inbox.inbox_id,
name="support-agent-key"
)


# The full key is only returned once
print(key.api_key)
```


This is useful when you want to give a specific service access to one inbox without exposing your organization-level API key.


### Lifecycle patterns


**Long-lived service inbox** : Created once during deployment, used for the lifetime of the service. Use` client_id` for idempotency.


**Per-customer inbox** : Created when a customer signs up, deleted when they churn. Isolates conversations per customer.


**Per-task inbox** : Created for a specific task (verification, one-time outreach), deleted when the task completes.


**Per-campaign inbox** : Created for a marketing or sales campaign, kept for reporting, archived after the campaign ends.


## Chapter 5: Sending: Deliverability, Formatting, Threading


### Basic sending


```text
client.inboxes.messages.send(
inbox.inbox_id,
to="your-email@example.com",
subject="Hello from AgentMail!",
text="This is my first email sent with the AgentMail API.",
)
```


### HTML content


Always include a plain text fallback. Some email clients (and some agents) only read plain text.


```text
# Always provide both html and text when possible
client.inboxes.messages.send(
inbox_id="outreach@agentmail.to",
to=["potential-customer@example.com"],
subject="Following up",
text="Hi Jane,\n\nThis is a plain-text version of our email.",
html="<p>Hi Jane,</p><p>This is a <strong>rich HTML</strong> version of our email.</p>",
labels=["outreach-campaign"]
)
```


### Multiple recipients


```text
client.inboxes.messages.send(
inbox_id="outreach@agentmail.to",
to=["customer-1@example.com", "customer-2@example.com"],
subject="Quarterly update",
text="Here's our Q4 update for you both.",
)
```


### Labels on send


Attach labels to outgoing messages at send time to track state from the beginning:


```text
sent_message = client.inboxes.messages.send(
inbox_id="outbound@agentmail.to",
to=["test@example.com"],
subject="Following up on our conversation",
text="Here is the information you requested.",
labels=["follow-up", "q4-campaign"]
)
```


### Deliverability


AgentMail handles SPF, DKIM, and DMARC automatically for all` @agentmail.to` addresses. Every message is properly authenticated out of the box. No DNS configuration needed.


If you configure your own custom domain, you will need to set up the appropriate DNS records (SPF, DKIM, DMARC) to authorize AgentMail to send on your behalf.


What you do need to worry about regardless of domain:


- **Content quality** : Spammy content gets flagged regardless of authentication
- **Sending volume** : Ramp up gradually on new inboxes (see Chapter 18)
- **Recipient engagement** : If recipients consistently mark your emails as spam, deliverability drops
- **Unsubscribe headers** : Include opt-out mechanisms in bulk email


### Threading


Email threading is handled by the` In-Reply-To` and` References` headers. When you use` client.inboxes.messages.reply()` , these headers are set automatically. The recipient sees a threaded conversation. This is AgentMail's differentiator.


```text
# Send the reply
client.inboxes.messages.reply(
inbox_id="support@agentmail.to",
message_id=message_id_to_reply_to,
text="This is our agent's helpful reply!"
)
```


When you use` client.inboxes.messages.send()` with a new subject, it starts a new thread.


## Chapter 6: Receiving: Webhooks, WebSockets, and When to Poll


### Webhooks (recommended for production)


Webhooks push messages to your server the moment they arrive. No delay, no wasted requests, no missed messages during sleep intervals.


Configure a webhook URL in the AgentMail dashboard or via API. When an email arrives at any of your inboxes, AgentMail sends a POST request to your URL with the full message payload.


Events include:


- ` message.received` : a new authenticated email arrived
- ` message.received.unauthenticated` : a new email arrived that failed SPF/DKIM/DMARC checks


Register a webhook:


```text
client.webhooks.create(
url="https://<your-server>.com/webhooks",
event_types=["message.received", "message.sent"],
)
```


Receive webhook events:


```text
from flask import Flask, request, Response


app = Flask(__name__)


@app.route('/webhooks', methods=['POST'])
def receive_webhook():
"""Receives webhook events from AgentMail"""
payload = request.json


event_type = payload.get('event_type')
message = payload.get('message', {})


print(f"\nWebhook received: {event_type}")
print(f"From: {message.get('from_')}")
print(f"Subject: {message.get('subject')}\n")


return Response(status=200)


if __name__ == '__main__':
print("Starting webhook receiver on http://127.0.0.1:3000")
app.run(port=3000)
```


Your webhook endpoint should:


1. Respond with 200 quickly (within 5 seconds)
2. Process the message asynchronously if handling takes longer
3. Be idempotent (you may receive the same event more than once)


### WebSockets (real-time, no public URL)


WebSockets are ideal for local development, CLI tools, or environments where you cannot expose a public endpoint. The connection is persistent, so messages arrive with sub-second latency.


```text
import asyncio
from agentmail import AsyncAgentMail, Subscribe, Subscribed, MessageReceivedEvent


client = AsyncAgentMail(api_key="YOUR_API_KEY")


async def main():
async with client.websockets.connect() as socket:
# Subscribe to inboxes
await socket.send_subscribe(Subscribe(inbox_ids=["agent@agentmail.to"]))


# Process events as they arrive
async for event in socket:
if isinstance(event, Subscribed):
print(f"Subscribed to: {event.inbox_ids}")
elif isinstance(event, MessageReceivedEvent):
print(f"New email from: {event.message.from_}")
print(f"Subject: {event.message.subject}")


asyncio.run(main())
```


TypeScript:


```text
import { AgentMailClient, AgentMail } from "agentmail";


const client = new AgentMailClient({
apiKey: process.env.AGENTMAIL_API_KEY,
});


async function main() {
const socket = await client.websockets.connect();


socket.on("open", () => {
console.log("Connected");
socket.sendSubscribe({
type: "subscribe",
inboxIds: ["agent@agentmail.to"],
});
});


socket.on("message", (event: AgentMail.Subscribed | AgentMail.MessageReceivedEvent) => {
if (event.type === "subscribed") {
console.log("Subscribed to:", event.inboxIds);
} else if (event.type === "message_received") {
console.log("New email from:", event.message.from_);
console.log("Subject:", event.message.subject);
}
});


socket.on("close", (event) => {
console.log("Disconnected:", event.code, event.reason);
});


socket.on("error", (error) => {
console.error("Error:", error);
});
}


main();
```


### When polling is acceptable


Polling (repeatedly calling` client.inboxes.messages.list()` ) is acceptable only for:


- Quick prototyping and demos
- Scripts that run once and exit
- Environments where neither webhooks nor WebSockets are available


```text
# receive messages
for msg in client.inboxes.messages.list(inbox.inbox_id, limit=10).messages:
print(msg.subject, msg.extracted_text or msg.text)
```


In production, polling wastes compute, introduces latency (up to your poll interval), and can miss messages if the interval is too long. Use webhooks or websockets in most cases.


## Chapter 7: Thread Management and Conversation State


### What is a thread


A thread is a sequence of related messages. When you send an email and someone replies, those two messages are in the same thread. Subsequent replies continue the thread.


### Retrieving threads


```text
# This loads a single thread and its messages
thread = client.threads.get(
thread_id="thread_id"
)


print(f"Retrieved thread {thread.thread_id} with {len(thread.messages)} messages.")
```


Inbox-scoped thread retrieval:


```text
thread = client.inboxes.threads.get(inbox_id, thread_id)
```


### Deleting threads


```text
client.inboxes.threads.delete(inbox_id, thread_id)
```


### Thread-based workflows


Threads are the natural unit of work for most agent workflows:


- **Support ticket** : One thread per ticket. The conversation between agent and customer lives in a single thread.
- **Sales outreach** : One thread per prospect. The initial outreach, follow-ups, and replies are all threaded together.
- **Negotiation** : One thread per vendor. Multiple rounds of quotes and counter-offers in one thread.


### Thread state via labels


Combine threads with labels to track workflow state:


```text
# Find all threads from a specific campaign that need a follow-up
filtered_threads = client.inboxes.threads.list(
inbox_id='outbound-agent@domain.com',
labels=[
"q4-campaign",
"follow_up"
]
)


print(f"Found {filtered_threads.count} threads that need a follow-up.")
```


## Chapter 8: Labels as a State Machine


Labels are the most underappreciated feature in agent email. They turn an inbox into a lightweight workflow engine.


### How labels work


Labels are arbitrary strings attached to individual messages. You can add and remove them at any time:


```text
# Let's add a 'resolved' label to a message
client.messages.update(
inbox_id='outbound@domain.com',
message_id='<abc123@agentmail.to>',
add_labels=["resolved"],
remove_labels=['unresolved']
)
```


### Labels as workflow states


Define your workflow states as labels:


```text
# Mark a message as read after processing
client.inboxes.messages.update(
inbox_id="agent@agentmail.to",
message_id=msg.message_id,
add_labels=["read"],
remove_labels=["unread"]
)
```


Each transition is an update call that removes the old state and adds the new one. Query messages by label to find everything in a given state:


```text
# Only fetch unread messages
unread = client.inboxes.messages.list(
inbox_id="agent@agentmail.to",
labels=["unread"]
)
```


### Multi-dimensional labels


Labels are not limited to workflow state. Use multiple dimensions:


- **State** :` unread` ,` processing` ,` resolved` ,` escalated`
- **Category** :` billing` ,` technical` ,` feature-request` ,` bug`
- **Priority** :` urgent` ,` high` ,` normal` ,` low`
- **Source** :` inbound` ,` outbound` ,` forwarded`
- **Campaign** :` q3-outreach` ,` product-launch` ,` renewal`


A single message can have labels from every dimension:` \["processing", "billing", "urgent", "inbound"\]` .


### Why not a database?


You might ask: why not store state in a database? You can, and for complex workflows, you should. But labels give you a zero-infrastructure state machine that lives on the communication itself. The state is right there on the message, visible to anyone (human or agent) who reads the thread.


For many agent workflows, labels are all you need. Graduate to a database when you need:


- Complex queries (joins, aggregations)
- Historical state transitions (audit log of state changes)
- Relationships between entities beyond the email thread


## Chapter 9: Drafts and Scheduled Sending


### Creating drafts


Drafts let you compose a message without sending it immediately. This is useful for human approval workflows, scheduled sending, and batch preparation.


```text
new_draft = client.inboxes.drafts.create(
inbox_id="outbound@domain.com",
to=["review-team@example.com"],
subject="[NEEDS REVIEW] Agent's proposed response"
)


print(f"Draft created successfully with ID: {new_draft.draft_id}")
```


### Scheduled sending


Schedule a draft to send at a specific time using the` send_at` parameter:


```text
from datetime import datetime, timedelta


# Schedule an email for tomorrow at 9:00 AM UTC
send_time = (datetime.utcnow() + timedelta(days=1)).replace(
hour=9, minute=0, second=0
)


scheduled_draft = client.inboxes.drafts.create(
inbox_id="outreach@domain.com",
to=["prospect@example.com"],
subject="Following up on our conversation",
text="Hi, just wanted to follow up on our chat yesterday...",
send_at=send_time.isoformat() + "Z"
)


print(f"Draft scheduled for {scheduled_draft.send_at}")
# send_status will be "scheduled"
```


This is essential for follow-up cadences. Schedule follow-up 1 for day 3, follow-up 2 for day 7, and a break-up email for day 14, all at creation time.


### Managing drafts


```text
# List all scheduled drafts in an inbox
scheduled = client.inboxes.drafts.list(
inbox_id="outreach@domain.com",
labels=["scheduled"]
)


for draft in scheduled.drafts:
print(f"{draft.subject}: scheduled for {draft.send_at} ({draft.send_status})")
```


Get all drafts across the entire organization:


```text
all_drafts = client.drafts.list()
print(f"Found {all_drafts.count} drafts pending review.")
```


### Sending a draft


```text
# This sends the draft and deletes it
sent_message = client.inboxes.drafts.send(
inbox_id='my_inbox@domain.com',
draft_id='draft_id_123'
)


print(f"Draft sent! New message ID: {sent_message.message_id}")
```


### Approval workflow with drafts


Drafts enable a clean approval pattern:


1. Agent creates a draft response
2. Agent notifies a human reviewer
3. Human reviews and approves (or edits)
4. Agent sends the draft


## Chapter 10: Attachments and Rich Content


### Sending attachments


```text
import base64


# A simple text file for this example
file_content = "This is the content of our report."


# You must Base64 encode the file content before sending
encoded_content = base64.b64encode(file_content.encode()).decode()


sent_message = client.inboxes.messages.send(
inbox_id="reports@agentmail.to",
to=["supervisor@example.com"],
subject="Q4 Financial Report",
text="Please see the attached report.",
attachments=[{
"content": encoded_content,
"filename": "Q4-report.txt",
"content_type": "text/plain"
}]
)
```


### Receiving attachments


When a message has attachments, retrieve them via the message:


```text
inbox_id = "inbox_123"
message_id = "<def456@agentmail.to>"
attachment_id = "attach_789"  # From the message object


file_data = client.inboxes.messages.get_attachment(
inbox_id=inbox_id,
message_id=message_id,
attachment_id=attachment_id
)


# Now you can save the file
with open("downloaded_file.pdf", "wb") as f:
f.write(file_data)
```


You can also retrieve attachments at the thread level:


```text
inbox_id = "inbox_123"
thread_id = "thread_abc"
attachment_id = "attach_789"  # From a message within the thread


file_data = client.inboxes.threads.get_attachment(
inbox_id=inbox_id,
thread_id=thread_id,
attachment_id=attachment_id
)
```


### Raw message access


For advanced processing, retrieve the raw email (full MIME content):


```text
raw_mime = client.inboxes.messages.get_raw(inbox_id, message_id)
```


This gives you access to all headers, MIME parts, and encoded content exactly as received.


### Common attachment patterns for agents


- **Receipt parsing** : Customer forwards a receipt, agent extracts data from the PDF/image
- **Contract review** : Customer sends a contract, agent flags risky clauses
- **Resume screening** : Candidate sends a resume, agent extracts qualifications
- **Report generation** : Agent generates a PDF report and emails it to stakeholders


### HTML email for rich content


For agents that need to send formatted content (tables, charts, styled text), use the` html` parameter. Keep it simple. Stick to inline CSS. Email HTML rendering is notoriously inconsistent across clients.


## Chapter 11: Allow and Block Lists


AgentMail provides allow and block lists to control who can send to and receive from your inboxes. Lists can be set at the organization level or per inbox.


For the full API reference and working code examples, see the[Lists documentation](https://docs.agentmail.to/lists) .


### Use cases


- **Unsubscribe handling** : When a recipient opts out, add them to the send block list. The agent physically cannot email them again.
- **Spam prevention** : Block known spam senders at the organization level.
- **Compliance** : Restrict which domains an agent can contact (e.g., only approved vendors).
- **Security** : Limit inbound email to known partners for sensitive inboxes.


## Chapter 12: One Inbox per Agent vs Shared Inboxes


### One inbox per agent


The simplest pattern. Each agent instance has its own email address.


**Pros** : Clean separation of concerns. Easy to track what each agent is doing. No routing logic needed.


**Cons** : More inboxes to manage. External parties need to know which address to email.


**Best for** : Dedicated-purpose agents (support agent, sales agent, recruiting agent).


### Shared inbox


Multiple agents (or agent instances) share a single inbox. Messages are routed to the appropriate handler based on content, labels, or round-robin.


**Pros** : Single email address for external parties. Central point for monitoring. Flexible routing.


**Cons** : Need routing logic to avoid duplicate processing. Harder to debug (which agent handled which message?).


**Best for** : Team inboxes where the sender should not care which agent handles their message.


### The hybrid pattern


Use a shared inbox for receiving, then forward to per-agent inboxes for processing.


## Chapter 13: Multi-Tenant Email with Pods


### What are Pods


Pods are isolated groups of inboxes. Think of them as namespaces. Each Pod has its own set of inboxes, threads, and messages, completely isolated from other Pods.


### Why Pods matter


If you are building a SaaS product where each customer gets their own agent, you need isolation. Customer A's emails should never be visible to Customer B. Pods provide this isolation at the infrastructure level.


```text
from agentmail import AgentMail


client = AgentMail()


# Use client_id to map to your internal tenant ID so
# you don't need to maintain a separate mapping table
pod_a = client.pods.create(client_id="tenant-acme-123")
pod_b = client.pods.create(client_id="tenant-globex-456")


# Each customer gets isolated inboxes
inbox_a = client.pods.inboxes.create(
pod_a.pod_id,
username="support",
display_name="Acme Support"
)


inbox_b = client.pods.inboxes.create(
pod_b.pod_id,
username="support",
display_name="Globex Support"
)
```


### Idempotent pods


Just like inboxes, Pods support` client_id` for idempotent creation. If you call` create` twice with the same` client_id` , you get the same Pod back.


### Per-pod API keys


You can scope API keys to a specific Pod, so a customer's service can only access their own resources:


```text
# Create a key that can only access the support inbox
inbox_key = client.inboxes.api_keys.create(
inbox.inbox_id,
name="support-inbox-key"
)


print(inbox_key.api_key)
```


### Pod use cases


- **Multi-tenant SaaS** : Each customer gets their own Pod with isolated inboxes
- **Department isolation** : Sales, support, and recruiting Pods within a company
- **Environment separation** : Dev, staging, and production Pods
- **Project isolation** : Each project or campaign gets its own Pod


### Pod lifecycle


Pods can be created and deleted programmatically. Deleting a Pod requires cleaning up its resources first:


```text
// This will FAIL if the pod has any resources
await client.pods.delete(podId);


// Correct approach: Clean up resources first
async function offboardCustomer(podId: string) {
// 1. Delete all inboxes
const inboxRes = await client.inboxes.list(podId);
for (const inbox of inboxRes.inboxes) {
await client.inboxes.delete(inbox.inbox_id);
}


// 2. Delete all domains
const domainRes = await client.domains.list({ podId });
for (const domain of domainRes.domains) {
await client.domains.delete(domain.domain_id);
}


// 3. Now you can delete the pod
await client.pods.delete(podId);
}
```


Use this for cleanup when a customer churns or a project ends.


## Chapter 14: Agent-to-Agent Communication over Email


### Why agents email each other


When agents are in different systems, organizations, or trust boundaries, email is the simplest way for them to communicate. No shared database, no API integration, no service mesh.


### Structured data in email


For agent-to-agent communication, the email body can contain structured data (JSON, XML, CSV). The subject line can encode the message type or priority. Labels can track the conversation state.


This is less efficient than an API call but far more flexible. No schema negotiation, no versioning headaches, and a complete audit trail of every interaction.


### When this pattern shines


- **Cross-organization agent collaboration** : A vendor's agent and a buyer's agent negotiating over email
- **Heterogeneous systems** : Agents built with different frameworks (CrewAI, AutoGen, custom) communicating via a universal protocol
- **Audit requirements** : Every inter-agent communication is logged as email


## Chapter 15: Human-in-the-Loop Patterns


Every production agent needs a way to involve humans. The question is when and how.


### Pattern 1: Escalation


The agent handles what it can and forwards the rest to a human.


### Pattern 2: Approval via drafts


The agent drafts a response and waits for human approval before sending. This uses the drafts API (see Chapter 9):


1. Agent creates a draft response
2. Agent notifies a human reviewer
3. Human reviews and approves (or edits)
4. Draft is sent


### Pattern 3: CC oversight


The agent operates autonomously but CCs a human on every message. The human can intervene at any time by replying.


### Pattern 4: Periodic review


The agent operates autonomously. A human reviews a summary of actions taken at regular intervals (daily, weekly) and adjusts the agent's behavior.


### Choosing the right pattern


- **High stakes, low volume** : Approval (legal, finance)
- **Low stakes, high volume** : Escalation with thresholds (support, sales)
- **Medium stakes** : CC oversight (recruiting, operations)
- **Low stakes, well-tested** : Periodic review (internal ops)


## Chapter 16: Error Handling and Retry Strategies


### What can go wrong


1. **Send failures** : SMTP errors, rate limits, invalid recipients
2. **Receive failures** : Webhook endpoint down, WebSocket disconnected
3. **Processing failures** : LLM errors, parsing failures, business logic bugs
4. **External failures** : Recipient's mail server rejects the message, bounce-backs


### Idempotency


Every operation that might be retried should be idempotent. Use` client_id` for inbox creation. Check labels before processing a message (skip if already labeled processed). See the[Idempotency documentation](https://docs.agentmail.to/idempotency) for the full pattern.


### Retry with backoff


For transient failures (network errors, rate limits), retry with exponential backoff. AgentMail returns 429 responses with a` Retry-After` header when you hit rate limits. Respect it.


## Chapter 17: Deliverability for Non-Human Senders


Deliverability is the percentage of emails that reach the recipient's inbox (not spam folder). For agents, this is a make-or-break metric.


### Why agents face unique challenges


Email providers use sender reputation to filter spam. A new agent inbox has no reputation. Sending 500 emails on day one from a fresh address is a fast track to the spam folder.


### Warm-up strategy


Ramp up sending volume gradually:


- Day 1-3: 10-20 emails per day
- Day 4-7: 50 emails per day
- Week 2: 100 emails per day
- Week 3: 250 emails per day
- Week 4+: Full volume


### Content best practices


- **Personalize** : Generic template emails get flagged. Use the recipient's name, reference specific details
- **Balance text and HTML** : Pure HTML emails with no text alternative are suspicious
- **Avoid spam triggers** : "ACT NOW," "FREE," excessive exclamation marks, all caps
- **Include unsubscribe** : Required by law (CAN-SPAM) and improves deliverability
- **Keep it short** : Long emails with high link density get flagged


### Monitoring


Track these metrics:


- **Bounce rate** : Should be under 4%. Higher indicates list quality issues
- **Spam complaint rate** : Should be under 0.1%. Higher kills deliverability
- **Open rate** : Low open rates suggest spam folder placement
- **Reply rate** : High reply rates boost sender reputation


## Chapter 18: Rate Limiting and Warm-Up


### Understanding rate limits


Email providers rate limit in two dimensions:


1. **Sending rate** : How many emails per hour/day from a single address
2. **Receiving rate** : How many emails per hour to a single domain


### AgentMail rate limits


AgentMail's rate limits are designed for agent workloads, not human workloads. The specific limits depend on your plan, but they are significantly higher than consumer email providers. When you hit a limit, AgentMail returns a 429 response with a` Retry-After` header.


### Distributed sending


For high-volume use cases, distribute sending across multiple inboxes.


## Chapter 19: Monitoring and Observability


### What to monitor


**Message flow:**


- Messages sent per hour/day
- Messages received per hour/day
- Reply rate (outbound messages that received a reply)
- Average response time (time between receiving a message and sending a reply)


**Agent health:**


- Error rate (messages that failed processing)
- Escalation rate (messages forwarded to humans)
- Auto-resolution rate (messages handled without human intervention)
- Queue depth (unprocessed messages)


**Deliverability:**


- Bounce rate
- Spam complaint rate
- Delivery rate


## Chapter 20: Security: SPF, DKIM, DMARC for Agents


### Why email authentication matters for agents


Without authentication, anyone can send email pretending to be your agent. A bad actor could send emails from what appears to be your support agent's address, phishing your customers.


### SPF (Sender Policy Framework)


SPF specifies which mail servers are authorized to send email for your domain. AgentMail handles this automatically for` @agentmail.to` addresses. For custom domains, add AgentMail's SPF record to your DNS.


### DKIM


DKIM adds a cryptographic signature to every outgoing email. The recipient's mail server verifies the signature to confirm the email was not tampered with in transit. AgentMail signs all outgoing messages automatically.


### DMARC


DMARC tells receiving mail servers what to do when SPF or DKIM checks fail: nothing, quarantine, or reject. For` @agentmail.to` addresses, DMARC is configured automatically. For custom domains, publish a DMARC record.


### Inbound authentication


AgentMail also validates authentication on inbound email. Messages that fail SPF/DKIM/DMARC checks are delivered with a distinct webhook event (` message.received.unauthenticated` ) so your agent can handle them differently.


### Agent-specific security concerns


- **API key management** : Store API keys in environment variables, never in code. Use inbox-scoped API keys for fine-grained access (see Chapter 4).
- **Content sanitization** : Sanitize email content before processing (especially HTML) to prevent injection attacks
- **Allow/block lists** : Use send and receive allow/block lists to restrict who can email your agent and who your agent can email (see Chapter 11)


## Chapter 21: Use Cases in Production


### Sales (Autonomous SDR)


The autonomous SDR researches prospects, sends personalized outreach, handles replies, and books meetings.


**Key design decisions:**


- **Personalization depth** : Surface-level personalization (name, company) is not enough. Research the prospect's recent activity, company news, tech stack, and pain points.
- **Follow-up cadence** : Day 1 (initial outreach), Day 3 (follow-up 1), Day 7 (follow-up 2), Day 14 (break-up email). Use scheduled drafts for this.
- **Reply classification** : Classify replies as interested, not interested, question, out of office, or wrong person. Each triggers a different workflow.
- **Meeting booking** : When a prospect is interested, send a calendar link. Do not ask them to propose times.


**Metrics** : Open rate (target: 40-60%), reply rate (target: 5-15%), meeting book rate (target: 2-5%).


See[cold-email-researcher](https://github.com/agentmail-to/cold-email-researcher) for a working implementation.


### Customer Support


A tiered system where the AI agent handles first-line support, escalating to humans for complex issues.


**Key design decisions:**


- **Knowledge base** : Start with a simple JSON file of Q&A pairs. Upgrade to vector search only when you have 200+ pairs and matching accuracy drops.
- **Escalation context** : When escalating, forward the original message with the agent's classification, confidence score, and suggested response so the human has full context.


**Metrics** : Auto-resolution rate (target: 60-80%), first response time (target: < 60 seconds), CSAT on auto-resolved vs escalated.


See[agentmail-support-agent](https://github.com/agentmail-to/agentmail-support-agent) for a working implementation.


### Recruiting


A full pipeline from candidate outreach through screening to interview scheduling.


**Key principles:**


- Personalization is mandatory. Generic recruiting emails get ignored.
- Respect "no": When a candidate declines, stop immediately. Do not follow up.
- Screening questions should be tailored. Generate questions based on the role and the candidate's stated experience.
- Compliance: Include opt-out language in every outreach. Respect unsubscribe requests immediately.


See[recruiter-coordinator](https://github.com/agentmail-to/recruiter-coordinator) for a working implementation.


### Legal Intake


An agent that handles the first interaction with potential clients: receive inquiry, send intake questionnaire, classify the case type, score urgency, and route to the appropriate attorney.


**Why email works for legal** : Audit trail is mandatory. Clients can attach documents directly. Asynchronous communication matches the pace of legal work. Email is the expected channel.


See[legal-intake-agent](https://github.com/agentmail-to/legal-intake-agent) for a working implementation.


### Collections


An escalating reminder system: friendly (day 1), firm (day 7), urgent (day 14), final notice (day 30), escalate to human (day 45).


**Reply handling** : Paid: confirm and stop reminders. Dispute: acknowledge and escalate to finance. Payment plan: forward options to finance. No response: continue schedule.


**Compliance (FDCPA)** : Clearly identify as debt collection. Respect cease-and-desist immediately. Maintain complete audit trail. Do not contact outside allowed hours.


See[collections-agent](https://github.com/agentmail-to/collections-agent) for a working implementation.


### Internal Operations


Not every agent emails externally. High-value internal workflows include:


- **Expense reports** : Forward receipts to an agent inbox. Agent extracts data and generates weekly reports. See[receipt-parser-agent](https://github.com/agentmail-to/receipt-parser-agent) .
- **Contract review** : Forward contracts to an agent. Agent compares against standard terms and replies with flagged clauses. See[contract-redline-agent](https://github.com/agentmail-to/contract-redline-agent) .
- **The CC pattern** : CC an agent on existing email conversations. Agent provides summaries, extracts action items, or drafts replies. Replies only to the person who CC'd it. See[cc-the-agent](https://github.com/agentmail-to/cc-the-agent) .


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
