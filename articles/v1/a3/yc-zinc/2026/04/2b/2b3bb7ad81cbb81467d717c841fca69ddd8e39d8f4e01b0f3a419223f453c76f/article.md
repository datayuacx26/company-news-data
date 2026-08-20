---
schema_version: "1.0.0"
document_id: "2b3bb7ad81cbb81467d717c841fca69ddd8e39d8f4e01b0f3a419223f453c76f"
company_key: "yc-zinc"
company: "Zinc"
source_id: "yc-zinc-news-import-01c857d42648"
canonical_url: "https://www.zinc.com/blog/what-is-ap2"
published_at: "2026-04-16T00:00:00+00:00"
first_seen_at: "2026-07-22T21:08:25.932611+00:00"
fetched_at: "2026-07-28T22:15:52.361502+00:00"
content_hash: "sha256:232f4f47350a05a87de9fb6e45a3c8b159f6f64008bb773419804906c2ba7584"
---

# What Is AP2? The Agent Payments Protocol Explained

When an AI agent pays for something, there's a trust problem nobody talks about until they try to build for it at scale: **how does a merchant know the agent is actually authorized to spend the user's money?**


Signed API keys don't scale (every new service needs its own key). OAuth doesn't model delegated spending well. Pre-funded wallets work, but they leave no audit trail linking a specific purchase to a specific human authorization.


The Agent Payments Protocol (AP2) is Google's answer. Open protocol. Backed by 60+ partners including Adyen, American Express, Mastercard, PayPal, Coinbase, Revolut, and Worldpay. Designed as an extension of Google's Agent2Agent (A2A) protocol and Model Context Protocol (MCP). The core primitive is the **Mandate** : a cryptographically signed JSON credential that proves a specific human authorized a specific agent to make a specific kind of payment.


This guide walks through what AP2 is, its three mandate types (Intent, Cart, Payment), how it works across both fiat and crypto rails via the A2A x402 extension, and when to use it.


---


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


## AP2 in one paragraph


AP2 defines a set of cryptographically signed, verifiable digital credentials (VDCs) called **Mandates** that encode a user's payment instructions. The agent carries the mandate through a transaction. The merchant, payment network, and any other party can verify the mandate's signature to confirm the user authorized this specific action. It works with traditional card networks (through partnerships with Mastercard, Visa, Amex) and crypto (through the A2A x402 extension with Coinbase and MetaMask). The spec is at[ap2-protocol.org](https://ap2-protocol.org/) and the reference implementation is at[github.com/google-agentic-commerce/AP2](https://github.com/google-agentic-commerce/AP2) .


---


## The problem AP2 solves


Agent commerce creates a specific trust gap: the agent is acting on a user's behalf, but existing payment infrastructure has no way to verify that.


Consider what happens today when an agent makes a purchase:


1. The agent holds an API key or a pre-funded wallet
2. The agent calls a merchant, presents payment credentials
3. The merchant charges the credential
4. Nobody has a cryptographic record of what the user actually authorized


If the agent buys something the user didn't want, there's no verifiable proof of what was in bounds. Chargeback disputes become "he said / she said." Enterprise compliance becomes impossible. Regulators have no audit trail.


AP2 fixes this by making the user's authorization a first-class, verifiable, tamper-proof object. The user signs a Mandate once. The agent carries it through the transaction. Every party can verify cryptographically that this specific purchase was inside the user's stated intent.


This matters for:


- **Enterprise compliance** : Procurement officers need audit trails that hold up in a review
- **Chargeback resolution** : Card networks need proof the user actually authorized a specific charge
- **Regulatory reporting** : As agent commerce scales, regulators will require verifiable provenance
- **Multi-agent systems** : When agents call other agents, the authorization chain needs to be provable


---


## The three mandate types


AP2 defines three kinds of Mandates, each for a different trust model.


### Intent Mandate


The user signs an Intent Mandate to pre-authorize an agent to spend within a scope. The agent can act later without real-time user approval.


**What an Intent Mandate carries:**


- Spending limit (e.g., $500 total)
- Category constraints (e.g., "groceries only" or "business travel")
- Time window (e.g., "valid for 30 days")
- Approved merchants (optional allowlist)
- Agent identity (which agent is authorized)


**Example use case:** "My shopping agent can spend up to $100/week on groceries for the next month." The user signs this once. The agent orders Monday-Friday without further interaction.


### Cart Mandate


The user signs a Cart Mandate at the moment of purchase to approve a specific transaction.


**What a Cart Mandate carries:**


- Specific items and quantities
- Exact prices
- Merchant identity
- Shipping details
- User signature with timestamp


**Example use case:** The agent presents a cart with three items totaling $47.89. The user reviews, approves, signs. The agent proceeds to checkout. The merchant can later prove the user approved this exact cart.


### Payment Mandate


The Payment Mandate is sent to the payment network to signal an agent is involved and carries both the Intent and Cart mandates as proof.


**What a Payment Mandate carries:**


- References to Intent and Cart Mandates
- Payment method credentials (tokenized)
- Agent identity
- Fraud signals


**Example use case:** Mastercard receives a Payment Mandate, verifies the mandate chain (user → Intent → Cart → this specific charge), and processes the transaction with appropriate fraud scoring for agent-initiated payments.


---


## How AP2 works


The transaction flow depends on which mode you use: **human-present** (user approves in real time) or **human-not-present** (agent acts on pre-authorized Intent Mandate).


### Human-present mode


1. User asks agent to buy something
2. Agent finds products, assembles a cart
3. Agent presents Cart Mandate to user for signature
4. User reviews and signs the Cart Mandate (on their phone, in their browser, or through their wallet)
5. Agent forwards signed Cart Mandate to merchant with payment
6. Merchant verifies signatures, processes transaction
7. Payment network receives Payment Mandate and processes


### Human-not-present mode (delegated)


1. User signs Intent Mandate upfront (e.g., "Agent X can spend $200/month on groceries")
2. Time passes
3. Agent decides to make a purchase within the Intent's bounds
4. Agent assembles a cart, generates a Cart Mandate that references the Intent
5. Agent forwards to merchant with Payment Mandate
6. Merchant verifies the Intent is still valid and the Cart is within its scope
7. Transaction processes


The cryptographic chain lets any party verify that:


- A real human signed the Intent
- The Cart is within the Intent's constraints
- The agent executing this is the one the user authorized
- The payment is within the agreed scope


---


## AP2 and other protocols (A2A, MCP, x402)


AP2 doesn't exist in isolation. It stacks with other agent protocols:


### A2A (Agent2Agent)


A2A is Google's protocol for how agents discover and call other agents. AP2 is packaged as an extension to A2A so multi-agent workflows can carry payment authorization across hops.


Example: Agent A asks Agent B to execute a purchase. The Payment Mandate travels with the request, so Agent B can prove to the merchant that the original user authorized this action, not Agent A on its own.


### MCP (Model Context Protocol)


MCP defines how agents call tools. AP2 mandates can flow through MCP tool calls, so an LLM using an MCP server for checkout can attach the appropriate authorization.


### x402 (and A2A x402 extension)


[x402](https://www.zinc.com/blog/mpp-vs-x402) is Coinbase's[HTTP 402-based](https://www.zinc.com/blog/http-402-agentic-commerce) stablecoin payment protocol. The A2A x402 extension (co-developed with Coinbase and MetaMask) lets AP2 mandates carry crypto payment authority alongside traditional payment rails.


This means one AP2 implementation can authorize a card payment on Mastercard, a USDC payment on Base via x402, or a bank transfer - all through the same mandate framework.


---


## Where AP2 fits vs ACP, MPP, and x402


These four get lumped together, but AP2 sits a layer above the other three: it authorizes, while ACP, MPP, and x402 move the money.


These aren't a stack you assemble together - they come from competing companies and mostly solve separate problems, and most agents reach for one of them, not several. AP2 is the exception that can genuinely pair with another: because it only carries authorization and stays agnostic to how the payment settles, an AP2 mandate can sit upstream of a card rail or an[x402](https://www.zinc.com/blog/mpp-vs-x402) crypto payment. Which of[ACP](https://www.zinc.com/blog/what-is-acp) ,[MPP](https://www.zinc.com/blog/what-is-mpp) , or x402 you'd touch depends entirely on what the agent is paying for - merchant checkout, a service API, or a crypto micropayment - and that pairing with AP2 is still early in practice.


---


## AP2 in production


Google announced AP2 in early 2026 and the protocol has been in active development since. As of April 2026:


- **Partners** : 60+ including Adyen, American Express, Coinbase, Mastercard, MetaMask, PayPal, Revolut, Worldpay, Salesforce
- **Reference implementations** : Python samples on Android and Web via the[GitHub repo](https://github.com/google-agentic-commerce/AP2)
- **A2A x402 extension** : Production-ready for crypto payments
- **Broader card-based implementations** : Still maturing; rolling out with individual partners


The protocol's production status is best described as "authorization layer is production-ready for crypto, maturing for cards." Enterprises building on AP2 today typically pair it with a specific rail's implementation (e.g., Stripe for cards, x402 for stablecoin).


---


## Should you use AP2?


**Use AP2 if:**


- Your agents make purchases across multiple platforms where a merchant relationship doesn't exist upfront
- You need verifiable audit trails for compliance, enterprise procurement, or regulatory reporting
- You want cryptographic proof of what the user authorized (for chargeback defense)
- Your agents need to work across both fiat and crypto payment rails
- You're building multi-agent systems where authorization needs to flow through hops


**Skip AP2 if:**


- Your agents only operate within a single payment ecosystem (e.g., pure Stripe) where the PSP handles authorization internally
- Your use case is low-stakes micropayments where audit trails don't matter
- You can't commit to the overhead of managing signed credentials in your agent runtime


**A common practical path:** start with simpler authorization (API keys, pre-funded wallets, PSP-level controls) and add AP2 when compliance, audit, or multi-agent scale requirements emerge.


---


## Getting started with AP2


1. Read the AP2 specification at[ap2-protocol.org/ap2/specification](https://ap2-protocol.org/ap2/specification/)
2. Review the[Python samples](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python) for human-present card flows and x402 scenarios
3. Check the[A2A x402 samples](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python/scenarios/a2a/human-present/x402) if you need crypto payment integration
4. For Android, review[Digital Payment Credentials samples](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/android/scenarios/digital-payment-credentials)
5. For authorization without the full mandate stack, consider whether your PSP (Stripe, PayPal, Adyen) has simpler agent authorization primitives that cover your use case


**A caveat for physical goods commerce** : AP2 only completes a purchase if the merchant on the other end actually accepts AP2 mandates. So far that's happening on the payment-network side - PayPal's integration with Google Cloud's Conversational Commerce Agent, Mastercard's Agent Pay pilots, Gemini Spark - almost always through negotiated, PSP-level agreements. Most big-box retailers (Amazon, Walmart, Target, Best Buy) don't accept AP2 at checkout, and don't expose a shopping API an agent could call instead.


That gap is a different problem than AP2 solves, and it's what[Zinc](https://www.zinc.com/) addresses. Rather than waiting for a retailer to adopt an agent protocol, Zinc gives agents programmatic access to place real orders across 50+ retailers whether or not those retailers offer any API of their own. It is not an AP2 endpoint and doesn't consume mandates - it's the execution layer for the long tail of retail that AP2 doesn't reach. See the[Zinc quickstart](https://www.zinc.com/docs/quickstart) , the[AI shopping agent tutorial](https://www.zinc.com/blog/how-to-build-ai-shopping-agent) , or[Zinc GPT](https://www.zinc.com/blog/zinc-gpt) .


---


## Bottom line


AP2 is the authorization layer of agentic commerce. It doesn't move money. It proves that the user said "yes" to this specific action, in a way every party in the transaction can verify cryptographically.


For most indie builders and small agent platforms, simpler authorization patterns (direct PSP controls, pre-funded wallets) are enough today. For enterprises, multi-agent systems, and any use case that needs audit trails, AP2 becomes important fast as agent volume grows.


AP2 pairs well with the other protocols in the agent payment stack. Use it to authorize. Use[ACP](https://www.zinc.com/blog/what-is-acp) for merchant checkout. Use[MPP](https://www.zinc.com/blog/what-is-mpp) or[x402](https://www.zinc.com/blog/mpp-vs-x402) for service payments — and[Zinc's MPP integration](https://www.zinc.com/blog/mpp) when the agent needs to buy physical goods. Use[Zinc](https://www.zinc.com/) for physical retail execution at big-box stores that don't support any of these protocols natively.


For the full context on how these pieces fit together, read[Agentic Commerce in 2026: The Complete Developer Guide](https://www.zinc.com/blog/agentic-commerce) or[Zinc 2.0](https://www.zinc.com/blog/zinc-2-0) . To see an agent actually complete a real order, try[agent.zinc.com](https://agent.zinc.com/) .


---


## Frequently Asked Questions


### What does AP2 stand for?


AP2 stands for Agent Payments Protocol. It's an open protocol developed by Google with 60+ partners for authorizing AI agent payments using cryptographically signed mandates.


### Who created AP2?


AP2 was created by Google and co-developed with 60+ partners including Adyen, American Express, Coinbase, Mastercard, MetaMask, PayPal, Revolut, and Worldpay. The specification and reference implementation are open source at[ap2-protocol.org](https://ap2-protocol.org/) .


### How is AP2 different from MPP or x402?


AP2 is an authorization layer - it defines how agents prove a user authorized a payment.[MPP](https://www.zinc.com/blog/what-is-mpp) and x402 are settlement layers - they define how the payment actually moves. In a full stack, AP2 authorizes, then MPP or x402 (or traditional card rails) settles.


### What is a Mandate in AP2?


A Mandate is a cryptographically signed JSON-LD credential that encodes a user's payment instructions. AP2 defines three types: Intent Mandates (pre-authorized spending within scope), Cart Mandates (explicit approval for specific items), and Payment Mandates (sent to payment networks to signal agent involvement).


### Does AP2 work with crypto payments?


Yes. The A2A x402 extension, co-developed with Coinbase and MetaMask, lets AP2 mandates carry crypto payment authorization alongside traditional card rails. The same mandate framework works for fiat and stablecoin payments.


### Is AP2 production-ready?


The A2A x402 extension is production-ready for crypto payments. Broader card-based implementations are still maturing as partners roll out their own integrations. Enterprises using AP2 today typically pair it with a specific payment rail's implementation (Stripe, Coinbase, etc.).


### Do I need AP2 to build an AI shopping agent?


No, at least not at small scale. Simpler authorization patterns (API keys, pre-funded wallets, PSP-level agent controls) work for most builders today. AP2 becomes important for enterprise compliance, multi-agent systems, and high-volume agent platforms where verifiable audit trails matter.


### Where can I read the AP2 specification?


The open specification is at[ap2-protocol.org/ap2/specification](https://ap2-protocol.org/ap2/specification/) . The reference implementation is at[github.com/google-agentic-commerce/AP2](https://github.com/google-agentic-commerce/AP2) .


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
