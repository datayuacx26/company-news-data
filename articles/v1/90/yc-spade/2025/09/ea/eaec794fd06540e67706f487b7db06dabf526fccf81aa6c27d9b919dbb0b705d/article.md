---
schema_version: "1.0.0"
document_id: "eaec794fd06540e67706f487b7db06dabf526fccf81aa6c27d9b919dbb0b705d"
company_key: "yc-spade"
company: "Spade"
source_id: "yc-spade-news-import-2301de415681"
canonical_url: "https://spade.com/resources/why-mids-are-mid-the-challenge-of-merchant-identification/"
published_at: "2025-09-10T21:00:00+00:00"
first_seen_at: "2026-07-24T01:52:25.955244+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:dd793b626d9ddc499b9d975a5a1339af5c52d8fb0470be8dc377d9246d68ab44"
---

# Why MIDs Are Mid: The Challenge of Merchant Identification

If you’ve ever worked in payments or with transaction data, you’ve come across merchant IDs (also known as MIDs or acquirer IDs, or card acceptor IDs, or any number of other things). A MID is an alphanumeric ID that a merchant gets assigned at the processor level.


Unfortunately, MIDs aren’t standardized. A single brand can have thousands of them, and they change frequently as merchants update their systems, switch acquirers, or open new locations.


This can cause a number of challenges for card issuers and others working with transaction data. When MIDs can’t be tied back to a single, persistent identity, data becomes fragmented. This makes spend analysis, authorization decisioning, and rewards attribution far less reliable – and often, flat-out wrong.


Without a unified merchant identity, issuers can struggle to:


- Implement authorization rules that don’t create excessive false declines or customer complaints.
- Properly attribute merchant-based rewards programs
- Deliver accurate spending insights and budgeting tools to consumers
- Power AI-driven features like personalized offers, predictive spend alerts, and automated money movement


**Here’s one example…**


Let’s look at how Walmart shows up in these systems. They operate nearly 5,000 locations in the U.S., but across processors, there can be tens of thousands of unique MIDs in play. Without a way to unify those, systems can’t reliably recognize that all those transactions belong to the same merchant.


Over the past two years, we have seen >165,000 unique MIDs for Walmart transactions alone. And descriptors don’t solve the problem either – in just 3 months, we’ve seen over 65,000 descriptors, including:


- WAL-MART #4262
- WM SUPERCENTER #2358
- WMT SUPRCTR #1095
- WMT PLUS


Issuers invest significant resources to construct a unified view of a brand like Walmart. This often involves maintaining extensive lists of MIDs that require constant updates or relying on regular expressions to parse transaction descriptors — an approach that can result in substantial data leakage. Of the thousands of unique descriptors we identified, we estimate that only 80% are accurately captured by these custom processes. **Thus…MIDS are mid! And modern financial institutions need something better.**


****If you agree – let’s talk! We’re busy building something to help. Contact our team at****sales@spade.com ****to learn more.****
