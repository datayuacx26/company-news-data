---
schema_version: "1.0.0"
document_id: "f33e2aa315f5024bf95065de31a8aa36db818678c69f0b1fa05e968b58419445"
company_key: "yc-porter"
company: "Porter"
source_id: "yc-porter-news-import-d854f87d935e"
canonical_url: "https://www.porter.run/changelog/compliance-with-thoropass-grafana-add-on"
published_at: "2024-09-27T00:00:00+00:00"
first_seen_at: "2026-07-25T19:39:33.596384+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:0dd4cea3069bf6a8c9f69434321c00f15c324afa9469e576aca7eab6fdd164f3"
---

# Compliance with Thoropass and Grafana Add-on

## **Compliance with Thoropass**


Porter has integrated with end-to-end compliance and audit solution[Thoropass](https://thoropass.com/) . Users can achieve one-click SOC 2 compliance for AWS infrastructure that is provisioned and managed by Porter, like EKS and RDS.


## **Grafana Add-on (Analytics & Monitoring)**


[Grafana](https://grafana.com/grafana/) is now available as an add-on. Grafana is an open-source monitoring system, with this add-on allowing users to reap more granular insights from application metrics.


## **Upgraded Databases**


AWS RDS databases provisoned via Porter now default to General Purpose SSD gp3 storage, which is more cost-effective than the AWS default gp2 storage, allowing for configurable IOPS.


- For DBs with a storage size under 400GB, the default configuration is 12,000 IOPS and 125MiBps throughput
- For DBs with a storage size over 400GB, it’s 12,000 IOPS and 500MiBps throughput
