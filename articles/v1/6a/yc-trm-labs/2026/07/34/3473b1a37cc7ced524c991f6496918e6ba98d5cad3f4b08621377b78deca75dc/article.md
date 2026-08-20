---
schema_version: "1.0.0"
document_id: "3473b1a37cc7ced524c991f6496918e6ba98d5cad3f4b08621377b78deca75dc"
company_key: "yc-trm-labs"
company: "TRM Labs"
source_id: "yc-trm-labs-news-import-b34814ebf689"
canonical_url: "https://www.trmlabs.com/resources/blog/blockchain-evidence-in-court-a-framework-for-investigators"
published_at: "2026-07-10T21:10:00+00:00"
first_seen_at: "2026-07-22T17:19:07.599741+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:ca96b4dd3b9458ad929fe16e53a3a9e672c12b4b3eb7d5b0a648ffbd88a2e2e6"
---

# Blockchain Evidence in Court: A Framework for Investigators

In a typical investigation, the investigator who traces the funds is rarely the same person who builds the attribution database those funds are matched against. The investigator who screenshots a[TRM Forensics](https://www.trmlabs.com/blockchain-intelligence-platform/forensics) graph is not the author of the clustering logic that drew it. And the investigator who testifies to a wallet's link to a sanctioned actor is, in nearly every case, citing someone else's work for that final link.


This matters because the use of[blockchain intelligence](https://www.trmlabs.com/glossary/blockchain-intelligence) in court has provenance. The testifying investigator must be able to explain the origin of every fact in his or her report and must also defend those findings on cross-examination. Blockchain investigations involve at least three different sources of layered evidence; conflating them is a common reason that cases that should have held together fall apart in litigation.


This post is for blockchain investigators. It provides a framework for keeping evidence layers straight, confining testimony to its permissible scope, and building a record that can survive the scrutiny of sophisticated defense counsel.


## Key takeaways


- Investigators are typically presented as fact witnesses, not experts. Providing expert testimony without formal designation can lead to evidentiary exclusion.
- Tool selection is an evidentiary decision, because the vendor's defensibility becomes the investigator's defensibility. Apply reproducibility, transparency, and version-control tests before committing.
- Blockchain evidence sits on three layers: on-chain data (verified directly), analytical output (produced by the tool's methodology, but only becomes the investigator's evidence once corroborated), and attribution (sourced from the vendor). Conflating them breaks the record.
- Analytical conclusions (Layer 2 blockchain evidence) can become the investigator's evidence only if independently corroborated, for example, through a co-spend spreadsheet built and verified by the investigator.
- Attribution requires proper legal provenance through, for example, appropriate legal process that produces admissible records.
- The address itself is the most volatile input, so every finding must be anchored with a timestamp and block height.


‍


{{horizontal-line}}


## What an investigator can say in court


Courts make a critical distinction between a witness of fact and an expert witness.


**Fact witnesses** describe what they did, what they observed, what tools they used, and what the tools produced. They cannot offer opinions, interpretations, or expert framing of the data. If a report concludes "this address belongs to a sanctioned exchange," a witness of fact cannot testify to or defend that statement on the stand — they can only describe how they obtained it.


An investigator becomes an **expert witness** only after meeting the jurisdiction's qualification standard — court-appointed or party-retained — based on training, prior testimony, demonstrated methodology, and peer-recognized expertise. An expert can offer opinion evidence and is expected to defend the methodology behind it.


The boundary between the two is easy to cross unintentionally. For example, if an investigator writes a report using language like "this transaction pattern indicates layering," or "the destination wallet is a known mixer," that phrasing is considered opinion evidence. If the investigator hasn't been designated as an expert in the particular matter, the statement cannot appear in the report or testimony. Accordingly, the cross-examination question that follows, "by what authority do you reach that conclusion?", is one a witness of fact cannot answer.


It's vital to know which role you occupy in the matter *before* writing a single word of a report — and to stay inside those boundaries.


## What counts as a fact in blockchain evidence?


Blockchain investigations sit on three layers of evidence, each with a different owner.


### **Layer 1: On-chain data**


Transaction hashes, addresses, balances, block heights, timestamps, and[smart contract](https://www.trmlabs.com/glossary/smart-contract) calls. Any competent investigator with node access or a block explorer can verify these pieces independently. When investigators testify to a transfer between two addresses at a specific block height, they are testifying to a fact they can defend directly.


### **Layer 2: Analytical conclusions**


Clustering, behavioral pattern recognition, typology mapping, and hop-counting through transactional chains. The investigator ran the analysis, but the underlying clustering logic is the tool's methodology, not the investigator's direct observation. This is why an uncorroborated cluster claim is the tool's output, not the investigator's evidence.


A witness of fact can still absorb a Layer 2 claim into their own evidence — but only by corroborating it independently. If the tool clusters two addresses together, the investigator can build a master spreadsheet of the co-spending transactions that justify the cluster, work through each link manually, and present the corroborated cluster as a fact they verified themselves. A cluster cited without that corroboration remains the tool's claim, and a defense expert will press on the underlying methodology rather than the investigator's report.


### **Layer 3: Attribution**


Statements about who controls an address or a real-world identity; this is the link to a named exchange, sanctioned entity, or alleged perpetrator. Attribution typically rests on a vendor's labeled database, open-source intelligence, or subpoena returns from a counterparty.


Incorporating attribution into a report is easy — but defending it can be more complex. A bare citation — "the address is identified as Exchange X in the vendor's database" — will not survive a competent cross-examination. The defensible path is to obtain the underlying evidence through legal process, for example, a court order to the vendor for the basis of the attribution, a subpoena to the named exchange confirming control of the address, or a counterparty disclosure that anchors the link to a source with custody and provenance. The vendor label is a starting point for the investigation, but the court-ordered or subpoenaed evidence is what makes it usable in an evidential report.


## Calibrate language to the scope of your voice


Use plain language anchored in action. State what you did, what you observed, and where information came from. Avoid forensic-sounding verbs that imply more than your role authorizes.


A fact witness report should read like a record of actions and observations, not a characterization of meaning. "I queried TRM Forensics for the address, and the output identified it as belonging to Exchange X" is defensible because it describes the action and names the source. "The address is consistent with mixer activity" is not, because "consistent with" is an opinion, and a fact witness cannot provide opinion testimony.


Specific verbiage to avoid:


- **Forensic terms of art:** "Consistent with," "indicative of," and "characteristic of" sound precise but carry implicit professional opinion. Defense counsel will press on what the term means and on what basis it was used.
- **Probabilistic language without methodology:** "Very likely," "almost certainly," or "highly probable." If the analytical method does not produce a quantifiable confidence value, do not generate one in prose.
- **Conclusory shortcuts:** Definitive phrases like "this is a mixer" or "these addresses belong to the same person" are conclusions that depend on methodology and attribution work. A fact witness can describe what the tool reported, but should not adopt the tool's conclusions as their own statements.


> Evaluate every sentence in your report to confirm whether it describes an *action taken* or an *observation made* , or whether it characterizes *what something means* . The former is defensible; the latter is opinion (which requires expert designation).


When a Layer 2 conclusion has been independently corroborated by the investigator's own work, the framing flips. The investigator describes the corroboration first and cites the tool second. For example, "I built a spreadsheet of 47 co-spending transactions linking the addresses; TRM Forensics' cluster output corroborates this finding," is the investigator's evidence with tool corroboration. "TRM Forensics clusters these addresses" is the tool's conclusion that the investigator has not corroborated.


## Preserve the chain of custody for blockchain evidence


Blockchain analysis is never static: the ledger continues to grow, clustering models update as new heuristics are deployed, and attribution databases change as new intelligence arrives. An analysis run today may produce different outputs than one re-run six months from now.


Wallet addresses are the most volatile inputs of all. A wallet that held 12 bitcoin (BTC) at the time of analysis may be drained an hour later, or topped up overnight. A "dormant" address may receive funds the day after the report is finalized. Or an "empty" address may be the next destination of[laundered proceeds](https://www.trmlabs.com/glossary/money-laundering) . The blockchain doesn't pause for litigation. By the time a case reaches trial, the on-chain state may have changed materially — and the defense will use those changes to challenge findings that were accurate at the moment of analysis.


To mitigate this risk, investigators should use temporal qualifiers in their reports and preserve inputs for reconstruction.


### Use temporal qualifiers in the report


Temporal qualifiers are timestamps and block heights that pin a finding to a precise moment in time, replacing present-tense claims about what *is* with past-tense statements about what *was* at a specific block. For example:


- Temporal: "The address held 12 BTC as of block 812,453, recorded at 14:32 UTC on 24 May 2026."


- Not temporal: "The address holds 12 BTC."


- Temporal: "The funds reached this address at block 812,453 and remained there as of the analysis."


- Not temporal: "The funds are at this address."


Treat tense and timestamps as evidentiary boundaries. In your report's methodology section, state the date and block height to which every finding is anchored, so readers aren’t left guessing whether a claim is current or historical.


### Preserve the inputs for reconstruction


Preserve every input that was used in the production of your report:


- Tool version, with date and time of analysis logged
- Observations anchored to specific block heights (rather than relative descriptions like "the most recent block")
- Address state captured contemporaneously — balances, transaction history, and attribution labels — as it appeared at the moment of analysis
- Screenshots, exports, and tool outputs captured at the time of analysis and stored in the case file
- Underlying transaction data — hashes, addresses, raw call data — preserved independently of the analytical tool
- Analytical workflow, not just the conclusion (e.g. which queries were run, in what order, with what parameters)


Reconstruction is the standard: 18 months after the original analysis, could another investigator load the preserved inputs into the same tool version and reproduce the result? If the answer is uncertain, the record is not yet[defensible](https://www.trmlabs.com/glossary/defensible-blockchain-attribution) . When the case reaches trial, the investigator should expect to be asked about state changes since the analysis was performed — and be prepared either to re-run the analysis against current data or to defend why the snapshot remains the relevant evidentiary frame.


## Choose tools whose outputs you can defend


Tool selection is itself an evidentiary decision. Because Layer 2 and Layer 3 evidence inherits the vendor's methodology, the vendor's defensibility becomes the investigator's defensibility. For teams evaluating blockchain intelligence platforms, there are three key requirements to consider: reproducibility, transparency, and version control.


### 1. Reproducibility


Given the same inputs and tool version, does the tool produce the same outputs? Does another investigator with the same access reach the same conclusion?


### 2. Transparency


Does the tool surface the basis for its outputs, or does it provide only a conclusion? An attribution label of "high risk" with no underlying source, transactional evidence, or analytical reasoning cannot be defended on the stand. An attribution accompanied by its sources can be examined.


> [Glass box attribution](https://www.trmlabs.com/glossary/glass-box-attribution) is a feature of TRM's blockchain intelligence platform that provides full transparency into how attributions are derived — surfacing underlying sources, confidence levels, and reasoning behind each attribution. This is in contrast to black box approaches that produce conclusions without explaining how they were reached.


### 3. Version control


Are outputs date- and version-stamped? Can a specific finding be tied to a specific version of the tool's data and logic? A tool whose attribution model silently updates without versioning makes contemporaneous evidence preservation impossible.


[TRM Forensics](https://www.trmlabs.com/blockchain-intelligence-platform/forensics) is built around these properties — version-stamped outputs,[glass box attribution](https://www.trmlabs.com/glossary/glass-box-attribution) and source-attributed labels, and reproducible analytical workflows. Agencies have used TRM Forensics in evidence across criminal, civil, regulatory, and sanctions matters in jurisdictions around the world — both in matters where TRM provided direct expert support and in matters where the agency operated the tools independently.


## Engage prosecutors early — and clarify your role


Most evidentiary problems in[blockchain cases](https://www.trmlabs.com/resources/blog/building-strong-cases-with-blockchain-evidence-admissibility-chain-of-custody-experts-and-court-ready-reporting) are sequencing failures, where the report is written before the witness designation is settled, legal process for attribution has been initiated, or prosecutors know the limitations of the final analysis. By the time those gaps surface, the record is already built around them. Three conversations need to happen before the report is drafted.


### 1. Settle the witness designation


Will the investigator be presented as a witness of fact, or will the prosecution seek expert designation? The answer shapes what the report should and should not say. A fact witness’ report describes the trace and cites the tool's outputs. An expert witness's report can also offer opinions and defend the methodology, but only if the court qualifies the witness as an expert.


### 2. Identify the legal process required


Will subpoenas to the vendor be needed to anchor the basis for attribution? Will court orders to counterparty exchanges or service providers be required to convert tool-cited identities into properly sourced evidence? Will additional co-spend or transactional data need to be obtained from a custodian to support independent corroboration of clusters?


### 3. Surface the questions the court will ask


How was the address linked to the suspect? Could the funds have been moved by someone else with access to the same wallet? What is the basis for each attribution claim? Surfacing these questions early shapes both the analysis and the legal process — and lets the investigator collect corroborating evidence at the time of the trace, when it is easy.


## Testify within your designated role


In court, the investigator who performed the analysis is the person who has to defend it. The preparation differs depending on the witness' role.


### As a witness of fact


Prepare to describe what was done, what was observed, what tools were used, and what those tools produced. Stay in the lane of direct experience. If asked to characterize, interpret, or opine (e.g. "In your professional judgment, is this a typical pattern?"), the appropriate answer is to defer to the limits of the witness role. Resisting the pull to over-answer protects the entire record.


### As a designated expert


Prepare to defend the methodology in plain language. Walk through analytical steps without invoking brand names as a substitute for reasoning. Disclose what the tool does and does not do (e.g. attribution depends on off-chain corroboration; the tool does not produce real-world identity by itself). Distinguish what the tool reported from what the investigator concluded.


The cross-examination trap is the mixing of roles. A witness of fact who slips into expert language opens the door to having the entire characterization struck. An expert who relies on the tool's brand reputation in place of methodology defense gives opposing counsel the opening to challenge qualification.


‍


{{horizontal-line}}


## Frequently asked questions


### 1. What is the difference between a witness of fact and an expert witness in blockchain investigations?


A witness of fact describes what they did, what they observed, and what the tools they used produced — without interpreting or characterizing what the data means. An expert witness, only after formal court designation, may offer opinion evidence and is expected to defend the underlying methodology. Unintentional crossing of these distinct roles is a common source of evidentiary exclusion.


### 2. Can an investigator use blockchain analytics tools without being an expert witness?


Yes. A witness of fact can use, cite, and reference the outputs of a[blockchain analytics](https://www.trmlabs.com/glossary/blockchain-analytics) tool, provided the report describes what the tool produced and avoids characterizing what those outputs mean. Independent corroboration of analytical conclusions — for example, manually verifying a clustering result with a co-spend spreadsheet — can convert tool output into the investigator's own evidence.


### 3. Is a vendor's attribution label enough to use in court?


No. A bare citation stating that an address is identified as a specific entity in a vendor's database will not survive competent cross-examination. The defensible path is to obtain the underlying evidence through legal process — court orders to the vendor, subpoenas to the named counterparty, or counterparty disclosures that anchor the link to a source with proper custody and provenance.


That said, a vendor's attribution label alone is often sufficient to open an investigation or support a warrant application, where the standard is reasonable suspicion or probable cause rather than trial-admissible evidence. The legal process above becomes necessary when that attribution needs to hold up as evidence at trial, not at the investigative stage.


### 4. How long should the analytical chain of custody last?


A defensible record should allow another investigator, 18 months or more after the original analysis, to load the preserved inputs into the same tool version and reproduce the result. The standard is reconstruction from preserved artifacts, not reliance on memory or live re-querying.


### 5. What language should an investigator avoid in a blockchain evidentiary report?


Forensic terms of art such as "consistent with," "indicative of," and "characteristic of" carry implicit professional opinion that a witness of fact has no standing to use. Probabilistic claims like "very likely" or "almost certainly" should not appear unless the analytical method produces a quantifiable confidence value. Conclusory shortcuts such as "this is a mixer" adopt the tool's characterization as the investigator's own statement, which a fact witness should avoid.
