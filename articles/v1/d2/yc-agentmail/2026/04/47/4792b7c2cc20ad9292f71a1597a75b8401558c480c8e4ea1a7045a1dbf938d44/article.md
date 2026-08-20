---
schema_version: "1.0.0"
document_id: "4792b7c2cc20ad9292f71a1597a75b8401558c480c8e4ea1a7045a1dbf938d44"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/building-real-time-ai-agents-agentmail-webhooks"
published_at: "2026-04-18T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:b71cbce8b025f2c29659b7372ceda19a12af6be29bfb9e7708537c0ffff58620"
---

# Building real-time AI agents with AgentMail webhooks

Most people building email agents start with polling. Makes sense. Call` list()` on a loop, check for new messages, handle them. It works well enough to ship and there's nothing obviously wrong with it until you try to build something that actually has to feel responsive.


The problem is that polling adds latency you can't fully control. Your agent keeps asking "anything new?" on a fixed interval, but events don't arrive on your schedule. A user replies seconds after receiving your agent's email and your agent won't know until the next cycle. In a conversational product that gap is the thing that makes it feel like a bot. In an outbound sequence it means your agent might fire a follow-up to an address that already bounced, because the bounce event hasn't been ingested yet. Nothing wrong with the agent itself. It just has no way to know what happened between polls.


That's the core issue. It's not a bug. It's an agent operating on a stale snapshot of the world.


There's also a pure efficiency problem. Polling makes an API call every cycle whether or not anything happened. At 10-second intervals that's 8,640 API calls a day. If you're processing 50 emails a day, 8,590 of those calls returned nothing. It's not a token cost since listing messages doesn't touch an LLM, but it is real API quota, and across multiple agents and inboxes, it compounds fast.


AgentMail solves this by flipping the model. Instead of your agent asking "anything new?", AgentMail tells your agent when something happens. A reply lands, your agent knows immediately. An email bounces, your agent knows before it can send a follow-up to a dead address. For anything with a real human on the other end, that's the difference between something that feels alive and something that feels automated.


## Polling vs webhooks


Both are supported. Here's when each makes sense:


Polling Webhooks


Setup No public URL needed Requires public URL


Latency Seconds, depends on your interval Near-instant


API calls 8,640 calls/day at 10s intervals, most return nothing One call per actual event


Missed events Yes, anything that happens mid-cycle No


Best for Batch jobs, scripts, low-frequency checks Any agent with a human on the other end


Neither is wrong. If you're running a nightly batch job that processes emails once an hour, polling is fine. If someone is waiting for a reply, polling is the thing that's going to make it feel slow.


## Getting started with webhooks: four steps


### 1. Create an inbox


```text
from agentmail import AgentMail
from agentmail.inboxes import CreateInboxRequest


client = AgentMail(api_key="YOUR_API_KEY")


inbox = client.inboxes.create(
request=CreateInboxRequest(
username="my-agent",
client_id="my-agent-inbox"  # idempotency key, safe to run on every deploy
)
)


print(inbox.inbox_id)  # my-agent@agentmail.to
```


### 2. Register your webhook


```text
client.webhooks.create(
url="https://your-server.com/webhooks",
event_types=["message.received", "message.bounced"],
client_id="my-webhook-v1"  # idempotency key
)
```


Subscribe to` message.received` to trigger your agent on every inbound reply. Add` message.bounced` to catch hard bounces before your sequence logic fires another email to a dead address.


### 3. Set up your endpoint


```text
import threading
from flask import Flask, request, Response


app = Flask(__name__)


@app.route("/webhooks", methods=["POST"])
def receive_webhook():
payload = request.json


if payload.get("event_type") == "message.received":
msg = payload["message"]
threading.Thread(
target=handle_message,
args=(msg["inbox_id"], msg)
).start()


if payload.get("event_type") == "message.bounced":
threading.Thread(
target=handle_bounce,
args=(payload["bounce"],)
).start()


return Response(status=200)  # return 200 immediately, process in background
```


### 4. Send replies from your handler


```text
def handle_message(inbox_id, msg):
reply_text = your_agent_logic(msg.get("text", ""))


client.inboxes.messages.reply(
inbox_id=inbox_id,
message_id=msg["message_id"],
to=[msg.get("from_") or msg.get("from")],
text=reply_text
)
```


` message_id` threads the reply correctly so it lands in the same conversation, not as a separate email.


## Local dev


You need a public URL for AgentMail to POST to. ngrok is the standard approach and the[setup guide](https://docs.agentmail.to/webhook-setup) covers it in about five minutes. If you'd rather skip that entirely, AgentMail supports WebSockets too, which keep a persistent connection open without needing a public endpoint.


The fastest way to see all of this wired together is the[auto-reply agent example](https://docs.agentmail.to/documentation/examples/auto-reply-agent) in the docs. Inbox creation, webhook registration, reply handling, and idempotency in one complete script. Get your API key at[console.agentmail.to](https://console.agentmail.to/) and you can have this running in under 10 minutes.


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
