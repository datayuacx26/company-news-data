---
schema_version: "1.0.0"
document_id: "171990af4c49e77eb97fa1de8593661f3264f471995aca0861b46e93974ee396"
company_key: "yc-sola"
company: "Sola"
source_id: "yc-sola-news-import-2dc63e1087a6"
canonical_url: "https://sola.security/insights/cross-platform-ispm-benchmark-ai-identity/"
published_at: "2026-06-03T18:21:07+00:00"
first_seen_at: "2026-07-24T01:34:47.750416+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:4811f3f8860ea125608d0bb0f90427a6fbdc5a140b7ce81256c1c288d2cc846c"
---

# Cross-platform ISPM benchmark: context improves AI models’ accuracy by 34%

[Read the full research on arXiv (PDF download)](https://arxiv.org/pdf/2606.02674)


- Cross-system identity risk is one of the hardest problems for security teams to measure, because the connections between platforms are invisible unless you build or maintain a map of them.
- We built a benchmark to test how well AI agents navigate those connections across eight enterprise platforms (like AWS, Google Workspace and Okta) running four frontier models through five levels of relational context.
- Across every model tested, reasoning quality was high even without any supporting context, but complete, evidence-backed answers required explicit relational grounding.
- Adding a cross-vendor relationship map raised answer correctness by 34% relatively and cut exploration queries by roughly 70%.
- The finding that holds across all models: the quality of the relational context around an AI agent matters more than which model runs underneath it.


Earlier this year, we published[the Sola ISPM Visibility Benchmark](https://sola.security/insights/ispm-visibility-ai-security-benchmarks/) , evaluating how well AI agents perform on single-platform identity security tasks. The results were meaningful: agents could answer complex identity questions accurately on a platform-by-platform basis.


Single-platform, however, is not how enterprise identity works. The same person appears across an HR system, one or two identity providers, cloud infrastructure, developer tools, and productivity platforms, in different formats with no shared identifier between them. The risks that matter most surface only when you correlate data across all of them at once.


The Cross-Vendor Sola ISPM Benchmark extends that earlier work to these harder, multi-system scenarios. The benchmark tests four frontier AI models (Claude Opus 4.8, Claude Sonnet 4.6, GPT-5.5 and Gemini 3.1 Pro) across 50 cross-vendor identity tasks, each requiring data from more than one platform. The piece covers three questions: where AI already performs well on this problem, where it consistently falls short, and what structural factor determines the difference.


## The correlation gap that makes cross-platform ISPM hard


Our research defines this obstacle as the Correlation Gap: the challenge of linking the same identity across platforms that share no common schema and provide no explicit foreign keys between them.


A concrete example from the paper makes the difficulty clear. Recognizing that[\[email protected\]](https://sola.security/cdn-cgi/l/email-protection) in an identity provider corresponds to arn:aws:iam::123:role/JDoe-Dev in AWS requires inferring a connection that exists in neither system’s data. Neither platform references the other. The agent has to construct that bridge on its own, across systems no one designed to interoperate.


Traditional security tools and AI evaluations test platforms in isolation. Risks that only appear at the intersection of two or more systems go unmeasured by any single-platform approach. GitHub organization members with no corresponding identity in Azure AD, Google Workspace administrators who can bypass MFA through a GCP IAM permission, offboarded employees who retain active AWS SSO access after their HR record marks them as gone. None of these surface from one platform alone.


We built the benchmark as a direct response to that gap.


## How we built the benchmark, and how we tested it


Our benchmark ran on a live, production-grade enterprise environment connecting eight platforms: HiBob (HR system and source of truth for employee status), Okta and Microsoft Azure Active Directory (identity providers), AWS and GCP (cloud infrastructure), Google Workspace (collaboration and productivity), GitHub (developer infrastructure), and MongoDB Atlas (data persistence).


Each of the 50 tasks requires data from more than one of these platforms. Every task reflects a federated-identity investigation a security team would run in practice.


We tested four frontier models: Claude Opus 4.8, Claude Sonnet 4.6, GPT-5.5, and Gemini 3.1 Pro. Each ran across five context configurations, from No Context (raw environment access, no supporting metadata) to Full Context (schemas, example queries, and a cross-vendor relationship map all supplied)\[1\] . Evaluation graded the full reasoning chain, not the final answer alone, across four dimensions: answer correctness, reasoning quality, SQL accuracy, and evidence retrieval quality\[2\] . A dual-judge consensus panel scored each response\[3\] .


## The models knew what to look for. They lacked the evidence to prove it


The most practically significant finding is about the gap between knowing a risk exists and being able to act on it.


Even with zero context injected, every model scored between 0.94 and 0.99 on reasoning utility. They attempted the right investigations, queried the right types of data, and formulated coherent security logic. The failure was not in security reasoning, it was at the evidence layer: without explicit relational context, models queried tables that didn’t exist, made incorrect cross-system entity assumptions, and missed affected identities even when they correctly identified that a risk was present.


Under Full Context (including schemas, Security Graph, and example queries all supplied together), Claude Opus 4.8 reached the correct high-level security verdict 94% of the time. Its complete, evidence-backed answer was correct 78% of the time. The same gap appeared consistently across all four models and all context tiers:


We identify this as the primary failure mode: agents correctly determine that a risk exists while failing to enumerate all affected identities, recover the exact supporting entities, and reproduce the complete evidence chain. For remediation, that distinction is operational. Confirming that a privilege escalation path exists is not enough to close it. A security team needs the full list of affected identities, and the AI failed to produce it reliably without relational grounding.


An AI agent without explicit relational context can function as a directional signal. Without that grounding, it doesn’t reliably produce a complete investigation. The data on what happens when that grounding is supplied tells the rest of the story.


## Accuracy: context decided the outcome more than model choice did


Across all four models, answer accuracy rose consistently as the agent received more structured context. The pattern held regardless of which model powered the agent. Moving from No Context to Full Context improved answer correctness by roughly 34% relatively, with the best-performing configuration reaching a correctness score of 0.78.


The single largest accuracy jump came from adding the **Security Graph** , a Sola-built cross-platform relationship map that explicitly defines how entities in one platform connect to entities in another. As a methodology note: the Security Graph is a Sola-proprietary artifact, and its role in the results is part of what the benchmark measures.


Failure rate reductions tell the same story. Opus dropped from 28% complete failures under No Context to 4% under Full Context. Sonnet dropped from 38% to 8%.


For practitioners and leaders evaluating AI for identity security: which model to choose matters less than what relational context the agent receives.


## Efficiency: richer context also meant fewer queries


The accuracy gains came *alongside* efficiency gains, not at their expense. Moving from No Context to Full Context reduced the average number of exploratory SQL queries per task by roughly 70% across all models.


Without relational context, agents rely on trial-and-error exploration, issuing queries against inferred or hallucinated tables and iterating through dead ends. With the Security Graph supplied, the same models shifted to targeted execution, locating the correct join paths without the iterative probing.


The four models handled that efficiency gain differently, which reflects architectural differences in how each takes advantage of relational guidance rather than differences in overall accuracy:


- Claude Opus 4.8 achieved the highest accuracy while maintaining low query counts under the Full Context configuration.
- GPT-5.5 used the fewest queries across the benchmark, converging fast. That speed came with lower accuracy than the top Claude configurations, pointing to a tradeoff between execution efficiency and depth of relational reasoning.
- Claude Sonnet 4.6 showed the sharpest efficiency gains from contextual enrichment. Under richer settings its query counts dropped substantially, converging toward Opus-level execution efficiency.
- Gemini 3.1 Pro responded strongly to context enrichment but reached a mild saturation point where additional metadata layers yielded diminishing returns on accuracy even as relational execution continued to improve.


Lower query counts matter independently from accuracy. In production deployments, fewer exploratory queries mean lower computational cost and faster responses. The efficiency gain is a separate benefit, not a proxy for the accuracy result.


Together, the accuracy and efficiency data point to the same variable.


## What the findings mean for security teams


An AI agent without relational grounding produces directional verdicts, not complete evidence sets. The verdict-to-correctness gap in this benchmark, 94% versus 78% under the strongest configuration, shows that even the best-performing setup leaves meaningful evidence gaps. For remediation, that gap matters: knowing a risk exists is not the same as knowing which identities it touches. The relational context the agent receives determines whether its output is a usable starting point or a complete investigation.


On model selection: the data shows that choosing the right model is a secondary decision compared to what relational context the agent has access to. Teams assessing AI for identity security should ask how the agent resolves entity references across platforms, what structured context it receives, and how it surfaces failure cases when relational data is missing or incomplete.


Our benchmark covers identity posture visibility, and is restricted in its scope. It measures the ability to detect misconfigurations, enumerate identities, and trace cross-system access paths. It doesn’t cover remediation, behavioral analytics, identity risk scoring, or governance alignment.


Within that scope of ISPM visibility, the results are clear, and they point toward what comes next.


## What the gap tells us about AI and ISPM


Our benchmark establishes where frontier models currently stand on cross-platform identity reasoning. They arrive with strong security intuition already in place. What limits them is not what they know about security, but whether they can see how the systems they’re querying connect to each other.


The field still needs to measure deeper multi-hop reasoning (the current task set goes up to three sequential cross-system hops), remediation workflows, behavioral analytics, and identity risk scoring. The benchmark covers one part of the ISPM problem space. Future work will extend it.


We published the question suite as a public resource for the security research community to build on. The benchmark environment is Sola’s production platform, and the Security Graph tested here is a component of Sola’s context orchestration layer. Our research informs how Sola continues to develop its approach to cross-platform identity reasoning.


## Get the full research on arXiv


### Footnotes:


\[1\] **Context configurations (five tiers)** :
No Context: the agent receives raw environment access only, with no schema or relational metadata supplied.
Schema Only: full database schemas for all eight platforms are provided.
Schema + Graph: schemas plus the Security Graph, a cross-vendor relationship map that explicitly defines how entities in one platform connect to entities in another.
Schema + Examples: schemas plus retrieved example queries relevant to each task, without the graph.
Full Context: schemas, Security Graph, and example queries all supplied together.


\[2\] **Evaluation dimensions (four metrics).**
Answer correctness: semantic equivalence between the agent’s complete answer and the expert-validated ground truth.
Reasoning quality: whether the agent investigated the right systems, relationships, and entities for the task.
SQL accuracy: technical correctness and executability of the queries generated.
Evidence retrieval quality: whether the entities and evidence the agent recovered matched the ground-truth evidence set, including completeness of affected identities.


\[3\] **Dual-judge consensus mechanism.**
Scoring used a panel of two LLM judges (Claude Sonnet 4.6 and GPT-4.1), selected for complementary strengths in structured reasoning and semantic precision respectively. Each judge evaluated every execution trace independently across multiple samples. Final scores were determined by taking the statistical mode of the combined pool of individual judgments (majority vote consensus), producing a bias-corrected score that smooths out individual judge variance.
