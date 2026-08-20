---
schema_version: "1.0.0"
document_id: "063550295615ae21add955d4e9dd502a2079265fd94dca12db66b7262b11c551"
company_key: "yc-paycaddy"
company: "PayCaddy"
source_id: "yc-paycaddy-news-import-4989b9f70079"
canonical_url: "https://paycaddy.com/en/blog/integrate-card-product-latam.html"
published_at: null
first_seen_at: "2026-07-25T18:55:15.733062+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:12a77b78ce142bc920022250ce08ef4975d863be2519e2c9c839f8f3d6ff2258"
---

# How to Integrate a Card Product in Latin America Easily

This guide addresses companies seeking to issue cards without navigating bureaucratic processes independently. We recommend using a single provider covering the entire process rather than managing banking partners and card brands separately.


## Determine Your Tech Expertise


The first consideration involves assessing whether your company already provides digital experiences to card users and whether you have an in-house tech team. PayCaddy offers solutions for both technical and non-technical companies.


### For Non-Technical Companies (Express Solution)


- Define the product specifications, target user understanding, required features, and operational management flow
- Choose design and printing methods, including card materials and stock management (factory-made inventory or OnDemand printing)
- Brand the whitelabel app
- Access the Client Backoffice


### For Technical Companies (Bespoke Solution)


For companies building Bespoke projects requiring greater control over cardholder UX/UI and product granularity, the RESTful API accommodates simple flows, though technical expertise becomes necessary.


## Integrate Essential Features


All integration work occurs in QA environment first before production launch coordination.


### Core Entities Requiring Interaction


- Users
- Wallets
- Cards


### Creating a Card


A user must be activated through KYC verification before card creation. PayCaddy's integrated solution provides verification links, with webhook notifications confirming verification results. Alternatively, delegated verification workflows can be discussed with the compliance team.


Once activated, card creation requires sending a request with the userId, walletId, and product code specifying card type (Physical or Virtual).


### Managing a Card


**Physical card operations:**


- Acknowledge Reception endpoint allows cardholders to confirm delivery and enable first-time activation


**Self-management features:**


- Check PAN to view card number and expiration date
- Check CVV to view security code
- Check PIN to view personal identification number
- Unblock PIN after three incorrect attempts
- Change PIN for custom four-digit selection
- Block card for temporary deactivation
- Unblock card to reinstate activity


The platform provides Cardholder App Front End Guidelines to ensure intuitive feature integration.


### Transactions


Transaction notifications arrive via webhook with 11 data attributes identifying cards, describing transactions, and showing wallet balance effects. This information should be stored for easy retrieval and filtering within the cardholder UX/UI.


### Funding Wallets


Integration requires Pay In and Transfer endpoints for introducing funds into wallets.


**Pay In function:**


- Declares amount and target wallet currency
- Makes money instantly available
- Registers PayIns for daily settlement operations


**Transfer function:**


- Moves funds between wallets
- Useful for admin panel implementation
- Requires matching currencies between sending and receiving wallets
- Works between wallets of same or different users


## Conclusion


Upon completing these integration steps, companies successfully launch card products in Latin America. Further preparation involves reviewing resources in PayCaddy's Newsroom regarding program management and successful card issuing in the region.
