---
schema_version: "1.0.0"
document_id: "08c74fe2d53a2c4348cc5d858066266e1bb946ef30f7ffeada0a1f2bf13ab2dd"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/agent-feeds-protocols-what-to-ship-first"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-24T22:51:30.785703+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:061063c014ca583b45aa34e98c16951cbd99d1d2e05cf9d06fb42497d8c2d127"
---

# Feeds, checkout, and live data: what UCP, ACP, MCP and the ChatGPT product feed actually are

Four acronyms show up in the same meeting and get treated as one decision that has to be made all at once. They are not one decision. They are not even the same kind of thing. One is a file you upload, one is a way to take payment, one is a description of what your commerce systems can do, and one is a way to answer a question in real time.


Sorting them properly changes the roadmap, and for most industrial distributors it changes it in a direction nobody expects. This is the channel layer of[how to structure product data for AI agents](https://www.anglera.com/blog/structure-product-data-for-ai-agents) — what you actually connect the data to once it is in shape.


## What are these four things, actually?


Different layers of the same stack, not four competing options. One publishes a catalog on a schedule, one completes a purchase, one declares capabilities, and one answers live queries. The mistake that costs the most time is treating a checkout protocol and a feed spec as alternatives to each other.


What it is Kind Governance Status as of July 2026


**ChatGPT product feed** A structured catalog file (or API push) merchants supply so ChatGPT can index and display products Feed OpenAI, unilaterally Approved partners only


**ACP** (Agentic Commerce Protocol) An API contract for an agent to create and complete a checkout session against the merchant's systems, with a scoped delegated payment token Checkout Technical Steering Committee; OpenAI, Stripe, Meta as lead maintainers Date-versioned;` 2026-04-17` latest stable


**UCP** (Universal Commerce Protocol) A machine-readable profile at` /.well-known/ucp` declaring which commerce capabilities a business supports — catalog, cart, checkout, order, identity Capability profile spanning all three Governing Council electing Domain Tech Councils; multi-vendor Announced Jan 2026; spec version` 2026-04-08`


**MCP** (Model Context Protocol) A JSON-RPC interface for exposing tools, resources and prompts to a model host Live data / tool interface Agentic AI Foundation, a directed fund under the Linux Foundation Current revision` 2025-11-25` ; next revision due 28 July 2026


Two of those four are open standards you can read in full without asking anyone.[ACP's repository](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) and[UCP's](https://github.com/universal-commerce-protocol/ucp) are both Apache-2.0. That matters more than it sounds: you can evaluate whether your business is expressible in the data model before committing engineering time, which is exactly the check most teams skip.


## Which one is the feed, and what does it cost to publish?


OpenAI's[Product Feed](https://www.anglera.com/glossary/product-feed) Spec is the feed. It is a daily full catalog file with intraday updates, and OpenAI's own guidance is to["provide the entire feed once a day via file upload, and then send updates throughout the day via the API"](https://developers.openai.com/commerce/guides/get-started) . The required fields are unsurprising — item ID, title, description, URL, brand, image, price, availability, seller name and URL, target countries — plus two eligibility flags that decide whether a product appears in search, in checkout, or both.


Two constraints matter far more than the field list.


The first is access. OpenAI states plainly that["onboarding product feeds in ChatGPT is currently available to approved partners"](https://developers.openai.com/commerce/guides/get-started) , with an application form. This is not a spec you implement over a weekend and go live with.


The second is category. OpenAI's commerce documentation says it only allows["products and services that are legal, safe, and appropriate for a general audience,"](https://developers.openai.com/commerce/llms-full.txt) with prohibited categories including "harmful or dangerous materials, weapons, prescription-only medications" and others. Read that against an MRO catalog. Solvents, aerosols, compressed gases, abrasives, cutting fluids, ammunition components in an ag-and-outdoor line — a meaningful share of an industrial assortment is not a general-audience product, and no amount of enrichment changes that.


If you sell janitorial paper goods, foodservice smallwares, or office and packaging supplies, the consumer shopping feed may well be worth applying for. If your top revenue categories are welding gas, chemical, or industrial safety, it is honest to say the channel is not built for you yet and spend the quarter elsewhere.


## What does ACP actually cover?


Checkout and payment, plus a feed schema. Its stated scope is["an interaction model and open standard for connecting buyers, their AI agents, and businesses to complete purchases seamlessly."](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol) The` 2026-04-17` spec directory carries schemas for[agentic checkout](https://www.anglera.com/glossary/agentic-checkout) , delegated payment, delegated authentication, cart, discount, extensions, and a product feed.


The B2B question comes up immediately, and the answer is narrow but real. ACP's checkout payload includes` purchase_order_number` and a` payment_terms` field with values from` immediate` through` net_90` . Those arrived in the January 2026 revision and persist into the current one.


Note precisely what they are. They describe how a transaction settles. They do not describe how a price is arrived at. There is no field in the specification for a contract price, a customer price level, a quantity break, or a quote — the entire pricing model assumes the price was already fixed before the agent showed up. For a distributor whose customers each see different numbers on the same SKU, that is not a gap in the implementation. It is a structural property of publishing prices in a file.


Stripe describes ACP as["an open standard created by Stripe, OpenAI, and Meta that defines how AI agents interact with businesses to complete purchases on behalf of buyers."](https://docs.stripe.com/agentic-commerce/acp) Worth reading as written: purchases, on behalf of buyers, at a known price.


## What does UCP do differently?


It moves the question to request time. Rather than publishing a catalog and hoping the values hold, a UCP-participating business hosts a profile declaring what it can do, and agents call it. Google describes UCP as["an open-source standard designed to power the next generation of agentic commerce,"](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/) noting that it is compatible with the Agent Payments Protocol and offers integration["via APIs, Agent2Agent (A2A), and the Model Context Protocol (MCP)."](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/)


That last clause is the one to underline, because it means UCP and MCP are not alternatives. In production they are already the same stack: Shopify's documentation states that its["Global Catalog MCP implements the UCP Catalog capability and its MCP binding,"](https://shopify.dev/docs/agents/catalog/global-catalog) with tool names and request shapes conforming to the UCP specification.


UCP is also the only one of the four with a coherent answer on customer-specific pricing, and it gets there by refusing to model it as a field. Its identity-linking specification carries a worked B2B wholesaler example and states the principle directly: whether a user is B2B-eligible, what pricing they see, and what payment terms apply are["user attributes the merchant resolves at runtime, not additional scopes."](https://github.com/universal-commerce-protocol/ucp) The catalog scopes follow the same logic, covering lookup and search on behalf of an authenticated user with "personalized pricing or availability."


The limits are geographic and operational. Google's Merchant Center guidance on UCP checkout states it["only applies to products with eligibility in the United States, Canada, and Australia, and for participating merchants and partners,"](https://support.google.com/merchants/answer/16837055) and requires a Google Pay and Wallet Console account, a PSP integrated with the Google Pay API, and a checkout-eligibility product attribute. It also confirms the thing every distributor asks about first: "You will remain the seller of record."


## Why MCP is the one that fits a distributor


Because it is not a commerce protocol, and a distributor's business is not a consumer checkout. MCP defines how a server exposes["Resources"](https://modelcontextprotocol.io/specification/2025-11-25) — context and data — along with prompts and tools, over JSON-RPC. It carries no opinion about carts, shipping options, or who the seller of record is. You expose the operations that describe your business and nothing else.


For a catalog, that shape is close to ideal. A tool that looks up a part number and returns attributes. A tool that resolves a superseded number to its current replacement. A tool that returns availability across branches, or a price for an authenticated account. None of those are expressible in a static feed, and all of them are ordinary MCP tools.


Governance is also now less of a bet than it was. Anthropic donated MCP to the[Agentic AI Foundation](https://blog.modelcontextprotocol.io/posts/2025-12-09-mcp-joins-agentic-ai-foundation/) , a directed fund under the Linux Foundation, in December 2025 —[co-founded by Anthropic, Block and OpenAI with support from Google, Microsoft, AWS, Cloudflare and Bloomberg](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) . The[current protocol revision is 2025-11-25](https://modelcontextprotocol.io/specification/versioning) , with the next revision scheduled for 28 July 2026, so pin a version rather than tracking the tip.


On the consumption side, both major assistants already accept remote MCP servers from end users. Claude calls them[custom connectors](https://claude.com/docs/connectors/custom/remote-mcp) . OpenAI now packages them as plugins, reserving "connectors" for its own[maintained MCP wrappers](https://developers.openai.com/api/docs/guides/tools-connectors-mcp.md) . Either way, the artifact you build is an MCP server.


## What to ship, in order


1. **Server-rendered product data with correct structured markup.** This is not a protocol decision and it precedes all of them. Every interface above reads a catalog, and none of them repairs one. If your specs render client-side or live in linked PDFs, everything downstream inherits the hole.
2. **An MCP server over your catalog.** Broadest reach for the least commitment, works for authenticated B2B pricing, and — because UCP binds to MCP — it is not wasted work if you go further later.
3. **UCP, if you sell into US, Canada or Australia and your assortment is consumer-adjacent.** Read the specification first and check whether your pricing model survives the round trip.
4. **The ChatGPT product feed, if you are approved and your categories qualify.** For a pure industrial catalog, that is a real if.


## What to skip for now


Anything that requires you to re-platform to reach an agent. Anything that asks you to publish contract prices in a static file, which the standards do not support and your customers would not want. And an` llms.txt` file, which has attracted[adoption in the neighborhood of one in ten large sites](https://www.rankability.com/data/llms-txt-adoption/) and is not yet fetched at meaningful volume by the crawlers that matter.


The uncomfortable summary for an industrial distributor in mid-2026 is that the headline channel is not open to you, and the boring answer is the right one: complete attributes, correct identifiers, server-rendered pages, and a live interface over the top. That combination works for every reader in the table, including the ones that don't exist yet.


Sources:


- [OpenAI: Agentic commerce — get started](https://developers.openai.com/commerce/guides/get-started)
- [OpenAI: Commerce documentation, full text](https://developers.openai.com/commerce/llms-full.txt)
- [Agentic Commerce Protocol repository](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol)
- [Stripe: Agentic Commerce Protocol](https://docs.stripe.com/agentic-commerce/acp)
- [Google Developers Blog: Under the hood — Universal Commerce Protocol](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/)
- [Google Merchant Center: Checkout with UCP](https://support.google.com/merchants/answer/16837055)
- [Universal Commerce Protocol repository](https://github.com/universal-commerce-protocol/ucp)
- [Shopify: Global Catalog MCP](https://shopify.dev/docs/agents/catalog/global-catalog)
- [Model Context Protocol: specification 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25)
- [Model Context Protocol joins the Agentic AI Foundation](https://blog.modelcontextprotocol.io/posts/2025-12-09-mcp-joins-agentic-ai-foundation/)
