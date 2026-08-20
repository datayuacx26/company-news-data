---
schema_version: "1.0.0"
document_id: "25d93467294396e3f667e970ebde11d57f52508de306e1b2bc7fd704766ced66"
company_key: "bit-digital-inc-ordinary-shares"
company: "Bit Digital Inc."
source_id: "bit-digital-inc-ordinary-shares-news-import-3dee2047154f"
canonical_url: "https://bit-digital.com/blog/how-to-evaluate-staking-providers/"
published_at: "2025-11-19T13:30:21+00:00"
first_seen_at: "2026-07-24T21:07:37.610803+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:56a3b392918f739552f1bd20043f81fb603d26c58821e76004e6b37332b0fe9d"
---

# How To Evaluate Staking Providers

- November 19, 2025


– Bit Digital


If you manage institutional capital, your choice of staking provider is not a technical detail. It is a core counterparty decision that touches security, yield, governance, and regulatory exposure. The wrong choice can turn a low risk yield strategy into operational and reputational damage. The right choice gives you reliable income, clean audit trails, and a credible story for your investment committee.


In this guide, we will walk through how to evaluate staking providers using an institutional due diligence lens. We will focus on uptime, security, slashing record, governance participation, and regulatory compliance, with practical questions you can use in RFPs and manager reviews.


Subscribe to stay updated on our Ethereum treasury and corporate news.


## **Start with Market Context**


Before you look at specific providers, it helps to understand the scale and concentration of Ethereum staking.


- As of November 2025,


[36 million ETH](https://cryptoquant.com/asset/eth/chart/eth2/total-value-staked?window=DAY&sma=0&ema=0&priceScale=log&metricScale=linear&chartStyle=line) is staked on Ethereum, representing over 25 percent of the total supply.


- Liquid staking providers are highly concentrated. Lido alone accounts for roughly 25 percent of all staked ETH, while the largest centralized exchanges also control a significant share.


For you as an institutional allocator, this means you should treat provider selection and concentration as systemic risks, not just operational details.


## **Uptime and Performance**


Uptime is the first screen. A provider that cannot keep validators online will not deliver the headline staking rate you use in your models.


**Questions to ask:**


- What is your historical validator uptime on Ethereum, expressed over 30, 90, and 365 days?


- How do your realized rewards compare to network reference rates over the same periods?


- What is your target service level for validator uptime and how is it monitored?


**What you should see:**


- Public or auditor-verifiable performance data, ideally cross-checked against independent explorers such as beaconcha.in and Rated.


- Clear SRE practices: 24/7 monitoring, on call rotation, alerting thresholds, and incident reports.


- Infrastructure diversity: multiple regions, multiple availability zones, and multi client stacks so a single bug or outage does not take all validators offline at once.


You can benchmark claims against public network data. In recent snapshots, Ethereum has more than 1 million active validators with participation rates typically above 98 percent, which sets a high bar for what constitutes acceptable uptime.


## **Security Architecture**


For institutions, security architecture is just as important as headline yield. Most slashing incidents and catastrophic losses have roots in poor key management or unsafe operational shortcuts.


Key areas to review:


### **Key management and signing**


- Does the provider use hardware security modules or secure enclaves for key storage and signing?


- Are validator keys ever present in plain text on disk or in memory outside secure hardware?


- Is there a clear separation between withdrawal keys and validator keys, with documented controls?


You should prefer providers that use remote signing with HSMs or equivalent, clear key ceremonies, and role based access to signing infrastructure.


### **Infrastructure and process security**


- What phishing, intrusion detection, and change management processes are in place?


- How often are backups tested and disaster recovery rehearsed?


- Has the provider undergone recent security assessments, such as SOC 2 or ISO 27001, and can you see the reports under NDA?


Look for written security policies, security officer ownership, and evidence of regular reviews.


## **Slashing and Penalty Record**


Slashing is rare but real. It is also one of the few easily measurable indicators of operational discipline.


**Questions to ask:**


- Have any of your validators ever been slashed on Ethereum? If yes, how many, when, and why?


- What restitution policies do you have if a client’s stake is slashed due to your error?


- How do you avoid double signing and other common slashing causes?


**What you should see:**


- A provider that can discuss slashing risk in concrete terms and ideally has zero or very low incidents relative to the size of its validator set.


- Transparent incident post mortems if slashings have occurred, with changes implemented after the event.


- Architectural controls that prevent running the same validator key in multiple locations.


You can cross check answers using on-chain data and public explorers that track slashing events. An incident involving 39 validators being slashed in a single correlated event in 2025 illustrated how a single operator level failure can damage many validators at once, which is exactly the risk institutional allocators want to avoid.


## **Governance Participation and Ecosystem Alignment**


For many institutions, it is not enough that a provider runs validators. You also want a partner who understands and influences Ethereum’s roadmap in a way that protects long term value.


**Questions to ask:**


- Do you participate in Ethereum governance discussions, such as Ethereum Magicians, research calls, or client working groups?


- How do you track and prepare for upcoming upgrades that might affect validator operations or staking economics?


- Have you published research, improvement proposals, or public commentary on EIPs that affect stakers?


**What you should see:**


- Evidence that the provider follows and understands major governance changes, such as Dencun, Pectra, and staking related EIPs.


- Clear internal processes for upgrade testing, dry runs on testnets, and production rollout with rollback plans.


- A track record of public contributions, whether in research, tooling, client diversity advocacy, or security reviews.


Providers who are active in governance are often better prepared for changes that affect your returns and operational risk profile.


## **Regulatory Compliance and Jurisdiction**


Institutions need staking partners who can stand up to regulatory and audit scrutiny.


Key topics to cover:


### **Licensing and registration**


- In which jurisdictions does the provider operate and what licenses, if any, do they hold?


- Do they have regulated entities for custody, broker dealer activity, or investment advisory services where relevant?


- Have they been subject to regulatory actions, fines, or censures?


### **KYC, AML, and sanctions**


- How does the provider screen clients and counterparties for AML and sanctions?


- Do they comply with relevant sanctions lists when engaging with MEV relays, counterparties, and DeFi integrations?


- Can they demonstrate controls for avoiding interaction with blocked addresses where required?


### **Reporting and audit**


- Can the provider produce the reports your auditors will request, including transaction histories, validator assignments, fee breakdowns, and policies?


- Do they support proof of reserves or equivalent attestations where relevant for pooled structures?


You want a provider that understands not only protocol level rules but also the regulatory perimeter in your jurisdictions.


## **Operational Transparency and Reporting**


You will have to explain this relationship to your investment committee and auditors. Transparent reporting makes that easier.


What to request:


- Regular validator level reports, including uptime, missed duties, penalties, and realized rewards.


- Fee breakdowns that clearly show provider fees, protocol rewards, and any third party costs such as relay payments.


- Incident reports and maintenance notifications with clear timelines and remediation actions.


Some leading platforms provide near real time dashboards and CSV exports so risk teams can monitor performance and reconcile positions. That should be your benchmark.


## **Diversification and Exit Options**


Finally, treat staking as a portfolio of relationships, not a single binary choice.


Consider:


- Using more than one provider or combining a provider with self staking for diversification.


- Ensuring that your contracts include clear termination and offboarding clauses, including timelines for exiting validators and transferring withdrawal credentials where appropriate.


- Avoiding over concentration in a single liquid staking token or single provider, given that the largest custodial and liquid staking entities already control a large share of staked ETH.


## **A Practical Due Diligence Checklist**


- Uptime and performance: verifiable 12 month history, target SLOs, and monitoring stack.


- Security: HSM or equivalent key management, documented security program, independent audits.


- Slashing: clear incident record, restitution policy, and architecture to prevent double signing.


- Governance: active tracking of EIPs and upgrades, testnet participation, and public contributions.


- Compliance: licensing where needed, AML and sanctions controls, and audit ready reporting.


- Transparency: dashboards, exports, and incident communications.


- Diversification: plan to avoid single provider and single token concentration.


## **Key Takeaways**


- Treat staking provider selection as a core counterparty decision, not a technical detail.


- Uptime, infrastructure diversity, and consistent validator performance determine whether you capture the reference staking rate.


- Security architecture is critical. HSM based key management, strict operational controls, and documented incident processes reduce the risk of slashing or key compromise.


- A clean or minimal slashing record, supported by transparent post mortems where needed, is a direct signal of operational discipline.


- Providers that follow and participate in Ethereum governance are better positioned to protect your returns during upgrades.


- Regulatory readiness matters. Institutions need partners with documented KYC and AML controls, audit ready reporting, and jurisdiction appropriate licensing.


- Diversification across providers and staking methods reduces operational and concentration risk, especially in a market where a few entities control a large share of staked ETH.


## **Conclusion**


Choosing a staking provider is one of the highest leverage decisions you can make when deploying institutional capital into Ethereum. Reliable operators deliver stable income, reduce operational risk, and help you navigate upgrades with confidence. Weak providers can turn a low volatility yield strategy into a source of downtime, penalties, and reputational exposure.


By focusing on uptime, security, slashing history, governance alignment, and regulatory compliance, you can evaluate providers with the same rigor you apply to any other institutional counterparty. Use this framework to structure your due diligence, build diversified exposure, and ensure that staking strengthens your portfolio rather than complicates it.


Stay updated on our Ethereum treasury and corporate news,


**subscribe now** .
