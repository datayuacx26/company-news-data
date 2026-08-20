---
schema_version: "1.0.0"
document_id: "c2fae5e48455a9d453ccd5343bfb3ea826ed90181b4d084ea4fdbe04e45a3fc1"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/how-fastly-skyfire-enable-trusted-agentic-commerce-at-the-edge/"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:bf5cb186380569a6ca98c5b3b9df851cb89a38af04d26d690ceb6045081909f4"
---

# How Fastly and Skyfire Enable Trusted Agentic Commerce at the Edge

What happens when non-human users become legitimate customers?


AI systems are generating a growing share of traffic across Fastly’s network, and increasingly, some of those systems are acting on behalf of real users. They can retrieve information, compare options, check availability, answer questions, and complete tasks autonomously.


And the trend is accelerating. From January 2026 through May 2026, Fastly observed AI requests[grew approximately 30% – about 6.5 times faster than human traffic](https://www.fastly.com/blog/ai-traffic-grew-6-5x-faster-than-human-traffic-this-year) during the same period. Across our platform, autonomous machine-to-machine traffic, including crawlers, bots, agents, API-driven systems, and other machine-initiated interactions, is approaching half of all internet requests.


The infrastructure underneath the legacy internet wasn’t designed for this. For years, automated traffic was treated as hostile by default. Bots meant scraping, fraud, credential stuffing, inventory hoarding, or abuse. Security systems evolved around blocking non-human behavior by default because most of the time, that behavior was malicious.


Now the line is getting harder to draw. An AI agent comparing hotel prices or restocking supplies on behalf of a customer represents a real user. It may carry payment credentials and be authorized to complete transactions autonomously. But when it arrives at a website, it often looks identical to the same automated traffic companies have spent years trying to filter out.


Businesses need better ways to decide which automation deserves access.


Fastly and[Skyfire](http://skyfire.xyz/) are empowering businesses to do just that: to verify which AI agents are legitimate before granting them access to services, content, or transactions.


*Watch how Fastly and Skyfire Enable Trusted Agentic Commerce at the Edge*


## How Skyfire Helps Businesses Identify and Trust AI Agents


Skyfire is an agentic commerce infrastructure that gives AI agents and the platforms that host them two things they currently lack: verified identity and the ability to pay.


On the identity side, Skyfire issues Know Your Agent (KYA) credentials (tokenized identity that travels with the agent as a standard JSON Web Token). A KYA token tells a merchant or service provider who the agent is, which human or enterprise it represents, and whether it has been verified through Skyfire's registration process. The design borrows from established compliance frameworks like Know Your Customer (KYC) and Know Your Business (KYB), adapted for a class of actors that don't have driver's licenses or incorporation documents.


On the payment side, Skyfire provides wallets that support both stablecoins and tokenized credit cards, allowing agents to complete transactions in real time. An agent carrying Skyfire credentials can check out through a standard e-commerce flow, pay for API access on a per-call basis, or settle a micropayment without requiring a human to pull out a credit card at the moment of purchase.


For businesses on the receiving end, Skyfire provides the verification layer that lets them distinguish a funded, accountable agent from anonymous automation. Publishers, merchants, and data providers can accept agent traffic through their existing web infrastructure, security controls, and authentication layers – no re-platforming required.


## How Fastly Enables Trusted Agent Traffic


AI agents often operate inside tight execution windows. A shopping agent checking inventory across retailers, for example, may need to evaluate multiple systems simultaneously before inventory changes or pricing expires. Introducing additional round trips to centralized systems adds latency that compounds rapidly at scale.


[Fastly’s platform data shows](https://www.fastly.com/blog/bot-defense-table-stakes-machine-traffic-requires-business-strategy) that AI traffic is significantly more likely than human traffic to require fresh data from origin infrastructure, reinforcing the need for scalable enforcement closer to the edge.


As agentic traffic grows, origin-centric enforcement models become increasingly expensive and difficult to scale. That’s where edge infrastructure becomes critical.


Fastly’s security products inspect over 8 trillion requests per month and protect 130,000+ apps and APIs. Skyfire has integrated their identity and payment-backed credentials into Fastly’s programmable edge cloud platform, enabling agent identity verification and payment validation to occur at Fastly's globally distributed points of presence before a request ever reaches the origin infrastructure.


The integration supports several enforcement models:


-


A merchant can allow verified agents to access product catalogs and complete purchases while rate-limiting unverified agent traffic.


-


A publisher can grant full content access to agents with valid payment credentials while redirecting uncredentialed agents to a paywall.


-


A data provider can apply tiered pricing based on the volume and frequency of agent requests, with enforcement handled automatically at the edge.


In each case, the business retains full control over its policies. Skyfire provides the identity and payment signals. Fastly provides the enforcement environment. The business decides what to do with both.


## How to Implement Skyfire on Fastly


Skyfire's KYA protocol is built on standard web infrastructure such as JWTs, OAuth2, and JWKS, which means it fits natively into the HTTP request lifecycle that Fastly's platform already manages: no proprietary SDKs, no middleware layer, no separate API gateway.


For businesses already using[Fastly Compute](https://www.fastly.com/products/edge-compute) , enabling Skyfire-verified agent traffic is a configuration change rather than an engineering project. Skyfire has developed a reference implementation that demonstrates how to use KYA tokens and Fastly Compute to verify agent identity and control access to protected content and APIs at the edge. The implementation is available as an open reference implementation for Fastly customers interested in enabling trusted agent traffic.


[Visit the Skyfire Fastly Compute demo repository](https://github.com/skyfire-xyz/skyfire-solutions-fastly-compute-news-crawler-demo) and follow the instructions in the README file.


The guide walks you through creating a Skyfire API key, onboarding your Buyer and Seller accounts, deploying the Fastly Compute application, and testing requests with and without KYA tokens.
