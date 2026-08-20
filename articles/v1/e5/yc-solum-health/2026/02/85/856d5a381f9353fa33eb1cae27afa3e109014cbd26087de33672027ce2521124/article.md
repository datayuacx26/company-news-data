---
schema_version: "1.0.0"
document_id: "856d5a381f9353fa33eb1cae27afa3e109014cbd26087de33672027ce2521124"
company_key: "yc-solum-health"
company: "Solum Health"
source_id: "yc-solum-health-news-import-cf3e7d0d0486"
canonical_url: "https://getsolum.com/blog/medicaid-provider-spending-data-healthcare-automation-gap"
published_at: "2026-02-27T00:00:00+00:00"
first_seen_at: "2026-08-10T03:24:35.281168+00:00"
fetched_at: "2026-08-10T03:24:36.479787+00:00"
content_hash: "sha256:87cf352ed7e24f5bfb19ed57f43589e37b18084ff5e507b53261de9047b7a28c"
---

# Medicaid Provider Spending Data: The $909 Billion Automation Gap

[Home](https://getsolum.com/) /


[Blog](https://getsolum.com/blog/) /


Medicaid Provider Spending Data: The $909 Billion Automation Gap


Healthcare Data


# Medicaid Provider Spending Data: The $909 Billion Automation Gap


2026-02-27


LinkedInTwitter


## Someone finally turned on the lights.


On February 9, 2026, the Department of Health and Human Services did something I almost missed. Buried in the usual stream of federal data releases, HHS published a dataset containing every fee-for-service, managed care, and CHIP claim processed through Medicaid from January 2018 through December 2024. Seven years. Provider-level spending. 11.1 gigabytes of raw operational reality, broken down by billing provider, servicing provider, procedure code, and month.


It lives on[opendata.hhs.gov](https://opendata.hhs.gov/datasets/medicaid-provider-spending/) , and I've spent the better part of two weeks pulling it apart.


If you work in healthcare operations, this dataset doesn't read like a spreadsheet. It reads like a diagnostic scan of the system itself. Not the clinical kind, but the operational kind. The one that shows where money flows, where it pools, and where it quietly bleeds out.


## The scale problem


Medicaid expenditures on medical services in federal fiscal year 2024 totaled **$908.8 billion** , according to preliminary CMS-64 estimates. Growth has been running at roughly 8 to 9 percent annually, which would be alarming enough on its own. But here's what makes it genuinely strange: enrollment cratered. We went from 96 million beneficiaries in FY2023 to approximately 83 million in FY2024, a drop of nearly 14 percent once pandemic-era continuous enrollment requirements expired.


Fewer patients. More spending.


Per-enrollee costs surged 15.2 percent in 2024, compared with 7.1 percent the prior year. And yes, I double-checked that number. The people still on Medicaid rolls tend to carry greater health needs and higher acuity, which explains part of the math. But acuity alone doesn't cover the whole gap. Not even close.


The federal government picked up 64.7 percent of that spending in FFY 2024. State general fund spending jumped 16 percent in a single year. Seven states (California, New York, Texas, Pennsylvania, Florida, Ohio, and Illinois) account for nearly half of all Medicaid expenditures nationally. New York remains the spending outlier everyone talks about at conferences but nobody seems to fix, with per-resident Medicaid outlays 24 percent higher than any other state and 77 percent above the national average.


## What the provider-level data actually exposes


The HHS dataset is structured at the provider-by-procedure-by-month level. Each row ties a billing provider NPI to a servicing provider NPI and a HCPCS code, then gives you three outcome measures: total unique beneficiaries served, total claims submitted, and total amount paid.


That granularity is what makes this useful instead of decorative. You can trace spending concentration across providers. You can spot procedure codes consuming wildly disproportionate resources. You can watch seasonal claim volume patterns emerge, patterns that map directly to the administrative workload cycles every practice manager already feels in their bones but has never been able to quantify against national data.


Think about what each claim represents operationally. Every single row in that dataset required someone, somewhere, to verify eligibility, confirm coverage, obtain authorization when needed, submit the claim, respond to payer queries, and reconcile payment. At 12.64 minutes per manual[eligibility check](https://getsolum.com/glossary/automated-eligibility-check) (per MGMA benchmarks), the labor embedded in this data is staggering.


## The administrative tax nobody budgets for


According to[McKinsey and Harvard researchers](https://www.healthcaredive.com/news/artificial-intelligence-healthcare-savings-harvard-mckinsey-report/641163/) , AI could save the U.S. healthcare system up to **$360 billion annually** if adopted widely. Administrative processes represent the single largest opportunity. McKinsey's estimates for health insurers project net savings of 13 to 25 percent in administrative costs and 5 to 11 percent in medical costs through technology that already exists today. For every $10 billion in payer revenue, that translates to $150 million to $300 million in administrative savings.


I've talked to enough practice managers to know this won't surprise them: healthcare workers currently spend up to 70 percent of their time on administrative tasks. The average physician practice burns 14 hours per week on[prior authorizations](https://getsolum.com/glossary/prior-authorization-automation) alone. Automation could cut that by 80 to 90 percent.


Now map those efficiency gaps onto the Medicaid spending data. A program processing $909 billion in annual claims, with administrative overhead eating 15 to 30 percent of every dollar, means somewhere between **$136 billion and $273 billion** goes to processing and administration rather than patient care. That's not a rounding error. That's a parallel healthcare budget being spent on paperwork.


## Denials are getting worse, not better


This one stopped me cold.


In 2022, 30 percent of providers reported that at least 10 percent of their claims were denied. By 2024, that figure grew to 38 percent. In 2025,[41 percent of providers](https://www.experian.com/blogs/healthcare/healthcare-claim-denials-statistics-state-of-claims-report/) report denial rates exceeding that threshold. Despite investments in AI-powered coding, denials increased 51 percent between 2021 and 2023.


Each denied claim costs an average of $57.23 in administrative labor to rework. But between 35 and 60 percent of claims denied for eligibility reasons are never successfully resubmitted. They just vanish. Permanent revenue losses that nobody tracks because the tracking itself costs money nobody has.


For Medicaid providers specifically, the problem compounds fast. Medicaid reimbursement rates already lag commercial payer rates, so every denied claim hits thinner margins harder. When 65 percent of denied claims go unworked, the cumulative revenue leakage is measured in billions.


And the data backs up the case for action: among providers who have adopted AI for[denial management](https://getsolum.com/glossary/denial-management) , 69 percent report reduced denials or improved resubmission success. Hospitals report ROI of $3.20 for every $1 spent on AI, often within 14 months. The math works. The adoption doesn't.


## The lopsided arms race


About 46 percent of U.S. hospitals report using AI in revenue cycle operations. 74 percent have implemented some form of automation. But only **14 percent** are using AI specifically to reduce denials, the single most impactful application. Roughly half of providers still review claims manually.


Meanwhile, payers have invested heavily in AI-driven claim auditing.


Read that asymmetry again. Insurers deploy AI to reject claims faster while most providers process responses by hand. It's a knife fight where one side brought a butter knife and the other brought a[surgical robot](https://www.hfma.org/revenue-cycle/denials-management/health-systems-start-to-fight-back-against-ai-powered-robots-driving-denial-rates-higher/) .


The Medical University of South Carolina offers a glimpse of what's possible on the other side: AI-powered prior authorization that reclaimed more than 5,000 staff hours per month and achieved first-pass approval rates above 95 percent.


## The 2026 regulatory shift is already underway


CMS launched the[WISeR program](https://www.jonesday.com/en/insights/2025/08/coming-january-2026-cms-launches-ai-program-to-screen-prior-authorization-requests-for-treatments) on January 1, 2026, testing[AI-driven prior authorization](https://getsolum.com/blog/ai-prior-authorization-cms-wiser-program-2026) in six states: Arizona, Ohio, Oklahoma, New Jersey, Texas, and Washington. The program affects 6.4 million traditional Medicare beneficiaries.


Starting this year, CMS requires payers to provide specific reasons for every AI-assisted denial and publish aggregate approval data. Medicare Advantage, Medicaid, and ACA marketplace plans must answer urgent prior authorization requests within 72 hours, standard requests within seven days, and expose decisions through FHIR-based APIs by 2027.


More than 50 major insurers have pledged to simplify prior authorization starting in 2026. Texas, Arizona, and Maryland have already adopted laws prohibiting AI as the sole basis for medical necessity denials. Whether those pledges hold up under quarterly earnings pressure is another question entirely.


## Reading the data with an operational eye


For an analytics-minded practice willing to do the work, seven years of provider-level claims data opens up real exercises. Identify the HCPCS codes driving highest claim volumes in your specialty. Benchmark authorization and denial patterns against national trends. Model administrative labor by multiplying monthly claim count by average processing time per claim. None of this requires a data science team. It requires curiosity and a spreadsheet.


The data reveals seasonality that experienced billers already suspect but can now prove. January and February show claim volume spikes as deductibles reset, followed by processing backlogs in March. September and October see elevated prior authorization activity ahead of open enrollment. These patterns are predictable. Which means staffing and technology responses can be planned rather than reactive.


## Where this leaves us


The Medicaid Provider Spending dataset reflects a $909 billion system where per-enrollee costs are rising 15 percent annually, where denial rates have climbed to 41 percent, and where $136 billion to $273 billion in annual spending goes to administration rather than patient care.


The providers who'll survive this are treating automation as infrastructure. Not a pilot program. Not a nice-to-have line item that gets cut in Q3.


The data is public now. The patterns are clear. The only real question is which providers move first, and which ones are still manually verifying eligibility when the lights finally come on for good.


#### JP Montoya


Founder & CEO, Solum Health · Forbes Technology Council Member · Speaker on Healthcare AI


JP Montoya builds and scales healthcare administrative automation at Solum Health, working with ABA, PT, ST/OT, and other therapy practices across 40+ states. A Forbes Technology Council member and speaker on healthcare AI, he writes about healthcare operations, AI implementation, and practice management from direct experience building and running clinical workflows.
