---
schema_version: "1.0.0"
document_id: "28cccf1dc0144406062692c55b07c81008836d8f243c0f5c2a2b491fa05920d6"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/what-is-legal-ai-really"
published_at: "2026-04-26T00:00:00+00:00"
first_seen_at: "2026-07-22T03:05:30.029696+00:00"
fetched_at: "2026-07-28T21:56:40.338047+00:00"
content_hash: "sha256:c3e798844a607a6abe55b68fc26109619bd1a5251d5f8126768768bd29ef0223"
---

# What Is Legal AI, Really?

This is for the attorney who searched the question directly. We get asked it on roughly every fourth call: "What is legal AI, really?" The honest, direct answer is below, written for a partner deciding whether to deploy.


## The category


Legal AI is software that reads, analyzes, drafts, and reasons about legal documents using current-generation language models, packaged into workflows attorneys actually use. Two parts of that sentence matter:


- **Current-generation language models.** The same Claude, GPT, Gemini etc. that power consumer AI products. The models are commodity-trending; almost every serious legal AI product is running similar engines underneath.
- **Workflows attorneys actually use.** This is where the products differ. The chassis around the engine is what determines whether the tool ships partner-grade output or generates plausible-looking hallucinations.


We wrote about the chassis-versus-engine framing in[The F1 Engine Problem](https://magelegal.com/blog/f1-engine-problem) . The TL;DR: stop comparing legal AI tools on which model they use. Compare them on the chassis.


## The three categories


Inside the category, three sub-categories matter. The differences are about scope and shape, not quality.


### 1. Generic LLMs (ChatGPT, Claude, Gemini, etc.)


What it is: the consumer product, used directly by attorneys without any legal-specific layer.


Good for: first-draft writing assistance on non-privileged content. Brainstorming. Reformulating dense legalese into client language. Drafting non-controversial provision language.


Not good for: production diligence. Generic LLMs hallucinate clause references, cannot resolve amendment chains, have no privilege posture by default, and have no workflow. The partner who relies on them to find issues will miss them.


When to use: low-stakes writing assistance. Stop short of production legal work.


### 2. Firm-wide assistants (Harvey, Legora, similar)


What it is: legal-specific AI assistants designed to serve a whole law firm across practice areas.


Good for: cross-practice coverage. Question-answering across the firm's document corpus. First-draft writing assistance with legal grounding. Research support. Memo drafting at the level of a competent first-year associate.


Not good for: owning a specific practice's deep workflow end-to-end. A firm-wide assistant is positioned as a copilot for everyday legal work, not as the platform that runs a deal team's diligence workstream.


When to use: a firm whose primary AI need is broad coverage across multiple practices.


### 3. Practice specialists (Mage for M&A, others for litigation discovery, contract management, etc.)


What it is: AI tools built around a specific practice area's full workflow.


Good for: end-to-end ownership of a practice's daily work. For M&A specifically, Mage covers data room ingestion, risk-driven document review, amendment chain resolution, disclosure schedule synthesis, memo drafting, redline review, and post-signing covenant tracking — the full deal-team workstream.


Not good for: cross-practice generality. A specialist tool is the wrong place to ask about your firm's litigation precedents.


When to use: a practice whose deal volume is high enough to justify a tool built around its specific workflow.


The categories are not substitutes for each other. Many large firms run a generalist plus a specialist for high-volume practices. Picking the right category is the first decision.


## What legal AI can actually do, in 2026


Concrete capabilities, with sourced references where applicable:


- **Ingest a 1,500-document data room** in under an hour, with documents classified by type and prioritized by partner-defined relevance.
- **Run a partner-defined risk pass** against every contract: change-of-control, indemnity caps, anti-assignment, MFN, audit rights, exclusivity, termination, MAC, IP assignment, and dozens more. Output is a sortable findings list with citations to source.
- **Resolve amendment chains.** Reconstruct the current operative state of a contract that has been amended five or fifteen times. We covered the technical hard problem in[Amendment Chain Resolution: The Hardest Problem in Legal AI](https://magelegal.com/blog/amendment-chain-resolution-hardest-problem-legal-ai) .
- **Draft disclosure schedules** from the underlying agreements. Sell-side time spent on schedules drops from 80-120 hours to 20-30.
- **Draft deal memos in firm voice.** Partner-reviewable first drafts with citations to the source clauses.
- **Compare counterparty redlines** against firm preferred positions, surfacing material deviations and proposing language.
- **Track post-signing covenants** through the closing window. The interim covenants that get missed when teams use spreadsheets.


None of these was production-quality in 2023. All of them are production-quality now, in serious tools. The marketing claim and the reality have converged.


## What legal AI cannot do


This is the more important list, because it sets the boundary on the work that is not changing:


- **Make materiality calls.** "Is this a real issue or boilerplate?" is judgment.
- **Negotiate.** Knowing what to push on, when, with which leverage, is judgment.
- **Counsel the client.** "Should you do this deal?" is judgment.
- **Predict legal evolution.** "How will the Delaware courts rule on this in five years?" is judgment.
- **Hold privilege.** A tool can preserve privilege by handling content correctly; the privilege itself is held by the attorney and client.


The work attorneys are paid for is mostly in the second list. The work that bottlenecks attorneys is mostly in the first list. AI shifts the balance.


## Why generic AI is not enough


This is the question we are asked most often: "We could just use ChatGPT, right?"


You can. The result is the failure modes we listed at the top: hallucinated clause references, missed amendment chains, no workflow, no privilege posture. We have written about each one in detail. See[LLM Hallucination in Contract Analysis](https://magelegal.com/blog/llm-hallucination-in-contract-analysis) ,[Amendment Chain Resolution](https://magelegal.com/blog/amendment-chain-resolution-hardest-problem-legal-ai) , and[The F1 Engine Problem](https://magelegal.com/blog/f1-engine-problem) .


The shorthand: generic AI is a great writing assistant. It is a poor diligence engine. The work an M&A team needs done is in the second category.


## How to evaluate before buying


The honest evaluation method is to run a real deal through the candidate tools and compare against ground truth. Vendor demos are designed to win. Real deals are designed to ship. We laid out the framework in[Evaluating Legal AI Tools: A Buyer's Guide for M&A Counsel](https://magelegal.com/blog/legal-ai-tools-for-manda-evaluation-framework) .


The minimum bar:


1. SOC 2 Type II report
2. Written no-training-on-customer-data clause in the DPA
3. Recall against ground truth ≥ associate baseline
4. Precision ≥ 70% (so the partner doesn't have to re-read everything)
5. Output rewrite percentage <30% (so the team adopts unprompted)


A tool that meets these on a real deal is a defensible buy. A tool that fails on more than one is not.


## Where to start


The pillar guides walk through the category at increasing depth:


- [Legal AI for M&A: The Practitioner's Guide](https://magelegal.com/blog/topics/due-diligence) — master hub, what the category is and what each tool category does.
- [AI Due Diligence: An Operational Playbook](https://magelegal.com/blog/topics/due-diligence) — how the workflow runs on a real deal.
- [Legal AI vs. Harvey vs. Generic AI](https://magelegal.com/blog/harvey-vs-kira-vs-infrastructure-legal-ai) — competitive landscape, honestly framed.
- [Evaluating Legal AI Tools](https://magelegal.com/blog/legal-ai-tools-for-manda-evaluation-framework) — the buyer's framework.


If you want to see Mage on a real deal,[request a demo](https://magelegal.com/?demo=1) .
