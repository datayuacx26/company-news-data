---
schema_version: "1.0.0"
document_id: "580a89b80a547d0b33ffd9fc1be72f5d9a648e2c67c41245d82537fed0f845a8"
company_key: "yc-finch"
company: "Finch"
source_id: "yc-finch-news-import-d853ae2d7ef6"
canonical_url: "https://www.tryfinch.com/blog/product-recap-q3-2025"
published_at: "2025-12-02T00:00:00+00:00"
first_seen_at: "2026-07-24T08:01:05.068281+00:00"
fetched_at: "2026-07-28T22:25:06.703683+00:00"
content_hash: "sha256:846d0670e066282b5cfc94316435d60a8d18ba278e2699f64eeeb1be9402586d"
---

# Product Recap: Q3 2025

*Finch’s product is continuously improving, with releases posted to the*[changelog](https://developer.tryfinch.com/changelog) *as they go live. Once a quarter, VP of Product Gayathri Somanath shares a digest — one place to catch up on everything we released over the previous 3 months, why it matters, and what’s next on our roadmap.*


Finch is on a mission to build the infrastructure that powers every facet of work. This requires going deep into the data that powers employment systems — not just surface-level breadth, but comprehensive, reliable coverage and functionality that our users can trust.


In Q3, we introduced platform intelligence to drive more automation and strengthened our support for complex use cases. We also made critical investments in our system reliability to support our rapid growth in active employer connections, which **increased 132% over Q3 2024.**


## Enhanced platform intelligence


Connecting the employment ecosystem starts with making employer data accessible—but the real impact comes from how that data is used. Our intelligent platform features make this data more actionable through normalization, real-time validations, and smart tools that automate complex calculations for deductions and contributions.


### Intelligent data normalization


Payroll systems are notoriously inconsistent in how they structure and label data, making standardization a prerequisite before that data can be used — particularly when it comes to census management and benefits enrollment. For many businesses, this process is slow, manual, and prone to errors.


That’s why we built **intelligent data normalization** to automate this critical and time-intensive process. Using the millions of pay statement records processed through Finch and years of in-house expert mapping, we built a machine learning model to automatically derive pay frequency and classify pay statement types. These data points are key to our standardized data model for retirement and benefits, but are rarely available within payroll systems and can only be interpreted via multiple signals that vary from system to system:


- **Automatic pay frequency derivation** intelligently infers missing pay frequencies (bi-weekly, bi-monthly, etc.) based on historical patterns.
- **Automatic pay statement type classifications** categorizes the “Finch” type field for earnings, taxes, deductions, and contributions within Pay Statements with high accuracy. ([See field examples here](https://developer.tryfinch.com/api-reference/payroll/pay-statement) )


Finch uses AI exclusively for metadata standardization and classification — **never for PII.** Employee personal information remains secure and isolated from AI processing.


### Real-time data validation


This quarter, we fully upgraded Finch’s database infrastructure and introduced an **enhanced real-time validation layer** . The layer runs deep validations as data syncs, leveraging learnings from the millions of records processed through the Finch platform to catch errors or missing fields early and safeguard quality before anything is written to our users’ systems. This is a major platform milestone that raises the bar for data integrity across every connection.


### Tiered employer match support


Employer matches are notoriously complex, especially when the match structure differs across employee deferral “tiers.” Instead of one flat match rate, the match changes once the employee’s contribution crosses certain percentage thresholds. For example, an employer may match 100% of the first 3% and 50% of the next 2% of the employee’s contributions.


With Finch’s Employer Match (beta) feature, users can automate the management of all elective and non-elective employer contributions across payroll providers, even if the payroll provider doesn’t support tiering natively.


Using programmatic logic, Finch automatically creates and updates any type of employer match, whether it’s fixed, percentage-based, or tiered. Our unified data standard handles provider-specific logic behind the scenes.


In Q3, we rolled out a new[comprehensive guide to tiered employer matches](https://developer.tryfinch.com/developer-resources/Tiered-Employer-Matching#tiered-employer-matching) and added field support for Gusto. We look forward to expanding this feature across more providers in 2026.


## Wider coverage and deeper use-case support


Over the last few months, we’ve continued to expand our provider network, rolled out support for additional fields, and made a major improvement to Finch Connect that simplifies the onboarding process for employers with multiple entities.


### New features and field support


- **New feature: Multi-entity mode** – Employers can now connect all of their entities — whether that’s two EINs or ten divisions — through a single Finch connection.[Multi-entity mode](https://www.tryfinch.com/blog/multi-entity-mode-one-connection-per-employer) enables faster employer onboarding, simpler connection management, more predictable billing, and broader support for businesses of all sizes and structures.


- **Field support: Employment Status** – Users can now pull granular employment status fields (active, terminated, on leave, deceased, etc.) for more accurate workforce tracking.
- **Field support: Worker Type Coverage** – Finch extended support to include compensation data for 1099 contractors, enabling customers to serve the full spectrum of employment types.


### Expanded network coverage and partnerships


Q3 saw Finch expand our provider network, expand write-back capabilities for UKG Pro, and deepen our partnerships with key retirement platforms.


- **Expanded provider network** – We’ve added Fingercheck, Payworks, and Connect Pay to our network of 250+ providers.
- **UKG Pro updates** – Finch now supports customer post-tax deductions with UKG Pro, which is also now available to retirement users through our integration with FIS Relius.
- **Deepened partnership with Stax.ai** – Finch is now natively embedded within[Stax.ai](http://stax.ai/) , the AI-powered operating system for retirement plan administrators, eliminating bottlenecks to critical census data collection across the platform’s census, compliance, and client portal workflows.


**Finch and Stax.ai Partnership**


Stax.ai founder and CEO Naru Muraleedharan describes how Finch’s payroll integrations increased sponsor satisfaction by 28%. **[Watch →](https://www.tryfinch.com/customer-stories/staxai)**


### Continued investment in platform reliability and developer resources


In addition to upgrading our database infrastructure, we continued to invest in our platform’s reliability, reducing latency spikes across our highest-volume payroll providers, adding real-time provider incident status to Finch Connect and the Dashboard, and fully deprecating static Finch Connect URLs following our shift to[Finch Connect sessions](https://www.tryfinch.com/blog/the-new-finch-connection-experience#secure-sessions-and-authentication-tracking) .


We also launched two new SDKs that deliver more predictable behavior and easier debugging:


- **JavaScript SDK v2.0.0** – A complete rewrite with improved session handling, stability, and security
- **React SDK v4.0.0** – Enhanced component architecture and developer ergonomics


## Looking ahead


With Q3 in the rearview, we’re entering the busiest time of year for our retirement and benefits customers. We’re preparing our infrastructure to handle the surge in census data pulls, enrollment changes, and deduction processing that comes with open enrollment, compliance season, and year-end planning.


Throughout this last quarter, we’ll maintain a strong focus on the themes that have carried us through 2025:


- Scaling reliability and the Finch Connect experience for our entire provider network
- Further expanding provider and field coverage in line with our customers’ needs
- Deepening Finch’s data standardization capabilities across more providers


If you're a developer interested in building the next era of employment technology,[get in touch](https://www.tryfinch.com/sales) or[sign up](https://dashboard.tryfinch.com/signup) to explore what Finch can power for you.
