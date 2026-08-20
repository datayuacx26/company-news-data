---
schema_version: "1.0.0"
document_id: "a22e40119754d79cf41e8b8c0eb01c6dd277533e17a520dff10b78c012a9a2b0"
company_key: "yc-anglera"
company: "Anglera"
source_id: "yc-anglera-rss-43f494d1c3a6"
canonical_url: "https://www.anglera.com/blog/measured-ai-divide-distributors-2026"
published_at: "2026-06-28T00:00:00+00:00"
first_seen_at: "2026-08-06T19:14:01.668988+00:00"
fetched_at: "2026-08-06T19:14:09.920895+00:00"
content_hash: "sha256:3cd1cc8673734f9695e509922194eb625c7342006b1970f916514fac5c47a769"
---

# The AI Divide Is Real — But the League Tables Are Measuring Talk. Here's What Live-Site Scoring Shows

Distribution Strategy Group's AI Top 25 has become the industry's shorthand for who's winning on AI, and its newest wave — the["six habits" of the leaders](https://distributionstrategy.com/2026/08/ai-top-25-benchmark-reveals-six-habits-separating-distributions-ai-leaders/) — is genuinely useful management reading. But look at what the ranking is built from: earnings calls, executive interviews, customer case studies, vendor documentation, and[a survey of 233 distribution executives](https://distributionstrategy.com/2026/07/the-ai-divide-dsgs-ai-top-25-identifies-the-distributors-rewriting-the-rules-of-competition/) . That's a measure of what companies say about their AI program. We scored the same industry from the outside, off the live product pages a customer or an AI shopping agent actually hits, and the map doesn't match. Some of DSG's own Platinum-tier names score near the bottom of what a machine can actually read on their sites. Several distributors nobody would recognize from a conference stage score near the top.


## What DSG measured, and what it can't


To be fair to the research: DSG's screening bar is real. It excluded pilots and press releases and kept only "sustained production deployments," and it's right that only 26 of 300-plus qualified. That's a useful corrective to the 93% of distributors who[tell DSG's surveyors AI is a strategic priority](https://distributionstrategy.com/2026/07/what-separates-ai-leaders-from-the-rest-of-distribution/) while doing very little about it.


But every input to that screen is something the company chose to disclose — an earnings call script, an interview answer, a case study its own marketing team approved. That's not a knock on the rigor; it's a structural limit of any benchmark built from self-report. A company can be genuinely disciplined about deploying AI in its warehouse, its pricing engine, its call center, and still leave its public product catalog unreadable to the exact AI shopping agents everyone's building for. Narrative and infrastructure are different things, and a Top 25 built from the former will systematically miss gaps in the latter.


## What we measured instead


Anglera's[Digital Readiness Index](https://anglera.com/blog/top-distributors-2026/methodology) doesn't ask anyone anything. It samples five product pages from five categories of a distributor's own live catalog, scores each one deterministically against 14 signals across four pillars — Product Data Depth, Buyer Answerability, Commerce Transparency, and Machine & Agent Readiness — and separately probes the site itself for a sitemap, schema.org markup, and what its` robots.txt` says about AI crawlers by name. Critically, we score the HTML a crawler actually receives, not the DOM after client-side scripts finish running, because that's the version most agents and search engines see. No model judgment, no interview, no press release. It's part of our[Top Distributors 2026 index](https://anglera.com/blog/top-distributors-2026) of 200+ distributors across six operating archetypes, and the measured slice — 37 companies fully probed so far, median score 56 out of 100 — includes four names DSG places in its Platinum tier.


## The divide, redrawn


Wesco International is one of DSG's six Platinum-tier AI leaders. Its measured Digital Readiness Index score is 40 — well below our median of 56. Our probe couldn't reliably fetch Wesco's own product pages through a normal crawler at all; we had to fall back to a full browser to get anything back. No sitemap, no schema.org product markup, no stated policy on AI crawlers either way.


W.W. Grainger, another Platinum name, scores better overall (66) — but its` robots.txt` explicitly names and blocks[GPTBot](https://www.anglera.com/glossary/gptbot) ,[ClaudeBot](https://www.anglera.com/glossary/claudebot) ,[Google-Extended](https://www.anglera.com/glossary/google-extended) , and Meta's crawler. A company DSG holds up as an AI leader has, in writing, told the leading AI agents not to read its catalog. Fastenal, praised for routing 62% of sales through connected vending devices, scores 55 with no sitemap and no structured product data on the pages we sampled — real operational automation on the inside, invisible catalog data on the outside. These aren't gotchas; they're the predictable result of judging AI leadership by what a company says on an earnings call rather than what a machine can parse on its website.


Meanwhile the highest scores in our measured set belong to companies that don't headline anyone's AI keynote: TricorBraun, a packaging distributor, scores 70 — full marks on commerce transparency, a working sitemap, live[JSON-LD](https://www.anglera.com/glossary/json-ld) , and a genuinely comparison-shoppable catalog. Future Electronics and E&T Plastics both score 68. None of these three appears anywhere in DSG's coverage of the AI Divide. They're not "AI-first" by any narrative measure. They're just legible.


## The gap isn't limited to distribution


This isn't a distribution-specific quirk of measurement, either. Wholesale trade's own reported AI adoption trails the broader economy —[Census Bureau survey data](https://www.mdm.com/news/technology/ai/census-ai-adoption-accelerating-across-u-s-businesses-but-use-cases-remain-narrow/) put the sector in the high teens to mid-twenties percent depending on how "using AI" is defined, against a national figure near 20%. And at the macro level, MIT's widely cited 2025 study found that roughly[95% of enterprise generative-AI pilots were showing zero measurable return](https://www.forbes.com/sites/andreahill/2025/08/21/why-95-of-ai-pilots-fail-and-what-business-leaders-should-do-instead/) — not because the technology failed, but because the surrounding workflow and data infrastructure wasn't built to receive it. Distribution isn't unusually behind. It's unusually well-covered by a press that keeps asking companies to grade their own homework.


## What operators should actually check


If you run a distribution business and you're wondering whether you're behind, don't start by benchmarking your AI narrative against DSG's Top 25. Start by benchmarking your product pages against what an outside machine can verify today. Pull up your` robots.txt` and see whether it names GPTBot, ClaudeBot, or Google-Extended, and what it says. Check whether your sitemap actually resolves. View source on a product page and look for` Product` schema in the raw HTML — not what renders after JavaScript runs, what a crawler gets on the first request. Ask whether a page has a machine-parseable price and stock status, or a "call for pricing" wall.


None of that requires a keynote slot or an earnings-call talking point. It requires product data that's structured, complete, and exposed — which is precisely the layer most distributors have underinvested in relative to everything else in their stack. That's the gap Anglera exists to close: your PIM stores the data, and we do the work of getting it deep enough and structured enough that the pages behind it are as legible to a machine as your executives are to a reporter. The AI divide is real. It's just being measured by the wrong instrument.
