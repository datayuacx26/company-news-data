---
schema_version: "1.0.0"
document_id: "7ed851da0b180d69adb496c5493e79af4d4bdc466115b187747a13f8e7e9cd19"
company_key: "yc-invofox"
company: "Invofox"
source_id: "yc-invofox-news-import-19c8879c7fc9"
canonical_url: "https://www.invofox.com/en/blog/the-problems-youll-run-into-using-google-document-ai/"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-22T00:28:19.833120+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:304f462647c1001c5ba9b7261c3c8754dbe844c62df541943202cc1e8f87d3b2"
---

# Google Cloud Document AI: The Problems You'll Run Into at Scale

Table of contents


- Disclaimer
- Document AI is two products in one wrapper
- At a glance: Prebuilt vs Custom Processor
- Issue #1: The unit price isn’t the pipeline price
- Issue #2: Custom Processor is a labeling, training and deployment workflow you operate
- Issue #3: Hosting bills compound with every version you keep
- Issue #4: Field names and schemas are immutable
- Issue #5: Customer-specific corrections don’t compose without tenant isolation
- The 3pm-on-a-Friday test
- Smaller (but painful) issues
- Who Google Document AI is good for
- How Invofox approaches these problems
- Final thoughts


## Disclaimer


I’m Head of Product at Invofox, where we build a managed document extraction platform. I have a horse in this race and you should read what follows with that in mind. I’ve tried to keep the body of this post focused on what Google’s own documentation says, and to keep the comparison with how managed alternatives approach the same problems in one clearly labeled section near the end.


Over the last few years I’ve sat in architecture reviews with dozens of engineering and product teams either evaluating Google Document AI or already running it in production — invoice processing, mortgage, payroll, KYC. The recurring patterns and limitations below are what those conversations surfaced for teams evaluating Google Document AI alternatives.


Every pricing figure and limit cited is taken directly from cloud.google.com/document-ai/pricing and the official Document AI documentation as of May 2026. Links at the end.


## Document AI is two products in one wrapper


When teams say “we’ll just use Google Document AI”, they usually mean one of two different products that share a brand:


- **Path 1 — Prebuilt (out-of-the-box) processors.** A library of pretrained parsers for common document types: invoice, expense, bank statement, payslip, W-2, driver’s license, passport, utility bill, ID proofing. Closed schemas. Per-document pricing.
- **Path 2 — Custom Processor.** A Custom Extractor / Classifier / Splitter that you label, train, version and deploy yourself, on your own documents. Open schema (with caveats). Per-page pricing on top of per-version hosting.


These look like two points on a single continuum. In practice they have different pricing models, different failure modes, and different paths to “fix it” when something breaks in production. Teams typically pick one during the POC and end up running both within a year, with the integration between them owned by the customer.


## At a glance: Prebuilt vs Custom Processor


Dimension Prebuilt processors Custom Processor


Setup effort Low — call the API High — schema, labels, training


Schema flexibility None — fixed fields Full — you define the schema


Retraining Not available Required for any field or behaviour change


Hosting cost None — paid per use $0.05 / hour per deployed version


Version management Not applicable Owner-operated


Multi-tenant support None — one schema for all customers Build-it-yourself per processor


OCR layer Google OCR (fixed) Google OCR (fixed)


Pricing model Per document Per 1,000 pages + hosting


Multi-document PDFs Only the first eligible form is processed Requires Custom Splitter upstream


The rest of the post walks through where these dimensions tend to bite in production, with the supporting pricing and documentation references.


## Issue #1: The unit price isn’t the pipeline price


A note before the numbers below: these are public list prices and they change. Google updates tiers periodically, and customers with significant volume can negotiate volume discounts or private commercial agreements that don’t show up on the public page. The figures in this section use the published list as the only reference. Your real bill may be lower depending on the deal you sign, but the operational shape — splitter required, hosting per version, retraining lifecycle — doesn’t change with the discount.


Document AI’s pricing page reads cleanly line by line. The current list, from cloud.google.com/document-ai/pricing:


**Prebuilt processors (per document):**


- Invoice / Expense / Utility parser: $0.10 per 10 pages in a document
- Bank statement parser: $0.75 per classified document
- Pay slip parser: $0.30 per classified document
- W-2 parser: $0.30 per classified document
- US driver license / US passport / ID proofing: $0.10 per document
- Lending splitter & classifier: $0.05 per classified document


Note the difference in unit. Prebuilt parsers are billed per document: a single-page receipt and a 9-page invoice both cost $0.10 on the Invoice / Expense parser; an 11-page invoice jumps to $0.20 because it crosses into the second 10-page bracket. Bank statements, payslips and W-2s are billed per classified document regardless of length. Custom Processor is billed per page: every 1,000 pages costs $30 on Custom Extractor, no matter how those pages are grouped into documents. Two different cost models that are hard to compare apples-to-apples until you map them against your actual document mix.


**Custom Processor (per 1,000 pages):**


- Custom Extractor: $30 / 1k pages (up to 1M pages/month), $20 / 1k pages above
- Form Parser: $30 / 1k pages (up to 1M), $20 / 1k pages above
- Custom Classifier: $5 / 1k pages (up to 1M), $3 / 1k pages above
- Custom Splitter: $5 / 1k pages (up to 1M), $3 / 1k pages above


Plus Enterprise Document OCR if you need raw text: $1.50 / 1k pages (up to 5M), $0.60 / 1k pages above. Plus hosting for any Custom Processor version kept deployed (Issue #3).


### What this looks like in practice


A typical onboarding pipeline. Your customer uploads one PDF that contains an ID, a payslip, a bank statement and an invoice. Order unknown, document count unknown, all glued together as a single scanned file.


To process that one upload with Document AI you need to:


1. Run a Custom Splitter to break the PDF into sub-documents ($5 / 1k pages).
2. Run a Custom Classifier to tag each sub-document by type ($5 / 1k pages).
3. Route each sub-document to the matching prebuilt processor: ID → $0.10 / doc; Payslip → $0.30 / doc; Bank statement → $0.75 / doc; Invoice → $0.10 / doc (per 10 pages).
4. If any field your customer cares about isn’t covered by the prebuilt schema — and one always isn’t — fall back to a Custom Extractor for that document type ($30 / 1k pages, plus the labeling and training that produced it).
5. Pay for the OCR underneath if you also want raw text for audit ($1.50 / 1k pages).


That $0.10 invoice quickly becomes $0.10 + splitter + classifier + the Custom Extractor pages for the long-tail fields + the OCR pass. Per-document pricing is accurate at the line-item level, but the effective per-document cost is set by the pipeline, not by the parser alone.


### The footnote that breaks the cheap path


A single sentence on the pricing page sets a hard constraint that’s easy to miss:


> “If the input document has multiple eligible forms, only the first is processed and charged the price listed.”


A PDF with three invoices stacked will be parsed by the prebuilt Invoice Parser for the first one only — the rest are silently ignored. That makes a Custom Splitter a hard prerequisite for any multi-document workflow, which is most real-world workflows. The minimum viable pipeline becomes Splitter → Classifier → Parser, not just Parser.


Two more limits worth knowing up front: synchronous requests cap at 10 pages per document, and batch requests cap at 200 pages per document. Mortgage packages, legal discovery and financial disclosures regularly exceed both and have to be chunked, which interacts with rate limits and reinforces the Splitter dependency.


## Issue #2: Custom Processor is a labeling, training and deployment workflow you operate


Once a customer needs a field that isn’t on a prebuilt schema, the workflow shifts from “API call” to “ML lifecycle.” From Google’s own Custom Extractor mechanisms documentation, the steps are:


1. **Collect a labeled dataset.** You draw bounding boxes on the source document and assign each one to a schema field. The native exchange format is Document.proto (Google’s JSON Document schema). Labels can be produced by your team, by your client, or via Google’s annotation console.
2. **Hit the minimums.** A custom model requires at least 10 training and 10 test instances per field. Selective labeling needs at least 100 documents in training and 25 in test before its sampling suggestions become useful.
3. **Train.** Hours of compute per run. Iteration is slow.
4. **Evaluate against a golden set.** That golden set is owner-built and owner-maintained; without it you can’t separate model improvements from sampling variance.
5. **Version and deploy.** Choose which version serves which customer, manage rollout, monitor for regressions.
6. **Repeat** per document type, per customer schema change, per language.


A specific gotcha from the labeling documentation worth surfacing: *“If the value of the label is not correctly detected by OCR, don’t manually correct the value. That would render it unusable for training purposes.”* In other words, when the OCR misreads the source, the affected documents are removed from the training set rather than corrected. This is a workflow rule most annotators discover after a week of labeling.


In practice, teams adopting Custom Processor end up owning workflows typically associated with ML teams: labeling pipelines and UIs (Google’s console is not meant for end users), golden-set management, per-tenant model routing, canary and rollback layers (Document AI doesn’t ship one), evaluation harnesses that run on every retrain, and accuracy dashboards. This is the bulk of what specialist document AI platforms provide as managed infrastructure. Teams that already planned to build it will see Document AI as a reasonable foundation; teams that didn’t tend to discover the scope mid-implementation.


## Issue #3: Hosting bills compound with every version you keep


This is the line item that’s easy to miss in a build-vs-buy spreadsheet, and it often grows larger than the per-page bill in multi-tenant deployments.


From the pricing page:


> “Hosting: $0.05 per hour per deployed processor version you create. One processor version deployed for a year costs $438 ($0.05 × 24 × 365).”


Hosting is billed **per version** , not per processor. Every retrain creates a new version. Keep an old version deployed for rollback and you’re paying for two; keep a previous one for canary and you’re paying for three.


A reasonable multi-tenant footprint — 5 document types × 3 regional variants × 2 hot versions per variant for rollback and canary — already produces 30 deployed versions at $13,140 / year in hosting alone, before processing a single page. The number grows linearly with new customer schema tweaks, retrains and regions.


Aggressively deleting old versions keeps the bill flat but eliminates the rollback story and the ability to reproduce historical predictions, which is a real audit concern in regulated industries. Per-version hosting is a more strategic commercial decision than the per-page rate, because it scales with the operational pattern, not with traffic.


## Issue #4: Field names and schemas are immutable


This one is documented explicitly. From the[labeling guide](https://docs.cloud.google.com/document-ai/docs/label-documents) :


> “Document AI has a limitation that does not allow field names to change.”


And from[Custom Extractor mechanisms](https://docs.cloud.google.com/document-ai/docs/ce-mechanisms) :


> “After you create a processor version, you can’t change or delete fields you have created. You can disable them on the fields page if you no longer need them.”


The official workaround for renaming is a[community-maintained Python tool on GitHub](https://github.com/GoogleCloudPlatform/document-ai-samples/tree/main/incubator-tools/rename_entity_type) that rewrites the dataset.


In practice, this means the schema you ship on day one is the schema you live with, or it’s a re-labeling project. When product asks to rename` supplier` to` vendor_name` to match the rest of the platform, the options are: disable the old field, add a new one and re-label the dataset; use the GitHub script to rewrite the dataset and create a new processor version; or live with inconsistent naming and translate in your application layer. Each of those creates either a new processor version (back to Issue #3) or accumulating technical debt.


For SaaS products where schemas are still maturing, schema immutability forces a choice between over-designing up front or over-versioning over time. Both are real costs.


## Issue #5: Customer-specific corrections don’t compose without tenant isolation


A pattern that comes up repeatedly in multi-customer deployments:


- Customer A corrects “supplier” to the legal name on an invoice.
- Customer B corrects the same field, on the same vendor, to the trade name.


Both corrections are correct for their respective workflow. A single Custom Extractor trained on both, treating them as agreement, will produce a model that’s worse for both customers.


Custom Processor doesn’t expose a per-tenant feedback layer. One model per processor version, one ground truth per processor version. Tenant isolation has to be implemented externally: separate processors per customer, separate datasets, separate training runs, separate deploys, separate monitoring — and a separate $0.05/hour hosting line item per version (Issue #3 applies).


There are two operational responses inside Document AI, both with trade-offs: collapse everything onto a shared model and accept that some customers get worse output, or split per tenant and watch hosting compound. The decision is structural; Document AI is missing the primitive, rather than offering a configuration to set.


## The 3pm-on-a-Friday test


A useful test before committing to any document extraction stack.


A customer emails at 3pm on a Friday: “the` documentNumber` is wrong on this batch of 200 invoices.”


What does each path actually require to ship a fix before close of business?


**Prebuilt processor path.** The field isn’t on the prebuilt schema, or the prebuilt parser is reading it from the wrong location. Neither is configurable. The path forward is either migrating that document type to Custom Extractor — a project, not a Friday fix — or telling the customer the prebuilt processor doesn’t support what they need.


**Custom Processor path.** The remediation loop runs through the lifecycle above:


1. Pull the failing documents from production.
2. Re-label the affected fields in Document.proto format, on your team or via Google’s console.
3. Confirm the dataset still meets the 10-train / 10-test minimum per field after the changes.
4. Run training (hours of compute).
5. Create a new processor version and deploy it (an additional $0.05/hour starts at this point).
6. Roll it out, watching for regressions on other customers sharing the model.
7. Keep the old version deployed for rollback (more hosting) or remove it (no safety net).


Both paths land in the same place: this isn’t a Friday-afternoon fix. It’s at least a week of work, with regression risk for other customers on the same processor. That’s a useful signal to factor in when sizing engineering capacity around a Document AI deployment.


## Smaller (but painful) issues


A few additional items that don’t sink projects but compound over time:


- **OCR is fixed.** Both Document AI paths run on Google’s OCR. If the OCR misreads your specific scan quality, language, or layout (mixed-script documents, low-DPI fax scans, heavy stamps and signatures, handwritten fields), you cannot swap it. This is the constraint that most often caps long-term accuracy improvement.
- **“First eligible form only” billing.** Already covered in Issue #1, worth repeating because it’s the single footnote that quietly forces multi-document workflows off the prebuilt path.
- **Synchronous 10-page cap, batch 200-page cap.** Larger documents must be chunked manually, which interacts with rate limits and increases the Splitter dependency.
- **Limited-access processors.** Some prebuilt parsers — utility bill, procurement splitter/classifier — are gated behind a Google access-request form. That’s an unusual procurement step to discover mid-implementation.
- **Workbench Studio vs API drift.** Studio is the demo surface; the API is the production surface. They use different defaults and parameters, so Studio-based POCs routinely miss production failure modes.
- **Auto-labeling requires manual review.** From the docs: “Schema compliance isn’t enforced during auto-labeling. You must label all instances of each entity for training purposes.” Auto-label saves typing, not validation.
- **No default retention story.** Intermediate JSON, OCR text and exported datasets remain in your GCP buckets until you clean them. Any “no retention” commitment to enterprise customers has to be enforced by your code, not by the platform.
- **No webhooks on long-running operations.** Custom Processor uses LROs that have to be polled, which is the same pattern that causes throughput issues on Azure Document Intelligence at scale.


## Who Google Document AI is good for


The issues above don’t make Document AI the wrong choice in every context. It’s a strong fit when:


- You process one or two document types, with a schema that genuinely matches one of the prebuilt processors.
- You’re treating extraction as best-effort, not as a quality-critical part of the product. The prebuilt parsers’ accuracy on your document mix is good enough as-is, and you don’t plan to invest in iterating on the models or building feedback loops to improve them customer by customer.
- You’re willing to dedicate internal MLOps and labeling capacity to this. Having an ML team isn’t enough on its own — you need to actively allocate at least one engineer to owning the processor lifecycle and the annotation throughput for ongoing retraining.
- Customer schemas are stable, or you have the leverage to push back when a customer asks for fields outside the closed schema.


When most of those hold, Document AI is solid infrastructure and the per-page price is fair. When two or more don’t, the operational overhead from Issues #2, #3 and #5 tends to dominate the total cost of ownership.


## How Invofox approaches these problems


The issues above show up in any architecture where the model is provided as a service but the surrounding pipeline — ingestion, splitting, classification, schema management, feedback, versioning, tenant isolation — is left to the customer to assemble. Invofox is a managed document extraction platform built to absorb that surrounding pipeline, so engineering teams interact with a single contract rather than a portfolio of building blocks.


Where Invofox differs from Document AI in operational shape:


- **One integration covers the full pipeline.** Ingestion, splitter, classifier and extractor sit behind a single API and a single bill. Multi-document PDFs are handled natively rather than requiring an explicit Splitter step.
- **Schemas are mutable.** Adding or renaming a field is a configuration change, not a re-labeling and retraining project.
- **Feedback is a first-class endpoint.** Customers submit corrections through the API; the platform handles per-tenant ground truth, regression checks and rollout internally, without exposing a labeling console to end users.
- **No per-version hosting.** Models are managed by the vendor, so the cost structure scales with traffic rather than with the number of versions kept deployed.
- **Tenant isolation is built-in.** Customer-specific corrections don’t bleed across tenants by default.


The trade-off is the inverse of Document AI’s: less direct control over the underlying model, more of the surrounding infrastructure provided as a managed product. Whether that trade-off fits depends on how much of the operational surface a team wants to own.


For a point-by-point side-by-side on the operational shape of each, see[Invofox vs Google Document AI](https://www.invofox.com/en/vs/google-document-ai/) .


## Final thoughts


The hard part of document extraction in production is rarely the model. It’s the surrounding pipeline: ingestion, splitting, classification, schema management, feedback capture, retraining safety, multi-tenant isolation, drift detection, version hosting, retention. Document AI prices the model accurately and leaves the rest as an exercise for the customer.


For a CTO evaluating the stack, the decision-relevant question isn’t which model has the best benchmark today. It’s how much engineering capacity the team is willing to dedicate, quarter after quarter, to operating the surrounding pipeline — labeling, retraining, version management, tenant isolation, OCR fallbacks. The right answer is different for every team. The number on the pricing page is rarely the decisive input.


If you’re evaluating Document AI right now and want a side-by-side on your own documents —[Book a demo](https://www.invofox.com/en/sign-up) — that’s exactly the kind of conversation we like having.
