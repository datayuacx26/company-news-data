---
schema_version: "1.0.0"
document_id: "2d9a1ca918c878c8a0f3123685a2c990347104781adfb5783fde626ee816e549"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/state-of-pay-agentic-commerce-payments-and-the-edge/"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:7652bfbce0cb51e89fff450516616f76b74c7519d1dfa16f3e694183a544f934"
---

# State of Pay: Agentic Commerce, Payments, and the Edge

AI agents are starting to search, compare, recommend, negotiate, and in some cases complete transactions on behalf of people or businesses. That creates a new opportunity for the web.


The web is very good at handling human-driven commerce. A person clicks through a storefront, signs in, adds something to a cart, enters payment details, and checks out. That flow assumes a browser, a user session, a checkout page, and a human making the final decision.


Agentic commerce changes that. The buyer may be an AI agent. The seller may be an API, a marketplace, a retail site, or another automated service. The transaction may be a normal cart checkout, a delegated purchase, a subscription, or a tiny machine-to-machine payment for a single request.


That puts new pressure on the protocol layer.


Agentic commerce needs common ways to express intent, identity, authorization, payment requirements, payment proof, and merchant response. Without that, every agent, merchant, payment provider, and platform ends up building a one-off integration. That may work for demos, but it does not scale into a real commerce ecosystem.


This is the beginning of a technical series on agentic commerce and how the edge fits in. In this post, we'll look at the current landscape of agentic commerce, and explore why the edge is becoming an important enforcement point for agent-driven transactions.


## **M2M Payments vs. Agentic Commerce**


Machine-to-machine payments are about one system paying another system. A printer ordering its own ink is a good example. The flow can be narrow: ink supply is low, payment is required, the printer provides proof of payment, and the order API returns a confirmation.


Agentic commerce is broader. A chatbot helping you buy a new wardrobe has to understand your intent, preferences, budget, size, style, merchant options, cart changes, checkout, returns, and support. Payment is only one step in that path.


There is overlap, but they are not the same thing. M2M payments can power agentic commerce, especially when agents pay APIs, tools, or services directly. But agentic commerce also includes cases where an AI agent helps a person buy from a normal merchant experience, such as a website, marketplace, or checkout flow.


That distinction matters at the edge.


**An M2M payment flow can look like access control:**


-


Does this request require payment?


-


Has payment proof been supplied?


-


Has it been verified?


-


Should the request reach the origin?


**An agentic commerce flow has a wider surface area:**


-


Is this a trusted agent?


-


Is the request consistent with the user’s mandate?


-


Should the agent see this product data?


-


Is the request suspicious?


-


What should be logged for compliance, debugging, or dispute resolution?


Both models benefit from being handled close to the request. That is where payment policy, identity, routing, fraud signals, and origin protection start to come together.


## What Changes For Ecommerce


There is a useful parallel with publishing. Search, social, and AI summaries change how readers find content. Publishers still create the content, but more of the audience relationship moved to the platforms that aggregate and rank it.


Agentic commerce could do something similar to ecommerce. If a shopper asks an agent to “buy me the best running shoes under $150,” the agent may compare options across retailers, choose a product, and complete the purchase without the shopper ever visiting the merchant’s site.


That is convenient for the buyer, but it creates real questions for merchants. What happens to the storefront, the product page, the loyalty offer, the bundle, the upsell, and the brand experience? If the agent controls discovery and comparison, merchants may have fewer chances to build loyalty or influence the final decision.


We have already seen parts of this pattern with Google Shopping, Amazon, and marketplaces. Agentic commerce may push it further by moving more of the shopping journey into the automated layer.


That means that ecommerce teams will need more than payment support. They will need ways to control agent access, protect product and checkout APIs, verify trusted agents, understand automated traffic, and enforce policy before those requests reach the origin.


## **Managing Agentic Traffic at the Edge**


The edge can become the programmable control point where agentic commerce policy meets real traffic. Fastly’s Edge Cloud Platform already sits in the request path and can inspect traffic, enforce policy, verify signals, route requests, protect origins, and make commerce flows observable.


The edge is a natural enforcement point for agentic commerce because it already sits between clients and applications. A request comes in. Fastly can inspect it, apply policy, evaluate identity and authorization signals, verify payment-related metadata, and decide whether to serve, block, challenge, or route the request. For APIs and machine traffic, that can mean enforcing payment before an origin is reached. For commerce flows, it can mean helping merchants control which agents can access which routes, products, or checkout actions.


This is not about replacing payment providers. Payments will still happen through Stripe, Visa, Mastercard, Coinbase, wallets, facilitators, banks, or whatever rail the application chooses.


Fastly’s role is the programmable edge layer around the transaction.


**That layer matters because agentic commerce will create new traffic patterns:**


-


More automated product discovery


-


More API-like checkout flows


-


More delegated actions


-


More requests that look like bots but may represent real buyers


-


More need for clear policy, verification, routing, and observability before traffic reaches the origin


The agentic commerce ecosystem is still taking shape. New protocols, frameworks, and payment primitives are emerging quickly. Existing efforts are changing fast, and it is too early to know which standards will have lasting adoption.


That uncertainty is familiar territory for Fastly. We have helped customers adapt through major protocol shifts, from IPv6 and TLS 1.3 to HTTP/2 and HTTP/3. More recently, we have helped customers respond to AI bot traffic at the edge: understand which AI bots are crawling, decide which ones to allow or block, and apply policy before unwanted automation reaches the application.


Agentic commerce follows a similar pattern. Some AI agents will represent real customers and legitimate buying intent. Others may scrape product data, abuse checkout flows, or create new fraud and origin load.


Fastly gives you confidence that your edge infrastructure is built to adapt alongside the ecosystem, so your teams can stay focused on delivering great customer experiences.


## **Adjacent Agent Protocols**


Before getting into commerce and payment-specific protocols, it is worth separating them from broader agent infrastructure like MCP and A2A. MCP helps AI applications connect to tools, data, and workflows. A2A focuses on agent-to-agent communication and coordination. Both may show up in agentic commerce flows, but they do not solve the commerce problem by themselves. They help agents act and collaborate. The protocols below are more focused on shopping, authorization, payment, and trust.


### **Commerce Protocols**


Right now, the industry is rallying around two major commerce frameworks to help AI agents navigate the actual shopping experience:


1.


**UCP, or Universal Commerce Protocol**


This is backed by Google and Shopify. It simplifies how AI agents interact with commerce systems, including merchant catalogs, product availability, pricing, carts, checkout, fulfillment, customer context, and order state. The core idea is that merchants should not need a custom integration for every AI surface. If commerce data and actions can be expressed in a shared format, agents can discover products, build carts, and move toward checkout in a more consistent way.


[Read more about the UCP protocol here](https://developers.google.com/merchant/ucp)


1.


**ACP, or Agentic Commerce Protocol**


This is an open standard backed by OpenAI and Stripe. ACP is focused on programmatic commerce flows between buyers, agents, and businesses. It is the protocol behind OpenAI’s Instant Checkout work with Stripe, and it gives merchants a way to expose products and checkout flows to agent-driven experiences. ACP is less about raw payment settlement and more about making the commerce interaction itself agent-ready.


[Read more about the ACP protocol here](https://www.agenticcommerce.dev/)


While both UCP and ACP enable AI agents to autonomously shop for users, their approaches differ. UCP is an open, decentralized standard designed for broad product discovery and cataloging across the open web. Conversely, ACP is a centralized, highly optimized checkout rail for rapid, tokenized payments within specific AI platforms. Ultimately, they are complementary: merchants can leverage UCP for open-web visibility while utilizing ACP to execute a secure, frictionless final checkout.


### **Authorization and Payment Protocols**


Finding a product and adding it to a cart is not a very difficult infrastructure challenge to solve. The complexity begins when an AI agent needs to authorize a transaction and move actual money.


Unlike a human, a bot cannot input a credit card number or solve a CAPTCHA.


**Making these autonomous transactions work requires backend infrastructure that can handle two distinct steps:**


1.


**Authorization** (proving an agent has the user's explicit mandate to spend money)


2.


**Payment** (the programmatic routing of funds, which ranges from traditional credit networks to raw, machine-to-machine API pings).


**A few competing protocols are emerging to handle these authorization and payment challenges:**


-


**AP2, or Agent Payments Protocol, was** introduced by Google as an open protocol for secure agent payments. The important idea in AP2 is authorization. When an agent spends on behalf of a user, merchants and payment providers need proof that the user actually delegated that authority. AP2 introduces signed mandates that can represent the user’s intent, constraints, and approval.[https://ap2-protocol.org/](https://ap2-protocol.org/)


-


**Visa TAP, or Trusted Agent Protocol,** is Visa’s approach to establishing trust between agents and merchants. TAP is about helping merchants distinguish legitimate delegated agent activity from unknown automation or abusive bots. This is important because agent traffic can look like bot traffic unless there is a trustworthy way to identify who the agent is, who it represents, and what it is authorized to do.[https://developer.visa.com/capabilities/trusted-agent-protocol](https://developer.visa.com/capabilities/trusted-agent-protocol)


-


**Mastercard Agent Pay** is Mastercard’s agentic payments framework. It focuses on trusted AI agents participating in payment flows using Mastercard’s network, tokenization, controls, and authentication capabilities. Mastercard has also introduced Agent Pay for Machines, which is aimed at high-frequency, low-value machine payments that happen continuously in the background.[https://www.mastercard.com/us/en/business/artificial-intelligence/mastercard-agent-pay.html](https://www.mastercard.com/us/en/business/artificial-intelligence/mastercard-agent-pay.html)


-


**x402** takes a different path. It revives the HTTP 402 Payment Required status code and uses it as a web-native payment challenge. A client requests a protected resource. The server returns a 402 with payment requirements. The client pays and retries with payment proof. This is especially interesting for API monetization, agent access, and machine-to-machine payments because payment becomes part of the HTTP request flow.[https://www.x402.org/](https://www.x402.org/)


-


**Stripe MPP, or Machine Payments Protocol,** is an open protocol from Stripe and Tempo for programmatic payments between agents and services. MPP is designed for machine payments such as microtransactions, recurring payments, and automated service-to-service purchases. Like x402, it is closer to the payment primitive layer than the full shopping flow layer.[https://stripe.com/blog/machine-payments-protocol](https://stripe.com/blog/machine-payments-protocol)


In addition to these high-profile protocol initiatives, third parties are also building the identity, wallet, payment, and verification layers around agentic commerce.


**Protocol**


**Layer**


**Primary Function**


**Example Action**


A2A


Comms


Enables direct task delegation and messaging between different AI agents.


A buyer's personal agent negotiates a bulk discount directly with a merchant's sales agent.


MCP


Data Foundation


Securely accesses external data.


Agent reads a store's live inventory and pricing.


UCP


Orchestration


Manages the full shopping lifecycle.


Agent browses items, compares prices, and manages the cart.


ACP


Execution


Handles rapid, in-surface checkout.


Agent executes the final, platform-mediated checkout flow.


Visa TAP


Identity & Trust


Cryptographically verifies agent legitimacy.


Merchant confirms the incoming HTTP request is from a known, trusted AI agent, not a scraping bot.


Mastercard Agent Pay


Credentialing


Issues tokenized cards scoped to specific agents.


Agent checks out using a Mastercard network token bound directly to its identity with a strict $100 limit.


AP2


Authorization


Proves human consent cryptographically.


Bank verifies the user actually approved the $50 spend.


x402


Machine Payments


Handles instant crypto micropayments.


Agent pays a logistics API $0.02 to route a delivery.


MPP


Machine Payments


Enables HTTP 402 payments with session-based streaming across fiat and crypto.


Agent streams $0.003 per API call over Stripe fiat rails without needing to sign up for an account.


## **What We Will Explore Next**


In the rest of this series, we will go deeper on the commerce protocols and the authorization and payment protocols shaping agentic commerce.


We will start with x402 because it maps cleanly to the request path and shows how payment can become part of HTTP access control. From there, we will continue with related posts and demos around Stripe MPP, ACP, and other emerging protocol work.


The goal is to stay close to practical implementation: how these protocols behave in real request flows, where the edge can help connect policy and routing, and what Fastly could bring to the broader agentic commerce ecosystem as it matures.


*Read*[How Fastly and Skyfire Enable Trusted Agentic Commerce at the Edge](https://www.fastly.com/blog/how-fastly-skyfire-enable-trusted-agentic-commerce-at-the-edge) *to explore a real-world implementation using Skyfire's Know Your Agent (KYA) credentials and Fastly Compute to verify AI agents, validate payment signals, and control access to commerce and content experiences.*
