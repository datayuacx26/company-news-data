---
schema_version: "1.0.0"
document_id: "55ebce9c8f5fa19dfbe1bab5416daeb27597fa5467d36bc59fe81b1c1264e044"
company_key: "yc-suger"
company: "Suger"
source_id: "yc-suger-news-import-2505a48c7131"
canonical_url: "https://www.suger.io/resources/blog/tax-and-withholding-on-marketplace-revenue/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-15T09:12:25.375763+00:00"
fetched_at: "2026-08-15T09:12:26.807099+00:00"
content_hash: "sha256:c3fbe6769abea74ea9fad8880730484de2749971ac5ba4c0c443b13b7dd35bc8"
---

# Tax and Withholding on Marketplace Revenue

*On a cloud marketplace sale, the party responsible for calculating and remitting transaction tax is decided by the marketplace and varies by country. It is not a property of your company, and it is not consistent across the marketplaces you sell on.*


---


This post is not tax advice. It describes how the marketplaces structure the question, so that the people in your business who *do* give tax advice are looking at the right one.


The reason it matters operationally is simple: the amount that reaches your bank account is not the amount on the customer’s order. Between the two sit a service fee, tax on that fee, transaction tax that may or may not be yours, and withholding. Finance teams that model marketplace revenue as “list price minus a percentage” get a surprise on the first payout and a worse one at year end.


---


## **The three-way split nobody expects**


Microsoft documents the structure most explicitly, and its model is a good lens for the general problem. Every country a publisher sells into falls into[one of three categories](https://learn.microsoft.com/en-us/partner-center/marketplace-offers/tax-details-marketplace) :


Category Who handles transaction tax What it means for you


**Publisher-Managed** You do — entirely Registration, calculation, collection, remittance, validating customer business status, and issuing tax invoices are all yours


**Microsoft-Managed** Microsoft does Microsoft calculates, collects and remits, invoices under its own registration number, and includes the tax on its return


**Reseller** Microsoft does, as reseller Microsoft buys from you and resells; you owe tax on the sale *to Microsoft* , Microsoft owes it on the resale


The Microsoft-Managed list is long — it includes the United States, the United Kingdom, Australia, Canada, most of the EU, Japan’s neighbours across APAC, and dozens more. **Brazil is the only Reseller country/region.** Publisher-Managed is defined by subtraction: anywhere Microsoft supports marketplace sales that is not one of the other two.


Three countries behave differently between commercial and consumer marketplaces. India is Reseller on the commercial side and Microsoft-Managed on the consumer side. Mexico is Publisher-Managed except where the customer buys through a local Enterprise Agreement, in which case it is Reseller. China is not enabled for the commercial marketplace at all.


The operational consequence: **your tax obligation is a function of where your customer is, not where you are.** A single quarter’s sales can span all three categories, and your finance system needs to be able to tell them apart from the payout report.


---


## **What comes out before you get paid**


In every category, Microsoft deducts the Store Service Fee, any transaction taxes applicable to that fee, and any applicable withholding tax before paying you. In Microsoft-Managed and Reseller countries it also deducts the transaction taxes it collected as part of offer taxation.


Two things follow that finance teams routinely miss:


**Tax on the service fee is a separate line from tax on the sale.** Microsoft notes it might collect transaction tax on the Store Service Fee in Australia, Canada, Mexico, New Zealand and Singapore. That is tax on *its* fee to *you* , not tax on your sale to the customer — a different account, and often a recoverable input tax.


**Withholding is deducted at source.** Where withholding applies, it comes out before the payout. It is not lost — it is generally creditable — but only if somebody records it as withholding rather than as a smaller sale. A payout modelled as net revenue silently writes off a tax credit.


Microsoft is explicit that it makes no warranty that its actions fully satisfy a publisher’s obligations even in Microsoft-Managed countries, and recommends publishers work with their own tax advisors. That caveat is the important one: a marketplace handling remittance is not the same as a marketplace discharging your obligation.


---


## **The reconciliation problem this creates**


Marketplace payout reports are the only place these components are broken out, and they arrive on the marketplace’s schedule, in the marketplace’s format, per marketplace.


Practically, a finance team needs to be able to answer three questions per transaction:


1. **Was transaction tax collected, and by whom?** Determines whether the tax is your liability or already remitted.
2. **What was deducted from the payout, and under what heading?** Service fee, tax on service fee, transaction tax, withholding — four different accounts.
3. **Which of those are recoverable?** Input tax on a service fee and creditable withholding both need to be recorded as such at the time, not reconstructed at year end.


None of that is derivable from the order value. It comes out of the payout data, which is why[exporting marketplace data properly](https://www.suger.io/resources/blog/exporting-marketplace-data-to-your-warehouse/) matters more for tax than for almost anything else — and why netting adjustments at ingest destroys exactly the detail you need here.


Where the numbers diverge more generally, and the four usual causes, is covered in[Why Marketplace Numbers Never Match](https://www.suger.io/resources/blog/marketplace-revenue-reconciliation/) .


---


## **Selling into more countries than you have registrations for**


The listing flow makes it trivial to select many countries. The tax consequence of doing so is not trivial.


Every Publisher-Managed country you enable is a country where you have taken on registration, calculation, collection, remittance and invoicing — whether or not you have noticed. If you sell one deal there, the obligation exists.


The pragmatic pattern most teams settle on:


- Start with the countries the marketplace manages, where the tax handling comes with the platform.
- Add Publisher-Managed countries deliberately, one at a time, each with a decision from your tax advisors about registration.
- Treat “enable everywhere” as a decision with a compliance cost, not as a default.


That is a narrower launch than most GTM plans assume, and it is much easier than unwinding an unregistered obligation two years later.


---


## **Frequently asked questions**


**Who pays the sales tax on a cloud marketplace transaction?** It depends on the country. Microsoft classifies each country as Publisher-Managed, where the publisher handles all tax, Microsoft-Managed, where Microsoft calculates and remits, or Reseller, where Microsoft buys and resells.


**Which countries are Reseller countries for Microsoft Marketplace?** Brazil is the only Reseller country/region. India is Reseller for the commercial marketplace, and Mexico is Reseller where the customer buys through a local Enterprise Agreement.


**Does the marketplace handling tax discharge my obligation?** No. Microsoft states it makes no warranty that its actions completely satisfy publisher obligations, even in Microsoft-Managed countries, and recommends publishers work with their own tax advisors.


**What is deducted from a marketplace payout?** The store service fee, any transaction tax on that fee, any applicable withholding tax, and in Microsoft-Managed and Reseller countries the transaction tax collected from the customer.


**Why record withholding separately from revenue?** Withholding is deducted at source but is generally creditable. Booking the payout as a smaller sale rather than as revenue less withholding writes off a tax credit you were entitled to.


---


## **Takeaways**


- Who remits transaction tax is decided per country by the marketplace, not by your company’s location.
- Microsoft splits countries three ways; Brazil is the only Reseller region, and Publisher-Managed is everything not otherwise classified.
- Four different things come out of a payout — service fee, tax on that fee, transaction tax, withholding — and they belong in four different accounts.
- Withholding recorded as reduced revenue is a written-off tax credit.
- Enabling a Publisher-Managed country is taking on a registration and remittance obligation. Do it deliberately.


This post is a description of how marketplaces structure tax responsibility, not tax advice — decisions here belong with your own advisors. Suger keeps the payout detail those decisions need, broken out per marketplace and per transaction rather than netted.[See how Suger handles reporting](https://www.suger.io/platform/reporting/) , or[talk to our team](https://www.suger.io/contact-us/) .
