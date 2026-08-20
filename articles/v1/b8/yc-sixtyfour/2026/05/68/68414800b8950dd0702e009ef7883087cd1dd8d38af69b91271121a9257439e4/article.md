---
schema_version: "1.0.0"
document_id: "68414800b8950dd0702e009ef7883087cd1dd8d38af69b91271121a9257439e4"
company_key: "yc-sixtyfour"
company: "Sixtyfour"
source_id: "yc-sixtyfour-news-import-61b6ef0fd08d"
canonical_url: "https://www.sixtyfour.ai/blogs/how-we-use-agents-to-detect-rented-gig-worker-accounts"
published_at: "2026-05-08T00:00:00+00:00"
first_seen_at: "2026-07-22T13:40:53.215025+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:3d1a73d3fc8516fd65dfc76e06824c3ce2b2b6f2a784e152055ca07432718c15"
---

# How We Use Agents to Detect Rented Gig-Worker Accounts

The app said a woman was delivering her food. A man showed up. He had paid $65 for that account on a Facebook group. This is what a verification gap looks like in the real world.


The Problem


-


The February 2025 Wilbraham, MA Uber Eats assault was committed by a man using a rented account registered to a woman.


-


1 in 4 gig workers has rented or sold their verified account; this rises to nearly 1 in 3 for Millennial and Gen Z drivers.


-


Standard KYC and background checks at signup miss this completely. The problem is a failure of continuous identity verification, not onboarding.


The Gig Economy's Continuous Identity Problem


Every major gig platform runs background checks. The accounts are legitimate. The people who passed are real. The issue is what happens after verification.


These accounts are openly traded in private Facebook groups and Telegram channels for anywhere from $65 to over $400 a month. The platform recognizes a valid account ID, but the trust-and-safety team is unaware of the transaction that put an unvetted person behind the wheel.


How We Trace the Real Person


### The Input: A Flagged Account


An investigation starts with a driver account for "Sarah L." Flagged for a sudden spike in negative reviews and trips running 18 hours a day. The only hard data point: her phone number.


### The First Trace: Anchoring the Real Owner


Our agent cross-references the phone number against our identity graph, breach data, and public social profiles. Within seconds: LinkedIn, GitHub, forum usernames, and two breach records — all linking back to the same person.


### The Second Trace: Surfacing the Renter


The device ID logging trips is an Android model Sarah has never used. Trip origins are 60 miles from her address. Active hours don't align with her social posts. Our agent finds the renter on a dark web forum using a password fragment from one of Sarah's old breaches.


How Identity Resolution Differs From Standard KYC


Traditional KYC answers one question: is this person who they say they are right now? Our agents answer a different one: who is this person, everywhere?


1. 01


The Input — A single flagged identifier: email, phone, username, crypto wallet, or name.


1. 01


The Search — Cross-referenced across Sixtyfour's proprietary databases, dark web sources, breach records, social profiles, and public records.


1. 01


The Resolution — An LLM-driven inference layer weighs signal quality and maps resolved entities into a unified graph.


1. 01


The Output — A graph of connected accounts with confidence scores. Not a single data point — a complete picture.


1. 01


What We Don't Do — No IP-based ID, no device fingerprinting, no private platform data, no legal determinations.
