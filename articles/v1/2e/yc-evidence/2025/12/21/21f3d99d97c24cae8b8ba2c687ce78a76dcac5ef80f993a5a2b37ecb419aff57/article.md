---
schema_version: "1.0.0"
document_id: "21f3d99d97c24cae8b8ba2c687ce78a76dcac5ef80f993a5a2b37ecb419aff57"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/embedded-analytics"
published_at: "2025-12-18T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:27:04.798558+00:00"
content_hash: "sha256:7d3cb6d7d2bbf1b4a43c9a4e9905cf25845f35078df59bcc8475ff8ba3ff0c97"
---

# Embedded Analytics as Code

# Embedded Analytics as Code


A modern development workflow for your most important data products


[Sean Hughes December 18, 2025 · 3 min read](https://www.linkedin.com/in/hughessean/)


Embedded reporting is often a company’s most valuable (and most scrutinized) data product. Customers interact with it directly, and it becomes part of their core product experience. Expectations around polish, performance, and reliability are much higher than for internal dashboards.


In practice, though, embedded analytics is typically built using tools and workflows designed for internal reporting. Reports are edited through drag-and-drop interfaces, changes are hard to review, and updates frequently bypass standard deployment processes. When something breaks, customers are the first to notice.


This creates a mismatch: a production surface area managed without production-grade workflows.


Over the past few months, we’ve been working closely with leading teams to develop a solution to this problem — and today we’re excited to launch **Embedded Analytics as Code** .


It includes everything you need to take an embedded use case to production, including[row-level security](https://docs.evidence.studio/core-concepts/access-control#row-level-security) ,[custom themes](https://docs.evidence.studio/core-concepts/themes) , and[API-driven session management](https://docs.evidence.studio/core-concepts/embedded) that easily integrate with your app.


Our early embedded customers are already using Evidence to deliver analytics to thousands of their customers, including professional sports teams and Fortune 500 companies.


## Why embedded analytics needs a development workflow


Most data teams already treat their queries, models, and transformations as code. They live in version control, changes are reviewed, and deployments are deliberate.


Embedded analytics rarely follows the same pattern. Data teams are often forced to use tools where report definitions live in UI state, changes are applied directly in production, and there’s little visibility into what changed or why. As usage grows, this becomes a source of risk and friction rather than leverage.


**Treating embedded analytics as a software artifact** — rather than a configured dashboard — addresses this gap.


## How Evidence approaches embedded analytics


In Evidence, embedded reports are defined declaratively using a markdown-based syntax inside a web-based IDE. The entire report — queries, visualizations, layout, and interactions — is expressed as code.


Because reports are defined as code, they are version controlled by default. Teams can review changes, track history, and roll back safely, using the same practices they already rely on elsewhere in their data stack.


Evidence provides the complete technical infrastructure to build production-grade analytics: a visualization library, interactivity, performance, security, deployment, and more.


This means **custom embedded analytics can be owned directly by the data team** , without requiring a separate engineering layer to re-implement metrics, logic, or layouts. The people who understand the data models and business rules define the output customers see.


### Professional design and theming


Evidence ships with production-ready visualization components, layout primitives, and interaction patterns designed to work together out of the box. Embedded reports feel native to the product experience rather than bolted on.


[Custom themes](https://docs.evidence.studio/core-concepts/themes) allow teams to match the visual style to their branding, with support for both light and dark modes. A live preview shows how changes will look before they’re published, making it easy to iterate on branding without rebuilding components or guessing how reports will render in production.


### Performance


Customer-facing analytics needs to be fast and predictable under real usage, without requiring manual optimization. Evidence is built for interactive embedded workloads, with an optimized query engine and intelligent caching that delivers sub-second interactions even when working with millions of records.


Your browser does not support the video tag.


### Row-level security


Evidence enforces row-level security at the database level — teams define access rules once at the table level, and these rules are applied automatically at query time. There’s no need to manually insert user attributes into every SQL query. Encrypted customer attributes are passed through the Embed API, filtering every query based on who’s viewing the report, with no custom access layers required.


### Easy integration & enterprise-ready


Embedding Evidence reports in your application is straightforward — it works through a simple iframe and API endpoint that integrates with any web stack. The Evidence Embed API uses JWE encryption with single-use URLs that expire after use, providing secure session management without requiring complex authentication flows.


With SOC 2 Type II certification, data residency options in 20+ regions, and multi-language support for serving customers worldwide, Evidence can support organizations of any size.


## Getting started with Embedded Analytics


Embedded Analytics is available on the Enterprise plan in Evidence Studio.


If you’re exploring embedded analytics for your product, book a demo to see what embedded analytics looks like when it’s built and shipped as code.


[Book a demo →](https://calendly.com/evidence-studio/enterprise)
