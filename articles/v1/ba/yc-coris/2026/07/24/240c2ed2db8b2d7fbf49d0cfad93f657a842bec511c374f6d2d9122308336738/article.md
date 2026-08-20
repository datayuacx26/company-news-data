---
schema_version: "1.0.0"
document_id: "240c2ed2db8b2d7fbf49d0cfad93f657a842bec511c374f6d2d9122308336738"
company_key: "yc-coris"
company: "Coris"
source_id: "yc-coris-news-import-dd89c9ff7287"
canonical_url: "https://www.coris.ai/blogs/merchant-risk-management-operations"
published_at: null
first_seen_at: "2026-07-28T18:01:52.443995+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:1fb61411f4dcdaab81abc389e822cf825ed337b602e7ca6ce3ce395f8b79bb8a"
---

# How to Build Merchant Risk Management Operations That Scale

Your merchant portfolio doubled last year. Your risk team didn't. Now every approval takes longer, alerts pile up faster than analysts can review them.


You're one bad actor away from a card brand monitoring program.


Scaling merchant risk operations isn't about hiring more analysts, it's about building systems that make consistent decisions without constant human intervention. This guide covers the workflows, transaction monitoring, data signals, and automation strategies that let risk teams handle 10x the volume without 10x the headcount.


‍


## What Are Merchant Risk Management Operations


Merchant risk management operations protect payment processors, acquirers, and marketplaces by evaluating and mitigating risks like fraud, chargebacks, and compliance violations. The core functions include[merchant underwriting](https://www.coris.ai/blogs/automated-merchant-underwriting) , portfolio monitoring, transaction fraud detection, and case management. Operations teams balance security with approvals to protect revenue and brand reputation.


Think of risk operations as the execution layer of your risk program. Strategy defines what risks matter and how much exposure is acceptable. Operations translate strategy into daily decisions, which merchants to approve, which alerts to investigate, when to pause payouts.


The teams running merchant risk operations include payfacs, ISOs, platforms with embedded payments, and acquiring banks. Each faces similar challenges: growing portfolios, sophisticated fraud, and limited analyst headcount.


- **Merchant risk** — the potential for financial loss, fraud, or compliance violations tied to a merchant relationship
- **Risk operations** — the people, processes, and technology that execute risk decisions daily
- **Merchant lifecycle** —[onboarding](https://www.coris.ai/blogs/merchant-onboarding) , underwriting, ongoing monitoring, and offboarding


For a broader foundation on how risk operations fit into your overall payments strategy, see our[Complete Guide for ISOs and Payment Platforms](https://www.coris.ai/blogs/payment-risk-management) .


‍


## Why Merchant Risk Operations Need to Scale Now


Portfolio growth is the most obvious pressure. More merchants means more applications to review, more alerts to triage, and more transactions to monitor. Hiring risk analysts at the same rate your portfolio grows isn't realistic.


Fraud has also become more sophisticated, with[40% of financial institutions](https://www.businesswire.com/news/home/20260610637404/en/Mitek-and-Datos-Insights-Report-Finds-Synthetic-Identity-Fraud-Is-Emerging-as-the-Defining-Fraud-Threat-of-2026) reporting increased AI-driven attack rates. Synthetic identities,[up eight-fold globally in 2025](https://risk.lexisnexis.com/about-us/press-room/press-release/20260408-cybercrime-report) , and business impersonation schemes require continuous reassessment, not just a one-time check at onboarding. A merchant that looked legitimate six months ago might be running a completely different operation today.


Meanwhile, card brand rules and regulatory expectations demand full auditability of every decision. Manual processes that worked at 500 merchants break at 5,000. The math simply doesn't work when every decision requires[manual review](https://www.coris.ai/blog/the-true-cost-of-manual-merchant-underwriting-and-how-to-automate-it) .


- **Portfolio growth** — more merchants means more decisions per day
- **Fraud sophistication** — synthetic identities and business impersonation require continuous reassessment
- **Regulatory expectations** — card brand rules and compliance mandates demand auditability
- **Talent constraints** — risk analyst hiring cannot keep pace with transaction volume


‍


## Core Workflows, Transaction Monitoring, and the Merchant Lifecycle


Five workflows define merchant risk operations. Each has distinct inputs, outputs, and SLAs. Understanding how they connect helps you identify where automation can have the biggest impact.


### Merchant Onboarding and Underwriting


Underwriting is the decision to approve, decline, or apply conditions to a merchant application. The process involves KYB (Know Your Business) checks, document collection, beneficial ownership verification, and initial risk scoring.


KYB is the merchant equivalent of KYC (Know Your Customer). Where KYC verifies individual identity, KYB verifies business legitimacy, confirming the business exists, operates legally, and is owned by who it claims to be.


The goal is catching bad actors before they process a single transaction while approving legitimate merchants quickly. A slow underwriting process loses good merchants to competitors. Learn more about[Automated Merchant Onboarding Tools](https://www.coris.ai/blogs/automated-merchant-onboarding-tools) .


### Ongoing Portfolio Monitoring


Monitoring happens after approval. Merchants change over time, websites update, ownership transfers, business models pivot.


A subscription software company might quietly add cryptocurrency services. A retail merchant might start dropshipping from overseas suppliers.


Continuous reassessment catches shifts through signals like[website content changes](https://www.coris.ai/blog/introducing-website-monitoring) , business registration updates, review sentiment, and litigation filings. Without ongoing monitoring, your risk assessment becomes stale the moment you approve a merchant.


### Real-Time Transaction Monitoring


[Transaction monitoring](https://www.coris.ai/blogs/transaction-monitoring) analyzes payment activity to detect anomalies, fraud patterns, or policy violations before settlement. According to[Visa's transaction monitoring insights](https://corporate.visa.com/en/solutions/visa-protect/insights/transaction-monitoring.html) , smarter monitoring is essential for secure payments. This is where you stop fraud before money moves, rather than chasing losses after the fact.


Transaction monitoring looks at patterns like sudden volume spikes, unusual ticket sizes, geographic anomalies, and velocity changes. A merchant that typically processes $10,000 daily suddenly processing $100,000 warrants investigation.


### Alert Triage and Case Investigation


Monitoring systems generate alerts. Someone, or something, has to review them. Alert triage involves prioritization, research, escalation, and resolution.


Without clear[routing rules](https://www.coris.ai/blog/introducing-case-management) , analysts drown in low-priority noise while high-severity signals get buried. The merchant with a minor website change gets the same attention as the merchant showing signs of transaction laundering.


### Disputes and Chargeback Management


Chargeback handling includes[evidence gathering, response filing](https://www.coris.ai/blogs/introducing-dispute-agent) , and pattern analysis. Beyond individual disputes, this workflow, alongside transaction monitoring—identifies repeat offenders and systemic issues that signal deeper problems.


A single chargeback is a data point. A pattern of chargebacks across similar merchants or transaction types reveals operational gaps worth addressing.


‍


Workflow Trigger Key Output


Onboarding New merchant application Approve, decline, or condition


Portfolio Monitoring Scheduled or signal-based Risk reassessment, alert


Transaction Monitoring Payment activity Block, flag, or allow


Alert Triage Alert generated Case resolved or escalated


Disputes Chargeback received Response filed, merchant flagged


‍


## Types of Merchant Risk Operations Teams Manage


A single merchant can present multiple risk types simultaneously. A business might have credit risk due to thin financials, fraud risk from a new domain, and compliance risk from ambiguous product descriptions. Understanding risk categories helps you design workflows that address each appropriately.


### Credit and Financial Risk


Credit risk reflects a merchant's ability to fulfill financial obligations, refunds, reserves, and chargebacks. If a merchant goes out of business, the acquirer is often left holding the bag for outstanding refunds and disputes.


Indicators include business age, revenue patterns, industry stability, and financial documentation. A three-month-old business in a volatile industry presents different credit risk than a ten-year-old retailer with audited financials.


### Fraud and Merchant Impersonation


Merchant fraud differs from consumer fraud. Here, the merchant is the bad actor, or someone is pretending to be a legitimate merchant.


- **First-party fraud** — the merchant itself commits fraud, such as processing fake transactions or misrepresenting products
- **Business impersonation** — a fraudster poses as an established, legitimate business to gain approval
- **Synthetic merchant identities** — fabricated businesses built from real and fake data, designed to appear legitimate


Detecting merchant fraud requires merchant-specific signals, not just transaction data.


### Chargeback and Transaction Risk


Excessive chargebacks, projected to reach[337 million transactions by 2026](https://chargebacks911.com/chargeback-stats/) , trigger card brand monitoring programs and fines. Card networks track chargeback ratios, and merchants exceeding thresholds face penalties, increased fees, or termination.[Real-time versus batch processing](https://lucinity.com/blog/real-time-vs-batch-processing-choosing-the-right-transaction-monitoring-approach-for-your-institution) is a key consideration when designing monitoring systems.


Transaction laundering represents one of the most serious compliance violations. This occurs when a merchant processes payments for undisclosed or prohibited goods through an approved merchant account. A seemingly legitimate e-commerce store might be fronting for illegal products.


### Regulatory and Compliance Risk


Compliance risk spans PCI DSS (data security standards), card brand rules, AML obligations, and prohibited merchant categories. Non-compliance can result in fines, program termination, or loss of processing privileges entirely.


Card networks maintain lists of prohibited and restricted merchant categories. Processing payments for prohibited merchants, even unknowingly—exposes acquirers to significant liability.


### Reputational and Brand Risk


Merchant behavior reflects on the acquirer or platform. Negative press, illegal products, or brand safety concerns can damage your reputation even when no direct financial loss occurs.


A viral story about a fraudulent merchant affects customer trust, partner relationships, and regulatory scrutiny, regardless of whether you lost money on that merchant.


‍


## How to Tier and Score Merchants by Risk Exposure


Risk tiering allocates operational resources proportionally. Not every merchant deserves the same level of scrutiny. Treating all merchants equally wastes analyst time on low-risk accounts while potentially missing high-risk ones.


Risk scoring quantifies qualitative signals into actionable categories. A score combines multiple data points, business age, industry, chargeback history, website signals—into a single risk tier that determines workflow routing.


### Low-Risk Merchants


Established businesses in stable industries with clean history. Characteristics include years of operating history, low chargeback rates, clear business models, and verifiable ownership.


Low-risk merchants typically qualify for streamlined approval and lighter ongoing monitoring. Manual review adds cost without proportional risk reduction.


### Medium-Risk Merchants


Businesses with elevated signals, newer entities, moderate chargeback history, or industries with higher dispute rates. The business might be legitimate but warrants closer attention.


Conditional approvals and enhanced monitoring are common here. A reserve requirement or lower processing limit can mitigate exposure while the merchant establishes a track record.


### High-Risk Merchants


Prohibited or restricted industries, adverse history, or complex ownership structures. High-risk merchants require[enhanced due diligence](https://www.coris.ai/blogs/high-risk-business-screening) , reserves, or decline decisions.


See our guide on[prohibited, restricted, and high-risk businesses](https://www.coris.ai/blogs/prohibited-restricted-and-high-risk-businesses-what-they-are-and-how-to-automatically-screen-for-them) .


**Example:** A new merchant in subscription billing with no chargeback history but a recently registered domain might be tiered as medium risk. The business model is legitimate, but the thin history warrants additional website verification before approval.


‍


## The Data and Signals That Power Merchant Risk Decisions


Risk decisions are only as good as the data behind them. Most teams pull from five signal categories, though the specific sources vary by platform and risk appetite.


- **KYB and business registration data** — business registration verification, beneficial ownership, EIN validation, and entity status checks confirm the business exists and operates legally
- **Website and product signals** — product listings, pricing, terms of service, and domain age reveal merchant legitimacy
- **Litigation, reviews, and adverse media** — court filings, BBB complaints, online reviews, and news mentions surface problems that don't appear in structured data
- **Chargeback and transaction patterns** — historical dispute rates, refund velocity, average ticket size anomalies, and volume spikes indicate operational issues or fraud
- **Proprietary portfolio data** — internal notes, application history, and payout velocity unique to your platform


**Example signals worth automating into**[monitoring rules](https://www.coris.ai/blogs/cut-risk-with-transaction-monitoring-automation) **:**


- Domain age under 90 days
- Mismatch between stated MCC and website products
- Sudden spike in refund requests
- Beneficial owner linked to previously terminated merchant


‍


## How to Design a Scalable Merchant Risk Operating Model


Building operations that grow with your portfolio requires repeatability and consistency. Ad-hoc decisions made by individual analysts don't scale. Documented workflows executed by systems do.


### Step 1: Codify Risk Policies as Workflows


Turn policy documents into executable rules, if/then logic, approval thresholds, and escalation triggers. A policy that says "decline merchants with excessive chargebacks" becomes a rule that declines applications where chargeback ratio exceeds a specific threshold.


Undocumented policies live in individual analysts' heads. When those analysts leave or get busy, decisions become inconsistent.


### Step 2: Centralize Merchant Data and Alerts


Consolidate signals from multiple sources into a single merchant view. Fragmented tools and siloed data make consistent decisions impossible.


When KYB data lives in one system, website monitoring in another, and transaction data in a third, analysts waste time gathering information. They should be analyzing it instead. This fragmentation slows decisions.


### Step 3: Route Alerts by Severity and Merchant Tier


Not all alerts deserve equal attention. High-risk merchants and severe signals route to senior analysts. Low-risk alerts can auto-resolve or queue for batch review.


A website change on a low-risk merchant with clean history doesn't warrant the same urgency as a volume spike on a newly approved high-risk merchant.


### Step 4: Automate Routine Decisions with AI Agents


[AI agents](https://www.coris.ai/product/ai-agents) are configurable automation that can research merchants, decision alerts, and resolve cases without human intervention, while maintaining full audit trails. Unlike simple rules, agents can handle multi-step workflows that previously required analyst judgment.


Explore how[AI Risk Management for Payment Processors](https://www.coris.ai/blog/beyond-manual-reviews-ai-risk-management-for-payment-processors) reduces manual workload.


### Step 5: Instrument Audit Trails and Quality Reviews


Every decision gets logged. Every action is attributable. This isn't just about compliance, it's about learning. QA sampling catches drift and improves models over time.


When you can't explain why a decision was made, you can't improve the process that made it.


**Example:** A platform might configure an AI agent to auto-approve applications where KYB passes, domain age exceeds 180 days, and no adverse signals appear. Manual review becomes exception handling only.


## KPIs to Measure Merchant Risk Operations Performance


Scaling requires measurement. Without metrics, you can't tell whether changes improve outcomes or just shift problems around.


‍


KPI What It Measures Why It Matters


Manual Review Rate Percentage of decisions requiring human intervention Operational cost and speed


Time to Approve Turnaround from application to decision Merchant experience


False Positive Rate Alerts or declines that turn out to be legitimate Lost revenue, analyst burden


Fraud Loss Rate Actual losses from fraud and disputes Financial exposure


Reassessment Cadence How often each merchant is re-evaluated Portfolio drift detection


‍


The tension between false positives and fraud losses is constant. Tightening rules reduces fraud but increases[false positives](https://www.coris.ai/blogs/cutting-false-positives-how-ai-native-models-improve-fraud-detection-for-fintechs) . Research from[Alessa on reducing false positives](https://alessa.com/blog/navigating-false-positives-transaction-monitoring/) offers additional strategies for AML monitoring teams.


Loosening rules improves approval rates but increases losses. The right balance depends on your risk appetite and business model.


‍


## Common Challenges in Scaling Merchant Risk Operations


- **Fragmented tooling** — data spread across processors, CRMs, spreadsheets, and email makes consistent decisions impossible
- **Manual bottlenecks** — every decision requiring human review creates backlogs and slows approvals
- **Policy drift** — undocumented or inconsistently applied rules lead to audit failures and uneven risk exposure
- **Alert fatigue** — too many low-priority alerts bury the signals that matter
- **Talent constraints** — hiring and training risk analysts cannot keep pace with portfolio growth


‍


## Best Practices for Building Merchant Risk Management Operations


- **Start with policy clarity** — document decision criteria before building workflows
- **Prioritize data consolidation** — a unified merchant view is prerequisite to automation
- **Tier your portfolio** — apply resources proportional to risk, not uniformly
- **Automate the routine** — reserve human judgment for edge cases and high-stakes decisions
- **Measure continuously** — track KPIs and adjust thresholds based on outcomes
- **Build for auditability** — every decision is traceable and explainable


‍


## Scale Merchant Risk Operations with Coris


Coris brings together everything covered in this article into a single platform. Merchant Intelligence consolidates external signals and proprietary data into one merchant view. The[Risk Platform](https://www.coris.ai/product/risk-platform) turns policies into repeatable workflows.


Transaction monitoring provides real-time coverage before money moves. And AI Agents automate routine decisions with full audit trails.


The platform[integrates with major payment processors](https://www.coris.ai/integrations) , CRMs, and support tools, so you can scale risk operations without scaling headcount.


‍


## Frequently Asked Questions About Merchant Risk Management Operations


### What is merchant risk management?


Merchant risk management is the process of identifying, assessing, and mitigating risks tied to merchant relationships, including fraud, credit exposure, chargebacks, and compliance violations.


### What are the 5 steps of the risk management process?


The five steps are: identify risks, assess their likelihood and impact, prioritize based on severity, implement controls or mitigations, and monitor outcomes continuously.


### What does a merchant risk analyst do?


A merchant risk analyst reviews merchant applications, investigates alerts, and monitors portfolio risk signals. Analysts decide to approve, decline, or escalate accounts based on established policies.
