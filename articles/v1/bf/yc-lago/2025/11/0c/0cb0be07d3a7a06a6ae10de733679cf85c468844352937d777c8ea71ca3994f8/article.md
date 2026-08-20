---
schema_version: "1.0.0"
document_id: "0cb0be07d3a7a06a6ae10de733679cf85c468844352937d777c8ea71ca3994f8"
company_key: "yc-lago"
company: "Lago"
source_id: "yc-lago-news-import-cc6c03d3f684"
canonical_url: "https://getlago.com/blog/1nce-billing"
published_at: "2025-11-13T14:46:28+00:00"
first_seen_at: "2026-07-25T11:29:02.565124+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:2eee5af566ad5eb057a83340fc036c11ff8e45555fcc3e15249846f2bdd9be47"
---

# How 1NCE scaled global IoT billing with Lago

## Introduction


1NCE disrupted the IoT connectivity industry in 2018 with a bold offer: 10 years of global software and connectivity for €10. The model was simple, affordable, and perfectly suited to low-data use cases like smart metering, asset tracking, and street lighting.


But as the IoT market evolved, so did customer needs. Many use cases demanded higher data volumes and faster speeds. In response, 1NCE launched High Data IoT: a pay-as-you-go model offering 5€/GB with speeds up to 25 Mb/s. This expanded portfolio helped customers scale data-intensive projects with flexibility — but it also introduced new billing complexity.


While 1NCE has mastered building its own products, it sought a reliable partner that could support its evolving usage-based business model and keep pace with its product roadmap—so it turned to Lago.


## Challenges


By mid-2024, 1NCE's was expanding beyond its original, prepaid-only business model and realized the complexity of any billing system involved.


- Per-GB, per-country pricing. Tooling understood "€10 once" but couldn't handle region-specific gigabyte pricing.


- Customer-specific overrides. Price tweaks were applied with manual scripts every quarter.


- Tax and compliance. Each invoice required a hand-checked VAT or US sales-tax rate before exporting to NetSuite.


### Solutions and impact


1NCE deployed Lago to gain full control and alleviate security concerns. Because Lago is open-source, it also enables the team to extend or integrate Lago to work best with internal tooling. And on top of that, it makes it easier to test Lago before buying.


1NCE then streamed usage events from the existing data lake, and shipped the first production invoices in just ten weeks.


1. Real-time usage metering – Lago ingests SIM traffic events and aggregates them by SIM, country group, and plan within minutes.


2.[Per-customer price overrides](https://getlago.com/blog/usage-based-pricing-tactics-for-saas-and-ai) – Ops can set bespoke gigabyte prices with a click or a single API call—no plan cloning.


3. Automated tax engine – Lago applies the correct VAT or sales-tax rate based on customer location before posting. This is something unique to Lago. We originate in Europe and know the pain, so we built a native EU tax feature to help apply the right tax rate every time.


4. Direct NetSuite sync – Invoices flow straight into NetSuite, the ERP system 1NCE uses; Finance never touches spreadsheets.


### Pricing flexibility


With billing friction gone, the commercial team is free to experiment with pricing plans:


- Bespoke plans like per-active-SIM per-month pricing that only makes sense for some users, but can now be shipped easily.


- Country-specific burst bundles


All variants are configured directly in Lago's no-code user interface—no new code or migrations. "Lago became an enabler to think about different business models," says Mark.


### Growing together


1NCE's roadmap is global and fast. Lago is keeping pace with:


- Multi-entity invoicing as new regional HQs come online


- Deeper NetSuite sync for revenue-recognition schedules


- Usage-analytics dashboards to surface SIM churn and gigabyte hot-spots


- Audit-ready compliance (SOC 2, GDPR) demanded by Fortune 500 customers


With billing off their plate, 1NCE's engineers are free to push IoT into its next decade—while every gigabyte or tax nuances are handled by Lago.
