---
schema_version: "1.0.0"
document_id: "4ccf4a0c955ac2579e36a395169b9b6a1cc58b04a64b040ab1c42a4f281a231f"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/ai-agent-sms-texting-api"
published_at: "2026-04-05T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:d3704bf151f9fe2b0e4d42fc70f6df604bd2b75e1938c302fc38a8a500735940"
---

# AI Agent Texting API: Send SMS & iMessage from Any LLM or AI Agent

The integration is simple: your AI agent generates text, Sendblue delivers it. Here is the full pattern — send a message, receive a reply via webhook, pass it to your LLM, and respond.


### Send a message (Python)


` import requests import json SENDBLUE_API_KEY = "your_api_key" SENDBLUE_API_SECRET = "your_api_secret" def send_message(to_number: str, message: str) -> dict: """Send an SMS or iMessage via Sendblue.""" response = requests.post( "https://api.sendblue.co/api/send-message", headers={ "sb-api-key-id": SENDBLUE_API_KEY, "sb-api-secret-key": SENDBLUE_API_SECRET, "Content-Type": "application/json", }, json={ "number": to_number, "content": message, "send_style": "invisible", # optional iMessage effect }, ) return response.json() # Example: AI agent sends a follow-up send_message("+19175551234", "Hey! Just checking in on your demo request. Want to book a time this week?")`


### Send a message (Node.js)


` const axios = require('axios'); const SENDBLUE_API_KEY = 'your_api_key'; const SENDBLUE_API_SECRET = 'your_api_secret'; async function sendMessage(toNumber, message) { const { data } = await axios.post( 'https://api.sendblue.co/api/send-message', { number: toNumber, content: message, }, { headers: { 'sb-api-key-id': SENDBLUE_API_KEY, 'sb-api-secret-key': SENDBLUE_API_SECRET, 'Content-Type': 'application/json', }, } ); return data; } // Example: AI agent sends a follow-up sendMessage('+19175551234', 'Hey! Just checking in on your demo request. Want to book a time this week?');`


### Handle inbound replies (webhook)


Register your webhook URL in the Sendblue dashboard. When a user replies, Sendblue sends a POST request to your endpoint:


` # Flask webhook handler with OpenAI from flask import Flask, request, jsonify from openai import OpenAI app = Flask(__name__) client = OpenAI() # Store conversation history per phone number conversations = {} @app.route("/webhook/inbound", methods=\["POST"\]) def handle_inbound(): data = request.json phone = data\["number"\] # sender's phone number message = data\["content"\] # the text they sent # Build conversation history if phone not in conversations: conversations\[phone\] = \[ {"role": "system", "content": "You are a helpful sales assistant for Sendblue. Be concise and friendly. Keep responses under 300 characters for texting."} \] conversations\[phone\].append({"role": "user", "content": message}) # Generate LLM response completion = client.chat.completions.create( model="gpt-4o", messages=conversations\[phone\], ) reply = completion.choices\[0\].message.content conversations\[phone\].append({"role": "assistant", "content": reply}) # Send the reply back via Sendblue send_message(phone, reply) return jsonify({"status": "ok"})`


That is the entire loop. Your LLM receives human messages via webhook, generates responses, and sends them back through Sendblue. The recipient just sees a normal text conversation.
