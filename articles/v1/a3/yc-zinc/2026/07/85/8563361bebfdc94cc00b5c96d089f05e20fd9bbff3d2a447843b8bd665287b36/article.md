---
schema_version: "1.0.0"
document_id: "8563361bebfdc94cc00b5c96d089f05e20fd9bbff3d2a447843b8bd665287b36"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/agentic-commerce"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-24T09:14:52.140847+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:35da053ef7f7c897bbd5805858bbdd1381b3b4768f559d2e4543b6622fd6c7f8"
---

# Agentic Commerce in 2026: Where We Stand & What's Next

A year ago, everyone assumed **agentic commerce** meant chatbots. You would ask ChatGPT for socks, and it would buy them.


That experiment ran, and it failed. Walmart put 200,000 products into ChatGPT and saw[conversion rates three times lower](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html) than on its own site. Its EVP of AI called it "a very temporary moment in time." OpenAI has since dropped in-chat checkout entirely.


The energy moved to **background server agents** . Not a consumer chatting with a bot, but an agent in a coding harness or on a cloud server, triggered by a webhook or cron job, *quietly buying things* . The work a VA or ops person used to click through by hand.


**In this guide, you'll learn:**


- What has been tried : the consumer chatbot experiment and why it stalled
- What is actually working today : the honest, short list
- The current hypothesis : agents in a harness, replacing manual buying
- The three-layer stack : model, payment, and execution in production


Here's what to know before you build.


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


## What has been tried: the consumer chatbot experiment


Early predictions imagined every consumer using an AI assistant as their primary shopping interface. The experiment ran, and the results are in:


1. **Conversion collapsed in-chat.** Walmart's Instant Checkout data showed[in-chat purchases converting at a third the rate](https://www.cnbc.com/2026/03/20/open-ai-agentic-shopping-etsy-shopify-walmart-amazon.html) of its own website. OpenAI has pivoted from in-chat checkout to rerouting users back to retailer sites.
2. **Chat strips away visual discovery.** Shopping for fashion, home decor, or personal items is visual. A text thread can't compete with a product grid, reviews, and photos.
3. **Trust stops at the credit card.** People happily ask AI for recommendations. Handing it payment authority inside a chat window is where adoption stalls.


As Walmart's Daniel Danker put it: "This idea that it will all become automated might be a little bit far-fetched."


## What is actually working today?


If you read consulting reports, agentic commerce sounds like a multi-trillion-dollar reality already everywhere. The honest answer: **nothing is at scale today.** What actually exists is:


- **Lightweight search-and-checkout tests in ChatGPT and Perplexity.** Product cards with buy buttons, primarily for Shopify-integrated merchants via[ACP](https://www.zinc.com/blog/what-is-acp) . Real, but early and low-volume.
- **Working demos with real checkout.**[Zinc GPT](https://www.zinc.com/blog/zinc-gpt) and the[Zinc Agent playground](https://agent.zinc.com/) complete actual orders end to end: reference implementations, not mass-market products.


That's the list. Everything else (autonomous fashion agents, agent-to-agent negotiation, high-consideration consumer buying) is still demos and decks.


## The current hypothesis: agents in a harness


The shift is from a broad consumer use case to a narrow, high-value business one: **a business cleaning up the manual purchasing work it always hated doing.**


There used to be a VA or an ops person who clicked through checkouts all day. Outsourcing that to an agent makes far more sense than asking a consumer to change how they shop. These agents don't live in a chat window; instead, they run in a coding environment, on a machine or in the cloud, triggered by inventory thresholds, HR events, or API calls. The user never sees "agentic commerce." They see that the thing got bought.


That's where the energy is, and it's what the rest of this guide is built around.


## The three layers of the agentic commerce stack


Building an agentic commerce system requires three distinct layers. Confusing them is the most common architectural mistake teams make.


### Layer 1: Model & Reasoning (The Brain)


Where the agent parses intent and formulates a plan.


- **Foundation models:** GPT-5, Claude, Gemini
- **Agent frameworks:** Mastra, LangChain, LangGraph, Anthropic's MCP tool ecosystem
- **Environments:** Server-side coding harnesses, cloud workers, headless agents


This layer decides *what* to buy based on user inputs or system triggers. It does not handle payments or fulfillment.


### Layer 2: Payment & Authorization Protocols (The Wallet)


How the agent authenticates and proves payment authority.


- **[ACP (Agentic Commerce Protocol)](https://www.zinc.com/blog/what-is-acp) :** Stripe and OpenAI's standard for merchant-side checkout endpoints (strongly adopted across Shopify).
- **[AP2 (Agent Payments Protocol)](https://www.zinc.com/blog/what-is-ap2) :** Google's protocol for cryptographically signed payment mandates.
- **[MPP (Machine Payments Protocol)](https://www.zinc.com/blog/what-is-mpp) :** An[HTTP 402 Payment Required](https://www.zinc.com/blog/http-402-agentic-commerce) standard letting agents pay inline via stablecoins or card rails without a pre-existing account. (See our[MPP vs x402 comparison](https://www.zinc.com/blog/mpp-vs-x402) ).


These protocols solve *how an agent pays* without requiring a human to manually enter credit card details during execution.


### Layer 3: Execution & Fulfillment (The Hands)


Where the order is actually placed and tracked at a retailer.


- **Bespoke browser automation:** Tools like Browserbase, Browser Use, Puppeteer, or Playwright running headless browser sessions.
- **Dedicated execution APIs:** Platforms like[Zinc](https://www.zinc.com/) providing programmatic purchasing, tracking, and returns across Amazon, Walmart, Target, Best Buy, and 50+ retailers.


#### Browser automation vs. dedicated execution APIs


Many developers start Layer 3 with Browserbase or Browser Use. Browser agents are great for prototypes, but checkout-by-browser at scale is bespoke and volatile:


- Retailer checkout flows change frequently, breaking selectors and prompts.
- Bot protection, CAPTCHAs, and login walls block headless sessions.
- Every failure mode is yours to handle: a mid-checkout price change, an out-of-stock variant, a payment retry, a lost session.


The unglamorous work is what makes execution actually reliable: automated retries when a checkout fails halfway,[tracking-number extraction](https://www.zinc.com/blog/tracking) from retailer emails and portals, price ceilings so an agent never overpays, and[webhooks](https://www.zinc.com/blog/webhooks) so the calling system knows what happened. That operational layer is years of accumulated edge cases: the part a weekend browser script can't replicate, and what a dedicated execution API provides behind a REST interface.


## Why traditional retailer APIs break for agents


When developers try to connect AI agents directly to major retailers, they quickly discover that official retailer APIs do not support consumer checkout:


- **Amazon PA-API:** Read-only product search for affiliates. You cannot place orders.
- **Amazon SP-API:** Seller-only inventory management for merchants selling on Amazon.
- **Walmart Marketplace API:** Seller-side management only.


Official retailer APIs are designed for seller inventory or affiliate referral links, not for an external agent buying products on behalf of a user. That gap is why the execution layer exists: bridging agent intent with real retailer fulfillment.


## Summary & Next Steps


Agentic commerce is shifting from consumer-facing chatbots to background server agents running in harnesses and cloud environments. The models handle reasoning, payment protocols handle authorization, and execution APIs like Zinc handle fulfillment across retailers.


To dive deeper into building and integrating agentic commerce:


- **Build an agent:** Follow our step-by-step tutorial on[How to Build an AI Shopping Agent](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) .
- **Explore protocols:** Read[What Is ACP?](https://www.zinc.com/blog/what-is-acp) ,[What Is MPP?](https://www.zinc.com/blog/what-is-mpp) , and[What Is AP2?](https://www.zinc.com/blog/what-is-ap2) .
- **Start executing:** Explore the[Zinc API Docs](https://www.zinc.com/docs) or test agent-native checkout at[agent.zinc.com](https://agent.zinc.com/) .


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
