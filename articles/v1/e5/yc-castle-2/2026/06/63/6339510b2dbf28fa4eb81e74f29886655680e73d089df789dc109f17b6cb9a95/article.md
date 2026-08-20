---
schema_version: "1.0.0"
document_id: "6339510b2dbf28fa4eb81e74f29886655680e73d089df789dc109f17b6cb9a95"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/new-event-field-emailregistered_domain"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-27T11:16:58.513640+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:ffc4f940bb671b2fd477be53b375b2fd1a2e0dd15adecf1dee620ff978c5c67d"
---

# New event field: `email.registered_domain`

[Back to All](https://docs.castle.io/changelog)


Added


Castle already provides the` email.domain` event field, reflecting the full domain from the email address. With this release, we're adding` email.registered_domain` , which reduces any subdomain to its registered part using the public suffix list.


Fraudulent domains often rotate across subdomains (` a.domain.com` ,` b.domain.com` , and so on).` registered_domain` collapses those to` domain.com` , so you can match the registration itself instead of chasing individual subdomains. It handles multi-level TLDs correctly (` example.co.uk` stays` example.co.uk` , not` co.uk` ), and since it's derived purely from the email address, it's present even when we've never resolved the domain.


```text
{
"email": {
"address": " [email protected]  ",
"domain": "a.sub.domain.com",
"registered_domain": "domain.com"
}
}
```


As with any email or string field, you can filter on` email.registered_domain` in Explore and build Policies around it, for example blocking a whole registered domain in one rule.
