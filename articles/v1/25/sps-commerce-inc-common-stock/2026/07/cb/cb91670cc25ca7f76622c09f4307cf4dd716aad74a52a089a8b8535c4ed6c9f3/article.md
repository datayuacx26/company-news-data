---
schema_version: "1.0.0"
document_id: "cb91670cc25ca7f76622c09f4307cf4dd716aad74a52a089a8b8535c4ed6c9f3"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/how-to-document-tariff-charges-in-edi"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-23T00:37:49.401871+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:bc0d08107f7840407383e24a0e423739f5a36a89ee336dff2edfef8f412e8a75"
---

# How to Document Tariff Charges in EDI

# How to Document Tariff Charges in EDI


By[Peter Spaulding](https://www.spscommerce.com/community/profile/peter-spaulding) , *Sr. Content Writer*


Last Updated July 21, 2026


7 min read


In this article, learn about:


-


What’s driving the current urgency around tariff documentation


-


Five ways trading partners are documenting tariffs in EDI invoices today


-


How to choose the right method for your systems and your trading partners


-


How[SPS Commerce Fulfillment](https://www.spscommerce.com/products/fulfillment/edi/) automates tariff tracking across every EDI invoice


Two federal deadlines are colliding this month, and they are reshaping how quickly goods move through U.S. ports. The[temporary 10% Section 122 tariff surcharge](https://www.skadden.com/insights/publications/2026/05/us-trade-court-strikes-down-section-122-tariffs) sunsets on July 24, and a separate round of tariffs tied to forced labor enforcement could follow soon after. Suppliers shipping internationally are accelerating orders to avoid getting caught with cargo at sea when new tariffs take effect, and that speed only works if tariff charges show up cleanly on every invoice a trading partner receives.


That is where[Electronic Data Interchange (EDI)](https://www.spscommerce.com/what-is-edi/) documentation comes in. Undocumented or inconsistently documented tariffs create reconciliation problems between trading partners exactly when speed matters most. This guide walks through five ways suppliers are documenting tariff charges in EDI invoices today, plus how to choose the right one for your systems and your trading partners.


## Why Is Tariff Documentation Suddenly Urgent?


Import volumes have already responded to the deadline.[Hackett Associates founder Ben Hackett tied](https://nrf.com/media-center/press-releases/import-cargo-expected-to-set-new-record-ahead-of-potential-august-tariffs) a sharp rise in import volumes to businesses front-loading shipments ahead of expected tariff increases, with major U.S. ports handling their highest May volume in more than a year.


The Section 122 surcharge, a 10% tariff applied to most imports under the Trade Act of 1974, is scheduled to expire by operation of law at 12:01 a.m. EDT on July 24, 2026, 150 days after it took effect. Around the same time, the Office of the U.S. Trade Representative proposed additional duties of 10% to 12.5% on 60 economies following an investigation into forced labor enforcement, holding a public hearing on the proposal on July 7, 2026. No effective date has been set for that action yet, but the timing lines up closely enough with the Section 122 sunset that many importers are planning for both.


For suppliers, the practical effect is a compressed shipping window and a higher chance that tariff charges need to show up correctly on[an invoice](https://www.spscommerce.com/edi-document/edi-810-electronic-invoice/) under time pressure. A tariff charge that is not documented the way a trading partner’s EDI spec expects does not just create a billing question. It stalls the invoice, delays payment, and pulls both sides back into manual reconciliation right when neither side has time for it.


**Related Reading:**[Why Tariff Pressure Spikes Your Deduction Problem](https://www.spscommerce.com/community/articles/why-tariff-pressure-spikes-your-deduction-problem)


## What Are Your Options for Documenting Tariffs on an Invoice?


Every trading partner relationship needs one clear answer when tariff charges appear on an invoice. The methods below range from a separate line item both sides can see and audit (the most visible) to folding the charge into item costs (the least visible). None of them is universally correct. The right choice depends on the visibility your finance team needs and what your trading partner’s EDI spec already supports.


### Add Tariffs as a Separate Line Item


Adding tariffs as a distinct line item is the most straightforward way to give both sides clear visibility into the charge. This can happen in the summary tab of an invoice, coded as either a tax or an additional charge, or as a specific duty charge if the trading partner’s system supports it.


Some trading partners use a dedicated line-item code, for example a “T” designation, or an item-level charge code, to flag the tariff separately from the rest of the invoice. The exact code varies by trading partner, so double-check the convention before the first invoice goes out.


### Use the Charges or Allowances Field


SPS Commerce can accommodate tariffs within the charges or allowances field of an invoice. This approach works well, but it depends on clear agreement upfront: how the supplier is sending the tariff information, and how that information needs to convert for the trading partner’s system on the receiving end.


Settle this during the testing phase of the first invoice, not after go-live. A mismatch discovered after invoices are already flowing is far more expensive to fix than one caught in test.


### GL Account Tariff Tracking


Assigning tariffs to a dedicated General Ledger (GL) account number lets the system pull the tariff amount automatically whenever it posts under that account. This method is often the lowest-lift option since it does not require significant changes to existing systems, but it also gives less line-level visibility than a separate invoice line item.


### Fold Tariffs into Item Cost


Folding the tariff directly into the cost of the item is the simplest method to implement. It also gives up the most visibility, since the tariff amount is no longer broken out anywhere on the invoice. This works well when granular tariff tracking is not a priority for either side, but it becomes harder to unwind later if a trading partner asks for a tariff breakdown or if the tariff rate changes mid-contract.


This is far and away the most-used option. It is the most recent, real-world impact of tariffs on supply chain organizations.


## How Do You Choose the Right Method for Your Trading Partners?


There is no single method that fits every trading partner relationship. Weigh these four factors before you commit:


1.


**Visibility needs** : Does your finance team, or your trading partner’s, need to see the tariff amount broken out on its own, or is a rolled-up total sufficient?


2.


**System complexity and cost** : A separate line item or a charges and allowances field usually takes more setup than a GL account or item-cost approach.


3.


**Timing constraints** : If a trading partner relationship is new or a tariff deadline is close, the lower-lift options may be the more realistic near-term choice.


4.


**Partner-specific EDI spec requirements** : Some trading partners already have a documented preference. Check the spec before assuming you have a free choice.


Most suppliers use different methods for different trading partners rather than picking one method network-wide, since specs and finance requirements vary by relationship.


## How Do You Get EDI Specs and Communication Right?


Whichever method you choose, the[EDI spec](https://www.spscommerce.com/products/edi-testing/) has to reflect it accurately, and the trading partner needs to know what to expect before the first invoice goes through. SPS Commerce provides guidance on how to handle tariffs so the chosen method is documented correctly in the EDI spec.


Proactive communication matters here as much as the technical setup. Telling a trading partner how tariffs will appear on an invoice, before the first one arrives, prevents exactly the kind of confusion that tariff volatility is already creating across the supply chain.


## Frequently Asked Questions


### Can you combine tariffs with other charges on the same invoice?


Yes. Tariffs can sit alongside other charges and allowances on an invoice. The key is making sure your EDI spec distinguishes the tariff charge from other charges clearly enough that your trading partner’s system parses it correctly.


### Which method should a new trading partner relationship start with?


Start with whatever the trading partner’s EDI spec already documents. If the spec does not specify, the charges or allowances field is usually the easiest to test and double-check during onboarding, before committing to a separate line item or GL account setup.


### What happens if tariff documentation does not match a trading partner’s spec?


A mismatch typically stalls the invoice rather than rejecting it outright. That delay pushes back payment and pulls both sides into manual reconciliation, the exact outcome a clear EDI spec is meant to prevent.


### Does SPS Commerce Support All Five Methods?


SPS Commerce supports multiple approaches to documenting tariff charges, including the charges and allowances field, and provides guidance on aligning the EDI spec to whichever method a trading partner relationship uses.


**Related Reading:**[IEEPA Tariff Refunds and the Data Requirements Behind Them](https://www.spscommerce.com/community/articles/ieepa-tariff-refunds-and-the-data-requirements-behind-them)


## Track Your Tariffs on Every EDI Invoice


Tariff rates are shifting fast enough that manual reconciliation is not a sustainable plan. SPS Commerce Fulfillment automates the order-to-invoice cycle, including EDI invoicing, so tariff charges show up accurately and consistently across every trading partner, without a manual check on each invoice.


[Track your tariffs on EDI invoices with SPS Commerce Fulfillment](https://www.spscommerce.com/products/fulfillment/edi/) .


Not ready to change your EDI setup yet?[The Supply Chain Source](https://www.spscommerce.com/community/) has more guides on EDI compliance as the tariff landscape keeps shifting.
