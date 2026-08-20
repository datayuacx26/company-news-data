---
schema_version: "1.0.0"
document_id: "715c4b5f1f1ac610fb8b70780e99635ea3da6062c6d259c20892e4acdbfe526d"
company_key: "yc-ashby"
company: "Ashby"
source_id: "yc-ashby-news-import-72f22fd66301"
canonical_url: "https://www.ashbyhq.com/blog/recruiting/mcp-recruiting-analytics"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-07T05:32:48.602521+00:00"
fetched_at: "2026-08-07T05:32:50.372566+00:00"
content_hash: "sha256:d3fdcc61882030b2813a1fcbac309e05bd9a76dbbbc58d499c9af4a14662ec3b"
---

# Why MCP Alone Doesn't Cure Weak Recruiting Reporting

While MCP is an exciting technology,[which we also recently added to Ashby](https://www.ashbyhq.com/product-updates/mcp) , it isn't a magical cure for product shortcomings, especially not when it comes to reporting. Some products that offer limited reporting may point to MCP as a workaround.


There are three core reasons why many reporting use cases aren't well served by MCPs:


- Accuracy & reliability
- Cost
- Access & governance


We'll go into greater detail on all of these points, but this table highlights some of the tradeoffs between in-app and MCP-based reporting.


It's also important to note that **LLM + MCP-based reporting will perform better the stronger the underlying tools your vendor provides** . For example, Ashby offers powerful data filtering capabilities that we expose via our MCP. If your current vendor lacks strong in-product data filtering, that deficiency will likely carry over into the MCP tool design, amplifying the issues outlined below.


Dimension MCP-Based (LLM + MCP) In-App Reporting (e.g. Ashby)


Accuracy & Reliability Prone to partial / incorrect answers; limited by token, rate limits, and non-determinism Deterministic, transparent ("shows its work"), drill-down capable


Performance Can take seconds to minutes per query; recomputes every time Near-instant dashboards with optimized queries


Cost Ongoing inference cost per query; scales with data size Fixed cost; no per-query compute overhead


Data Access Limited by MCP tool design Full dataset access with rich filtering


Governance Tradeoff between central runs vs per-user runs Built-in permissioning and sharing


Best Use Case Ad hoc analysis, cross-system questions Core reporting, dashboards, operational metrics


## Accuracy & Reliability


An LLM + MCP alone will not produce accurate reporting. LLMs can leverage MCPs to pull different data points (depending on how the MCP tools by the vendor are designed). However, **MCP alone is not designed to pull large data sets for reporting** .


Accurate reporting requires a surprising amount of data points — even a simple question like "What is our pass-through rate in Engineering this year?" is often not served well by MCP.


It would require the LLM to scan all the tools in the MCP, see if there's a tool to fetch all applications in engineering, then fetch all stage transitions for all of these applications. In most cases the LLM would run into rate limits on the MCP or would run into a limit of how many tokens it can consume to answer an individual question. These problems get worse as you try to answer follow-up questions and dig deeper (e.g., grouping by an additional field, like location).


In the best case, the LLM informs you of its inability to answer the question. But LLMs are designed to be helpful, and often they will give a partial/inaccurate answer in a best-effort attempt. As a result, even basic reporting questions asked of a variety of products using MCP may give you incorrect answers. You've likely experienced a "you're absolutely right" when correcting an LLM. That's a scenario one wants to avoid when it comes to critical workflows like reporting. In contrast,[Ashby's in-product reporting](https://www.ashbyhq.com/analytics) clearly "shows its work" by detailing the data set and filters used, and allows for drilling down into data points to further inspect results.


And lastly, LLMs are non-deterministic. If they need to do a lot of heavy lifting on top of raw data, their approach to that may change over time, producing different reporting results for different users.


**Based on the limitations above, the quality of reporting via an MCP is largely still tied to the reporting quality offered in a product** . Since an LLM can't sync large amounts of data to answer reporting questions, it needs MCP tools that provide very granular filters. If you want to answer a question like, "How many hires did Mariah make this quarter, excluding internship hires," you will only get good results if the product the LLM is "speaking" to has tools with precise filters. **Ashby offers this, but most other ATSs don't, based on their existing reporting limitations** .


## Cost


One drawback of LLMs is that they need to re-run their analysis every time you want updated data (assuming you're relying on LLM prompts + MCP, without building a custom reporting app).


Since many reporting questions involve a lot of data, that means **you may spend multiple dollars of inference to rebuild a single analysis every single time you want to update it** . While many companies don't budget LLM costs tightly, and these costs are expected to come down over time, it will still be an inherent limitation of LLM-based reporting. This also translates into "load times." While in-app dashboards are available almost instantly, with up-to-date data, LLM analysis can run for multiple minutes to update results.


## Access & Governance


Ashby in-app reporting makes it easy to share dashboards in a permission-aware way. This means there's no need to rebuild the dashboard for specific audiences and no risk of confidential data leaking to audiences that should not have access.


LLM and MCP in comparison have some limitations here. Depending on how you design your solution, you either run analysis centrally with "master" permissions to make it easier to share the results with the entire team, or, alternatively, you have each user connect with MCP on their own, making each result permission-aware — but also requiring a separate analysis for each unique user (coming back to the cost and performance issues).


## How LLM and MCP *Can* Work for Reporting


Similar to how BI tools worked in the past, MCP can be used for advanced reporting needs. This scenario typically requires you to sync all data to a data warehouse, model it there, and then connect MCP to the data warehouse (you can already do this today, without waiting on your SaaS vendor to offer an MCP server).


This can work around some of the issues outlined above, because the data warehouse will offer advanced data querying tools (similar to what you will get with Ashby) which can help offset the accuracy issues outlined above.


## What Ashby Can Offer


Our goal is to offer the "best of both worlds." Best-in-class in-app reporting, which should be used in most cases, based on everything outlined above. We've also built an MCP that exposes the same advanced data filtering you see in the product today. This means unlike many other MCPs, you will be able to reliably do something like "fetch all candidates that were hired in Engineering and referred by Jane Doe." That's what makes MCP-based reporting in Ashby as viable as possible for the use cases where it actually shines: blending data across multiple sources or one-off deep dives that go beyond a series of reports and dashboards.
