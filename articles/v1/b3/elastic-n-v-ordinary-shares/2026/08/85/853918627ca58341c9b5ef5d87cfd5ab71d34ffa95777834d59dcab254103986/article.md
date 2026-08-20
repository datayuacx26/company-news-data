---
schema_version: "1.0.0"
document_id: "853918627ca58341c9b5ef5d87cfd5ab71d34ffa95777834d59dcab254103986"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/how-state-and-local-agencies-get-ahead-of-fraud"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T20:57:35.063329+00:00"
fetched_at: "2026-08-19T20:57:37.521726+00:00"
content_hash: "sha256:483be3f4d5751173a82fa7c721b4e94bd698045b9e8c32adc6aec0a8a287d6e1"
---

# How state and local agencies can get ahead of fraud starting with the data they already have

# How state and local agencies can get ahead of fraud starting with the data they already have


By


[Leanne Link](https://www.elastic.co/blog/author/leanne-link)


August 19, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


It's early on a Tuesday morning, and a fraud analyst at a state workforce agency notices an unusual spike in unemployment insurance claims. Hundreds were filed overnight, each with a different name and email address, but all routed to the same small cluster of bank accounts. She starts pulling records: claims data first, then identity information, then IP logs. But each piece lives in a different system managed by a different team. By the time she's assembled enough of the picture to confirm what she's looking at, the payments have already gone out.


This kind of situation isn't rare. According to the US Government Accountability Office (GAO),


[$233 billion–$521 billion](https://oversight.house.gov/release/gao-report-affirms-oversight-committee-bills-to-combat-fraud-in-federally-funded-state-administered-programs/) is lost to fraud each year across federal programs and operations.


Fraud investigators at


[state and local government](https://www.elastic.co/industries/public-sector/state-and-local) agencies experience these situations regularly. Though the signals are there, the data is spread across too many places to act on them quickly enough. When the information needed to detect fraud lives in disconnected systems that weren't designed to talk to each other, even experienced teams end up reacting instead of preventing.


## Data fragmentation is an obstacle to detecting fraud


Fraud has always been an operational challenge for government programs, but the scale and sophistication of it has grown considerably in recent years. Generative AI has lowered the barrier to entry significantly, making it relatively straightforward for bad actors to fabricate identities, automate phishing campaigns, or create synthetic personas that can pass basic verification checks. What used to require a skilled criminal network can increasingly be done cheaply and at scale.


In addition to more threats, the consequences are also growing. Agencies need to be ready to face regular audits, potential fines, and jeopardized federal funding for programs that experience high amounts of fraud and abuse.


State and local agencies, meanwhile, are managing more data than ever across more systems. There are benefits enrollment, tax filings, procurement records, licensing databases, medical claims, and financial aid applications. The information exists, but it tends to live in departmental or program-specific silos that reflect how agencies were organized, not how fraudsters operate. Someone committing unemployment insurance fraud while misrepresenting income on a SNAP application and filing a fraudulent tax refund claim looks like three separate, unrelated issues when viewed through three separate systems. But when unified, it's a pattern that stands out clearly.


That's the core opportunity: making better use of the data that already exists by unifying it so that investigators can see across programs, share findings, and identify anomalies before payments go out.


## How connected data helps state and local government combat common fraud threats


### Unemployment insurance fraud


Unemployment insurance (UI) programs are attractive targets partly because they're built for speed. Benefits are meant to reach people quickly, and high claim volumes during economic downturns or emergencies make it harder to catch fraud before payments go out. Organized rings can exploit that pressure, filing large numbers of claims using stolen or synthetic identities and routing payments to accounts they control. When claims data, identity records, and payment information are analyzed together, patterns like multiple claims from the same IP address or a single bank account tied to dozens of unrelated names become much easier to spot.


[California's Employment Development Department (EDD)](https://www.elastic.co/customers/caedd) is a good example of what's possible. By using Elastic to consolidate system and transactional data across more than 3,000 servers into a unified environment, EDD gained visibility into patterns it previously couldn't see, achieving a 99% reduction in mean time to response on threats and securing more than 850 billion records.


**See how agencies protect public services**


Discover how state and local agencies use Elastic to detect threats faster, improve visibility, and protect the services citizens rely on.


[Read more agency stories](https://www.elastic.co/resources/article/state-local-case-studies)


### Medicaid fraud


Medicaid fraud ranges from individual beneficiaries misrepresenting eligibility to coordinated provider schemes billing for services never rendered, upcoding procedures, or submitting claims for deceased or out-of-state patients. In fiscal year 2025, there was around $37 billion worth of improper Medicaid payments, according to the Centers for Medicare & Medicaid Services.


The data needed to catch these patterns, including enrollment records, claims, provider registries, and cross-agency information, typically exists but rarely lives in one place. Bringing it together lets investigators cross-reference billing against eligibility, compare provider patterns against peers, and surface relationships that wouldn't be visible any other way.


### SNAP and benefits fraud


Benefits fraud often involves misrepresenting household income, size, or residency. In the case of SNAP, it’s often trafficking benefits for cash or EBT card skimming by organized criminal networks.


The scale of the problem is growing. Fraudulent SNAP transactions increased


[by 55% between the final quarter of FY2024 and the first quarter of FY2025](https://www.newsweek.com/snap-benefit-theft-explodes-across-us-2082664) , according to USDA data, and states reported replacing more than


[$320 million in stolen benefits between 2022 and 2024 alone](https://www.gao.gov/blog/stolen-snap-benefits-cost-beneficiaries-millions) .


It's also one of the fraud types most likely to appear across multiple programs at once. A beneficiary whose reported income or address differs between two agency databases is exactly the kind of discrepancy that cross-program data access can surface systematically rather than leaving it to a tip or a periodic audit to catch.


### Mortgage and business loan fraud


State-run lending programs can be targets for deliberate misrepresentation of revenue, assets, or business purpose. In other instances, collateral may be double-pledged across programs and lenders, or fraud rings may set up shell companies to illegally transfer funds via state programs.


### Tax fraud


Tax refund fraud is among the most time-sensitive problems state revenue agencies face, since recovering a payment that's already been issued is difficult. Identity theft-based schemes, where fraudsters file early using stolen social security numbers to claim refunds before legitimate taxpayers, have grown significantly. The ability to cross-reference incoming filings against historical taxpayer data in real time helps catch these cases early, and shared data access across agencies can also surface individuals defrauding multiple programs simultaneously.


### Grant and procurement fraud


Bid rigging, fictitious vendors, inflated invoices, and conflicts of interest between staff and contractors can go undetected for years when financial controls and program data live in separate systems. Analyzing disbursement patterns across contracts and programs, flagging vendors appearing under slightly different names across multiple awards, or identifying unusual contractor relationships becomes feasible when the underlying data is connected.


### Identity theft and synthetic identity fraud


Synthetic identity fraud, which involves combining real and fabricated data to create new identities that pass standard verification, is particularly hard to catch because there's often no clear victim to report it. These identities are typically built up over time before being used for larger-scale fraud, so detection depends on looking for behavioral anomalies across systems rather than matching against known fraud lists. Agencies that can analyze patterns across databases are meaningfully better positioned to catch these schemes before they scale.


## What unified data actually makes possible for program integrity teams


Across all of these fraud types, the underlying theme is the same: Detection improves when analysts can see more of the picture at once


*from a single view*


before any funds have been moved.


When data is unified, investigators across departments can work from the same information, apply custom detection rules across all data, hand off cases without losing context, and build on findings from other parts of the organization. Cross-program fraud patterns that would never surface in a siloed environment become visible, especially when using


[industry standardization like OpenTelemetry](https://www.elastic.co/industries/public-sector/opentelemetry-using-elastic) .


Analysts spend less time on the mechanical work of pulling and reconciling records from multiple systems and more time on the analysis and judgment that actually requires their expertise. Detection rules and machine learning models have access to richer data, which means they generate fewer false positives and surface alerts that investigators can actually trust and act on. And anomaly detection tools that look for deviations from established baselines can flag fraud schemes that don't match any known pattern, which is increasingly important as bad actors use AI to develop new approaches.


## Getting started: Most agencies are closer than they think


Agencies don't necessarily need to undertake a large-scale technology overhaul to start capturing these benefits. The data already exists; the question is whether you can bring it together with the right context across data systems and data types.


Implementing a fraud detection and remediation engine should start with connecting and normalizing the data and systems you already have. This avoids a rip-and-replace, multiyear tech migration that’s expensive and daunting.


Practical starting points include:


-


Establishing data-sharing agreements between programs and departments


-


Auditing existing platforms to understand whether they can serve as a unified foundation for fraud data


-


Investing in entity resolution capabilities that allow records to be linked across systems even when names, addresses, or identifiers are formatted inconsistently


Configuring automated alerting on high-risk patterns is another step that many existing platforms support but that agencies often haven't fully set up.


Starting focused on one program area, connecting a few key data sources, and expanding from there tends to produce faster results than trying to solve everything at once, and it builds the internal confidence and institutional knowledge to take on more.


A sample architecture of how Elastic can ingest and normalize disparate data sources, apply detection rules and ML, and conduct AI-driven investigations in order to better detect and remediate fraud in government agencies


The conversational Elastic Agent Builder and semantic search empower analysts to uncover cross-program fraud across disparate data.


## Protecting programs and the people who depend on them


Every improper payment that gets stopped before it goes out and every fraudulent claim caught before it's processed is money that stays available for the people these programs are actually meant to serve. For agencies operating under real resource constraints, that's meaningful both in terms of program integrity and public trust.


Better fraud detection doesn't require starting from scratch. It requires getting more out of the data that agencies are already collecting by making it accessible, connectable, and analyzable across programs. The agencies that invest in that foundation are better positioned to stay ahead of fraud as it evolves, meet oversight requirements, and demonstrate to the public that they're good stewards of the funds entrusted to them.


###### Read more


- [Detecting the undetectable: Building a fraud detection framework with Elastic](https://www.elastic.co/blog/building-fraud-detection-framework)
- [Fighting fraud with data: How Elastic can help public sector organizations defend against fraud](https://www.elastic.co/learn/public-sector/elastic-fighting-fraud-with-data)


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
