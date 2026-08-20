---
schema_version: "1.0.0"
document_id: "1e3e3ff612063d096602c18c4fff2e3a03a4c9fc29f214e461cea2d57eb9396e"
company_key: "yc-taloflow"
company: "Taloflow"
source_id: "yc-taloflow-rss-cfae8c512c9e"
canonical_url: "https://www.taloflow.ai/blog/cloudflare-r2-vs-amazon-s3"
published_at: "2021-10-06T13:20:00+00:00"
first_seen_at: "2026-07-26T01:23:39.915767+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:3d369e9c58cc2b646a65022cb01295502582c3383797b3367f1463becbe08196"
---

# Cloudflare R2 vs Amazon S3

[Cloudflare R2](https://blog.cloudflare.com/introducing-r2-object-storage/) is a new[cloud object storage](https://www.taloflow.ai/cloud-object-storage) provider with an eye towards stealing market share from Amazon S3 by offering cheaper object storage that is S3-compatible. This value proposition resonates with many developers because Amazon S3, while powerful and deeply embedded with the rest of the AWS ecosystem, is expensive to use. AWS has also not made any price reductions for Amazon S3 since 2016, whereas some of its other staple services have seen significant price reductions.


At $15/TB[Cloudflare R2](https://www.taloflow.ai/blog/will-cloudflare-r2-win-customers-from-amazon-s3) is not as cheap as other alternatives — e.g. Backblaze B2, Wasabi and Storj DCS — but it does come in significantly cheaper than Amazon S3 Standard Storage, which ranges between $21/TB and $23/TB depending on your storage volume.


In addition, Cloudflare is saying that once launched, R2 will be significantly cheaper or more flexible on egress fees (they’re saving “free”) and the cost of read/write operations. This is where Amazon S3 gets dinged a lot: its operations and data transfer costs are not only expensive, but also notoriously complex to understand and optimize across zones, regions, tiers, and so on.


As far as developer experience goes, Cloudflare has been a darling in the developer community for over a decade now thanks to a comparatively easy to use console to AWS, low threshold of time/effort to getting up and running, and good performance across the board — its CDN RUM/uptime metrics are generally within a percentage point of Amazon’s Cloudfront CDN.


## Should You Choose Cloudflare R2 over Amazon S3?


**We don’t know everything yet.** R2 is still in early access, but nonetheless we’ll try to help you decide between Cloudflare R2 and Amazon S3. I’m going to cover the following:


1. Where Cloudflare R2 will excel over Amazon S3
2. Where Amazon S3 will be better than Cloudflare R2


### Where Cloudflare R2 will excel over Amazon S3


Cloudflare R2 will most likely excel over Amazon S3 in the following use cases:


- Does not involve backup and recovery.
- Does not involve archival and compliance.
- Is not deeply embedded with the rest of the AWS ecosystem.


### Where Amazon S3 will be better than Cloudflare R2


Amazon S3 will most likely excel over Cloudflare R2 in the following use cases:


- Backup and recovery because of its Amazon S3 Infrequent Access and Glacier tiers.
- Archival and compliance because of the extremely cheap (yet slow to access) Amazon S3 Glacier Deep Archive tier.
- Is deeply embedded with the rest of the AWS ecosystem. For example, you’re a heavy user of Amazon Athena.


### Shortlist cloud object storage vendors based on your use case without hours of soul sucking research


It takes five minutes to get your free, accurate recommendation


[Get my free recommendation](https://use.taloflow.ai/guide)
