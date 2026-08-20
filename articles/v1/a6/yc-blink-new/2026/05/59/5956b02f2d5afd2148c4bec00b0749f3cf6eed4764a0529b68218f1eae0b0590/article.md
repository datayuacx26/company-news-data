---
schema_version: "1.0.0"
document_id: "5956b02f2d5afd2148c4bec00b0749f3cf6eed4764a0529b68218f1eae0b0590"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-whatsapp-chatbot"
published_at: "2026-05-17T00:28:31+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T22:13:06.313483+00:00"
content_hash: "sha256:f89b8aec7bae94e2265e20b8f688bbd77c407c98c7da0888ddd729969e7923b9"
---

# How to Build a WhatsApp Chatbot with AI (Step-by-Step Guide)

## Building your WhatsApp chatbot with Blink


1


#### Open Blink and describe your chatbot


Go to[blink.new](https://blink.new/) and start a new project. Describe what you want: "Build a WhatsApp chatbot that answers customer questions about my store, remembers the conversation history, and can tell customers their order status."


Blink's AI agent generates the webhook endpoint, database schema for conversation history, and the message handler — all in one shot. No boilerplate to write.


2


#### Review the generated webhook handler


Blink creates a POST endpoint at something like` /api/whatsapp/webhook` . It handles:


- Parsing the incoming Twilio request body
- Loading the last N messages from the database for this phone number
- Sending the message and history to your chosen AI model
- Saving the new message and response to the database
- Returning the reply to Twilio in the correct XML format


You can see and edit every line. The database for conversation history is included automatically — no separate Supabase account needed.


3


#### Copy your webhook URL


Click **Deploy** in Blink. Your project gets a live HTTPS URL immediately — no Vercel config, no Railway setup.


Copy the webhook URL. It looks like:` https://your-project.blink.app/api/whatsapp/webhook`


With Blink, hosting is handled automatically — your webhook is public and production-ready from the moment you deploy.


4


#### Set up Twilio WhatsApp


Create a free account at[Twilio](https://twilio.com/) . In the Twilio Console:


1. Navigate to **Messaging → Try it out → Send a WhatsApp message**
2. Twilio gives you a sandbox number to test with immediately
3. Under **Sandbox Settings** , paste your Blink webhook URL into the **"When a message comes in"** field
4. Save


Twilio will now forward every WhatsApp message it receives to your Blink webhook.


5


#### Add your AI model and API key


In your Blink project, add your API key for whichever AI model you want. Blink supports 200+ models — Claude, GPT-4o, Gemini, Llama, and more.


Set it in the environment variables panel. The generated code already references it.


Start with a fast, cheap model like GPT-4o Mini for customer service bots. You can swap models in the Blink UI without touching the code.


6


#### Test with a real WhatsApp message


On your phone, send "join \[sandbox-keyword\]" to the Twilio sandbox number (the keyword appears in your Twilio Console). Once connected, send any message.


Your Blink webhook receives it, queries the AI, stores the exchange in the database, and sends the response back. You'll see it in WhatsApp within 2–3 seconds.


## Handling common message types


A production chatbot handles more than plain text. Here's what the generated handler covers — and what you'll want to customize.


**Text messages** are the default case. The handler strips whitespace, appends the message to the conversation history, calls the AI, and returns the response.


**Commands** are messages starting with a slash or keywords like "help", "reset", or "agent". Add a check before the AI call. For example,` reset` clears conversation history from the database so the bot starts fresh.


```text
if (body.trim().toLowerCase() === 'reset') {
await db.conversations.deleteWhere({ phone: from });
return twiml('Conversation cleared. How can I help you?');
}


```


**Images and media** — Twilio forwards a` MediaUrl` parameter when a user sends an image. Pass that URL to a vision-capable model (GPT-4o or Claude 3.5 Sonnet) and add "Analyze this image:" to the prompt.


**Read receipts** — the WhatsApp Business API supports marking messages as read. Add a` markAsRead(messageId)` call before processing to show the user their message was received.


Twilio's 24-hour customer service window applies: you can reply freely within 24 hours after a user messages you. Outside that window, outbound messages must use an approved template. Keep this in mind for proactive notifications like order updates or appointment reminders.


## Going to production


The Twilio sandbox is for testing. For a real phone number that customers can message:


1. **Register a WhatsApp Business number** in the Twilio Console, or apply directly through Meta's Business Manager
2. **Write a clear system prompt** — give the bot a persona and specific rules. Be direct: "You are a support agent for \[Company\]. Help customers with order tracking, returns, and product questions. Be concise. Escalate to a human when asked."
3. **Set a conversation memory limit** — loading the last 10–20 messages per conversation balances context quality with cost. Loading 100+ messages per request adds latency.
4. **Monitor with logs** — Blink's dashboard shows every request and response, so you can see exactly what the AI received and what it sent back.


Never put your AI API key in client-side code. Keep it in environment variables. Blink handles this automatically — your keys stay in the secure environment, not in the generated code.


WhatsApp chatbot deployed and live — receiving and responding to messages automatically


Blink


## Frequently Asked Questions


Not with Blink. You describe what the chatbot should do, and the AI agent generates the webhook handler, database schema, and response logic. If you want to customize behavior — adding commands, adjusting the system prompt, changing the AI model — the generated code is readable and editable. No DevOps or server configuration required.


Twilio charges $0.005 per message (inbound or outbound), plus Meta's per-template fee for outbound templates outside the 24-hour service window. A bot handling 1,000 conversations per month costs roughly $5–$15 in Twilio fees, plus your AI model's token costs. Blink's hosting is included in your plan — no separate server bill.


Twilio wraps Meta's API in a simpler developer interface and gives you a sandbox number to test with immediately. Meta's direct API requires a verified Facebook Business account and more initial setup. For most projects, Twilio is the faster starting point. Both use the same underlying WhatsApp Business Platform — Twilio adds its $0.005 per-message fee on top of Meta's template charges.


Yes — that's what the database is for. Blink includes a database automatically. The generated handler loads conversation history for each phone number before calling the AI, so the bot has context across multiple messages and sessions. You control how far back it looks — 10–20 messages is typically enough for smooth context without bloating the AI prompt.


Yes. Add a keyword check — when the user types "agent" or "human", the handler can send a Slack notification, create a support ticket, or email your team. Set a flag in the database to pause the bot for that conversation until a human marks it resolved. Describe the handoff flow to Blink and the AI will generate the logic.


Yes, with approved templates. WhatsApp allows businesses to send outbound messages using pre-approved templates — order confirmations, appointment reminders, shipping updates. Submit templates through Meta's Business Manager or the Twilio Console. Once approved, trigger them from a Blink backend endpoint or a scheduled job.


Twilio retries failed webhook deliveries. Blink's hosting is production-grade and handles uptime automatically. For mission-critical bots, set a fallback URL in the Twilio Console — a secondary endpoint that receives messages if the primary fails to respond.


Under an hour for a working chatbot connected to live WhatsApp. Describe the bot, connect Twilio, add your AI API key, and deploy. The hardest parts — wiring the webhook, database, and AI together — are handled by Blink's AI agent. Testing with the Twilio sandbox takes about 5 minutes once everything is connected.
