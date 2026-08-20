---
schema_version: "1.0.0"
document_id: "f188108db83e3258337e22a3c5b158f4341ad356dd9ac8a0ea11f6dbc644b0d4"
company_key: "yc-netbeez"
company: "NetBeez"
source_id: "yc-netbeez-rss-1a1c74f0723d"
canonical_url: "https://netbeez.net/blog/the-internet-quality-barometer/"
published_at: "2026-08-19T20:40:37+00:00"
first_seen_at: "2026-08-19T22:19:48.805766+00:00"
fetched_at: "2026-08-19T22:19:50.899134+00:00"
content_hash: "sha256:0432ce900f0f4ef7cb17e4d42c52c17ec0a23bdf279925d4cd9083fbe1b96805"
---

# The Internet Quality Barometer: Rating the Internet by What People Actually Do

Ask how good a connection is and you will almost always get an answer in Mbit/s. That made sense when throughput was the bottleneck. It makes a lot less sense today. A 500 Mbit/s link with 80 ms of latency and sporadic loss will wreck a video call that a well behaved 50 Mbit/s link handles without a hiccup. Speed tests measure what is easy to measure, not what users actually notice.


The[Internet Quality Barometer](https://www.measurementlab.net/blog/iqb/) (IQB) is Measurement Lab’s attempt at a better answer. It is an open source research initiative, funded by the Internet Society Foundation, that produces a composite score from 0 to 1 describing Internet quality rather than Internet speed. Between November 2023 and March 2025, M-Lab ran interviews and workshops with more than 60 experts from network research, public policy, digital inclusion advocacy, ISPs, and content providers to build it.


## How it works


IQB is built in three tiers.


**Use cases.** Nobody experiences their connection as latency and jitter. They experience it as whether the thing they are trying to do works. IQB defines six activities:


- web browsing
- video streaming
- audio streaming
- video conferencing
- online backup
- gaming


**Network requirements.** Each activity maps to four metrics you already track: download speed, upload speed, latency, and packet loss. Each metric gets a threshold for minimum and for high quality, plus a weight from 1 to 5 for how much it matters to that activity. The weights are the whole argument of the project, compressed into one table:


**Use case** **Download** **Upload** **Latency** **Packet loss**


Web browsing 3 2 4 4


Video streaming 4 2 4 4


Audio streaming 4 1 3 4


Video conferencing 4 4 4 4


Online backup 4 4 2 4


Gaming 4 4 5 4


Read the gaming row against the online backup row. Latency is the top weighted metric for gaming and nearly the bottom for backup, which is obvious to any engineer and invisible in a speed test result.


**Datasets.** Requirements are then evaluated against public measurement data: M-Lab’s NDT, Cloudflare, and Ookla’s published aggregates. Using more than one is intentional. All three measure throughput differently, so when they agree, the result is worth more than any single source.


The scoring itself is simple on purpose. Every metric either clears its threshold or it does not, a straight pass or fail, and those binary results roll up through three weighted averages: per requirement, per use case, and finally into the score. M-Lab compares it to a credit score or the Nutri-Score, a single number that summarizes something complicated without pretending to be precise.


An example makes the arithmetic concrete. Say a region clears the download, upload, and packet loss thresholds for gaming but misses on latency. Gaming weights sum to 4 + 4 + 5 + 4 = 17, and the failing metric is the one worth 5, so the use case scores 12/17, or 0.71. Now suppose that same region had cleared latency and missed download instead. It scores 13/17, or 0.76. Same number of failures, better score, because for gaming the framework cares more about latency than throughput.


Run the identical latency failure against online backup, where the weights sum to 14 and latency is worth 2, and the score is 12/14, or 0.86. One impairment, three different verdicts depending on what the user is doing. That is the entire point of the framework in one calculation.


One choice is worth knowing about before you read any IQB number. Measurements are aggregated at the 95th percentile, picked to approximate what the infrastructure can deliver rather than what a typical user gets. It is defensible, but it makes scores look generous. M-Lab’s own follow up analysis shows results shift noticeably at the 50th or 75th percentile.


## Where the project stands


The framework itself was published in 2025. A second phase added a Python library, a data pipeline running on BigQuery, and a web prototype. A sensitivity analysis followed in 2026, testing how much the scores depend on those percentile and threshold choices.


Be clear about scope. IQB reports at country, region, city, and sometimes ISP level, and it needs a large volume of measurements behind it. It is not a per connection diagnostic, so do not expect to point it at a branch office and get a verdict.


## Trying it


The prototype at[iqb.mlab-staging.measurementlab.net](https://iqb.mlab-staging.measurementlab.net/) lets you browse scores on a map, switch aggregation percentiles, and adjust thresholds. Ten minutes with the percentile slider will teach you more about the framework’s assumptions than the report does. It is a staging deployment, so treat the URL as temporary.


If you want to go further, the library is Apache 2.0 on GitHub, and the analysis/ directory has Jupyter notebooks showing real usage.


## What it means for monitoring


The interesting part of IQB, for anyone who monitors networks for a living, is not the score. It is the premise. Quality is defined by whether specific applications work, thresholds vary by application, and a single number pulled off a speed test cannot tell you any of that. You need a distribution, gathered over time, from where the users are.


That premise should sound familiar. It is the same reason NetBeez agents run continuous tests from every location you monitor instead of waiting for someone to open a ticket and run a speed test.


Get your free trial


now


Monitor your network from the user perspective


You can share


[Twitter](http://twitter.com/share?url=https://netbeez.net/blog/the-internet-quality-barometer/)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://netbeez.net/blog/the-internet-quality-barometer/)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://netbeez.net/blog/the-internet-quality-barometer/) Copy link


Link copied
