---
schema_version: "1.0.0"
document_id: "7d07467c9da473a05e3018723893a07aa63527866106be08ba10739a74919ab3"
company_key: "yc-sola"
company: "Sola"
source_id: "yc-sola-news-import-2dc63e1087a6"
canonical_url: "https://sola.security/insights/open-security-benchmark-towards-autonomous-enterprise-cyber-defense/"
published_at: "2026-08-11T12:34:31+00:00"
first_seen_at: "2026-08-11T21:33:23.657608+00:00"
fetched_at: "2026-08-11T21:33:25.499375+00:00"
content_hash: "sha256:d28597dbf6d7b46bf553df756951da438ad17599d984529f18173a8c4553f706"
---

# Open Security Benchmark: Towards autonomous enterprise cyber defense

[Read the full research on arXiv (PDF download)](https://arxiv.org/pdf/2607.27288)


## Abstract


Enterprises are moving toward *autonomous cyber defense* : agentic AI that builds situational awareness of an organization’s security state and reasons from it to assessments, decisions, and actions. This rests on a *holistic view of the enterprise’s security state* , the continuous, cross-vendor picture of identities, cloud and infrastructure, data, applications, and their configurations that security posture management assembles. As agents take on this work, what matters is not whether an agent *can* produce an answer but whether it *should be trusted* to. The field cannot yet answer this question. Real enterprise environments are private, cross-vendor, and deeply correlated, and none is exposed publicly as a shared, queryable target for evaluating such agents end to end. We call this the *environment data gap* . We present **Open Security Benchmark (OSB)** , a framework that benchmarks agentic AI on this work. OSB surfaces a curated enterprise environment-a frozen, holistic view of the security state-and evaluates posture investigation across two modalities: *text-to-SQL* over a relational snapshot and each vendor’s *native API* over a served instance of the same environment. Freezing the environment pins the target state as an immutable snapshot and anchors answers to a closed-form ground truth. OSB is built from five components: a data layer, a task and evaluation-set layer, a multi-dimensional scoring layer, a minimal auditable harness, and a bring-your-own path that serves public comparison and private tenant evaluation from one substrate. We instantiate the framework with two identity-security packs and a family of synthetic-organization environment datasets spanning multiple scales, and chart its extension to further posture subdomains, investigation modalities, and defense stages from assessment toward remediation.


## Introduction


Autonomous cyber defense, systems that sense, reason about, and act on cyber threats with limited human intervention, is now an explicit goal in enterprise security, pursued across reviews of automated and autonomous cyber defense \[[55](https://arxiv.org/html/2607.27288v1#bib.bib46) \] and training environments for autonomous agents \[[51](https://arxiv.org/html/2607.27288v1#bib.bib47) \]. Yet every stage of the sense-reason-act loop presupposes one foundation: an accurate, current, holistic understanding of the organization’s own security state. Situational-awareness research makes this precise \[[14](https://arxiv.org/html/2607.27288v1#bib.bib44) \], adapted in security as *cyber situational awareness* \[[17](https://arxiv.org/html/2607.27288v1#bib.bib45) \]: an agent cannot act on what it cannot see.


Establishing this self-knowledge is therefore a prerequisite for autonomous cyber defense. Security posture management, the continuous, holistic assessment of an organization’s defensive readiness and security state, supplies this situational-awareness foundation, on which any autonomous assessment or decision rests.


This assessment spans multiple subdomains: identity and access \[[47](https://arxiv.org/html/2607.27288v1#bib.bib6) \], cloud and infrastructure configuration \[[7](https://arxiv.org/html/2607.27288v1#bib.bib49) \], data exposure and classification \[[31](https://arxiv.org/html/2607.27288v1#bib.bib50) \], application security and vulnerability state \[[41](https://arxiv.org/html/2607.27288v1#bib.bib51) \], third-party and SaaS integrations \[[34](https://arxiv.org/html/2607.27288v1#bib.bib52) \], and compliance alignment with regulatory and contractual obligations \[[36](https://arxiv.org/html/2607.27288v1#bib.bib53) \]. Catalogs like NIST SP 800-53 \[[23](https://arxiv.org/html/2607.27288v1#bib.bib3) \], the CIS Critical Security Controls \[[5](https://arxiv.org/html/2607.27288v1#bib.bib4) \], and ISO/IEC 27001 \[[20](https://arxiv.org/html/2607.27288v1#bib.bib5) \] codify this work, and posture tooling from cloud auditors \[[37](https://arxiv.org/html/2607.27288v1#bib.bib7) ,[44](https://arxiv.org/html/2607.27288v1#bib.bib9) \] to SaaS scanners \[[10](https://arxiv.org/html/2607.27288v1#bib.bib8) \] operationalizes it across vendor stacks.


Enterprises increasingly apply agentic AI to this work, with reported gains in coverage and analyst throughput \[[18](https://arxiv.org/html/2607.27288v1#bib.bib14) \]. As deployment scales, the binding question changes: an agent can produce an answer, but should we trust it, in this environment, with this data, given the evidence it produces and the ways it is known to fail? Answering this requires systematic, reproducible evaluation of agents on posture work, faithful to how it is done.


Recent cybersecurity benchmarks advance LLM evaluation across adjacent capabilities \[[59](https://arxiv.org/html/2607.27288v1#bib.bib17) ,[48](https://arxiv.org/html/2607.27288v1#bib.bib22) ,[4](https://arxiv.org/html/2607.27288v1#bib.bib25) \], but each addresses a narrow slice in isolation (§[2](https://arxiv.org/html/2607.27288v1#S2) ). More fundamentally, the field faces an *environment data gap* : enterprise environments are proprietary, span many vendors, and interlink their data, and while executable environments exist for *offensive* security tasks \[[66](https://arxiv.org/html/2607.27288v1#bib.bib34) ,[49](https://arxiv.org/html/2607.27288v1#bib.bib35) \], none surfaces a cross-vendor enterprise environment as a shared, queryable substrate for evaluating *defensive* posture-investigation agents end to end with gold posture answers. Practitioners therefore cannot compare agents on the work they must deploy them for, and researchers lack a stable target.


We present **Open Security Benchmark (OSB)** , a framework for evaluating agentic AI on security posture tasks 1. OSB closes the environment data gap by surfacing curated enterprise environments as a single, end-to-end view of the security state. Its methodological commitment is a frozen environment that agents investigate through the surface they would use in practice: *text-to-SQL* over the relational snapshot, or each vendor’s *native API* over a served instance of the same environment. OSB aims at the full autonomous-cyber-defense loop, up through automated remediation, but starts at the assessment stage. Assessment is the read-only work of surfacing and substantiating findings, on which every later stage builds (§[7](https://arxiv.org/html/2607.27288v1#S7) ).


We formulate posture investigation as an agentic, execution-grounded text-to-SQL task. Each instance is a pair (D,q): D is a read-only relational snapshot of the organization’s holistic security state (identities, cloud and infrastructure, identity providers, SaaS), and q is a natural-language security question. The agent must discover the tables in D relevant to q, compose read-only SQL queries that retrieve the supporting evidence, and synthesize a natural-language answer a. The answer is graded against a closed-form reference (a verdict, a closed set of matching subjects, and the determining configuration facts), so scoring is plan-independent: two semantically equivalent queries score alike, in the denotation-based tradition of semantic parsing that judges a parse by its answer, not its logical form \[[3](https://arxiv.org/html/2607.27288v1#bib.bib48) \]. This extends the schema-realistic, agentic text-to-SQL paradigm of the Spider and BIRD lineage \[[64](https://arxiv.org/html/2607.27288v1#bib.bib15) ,[27](https://arxiv.org/html/2607.27288v1#bib.bib33) ,[25](https://arxiv.org/html/2607.27288v1#bib.bib16) \] to defensive security work, casting each investigation as a reason-and-act loop \[[62](https://arxiv.org/html/2607.27288v1#bib.bib26) \] over a frozen environment behind a constrained tool interface \[[61](https://arxiv.org/html/2607.27288v1#bib.bib29) \]. A second modality keeps the same D, question, and answer criteria but changes the surface: the agent interrogates a served instance of the same environment over its vendors’ native APIs with real vendor tooling, and its recorded request trace stands in for the SQL the relational modality would produce (§[3](https://arxiv.org/html/2607.27288v1#S3) ).


We ground this general substrate in a concrete first domain. Identity security posture management (ISPM) is a natural start: cross-vendor by nature, data-rich, consequential, and newly equipped with public benchmark definitions we build on \[[15](https://arxiv.org/html/2607.27288v1#bib.bib1) ,[63](https://arxiv.org/html/2607.27288v1#bib.bib2) \]. The framework extends across posture subdomains under the same methodology. It also extends along the progression that autonomous cyber defense automates: assessment, risk assessment and prioritization \[[33](https://arxiv.org/html/2607.27288v1#bib.bib54) \], mitigation and remediation recommendation \[[35](https://arxiv.org/html/2607.27288v1#bib.bib55) \], remediation and response implementation \[[55](https://arxiv.org/html/2607.27288v1#bib.bib46) ,[51](https://arxiv.org/html/2607.27288v1#bib.bib47) \], and post-remediation verification via continuous monitoring \[[32](https://arxiv.org/html/2607.27288v1#bib.bib56) \]. It instantiates only the assessment stage (§[7](https://arxiv.org/html/2607.27288v1#S7) ).


This paper makes three contributions: (i) **Open Security Benchmark** , an evaluation framework whose five components (a data layer, a task and evaluation-set layer, a multi-dimensional scoring layer, a minimal auditable harness, and a bring-your-own path) compose a standardized substrate for both relational and native-API investigation, with an explicit trust boundary; (ii) a curated *data catalog* of multi-vendor enterprise environments, packaged as synthetic-organization datasets that close the environment data gap; and (iii) initial *use-case packs* (ISPM Visibility and ISPM Cross-Vendor) that instantiate the framework end to end and open an extension path for community-authored packs across posture subdomains.


The rest of the paper is structured as follows. Section[2](https://arxiv.org/html/2607.27288v1#S2) surveys related work and positions OSB against it. Section[3](https://arxiv.org/html/2607.27288v1#S3) presents the two investigation modalities and why text-to-SQL is standardized first. Section[4](https://arxiv.org/html/2607.27288v1#S4) describes the framework’s five components. Section[5](https://arxiv.org/html/2607.27288v1#S5) presents the environment datasets and Section[6](https://arxiv.org/html/2607.27288v1#S6) the benchmarking packs. Section[7](https://arxiv.org/html/2607.27288v1#S7) discusses broader implications and limitations, and Section[8](https://arxiv.org/html/2607.27288v1#S8) concludes.


## Get the full research on arXiv
