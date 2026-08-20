---
schema_version: "1.0.0"
document_id: "af43749fc9d383e02b9ff6730557982071353d76d3305eefac9c87f28e79bb35"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-clean-rooms-minimum-aggregation-custom-analysis-rules"
published_at: "2026-08-13T12:00:00+00:00"
first_seen_at: "2026-08-14T00:27:41.812190+00:00"
fetched_at: "2026-08-14T00:27:44.909056+00:00"
content_hash: "sha256:ec73c2664ba1bfeb2d60642f143fc65f460aa3ed1f94a4cdf6b1eccc7577e2bb"
---

# AWS Clean Rooms supports minimum aggregation thresholds in custom analysis rules

AWS Clean Rooms now supports[minimum aggregation thresholds for the Custom analysis rule type](https://docs.aws.amazon.com/clean-rooms/latest/userguide/analysis-rules-custom.html) . Minimum aggregation helps protect the privacy of individual data subjects by preventing queries from returning results about individuals or small groups. With this launch, organizations can enforce minimum aggregation on custom SQL queries, ensuring that every row a query outputs represents at least the specified number of distinct values (e.g., user IDs). Data providers in a collaboration can specify their identity column and a minimum identity count to enforce on a query’s output, with the option to set a higher threshold for specific columns.


Previously, enforcing minimum aggregation thresholds on custom SQL required data providers to rely on pre-approved analysis templates and manual code reviews before queries could run. Now, data providers can configure the minimum aggregation threshold for custom SQL using the Custom analysis rule type, without using pre-structured queries or manual approval processes. Additionally, data providers can specify which columns can be filtered or joined across datasets. For example, a publisher collaborating with an advertiser for media planning use cases can enable ad-hoc queries to run on their data—and small, rural zip codes with fewer than 1,000 common users can be automatically filtered out from the result to help protect user privacy.


AWS Clean Rooms helps companies and their partners easily analyze and collaborate on their collective datasets without revealing or copying one another’s underlying data. For more information about the AWS Regions where AWS Clean Rooms is available, see the[AWS Regions](https://docs.aws.amazon.com/general/latest/gr/clean-rooms.html#clean-rooms_region) table. To learn more about collaborating with AWS Clean Rooms, visit[AWS Clean Rooms](https://aws.amazon.com/clean-rooms/) .
