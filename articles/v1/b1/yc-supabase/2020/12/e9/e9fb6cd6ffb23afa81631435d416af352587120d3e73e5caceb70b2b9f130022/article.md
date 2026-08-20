---
schema_version: "1.0.0"
document_id: "e9fb6cd6ffb23afa81631435d416af352587120d3e73e5caceb70b2b9f130022"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/case-study-xendit"
published_at: "2020-12-02T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:63708b61673ad91f3c24436a76de2659e7cf81a58209a00e449568f0e2f25444"
---

# Xendit Built a Counter-Fraud Watchlist for the Fintech Industry

[Customer Stories](https://supabase.com/customers)


# Xendit use Supabase and create a full solution shipped to production in less than one week.


As a payment processor, Xendit are responsible for verifying that all transactions are legal.


About


Xendit is a financial technology company that provides payment solutions and simplifies the payment process for businesses in Indonesia, the Philippines, and Southeast Asia, from SMEs and e-commerce startups to large enterprises.


[https://xendit.co](https://xendit.co/)


Founded


Jakarta Selatan


Ready to get started?


[Contact sales](https://supabase.com/contact/enterprise)


## Challenge#


As a payment processor, Xendit are responsible for verifying that all transactions are legal. Any transactions which are suspicions must be verified against a strict set of criteria, and the parties involved need to be checked against international sanctions lists. This is a critical anti-money-laundering operation and needs to be performed in realtime to prevent any delays on legitimate payments.


## Why they chose Supabase#


Xendit needed something fast. Something that was cheaper than using the global players like Worldcheck or Refinitiv. Xendit already uses Postgres for a lot of their critical infrastructure, and so Xendit team are familiar with the technology and comfortable in it's ability to scale.


## What they built#


Xendit parses international sanctions lists from the UN and the Indonesian government and loads them into Supabase. Since Supabase provides a full Postgres server, they can then use the[Trigram](https://www.postgresql.org/docs/current/pgtrgm.html) extension to perform full-text search on the lists, with a relevance score on every search.


Supabase was perfect for their use case, as they needed something built fast. The full solution was built and in production in less than one week.


> The full solution was built and in production in less than one week.


Xendit created a database function for searching, which they are able to call directly using their Python clients. They have plans to iterate on the current implementation using more advanced techniques, like machine learning, but for now the Supabase system has been in Production for 9 months without a problem.
