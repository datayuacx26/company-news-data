---
schema_version: "1.0.0"
document_id: "c072fe0ddebc8a2bf060c63aa8d73b8d9d3899d95e2a356d4841ddb816135ed5"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/7/amazon-cloudwatch-logs-insights-ql/"
published_at: "2026-07-15T21:56:00+00:00"
first_seen_at: "2026-07-25T01:09:33.185736+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:f9065917f9ca6ff7a623dfa4689a869dddf364ebd12b6fae9a3f281eb1d29834"
---

# Amazon CloudWatch Logs Insights adds 25 new query commands and functions

Amazon CloudWatch Logs Insights query language now supports 25 new commands and functions that expand your ability to query, transform, correlate, and analyze logs. Customers analyzing logs in CloudWatch Logs Insights often need to perform statistical aggregation, handle null values in time-series data, compare logs across time windows, detect outliers, and enrich events with lookup data.


With this launch, CloudWatch Logs Insights adds type conversion and encoding functions (hexToAscii, hexToDec, decToHex), date and time functions (parseDate, formatDate, queryStartTime, queryEndTime, queryTimeRange), string functions (messageSize), JSON inspection functions (jsonArraySize, jsonArrayContains), and a conditional validation function (isNumeric). It also introduces statistical commands (variance, topk, countFrequent), row-sequencing and null-handling commands (autoregress, accum, filldown, fillmissing), sessionization and time-comparison commands (sessionize, logcompare), a data analysis command (outlier), query-composition and join commands (where, appendcols), and a lookup enrichment command (cidrlookup).


These commands and functions are available today in all commercial AWS Regions. To learn more, see the Amazon CloudWatch Logs[documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_AnalyzeLogData_LogsInsights.html) .
