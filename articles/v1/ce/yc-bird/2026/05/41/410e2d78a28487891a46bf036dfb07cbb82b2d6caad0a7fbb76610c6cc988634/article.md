---
schema_version: "1.0.0"
document_id: "410e2d78a28487891a46bf036dfb07cbb82b2d6caad0a7fbb76610c6cc988634"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/what-is-an-email-validation-api"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:684dc86d0d3ffb5a5cc9ed872c6e1174a6dee44901af341240d10de5eb383360"
---

# What Is an Email Validation API?

An email validation API checks whether an email address is well-formed, real, and likely to accept mail, all before you ever send to it. You pass an address, and the API runs a series of checks (syntax, domain, mailbox, risk signals) and returns a verdict. The point is to catch bad addresses at the moment of collection rather than discovering them as bounces later.


## What does an email validation API check?


Validation happens in layers, from cheap and fast to deeper and slower:


- **Syntax.** Is the address structurally valid per the email format rules? This catches typos like a missing` @` or a stray space.
- **Domain and MX.** Does the domain exist, and does it publish MX records? A domain with no mail servers cannot receive mail.
- **Mailbox existence.** An SMTP probe opens a conversation with the receiving server to check whether the specific mailbox accepts mail, without actually delivering a message. Some providers accept everything (catch-all), which limits how certain this step can be.
- **Disposable detection.** Flags throwaway addresses from temporary-inbox services that are common in signup abuse.
- **Role detection.** Flags shared addresses like` info@` or` admin@` that are not tied to one person and often should not receive marketing mail.
- **Typo detection.** Spots likely misspellings (such as` gmial.com` ) and can suggest the correction.
- **Deliverability scoring.** Combines the signals into a single risk score or recommendation so you can decide whether to accept, reject, or flag the address.


## Why validate addresses at all?


Bad addresses are expensive. Every send to a dead mailbox is a[bounce](https://bird.com/en-us/blog/what-are-bounced-emails) , and a high bounce rate tells mailbox providers you are not maintaining your list, which drags down your[sender reputation](https://bird.com/en-us/blog/email-deliverability-best-practices) and pushes more of your legitimate mail toward the spam folder. Validation attacks the problem at the source:


- **Fewer bounces.** Rejecting undeliverable addresses before sending keeps your bounce rate low.
- **Protected reputation.** A clean list signals to providers that you send to engaged, real recipients, which helps keep you out of[the spam folder](https://bird.com/en-us/blog/why-are-my-emails-going-to-spam) .
- **Cleaner signup forms.** Real-time validation at the form catches typos while the user is still on the page, so you capture the address they meant to enter.


## Real-time validation or bulk cleaning?


There are two ways to apply it, and most teams use both.


**Real-time** validation runs at the point of collection, typically a signup or checkout form. You validate the single address as the user submits, surface a typo correction inline, and refuse obviously dead or disposable addresses before they enter your database. This keeps the list clean from day one.


**Bulk** validation runs over a list you already have. If you imported contacts, inherited an old list, or have not mailed a segment in a while, you run the whole list through validation and remove or suppress the addresses that come back risky before your next campaign. This is the cleanup pass for lists that predate real-time checks.


## Frequently asked questions


### Does validation guarantee an email will deliver?


No. Validation strongly reduces the odds of a bounce by removing addresses that are malformed, on dead domains, or clearly fake, but it cannot promise delivery. Mailbox state changes over time, catch-all domains accept everything regardless, and inbox placement still depends on authentication and reputation. Treat validation as risk reduction, not a guarantee.


### What is the difference between validation and verification?


The terms are used interchangeably in most products. Both describe checking an address against syntax, domain, mailbox, and risk signals to judge whether it is safe to send to. If a vendor draws a distinction, read their specific definitions rather than assuming.


### Will an SMTP probe send a test email?


No. The probe opens an SMTP conversation and asks the receiving server whether the mailbox would accept mail, then disconnects before any message is delivered. The recipient sees nothing. That said, catch-all servers accept every address, so the probe cannot always confirm a specific mailbox exists.


To validate addresses before they reach your list, see Bird's[recipient validation](https://bird.com/en-us/products/email/recipient-validation) . Pairing it with sound[deliverability practices](https://bird.com/en-us/blog/email-deliverability-best-practices) is what keeps bounce rates low over time.
