---
schema_version: "1.0.0"
document_id: "424530c3cf141c92e73d759aaf64ee78c3f35d603c4d3f596f743c7b3a182aea"
company_key: "yc-serif-health"
company: "Serif Health"
source_id: "yc-serif-health-news-import-98e88b19e297"
canonical_url: "https://www.serifhealth.com/blog/artificial-intelligence-enabled-medical-devices"
published_at: "2026-04-08T02:55:00+00:00"
first_seen_at: "2026-07-24T01:12:39.675945+00:00"
fetched_at: "2026-07-28T22:15:58.980681+00:00"
content_hash: "sha256:b835737eb3886a3600f011646f9c75659d659b75a501162dafbd7f2601a355f8"
---

# AI-Enabled Medical Devices: Clinical Artificial Intelligence Arrives

Artificial intelligence (AI) in healthcare is often discussed as a product story, or a venture capital story, or even a regulatory story. It’s the plenary session topic of nearly every recent healthcare conference. And for innovative manufacturers marketing new AI-enabled medical devices, it’s increasingly a coverage and reimbursement story, too!


Some of the clearest signals of real-world adoption of clinical AI don’t come from[press releases](https://www.calcalistech.com/ctechnews/article/hjjrzo1pbx) or the latest[product demos](https://claude.com/solutions/healthcare) . Instead, with the[FDA](https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-enabled-medical-devices) having approved 1,430 unique devices in this category so far, adoption of AI-enabled medical devices can now be readily estimated from real world data sources like price transparency and claims data. These new tools now touch a surprisingly broad set of billable clinical services, from retinal screening and coronary analysis to liver or breast imaging, ECG-based measurements, automated insulin dosing, and other procedure supports.


‘AI-enablement’ will span a wide range of diagnostic and interventional services across multiple specialties; so far[radiology and other imaging services](https://www.nature.com/articles/s41746-025-01800-1) have been key applications. Rather than trying to measure all clinical use, much of which is still embedded inside broader workflows or not separately billable, this blog focuses on the subset that has become visible through specific CPT billing pathways in commercial price transparency and claims data. We select 21 procedure groups spanning 48 CPT codes, anchoring on a prior[NEJM study](https://ai.nejm.org/doi/10.1056/AIoa2300030) and more recent AMA[guidance](https://www.ama-assn.org/practice-management/cpt/cpt-appendix-s-ai-taxonomy-medical-services-procedures) on AI-enabled medical device billing.


As of March 2026, each of the 21 AI-focused code groups appear as covered, in-network services across large numbers of US payer networks and provider groups in Serif’s[payer data catalogue](https://www.serifhealth.com/inventory/payers) . Many AI tools are not separately billable, do not map cleanly to a unique CPT or HCPCS pathway, or[remain embedded](https://arxiv.org/abs/2603.17234v1) inside broader service lines. But for the subset that are individually reimbursed, price transparency offers an unusually practical, real-time way to move beyond broad claims about adoption and ask a more grounded question:


**Where is AI-enabled reimbursement already visible in commercial healthcare delivery today, and what does that tell us about the early shape of clinical adoption?**


In this analytical blog, the third in our new, ongoing *TiC Takes* series, we utilize Serif Health’s comprehensive national Transparency-in-Coverage (TiC) data from March 2026, exploring first payer coverage and in-network providers. We then consider adoption and reimbursement trends over time, combining Serif’s historical price transparency data with national claims data to explore both reimbursement and procedure volume growth.


## Broad In-Network Coverage for AI-Enabled Medical Devices


Using Serif Health’s normalized Transparency in Coverage (TIC) data, we can begin to examine a defined set of AI-enabled medical device and related billing codes. The goal is straightforward: to understand where AI-related reimbursement is already concrete, how broadly those codes appear across payer networks and provider organizations, and how much pricing varies across this emerging code set.


First, we look at how broadly these AI-enabled codes appear across in-network payer networks and provider groups in March 2026, with bubble size reflecting the approximate median negotiated rate amount.


Negotiated rates for AI-enabled devices show up across a meaningful share of the commercial market,[based on Serif’s commercial payer data inventory](https://www.serifhealth.com/inventory/payers) . As of March 2026, breadth of coverage varies materially across the basket of code groups, with some of these services already widely contracted and others less so.


A majority of the 21 total code groups appear in over one hundred payer networks with tens of thousands of in-network provider organizations; some code groups are visible across more than 150k distinct provider groups while others span between 150 to 300 payer networks. Arguably the widest adopted CPT code, in terms of both unique payer networks and unique provider groups, is CPT code 92229, an ophthalmology service reflecting autonomous AI retinal imaging supporting earlier diagnoses of diabetic retinopathy by non-specialists.


The annotated bubbles introduce a second important point: prices for these services show significant variance. Even among codes that appear broadly across payer networks and provider groups, negotiated rates differ substantially across different payers and providers. That pattern is what we would expect in a market that is growing quickly but still being initially priced. The result is a category with both broad in-network visibility and meaningful rate dispersion for the same underlying types of services.


## Reimbursement for AI-Enabled MedTech Varies Wildly


A next practical question is what these services are actually reimbursed at once they show up as an in-network service. The answer is: it varies a lot! Across the 21 code groups considered, median in-network institutional reimbursement runs from roughly the low tens of dollars to just over $1,000 per billed service, while upper-percentile negotiated rates for some services reach into the thousands of dollars. In many cases, the 75th and 90th percentiles sit well above the median, a clear sign that reimbursement for the same billed AI-enabled service codes are far from standardized. As also shown in the prior figure, the three code groups with highest median in-network rates reflect specialized, AI clinical services in cardiology (e.g., coronary artery disease, echocardiogram measurement) and radiology/hepatology (e.g., liver MRI).


The dispersion in reimbursement for the same codes is especially notable because most of the code groups highlighted here function as discrete procedure, analysis, or interpretation services rather than other CPT codes billed over many units. For example: 75580 is a one-time,[augmentative coronary FFR-CT analysis](https://www.ama-assn.org/practice-management/cpt/cpt-code-set-facilitates-development-useful-ai-powered-devices) code while 0932T is an[augmentative echocardiogram measurement](https://www.ama-assn.org/press-center/ama-press-releases/ama-releases-cpt-2025-code-set) code just added for 2025. The spread reflects materially different negotiated payment levels for largely identical billable services, often reflecting the same underlying clinical task.


For institutional billing class pricing, reimbursement variation across AI-enabled device procedures is wide and often highly skewed at the upper tail. While some services (like the codes for coronary atherosclerosis) remain tightly clustered at low rate amounts, others, including multiorgan MRI, quantitative CT tissue characterization, liver MR, image-guided prostate biopsy, coronary artery disease, and quantitative MR cholangiopancreatography, show materially higher 75th and 90th percentile rates than their medians.


The epidural infusion procedure (code 0777T) features a particularly high upper distribution of institutional rates, with 75th and 90th percentile around $3,500 and $7,600, respectively, relative to a median rate of only $37! This code is[billed only](https://www.milestonescientific.com/wp-content/uploads/2023/09/CompuFlo-Billing-_-Coding-Guide.pdf) as an add-on service for a primary epidural injection procedure code (i.e., HCPCS 62320-62327); this add-on feature, as well as the device’s recent 2024 launch and[market access strategies by the manufacturer](https://www.snsinsider.com/blogs/milestone-scientific-launches-compuflo-advisor-program) , help explain the dramatic variation observed. Restricting to three national Aetna, Blue Cross Blue Shield, and Cigna PPO networks reporting institutional rates for 0777T shifts the distribution out further, to a median of $3,068, 75th percentile of $5,685, and 90th percentile of $11,127.


Professional billing class rates are more compressed around the median value, but still shows meaningful dispersion across payers and providers, especially for liver MRI, coronary artery disease, echocardiogram measurement, and quantitative MR cholangiopancreatography, where upper-percentile rates rise well above median levels. The broader takeaway from these results is straightforward: while commercial coverage for AI-enabled medical devices is real, pricing is still being worked out in real time.


## Trends in Utilization and Coverage


Finally, let’s turn from the cross-sectional view from March 2026 to assess recent trends using available historical data from 2024 - 2026. Let’s consider two complementary data sources: a third-party, national sample of retrospective claims and Serif Health’s archive of monthly-released payer rates, limited here to four representative national PPO networks with continuous reporting. This combination gives us a useful read on both billed utilization and in-network coverage. Claims show where these services are actually being billed and reimbursed, while payer price transparency data shows the consistent breadth of in-network providers for covered services.


Taken together, the first three trend lines above point in the same direction: use is clearly growing. In our claims sample, unique patients receiving any of the consider AI-enabled services rose from 6.3k in January 2024 to almost 17k by December 2025, while distinct billing NPIs increased from 827 to a peak of 1,161 unique providers in October 2025. Total allowed amounts summed across all claims followed the same pattern, increasing from $1.6 million in January 2024 to a high of $6.3 million in October 2025 (implying hundred of millions of dollars when extrapolated annually and out-of-sample for all claims). The claims data show more patients are receiving these services, more clinicians are billing them, and more reimbursed dollars are flowing through the category.


At the same time, the market does not look supply constrained. Both median allowed amounts in claims and median in-network rates (limiting here to professional rates only) move around month to month, but are very stable compared to total volume and total dollars over time: despite the entry of some new CPT codes into the mix over time. Across the four BUCA PPO networks, the number of in-network provider organizations associated with these codes rises from 142,260 EINs in January 2024 to more than 234,000 by March 2026, with the largest increase coming from a single payer network[(BCBS BlueCard)](https://www.serifhealth.com/inventory/payers/blue-card/networks/blue-card-usa) first including any rates associated with our AI CPT code basket in March 2025.


The realized billing footprint we observe in the claims data remains much smaller than the billable provider footprint visible in the rates data. By late 2025, the claims sample shows roughly 1,000 billing NPIs per month, while TiC suggests that well over 200,000 in-network provider organizations are already eligible to bill and be reimbursed for these services. This suggests the reimbursement infrastructure is already largely in place across a broad commercial provider base, even though realized billing remains much more concentrated today among early adopters. In our TiC data, we can observe top health systems like CommonSpirit Health, Banner Health System, HonorHealth, and many other large national healthcare organizations with in-network rates for all 21 of the AI-enabled devices we consider. Turning back to the claims data shows Stanford Health Care, Nebraska Methodist Hospital, Baptist Healthcare System, and William Beaumont Hospital as the top billing health systems reimbursed for AI-enabled medical devices: each organization received more than $3 million (i.e., total allowed amounts) in 2025 for the AI-enabled device code groups reviewed. Review of both sources suggests that adoption is being by driven by[a mix](https://healthit.gov/data/data-briefs/hospital-trends-use-evaluation-and-governance-predictive-ai-2023-2024/#:~:text=health%20IT%20characteristics.-,Findings,by%20hospital%20characteristics%2C%202023%2D2024) of both health systems and independent hospitals, as well as large[radiology](https://www.aidoc.com/learn/blog/rsna-2023-radiology-ai/) and[cardiology](https://cardiovascularbusiness.com/topics/artificial-intelligence/exploring-evolution-ai-cardiology) specialty groups, in specific service lines with traditionally higher costs.


## AI, Everywhere


A clear takeaway from this analysis is that billing pathways for AI-enabled clinical services are now mainstream. Across both price transparency and claims data, our results provide additional evidence that coverage is already broad enough to identify in-network providers and pricing, with ore and more provider organizations are showing up with negotiated codes over time. More billing clinicians are using them. And in claims, the same basket is showing increasing real-world billing activity. At the same time, reimbursement is anything but settled. For the same underlying services, negotiated rates can vary materially across networks and provider organizations. That combination matters. Adoption is real, but the commercial market is still being priced, shaped, and contested in real time.


That is consistent with the broader direction of travel. The FDA’s[public list](https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-enabled-medical-devices) of authorized AI-enabled medical devices continues to expand and is intended to give providers, patients, and innovators visibility into the evolving device landscape, while outside tracking shows new authorizations continuing to accumulate across a widening set of use cases and specialties. At the policy level, the[White House’s July 2025 AI Action Plan](https://www.whitehouse.gov/releases/2025/07/white-house-unveils-americas-ai-action-plan/) explicitly calls for reducing barriers to innovation and deployment, reinforcing that this category is not moving into a neutral environment. It is moving into one with strong commercial and policy tailwinds.


But faster commercialization does not eliminate the need for trust, with physician responses to clinical AI remaining constructive but measured. In the[AMA’s most recent 2026 survey](https://www.ama-assn.org/system/files/physician-ai-sentiment-report.pdf) , more than 75% of physicians said these tools can improve their ability to care for patients, yet 40% said they feel both excited and concerned, with privacy and the patient-physician relationship among the leading issues. In their own marketing materials,[device manufacturer groups](https://www.advamed.org/wp-content/uploads/2024/02/AI-Myths-vs.-Facts.pdf) have communicated about their products as tools that extend clinical capacity and expertise, rather than replacing healthcare providers directly. However, with an entire class of AI-enabled devices CPT codes categorized as[‘autonomous’](https://www.ama-assn.org/system/files/cpt-appendix-s.pdf) , these debates are not going away.


Our data provide a grounded, nationally comprehensive view into the adoption of AI-enabled medical devices, allowing key stakeholders to better understand where the market stands today. Whatever one’s views about the pace or promise of this category, clinical AI services and associated medical devices are being deployed across clinical settings and showing up in contracts, claims, and competitive reimbursement negotiations.


**About Serif Health**


At Serif Health, our focus is to turn every transparency disclosure into reliable, comparable reimbursement intelligence so teams can make decisions with confidence. We’ll continue to publish insights on[our blog](https://www.serifhealth.com/blog) and highlight how payer rates data shows up in[live MRFs](https://www.serifhealth.com/inventory/) via our payer inventory.


If you’re interested in applying price transparency data, we’d love to connect. Please reach out tohello@serifhealth.com or schedule a[demo](http://calendly.com/serif-health/30min) . Also feel free to check out our[sample data](https://app.serifhealth.com/sample/data) on our web platform[Signal](https://www.serifhealth.com/products/signal) .


## **Technical notes**


**Included AI-Enabled Device Codes**


- Code selection primarily referenced a[New England Journal of Medicine (2023) study](https://ai.nejm.org/doi/10.1056/AIoa2300030) that was among the first reporting a clinical AI procedure code framework, including which groups of related CPT codes and how they reflect underlying AI-enabled clinical procedures and specific products. This list was supplemented with newer AMA-recognized AI codes from a[2025 document](https://www.ama-assn.org/practice-management/cpt/cpt-appendix-s-ai-taxonomy-medical-services-procedures) to capture additional coding falling into AI categories (i.e., assistive, augmentative, and autonomous billing pathways.
- The final basket is intended to reflect the billable, code-visible subset of AI-enabled medical devices and services, not the full universe of clinical AI.


**Payer Price Transparency Data**


- Primary analyses use Serif Health’s normalized Transparency in Coverage, payer-reported in-network rates data from March 2026. Key fields used include billing code, network year-month, payer, network name, in-network rate, billing class, EIN, and claims-filtered NPI lists.
- For the March 2026 snapshot, we use distinct EINs to summarize provider-group footprint, distinct networks to summarize payer visibility, and negotiated dollar amounts to summarize reimbursement.
- For trend analysis, we track the same code basket back to January 2024, limiting to four national payer networks with high quality historical data: UnitedHealthcare Choice Plus, Aetna Open Access Managed Choice, BlueCard, and Cigna National OAP.
- We report reimbursement in dollar terms and did not require an available Medicare benchmark payment, unlike in many Serif Health analyses. Given that some of these codes are relatively new, some do not have a stable Medicare benchmark, and for this category a more practical question is whether a commercial reimbursement market is forming and how widely reimbursement varies.


**Claims Data**


- Claims analysis uses the same code basket to evaluate real-world billing activity over time, sourcing from a national third-part claims database licensed by Serif.
- Key fields used include procedure code, claim service date, billing NPI, claim ID, and allowed amount. Claim start date was used to assign the claim month across the time series. Main claims measures include distinct billing NPIs, distinct claims, and median paid amount by month.


**Limitations**


- As of March 2026, each of the 21 AI-focused code groups appear as covered, in-network services across large numbers of US payer networks and provider groups in Serif’s[payer data catalogue](https://www.serifhealth.com/inventory/payers) . At the same time, the market is far from uniform. Some codes now appear across hundreds of payer networks and tens of thousands of provider organizations, while for other code groups, adoption remains more limited. Provider reimbursement for AI-enabled medical devices can vary substantially, both across different codes as well for the same codes but different payers and providers; a reminder that reimbursement policies for AI-enabled medical devices are continuing to evolve.
- Our approach does not capture every important AI application in healthcare. Many AI tools are not separately billable, do not map cleanly to a unique CPT or HCPCS pathway, or remain embedded inside broader service lines.
- This blog presents a billing-code view of AI adoption for select medical device codes.Not every AI tool has a dedicated CPT or HCPCS pathway, and some AI-enabled services remain embedded within broader existing codes. The analysis is therefore best interpreted as a view of the reimbursable, code-visible subset of AI-enabled medical devices and associated clinical services.
