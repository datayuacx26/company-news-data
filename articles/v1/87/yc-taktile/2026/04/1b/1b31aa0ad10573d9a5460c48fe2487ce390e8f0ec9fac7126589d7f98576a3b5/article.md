---
schema_version: "1.0.0"
document_id: "1b31aa0ad10573d9a5460c48fe2487ce390e8f0ec9fac7126589d7f98576a3b5"
company_key: "yc-taktile"
company: "Taktile"
source_id: "yc-taktile-news-import-fe0773fa612c"
canonical_url: "https://taktile.com/articles/the-eu-ai-act-and-aml-how-to-prepare-your-program-for-the-next-wave-of-ai-scrutiny"
published_at: "2026-04-30T00:00:00+00:00"
first_seen_at: "2026-07-24T03:38:53.146880+00:00"
fetched_at: "2026-07-28T21:46:34.244883+00:00"
content_hash: "sha256:77bc3bbe4c26ab51337ef5e7ae263686e39328fa822ddf8ef669c000cfca7841"
---

# The EU AI Act and AML: How to prepare your program for the next wave of AI scrutiny

*By Dustin Eaton, Principal of Fraud & AML, Taktile*


If you work in AML, you already know the feeling: innovation crashes in waves, but regulation creeps in like the tide. Over the past few years, AI has moved from “interesting” to “embedded” across financial crime teams, especially in areas like transaction monitoring, investigations support, and sanctions screening.


At the same time, the EU has decided it no longer wants to rely on voluntary guidance and fragmented national approaches.


That is what makes the **EU AI Act, Regulation (EU) 2024/1689** , a watershed moment. It is widely viewed as[the first comprehensive, horizontal AI regulatory framework](https://artificialintelligenceact.eu/) in the world, and it sets a baseline that will influence how regulators and risk teams everywhere talk about AI governance.


The first point many institutions miss is scope. The AI Act is not “EU-only” in practice. Like GDPR,[its impact extends beyond the EU’s borders](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai) : if your AI system is placed on the EU market, or if its outputs affect people in the EU, the obligations can follow you.


For global banks, fintechs, and payment firms, that matters because AML operations are rarely neatly contained within one jurisdiction. Customer bases are international, vendor stacks are cross-border, and model development is often centralized.


In short: **if you are using AI to prevent or detect financial crime in an EU-facing context, the AI Act is now part of your AML risk landscape** , alongside prudential rules, conduct expectations, data protection law, and long-standing AML program obligations.


## **The AI Act’s risk-based framework and where AML fits**


The AI Act is built around a **risk-based classification** . While details and edge cases will continue to evolve through guidance, the core logic is stable: the more an AI system can influence people’s rights or safety, the more demanding the obligations.


In most summaries, you will see four tiers discussed:


- **Unacceptable risk** : Prohibited use cases.
- **High risk** : Permitted, but only with stringent governance and controls.
- **Limited risk** : Transparency obligations for certain interactions.
- **Minimal risk** : Largely unregulated under the AI Act.


So where does AML fit?


Here is the practical answer: **many AML use cases will feel “high-risk-adjacent” even when they are not explicitly listed as high-risk** .


In practice, while the AI Act explicitly exempts fraud detection systems from the Annex III high-risk list, the line blurs quickly. When AML transaction monitoring or customer risk scoring outputs drive downstream decisions like account closures, payment holds, or service denials, the functional impact starts to resemble the high-risk use cases the Act does cover.


Prudent institutions might treat these systems as if high-risk governance applies, even where the formal classification is arguable. AML systems influence access to services (account opening, account closures, payment holds), drive adverse decisions (exits, freezes, investigations), and can materially affect people’s lives. That’s the kind of downstream impact regulators might worry about.


At the same time, Annex III explicitly calls out financial-services-adjacent high-risk categories such as creditworthiness and credit scoring (and certain insurance risk assessments). The compliance challenge for AML teams is that modern financial crime stacks increasingly blend these domains:


- “Customer risk scoring” can influence onboarding decisions in ways similar to credit or eligibility scoring.
- Fraud and AML models often share data pipelines and features.
- Case management tooling can shape investigators’ decisions, prioritization, and narrative outcomes.


This is why financial-services guidance tends to emphasize that firms should not only look at labels, but also examine[how the AI output is used inside business processes](https://www.goodwinlaw.com/en/insights/publications/2024/08/alerts-practices-pif-key-points-for-financial-services-businesses) .


## **Provider vs. deployer: Compliance obligations for financial institutions**


One of the most operationally important distinctions in the AI Act is the difference between acting as a provider versus a deployer:


- A **provider** develops an AI system (or has it developed) and places it on the market or puts it into service.
- A **deployer** uses an AI system in its operations.


That distinction matters because it changes what your institution must be able to demonstrate: the maturity of your risk management system, the completeness of technical documentation, conformity assessment requirements, and how you monitor the system post-deployment.


In practical AML terms, there are two common patterns:


**1. In-house models and tooling**


Many larger institutions build bespoke transaction monitoring models, entity resolution, and alert prioritization. If you build and deploy internally, you are often straddling “provider-like” and “deployer-like” responsibilities.


That means your AI governance program needs to look more like a product-quality and compliance program, not just “model risk management light.”


**2. Vendor AI systems** If you purchase AML tools, you may think, “The vendor handles compliance.” But deployers still have meaningful obligations: you must use the system as intended, ensure human oversight is real, and maintain governance strong enough to detect drift, bias, or failures in real operations.


This is also where procurement and compliance teams need to converge. Vendor due diligence is no longer just about security questionnaires and SOC reports. Increasingly, it needs to cover things like: documentation availability, explainability artifacts, audit trails, and how post-market monitoring is executed in practice.


## **The core challenge for AI-driven AML will be maintaining transparency and explainability**


If there is one topic AML leaders typically underestimate, it is the gap between *model performance* and *regulatory defensibility* .


For high-risk AI systems,[transparency and “information to deployers” are central themes](https://artificialintelligenceact.eu/article/13/) in the AI Act’s architecture. And even outside strict high-risk classification, financial regulators increasingly expect that when AI influences critical decisions, the institution can explain:


- what data drove the outcome,
- what features were important,
- what controls prevented inappropriate outcomes, and
- what a human reviewer was expected to do.


AML teams feel this tension acutely because some of the most effective detection approaches can also be the least interpretable (for example, deep learning architectures). But the right question is not “black box or white box.” The right question is: **what level of interpretability is necessary for the decision context, the affected parties, and the regulator’s expectations?**


In my experience, the biggest gap isn't technical, it's that teams assume explainability is a model property when it's really a process property. I have seen teams spend months building an explainability layer for a model, only to realize the investigators never looked at the reason codes. You can have a perfectly interpretable model and still fail a regulatory exam if you can't show how that interpretability was actually used in case decisions.


In practice, institutions can often bridge the gap with a combination of the following:


- **Explainable AI techniques** that provide human-usable reason codes and sensitivity analysis,
- **Strong documentation and testing** (including bias and robustness testing),
- **Case-level audit trails** that show how an alert was generated and how it was dispositioned,
- **Human-in-the-loop design** that is genuine, not cosmetic.


The AML literature has long recognized that[AI can improve compliance outcomes](https://www.bu.edu/rbfl/files/2022/01/Shant-Vosgueritchian.pdf) , but only if institutions maintain governance and accountability structures that match the risk.


## **The interplay between the AI Act, GDPR, and the EU AML package**


AI governance does not live in a vacuum. For AML programs, the AI Act intersects most sharply with:


- **GDPR** , especially around lawful bases for processing, minimization, and safeguards when personal data is used in training and monitoring.
- The evolving **EU AML framework** , including supervisory convergence and new institutional structures.
- **Operational resilience** expectations, including DORA-style thinking about ICT risk and incident response.


One useful framing is that these regimes are not necessarily contradictory, but they are cumulative. The work is in integration: aligning governance, documentation, controls, and accountability so you can satisfy multiple regulatory expectations with one coherent program rather than multiple overlapping compliance “projects.” The EBA has emphasized this integration challenge for the banking and payments sector, and also highlights that further guidance is expected on high-risk classification and practical application.


## **Practical AI use cases in AML and their regulatory implications**


Let’s make this concrete. Below are common AML use cases and what “AI Act readiness” typically means in practice.


**1) Transaction monitoring (reducing false positives without losing coverage)**


AI can dramatically improve signal quality by learning patterns rules never capture. But that requires:


- documented objectives and risk tolerances,
- robust testing (including drift monitoring),
- clear human review workflows, and
- traceability from alert → features → outcome.


The FATF has emphasized that[new technologies can improve effectiveness](https://www.fatf-gafi.org/en/publications/Digitaltransformation/Opportunities-challenges-new-technologies-for-aml-cft.html) , but institutions must manage implementation risk and maintain sound governance.


**2) Customer risk scoring**


Customer risk scoring often acts as an upstream “router” that shapes downstream decisions (EDD triggers, review cadence, exits). That means the scoring system needs:


- defensible input data governance,
- bias testing where relevant,
- interpretability appropriate to the decision, and
- strong change management.


**3) SAR (or STR) narrative drafting**


Generative systems can accelerate drafting and standardization, but the governance focus shifts:


- prevent hallucinations and unsupported claims,
- ensure traceable citations to underlying case evidence,
- and enforce human validation before filing.


**4) Sanctions screening and entity resolution**


This is an area where AI can reduce operational burden significantly. In practice, deployers will need to demonstrate:


- quality controls on matching logic,
- clear escalation rules,
- and documentation that supports auditability.


Across all of these,[good industry guidance often converges on the same basics](https://www.duanemorris.com/articles/harnessing_artificial_intelligence_anti_money_laundering_compliance_1025.html) : cross-functional governance, vendor diligence, and rigorous monitoring and documentation.


## **International regulatory divergence: EU vs. US vs. FATF approaches**


A key challenge for multinational institutions is that **AI governance is fragmenting by jurisdiction** , even as financial crime risks are global.


- The **EU AI Act** is comprehensive and prescriptive. It aims to standardize obligations across sectors using a common taxonomy and explicit requirements.
- The **US approach** is more sector- and regulator-driven. Expectations often emerge through supervisory guidance, enforcement actions, and model risk management principles rather than one horizontal AI statute.
- **FATF** remains technology-neutral and risk-based. It pushes institutions to manage risk without dictating specific technical approaches.


For global institutions, this creates a real operational burden: policies, documentation, and monitoring programs must satisfy multiple “styles” of regulation at once. Firms that treat this as a governance design problem (rather than a set of separate compliance checklists) will be better positioned.


## **The road ahead: Implementation timeline and preparing for compliance**


The AI Act’s phased timeline is one more reason AML teams should start now. Prohibited AI practices have applied since February 2025, GPAI obligations since August 2025, and most high-risk Annex III obligations are set for August 2026, though the European Commission's Digital Omnibus proposal may push that deadline as late as December 2027 by linking it to the availability of harmonised standards and support tools. Even when specific obligations phase in over time, the capabilities you need (e.g. inventory, documentation, oversight, monitoring)[take longer to build than most institutions expect](https://www.moodys.com/web/en/us/insights/ai/ai-governance-navigating-eu-compliance-standards.html) .


A practical readiness program for AML and fraud teams usually includes:


**1. AI inventory and classification** Make a real inventory of AI systems in the AML stack. Include vendor tools, internal models, decision support systems, and genAI copilots.


**2. Governance and ownership** Define accountable owners (business, compliance, risk, and technology). Ensure model and system changes are controlled.


**3. Documentation and evidence pack** Build a repeatable “audit-ready” package: objectives, data lineage, test results, monitoring plan, and human oversight procedures.


**4. Testing beyond accuracy** Add bias, robustness, and failure-mode testing. In AML, the “cost of error” is not just a metric. It is regulatory exposure.


**5. Vendor due diligence upgrade** Treat AI governance as a vendor requirement, not a nice-to-have.


## **Compliance is now a competitive advantage in AML**


It is tempting to frame the EU AI Act as another layer of “burden” on already stretched compliance teams. I'd push back on that framing: for institutions that are serious about using AI in AML, the Act is a forcing function to do the work that should have been done anyway.


Done well, **compliant and explainable AI can become a competitive differentiator** :


The institutions that will win are not the ones that use the most AI. They are the ones that can prove it.


The institutions that will win are not the ones that “use the most AI.” They are the ones that can prove why the AI is trustworthy, how it is controlled, and where humans remain accountable.


## Ready to strengthen your AML programs?


[Book a meeting](https://taktile.com/demo)
