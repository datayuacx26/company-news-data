---
schema_version: "1.0.0"
document_id: "19bbaca0b37ce7708582b626846783e9496ea541b3bee046255208bead845cb6"
company_key: "yc-alpaca"
company: "Alpaca"
source_id: "yc-alpaca-rss-4f5e7678c8d4"
canonical_url: "https://alpaca.markets/blog/alpaca-launches-business-accounts-for-broker-partners/"
published_at: "2026-08-18T13:00:13+00:00"
first_seen_at: "2026-08-18T14:08:17.275847+00:00"
fetched_at: "2026-08-18T14:08:19.768650+00:00"
content_hash: "sha256:7d49ea6b8177ac553a9ff98444d4531ef36ef0b9b32bd36c38865266b7f6f09e"
---

# Alpaca Launches Business Accounts for Broker Partners

We're excited to announce that Business Accounts are now available for our broker partners through[Alpaca’s Broker API](https://alpaca.markets/broker?ref=alpaca.markets) . Eligible partners can now onboard legal entities, including LLCs and S-Corps, and authorized signers through a structured, API-native workflow, designed to streamline onboarding and support business account trading.


## Why We're Expanding Business Account Support


Many broker-dealers are seeing increased demand for business account onboarding. However, onboarding business accounts has traditionally relied on manual document collection, fragmented compliance workflows, and support-intensive reviews that don't scale.1 These operational challenges create friction for customers, increase costs for brokerages, and leave significant room for modern, automated onboarding solutions.


Alpaca’s Business Accounts streamlines the onboarding process through a fully structured, API-native workflow. Partners can now submit and manage multiple party types, including legal entities, and authorized signers, through the API, with KYB tied to the entity and KYC to each individual. The result is a single, automated workflow that supports programmatic onboarding of business accounts through a single workflow.


## The Benefits of Broker API Business Accounts


### Minimal Integration


Our Business Account solution is built on the same Broker API infrastructure, so you can leverage the same integration, tech stack, and systems you already use and maintain.


### Automated Onboarding and Lifecycle Management


Business onboarding is designed to support a streamlined workflow similar to individual account onboarding, with fewer manual document processes. Partners can submit required data through the API, submit verifications, and manage the full lifecycle from account opening through ongoing updates through a single integration.


Business Account documentation and onboarding workflows will also be available through[Alpaca's MCP Servers for Broker API](https://alpaca.markets/blog/alpaca-launches-mcp-servers-in-public-beta-for-broker-partners/) , enabling engineering and operations teams to explore endpoints and test workflows without digging through documentation manually.


### Scalable by Design


As partner volumes grow, Business Account workflows scale without requiring operations support for each account, meaning onboarding can be faster, continually updated, and managed programmatically at any volume.


## How Business Accounts Work with Alpaca's Broker API


Alpaca uses a party-centric model, enabling entities to be submitted as a data object through the API with explicit fields for legal entity type, ownership percentages, UBO relationships, and authorized signer roles.


The onboarding follows an API-first workflow:


1. Submit and update the legal entity and authorized signers as structured data
2. Post KYB for the legal entity and KYC for each individual through the API
3. Create the legal parties and then pass the legal entity party ID to the primary_account_holder_id, and per-account agreements with signer_party_id
4. Any changes to business and party records are managed through the same API, with key fields automatically triggering an account review flow


Trade confirmations, monthly statements, and tax reporting forms are generated automatically in the legal entity's name, so partners do not need to build separate document handling workflows.For more information on our Business Trading Accounts, please read our[Broker API Docs](https://docs.alpaca.markets/us/docs/business-trading-accounts?ref=alpaca.markets) .


## What's Next


Business Accounts are a critical step in Alpaca's product roadmap to support institutional trading. As we expand our support, we will continue to build out features and enhancements in collaboration with our partners.


"The complexity of onboarding business accounts has been an ongoing barrier in the industry. Paperwork, compliance requirements, and manual systems created a real barrier between brokers and their users,” said Hakan Bayraksan, Senior Director of Product at Alpaca. “We’ve engineered an API-first system that reflects how entities and compliance teams think about ownership and verification, so that our partners can expand their addressable market without rebuilding their stack. With this launch, we’ve removed a key structural barrier.”


## Eligibility


Business Accounts are available to partners who have been approved for KYB and KYC reliance. Partners not yet approved will need to complete this approval process before enabling business account onboarding.


## Get Started


For prospective partners, please reach out to Alpaca's Sales team at[\[email protected\]](https://alpaca.markets/cdn-cgi/l/email-protection#cebdafa2abbd8eafa2beafadafe0a3afbca5abbabd) or fill out the form below.


For existing partners who are looking to integrate Business Accounts or would like to run KYB and KYC reliance, please contact your Customer Success Manager or email[\[email protected\]](https://alpaca.markets/cdn-cgi/l/email-protection) .


## **Contact Alpaca**


---


References:


1. [From KYC to KYB – Know Your Business](https://www.garp.org/risk-intelligence/culture-governance/from-kyc-to-kyb-260327?ref=alpaca.markets)


The content of this blog is for general informational purposes only. All examples are for illustrative purposes only.


This is not an offer, solicitation of an offer, or advice to buy or sell securities or cryptocurrencies or open a brokerage account or cryptocurrency account in any jurisdiction where Alpaca Securities or Alpaca Crypto, respectively, are not registered or licensed, as applicable.
