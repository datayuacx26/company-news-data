---
schema_version: "1.0.0"
document_id: "d4d5074bb027aadec0382556f5251c57df91f03fc9ac9dd699fe2214546f4b53"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/azure-renewals-and-offer-transitions/"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T10:32:29.919238+00:00"
fetched_at: "2026-08-19T10:32:31.370146+00:00"
content_hash: "sha256:7896cdf42ef5e486d543073b084e7ed9f349320911fb4cda665b6916947f564c"
---

# Azure Marketplace Renewals and Offer Transitions

*An Azure Marketplace renewal is the continuation of a customer’s subscription when its current term ends. On Microsoft’s marketplace the interesting decisions are made months earlier, at offer-creation time: what the subscription renews into, whether it auto-renews at all, and whether the renewal is a fresh private offer or a fallback to the public plan.*


---


A direct renewal is a conversation you schedule. An Azure Marketplace renewal is a setting you chose at the last close, quietly executing on a date nobody put in the calendar.


That is the shift a deal desk has to absorb the first time a marketplace subscription comes up for renewal. There is no purchase order to countersign and no moment where the customer re-signs. The term ends, and the subscription lands somewhere — on your negotiated price, on your public list price, or nowhere — depending on how the original private offer was built.


This is the renewal-into-offer path on Azure Marketplace, and the decision points along it, described from Microsoft’s own publisher documentation.


---


## **How do renewals work on Azure Marketplace?**


An Azure Marketplace renewal is governed by two things the publisher sets when the private offer is created: the renewal target and the auto-renew default. Microsoft documents both on the private-offer setup page, not on a separate renewal screen — which is the first thing to internalise. The renewal is configured at the sale, not managed at the term end.


For SaaS private offers created with absolute pricing as a new plan, Microsoft states that “publishers define how the subscription renews when the current contract term ends. Customers can choose whether to turn on auto-renew during purchase; by default, auto-renew is off.”


Read that twice, because it inverts the assumption most direct-sales teams carry in. Auto-renew is **off unless the customer turns it on** . A subscription you assumed would roll can simply end, and the first signal you get is a subscription that stopped billing — not a renewal quote that went unanswered.


---


## **Renew into offer, or renew into public: the decision that matters**


The core decision on an Azure renewal is what the subscription renews *into* , and Microsoft gives you exactly two configured answers. You choose one at offer creation, and it decides whether next year’s price is the one you negotiated or the one on your public listing.


Microsoft’s two options, in its own words:


- **Renew into private offer.** “While the private offer remains active, the subscription renews using the private offer terms. After the private offer expires, future renewals use the selected public offer term.”
- **Renew into public offer.** “The subscription renews directly to the selected public offer term at the end of the current contract term. A public offer renewal term must always be selected.”


Two consequences fall out of this for a deal desk.


**A public offer term is mandatory either way.** Microsoft requires you to pick a public fallback: it “must always be selected” and “serves as the fallback renewal option when the private offer is no longer available.” So even a renew-into-offer subscription has a public list-price landing pad waiting behind it. If you never re-issue a private offer, the customer eventually renews at public pricing — quietly, at whatever your list price happens to be that year.


**A flexible billing schedule removes the offer-renewal option.** Microsoft is explicit: “Private offers that use a flexible billing schedule can’t renew into the existing private-offer term and instead renew using the selected public offer term.” If your deal used a flexible charge schedule, “renew into private offer” is off the table by construction. The subscription will fall back to the public term — plan for it before the customer discovers it.


---


## **What is the difference between a renewal and a new offer?**


A renewal is the automatic continuation of an existing subscription under previously chosen terms; a new offer is a fresh private offer you build and the customer must actively accept. On Azure Marketplace these are genuinely different mechanisms, and knowing which one you are running changes who has to act.


A renewal, in the “renew into private offer” sense, needs no new acceptance while the offer is live — the subscription simply continues on the negotiated terms. But an Azure private offer is not an open-ended instrument. It has a **customer private pricing end date** and, in Microsoft’s words, once the customer accepts it “you can no longer change the offer.” A submitted offer is immutable; the page tells you plainly that “after you submit a private offer, you can’t edit it.”


So when the terms need to change — a new price, a longer commitment, a different plan — you are not editing the renewal. You are creating the next offer. Microsoft’s supported shortcut for this is **clone** : “You can clone an existing offer and update its customer information… or, update the offer or plan pricing to send extra discounts to the same customer.” A re-negotiated renewal on Azure is, mechanically, a cloned-and-edited private offer that the customer accepts anew.


That distinction is the whole reason renewals belong on a deal desk’s forward calendar rather than a customer-success checklist. It is also the operational cousin of the[private offer versus private plan choice](https://www.suger.io/resources/blog/azure-private-offers-vs-private-plans/) — the construct you pick at the first sale shapes what the renewal can even be.


---


## **Where the transition decisions actually happen**


Every decision that shapes an Azure renewal is made at the private-offer setup stage, inside Partner Center, before the customer has accepted anything. There is no renewal wizard that surfaces later; the renewal is the downstream execution of choices already locked in.


Here is where each decision sits:


Decision Set when What it controls


Auto-renew default Offer creation; customer confirms at purchase Whether the subscription continues at all. Off by default.


Renew into private offer vs public offer Offer creation (SaaS absolute-price plans) Whether next term is negotiated price or public list price.


Public fallback term Offer creation — mandatory The list-price plan the subscription lands on when the private offer expires.


Renewal fee attestation Offer creation only Whether Microsoft applies the renewal store-fee discount. Cannot be added later.


Re-negotiated terms A new (cloned) offer, before term end Any price or term change — requires fresh customer acceptance.


The last row about the fee is worth its own note, because it is a benefit teams routinely forfeit by acting too late.


---


## **The renewal attestation you can only claim once**


Microsoft applies a reduced store fee to qualifying renewals, but only if the publisher declares the renewal status **at the moment the offer is created** . This is the one renewal decision with a hard, unrecoverable deadline.


Microsoft defines a qualifying customer renewal as at least one of: “renewal of a current private offer or paid marketplace deal,” renewal of an existing paid agreement “still in effect outside of the commercial marketplace that partners are migrating,” or “an upselling opportunity with an existing paid customer.” Qualifying renewals “receive a 50% discounted store fee.”


The catch is procedural, and it is absolute. “To receive the discount, you need to self-attest to the renewal status during private offer creation.” And: “Renewal self-attestation can only happen during private offer creation and can’t be done retroactively via Partner Center or support after offer creation.” Miss the checkbox, and there is no support ticket that recovers the discount — you would have to unwind the deal and re-issue it.


There is even a specific migration path in the docs: for deals enacted before a cutoff, “a customer will need to turn-off autorenewal or cancel their subscription and a new private offer set-up with the renewal attestation checked to be eligible for the fee discount.” That is a lot of moving parts to coordinate for a discount you could have claimed with one checkbox — which is exactly why the renewal belongs in the deal-desk workflow before the offer is ever submitted.


---


## **Why the renewal stays on the marketplace at all**


Enterprise customers keep renewals on Azure Marketplace for the same reason they bought there: marketplace spend can count against what they have already committed to Microsoft. Microsoft’s marketplace overview lists, among the benefits, helping customers “apply purchases toward their Azure consumption commitment.”


That is the gravity holding the renewal in place. A customer optimising their Microsoft Azure Consumption Commitment ([MACC](https://www.suger.io/resources/blog/what-is-a-macc-azure-committed-spend-explained/) ) has a strong reason to renew on the marketplace rather than move the contract off it — and a strong reason to notice if a lapsed auto-renew or a fumbled fallback quietly interrupts that drawdown. The renewal is not just your revenue continuing; it is their committed-spend plan continuing. Both break at the same term-end date if nobody is watching it.


---


## **Frequently asked questions**


**Does an Azure Marketplace subscription renew automatically?** Not by default. Microsoft states that for SaaS private offers, “customers can choose whether to turn on auto-renew during purchase; by default, auto-renew is off.” The publisher defines how it renews, but the customer decides whether it renews at all.


**What is the difference between “renew into private offer” and “renew into public offer”?** Renew into private offer continues the negotiated terms while the offer is active, then falls back to the public term. Renew into public offer sends the subscription straight to the public list-price term at the end of the current contract. A public term must be selected either way.


**Can I edit a private offer to change its renewal terms?** No. Once submitted, a private offer can’t be edited, and once the customer accepts it you can no longer change it. To alter renewal terms you clone the offer, edit the copy, and have the customer accept the new one.


**Is a renewal the same as a new offer on Azure Marketplace?** Mechanically, a re-negotiated renewal is a new (cloned) private offer the customer accepts anew. An unchanged renewal continues automatically under the terms set at the original sale, with no new acceptance while the offer is live.


**How do I get the renewal fee discount?** Self-attest to the renewal status during private offer creation. Microsoft applies a 50% discounted store fee to qualifying renewals, but the attestation “can’t be done retroactively” — it must be checked before the offer is submitted.


**What happens if a customer’s private offer expires and I haven’t sent a new one?** The subscription renews into the public offer term you selected as the mandatory fallback — meaning the customer moves to your public list price rather than negotiated pricing. Track offer end dates so a fallback is never a surprise.


---


## **Takeaways**


- The Azure renewal is configured at the sale, not managed at the term end. Every meaningful choice sits on the private-offer setup page.
- Auto-renew is off by default. Confirm the customer turned it on, or the subscription can simply end.
- “Renew into private offer” holds your negotiated price only while the offer is active; a public fallback term is mandatory and is where the subscription lands when the offer expires.
- A flexible billing schedule forces the public fallback — “renew into private offer” is unavailable for those deals.
- A re-negotiated renewal is a cloned, re-accepted private offer, not an edit. Submitted offers are immutable.
- The renewal fee discount must be attested at offer creation and can never be added retroactively.


---


Azure renewals reward the teams that treat them as forward-dated deals rather than end-of-term paperwork. A marketplace-native platform gives a deal desk one place to track private-offer end dates, renewal targets, and fallbacks across every cloud they sell on — see the[Suger platform](https://www.suger.io/platform/) and the[Azure Marketplace seller solution](https://www.suger.io/solutions/microsoft-marketplace/) for how that fits an ISV’s Microsoft motion.
