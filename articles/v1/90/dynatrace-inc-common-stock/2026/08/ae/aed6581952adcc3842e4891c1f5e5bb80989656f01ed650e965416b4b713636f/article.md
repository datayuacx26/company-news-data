---
schema_version: "1.0.0"
document_id: "aed6581952adcc3842e4891c1f5e5bb80989656f01ed650e965416b4b713636f"
company_key: "dynatrace-inc-common-stock"
company: "Dynatrace Inc."
source_id: "dynatrace-inc-common-stock-rss-2f172b160f47"
canonical_url: "https://www.dynatrace.com/news/blog/transforming-any-log-source-into-actionable-insights-with-dynatrace-openpipeline/"
published_at: "2026-08-05T16:09:51+00:00"
first_seen_at: "2026-08-05T16:32:46.861881+00:00"
fetched_at: "2026-08-05T16:32:47.965797+00:00"
content_hash: "sha256:d6b57cab27835e572b29067e466f4609ec8434b4efa95c9160244e03078491ff"
---

# Transforming any log source into actionable insights with Dynatrace OpenPipeline

When an incident hits, nobody wants to spend precious minutes scrolling through raw logs looking for clues.


Yet that’s exactly where many SRE and IT Operations teams find themselves. Logs are everywhere—application logs, infrastructure logs, web server logs, cloud services logs, and custom applications logs. Most teams already have plenty of logs. The challenge is turning those logs into something your teams can use.


## The hidden cost of unstructured logs


Logs are one of the richest sources of operational insight, but they’re only valuable if you can query, filter, and correlate them effectively.


Structured logs make this easy. Key information, such as log level, exception type, request path, HTTP status code, or user identity, is already separated into consistent fields. You can immediately filter, alert, dashboard, and investigate.


However, unstructured logs tell a different story.


Instead of discrete fields, everything is packed into a single,[often very large text string](https://www.dynatrace.com/news/blog/go-big-with-dynatrace-native-support-for-large-log-records/) . The information you need is technically there but buried inside a content field that requires manual interpretation every time you investigate an issue.


Consider a common troubleshooting scenario:


An application starts returning errors. You open your logs and see hundreds of records. The exception type, error message, status code, request path, and user information all exist somewhere in the log line, but none of them are searchable fields.


To find what matters, you need to manually inspect log entries, build custom queries, or write parsing logic on the fly.


At scale, this becomes a major operational bottleneck.


The issue isn’t the volume of logs. It’s the lack of structure.


## Why traditional log parsing doesn’t scale


Most operations teams understand the value of structured logs. The challenge is getting there.


In many environments, log formats are inconsistent. Legacy applications produce raw text. Third-party systems generate logs that don’t follow internal standards.


The typical solution is to write custom parsing rules. And that works for a while.


But as the number of applications and log sources grows, maintaining parsing rules becomes a significant operational burden. Teams end up spending time managing log formats instead of solving problems.


What they need is a way to automatically normalize and process logs before they’re stored and analyzed.


## Structuring logs at ingestion with OpenPipeline


[Dynatrace OpenPipeline](https://www.dynatrace.com/platform/openpipeline/) addresses this challenge by processing data as it flows into the platform.


Rather than waiting until query time to interpret log content, OpenPipeline can normalize, enrich, and structure data during ingestion. The result is consistent, searchable log data that’s ready for troubleshooting, alerting, and analytics from the moment it arrives.


[OpenPipeline supports data](https://docs.dynatrace.com/docs/platform/openpipeline) from multiple sources and formats, enabling organizations to build a unified log-processing strategy without introducing additional tools or workflows.


For practitioners, this means less time wrestling with raw data and more time identifying root causes.


## When your logs are already structured: Use technology bundles


Not every log source requires custom parsing.


Many technologies already produce logs in predictable formats. The challenge is mapping those formats into standardized fields that can be used consistently across the observability platform.


This is where **technology bundles** come in.


Technology bundles are predefined libraries of parsers that structure technology-specific logs in accordance with the[Dynatrace semantic dictionary](https://docs.dynatrace.com/docs/semantic-dictionary) . The semantic dictionary provides standardized field names that are used consistently across logs, spans, metrics, and entities.


For example, when Java application logs flow through OpenPipeline, teams can simply apply the Java Technology Bundle.


Dynatrace automatically extracts fields such as:


- Log level
- Message content
- Exception type
- Exception message
- Additional application-specific metadata


Before processing, all of this information may exist only within a single content field.


After processing, each element becomes a separate, queryable field.


That means teams can:


- Filter on specific exception types
- Create dashboards for recurring errors
- Trigger alerts based on exception messages
- Correlate log data with other observability signals


Most importantly, the technology bundles eliminate the need to build and maintain custom parsing logic for common technologies.


See how it works in this short demo:


## When your logs aren’t structured: Build once, automate forever


What about truly unstructured data?


Consider a common example: Nginx access logs.


Without processing, an Nginx log arrives as a single text string containing information such as:


- Client IP address
- User identity
- Timestamp
- HTTP method
- Request path
- HTTP version
- Status code
- Bytes transferred


The data exists, but none of it is available as separate fields.


As a result, teams can’t easily:


- Filter for failed requests
- Create dashboards for HTTP errors
- Alert on traffic anomalies
- Analyze client behavior


OpenPipeline makes it possible to transform that raw text into structured data.


A practical workflow without using a technology bundle looks like this:


1. Select a sample log record.
2. Prototype extraction logic directly against live data.
3. Use Dynatrace Pattern Language (DPL) to identify and capture the fields that matter.
4. Validate the extracted results immediately against your data before deploying
5. Copy the completed parsing rule into OpenPipeline.


A detailed step-by-step guide with examples is provided in our blog post “How to mask PII like email addresses appearing in logs with Dynatrace.”
[Read blog](https://www.dynatrace.com/news/blog/how-to-mask-pii-like-email-addresses-appearing-in-logs-with-dynatrace-an-advanced-use-case/)


Once a rule in OpenPipeline is deployed, every new log that enters the pipeline is processed automatically.


Parsing happens once at ingestion rather than repeatedly at query time. The rule is then applied automatically to incoming logs, with updates only required if the source log format changes.


The fields become permanently available for downstream use across Dynatrace.


See how it works in this short demo:


## Turning parsed logs into operational outcomes


The real value isn’t the parsing itself. The value comes from what becomes possible afterward.


Once fields like status code, request path, or exception type are available as structured attributes, teams can build workflows around them.


For example:


- Faster investigation –[Filter instantly on HTTP 500 errors](https://docs.dynatrace.com/docs/analyze-explore-automate/logs/lma-logs-app) instead of searching raw text.
- Better alerting –[Create alerts based on specific exception types](https://docs.dynatrace.com/docs/analyze-explore-automate/logs/lma-use-cases/lma-alert-log-based-events#expand--Open-problem-in-Problems--3) or response codes.
- Meaningful dashboards – Track error rates, request patterns, and operational trends using[visual dashboards](https://docs.dynatrace.com/docs/shortlink/dashboard-library-examples#recent-logs) .
- Improved correlation –[Connect log data with traces](https://docs.dynatrace.com/docs/analyze-explore-automate/logs/lma-log-enrichment) , metrics, and infrastructure telemetry using shared semantic fields.
- Reduced MTTR –[Spend less time interpreting logs](https://docs.dynatrace.com/docs/analyze-explore-automate/logs/lma-logs-app/problem-mode) and more time resolving issues.


The result is a workflow that scales with your environment rather than becoming more complex as new applications and services are introduced.


## Stop just collecting logs. Start actually using them.


Usability is a big challenge in log management. Unstructured logs slow investigations, increase operational overhead, and make it harder to extract meaningful insights. Simply collecting more data doesn’t solve the problem.


What matters is making that data actionable.


Log data processed through OpenPipeline lands in[Grail](https://www.dynatrace.com/platform/grail/) , Dynatrace’s scalable data lakehouse; teams can analyze logs alongside traces, metrics, events, security findings, and business data without moving or duplicating data across tools.[Smartscape](https://www.dynatrace.com/platform/application-topology-discovery/smartscape/) automatically maps dependencies across applications, services, infrastructure, and cloud resources, providing the context needed to understand where an issue originated and what it impacts.


[Dynatrace Intelligence](https://www.dynatrace.com/platform/artificial-intelligence/) then helps teams move from detection to resolution faster by surfacing patterns, correlations, and insights across all observability data.


The result is more than structured logging. It’s a unified troubleshooting workflow where logs become part of a broader operational context—helping teams reduce MTTR, eliminate tool sprawl, and resolve issues with greater confidence.


Want to see how quickly you can transform raw log data into actionable insights? Explore the Dynatrace Playground and experiment with modern log management in a guided environment.
[Explore Playground](https://www.dynatrace.com/signup/playground/log-management-and-analytics/)
