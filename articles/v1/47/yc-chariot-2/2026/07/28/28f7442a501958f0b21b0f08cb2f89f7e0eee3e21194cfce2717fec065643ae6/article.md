---
schema_version: "1.0.0"
document_id: "28f7442a501958f0b21b0f08cb2f89f7e0eee3e21194cfce2717fec065643ae6"
company_key: "yc-chariot-2"
company: "Chariot"
source_id: "yc-chariot-2-news-import-8ffee98b846e"
canonical_url: "https://www.givechariot.com/resources/insights/the-grants-that-go-nowhere-good"
published_at: null
first_seen_at: "2026-07-21T13:13:53.494154+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:de8259abbc5f6ed6998129918d7e753aba42ab78cb9777d24c92a2933c8f70b2"
---

# The Grants that Go Nowhere Good

I spoke to a Donor Advised Fund (DAF) provider a few months ago who decided to build their own ACH pipeline to send grants to nonprofits. The reasoning is simple - checks are expensive… Like really expensive at $4-20/check.¹ Printing, mailing, signing, reconciliation, reissuing, check returns, finding new addresses, etc. The whole paper trail adds up fast and sucks a lot of the DAF’s team time. Not to mention they are slow, so donors are frequently asking why the nonprofit hasn’t said they got the gift yet.


This specific DAF was already running ACH payments to vendors without too much friction. They paid contractors, lawyers, and others. So the logic followed naturally: nonprofits are just another payee. Let's build the same thing for the thousands of nonprofits we pay too and send them ACH payments.


## Why Grants Are Not Vendor Payments


There was a category error that’s easy to miss if you’re coming at this from a standard finance operations background.


When you run ACH to a vendor (a landlord, a software provider, a contractor) you set up the relationship. You verified the entity when you signed the contract. You initiated the banking details yourself. If something changes, you find out because you’re the customer. The payment is a pull arrangement you and the vendor control end to end. The vendor is “ **pulling** ” the money from you.


Grants are structurally different. You’re “ **pushing”** money to organizations you’ve often never met, based on banking information they submitted to you. You need to send those gifts with key donor details *(which creates a lot of*[other issues for nonprofits](https://www.givechariot.com/resources/insights/all-checks-no-balance-a-report-on-the-state-of-check-payments-to-nonprofits) *)* and with usually no notice the gift is on its way to the nonprofit. The DAF is the sender, not the customer. And senders don’t have the same natural checkpoints as a customer. That asymmetry makes it an easy target for fraud.


*I’ve written about the differnce between these pull and push payments in more depth*[here](https://aaronkahane.substack.com/p/why-nonprofits-struggle-to-get-paid) *.*


## The Granting Mistake


After the DAF set up these payments, some of their larger nonprofits (~10-20%) started submitting their banking details. Someone on the DAFs staff checked them against[LSEG’s access to the EWS (Early Warning Services)](https://www.lseg.com/en/risk-intelligence/account-verification-solutions) - a consortium database based on fuzzy name matching for verification. If they weren’t able to find the nonprofit in that database, the DAF used a voided check or a bank statement to verify the nonprofit.


The payments started going out. Tens of thousands of grants cycling through a system built on documents and name matching.


Then someone posed as a nonprofit officer, submitting fraudulent banking information, and a major grant ended up somewhere it was never supposed to go. Recovery took months, the donor was furious, and the DAF ended up rolling back their whole ACH system.


It would be easy to treat this as an edge case. The problem is it isn’t. Check² and ACH fraud³ have been roughly doubling every two years. Advances in AI have made forged documents and fabricated identities faster and cheaper. The FBI has flagged it.⁴


What makes DAFs especially exposed is a few things working together in a perfect storm:


1. Gifts often arrive without any advance notice to the nonprofit, which gives a fraudster a (potentially indefinite) window before anyone starts asking questions.
2. Many donors contributed to their DAF months or years earlier, so they’re not actively monitoring for confirmation of gift receipts for tax reasons.
3. Nonprofits usually[don’t thank DAF donors](https://www.givechariot.com/resources/insights/daf-gift-processing-why-it-matters-how-to-get-it-right) because of the limited information which means after the gift is initiated and sent to the bad actor know one checks in.


I recently watched *Catch Me If You Can* which reminded me of this problem. But I think Frank Abagnale needed a lot more skill than if he was working with DAF grant payments.


## The Bucket Is Leaking Before Anyone Touches It


There’s a second problem that doesn’t involve fraud at all, and it doesn’t get talked about enough.


Bank details go stale. People leave organizations. Accounts get restructured or closed. A nonprofit you verified eighteen months ago might be operating with completely different banking information today and nobody told you. One other DAF I spoke to had been manually re-verifying nonprofit banking details every quarter - hundreds of staff hours annually chasing down updated routing numbers, trying to determine whether a voided check or LSEG check was still current.


That’s a design problem. If there's no ongoing monitoring baked into how you've built this, you're always operating on information that's already aging. Most platforms in this space don't have it. Verification happens at onboarding and then the assumption is things are fine until something breaks.


## The Real Problem Is Identity


Here’s the thing that took me a while to fully articulate: routing numbers and account numbers are not identity. They’re coordinates or pointers where funds should go.


Most of the grants and DAF space treat these as equivalent. They're not, and that conflation is where the exposure lives.


The most common approach is fuzzy name matching - tools like LSEG or Plaid that check whether the name on the bank account roughly corresponds to the name of the nonprofit. It sounds reasonable until you think about it for a second. "American Cancer Society" and "Amer. Cancer Soc." might match. So might a fraudster who knows enough to name their bank account entity something similar. Fuzzy matching is pattern recognition on strings of text. It has no idea whether the organization is real, whether it's in good standing, or whether the person submitting the information has any authority to do so. It's like if Microsoft Word’s spellchecker did the job of a background check.


The step up from fuzzy matching is standard KYC (Know Your Customer) which is what banks are legally required to do and what tools like PayPal or Stripe implement. Name, SSN, date of birth, address, checked against private databases like LexisNexis. It confirms a real person exists behind the application. It catches straightforward fraud.


But standard KYC has two big caveats to its effectiveness against fraud:


1. First, it doesn’t catch the sophisticated attack. The fraudster who isn’t using a made-up identity, but a real one they have no right to use. Stolen credentials from a data breach. A synthetic identity stitched together from real and fabricated elements. Someone who clears every database check because technically they do.
2. The second is subtler but arguably more important in the DAF/grants context: standard KYC confirms a person is real, but it doesn’t confirm that the real person has any authority to act on behalf of the organization receiving the money. Those are two different questions. You can verify an org is a legitimate 501(c)(3) and verify that the person opening the account has an SSN, but if nothing connects those two things, you haven’t actually verified anything meaningful about the relationship between them. A bad actor who knows enough about an organization could pass both checks independently while having no legitimate claim to represent either one.


That gap, between verifying identity and verifying authority, is where a lot of the exposure in this space actually lives. And it’s where we’ve spent a lot of time at Chariot.


## [Chariot’s New Identity and Authority Model](https://www.givechariot.com/disbursements)


Like other tools in this space we start with standard KYC, SSN checked against private databases like LexisNexis. But we also verify that the individual is actually an authorized officer of the organization they’re claiming to represent through IRS data. And underneath that is a layer of enhanced checks that’s worth understanding, because this is where the sophisticated attacks actually get caught. Some examples below:


1. **Synthetic identities** are a good example of where standard KYC falls short. A synthetic identity combines real elements - often a legitimate SSN sourced from a data breach, sometimes from a child or someone with a thin credit file - with fabricated supporting details like a name or date of birth. On a standard check it can pass. We use models that analyze correlations between identity elements, patterns in credit history, and consistency across bureaus. A real person has a coherent trail that builds over time. A synthetic identity has seams where the pieces don’t quite fit. We look for the seams.
2. **Identity theft scoring** addresses a different scenario - where the credentials are real and the name matches, but the person submitting isn’t who they claim to be. Stolen credentials from a phishing attack, or from one of the many data breaches that have quietly put millions of SSNs into circulation. The score is built from mismatches: the identity elements check out, but behavioral signals don’t. Velocity is off. The way the form is being completed doesn’t match how the actual account holder typically behaves.
3. **Device risk screening** works at a different layer entirely. Devices leave fingerprints. A device associated with previous fraud attempts, routing through a VPN or proxy to mask its location, running through an emulator instead of a real phone - these are important signals worth paying attention to. Cross-referenced against known fraud device databases, they create a risk picture before a single document has been reviewed. Geography matters here too. If someone claims to be an authorized officer at a nonprofit in Chicago and the device is onboarding from a datacenter exit node somewhere else, that’s information we can act on.
4. **Behavioral signals during onboarding** are subtler but often where fraud rings actually get caught. Typing hesitancy. Copy-pasting into fields like SSN or date of birth rather than typing them - a pattern consistent with someone working from a list of stolen credentials, not someone entering their own information. Devices on networks with known bad reputations. None of these signals individually constitute proof of anything. But together, they paint a picture of whether the person on the other end of the form is who they’re presenting as.


Most applications don’t trigger any of this. The system runs quietly and grants flow normally. The value is in what happens when something does - caught before the money moves, not months later when someone files a complaint.


## The Infrastructure Underneath the Payment


Sending an ACH is more or less a solved problem. What isn’t solved in the nonprofit space and what most of the grants space is still handling through manual re-verification cycles, fuzzy name matching, and a reasonable amount of “pointer” trust - is the identity layer underneath it.


The grantmakers who scale confidently are the ones who’ve stopped treating verification as an onboarding checkbox and started treating it as ongoing infrastructure. Not just: did this organization pass a review once? But: is this still the right person, at the right organization, with the right authority, today?


The payment type is more straightforward. Everything that makes sure it lands where it’s supposed to - that’s what determines whether you can run this operation at scale or whether you’re always one convincing document away from a very big bad payment that doesn’t land in a nonprofit’s bank account.


¹ Bank of America says somewhere between[$4-$20/check](https://www.wsj.com/articles/SB10001424052702304732804579425233344430424?gaa_at=eafs&gaa_n=AWEtsqe4sPVVbJfh_l_9QFZhkG4-AzbDdJzAfoBh15kb_dOIbJ65EjeMSdwMuNJbHy8%3D&gaa_ts=69b713d1&gaa_sig=Eu4IYaZQcWdyHzrQLgiSWgdb7z-6TaqAqDhZxqNivBLk0u-LgD5eaHsa3q_l-gOQm139ZMh7uEdaOSB7NbxPKA%3D%3D) depending on labor costs of issuing, check returns, etc.


²[FBI & USPIS Public Service Announcement.](https://www.ic3.gov/PSA/2025/PSA250127)


³[Ohio Society of Association Professionals News Article.](https://associationdatabase.com/aws/OSAP/pt/sd/news_article/584997/_blank/layout_details/false)


⁴[FBI Public Service Announcement: Mail Theft-Related Check Fraud is on the Rise.](https://www.fbi.gov/investigate/cyber/alerts/2025/mail-theft-related-check-fraud-is-on-the-rise)
