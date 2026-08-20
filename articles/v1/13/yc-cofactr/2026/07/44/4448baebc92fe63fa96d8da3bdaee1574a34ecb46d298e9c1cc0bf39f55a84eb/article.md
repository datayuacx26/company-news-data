---
schema_version: "1.0.0"
document_id: "4448baebc92fe63fa96d8da3bdaee1574a34ecb46d298e9c1cc0bf39f55a84eb"
company_key: "yc-cofactr"
company: "Cofactr"
source_id: "yc-cofactr-news-import-06c16db4e4eb"
canonical_url: "https://www.cofactr.com/articles/definitive-guide-to-semiconductor-tariffs"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T21:33:44.258496+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:010dcb39609075bc67f92b7d4c4298a8f5616055e1a10565ba7e8d4319f68883"
---

# Definitive Guide to Semiconductor Tariffs

##


Tariffs on semiconductors have ranged from 10% to 195%. Seems like a pretty good reason to panic. There are four tariffs that have applied to semiconductors; Section 301, IEEPA, Reciprocal, and Section 122. We’ll break them down one-by-one, but when you consider how few Chinese components leave China as components, the real impact is a lot smaller than the headlines suggest.


**U.S. Chip Imports By Value**


Source: Semiconductor Industry Association and U.S. Census Bureau


## \[Pre-2025\] Section 301: The OG of Tariffs


Originally applied by President Trump during his first term as a 25% tariff, the most recent update to Section 301 tariffs came from the Office of the U.S. Trade Representative under President Biden on September 18, 2024 ([Federal Register No. 2024-21217](https://www.federalregister.gov/documents/2024/09/18/2024-21217/notice-of-modification-chinas-acts-policies-and-practices-related-to-technology-transfer) ).


The order doubled existing tariffs on key semiconductor categories from 25% to 50%, effective January 1, 2025. These tariffs apply to China and Hong Kong but not to other countries, which continue under MFN (most-favored nation) rules:


- **China and Hong Kong** : 50%
- **Mexico & Canada** : 0% No Section 301 tariff
- **All Other Countries** : 0% No Section 301 tariff


Based on current industry classifications and customs data, all[HTS codes](https://hts.usitc.gov/) we have observed for semiconductors, PCBs, and PCB assemblies sourced from China are subject to the Section 301 tariffs. This includes HTS codes in Chapters 85 and 90 most relevant to electronic components:


- **8541.x / 8542.x:** Semiconductors (now at 50%)
- **8534.00.0020 / 0040:** Two- and four-layer rigid PCBs excluded until May 31, 2025
- **8534.00.00:** Other bare PCBs—25% duty


Assemblies (like PCBAs) inherit their tariff treatment from the HTS code of the final product. So a Chinese component assembled into a PCBA in Thailand is NOT subject to China tariffs because the country of origin for the PCBA will be Thailand.


You can assume any Chinese PCBAs you import as PCBAs are tariffed (unless specifically excluded).


## \[NEW 2/1/25\] IEEPA Tariffs, aka 'Fentanyl Tariffs'


Under Executive Order 14195, signed February 1, 2025, and published February 7, 2025 ([Federal Register No. 2025-02408](https://www.federalregister.gov/d/2025-02408) ) the U.S. imposed tariffs using emergency powers under the International Emergency Economic Powers Act (IEEPA).


These tariffs took effect February 4, 2025, and apply to all goods from China (including Hong Kong), Mexico, and Canada. The original 10% duty on Chinese goods was increased to 20% via Executive Order 14228 on March 3, 2025 ([Federal Register No. 2025-03775](https://www.federalregister.gov/d/2025-03775) ):


- **China and Hong Kong: 20%**
- **Mexico and Canada: 0%** The 25% tariff was suspended as of April 2025 pending further negotiations with both countries. Imports from Mexico and Canada are currently not subject to the IEEPA duties, though the legal framework remains active. This suspension applies only to[goods with no tariff](https://ustr.gov/sites/default/files/files/agreements/FTA/USMCA/Text/US_Tariff_Schedule.pdf) under the[USMCA trade agreement](https://ustr.gov/trade-agreements/free-trade-agreements/united-states-mexico-canada-agreement/agreement-between) .
- **All Other Countries: 0%** Not included in Fentanyl Tariffs


These are blanket tariffs, and for China they are in addition to existing Section 301 tariffs. No HTS codes are exempt. Even goods eligible for free trade under USMCA are covered as currently written (though likely to be changed).


From our review, all HTS codes relevant to electronic components are included in the USMCA and therefore excluded for now with respect to Mexico and Canada.


**Note:** Canadian energy exports are an exception and remain at 10%.


## \[NEW 4/2/25\] Reciprocal Tariffs: Matching Foreign Barriers Globally


On April 2, 2025, the White House issued Executive Order 14257 creating the Reciprocal Tariffs Policy. This action, published April 7, 2025 ([Federal Register No. 2025-06063](https://www.federalregister.gov/d/2025-06063) ), established a new 10% base duty on nearly all imports from all countries except Mexico and Canada.


While country-specific surcharges were announced for 57 trading partners, **as of May 2025, all country-specific rates are suspended except for China and Hong Kong** . Imports from all other countries except Mexico and Canada currently face only the baseline 10% duty.


The China baseline rate was later increased to 125% under Executive Order 14266, signed April 9, 2025, and published April 15, 2025 ([Federal Register No. 2025-06462](https://www.federalregister.gov/d/2025-06462) ).


As of May 2025, this elevated rate applies only to China and Hong Kong, while the 10% rate remains in effect for other countries except Mexico and Canada:


- China and Hong Kong: 125%
- Mexico and Canada: 0%
- All Other Countries: 10%


Reciprocal tariffs are *in addition to* any applicable Section 301 or IEEPA tariffs.


Annex II to Executive Order 14257 outlines specific product exclusions from the Reciprocal Tariffs.


Notably,[semiconductors](https://www.whitehouse.gov/presidential-actions/2025/04/clarification-of-exceptions-under-executive-order-14257-of-april-2-2025-as-amended/) (HTS 8541 and 8542), semiconductor manufacturing equipment, and certain end-use consumer electronics like smartphones and laptops are exempt from the reciprocal tariffs.


**See Annex II Exclusion List** :[Annex II PDF](https://www.whitehouse.gov/wp-content/uploads/2025/04/Annex-II.pdf)


While semiconductors were exempted, passive components like capacitors and resistors are not.


**China Tariffs Summary Table**


This table summarizes current effective tariffs on Chinese-origin electronic components as of May 2025. Rates reflect formal U.S. trade actions under Section 301, IEEPA, and the Reciprocal Tariff Policy.


‍


## \[NEW 2/20/26\] Supreme Court Strikes Down IEEPA and Reciprocal Tariffs


On February 20, 2026 the U.S. Supreme Court decided[Learning Resources v. Trump](https://www.supremecourt.gov/opinions/25pdf/24-1287_4gcj.pdf) , ruling the International Emergency Economic Powers Act (IEEPA) does not grant the President power to impose tariffs. This ruling invalidates the IEEPA/Fentanyl Tariff and the Reciprocal Tariff.


Consequently, on February 20, 2026 the White House issued[Executive Order 14389](https://www.whitehouse.gov/presidential-actions/2026/02/ending-certain-tariff-actions/) Ending Certain Tariff Actions, which was published in the Federal Register on February 25, 2026 ([Federal Register No. 2026-03832](https://www.federalregister.gov/documents/2026/02/25/2026-03832/ending-certain-tariff-actions) ).


Also on February 20, 2026, the White House issued[Proclamation 10112](https://www.whitehouse.gov/presidential-actions/2026/02/imposing-a-temporary-import-surcharge-to-address-fundamental-international-payments-problems/) , “Imposing a Temporary Import Surcharge To Address Fundamental International Payments Problems.”


This action, published in the Federal Register on February 25, 2026 ([Federal Register No. 2026-03824](https://www.federalregister.gov/documents/2026/02/25/2026-03824/imposing-a-temporary-import-surcharge-to-address-fundamental-international-payments-problems) ), uses Section 122 of the Trade Act to impose a blanket 10% on all countries for all goods except those covered by the[USMCA trade agreement](https://ustr.gov/trade-agreements/free-trade-agreements/united-states-mexico-canada-agreement/agreement-between) and those goods excluded in Annexes I and II.


For our purposes, only[Annex II](https://www.whitehouse.gov/wp-content/uploads/2026/02/2026Section122.prc_.ANNEX2_.Final_.pdf) applies, and just like prior tariffs it excludes the primary semiconductor HTS codes beginning with 8541 and 8542. Passive components are subject to Section 122 tariffs. The current tariffs are:


- **China and Hong Kong** : 50% Section 301 on all goods
- **China and Hong Kong:** 10% Section 122 on non-exempt goods
- **Mexico and Canada:** 0%
- **All Other Countries** : 10% Section 122 on non-exempt goods


And the biggest unanswered question is whether companies will get refunds for tariffs already paid. The Supreme Court was silent on this question.


## \[NEW 3/04/26\] US Court of International Trade (CIT) Orders Refunds


On March 4, 2026 the CIT[issued an order](https://www.courthousenews.com/wp-content/uploads/2026/03/judge-eaton-court-of-international-trade-ieepa-liquidation-process-ruling.pdf) in Atmus Filtration, Inc. v. United States directing Customs and Border Patrol (CBP) to refund all tariffs ruled unlawful by the Supreme Court in the Learning Resources case.


The government is expected to appeal.


It should be noted that only the party that paid the tariff to the CBP at the border is legally entitled to refunds. For example, if a distributor charged you a tariff, they are legally entitled to any tariff they paid directly to CBP, but you are not legally entitled to a refund from the distributor.


But you should certainly ask!


## \[NEW 3/12/26\] US Court of International Trade (CIT) Stays Refund Order


On March 12, 2026 the CIT effectively[stayed its refund order](https://filehost.thompsonhine.com/uploads/CIT_Order_-_March_12_4292.pdf) issued on March 4, 2026 pursuant to the[Declaration of Brandon Lord](https://filehost.thompsonhine.com/uploads/CIT_Order_-_March_12_4292.pdf) , Executive Director, CBP, that the CBP was making substantive progress in implementing refunds.


## \[NEW 3/20/26\] US Court of International Trade (CIT) Stays Refund Order


On March 20, 2026 the CIT[continued staying](https://filehost.thompsonhine.com/uploads/March_20_2026_Order_b1ef.pdf) the refund order issued March 4, 2026 based on satisfactory progress by CBP and ordered a status update on March 31, 2026.


## \[NEW 4/27/26\] CBP Launches Tariff Refund Portal


On April 20, 2026, Customs and Border Patrol opened the[Consolidited Administration and Processing of Entries (CAPE](https://www.cbp.gov/trade/programs-administration/trade-remedies/ieepa-duty-refunds) ) system. The platform will handle refunds for $166 billion in tariffs imposed on 53 million shipments from 330,000 importers.


In the first week 13.3 million import entries cleared initial review, 15% were denied for entry specific validations, and 1.74 million entries are approved for refunds. The first refunds are expected to be issued by electronic payment around May 11, 2026.


Learn more about to submit a refund request[here](https://ktslaw.com/en/insights/alert/2026/4/want%20a%20tariff%20refund%20your%20essential%20checklist%20for%20the%20cape%20portal%20launch%20this%20monday) .


## \[NEW 7/24/26 \] Forced Labor Tariffs; new Section 301 tariff


On July 24, 2026, tariffs imposed on February 25, 2026 under Section 122 expired and were replaced by tariffs imposed by[Presidential Memorandum](https://www.whitehouse.gov/presidential-actions/2026/07/actions-by-the-united-states-in-the-investigations-under-section-301-of-the-trade-act-of-1974-of-the-acts-policies-and-practices-of-60-economies-related-to-the-failure-of-each-economy-to-impose-and/) under Section 301 related to forced labor, imposing 10-12.5% tariffs on 60 countries for all goods except goods excluded in the[Annex](https://www.whitehouse.gov/wp-content/uploads/2026/07/ANNEX.pdf) .


The Annex excludes the primary semiconductor HTS codes beginning with 8541 and 8542, PCBAs (8517), and Inductors (8504).


PCBs (8534), Capacitors (8532), and Resistors (8533) are subject to the new Forced Labor tariffs.


The current tariffs are:


- **China and Hong Kong** : 50% Section 301 on all goods
- **China and Hong Kong:** 12.5% Section 301 (new) on non-exempt goods
- **Mexico and Canada: 1** 0% Section 301 (new) on non-exempt goods


**Countries with 10% Section 301 Forced Labor Tariff**


Argentina, Bangladesh, Cambodia, Canada, Ecuador, El Salvador, Guatemala, Honduras, India, Indonesia, Jordan, Malaysia, Mexico, Pakistan, Sri Lanka, the United Kingdom, and Trinidad and Tobago


**Most Favored Nation (MFN) Countries with complex offsets**


EU, Taiwan, Japan, Korea, Switzerland


Effectively, all other countries are subject to 12.5% Section 301 Forced Labor Tariff.


## Who Pays and When


Tariffs are paid by the U.S. importer (think distributors like DigiKey and Mouser, or EMS companies) based on the country of origin and HTS code. Customs collects at the time of entry. In-transit goods before an EO’s effective date are exempt. See our[Expert Guide to Understanding Tariffs](https://blog.cofactr.com/articles/expert-guide-to-understanding-tariffs-on-electronic-components-what-you-really-need-to-know) for more on the basics.


Don’t get overcharged for new tariffs. Goods *already in the U.S.* are not subject to new tariffs. It takes weeks for a product to get shipped from China to the U.S. so anyone charging you tariffs right after implementation is probably over-charging.


**Pro Tip:** Tariffs are assessed on the distributor's *cost* , not the price you pay. A component with a 70% tariff that you pay $10.00 for should not cost you an additional $7.00. Estimate the tariff by assuming the[distributor's cost](https://blog.cofactr.com/articles/heres-the-trick-to-electronic-component-pricing) is 80% of online prices and use this cost to calculate reasonable tariffs.


Also watch for distributors or EMS companies wanting to add margin to the tariff. There’s nothing technically wrong with that, but most companies won’t do it to valued customers.


## The Real Impact on U.S. Manufacturing


The loudest voices say tariffs wreck hardware margins. Reality is more nuanced.


**Semiconductors**


- Only 3% of U.S. chip imports by value came from China in 2024
- Malaysia, Taiwan, Vietnam, and Thailand made up 60%+, these countries are subject to 10% Section 122 tariff.
- For most OEMs manufacturing PCBAs in the U.S., tariffs barely touch total product cost.


**Passive Components**


- 5–10% of a typical BOM cost, lower for high value BOMs
- Many passives come from China
- Most can be cross-referenced to non-China sources.
- A 195% tariff on a 1¢ part? That part is now about 3¢ per unit. Weigh the cost of mitigation against just paying the tariff.


**PCBs**


- These hurt more, and substantially all PCBs come from China
- A $3 board costs $7.35 with a 145% tariff
- That’s why buyers are qualifying shops in Taiwan, India, Thailand


## Where to Find HTS Codes and Country of Origin Data


[Arrow](https://www.arrow.com/) and[Avnet](https://www.avnet.com/apac/) both publish Country of Origin. DigiKey and Mouser do not.


[X-Refs.com](https://www.x-refs.com/index.html) provides a free tariff lookup tool for COO, HTS, and tariff rate. Silicon Expert, Accuris, and Z2 Data provide COO and HTS as part of data subscriptions.


[Cofactr](https://www.cofactr.com/) shows the reported Country of Origin and HTS code for every part, estimates tariff costs, and then shows the finalized tariff cost in the price of purchase, so you know what the exact cost will be.


Data aggregators like[Octopart](https://octopart.com/) and[Findchips](https://www.findchips.com/) do not typically provide country of origin. This is not their fault, it’s because the distributors are not providing it to them.


‍


## Final Word: Who Should Panic?


Tariffs matter—but not as much as some headlines imply. Most of the real cost pressure still comes from design choices, supply shortages, and vendor MOQs—not duties.


If you are currently sourcing PCBAs in the U.S. you should not see much impact, but watch out for your PCBs.


However, if you’re sourcing PCBAs, sub-assemblies, or final product through China it’s may be time to look elsewhere. Just don’t get spooked by a 195% number that only applies to a $0.01 part.


‍


**Want to make this easy?**[Schedule a free, no obligation Cofactr demo](https://www.cofactr.com/articles/definitive-guide-to-semiconductor-tariffs#contact) **to see how we can help you automate price evaluation, component swaps, and much more.**


‍


## Frequently Asked Questions (FAQ)


**What are the main types of tariffs affecting semiconductors?**
There are three major tariffs: Section 301, IEEPA tariffs, and Reciprocal tariffs. Each applies differently depending on product type and country of origin, with some overlapping effects.


**How do Section 301 tariffs impact semiconductor imports?**
As of January 1, 2025, Section 301 tariffs doubled to 50% for semiconductors and related products from China and Hong Kong. Other countries, including Mexico and Canada, are not subject to these tariffs.


**Why do tariff percentages look so high in headlines?**
While tariffs can reach up to 195% for some Chinese components, the real-world impact is lower because only a small percentage of U.S. semiconductor imports come from China, and some categories are exempt.


**Can I avoid tariffs by sourcing from assembly hubs like Thailand or Vietnam?**
Yes. If a Chinese component is assembled into a PCBA in another country, such as Thailand, the tariff is applied based on the final country of origin, not the source of the individual component.


**When did the new IEEPA tariffs take effect, and what do they cover?**
The IEEPA tariffs began on February 4, 2025, starting at 10% on Chinese goods and increasing to 20% by March. They apply to nearly all imports from China and Hong Kong, but Mexico and Canada are currently exempt.


**Are semiconductors exempt from Reciprocal Tariffs?**
Yes. While Reciprocal Tariffs impose a 10% base duty on most imports and a 125% rate for China and Hong Kong, semiconductors (HTS 8541 and 8542) are excluded, along with semiconductor manufacturing equipment and certain consumer electronics.


**Who is responsible for paying tariffs and when?**
Tariffs are paid by the U.S. importer, such as distributors or EMS companies, at the time of customs entry. Goods already in transit before a tariff’s effective date are not subject to the new rate.


**What is the real cost impact of tariffs on U.S. manufacturers?**
The effect is smaller than headlines suggest. For example, only 3% of U.S. chip imports came from China in 2024, while most imports came from countries facing much lower tariffs. The biggest pain points are PCBs, which are heavily China-dependent.


**Where can I find HTS codes and country of origin information for components?**
Resources include Arrow and Avnet (publish COO), X-Refs.com (free tariff lookup), and subscription services like Silicon Expert, Accuris, and Z2 Data. Cofactr also shows tariff costs at purchase.


**Is it necessary for every company to panic about semiconductor tariffs?**
Not really. Most tariffs add relatively little to overall product cost. The largest impacts are on PCBs and low-cost passive components, while semiconductors themselves are less affected. Businesses sourcing primarily from U.S. or non-China regions will feel minimal impact.


‍
