---
schema_version: "1.0.0"
document_id: "9ae02daeb5eb60b74a563a66b7171ddf397904a31b248c4651724546299b03f3"
company_key: "yc-sola"
company: "Sola"
source_id: "yc-sola-news-import-2dc63e1087a6"
canonical_url: "https://sola.security/insights/ispm-visibility-ai-security-benchmarks/"
published_at: "2026-01-29T10:24:16+00:00"
first_seen_at: "2026-07-24T01:34:47.750416+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:db0488c4c57b177281e71ad74caa74ccdf973972dd527edd1e224589697cb280"
---

# A practical benchmark for AI-driven identity security

“AI” has become the default adjective in security marketing. Nearly every platform today claims to be AI powered, AI driven, or AI native, promising systems that supposedly understand your environment and automatically surface risk. What most of these claims have in common is that they stop at the promise. Buyers are shown demos and diagrams, but they are rarely shown evidence.


For security teams making real decisions, this creates a familiar frustration. You are asked to trust that a system works without ever being shown how well it performs when confronted with real questions about a real environment. The industry has moved quickly from manual tools to AI messaging without building the evaluation layer in between.


Our AI research team decided to address that gap directly.


We built and released the first standardized benchmark for AI driven identity security visibility. We then tested our own agent against it, measured the results carefully, and made the entire methodology public so others can validate, critique, or replicate the work.


This article explains why we did that, what we tested, what we learned, and why research like this matters far beyond any single vendor.


[The full research on arXiv (PDF download)](https://arxiv.org/pdf/2601.07880)


## The gap between AI claims and evidence


In most areas of software, performance claims are eventually grounded in shared benchmarks. Databases are tested for throughput and latency, search systems are evaluated for relevance, language models are compared on standardized tasks. Security AI, especially in identity, has largely avoided this level of scrutiny.


That absence matters because identity security is both critically important and inherently complex. Identity data spans cloud providers, identity platforms, and SaaS applications. Answering even simple sounding questions often requires stitching together information from multiple systems and reasoning across relationships that are not explicitly defined.


Despite this complexity, vendors routinely claim that AI can handle identity risk automatically. When pressed for proof, buyers are usually offered a guided demo or a controlled proof of concept rather than data that reflects how the system performs across a broad set of real world questions.


Security teams deserve better than that. They need ways to compare systems based on evidence, not confidence.


## Why identity security was the right place to start


Identity has quietly become the practical perimeter of modern organizations. Excessive privileges, misconfigured trust relationships, and weak authentication controls sit at the center of many high impact breaches. At the same time, identity environments are dynamic and messy, which makes them a hard test for any AI system.


This combination makes identity security an ideal domain for benchmarking. If an AI agent cannot reliably answer foundational visibility questions about identities and access, then higher level promises about automation and remediation are built on sand.


Our research focused on this foundational layer, what is often referred to as identity security posture management (ISPM) visibility. The goal was not to test theoretical reasoning or abstract policy interpretation, but to evaluate whether an AI system can accurately answer the kinds of questions security teams ask every day.


## What we tested (and how)


We built a production-like environment spanning three widely used platforms: AWS, Okta, and Google Workspace. These systems represent a large portion of how modern organizations manage infrastructure access, workforce identity, and SaaS permissions.


From there, we developed a set of 77 questions derived from established security best practices and tooling guidance, including Scout Suite, ScubaGoggles, and Okta’s own security recommendations. Each question was designed to reflect a real operational task, such as identifying privileged identities without multi-factor authentication (MFA) or enumerating roles that trust external accounts.


Every question had clear evidence in the data. There were no synthetic shortcuts and no ambiguous prompts. The benchmark was designed to reflect the day to day work of security teams, not an idealized laboratory scenario.


The full methodology, question set, and evaluation criteria are[publicly available](https://arxiv.org/abs/2601.07880v1) so that others can run the same benchmark in their own environments.


## How accuracy was evaluated


Accuracy in security is rarely all or nothing; an answer can be fully correct, partially correct but still useful, or misleading in a way that creates risk. Because of this, we used a graded evaluation approach rather than a binary pass/fail model.


Each response was assessed on several dimensions, including:


- whether it addressed the question asked;
- whether it faithfully reflected the underlying data;
- whether the generated queries were appropriate; and
- whether the reasoning was coherent without hallucinating unsupported claims.


Evaluation was conducted in two complementary ways: First, a panel of five security experts reviewed the answers. Second, large-language models were used as “judges” to cross validate consistency. This dual approach helped reduce individual bias and surfaced disagreements worth examining more closely.


## What the results showed (and why context matters)


Across all seventy seven benchmark questions, Sola’s agent achieved an overall accuracy score of 80%.


**That number becomes more meaningful when placed in context.** In comparable enterprise AI tasks involving structured data querying, state-of-the-art systems tend to score in a similar range. For example, leading models on Spider 2.0, a well known enterprise text to SQL benchmark, typically achieve accuracy around 80-81%. This is not a direct comparison, since the data and tasks differ, but it provides a useful reference point for what strong performance looks like in practice.


Importantly, an 80% percent score does not imply that the remaining answers were useless; many were partially correct and operationally helpful. In a conversational system, users refine questions, follow up, and narrow scope, which means partial answers often move investigations forward rather than stopping them entirely.


Performance varied by platform: AWS hygiene questions showed particularly strong results, with expert accuracy reaching 94%. Google Workspace and identity inventory questions landed around 75%, while Okta questions scored closer to 65%. These differences reflect the underlying complexity and data consistency of each system rather than random variance.


The key takeaway is not perfection, but visibility. We can see where the system performs well, where it struggles, and how those areas change over time.


## Why the AI agent performed the way it did


The results were not accidental, and they were not driven by model size alone. One of the clearest findings from the research was a positive relationship between answer accuracy and what we refer to as *example adaptation* .


Example adaptation describes the agent’s ability to retrieve prior patterns and then adapt them to the current schema, environment, and question rather than copying them verbatim. This behavior is powered by Sola’s internal knowledge base, which encodes security insight patterns that provide structured context to the model.


When this contextual grounding was strong, the agent adapted effectively and produced accurate answers. When it was weak or incomplete, performance dropped. This reinforces a broader lesson that applies well beyond identity security. High quality context often matters more than simply using a larger model.fic providers. Cloud providers such as AWS, Azure, and GCP are named directly in 8.9% of prompts. Meanwhile, source code management tools show up in 6.7%. GitHub and GitLab references appear when teams want to understand repository risk.


## Why we published the benchmark


We did not publish this benchmark to make a one-time claim about our own product. We published it because the industry needs shared ways to evaluate security AI.


By making the benchmark public, we enable security teams to evaluate tools using the same criteria, vendors to validate or challenge results with their own systems, and researchers to build on existing work instead of starting from zero.


This kind of openness creates healthy pressure. It shifts competition toward measurable quality rather than louder messaging. If another vendor can outperform these results, they should publish their data. That is how trust is built.


## Why this matters beyond Sola


This benchmark focuses on one domain, identity visibility, but the underlying principle applies across security. As AI becomes embedded deeper into security operations, the cost of silent failure increases. False confidence can be more dangerous than acknowledged limitations.


Benchmarks force clarity. They expose tradeoffs. They give buyers a way to ask better questions and builders a way to improve based on evidence.


Sola benefits from this work because it validates our approach, but the larger beneficiary is the industry itself. Security teams gain a framework for demanding proof instead of accepting promises, and the market moves incrementally toward evidence based evaluation.


## What this signals about the future of security


Security is shifting away from collections of isolated tools toward systems that reason across context. That future cannot be built on unchecked claims or polished demos alone.


It requires measurement, transparency, and a willingness to show results even when they reveal limitations.


This benchmark is one step in that direction. It shows what intelligence based security looks like when quality is defined in practical terms and tested against reality.


And it sets a baseline for what security teams should expect from AI going forward.
