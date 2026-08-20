---
schema_version: "1.0.0"
document_id: "fbd1c8f161f36e28bec6e35546ccced231e2aabd2df5105a843b557ecfe1fbd8"
company_key: "yc-reworkd"
company: "Reworkd"
source_id: "yc-reworkd-news-import-d89d796ce0ad"
canonical_url: "https://www.reworkd.ai/blog/january-2025-product-update"
published_at: null
first_seen_at: "2026-07-22T11:47:56.930734+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:55ee277a4bfe32860f14a37dc5e797999df368c774c55b571e3046626b5e63cb"
---

# January Product Update

In January, we shipped a new review flow, stronger anti-bot bypasses, and performance improvements across our infrastructure.


Here's a detailed look at all our latest updates from the past month.


# **Review Flow**


With our scraping co-pilot, customers have generated scrapers for thousands of unique sites. Since data integrity was a priority, their workflow involved a QA team verifying the extracted data and the underlying Playwright code.


Previously, this was a manual and inefficient process, often managed through Google Sheets.


To solve this, we built a dedicated review flow into our platform:


- QA teams can filter for specific sites that need reviews, get a wholistic view of the website data, and provide feedback directly.
- Each source job now displays past QA comments and status changes, providing full context across iterations.


This enables a faster, more organized, and scalable QA process.


# **Anti-Bot Bypass**


For customers scraping e-commerce websites, anti-bot measures have been a major challenge. We've enhanced our browser’s stealth capabilities by adding bypasses for many of the largest anti-bot providers for $5 per 1,000 runs.


# **Improved Performance**


We've made key enhancements to boost speed and efficiency across our platform:


- **Faster Job Execution:** Improved caching and optimized indexes cut down redundant data pulls and speed up cancellation checks, *saving 14 hours per million jobs.*
- **Optimized Queue Processing** : Reduced job submission sleep time, executed callbacks concurrently, and improved query performance with an internal ID index, *saving about 13 hours per million jobs.*
- **Database & API Enhancements:** Used Redis for quicker network trace and job output retrieval, *saving 7 hours per million jobs.*


These upgrades collectively reduce execution times, optimize resource usage, and increase the platform’s ability to handle an order of magnitude more load per day.


---


Stay tuned for our platform launch on March 11th by following us on[Twitter](https://x.com/ReworkdAI) or[LinkedIn](https://www.linkedin.com/company/91361772/admin/dashboard/) . If you are interested in an early access please email me atsrijan@reworkd.ai
