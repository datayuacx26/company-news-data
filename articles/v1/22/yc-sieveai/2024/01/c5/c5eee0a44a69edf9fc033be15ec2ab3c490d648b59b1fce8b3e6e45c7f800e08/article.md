---
schema_version: "1.0.0"
document_id: "c5eee0a44a69edf9fc033be15ec2ab3c490d648b59b1fce8b3e6e45c7f800e08"
company_key: "yc-sieveai"
company: "sieve"
source_id: "yc-sieveai-news-import-c83e13839616"
canonical_url: "https://usesieve.com/blog/ESG-data-collection"
published_at: "2024-01-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:04:09.595708+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:1a31c6eb72eb16de2135fd5360ca4b4e2d8c527b4486950bd146e09c2ffefe7b"
---

# ESG data collection

## Context


Ryan is a data scientist at a leading ESG-focused fund. To improve their selection methodology, the research team wanted to gather new data on companies' environmental, social, and governance (ESG) metrics. The team evaluated traditional data vendors, but found the cost, quality, and licensing terms to be unacceptable, so they decided to build in-house.


## Issues


As a data scientist, Ryan's initial approach was to use frontier LLM models (e.g., Claude, OpenAI) to extract data from financial filings. However, the models were not able to extract the data they needed from the financial filings to the necessary degree of accuracy. Ryan spent weeks manually checking the data between source documents and LLM outputs to confirm correctness. This process was time-consuming and error-prone. He had to make a tradeoff between how thoroughly he checked the data, and how much of the data he could get to. In every batch, he'd check the first few documents rigorously, really looking closely at the source document. Of course, this level of fine-grained review would not scale to all the companies in their coverage universe. From then on, he'd skim the documents, and then would skim every tenth document. This made the process more scalable, but clearly less comprehensive. Ryan and his team wanted a scalable solution that wouldn't burden them with manual review.


## sieve solution


We built a set of scripts for Ryan that let him configure the companies and metrics he cared about, and used the sieve API to retrieve the data asynchronously. Behind the scenes, sieve finds the right document, uses AI to extract the requested data points, and forwards the data to a team of human reviewers. After careful human review to ensure accuracy, the data is returned to Ryan via the API. This was a blessing to Ryan and his team, since it gave them more time to focus on research, and improved the quality of the data they operated on.


### Stop manually verifying LLM outputs?


Eliminate weeks of manual validation work. Let sieve provide guaranteed-accurate ESG data extraction without burdening your team with review.


Contact us athello@usesieve.com to discuss your use case.
