---
schema_version: "1.0.0"
document_id: "0cea802c6c524a2f2fe3b5e0c2508fc4d60d1d7c2bbb552666ce183f5e49e8a7"
company_key: "yc-taktile"
company: "Taktile"
source_id: "yc-taktile-news-import-fe0773fa612c"
canonical_url: "https://taktile.com/articles/ai-under-the-acpr-s-lens-meeting-explainability-and-governance-standards-in-aml-and-fraud-detection"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-24T03:38:53.146880+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:fc76f57d14be81cdfe984937911e6d7146cdc712bcef4f7894b51bb56abacec9"
---

# AI under the ACPR’s Lens: Meeting explainability and governance standards in AML and fraud detection

If you run an AML or fraud program touching the French market, you might know that the[Autorité de contrôle prudentiel et de résolution (ACPR)](https://acpr.banque-france.fr/en) is one of the more demanding supervisors in Europe. What's shifted in recent years is the level of documentation they expect.


The bar used to be: do you have controls, and can you show how they work?


Now, the bar is increasingly becoming: can you show how your models work, why they made each decision, and who was accountable for the outcome?


That shift matters because many AML and fraud stacks were built primarily to generate alerts. This article walks through what the ACPR actually expects from AI in financial crime detection, and what an ACPR-ready program looks like in practice.


## **The ACPR: Where it came from and why its mandate cuts wider than people assume**


The ACPR was created in 2010 through[Ordinance No. 2010-76](https://acpr.banque-france.fr/en/lacpr/about-us) , which merged four predecessor bodies: the Banking Commission, ACAM, CEA, and CECEI. The ordinance was ratified by the French Banking and Financial Regulation Act later that year. It was, in every sense, a post-2008 reform: the goal was to consolidate fragmented supervision under a single authority with the resources to actually do the work.


In July 2013, it picked up resolution authority and became the ACPR rather than the ACP. That addition is more than a name change. **It means the same body that supervises your AML controls is also the one that can resolve your institution if it fails.**


The ACPR is staffed and backed by the[Banque de France](https://www.banque-france.fr/en) , which gives it real institutional weight. Its dual mandate is systemic stability and customer protection, and **its scope reaches further than people often realize** : banks, insurers, payment institutions, e-money institutions, and by extension the agents and partners those licensed entities work with.


## **What the ACPR actually expects from AI in finance moving forward**


The ACPR put out its[discussion paper on AI governance in finance](https://acpr.banque-france.fr/system/files/import/acpr/medias/documents/20200612_ai_governance_finance.pdf) in 2020. It focused on three areas: AML/CFT, credit scoring, and customer protection. Five years on, that paper remains one of the most concrete articulations of what a French regulator wants to see when AI is making decisions inside a financial institution.


The framework rests on four performance criteria:


- **Data management** : lineage, quality, governance over inputs.
- **Explainability** : the ability to explain what the model did, to the right audience, at the right level of detail.
- **Performance** : accuracy, robustness, and ongoing measurement.
- **Stability** : how the system behaves over time, under drift, and through change.


A piece many teams underestimate is the explainability layer. The ACPR articulates a[tiered model](https://acpr.banque-france.fr/system/files/import/acpr/medias/documents/20200612_ai_governance_finance.pdf) of transparency: observation, justification, approximation, replication. The level you need depends on the audience (internal validator, supervisor, affected customer) and the risk of the decision.


This is the part that often gets missed in vendor pitches. The ACPR is not asking for full transparency on every model, but for governance proportionate to the risk, with the ability to escalate to deeper explanation when the situation demands it.


In practice, institutions benefit from knowing in advance what level of explainability each AI system in its stack is capable of producing, and whether that level matches the decision the system is influencing.


## **What’s converging: AMLD6, the EU AI Act, AMLA, and DORA**


Four regulatory streams are now landing on the same desk.


An ACPR-supervised institution running AI inside its AML program in 2027 will need to satisfy ACPR governance criteria, AMLD6 obligations, AI Act provider/deployer duties, and DORA vendor oversight requirements, simultaneously, with one coherent control environment. The compounding effect is worth paying attention to.


- **AMLD6** ([Directive 2024/1640](https://www.europarl.europa.eu/legislative-train/theme-an-economy-that-works-for-people/file-6th-directive-on-amlcft-(amld6)) ) must be transposed into French law on **July 10, 2027** . It strengthens coordination between national supervisors and FIUs, harmonizes core AML obligations, and sets the stage for the new[EU Anti-Money Laundering Authority](https://www.europarl.europa.eu/legislative-train/theme-an-economy-that-works-for-people/file-6th-directive-on-amlcft-(amld6)) (AMLA), which will directly supervise the highest-risk cross-border institutions and oversee national supervisors for the rest.
- **The EU AI Act** classifies credit scoring systems as high-risk and applies a nuanced exemption to AML and fraud detection systems. The exemption is real but narrower than it looks. As[the EBA has flagged](https://www.eba.europa.eu/sites/default/files/2025-11/d8b999ce-a1d9-4964-9606-971bbc2aaf89/AI%20Act%20implications%20for%20the%20EU%20banking%20sector.pdf) , once an AML model's output drives a downstream decision like account closure or service denial, the functional impact starts to look high-risk-adjacent. Deployer liability is built in: institutions are not able to fully offload compliance onto their vendors.[Penalties](https://artificialintelligenceact.eu/article/99/) run up to **3% of global turnover** .
- **DORA** brings operational resilience, ICT risk management, and third-party vendor oversight into the supervisory perimeter. The ACPR enforces DORA for French entities. That can mean your AML vendor stack is now part of your operational resilience program, not a separate procurement concern.


## **Why legacy approaches can hold financial institutions back**


I have seen a lot of AML and fraud stacks. The ones that struggle most under the new regulatory weight tend to share a few characteristics.


**Rule-based monitoring built for a different alert volume.** When STR filings are running 20%+ above last year's pace and your transaction monitoring system is still tuned around static thresholds set three years ago, two things can happen. Alert volumes explode, and the signal-to-noise ratio collapses.


[False positive rates can run north of 95%](https://datos-insights.com/blog/are-you-too-negative-about-false-positives/) , and the operational cost is significant. The compliance risk, less obvious but more dangerous, is alert fatigue: when analysts cannot keep up, real cases get closed without proper review.


**Black-box models with no audit trail.** Some institutions have moved to ML-based detection but cannot explain why a given alert fired. Under the ACPR's framework, this can be viewed as a governance failure, not a technical curiosity. If you cannot produce justification or approximation-level explainability when a supervisor asks, the model itself can become a liability.


**Manual processes that break under examination.** When a regulator asks how a decision was made eighteen months ago, the answer is expected to be reproducible. Many programs cannot reconstruct decision logic, version history, or evidence of human oversight from that far back. The information exists somewhere, but it lives across spreadsheets, case management notes, and analyst memory.


**Fragmented vendor stacks with no unified audit trail.** Many institutions did not buy their AML and fraud capability as a single system. They assembled it from point solutions across screening, monitoring, case management, and customer risk scoring. Each vendor has its own audit logging, its own export format, its own retention policy. Stitching that into a coherent narrative for a supervisor can become a multi-month project.


## **What an ACPR-ready AML and fraud program looks like in practice**


Mapping the ACPR's four criteria back to operational capability is a useful exercise. It clarifies what to build and what to retire.


**Data management** translates to integrated data orchestration. The system needs to ingest, normalize, and version the inputs it acts on, and it needs to do that in a way you can show a supervisor.


**Explainability** translates to decision-level traceability. Every alert, every score, every escalation should produce an artifact that explains what the model saw, what features mattered, and what the model output meant.


**Performance** translates to ongoing measurement and challenge. Backtesting, champion/challenger setups, drift monitoring, and bias testing where relevant. Performance can be treated as a continuous obligation, and supervisors increasingly expect to see the evidence trail.


**Stability** translates to versioning and change control. Every model, every rule, every threshold change tracked, dated, attributed, and reversible. When a supervisor asks what the system was doing on a specific date eighteen months ago, the answer should take minutes, not weeks.


A few operational design points follow from this:


- **Real-time and batch monitoring need to coexist.** Real-time for the decisions where speed matters (payment authorization, account opening). Batch for the behavioral patterns that only emerge over windows of days or weeks. Modern programs run both, against the same underlying data and rule library, so you are not maintaining two parallel realities.


- **Human-in-the-loop benefits from being designed deliberately.** AI agents can autonomously handle low-risk alerts at high volume. What matters is that the escalation logic to human analysts is explicit, that the human review is genuine rather than rubber-stamping, and that the entire chain is auditable. The ACPR's governance expectations tend to be satisfied by demonstrating real oversight.


- **Audit trail design is upstream, not downstream.** The temptation is to add audit logging at the end. The right architecture treats every decision, from initial alert to final SAR filing, as a traceable event from the moment it is generated.


- **Compliance teams benefit from owning the detection logic.** When every rule change requires an engineering ticket, the program can struggle to keep pace with the threat landscape or the regulatory pace. The institutions that are pulling ahead are the ones where AML and fraud teams can author, test, and deploy detection logic directly, within governance guardrails.


## **Compliance as a real competitive advantage in 2026 and beyond**


The institutions best positioned to win in France over the next few years are the ones that can prove their AML and fraud programs are governed, explainable, and operationally resilient. They tend to win partnerships, because licensed entities are likely to choose partners they can defend to a supervisor. They can expand faster, because regulatory friction can become an asymmetric advantage for the firms that have already done the work. And they can avoid the reputational and financial damage that the Sanctions Committee can now inflict at scale.


The convergence of the ACPR's existing framework with AMLD6, the EU AI Act, AMLA, and DORA is becoming the new baseline. The bar is rising in a way that rewards institutions that built governance, explainability, and human oversight into their AI programs from the start.


## Want to build effective AML programs while staying compliant?


[Let's talk](https://taktile.com/demo)
