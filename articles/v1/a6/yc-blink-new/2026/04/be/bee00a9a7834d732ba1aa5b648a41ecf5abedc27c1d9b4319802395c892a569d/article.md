---
schema_version: "1.0.0"
document_id: "bee00a9a7834d732ba1aa5b648a41ecf5abedc27c1d9b4319802395c892a569d"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-an-ai-chatbot"
published_at: "2026-04-23T00:18:38+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:52:14.556861+00:00"
content_hash: "sha256:bac946e53daa8bee5729e8f7bac0fb111b1609dce6909ca6cf5d15e02ff75dfc"
---

# How to Build an AI Chatbot for Your Website

## The Real Cost of Off-the-Shelf Chatbot Tools


Here is what the popular platforms actually charge:


Tool Pricing What you get


Intercom (with AI Fin) $150–400/month Mostly conversation management + GPT layer. Knowledge base is basic.


Drift $2,500+/month Enterprise-grade but priced for enterprise. Overkill for most sites.


Freshchat $19–69/seat/month Per-seat pricing scales badly. 10-person team = $690+/month.


Custom on Blink $0–20/month plan + OpenAI usage Your data, your design, your database, your admin panel.


[Drift's pricing](https://builtabot.com/blog/drift-pricing-2026-alternatives-comparison) has been criticized as "$2,500/month for a product that used to cost $400." Intercom's AI Fin layer starts at $150 before you add seats.


A custom chatbot built on Blink runs on your AI model usage costs plus your Blink plan — typically under $30/month all-in at moderate volume.


Custom chatbot vs SaaS chatbot — the real cost difference at moderate usage


Blink


## How to Build Your Chatbot: Step by Step


1


#### Define the chatbot's job


Pick ONE primary job before building anything. Support (answer product questions), sales (qualify leads, book demos), onboarding (walk new users through setup), or FAQ (answer the same 20 questions forever). A chatbot trying to do all four does none well. Write the job in one sentence: "Answer questions about our pricing and features for logged-out website visitors."


2


#### Gather your knowledge base


Collect every document that answers the chatbot's job: help docs, FAQ pages, product pages, pricing tables, onboarding emails. Export them as plain text or markdown. This becomes the RAG corpus — what the LLM searches before answering. A sparse knowledge base produces a hallucinating bot. A thorough one produces a helpful one.


3


#### Build the chatbot on Blink


Go to[blink.new](https://blink.new/) and describe what you need: "Build a support chatbot with a knowledge base, conversation history stored in a database, and an admin panel to view all conversations." Blink generates the full-stack app — React chat widget, backend API, Postgres database for conversation logs, and admin dashboard. No config needed. Database and auth are included automatically. Choose from 200+ AI models to power the responses.


4


#### Test edge cases


Before deploying, test the scenarios that break chatbots: jailbreak attempts ("ignore all instructions"), off-topic questions, very long conversations, and the 5 questions your users ask most often. Edit your knowledge base based on failures. Set a fallback response for when the bot has no good answer.


5


#### Embed or deploy


Blink generates an embeddable chat widget and a standalone URL. Copy the embed snippet to your site's` <body>` tag. Your chatbot is live — with its own database, its own admin panel, and a conversation history that persists across sessions. Custom domain is included.


## What a Good Chatbot Conversation Flow Looks Like


A well-designed chatbot conversation has four states:


1. **Greet and scope.** "Hi — I can help with pricing questions, feature questions, and setup. What are you looking for?"
2. **Answer from knowledge base.** Pull the relevant doc section. Cite it. Keep answers under 150 words.
3. **Acknowledge uncertainty.** "I don't have a reliable answer for that. I can connect you with support if you want."
4. **Escalate gracefully.** Open a support ticket, send an email, or drop a Slack message. Don't leave the user in a dead end.


Conversations that follow this flow have satisfaction rates 40–60% higher than unstructured "ask me anything" bots, according to[Forrester's customer service benchmark](https://www.forrester.com/) . The average cost of a human support ticket is $15–25. A well-trained chatbot resolves 60–70% of tier-1 questions automatically.


Start with 20 high-quality FAQ pairs in your knowledge base. That is enough to handle 60–70% of first-contact questions for most products. Add more based on what users actually ask — not what you think they'll ask.


A good chatbot escalation flow — resolved conversations vs human handoffs


Blink


## Frequently Asked Questions


No. Blink generates the full-stack app from a text description — the chat widget, backend API, database, and admin panel. You describe what you want; Blink builds it. You'll need to gather your knowledge base content and test conversations, but no coding is required.


Add your docs, FAQs, and product pages to the knowledge base section of the app Blink builds for you. The chatbot uses retrieval-augmented generation (RAG) — it searches your content before answering, rather than guessing from general training data. Update the knowledge base anytime from the admin panel.


Design a fallback response in your chatbot's system prompt: "If you don't have a reliable answer, say so clearly and offer to connect the user with support." Blink lets you add escalation logic — open a Zendesk ticket, send an email, or post to Slack — when the bot falls back. Never let a chatbot make up an answer.


Your main cost is LLM API usage — roughly $0.002–0.01 per conversation at GPT-4o pricing for a typical 10-message session. At 1,000 conversations/month that's $2–10 in model costs. Add your Blink plan ($0 free tier or $20/month Pro). Compare that to Intercom at $150–400/month with per-seat fees on top.


Yes. A chatbot built on Blink generates a JavaScript embed snippet you can drop on any page. The same chatbot widget can run on your marketing site, docs site, and help center simultaneously. All conversations route to the same database and admin panel.


The admin panel Blink builds includes a full conversation log — every session, every message, timestamps, and whether the bot answered or escalated. Filter by date, user, or answer quality. Use this data to update your knowledge base weekly for the first month.
