---
schema_version: "1.0.0"
document_id: "dd2b175170451a79a01cb62c103624a9b68f0861a2348286e024cbe73b6bdd14"
company_key: "teradata-corporation-common-stock"
company: "Teradata Corporation"
source_id: "teradata-corporation-common-stock-rss-6caecaa75ce2"
canonical_url: "https://www.teradata.com/insights/data-analytics/the-mobile-deposits-dilemma"
published_at: "2026-08-13T07:00:00+00:00"
first_seen_at: "2026-08-13T17:43:04.322704+00:00"
fetched_at: "2026-08-13T17:43:06.296778+00:00"
content_hash: "sha256:a0ebd2e1a542a2b17f844d98ffaa4cfe4bc7701e9ff52ff8da5d08bb5f8bba06"
---

# The Mobile Deposits Dilemma

With over 5,000 banks across the U.S., paper checks remain the default way to pay someone directly from a bank account. Processing them is costly, so banks have long sought alternatives to in-person teller deposits.


The "Check 21" legislation of 2004 made a check image a legal substitute for the physical document. ATMs soon followed, introducing image processing technology capable of identifying the issuing bank and reading handwritten dollar amounts. Then, in 2009, USAA took the next leap: leveraging the growing popularity of mobile banking, it launched mobile remote deposit capture (MRDC)—a smartphone app that lets customers photograph the front and back of an endorsed check and submit it electronically.¹ MRDC was an early form of intelligent document processing, using AI to correct for creases and folds in the paper before parsing the handwritten amount.


¹ Becky Yerak, "New Wave of Banking: Check Deposit via Smart-phone Photo," July 2010.


## MRDC adoption: Benefits and risks


During my banking career, I was asked to evaluate the costs and benefits of MRDC. My team carefully modeled the economic value of customer adoption and found the gains extended well beyond direct teller cost savings—MRDC also improved customer retention and drove higher deposit account balances. ² We further found that a customer's likelihood of adopting MRDC was closely tied to whether they were already moving money through the digital banking app.


² Gary Class, "[Customer Banking Relationship in 5 Dimensions,](https://www.teradata.com/insights/white-papers/customer-banking-relationship-in-5-dimensions) " Teradata.com, 3 July 2024.


That said, MRDC introduces real fraud risk: a deposited check could be counterfeit or drawn on an account with insufficient funds. To manage this exposure, banks imposed two safeguards—a per-item dollar limit and a daily aggregate cap on remotely deposited checks. These risk controls, however, made banks reluctant to promote MRDC broadly.


## Considering adoption as a control limits problem


When I turned to identifying what was holding MRDC adoption back, a clear pattern emerged: affluent customers—those who sent and received checks most frequently—were the least likely to use MRDC. Customers who regularly deposited checks above the dollar threshold wouldn't even attempt it, let alone make it a habit.


This created a genuine strategic dilemma. The conservative eligibility cutoffs reduced fraud exposure, but they also pushed away the bank’s most valuable customers. The core question became similar to a classic control limits problem in statistics: How much were the risk thresholds actually suppressing MRDC adoption and usage? And under what conditions would relaxing them make sense—where the gains from broader usage would outweigh the incremental fraud losses?


## Simulation as the solution to identifying business impact


The path forward was to build a detailed simulation model to quantify the impact of changing the MRDC risk rules. We benchmarked competitor policies and found a clear pattern: Banks with less restrictive limits enjoyed noticeably higher MRDC adoption rates.


Our simulation captured the full range of benefits from increased mobile deposit usage—lower item-processing costs, stronger customer retention, and higher deposit balances across representative customer cohorts. The objective was to find the threshold where expected fraud losses were offset by those compounding benefits.


The results were compelling. Field trials confirmed that loosening the eligibility restrictions boosted customer usage without any meaningful increase in fraud losses. That confidence in our simulation’s predictive power then informed a targeted direct mail campaign to drive further adoption.


## How Teradata can help


Teradata offers a comprehensive library of econometric functions that run directly in-database—including model training and forecasting. That makes it well-suited for developing the kind of propensity models and scenario analyses described here.
