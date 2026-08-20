---
schema_version: "1.0.0"
document_id: "95038acee1a8f5d00d3abc77021d34dd914b245eb1ce5ab0ac75f713d25ba031"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/simplifying-lives-of-accountants-with-our-new-reconciliation-engine-part-1-fc8840632da7"
published_at: "2020-12-08T15:14:58+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:08bc45cb2a6893e36558b0fc2e948477ef7b4640e2738f1b5b4867d4d4e691d6"
---

# Simplifying lives of accountants with our new Reconciliation Engine — Part 1

# Simplifying lives of accountants with our new reconciliation engine — Part 1


[Abhinav Kumar](https://medium.com/@kumarabhinav75?source=post_page---byline--fc8840632da7---------------------------------------)


3 min read


·


Dec 8, 2020


--


Press enter or click to view image in full size


Before we deep-dive into our new reconciliation engine **(Mentat)** and how it helped improve overall process efficiency, let us understand about reconciliation in brief.


## What is Reconciliation?


In accounting, reconciliation is the process of ensuring that two sets of records, usually the balances of two account, are in agreement. Reconciliation is used to ensure that the money leaving an account matches the actual money spent. This is done by making sure that the balances match at the end of a particular accounting period.


At ClearTax, we used to have a reconciliation engine for GST return filing. It was supporting only one type of reconciliation — GSTR-2A vs Purchase Register.


## Challenges in our old reconciliation engine


- **Performance** — For reconciliation, document download and bulk action used to take up a lot of time(~2 hours). This is because we reconcile data on a shared infra that makes requests either wait or timeout
- **Wait time due to filters** — UI loads data from the backend for sorting/filtering. This leads to a non-interactive user experience. For every filter(like search and sort), users had to wait for 5–10 seconds
- **No scope of customization** — The old reconciliation was tightly coupled with a monolith system and hence would not support new requirements
- **Rules management** — Rule management was tough in our old reconciliation system


To tackle the problems listed above, we needed a new reconciliation engine which can be customisable and would be capable to improve performance. Hence, we built **Mentat** — our new reconciliation engine.


## How does Mentat help us?


- **Mentat is decoupled—** Our old reconciliation app was part of our GST application. Decoupling it from GST application helps us to support various other non-GST use cases.
- **Highly Customizable —** New Reconciliation Engine is extensible to handle our current and future matching requirements. This enables us to quickly support matching for various use cases like -
1. Government GSTR-2A vs Purchase Register
2. GSTR-1 vs Sales Register
3. GSTR-9 Table 8-A vs Purchase Register
4. GSTR-2B vs Purchase Register
- **User Configurable —** Instead of having pre-configured rules, the system should support user defined rules. This increases the overall efficiency of the engine


Example —


```text
{    "description": "Rule Description",    "task": {      "task1": {        "field_names": [          "field_name_to_be_transformed"        ],        "function": "transformation_function"      },      "task2": {        "field_names": [          "field_name_to_be_transformed"        ],        "function": "transformation_function2"      }    }  }
```


- **Scalable —** New engine supports enterprise users with a high volume of documents. This becomes possible with dynamic configured pods in Mentat


**Note : We have reduced time taken to roll-out new Recon use-case from 3~4 months to 1 Week now with Mentat.**


## Achieving 10x improvements with Mentat


Below is the comparison between old reconciliation vs Mentat. We could clearly see around 10x improvements over old service.


Press enter or click to view image in full size


## Introduction to k8 Pods


**How do k8 pods make a difference?**


Think about it like creating server on-demand. What if we launch new pods and simply call a server for each request? We can make our reconciliation faster and taking actions, filtering/sorting data would call an independent isolated server, which would definitely be faster. K8 pods are nothing but a list of containers where each container would be running different sets of reconciliation independently.


**Cost impacts of using k8 pods**


Since these containers are on demand we can significantly save on infra cost. While we compare our old recon vs Mentat we could get upto 60% of cost saving.


**We will talk about the Mentat architecture, server-side matching vs client-side matching, web-workers and more details about k8 pods in the next part of this blog.**
