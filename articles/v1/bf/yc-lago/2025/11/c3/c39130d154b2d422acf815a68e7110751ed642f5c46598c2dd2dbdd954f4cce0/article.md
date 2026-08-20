---
schema_version: "1.0.0"
document_id: "c39130d154b2d422acf815a68e7110751ed642f5c46598c2dd2dbdd954f4cce0"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/flipside-billing"
published_at: "2025-11-13T14:45:11+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:44bcf9db87608f6c2d52d2251c9fd961b64ad7e2c221f9e4f70bb86d7fba6496"
---

# How Flipside Crypto Built Usage-Based Billing for Blockchain Data

## Introduction


Flipside Crypto's data-driven growth platform helps blockchains attract users, drive engagement and analyze on-chain activity. Their data is freely available, but premium features like API access and Snowflake integration are charged mostly on usage. This required a robust metering and billing solution.


Flipside Crypto didn't have a billing system before using Lago. They needed to handle both recurring revenue and usage-based pricing without creating an engineering nightmare or diverting engineers from the core product.


I sat down with Simon Spencer and Kyle Fugere from the Flipside team to learn how Lago helped them simplify billing, pricing and monetization.


## Challenges


Flipside's value is directly tied to how much their customers use their platform. This is why their pricing model combines a monthly fee with a credit system. Customers get usage credits every month, with overages billed separately.


This might sound like a simple model, but it's technically complex. You it requires multiple elements that are individually hard to build.


• (Billable) usage metrics


•[A credit system and user wallet](https://getlago.com/blog/usage-based-pricing-tactics-for-saas-and-ai)


• A way to price, bill and invoice for overages


Building this manually wasn't an option. While Stripe handles payments well, its billing solution lacked the flexibility Flipside needed to track and charge for overages. Luckily, all of these features exist in Lago out of the box. Flipside didn't have to build them from scratch.


Before Lago, they didn't have a dedicated billing platform. From day one, they knew they needed a solution that could do both complex (credits + overages) and simple (subscription pricing) billing.


## Why Flipside chose Lago


Flipside evaluated their options and went with Lago right away. A few key reasons stood out:


- Usage-based billing out of the box: Lago made it easy to define hybrid pricing plans that included both subscription and usage-based charges, without complex engineering work.


- Seamless integration with existing tools: Flipside needed something that integrated with Stripe and their existing stack, and Lago fit in smoothly.


- No need to build in-house: With Lago, Flipside's team could focus on their core product instead of building custom billing infrastructure.


- Automated invoice management: Lago handles all customer invoices, payment tracking, and overdue notices. Flipside doesn't have to chase payments.


## How Flipside uses Lago


Lago has helped Flipside make life easier on multiple fronts across their billing operations:


• No more billing headaches: Lago makes it easy to generate, track, and change invoices. Even for complex pricing and monetization models, Flipside doesn't need to divert engineering resources.


• Better visibility into revenue and usage: Lago's API helps Flipside feed into Snowflake and Omni to track business metrics without needing to rely on multiple dashboards.


• Flexibility for enterprise customers: Because Lago is highly customizable, it's still easy to be flexible with individual enterprise deals.


As Simon Spencer told me: "I had to go in, void an invoice, create a new one-time payment, and generate a payment link—all without involving anyone else. That was really nice,"


With Lago, Flipside can focus on what they do best—helping blockchains grow—without thinking about billing.
