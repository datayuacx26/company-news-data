---
schema_version: "1.0.0"
document_id: "8698424082b4d3fed4fec6667ec5747e677b94b5bdfae79fa6476e017d3f0496"
company_key: "yc-quantstamp"
company: "Quantstamp"
source_id: "yc-quantstamp-rss-54cdced55685"
canonical_url: "https://quantstamp.com/blog/monthly-hacks-roundup-march-2024"
published_at: "2025-06-04T18:23:59+00:00"
first_seen_at: "2026-07-25T20:14:20.321573+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:e366e8df57135bf8bc7bdc5636559f0bcc140ae86ed01e1ff2835787a6e1feba"
---

# Monthly Hacks Roundup: March 2024

# Monthly Hacks Roundup: March 2024


March was a volatile month for the web3 security landscape, with significant security breaches totalling over $152 million in losses. Read on as we dive into four major security incidents and the trends from last month 👇


‍


## 🔒 Overview of Web3 Security Breaches


Here’s a breakdown of the losses experienced due to various incidents:


- **Smart Contract Hacks:** Over $47 million


- **Rug Pulls/Scams:** More than $100 million


- **Compromised Keys:** Approximately $4 million


- **Total:** Over $152 million


‍


## 🕵️ Incident Analysis


‍


### **Curio ($16M)**


An attacker managed to alter the governance system of CurioDAO by locking two governance tokens and using a malicious execution library. This allowed them to execute unauthorized actions including the mass minting of approximately 1 billion $CGT tokens, severely undermining the protocol's integrity.


‍


### **Prisma Finance ($11M)**


Prisma, a collateralized stablecoin platform, suffered an exploit involving the MigrateTroveZap contract. An attacker manipulated collateral amounts during a migration process, resulting in the improper adjustment of collateral values and the unauthorized opening of new financial positions. In a surprising twist, the white hat hacker reached out explaining that their aim is to raise awareness on serious contract audits, highlighting the need for rigorous contract audits and a serious attitude towards project responsibility.


‍


### **WOOFi ($8.5M)**


WOOFi lost $8.5 million due to a price manipulation exploit facilitated by a recent addition of a lending market by Silo Finance. Attackers inflated and deflated the price of the WOO token through strategic swaps, then exploited these manipulated prices to drain significant funds, exploiting weaknesses in WOOFi's Synthetic Proactive Market Making (sPMM) system and its misconfiguration of fallback oracles.


‍


### **Super Sushi Samurai Hack ($4.8M)**


This GameFi protocol was compromised due to a self-transfer bug in their SSS token, enabling infinite minting. The attacker drained $4.8 million worth of WETH from the SSS/ETH Thruster pool. Remarkably, the funds were returned in exchange for a bounty and the hacker was even brought onto the team as a security advisor.


‍


## 📉 Vulnerability and Auditing Trends


‍


### **Vulnerabilities**


- For the second consecutive month, a self-transfer/infinite mint bug ranked in the top four contract hacks (previously with MINER in February)
- Three hacks each over $1 million exploited user token approval (WOOFi, Unizen, Dolomite)
- Two $1 million+ hacks were caused by arbitrary external calls (WOOFi, Unizen) and insufficient validation (SSS, Dolomite)
- More than six price manipulation hacks occurred, underscoring the growing trend in exploiting price-related vulnerabilities


### **Auditing**


- The top two hacks this month involved vulnerabilities in unaudited code
- Even in cases where the code was audited (the next two largest hacks), the audits failed to detect the vulnerabilities, raising concerns about the effectiveness and thoroughness of current auditing practices


### **Trends and Outliers**


- Incidents on the BLAST protocol accounted for 44% of the total lost funds ($67.9M)
- Similar attacks targeted the newly launched BASE protocol in August, showing a pattern of attackers targeting new platforms
- Contracts were paused for two of the four largest hacks (SSS, WOOFi), indicating a reactive approach to handling breaches
- Two of the four largest hacks resulted in downtime extending beyond a month for the affected protocol’s main functionality (SSS, WOOFi)


‍


## 👋 See you next month!


To stay informed, secure, and ahead of the curve, follow us on Twitter[@Quantstamp](https://twitter.com/Quantstamp) and keep an eye out for next month’s roundup! Together, we can secure the future of web3 💪


‍
