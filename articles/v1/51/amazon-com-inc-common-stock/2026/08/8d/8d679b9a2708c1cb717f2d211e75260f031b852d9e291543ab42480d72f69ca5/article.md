---
schema_version: "1.0.0"
document_id: "8d679b9a2708c1cb717f2d211e75260f031b852d9e291543ab42480d72f69ca5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-drs-linux-uefi"
published_at: "2026-08-10T13:00:00+00:00"
first_seen_at: "2026-08-10T14:36:01.988627+00:00"
fetched_at: "2026-08-10T14:36:04.420697+00:00"
content_hash: "sha256:55a37c0a8dba81cfdb110c16fb69440a12444cdd5c0c64a14e3af666079fe7ee"
---

# AWS Elastic Disaster Recovery now preserves UEFI boot mode for Linux servers

AWS Elastic Disaster Recovery (AWS DRS) now preserves UEFI boot mode when recovering Linux source servers that boot with UEFI firmware. Previously, DRS launched these Linux servers in legacy BIOS mode, which could require extra configuration after recovery. Now your recovered Linux instances launch with the same UEFI boot mode as your source servers. This means your recovery instances more closely match your source environment, so applications that depend on UEFI boot behavior come back exactly as you expect — with no additional post-recovery steps. Boot mode preservation is automatic, with nothing to configure.


This capability is available in all AWS Regions where AWS DRS is offered, at no additional cost. To learn more, visit the[AWS Elastic Disaster Recovery User Guide](https://docs.aws.amazon.com/drs/latest/userguide/what-is-drs.html) .
