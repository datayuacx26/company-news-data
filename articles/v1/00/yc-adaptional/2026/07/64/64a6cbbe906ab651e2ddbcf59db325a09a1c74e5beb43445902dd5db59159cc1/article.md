---
schema_version: "1.0.0"
document_id: "64a6cbbe906ab651e2ddbcf59db325a09a1c74e5beb43445902dd5db59159cc1"
company_key: "yc-adaptional"
company: "Adaptional"
source_id: "yc-adaptional-news-import-c2112bd5ddd4"
canonical_url: "https://www.adaptional.com/blog/when-rules-dont-work-why-insurance-automation-needs-reasoning-not-templates"
published_at: null
first_seen_at: "2026-07-21T04:38:31.952963+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:98d3d3a0960d626a9f7c6a1bde1403ec43702795b6080b5ee3269ef1e5532b4c"
---

# When Rules Don't Work: Why Insurance Automation Needs Reasoning, Not Templates

### The Problem with “Automating Underwriting”


For years, the insurance industry has tried to automate underwriting. Every cycle brought new buzzwords: OCR, RPA, NLP. The idea sounded simple. Extract data from documents, feed it into a system, and remove manual work.


In practice, though, it never worked.


Traditional automation quickly breaks when exposed to real submissions: scanned ACORDs, broker emails, spreadsheets, or multi-page PDFs with inconsistent layouts. Each broker and carrier has their own format, and those formats change constantly.


### How Legacy Automation Actually Works


Most automation systems still rely on pattern matching rather than reasoning. They’re built on the assumption that key information will always appear in predictable places or under familiar labels.


For example, when the system sees “Effective Date: 02/12/25,” it extracts the next date. “Limit:” triggers a currency value. “Carrier” is matched against a known list of company names.


‍


*Figure 1. Keyword-Triggered Extraction in Rule-Based Systems*


In theory, this works. If every submission followed one format, rules would be enough. But in practice, every broker, carrier, and line of business adds subtle variations, and even small deviations break the logic. A renamed field, a new section header, or an extra line can cause the system to misread or skip data entirely.


These brittle rule chains, checking one field after another, let small assumptions ripple into silent errors. Systems that seem smart on paper often collapse in production.


‍


### Why Underwriting in Particular is Harder Than it Looks


The problem isn’t just that insurance documents vary in format. It’s that underwriting requires both extraction *and* interpretation. Rule-based systems only handle the first.


In most domains, automation means reading values from structured fields and placing them where they belong. But underwriting depends on what those values mean in context. Two carriers can use the same terms to mean entirely different things. “Limit” might be per-occurrence in one policy and aggregate in another. “Retroactive Date: Continuous” means nothing without the prior carrier and loss history.


Even identical numbers can imply different risks depending on where they appear. A $10M total insured value can represent very different exposures depending on the context:


‍


*Figure 2. Identical values can carry different meanings across underwriting contexts.*


This is why underwriting automation breaks where other document-processing problems succeed. Rule-based systems can mimic the structure of underwriting, but not its judgment.


‍


### From Rules to Reasoning


Large language models redefine automation. Instead of hard-coding thousands of brittle rules for edge cases, Adaptional treats each extraction as a reasoning task. If a human can reason it out, we can teach the model to do the same.


This shifts work from template engineering to prompt engineering, where prompts codify the step-by-step heuristics an experienced underwriter uses when reviewing a submission.


For example, here is the context and prompt we give an LLM to extract effective dates from any document.


‍


*Figure 3. Sample prompt for information retrieval*


Each line represents reasoning the model can apply to any document, regardless of format or terminology. The same logic works for scanned PDFs, handwritten forms, emails, and structured policy schedules because it relies on *intent* , not position.


By encoding human judgment into context, LLMs achieve what older systems never could: a reasoning layer that adapts to new document types without constant rewrites. Prompt-based extraction scales human intuition and makes interpretation repeatable.


‍


### Closing Thoughts


Automation in insurance hasn’t failed for lack of effort or funding. It failed because earlier technology couldn’t reason through ambiguity. OCR could read text, but not the meaning. RPA could mimic workflows but not understand them. Rule-based NLP could extract words but not context.


Now, AI reasoning systems change that.


However, underwriting will always depend on judgment. Our goal is not to replace that judgment, but to scale it and give every underwriter the ability to process complex submissions with the speed, precision, and memory of a collective intelligence built from the best of human expertise.


‍


### Acknowledgements


Article written by Jeffrey Xie
