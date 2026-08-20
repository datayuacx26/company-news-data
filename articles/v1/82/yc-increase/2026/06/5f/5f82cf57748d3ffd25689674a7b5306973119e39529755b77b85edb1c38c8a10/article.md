---
schema_version: "1.0.0"
document_id: "5f82cf57748d3ffd25689674a7b5306973119e39529755b77b85edb1c38c8a10"
company_key: "yc-increase"
company: "Increase"
source_id: "yc-increase-news-import-16282b6988d7"
canonical_url: "https://increase.com/articles/seeing-like-a-network"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-21T23:41:43.316828+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:4d0c2c8ea05224e004982c06380430dc3e28a4e9cabc47e44260521d00f87259"
---

# Seeing like a network: the merchant category code — Increase

Suppose you’re building the financial infrastructure for a logistics network. You need to issue physical corporate cards to truck drivers so they can pay for fuel and maintenance. Or maybe you’re engineering the payment flows for a delivery platform, issuing zero-balance cards to couriers to buy groceries for customers at the point of sale.


In both cases, you need to guarantee that each card buys what it was issued for, and nothing else.


At Increase, you enforce this programmatically via a synchronous webhook on[Real-Time Decisions](https://increase.com/documentation/real-time-decisions) .


When a courier swipes, dips, or taps their card, the payment terminal generates an ISO 8583` 0100` Authorization Request. This message traverses the card network on its way to us. We intercept it mid-flight and, while the terminal and network wait for an answer, send a real-time webhook to your server.


That thin webhook tells you an event happened and hands you an ID:


```text
{
"  type  "  :   "  event  "  ,
"  category  "  :   "  real_time_decision.card_authorization_requested  "  ,
"  associated_object_type  "  :   "  real_time_decision  "  ,
"  associated_object_id  "  :   "  real_time_decision_123abc  "
}
```


You fetch the decision itself with a` GET` to` /real_time_decisions/{id}` , which returns the authorization details:


```text
{
"  id  "  :   "  real_time_decision_123abc  "  ,
"  category  "  :   "  card_authorization_requested  "  ,
"  status  "  :   "  pending  "  ,
"  card_authorization  "  :   {
"  decision  "  :   null  ,
"  presentment_amount  "  :   14550  ,
"  presentment_currency  "  :   "  USD  "  ,
"  merchant_category_code  "  :   "  5411  "  ,
"  merchant_acceptor_id  "  :   "  1234567890  "  ,
"  merchant_descriptor  "  :   "  SAFEWAY STORE 0021  "
}
}
```


You have about four seconds to ingest this payload, evaluate it against your internal state machine, and` POST` your verdict back to` /real_time_decisions/{id}/action` :


```text
{
"  card_authorization  "  :   {
"  decision  "  :   "  approve  "
}
}
```


Once your action` POST` completes, you return a` 200 OK` to the original webhook, and the transaction resumes.


One of the highest-signal fields for evaluating the authorization request is` merchant_category_code` .


You don’t have to make every call live, either. Increase’s API lets you pre-configure static[authorization controls](https://increase.com/documentation/api/cards#card-object.authorization_controls) on a card, allow-listing or blocking specific merchant category codes outright, so you can reserve Real-Time Decisions for the cases that genuinely need a judgment call.


## Seeing like a network


In his book *Seeing Like a State* , James C. Scott explores the concept of “legibility.” A state (or any complex organization) can’t govern without first simplifying reality. To manage a dense, old-growth forest, for example, the state clears it and plants identical trees in straight rows. To calculate taxes, a 19th-century tariff regime declares that a tomato is a “vegetable,” not a fruit.


The global payment networks operate the same way. Governed by the International Organization for Standardization under ISO 18245, the Merchant Category Code (MCC) is Visa and Mastercard’s instrument of legibility.


Modeling a messy, contradictory world is an impossible task; no taxonomy ever fits perfectly, and some are simply better than others. The MCC is serviceable, but its quirks run deep.


Originally designed in the 1970s to calculate tiered interchange fees (a jewelry store presents a higher fraud risk than a bakery, and is priced accordingly), the MCC maps global commerce onto a rigid numeric grid, keyed to a business’s *primary consumer-facing activity* . But how do you classify a 200,000-square-foot mega-mart that sells tires, prescriptions, milk, and life insurance? You oversimplify. To the network, a merchant is whatever it sells most of. The grid compresses reality into a four-digit integer, producing outcomes by turns comical and genuinely inconvenient.


## Hardcoded history


The full ISO 18245 taxonomy reads like a fossil record of 20th-century commerce. The system encodes point-in-time business realities as permanent infrastructure.


Specific entities from the 1970s and 80s are hardcoded into the global grid. Major carriers get their own: United is` 3000` , American is` 3001` , each reserved exclusively for that airline. The networks maintain` 3667` purely to identify transactions at a single casino hotel: the Luxor in Las Vegas.


The grid is just as lopsided in the other direction. For every entity rich enough to earn its own code, whole swaths of commerce funnel through a single overloaded one. A corner café, a food truck, and a hotel banquet hall can all land under the same restaurant code, while a genuinely new kind of business picks among a half-dozen overlapping food-service options, none a clean fit. And the granularity is wildly uneven. The same grid that crams all of food service into a handful of codes keeps dedicated entries for furriers (` 5681` ), wig shops (` 5698` ), and snowmobile dealers (` 5598` ).


## When the abstraction leaks


The grid’s real trouble is that it can’t keep up. When new business models emerge, it buckles.


Authorizing a fleet card seems trivial: you allow-list MCC` 5542` (Automated Fuel Dispensers). But when electric vehicle charging networks scaled, they lacked a dedicated identifier. Some acquirers miscategorized EV chargers as generic utilities (` 4900` ). If your fleet transitioned to EVs, your programmatic webhooks started declining legitimate charges or approving ambiguous ones.


A specific code for EV charging (` 5552` ) was eventually introduced, but implementation remains fragmented. Updating an MCC isn’t a cloud deployment; it often requires the acquiring bank to reclassify the merchant in its core systems and manually push a configuration change to a physical point-of-sale terminal in a parking lot. For an independent station owner, there’s zero financial incentive to initiate this process just so a driver’s corporate card categorizes the spend correctly.


This kind of mismatch reveals a deeper limitation. MCCs classify the *entity* , not the *basket* . By default, the ISO 8583 payload can’t distinguish between a cart of groceries and a flat-screen television rung up at the same big-box store. The code tells you where the card was swiped, but nothing about what was actually bought.


Some transactions do carry richer, itemized data alongside the authorization, but it’s supplemental, inconsistently supported, and a subject of its own; the base authorization still identifies the merchant, not the purchase. When you’re writing authorization logic against the category code, you’re making decisions based on who the merchant says they are, not what the cardholder is doing.


## From entity to intent: ISO 20022


The MCC has been a durable piece of financial infrastructure for fifty years. But as global commerce shifts, its dominance is waning.


The structural threat isn’t a new set of codes; it’s the bypass of the card networks entirely. As Open Banking and direct account-to-account (A2A) payments scale via rails like the UK’s Faster Payments, Europe’s SEPA Instant, or the FedNow network, transactions no longer traverse Visa or Mastercard. They don’t use ISO 8583 or carry an MCC.


To prevent a data void in spend categorization, the global financial system is migrating to the **ISO 20022** messaging standard. This replaces legacy flat files with highly structured, extensible (if charmless) XML.


Imagine you’re building the financial flows for that same logistics network, but ten years from now. Instead of a driver swiping a physical corporate card at a gas station, the connected fleet vehicle initiates a real-time A2A transfer over FedNow to settle the charging fee directly with the EV station.


When your webhook fires, you aren’t checking a merchant’s category code. ISO 20022 carries a dedicated Purpose Code that defines the *intent* of the transfer. Inside a standard` pacs.008` customer credit transfer payload, you parse the` <Purp>` element:


```text
<  Purp  >
<  Cd  >  ENRG  </  Cd  >
</  Purp  >
```


Values like` ENRG` for energy,` AIRB` for air transport, or` SALA` for salary define intent natively in the payload. The classification moves from the *entity* (who the merchant is) to the *intent* (why the money is moving).


Note` AIRB` (and resist the autocomplete to AirBnB): it names the *purpose* of the payment, not the merchant behind it. Where the card networks split air travel across dedicated carrier codes like United’s` 3000` and a generic catch-all (` 4511` ), ISO 20022 simply carries a field that means “air travel.”


Migrations of core financial infrastructure are slow. The transition to ISO 20022 has been repeatedly delayed by central banks, and the volume of physical point-of-sale hardware ensures legacy mainframes will remain operational for decades. SWIFT held its November 2025 deadline to retire cross-border MT payment messages, but a long tail of message types and address formats runs into 2027. The transition will be measured in business cycles, not quarters.


Until that migration is complete, if you’re building programmatic spend controls, you’re living in the world of the MCC. That’s a more workable place than it sounds. The taxonomy is messy, but it’s also shared and public, so you can design for its edges deliberately instead of discovering them in production. Treat the category code as a strong hint rather than a verdict: keep your allow-lists in configuration, not code, so they change in minutes rather than a deploy; log the code and your decision on every authorization, so a miscategorized EV charger surfaces as a pattern instead of a support ticket; and fold in new codes like` 5552` as they come into use.


The highest-leverage move is making the taxonomy more legible to each other. The 50-year-old grid isn’t going anywhere soon, but the tooling we wrap around it can be modern, shared, and a good deal more legible than the grid itself.
