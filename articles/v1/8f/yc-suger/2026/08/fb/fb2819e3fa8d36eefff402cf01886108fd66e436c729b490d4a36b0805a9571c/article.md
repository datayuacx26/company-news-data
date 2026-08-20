---
schema_version: "1.0.0"
document_id: "fb2819e3fa8d36eefff402cf01886108fd66e436c729b490d4a36b0805a9571c"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/per-market-pricing-on-azure-marketplace/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-20T00:55:49.760493+00:00"
fetched_at: "2026-08-20T00:55:51.412607+00:00"
content_hash: "sha256:5e10e744f5e7127b513b680dbd2c96772547815fab13b1dfab9cda89a69ced69"
---

# Per-Market Pricing on Azure Marketplace, Explained

*Per-market pricing on Azure Marketplace is not a grid of independent country prices. It is one base price — set in USD or uploaded per currency — mapped onto Microsoft’s fixed table of which currency each of 141 geographies transacts in. What you actually control is the base and the market list; the currency a buyer is shown is assigned, not chosen.*


---


A deal desk pricing an Azure offer for a European buyer usually starts from the wrong mental model: that Azure Marketplace works like a pricing table where you type a number for Germany, another for France, another for Japan. It does not, and the gap between that expectation and the mechanic is where publish-day surprises come from.


The real mechanic is narrower and, once you see it, simpler to reason about. This is the annotated checklist of what per-market pricing on Azure actually asks you to decide — and which of those decisions you cannot take back.


---


## How Azure Marketplace multi-currency pricing actually works


Azure Marketplace multi-currency pricing is a currency *conversion* model, not a per-country price-entry model. You set one price in USD and Microsoft converts it into each local currency, or you upload local-currency prices directly — but the currency each geography uses is fixed by Microsoft, not selected by you.


Per Microsoft’s own documentation, offers can be purchased in[141 geographies, each with an assigned currency](https://learn.microsoft.com/en-us/partner-center/marketplace-offers/marketplace-geo-availability-currencies) , defined by the customer’s billing address. That table is the load-bearing fact: Germany transacts in EUR, the United Kingdom in GBP, Japan in JPY, Australia in AUD, and the long tail of markets in USD. You do not pick which currency a German buyer sees — the market map does.


Two consequences follow immediately, and both matter to a deal desk:


- **A “market” is a billing geography, not a price band.** Enabling Germany does not create a Germany price you tune in isolation; it opts that geography’s assigned currency into your plan.
- **A buyer is shown the price in one currency, and it is theirs.** Microsoft’s docs are explicit: a buyer views the price in their tenant currency, or their billing-account currency if they bought under a specific agreement — and *“you cannot pick a different currency to view prices.”*


So the European-buyer question — “what will this cost in euros?” — is answered by your base price and one conversion step, not by a euro figure you typed.


---


## The two ways to set the numbers


There are exactly two ways to establish local prices, and the choice between them is the first decision to settle. You either enter a single USD price and let Microsoft convert it, or you upload a price in each local currency yourself.


**Option A — enter USD, let it convert.** You type a USD price on the Pricing and Availability page in Partner Center. Microsoft converts it to each customer’s static local currency using the exchange rate *at the time you first save the price for the plan.* This is the fast path, and the phrase “at first save” is the one to internalize — the conversion happens once, at save, and then freezes.


**Option B — upload local prices.** Microsoft lets you export a pricing spreadsheet, review the converted price in every market, adjust any of them, and import the sheet back. The documented flow is three steps:


1. Export the pricing spreadsheet.
2. Review the prices in each market.
3. Import the spreadsheet back into Partner Center.


Option B is what you reach for when a converted price lands somewhere untidy — a figure that reads as odd in a local storefront, or a market where you want a deliberately different number. It is the only mechanism that lets a per-market price differ from a straight conversion of your USD base.


This post keeps to the mechanics and deliberately states no exchange rates and no target margins — those move, and they are yours to set. What is fixed is the *shape* of the control: one base, or one upload per currency, converted once and then held static.


---


## ”Static” is the word that catches people


Once a plan is saved, every local-currency price is static — it does not move when exchange rates move. This is a feature (predictability for the customer) and a trap (your carefully-converted price drifts from your intent as FX shifts), and the trap is the more expensive of the two to discover late.


Microsoft states it plainly: after you create and save a plan, the prices in all local currencies are static and do not update automatically. If you want a stale local price to change, you have a defined set of levers — and no others:


Lever What it does When to use it


**Stop selling in a market** Removes the offer from that geography The local price is no longer viable and you would rather withdraw than re-price


**Re-import local prices** Export, adjust the specific markets, import, republish You want to correct one or a few currencies deliberately


**Bump the USD base, republish** Re-converts every local price at the current rate You want a fresh conversion across the board


**Private Offer** Sets the exact local price a *specific* customer pays One buyer, one negotiated number, outside the public plan


The[Private Offer](https://www.suger.io/resources/blog/azure-private-offers-vs-private-plans/) row is the one deal desks reach for most, because it operates at the level a deal desk actually works: a single customer and a single agreed number, without touching the public plan every other buyer sees.


---


## The two edits you cannot take back cheaply


Some pricing decisions on Azure are reversible with a republish; two are not, and they belong at the top of the checklist because getting them wrong is expensive.


**The pricing model is immutable after publish.** Suger’s[Azure Marketplace pricing-model documentation](https://doc.suger.io/azure-marketplace/pricing-model) is direct about this: you choose one model per offer — flat rate (a single monthly or annual price, the only model that also supports optional usage-based metering) or per-user (seat-based, with a minimum and maximum seat count) — and *after the offer is published, you cannot change the pricing model.* Every plan in the offer must share it. Choosing flat-rate versus per-user is therefore a pre-publish decision with no undo, per market or otherwise.


**A price increase takes at least 90 days to reach customers.** Microsoft’s documentation states it takes at least 90 days for price increases to become effective for existing customers. So “raise the euro price” is not a same-day action — it is a change you schedule and then wait out, which is the general shape covered in[changing your marketplace price after launch](https://www.suger.io/resources/blog/change-marketplace-price-after-launch/) .


There is also a category of plan you simply cannot re-price: per Microsoft, if an offer has hidden plans or plans targeting government clouds, you cannot update prices at all — the only option is to stop selling that plan in those markets and create a new plan with the new prices.


---


## Who handles tax, and why it is not your line to price


You do not price the tax into your per-market number, because in most Azure markets Microsoft is handling it. Microsoft’s overview describes selling across dozens of *Microsoft-managed markets* where taxation is simplified on your behalf — the platform is transacting with the customer and managing the tax treatment for that geography.


The practical rule for a deal desk: set your price as your price, and treat the customer-facing tax line as Microsoft’s mechanism, not a component you build into the local figure. Where withholding and payout tax *do* land on you is a separate, downstream question we cover in[tax and withholding on marketplace revenue](https://www.suger.io/resources/blog/tax-and-withholding-on-marketplace-revenue/) — but that is about what reaches your bank, not about what a buyer sees at checkout.


---


## The annotated checklist: decide these before you publish


Settle each of these before you save a plan, because the first four are hard or slow to change afterward.


1. **Pricing model — flat-rate or per-user?** Immutable after publish. Shared by every plan in the offer. Decide first.
2. **USD base, or per-currency upload?** Option A is fast and converts once at save. Option B (Export/Import) is the only way to make a market’s price differ from a straight conversion.
3. **Which markets?** Enabling a geography opts in *its* assigned currency, not a price band you tune. Confirm each target market’s currency against Microsoft’s table so nothing transacts in USD by accident when you expected a local currency.
4. **Have you reviewed every converted local price before publish?** Export the sheet and read the number a buyer will actually see in each market. This is your last cheap chance to change any of them.
5. **What is your re-pricing plan when a static price drifts?** Pick your lever ahead of time — re-import, USD bump, stop-selling, or Private Offers for individual deals — so a stale euro price is a planned change, not a scramble.
6. **Are any target plans hidden or government-cloud?** Those cannot be re-priced. If a market matters and might need a price change later, do not bury it in a plan type you cannot edit.


For the full listing motion this sits inside — eligibility, offer types, Partner Center, and payouts — start from the[how to sell on Azure Marketplace ISV guide](https://www.suger.io/resources/blog/how-to-sell-on-azure-marketplace-isv-guide/) .


---


## Frequently asked questions


**Can I set a different price for each country on Azure Marketplace?** Not as free-form per-country prices. You set one USD base that Microsoft converts, or you upload a price per currency via the pricing spreadsheet. Uploading is the only way a market’s price can differ from a straight conversion.


**How does currency get chosen for an Azure Marketplace buyer?** It is assigned by billing geography, not chosen. Microsoft maps each of 141 geographies to a fixed currency, and a buyer is shown the price in their tenant or agreement currency. You cannot pick a different currency for them to view.


**Do Azure Marketplace local prices update when exchange rates move?** No. Once you save a plan, every local-currency price is static. To change one you stop selling in that market, re-import an adjusted local price and republish, bump the USD base to re-convert, or use a Private Offer for a specific customer.


**How long does an Azure Marketplace price increase take to apply?** At least 90 days for existing customers, per Microsoft. A price increase is a scheduled change you wait out, not a same-day edit, so plan re-pricing well ahead of when you need it live.


**Can I change the Azure Marketplace pricing model after publishing?** No. You choose flat-rate or per-user once per offer, every plan shares it, and it is immutable after publish. Decide the model before you publish, because there is no per-market or after-the-fact override.


---


## Takeaways


- Per-market pricing on Azure is one base price plus Microsoft’s fixed currency-per-market map, not a grid of independent country prices.
- Currency is assigned by billing geography across 141 geographies; a buyer views their own currency and cannot switch it.
- Set numbers one of two ways: a USD base that converts once at save, or a per-currency upload via Export/Import — the latter is the only way a market’s price differs from a straight conversion.
- Local prices are static after save. Know your re-pricing lever — re-import, USD bump, stop-selling, or a Private Offer — before you need it.
- Two decisions have no cheap undo: the pricing model is immutable after publish, and a price increase takes at least 90 days to reach existing customers.


Suger configures Azure Marketplace plans, markets, and pricing models — and keeps them in sync with your co-sell and billing — from one place across every marketplace it supports.[See the Azure Marketplace seller solution](https://www.suger.io/solutions/microsoft-marketplace/) , read the[Azure Marketplace pricing-model docs](https://doc.suger.io/azure-marketplace/pricing-model) , or[talk to our team](https://www.suger.io/contact-us/) .
