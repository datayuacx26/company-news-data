---
schema_version: "1.0.0"
document_id: "5dfe15bf9a0eabb76d28be4915ac25aa3c1f6721bbf380c97a6132c36301f8c3"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/zinc-gpt"
published_at: "2026-01-23T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T22:23:13.149633+00:00"
content_hash: "sha256:8d6dc4f5e32b5d2b39c62ba9d2f218adab172badb141ccd7202e3e1441bcefae"
---

# Zinc GPT: Open-Source AI Shopping Agent with Checkout

**Zinc GPT** is an open-source **AI shopping agent** that searches US retailers in conversation and completes real checkouts — not just product recommendations.


Most agent demos stop at search. Zinc GPT goes end to end: Stripe collects payment,[MPP](https://www.zinc.com/blog/mpp) handles agent-native pay flows, and Zinc places the order. It's a reference implementation for[AI agent commerce](https://www.zinc.com/solutions/ai) you can fork, deploy, or adapt.


**In this post, you'll learn:**


- Why we built this — a working purchase flow, not another search wrapper
- How it works — LLM, SerpAPI, Stripe, and Zinc wired together
- What's included — moderation, webhooks, rate limiting, and checkout
- Get started — clone the repo and run it locally


New to the stack? Read our[AI shopping agent guide](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) , explore[agent skills](https://www.zinc.com/docs/v2/agent-skills/overview) in the[Zinc docs](https://www.zinc.com/docs) , then try the live demo or fork the repo below.


It's open source. You can deploy it yourself.


[Try it live](https://gpt.zinc.com/) |[GitHub](https://github.com/zincio/zinc-gpt)


Zinc Agent


Give your AI agent commerce powers


Connect an agent to Zinc to search products, place orders, track shipments, and handle returns across top retailers.


[Try Zinc Agent](https://agent.zinc.com/)[Read the docs](https://www.zinc.com/docs)


Amazon order


In transit


POST


/orders


Product


AirPods Pro


Tracking


webhook sent


Returns


label ready


## Why we built this


Every few weeks, someone asks us: "What would it look like to build an AI agent that can actually buy things?"


Not just search. Actually complete a purchase — payment, fulfillment, tracking.


So we built the whole thing. Zinc GPT is a working reference implementation. Fork it, break it apart, use it as a starting point for whatever you're building.


## How it works


You chat with an LLM. It searches for products via SerpAPI. When you want to buy, Stripe handles payment, then Zinc places the order. Webhooks keep everything in sync.


## What's included


- AI-powered natural language search across major US retailers
- Stripe checkout for payments
- Zinc integration for order fulfillment
- Webhook handlers for Stripe and Zinc events
- Content moderation for prohibited items
- Rate limiting and input validation


## Get started


Clone the repo, add your API keys, and run it locally:


```text
git   clone https://github.com/zincio/zinc-gpt.git
cd   zinc-gpt
npm     install
cp   .env.example .env
npm   run dev
```


Full setup instructions are in the README:[github.com/zincio/zinc-gpt](https://github.com/zincio/zinc-gpt)


## More demos coming


This is the first. We're going to ship more of these — practical examples of what you can build with Zinc.


What do you want to see next? Reply to this post or email us atsupport@zinc.com .


Zinc Agent


Give your AI agent commerce powers


Amazon order


In transit


POST


/orders


Product


AirPods Pro


Tracking


webhook sent


Returns


label ready
