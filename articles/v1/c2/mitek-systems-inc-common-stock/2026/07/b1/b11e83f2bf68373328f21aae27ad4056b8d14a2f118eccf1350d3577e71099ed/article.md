---
schema_version: "1.0.0"
document_id: "b11e83f2bf68373328f21aae27ad4056b8d14a2f118eccf1350d3577e71099ed"
company_key: "mitek-systems-inc-common-stock"
company: "Mitek Systems Inc."
source_id: "mitek-systems-inc-common-stock-rss-3bf79578716e"
canonical_url: "https://www.miteksystems.com/blog/what-is-check-kiting-how-this-fraud-scheme-works-and-why-it-still-happens"
published_at: "2026-07-02T12:30:00+00:00"
first_seen_at: "2026-07-20T23:21:50.456876+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:a4972737356fa900ccff088efa82bbb9bbdf54e5bb9bcc23cbb1a0bd2f00ea6f"
---

# What is check kiting? How this fraud scheme works and why it still happens

Check kiting has been around about as long as checks themselves. And despite everything the industry has done – AI-powered monitoring, real-time payment rails, smarter onboarding controls – it’s still showing up in fraud queues at banks, credit unions and fintechs. If you’re on a risk or fraud team, understanding how this scheme works and why it keeps working is foundational to building defenses that actually hold.


## Quick answer: What is check kiting?


At its core, check kiting is pretty simple. A fraudster writes a check from an account with no money in it, deposits it into another account they control, and pulls the funds out before the bank figures out the check is going to bounce. The whole scheme runs on one thing: the float – that window between when a check is deposited and when it’s actually verified.


## How check kiting works (step by step explanation)


Unlike account takeover fraud where someone has to steal credentials, or check washing where a physical document needs to be intercepted and altered, kiting doesn’t require much. Two accounts and good timing. That’s it.


Here’s how a basic scheme plays out:


1. The fraudster writes a check from Account A, which has insufficient funds to cover it.
2. They deposit that check into Account B, typically at a different institution.
3. The institution holding Account B credits the funds as provisionally available – standard practice under their funds availability policy.
4. The fraudster immediately withdraws or transfers those funds out of Account B (as they’re made available).
5. When the check from Account A bounces, the bank that released the funds from Account B eats the loss.


In practice, it might look like this: someone opens checking accounts at two regional banks, puts a small amount in each, then writes a $9,500 check from Account A and mobile deposits it into Account B. The next day, under Reg CC, a portion of those funds are available. They withdraw it as cash. By the time Account A’s institution returns the check unpaid, the money is gone.


## Why check kiting works: Understanding the “float”


The "float" is the gap between deposit and final settlement. During that window, the receiving bank has extended provisional credit without being able to confirm the check is actually good. That’s not negligence – it’s just how the check clearing process works. Even with improvements like Check 21, which enabled image exchange and sped up processing, deposits crossing between institutions still move through clearing houses and correspondent relationships before they’re truly settled. And Reg CC requires banks to make funds available on a defined timeline regardless.


Digital banking has compressed the float, but it hasn’t closed it. Whether a check comes in through mobile deposit, an ATM, or a teller window, it generates the same provisional credit sitting on the same regulatory framework. If anything, mobile deposit has made kiting easier – fraudsters can now submit check images across multiple institutions within minutes, exploiting the gap between siloed systems at scale that wasn’t possible before.


## Is check kiting illegal?


Yes – it’s bank fraud under federal law, prosecutable under 18 U.S.C. § 1344. We’re talking potential prison time, restitution orders, and fines. On top of that, institutions report this activity to systems which will effectively blacklist perpetrators from the banking system. Financial institutions experiencing losses file Suspicious Activity Reports with FinCEN.


## Check kiting vs check fraud: What’s the difference?


Check fraud is the broad category – any deceptive activity involving a paper check or imaged check. Check kiting is one specific type that exploits float across multiple accounts. Here’s how it sits alongside some of the other schemes we see regularly:


This table provides several examples from the check fraud ecosystem:


Scheme What it exploits


Check kiting The float between deposit and clearing, across two or more accounts


Forged checks An unauthorized signature or endorsement


Altered checks A check where the payee, amount, and/or another field has been modified


Counterfeit checks Entirely fabricated instruments, sometimes AI-generated


## Real-world example of check kiting


The two-account scheme I described above is the textbook version. But check kiting doesn’t always start with malicious intent.


Consider a small business owner with accounts at two banks. Cash flow gets tight. To buy some time, they write a $25,000 check from Bank A, deposit it at Bank B, and write $20,000 to a vendor. A couple days later, before Bank A returns it unpaid, they write a $30,000 check from Bank B back to Bank A to cover it. When things don’t improve, the cycle continues – and each round has to be bigger to cover the last one. Eventually one back catches the circular activity. By then, losses can easily be six figures. What started as a cash flow stopgap is now a federal crime.


This is one of the more tragic patterns we see in practice. The intent matters legally, but the loss is real either way.


## How banks detect check kiting


No single red flag catches kiting – it’s pattern recognition that does the work. Fraud teams are looking for:


- High-velocity deposits and withdrawals between a set of accounts, especially when those accounts share signers, addresses, or device fingerprints
- Circular money movement – funds running in a closed loop across institutions


- Large deposits followed by immediate or next-day withdrawals before items can clear


AI-powered transaction monitoring and analysis, as well as consortium-based intelligence sharing, make it possible to detect these patterns with increasing accuracy. For example, Mitek’s Check Fraud Defender can analyze deposits across ATMs, teller windows, and mobile deposit capture channels in real time, combining sophisticated analysis with data from a consortium of over 8,300 financial institutions to incorporate cross-channel signals that inform decisioning in real time.


## Warning signs to watch for


A few patterns consistently surface when institutions are monitoring for kiting:


- Activity that doesn’t match a customer’s profile – an individual account suddenly moving large sums between institutions
- Repeatedly bouncing checks, especially larger ones, especially between accounts with some sort of link
- Customer pressure to release holds – calls to the branch, urgency, pushback
- New accounts with high transaction volume right from the start
- Duplicate presentments across mobile and physical channels


The common thread is money moving faster and in more circular patterns than makes sense. Urgency + large transactions + circular flow + unexpected behavior = a combo that is worth investigating.


## How to protect yourself from check kiting


The fundamentals here are pretty straightforward:


- Hold funds close to Reg CC minimums for new accounts or accounts showing behavioral red flags
- Monitor velocity and circular patterns daily, with particular focus on accounts with common ownership
- Implementing duplicate detection that catches the same check being presented multiple times – across channels at your institution, and across institutions through consortium sharing


## Why check kiting still matters today


Check volumes are down significantly from their peak. But check fraud – including kiting – has gone the other direction. FinCEN data shows financial institutions filed more than 350,000 check fraud SARs in 2021, with volume more than tripling between 2018 and 2022. The U.S. Treasury has reported a post-pandemic increase in check fraud of over 380%. In a 2025 industry survey, nearly two-thirds of institutions reported experiencing check fraud in the prior year, with it accounting for roughly 30% of overall fraud losses – second only to card fraud.


Checks aren’t going away. Business-to-business payments and government disbursements still rely heavily on them. Wherever checks exist, float exists. And fraudsters know exactly how to use it.


## Key takeaways


Check kiting is the deliberate exploitation of float to extract funds that don’t exist. It’s federal bank fraud with serious consequences. And despite the broader decline in check usage, it’s surging – because wherever checks exist, the float exists, and fraudsters will always find ways to exploit it.


Effective defense requires cross-channel detection that can identity circular patterns and unexpected money movement before the loss is realized. Closing the gaps fraudsters exploit means seeing across channels and institutions – not just within your own walls.
