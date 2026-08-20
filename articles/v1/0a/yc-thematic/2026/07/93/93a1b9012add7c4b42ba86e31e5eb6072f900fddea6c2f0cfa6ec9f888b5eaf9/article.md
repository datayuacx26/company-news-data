---
schema_version: "1.0.0"
document_id: "93a1b9012add7c4b42ba86e31e5eb6072f900fddea6c2f0cfa6ec9f888b5eaf9"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/chatgpt-copilot-feedback-analysis"
published_at: null
first_seen_at: "2026-07-22T16:27:08.884527+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:028c7bcd0e38c13e96fb0635a69c1ac1a1e433cbfc1efd416011087e32049dd6"
---

# What's wrong with using ChatGPT or Copilot to analyze customer feedback?

Using ChatGPT or Microsoft Copilot to analyze customer feedback is fine for a one-time summary. It breaks down entirely for systematic[voice-of-customer](https://getthematic.com/insights/voice-of-customer-strategy) analysis.


When CX analysts paste[NPS verbatims](https://getthematic.com/insights/five-practical-use-cases-of-customer-sentiment-analysis-for-nps) or support tickets into ChatGPT, they get a plausible-looking list of themes. What they don't get is consistency, comparability, an audit trail, or meaningful data protection. Each new session starts from scratch. The themes that surfaced in January disappear when February's data goes in. Three analysts running the same dataset produce three different theme structures. When a CFO challenges the finding in a board presentation, there is no traceable path from the claim back to the raw customer comments.


These are not edge cases. They are structural properties of how large language models work. Thematic was built to close this gap: to give enterprise CX teams the repeatable, governed intelligence they need to make decisions that stick.


## Why analysts reach for ChatGPT in the first place


The workaround is understandable. Most enterprise VoC platforms include text analytics, but the built-in versions frequently disappoint. Many CX analysts describe their platform's built-in[text analytics](https://getthematic.com/insights/text-analytics-applications) as inadequate and resort to downloading verbatims to[analyze in ChatGPT](https://getthematic.com/insights/how-to-analyze-your-customer-feedback-using-chatgpt) instead. Taxonomy-based approaches bundled into major CX platforms require professional services every time customer language shifts, which it does constantly.


The result: analysts do the sensible thing. They copy-paste verbatims into the tool that actually seems to understand language. ChatGPT reads plain English. It produces readable summaries. It appears to solve the problem the platform failed to solve.


The issue is what "appears" is doing in that sentence.


## Four things that break at scale


ChatGPT and Copilot break down for systematic VoC analysis in four specific ways:


- **Inconsistency:** Results change across sessions and analysts, even with the same data
- **No longitudinal tracking:** Themes reset each session; month-over-month comparison is not possible
- **No audit trail:** Findings cannot be traced back to the source comments
- **[Data privacy exposure](https://getthematic.com/insights/lethal-trifecta-llm-security) :** Customer PII routinely leaves the organization without a data-processing agreement


Here is what each of those looks like in practice.


**Inconsistency across sessions and analysts**


A 2025 academic study ([arXiv 2602.14349](https://arxiv.org/abs/2602.14349) ) found that "even for consistent configurations, there is considerable variation in analytical results, suggesting that data analysis by LLMs can vary even when given the same task, data, and settings." For a single analyst doing a one-time synthesis, this variability is usually tolerable. For a team of five analysts coding the same 3,000-comment dataset across three markets, it produces five different theme structures that can't be merged into a single finding.


**No longitudinal tracking**


Each ChatGPT or Copilot session is stateless. January's themes are not stored anywhere. When February's data arrives, the model generates a fresh set of themes with fresh labels and fresh groupings. "Shipping delays" might appear as a theme in Q1 and as "delivery problems" in Q2. These are not comparable. Month-over-month tracking, the foundation of any serious VoC program, is not possible in a stateless environment.


**No audit trail**


When a CFO challenges a CX claim, the expected response is: here is the verbatim evidence, here is the methodology, here are the 847 comments that produced this theme. A ChatGPT output cannot be interrogated this way. There is no traceable path between the finding and the underlying comments.[NIST's AI Risk Management Framework](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) (AI 600-1) classifies this as a material risk category: AI outputs without explainability mechanisms pose reliability and accountability problems at the organizational level.


**Data privacy exposure**


This is where the risk becomes concrete. The default consumer tier of ChatGPT sends conversation content, including any pasted text, to OpenAI for model training. A[Cyberhaven analysis](https://www.cyberhaven.com/engineering/cyberhavens-research-on-how-employees-are-using-chatgpt/) of workplace ChatGPT usage found that 73.8% of employees accessing ChatGPT at work are doing so on personal, non-enterprise accounts. That means customer PII, NPS comments, support ticket text, and call transcript excerpts are routinely leaving organizations with no data-processing agreement in place.


In 2023, Samsung employees pasted proprietary source code and internal meeting notes into ChatGPT on personal accounts, triggering[three separate data incidents](https://incidentdatabase.ai/cite/768/) in 20 days. The company subsequently restricted ChatGPT access across the organization. A CX team doing the same thing with customer verbatims creates the same exposure, often at larger scale. And the data at risk belongs to customers, not the company.


Microsoft Copilot's enterprise version provides stronger data controls than consumer ChatGPT. But those controls only apply when an organization has procured and configured Copilot for Microsoft 365 under an enterprise data protection agreement. Without that, the default Microsoft Consumer Privacy Statement applies. Many enterprise procurement teams have not verified which version their CX analysts are actually using.


## The governance problem compounds over time


Regulatory pressure on this behavior is growing. The Italian Data Protection Authority took[enforcement action against OpenAI](https://digitalpolicyalert.org/event/25456) in 2023, grounded in the processing of EU residents' personal data without an adequate legal basis. The associated fine was annulled by the Court of Rome in March 2026, but the underlying legal framework remains in force. Analysts pasting customer verbatims into consumer AI tools are creating exactly the fact pattern regulators investigate.


The pattern is consistent across industries: governance failures around AI data handling expose organizations to regulatory scrutiny and internal credibility problems long before any formal action. VoC programs built on ad-hoc AI have no consistent methodology, no governance layer, and no defensible outputs when those questions arrive.


## What a structured approach actually delivers


Thematic replaces the ad-hoc loop with a continuous intelligence layer that processes the same feedback data but produces outputs that are consistent, traceable, and comparable across time.


The difference in practice: every theme is stored with the verbatim evidence behind it. Themes are maintained across months, so a Q1-to-Q2 drift in "checkout experience" complaints appears as a tracked movement, not a naming coincidence. The taxonomy is editable and explainable, so a skeptical CFO can interrogate the methodology and[trace any finding back](https://getthematic.com/insights/auditable-transparent-ai-feedback-analytics) to the underlying customer comments.


The data never leaves a governed environment. Thematic processes feedback inside enterprise data agreements, with PII governance applied at ingestion.


## What enterprise CX teams have achieved


Atom Bank, a UK digital bank, operates across seven feedback channels. After moving from fragmented text analytics to Thematic, they reduced calls on their three highest-contact-volume reasons by 69% (mortgage queries), 43% (savings), and 40% (device issues). Their customer base grew 110% in the same period. Michael Sherwood, Head of Customer Insight at Atom Bank, described the result: "Thematic lets us quickly turn unstructured feedback from across channels into clear insights that directly inform our product roadmap and corporate strategy."


Vodafone New Zealand applied Thematic to Touchpoint NPS verbatims and recovered 60 analyst hours per month. After nine months, they tracked a double-digit tNPS increase attributable to specific operational changes the analysis surfaced.


DoorDash used Thematic to analyze Consumer, Merchant, and Dasher NPS simultaneously. The analysis drove a 12-point eNPS improvement and identified a menu load time issue. Load times dropped from 11 seconds to 3 seconds.


A[Forrester Total Economic Impact study](https://getthematic.com/forrester-total-economic-impact-study) on Thematic customers found an average return of 543% over three years, 4,250 hours of analyst time recovered annually, and payback in under six months.


## Five questions to ask before using ad-hoc AI for VoC


1. **Is this a one-time synthesis or a repeatable program?** Ad-hoc AI is appropriate for a one-time summary. It is not appropriate if you need to compare results next month.
2. **Are multiple analysts involved?** If more than one person codes the data, ad-hoc AI produces divergent theme sets that can't be merged.
3. **Do you need to defend the findings to a skeptical stakeholder?** If yes, you need a traceable methodology, not a generated summary.
4. **Is customer PII in the dataset?** If yes, verify the data-processing agreement for every tool in the chain before pasting anything.
5. **Do you need month-over-month trend data?** If yes, the tool must persist themes across sessions. Consumer ChatGPT and consumer Copilot do not.


## The short answer


ChatGPT and Microsoft Copilot produce plausible-looking feedback summaries. They do not produce the consistent, traceable, month-to-month analysis that enterprise VoC programs require. The gap is not a flaw in the AI itself. It is a structural mismatch between a stateless general-purpose tool and a systematic program. The teams achieving measurable NPS gains and governance-ready outputs are not relying on ad-hoc AI. They are using a purpose-built intelligence layer like Thematic, designed for exactly this work.
