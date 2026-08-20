---
schema_version: "1.0.0"
document_id: "678980db13b34386de4c7b218bf01d9b9ab14761ce61ee0e983313d5e4d262be"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/what-is-dmarc"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:e3b5c929b5eb788996de8ea03afb63c2a9a220743f9fcdc1f05f37dee5569b07"
---

# What is DMARC?

DMARC is one of those things that sounds intimidating and turns out to be pretty reasonable once someone walks you through it. It stands for Domain-based Message Authentication, Reporting and Conformance, and it does two jobs: it tells receiving mail servers how to check that a message claiming to be from your domain is genuinely from you, and it tells them what to do when that check fails.


## What does DMARC stand for?


The acronym unpacks into the three things it actually does. "Domain-based Message Authentication" is the check itself, tied to the domain in your visible From address. "Reporting" is the part people underrate: receivers send you data about who is sending mail under your name. "Conformance" is the policy you publish, the instruction that says how strictly a failing message should be handled.


DMARC is not an authentication method on its own. It sits on top of two protocols you may already have in place. SPF says which servers are allowed to send mail for your domain, and DKIM adds a cryptographic signature that proves a message wasn't altered in transit. DMARC is the policy layer that ties them to the domain your recipients actually see. If you want the side-by-side,[SPF vs DKIM vs DMARC](https://bird.com/en-us/blog/spf-vs-dkim-vs-dmarc) breaks down how the three fit together.


## Why does DMARC exist?


Plain SMTP, the protocol that moves email around, never had a way to verify who a message is really from. Anyone can put your domain in the From field, which is exactly how phishing and brand spoofing work. SPF and DKIM each closed part of the gap, but they check domains the recipient never sees (the Return-Path for SPF, the signing domain for DKIM). A scammer could pass both while still showing your brand in the From line.


DMARC closes that last gap with a concept called alignment: the authenticated domain has to match the domain in the visible From address. Then it adds the reporting loop so you can see what is happening. For the full mechanics, see[how DMARC works](https://bird.com/en-us/blog/how-does-dmarc-work) .


## What does DMARC actually do for you?


Three concrete things, in the order most people notice them.


It stops look-alike abuse of your exact domain. Once you publish an enforcing policy, a message that fails authentication and claims to be from your domain gets quarantined or rejected at the receiver, before it reaches your customer.


It gives you visibility. The reports show every source sending mail under your domain, legitimate or not. The reporting is the underrated benefit: most teams discover a forgotten newsletter tool or a regional office sending real mail they never knew about. Learning to[read a DMARC report](https://bird.com/en-us/blog/how-to-read-a-dmarc-report) is where the value compounds.


It helps your legitimate mail land. Mailbox providers treat authenticated, aligned domains as more trustworthy, which supports[inbox placement](https://bird.com/en-us/products/email/deliverability) for the mail you want delivered.


## How do you start without breaking anything?


You start in monitor mode. A DMARC[policy](https://bird.com/en-us/blog/what-is-a-dmarc-policy) of` p=none` changes nothing about how your mail is handled, it just turns on the reports. You watch the data for a few weeks, confirm every legitimate sender is passing, then tighten to quarantine and eventually reject. That staged ramp is the whole point: you move at your own pace instead of flipping a risky switch on day one.


Setting it up is a single DNS[TXT record](https://bird.com/en-us/blog/what-is-a-dmarc-record) published at a specific name on your domain. If you want the click-by-click version,[how to set up DMARC](https://bird.com/en-us/blog/how-to-set-up-dmarc) covers it, including the cPanel steps.


## Do you actually need it?


If you send any real volume, increasingly yes. Since February 2024, Google and Yahoo require a DMARC record for anyone sending more than 5,000 messages a day to their users. Beyond the requirement, any domain that customers recognize is a spoofing target worth protecting. The fuller answer, including who can stay in monitor mode, is in[do I need DMARC](https://bird.com/en-us/blog/do-i-need-dmarc) .


If you send through Bird, DKIM and SPF alignment come from the records you publish when you add your sending domain, so the work that remains is mostly the DMARC record and reading what comes back. You can also dig into the technical guide on[SPF, DKIM, and DMARC](https://bird.com/en-us/docs/guides/email/dkim-spf-dmarc) in the docs.


DMARC rewards patience. Publish the record, read the reports, tighten the policy when the data says it's safe. That's the entire arc, and it's a good idea.
