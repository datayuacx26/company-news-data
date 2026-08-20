---
schema_version: "1.0.0"
document_id: "700448482d7a3a1013216a4a453e26fc73ce5a5c85d269170b60fa88d73b7b93"
company_key: "pegasystems-inc-common-stock"
company: "Pegasystems Inc."
source_id: "pegasystems-inc-common-stock-news-import-897a88525e2a"
canonical_url: "https://www.pega.com/insights/articles/decisions-you-can-defend-rethinking-kyc-age-agents"
published_at: "2026-08-13T12:00:00+00:00"
first_seen_at: "2026-08-18T19:04:24.123936+00:00"
fetched_at: "2026-08-18T19:04:26.451896+00:00"
content_hash: "sha256:1732fe9d1fb18aa5abec4edb4caa733e1d10f8cd799a4e93c1efd9be0a20f2fc"
---

# KYC decision intelligence: Building defensible decisions

## The decision is the regulated artifact


*Why the decision – not the model, not the data – is the regulated artifact, and why explainability lives in the workflow rather than the algorithm.*


In Part 1 we argued that the race to onboard faster optimizes the experience while also being the least consequential moment in the client relationship from a regulatory standpoint. The better question is how to make, govern, and evidence risk decisions across the whole lifecycle. That question raises an obvious follow-up: Of everything an institution builds and buys, what is the part that actually has to hold up?


Most Know your customer ([KYC](https://www.pega.com/industries/financial-services/know-your-customer) ) transformation budgets of the last 10 years went into inputs: more data, more sources, more documents, better screening lists, document-extraction models, beneficial-ownership graphs, perpetual-KYC signal feeds. All useful and needed, though none is the thing actually under supervision. So what is?


A regulator does not examine your data lake. An examiner does not grade your model’s[AUC](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc) – a high AUC tells you a model discriminates well on average, but says nothing about whether you can *defend a specific decision* about a specific client.


What an examiner questions is a *decision* – why this client was rated high risk and that one was not, why this alert was closed and that SAR was filed, who approved the exception and under which version of which policy. The regulated artifact has always been the decision and its defensibility. Everything else is evidence in service of it.


This is the quiet failure in most architectures. Today they are rich in data and models and yet often light in decision explainability and traceability. They can tell you the answer but not reconstruct how the answer was reached – precisely the question that gets asked in an exam, a lookback, or litigation. An institution that cannot reproduce the conditions of a past decision does not really have a control; it has an outcome it hopes was right.


And the decision is rarely about a single, tidy entity. The “who” in Know your customer is usually a network: beneficial owners, officers, counterparties, brokers, agents, suppliers, related parties, the layers of a multi-jurisdiction group structure. Risk lives in the relationships between those parties at least as much as in any one of them, and the picture only resolves when you can see all of them together. Multi-country due diligence sharpens the point: the same entity is read against different registries, different ownership-disclosure regimes, different sanctions and tax rules, and different definitions of what even counts as a controlling interest. Assess one node in isolation, through one jurisdiction’s lens, and you are not looking at the real risk – you are looking at a fragment of it and calling it the whole. A financial firm has to hold that whole-network, multi-jurisdiction view, or the decisions it records are confidently wrong.


The institutions pulling ahead treat CLM-KYC as a unified system for *decisions* , not for KYC data. That is a meaningfully different design goal,and very few platforms are actually truly built for it.


## Explainability is a property of the process, not the model


There is a comfortable myth in the industry that you can take a black-box model, bolt on a layer of feature-importance scores, and call the result “explainable AI”. In a regulated KYC context, this is close to meaningless.


What an examiner wants is not a heat map of which variables nudged a score. It is the ability to reconstruct the decision in full: the policy in force at the time, the thresholds that applied, the data that was used, who reviewed it, the escalation path it travelled, and – crucially – what the alternative outcomes were and why they were rejected. All of which is recoverable from the *workflow* the model operated inside.


This is the insight that inverts the whole AI conversation. Explainability is not something you extract from a model after the fact. It is something the surrounding process either provides by design or never provides at all. The most perfect model in the world, dropped into an ungoverned path whose output flows straight into action – no record of which policy applied, which thresholds were used, what data it saw, or who approved the result – produces decisions you cannot defend, because nothing around the model ever captured why.[A model inside a governed workflow that records all of that produces decisions you can](https://www.pega.com/predictable-ai) .


It matters here that KYC decisions are asymmetric in a way most AI use cases are not. A bad KYC decision can debank a legitimate business, exclude a person from the financial system, or file a suspicious-activity report against someone who will never see it and cannot contest it. The party harmed by an unexplainable wrong decision usually has no recourse. That asymmetry raises the bar far above “accurate enough.” The standard must be *defensible* , and defensibility is earned in the process, not the model.


# Next in the series →


*Knowing the decision must be defensible is one thing; building AI you can actually trust to make or inform it is another. In Part 3 – “Predictable AI, and the operating model that follows” – we get concrete: how to put guardrails around agentic automation, and how a governed lifecycle makes most KYC work simply disappear.*
