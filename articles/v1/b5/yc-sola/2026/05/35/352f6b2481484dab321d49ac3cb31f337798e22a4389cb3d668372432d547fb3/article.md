---
schema_version: "1.0.0"
document_id: "352f6b2481484dab321d49ac3cb31f337798e22a4389cb3d668372432d547fb3"
company_key: "yc-sola"
company: "Sola"
source_id: "yc-sola-news-import-2dc63e1087a6"
canonical_url: "https://sola.security/insights/ai-native-asset-intelligence/"
published_at: "2026-05-19T12:18:17+00:00"
first_seen_at: "2026-07-24T01:34:47.750416+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:660a35a7f57f36296d007c86d8c09fe30752d03edfa83c19d3d9f19b1d6f8263"
---

# Research: AI-native asset intelligence- Sola Security

[Read the full research on arXiv (PDF download)](https://arxiv.org/pdf/2605.09115)


## AI security prioritization keeps drifting


You have probably seen the pattern. You ask your AI security assistant for the top five risky S3 buckets, and the next morning you ask the same question and get a different list. Nothing changed in the environment overnight. The model did not get worse. So what produced the drift?


The instability is not a model problem. It is a data architecture problem. AI security assistants reason directly over signals pulled from cloud providers, identity systems, code platforms, and security tools, none of which were designed to be compared against each other. Each source carries its own severity scale, its own context, and its own assumptions about what an asset even is. When a model is asked to rank assets across that fragmented surface, it has no stable basis for comparison. Two queries can return two different answers because the underlying data has no shared representation.


Standard severity frameworks in security do not fill the gap. CVSS was built to express vulnerability severity in isolation, and static CIS-style severities were built to flag configuration violations against rules. Both are useful for what they were designed for. But neither captures the factors that determine real impact: whether the asset is reachable, what it depends on, what role it plays in the business, what data it holds.


The result is a reasoning layer asked to do work the data underneath cannot support, and the fix should start lower in the stack. What is structurally missing is an intelligence layer that organizes assets, identities, relationships, and surrounding *context* into a single representation before any AI sits on top of it.


## An intelligence layer above the data


Most current approaches do the opposite. They place a more capable assistant on top of the same fragmented data and trust the model to reconcile what the data does not. Bigger models, better prompting, more retrieval. The substrate underneath stays unchanged, so the same instability shows up no matter how capable the assistant becomes.


Our research argues the inverse. The intelligence layer comes first, and it is itself AI-native. AI then reasons over a structured representation rather than directly over raw signals. The architectural move is to invest in the layer before investing in the reasoning that sits on top of it.


In plain terms, an intelligence layer for AI for security posture is a canonical representation of assets, identities, relationships, controls, attack vectors, and blast-radius patterns. It defines what an asset is, how assets connect to each other, what counts as a control violation, what counts as a reachable attack path, and what counts as downstream impact. Once those definitions are stable and shared, comparing one asset to another becomes a well-formed operation rather than an open-ended interpretation.


The framework organizes that representation into two cooperating sub-layers: A **modeling** layer builds it, ingesting heterogeneous data and lifting it into a structured form. A **scoring** layer converts the representation into a single bounded measure of asset importance. The two sub-layers cooperate but solve different problems.


The next two sections walk through each in turn, starting with how the modeling layer constructs the representation that everything else depends on.


## Modeling: how the intelligence layer is actually built


The modeling layer starts with vendor-agnostic ingestion. We pull configuration, identity, and telemetry data from public-cloud control planes (AWS, GCP, Azure), identity providers (Okta and similar), code-hosting platforms, and third-party security tooling. Provider-specific extractors normalize raw API responses into a vendor-neutral form and persist them to a versioned columnar store. Every downstream computation is then reproducible against a specific snapshot.


The tabular data is lifted into a typed property graph. Nodes represent assets (identities, workloads, data stores, network endpoints, code artifacts), and edges represent semantically meaningful relationships between them: ownership, identity assumption, data flow, network reachability, configuration attachment, control attestation.


A vulnerability rarely sits in isolation. It sits inside an execution path, connected to identities and downstream systems in ways that determine whether it can be reached and what it can touch. The graph captures those connections explicitly so AI has something structured to reason over.


On top of the asset graph, we maintain three classes of security knowledge as overlays: *security controls, attack vectors,* and *blast-radius patterns* . Controls express posture requirements as queryable predicates. Attack vectors describe how an adversary may reach or compromise an asset. Blast-radius patterns enumerate what downstream assets are exposed if a given asset is compromised.


The distinctive piece is how those overlays are generated. Manually authoring them at the scale of a real cloud environment is not feasible. So we treat their generation as the work of an **agentic researcher** . The agent samples the live data to learn schemas and idioms, grounds its hypotheses in public knowledge bases like MITRE ATT&CK, CWE, and CAPEC, and validates candidate patterns against the asset graph itself.


Every artifact then passes through human review by Sola Research’s security experts before promotion. The design scales knowledge generation while keeping each artifact auditable. The graph and its overlays are what the scoring layer then operates on.


## Scoring: separating what is exposed from what actually matters


The scoring layer’s central move is separating intrinsic exposure from contextual importance. Intrinsic exposure asks whether an asset is technically at risk. Contextual importance asks whether that risk actually *matters* . Treating them as a single signal collapses information the framework needs to keep distinct.


Intrinsic exposure has two channels. The first is misconfiguration findings: failed security controls observed on the asset. The second is reachable attack vectors: concrete patterns by which an adversary may compromise the asset. The two channels are combined as a maximum rather than a sum, because misconfigurations and attack vectors often describe overlapping evidence for the same underlying weakness. Adding them would double-count.


The misconfiguration channel also goes through a refinement step. Vendor-assigned severities and static CIS-style scores are produced without knowledge of the specific asset, its relationships, or its surrounding environment, so an agent re-evaluates each failing control against the asset’s actual context, with public taxonomies like MITRE ATT&CK, CWE, and CAPEC available as grounding.


The agent may downgrade a finding when the asset is intentionally configured the way the control flags. It may also retain the severity when it matches the asset’s reality, or amplify it when the asset’s role makes the finding more consequential than the label implies. The misconfiguration channel then reflects what a finding actually means in context, not what the rule said in the abstract.


Contextual importance is built from four components: configuration anomaly, blast radius, business-function criticality, and data criticality. A missing-MFA finding on a developer test account is not the same problem as the same finding on a cloud administrator with broad access. The control label is identical. The operational reality is not. Contextual importance is what makes that difference visible in the score.


## A worked example: scoring a publicly writable S3 bucket


One asset from the evaluation makes the pipeline concrete: a publicly writable S3 bucket configured to trigger a Lambda function on new object creation. External principals can upload objects, and those uploads initiate downstream compute. The bucket carries two findings: public access labeled High, and public PutObject labeled Critical.


The misconfiguration channel evaluates those two findings together. Because severities accumulate with diminishing returns under a bounded cap, two severe findings stack into a high misconfiguration exposure without saturating the channel. The score reaches 0.75.


The attack-vector channel reflects a different kind of evidence. The bucket sits inside a concrete exploitability pattern (adversary-controlled uploads can trigger downstream compute), which is not a generic hygiene issue but a specific reachable path. The attack-vector channel rises to 0.811.


The base exposure takes the maximum of the two channels, 0.811. The maximum (rather than a sum) prevents the same underlying weakness from being counted twice when both channels surface it.


In this example, **context multiplies the score** . The data in the bucket is regulated, the bucket sits in production, and the blast radius extends past the bucket into the Lambda execution path. The four contextual components (configuration anomaly, blast radius, business-function criticality, data criticality) combine into a context multiplier that lands at 1.14, inside the bounded range the framework allows. Context can amplify or attenuate exposure within set limits, but it cannot generate risk where there is no exposure.


The final score is 0.925.


The point of walking through the numbers is not the math itself. The point is that every component has a defined meaning, every component is bounded, and every adjustment can be traced back to a specific input. Misconfiguration evidence, attack-vector evidence, and contextual importance each contribute in a documented way. A reviewer can ask why the score landed where it did and get an answer that does not collapse into “the model decided.”


We tested the scoring components against a production snapshot containing 131,625 resources across 15 vendors and 178 asset types. The S3-Lambda asset is one illustrative case from that environment, chosen because every part of the pipeline contributes to its final score.


## Where AI belongs in this frame, and where it does not


AI inside the framework has a defined job. It interprets asset-specific context. It may downgrade a public-access finding when the bucket is intentionally serving as a CDN, or amplify the same finding when the bucket holds regulated data. AI also classifies semantic dimensions, like whether an asset plays a core business-function role or what kind of data it holds.


What AI does **not** do is assign the final risk score. The deterministic model does. The split is the point; AI used as an unconstrained ranking oracle produces the same drift practitioners are already experiencing: ask twice, get two answers. AI used as a bounded refinement inside a structured scoring frame produces outputs a reviewer can trace, reproduce, and challenge.


Our evaluation reinforces this. We measured what AI-based severity adjustment actually does to the misconfiguration channel on resources where the adjustment was active, across several severity-weight configurations. The dominant pattern is refinement in the lower and middle ranges of the score distribution, not the creation of new extreme scores. The mechanism behaves like a calibration step, not a ranking oracle.


The design choice is deliberate: AI works as a bounded contextualizer, not an unbounded ranker. AI maps heterogeneous evidence into the structured, bounded scoring inputs the deterministic model then aggregates. The model preserves the final-score semantics. AI improves the interpretation of the signals it feeds into that model. Each role has a defined boundary, and the framework’s stability depends on keeping them distinct.


## Proactive security is downstream of stable reasoning


Proactive insight generation is usually framed as a separate capability. Surface the risky assets before anyone asks. Bring the answer to the analyst rather than waiting for a query. Our research argues something different: proactive behavior is not a feature to bolt on. It is what becomes possible once the reasoning underneath is stable and reproducible.


A system that returns different rankings when asked the same question twice cannot reliably surface the right assets without being asked. The instability that shows up in interactive queries also shows up when the system tries to act on its own. Reliable inference is the prerequisite for reliable proactivity. Our framing is direct on this point.


Once the substrate is stable, proactive behavior follows. The system can continuously evaluate the environment against a fixed scoring frame, identify resources where exposure and context align, and surface them without depending on the analyst to ask the exact right question. The work of figuring out which assets deserve attention moves from the practitioner’s queue into the system itself.


The evaluation has a defined scope, and the framework’s claims sit inside it. Our evaluation is behavioral: sensitivity studies and ablations on a single production snapshot showing the components behave as designed.


Future evaluations will examine business-context accuracy and predictive validity across additional organizations. The current contribution is the framework and the evidence that its components work the way they are supposed to.


The full paper is available on arXiv, where you can drill down into the conceptual architecture, the formal scoring model, and the full set of behavioral evaluations.


## Get the full research on arXiv


The same framework also runs inside Lumina Signal, where the modeling layer, the scoring system, and the AI contextualization described in this piece operate on live customer environments. You can find more about Lumina Signal[here](https://sola.security/lumina/) .
