---
schema_version: "1.0.0"
document_id: "67dca1d39e0786792258f6b26b7a45b2166017c26c6ec68268be86c9bdcae885"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-d1044ff9c1aa"
canonical_url: "https://www.photoroom.com/blog/agentic-commerce-protocols-ecommerce"
published_at: null
first_seen_at: "2026-08-10T17:04:42.488487+00:00"
fetched_at: "2026-08-10T17:04:45.659684+00:00"
content_hash: "sha256:5945d5947f50ae9eb81e60e544c4b94162c931b4e3e49c6653db5a2bbcabc111"
---

# ACP vs UCP vs AP2: agentic commerce protocols explained for sellers

Agentic commerce has quickly produced an alphabet of new standards for e‑commerce sellers, including ACP, UCP, AP2, MCP, and A2A. These are agentic commerce protocols: the connective layer between systems and agents that make much of agentic commerce possible.


Remembering each acronym can be difficult, but the real challenge lies in knowing the impacts these protocols have on your small business. Which affects product discovery? Which support checkout? Which authorizes payment? And do any of them require you to take action today?


These protocols and others all serve separate but important functions. ACP and UCP enable commerce between agents and merchants: ACP is designed around “chat-to-buy” experiences, while UCP supports “search-to-buy” journeys. AP2 sits alongside these commerce flows to provide proof of payment authorization. MCP connects AI platforms with external tools and data sources, while A2A allows agents to communicate directly with one another.


For sellers,these protocols create new ways for shoppers to discover and purchase products without replacing the fundamentals of e‑commerce. If anything, **agentic commerce makes a[reliable visual catalog](https://www.photoroom.com/product-catalog) product catalog more important** . Agents need accurate information about products, variants, prices, inventory, merchant policies, and visual assets to understand what you sell and represent it correctly across AI-mediated shopping experiences.


This guide explains how the main agentic commerce protocols differ, how they work together, and what e‑commerce sellers can do to prepare their catalogs for this new buying environment. We’ll also look at where tools like Photoroom can help strengthen the visual product data agents increasingly depend on.


## ACP vs UCP vs AP2: key differences for e‑commerce sellers


**Protocol**


**What it does**


**What it covers**


**What e‑commerce sellers need to know**


ACP


Connects compatible AI applications and sellers for product discovery and commerce flows.


Catalog synchronization, product discovery, fulfillment choices, payment handoff, order creation, and related merchant interactions.


Usually accessed through ChatGPT, Stripe, commerce platforms, or other compatible integrations rather than implemented directly by sellers.


UCP


Provides a broad common language for agentic commerce across consumer surfaces, retailers, and payment providers.


Product details, real-time inventory and pricing, cart management, identity linking, loyalty, checkout, and extensible commerce capabilities.


Available through Google's shopping ecosystem and participating commerce platforms.


AP2


Provides cryptographic proof that the user authorized an agent-initiated payment.


Intent rules, spending limits, merchant restrictions, payment mandates, approval records, and audit trails.


Usually handled by the payment, agent, or commerce infrastructure. It doesn’t determine how a product is discovered.


## What an agentic commerce protocol actually does


Traditional e‑commerce integrations between platforms are usually built one connection at a time. A marketplace, payment provider, shopping assistant, and merchant system all speak different languages through different APIs, data models, and checkout logic. Agentic commerce protocols provide a shared language so those systems can exchange information and actions.


Through protocols, AI agents can request product information, build or update a cart, collect the shopper's approval, pass payment credentials, submit an order, and return a confirmation—all within one, unified experience.


## What are ACP, UCP, and AP2 in plain language?


### Agentic Commerce Protocol: commerce between an agent and a seller


-


**What it is:** The Agentic Commerce Protocol, or ACP, is an open standard co-developed by[OpenAI](https://developers.openai.com/commerce) and[Stripe](https://docs.stripe.com/agentic-commerce/acp) . It was introduced to support commerce flows between compatible AI applications and merchants.


-


**What it does:** ACP defines how an application such as an AI assistant can communicate with a seller to initiate and complete a checkout. The seller remains responsible for much of the customer experience: existing product, order, payment, fulfillment, returns, and customer service systems. The agent simply presents the experience and securely passes the information required to complete the transaction.


-


**Commerce example** : A shopper asks ChatGPT to find a birthday gift under $50 for a friend who's "obsessed with coffee." ChatGPT surfaces a few options pulled live from an Etsy seller's product feed —photos, price, and stock included—then hands the shopper off to the seller's own checkout page to complete the purchase.


-


**How it’s being used in 2026:**[OpenAI shifted ACP's role in March 2026](https://openai.com/index/powering-product-discovery-in-chatgpt/) from in-chat checkout (Instant Checkout) to product discovery. Stripe continues to co-maintain the protocol and offers agentic-commerce tooling for businesses and platforms.


-


**What this means for sellers:** ACP depends on[accurate visuals](https://www.photoroom.com/blog/fidelity-gap-ai-product-photography) and product information to help shoppers discover products. Even when checkout happens on the seller’s site, inconsistent product images, missing variants, or unclear catalog data can weaken how products are represented before the shopper ever reaches checkout.


### Universal Commerce Protocol: a broader commerce language


-


**What it is:**[The Universal Commerce Protocol](https://ucp.dev/) , or UCP, is Google's open-source standard for agentic commerce. Google developed it with retailers and commerce platforms including[Shopify](https://www.photoroom.com/product-catalog/shopify) ,[Etsy](https://www.photoroom.com/use-cases/sell-on-etsy) , Wayfair, Target, and Walmart, with support from a wider group of payments and retail companies.


-


**What it does:** UCP covers the entire commerce journey across consumer surfaces, businesses, and payment providers. It can retrieve real-time product information, manage carts, link customer identity and loyalty, and support checkout experiences.


-


This flexibility is what makes UCP unique; it can handle your store’s specific quirks—such as discount rules, shipping options, and product variants—which allows for broader discovery and stronger customer/product match.


-


**Commerce example:** A shopper searches Google's AI Mode for "waterproof hiking boots, size 9, under $120." The results pull live inventory and pricing straight from a Shopify seller's store, and the shopper adds the boots to a Universal Cart alongside items from two other retailers. The shopper can check out for each item under one unified transaction.


-


**How it’s being used in 2026:** Google is using UCP in its own shopping ecosystem, including Universal Cart and agentic-shopping experiences across Search, Gemini, YouTube, and Gmail. Shopify sellers can opt into UCP directly from the Shopify admin to make their catalog visible on these platforms.


-


**What this means for sellers:** UCP makes catalog quality especially important because agents need access to live product information, variants, availability, and other structured commerce data. Product images need to stay aligned with the correct product and variant, so the image an AI shopping experience shows accurately represents what the shopper can actually buy.


### Agent Payments Protocol: proof of authorization


-


**What it is:**[The Agent Payments Protocol](https://ap2-protocol.org/) , or AP2, provides a verifiable record of what a user authorized an agent to do.


-


**What it does:** AP2 uses signed mandates to record the shopper's intent, the approved purchase conditions, and the payment connected to the transaction. This creates an audit trail and allows for guardrails like approved merchants, spending limits, refundability requirements, or a need for explicit confirmation.


-


**Commerce example:** A shopper uses an AI agent to buy a handmade ceramic mug from an Etsy shop, with a standing rule to only auto-approve purchases under $75 from sellers with strong reviews. Weeks later, the shopper disputes the charge with their bank. The seller can point to AP2's signed mandate as proof the shopper's own agent authorized that exact purchase, at that exact price, under those exact conditions.


-


**How it’s being used in 2026:** For small businesses, AP2 is already built into popular marketplaces and payment processors, including Mastercard and Visa.


-


**What this means for sellers:** AP2 doesn’t change how product images are created or managed. Its relevance comes later in the shopping journey: once an agent has identified a product, AP2 helps establish that the associated purchase was authorized under the shopper’s rules. For sellers, it’s another reminder that agentic commerce depends on different systems working together—from accurate product discovery to trusted payment authorization.


The common thread for e‑commerce sellers is catalog readiness. These protocols address different parts of agentic commerce, but none of them compensate for an incomplete or inconsistent product catalog. AI shopping systems still need accurate titles, attributes, variants, availability, and product visuals to correctly identify and represent products.


For sellers, preparing for agentic commerce starts well before checkout: with a catalog that machines can understand and shoppers can trust. Photoroom helps sellers standardize and[scale the visual side of that catalog](https://www.photoroom.com/blog/scaling-product-photos) —from consistent product images and variant-ready assets to accurate AI-generated visuals—so the product an AI shopping system shows is aligned with the item you’re selling.


### Are ACP and UCP competitors?


ACP and UCP both aim to standardize commerce interactions between agents and merchant systems, but they’re not mutually exclusive. ACP is designed to work across agents, payment processors, and business types, and UCP is compatible with other industry protocols. Commerce platforms and infrastructure providers may ultimately support several standards behind one merchant integration.


For your e‑commerce business, focus less on choosing a winner and more on confirming your underlying commerce systems and product catalog can connect through whichever standards the platform adopts.


## How agentic commerce protocols fit together


The simplest way to understand the relationship between protocols is to follow one potential purchase:


1.


A shopper tells an AI assistant what they want and sets constraints such as budget, color, delivery date, or merchant preference.


2.


The shopping experience identifies relevant, suitable products and retrieves current product, price, availability, and offer information from participating merchants.


3.


The agent creates or updates a cart through a commerce protocol such as ACP or UCP.


4.


The shopper approves the purchase directly, or has previously authorized the agent to act within defined limits.


5.


A payment-authorization layer (such as AP2) can provide verifiable proof of that approval and its conditions.


6.


The seller processes the transaction through its existing payments and order systems, then handles fulfillment and support. The purchase is done through protocols, but the seller remains the merchant of record.


Not every real implementation will use all three standards. An ACP commerce flow may use other delegated-payment methods, while a UCP flow may use AP2 as an extension. The important point is that commerce communication and payment authorization are separate responsibilities, even when the shopper never notices the difference.


## How do MCP and A2A fit in agentic commerce?


[Model Context Protocol](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) (MCP) and[Agent to Agent](https://a2a-protocol.org/latest/) (A2A) are often included in protocol lists, but they aren’t direct substitutes for ACP, UCP, or AP2. Here’s how they fit within agentic commerce protocols.


**Protocol**


**What it does**


**Commerce example**


MCP


Lets an AI application connect to external tools, data, and services through a standardized interface.


An agent accesses a merchant's inventory or product-search tool.


A2A


Supports communication and task coordination between separate agents.


A shopper's agent communicates with a retailer or logistics agent.


ACP / UCP


Provides commerce-specific capabilities and data exchanges.


The agent retrieves an offer, builds a cart, or initiates checkout.


AP2


Records authorization and payment conditions.


The transaction is associated with a digital trail that shows shopper approval.


## What e‑commerce sellers should do to prepare for agentic commerce


Most sellers don’t need to implement ACP, UCP, or AP2 directly because many e‑commerce platforms are already integrated with agentic commerce protocols, including Shopify, Google, and Stripe. True preparation requires keeping your store and product catalog optimized for agentic commerce workflows.


### 1. Check whether your e‑commerce platform supports agentic commerce


Regularly check for integrations in your e‑commerce platform, payment provider, and marketplaces. Some platforms might introduce toggles for “AI shopping,” “agentic checkout,” or “Instant Checkout” in Settings or Features. Avoid creating a custom protocol implementation without a clear business case.


### 2. Keep your store policies clear and up to date


Maintain accurate titles, prices, inventory counts, shipping policies, and return policies. This will help agents represent your business accurately and reduce the chance a shopper's order gets delayed, rejected, or escalated to a human because the agent couldn't verify a policy on the spot.


### 3. Make your product catalog easy for AI agents to understand


Agents rely on[accurate, detailed product catalogs](https://www.photoroom.com/use-cases/visual-product-catalog) to surface relevant items to shoppers. The strongest catalogs have consistent visuals, alt-text, and structured file names that AI agents can easily decipher. The accuracy of your product catalog will only become more important as new distribution paths become available.


## Why product images matter for agentic commerce


Photoroom’s[State of GenAI in Marketplaces](https://www.photoroom.com/industry-trends/state-of-genai-in-marketplaces-2026) (2026) report found that 87% of shoppers ranked product visuals as the most important factor influencing purchase decisions. The emergence of agentic commerce and ACP, UCP, and AP2 protocols doesn’t change this fact.


Data based on a recent Photoroom's survey with 1,500 respondents in the UK.


OpenAI has expanded ChatGPT shopping to support visual browsing, side-by-side comparison, and image-led discovery. Google's agentic shopping experiences depend on product data and imagery supplied through merchant systems. Your product catalog now needs to work for two audiences: shoppers making purchase decisions and AI systems interpreting, comparing, and surfacing products.


Each product image should:


-


Be associated with the correct product and variant


-


Meet the relevant channel specifications


-


Accurately represent color, shape, materials, labels, and other product details


-


Remain consistent with the title, attributes, price, and offer


-


Stay visually consistent across the catalog


**This is where Photoroom fits into agentic commerce** . Photoroom helps sellers build and maintain the visual layer of a reliable product catalog: standardizing product images, creating additional listing visuals, preparing channel-ready assets, and[processing catalog updates at scale](https://www.photoroom.com/blog/ecommerce-ai-tools) .


For[Shopify sellers](https://www.photoroom.com/integrations/shopify) , Photoroom connects product images directly to the catalog. Sellers can edit and[generate listing images](https://www.photoroom.com/tools/ai-images) , associate images with individual variants, generate descriptive alt text and SEO-friendly file names, and publish updated assets back to Shopify. Photoroom also supports[editing hundreds of catalog images at once](https://www.photoroom.com/batch) , making it easier to keep visual assets consistent as catalogs grow.


Across all these functions, Photoroom prioritizes product fidelity—the built-in[AI Product Fixer](https://www.photoroom.com/tools/product-fixer) allows for automatic and manual updates to AI-edited product photos, so you can[accurately showcase any product](https://www.photoroom.com/blog/how-to-make-ai-product-images-look-real-for-e-commerce) and maintain customer trust.


The goal isn’t to make your product photos look better. It’s to **create a consistent, accurate visual catalog that can function reliably across storefronts, search engines, marketplaces, and AI-shopping experiences.**


## Optimizing product catalogs for e‑commerce protocols


ACP may expand beyond its current ecosystem, UCP may gain more platform implementations, AP2 may become a common authorization layer, and commerce providers may hide several standards behind one merchant-facing integration. At this speed of change, creating new catalogs for every e‑commerce protocol or AI assistant is time-consuming, especially for small businesses.


The smarter choice is to maintain a single authoritative product catalog with accurate data, reliable variants, up-to-date offers, accessible policies, and high-quality visual assets. Photoroom helps make the visual layer of that catalog scalable.


###
