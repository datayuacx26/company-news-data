---
schema_version: "1.0.0"
document_id: "c2eb58e547028907fce376ac923c8dc67e3fb9d8f0006404d7d095e082217cde"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/fraudulent-email-domain-signal"
published_at: "2025-10-31T00:00:00+00:00"
first_seen_at: "2026-07-21T12:42:16.969450+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:c7d336d4f51bedf32e0282c02737d8778f78e5f74cc56af43e15fb3d156c1b3b"
---

# Fraudulent Email Domain signal

[Back to All](https://docs.castle.io/changelog)


Added


Castle now detects email domains that have been confirmed as actively used in fraud and abuse campaigns. Unlike disposable email services (Mailinator, TempMail), these are domains registered specifically for fraud that appear legitimate but exist solely for bot signups and fake accounts.


- **High confidence** : Flagged domains remain fraudulent. Permanent state like Disposable Email Domain.
- **Research-backed** : Identified through Castle's network-wide behavioral analysis and manual review
- **Attack-focused** : Catches coordinated campaigns, not just suspicious patterns


###


Example response


```text
{
"policy": {
"action": "deny",
...
},
"signals": ["fraudulent_email_domain"],
"email": {
"address": " [email protected]  ",
"domain": "nemomo.org"
...
}
...
}
```


###


Block with confidence


Use` Fraudulent email domain` in your Policies to confidently block confirmed fraud domains without false positives.


###


Spot attack patterns


Filter by` Fraudulent email domain` in Explore to quickly identify and analyze coordinated fraud campaigns
