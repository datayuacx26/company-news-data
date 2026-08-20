---
schema_version: "1.0.0"
document_id: "30aef04772efebbe29491e4805bab3df4f95b7e9d2d6ab25d94cae01c4235d82"
company_key: "yc-general-legal"
company: "General Legal"
source_id: "yc-general-legal-news-import-cc42147fa719"
canonical_url: "https://general.legal/blog/gdpr-sub-processors"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-29T04:44:54.246082+00:00"
fetched_at: "2026-07-29T04:44:55.619052+00:00"
content_hash: "sha256:50c38ed74823d8060a361329bca6ff14dcdc08129e63711e660e67c79ce33192"
---

# Your Sub-Processor List Is Probably Wrong [Explained]

Ask most startups for a list of their sub-processors and you'll get a vendor list instead. Those aren't the same thing, and the gap between them is where a lot of GDPR exposure quietly sits.


# Three words, three different meanings


- **Vendor** is just business language. Anyone you pay for anything - your accountant, your office cleaning company, AWS.
- **Subcontractor** is a contract law term. Someone your vendor uses to help deliver their service to you.
- **Sub-processor** is a GDPR term, and it's narrower than both. It's a subcontractor who actually processes personal data on your behalf. Your vendor's cleaning company is a subcontractor. Your vendor's cleaning company is not a sub-processor, because it never touches your data. The email tool your vendor uses to send you invoices might be a sub-processor, if personal data flows through it.


The distinction matters because GDPR only cares about the third category, and figuring out who's in it requires knowing what a vendor actually does with data - not just that you pay them.


## It was never anyone's job to check


Here's where it usually breaks down. Vendor intake at most startups runs through procurement or finance. Privacy isn't in the room. So when a new vendor gets added, someone approves a contract and a price, but nobody asks: does this thing touch personal data, and if so, whose?


Six months later, you have a vendor list with twenty names on it, and genuinely nobody in the company could tell you which of them are sub-processors under GDPR. Not because anyone was careless - because the question was never anyone's job to ask.


## Why this actually matters


Regulatory enforcement is obviously a risk. But the more immediate problem tends to show up elsewhere first: due diligence. If you're going for an ISO certification, an auditor will ask for your sub-processor list, and "we don't really have one" is a bad answer in that room.


Same thing happens when you're the vendor: if an enterprise customer or investor asks for your sub-processor list during their due diligence, and you hand them three names when the realistic number is closer to fifteen, that doesn't just look incomplete - it makes you look like you don't actually understand your own data flows. That's a credibility problem, and credibility problems slow down or kill deals.


## The DUE test


Run every vendor on your list through three questions - call it the DUE test, since this is exactly what shows up in due diligence:


- **D - Data.** Do they receive personal data from us, or just invoices and contracts?
- **U - Uses.** Do they use anyone else to deliver their service to us?
- **E - Exposure.** If that "anyone else" had a breach, would our customers' data be exposed?


Yes to D and U means you've found a sub-processor, whether it's on your list or not. Run the DUE test across a real vendor list and it's rarely a clean result - most companies find their sub-processor count is higher, and their paperwork thinner, than they assumed going in.


*If you want help running this properly across your vendor list, that's something we can do with you.*
