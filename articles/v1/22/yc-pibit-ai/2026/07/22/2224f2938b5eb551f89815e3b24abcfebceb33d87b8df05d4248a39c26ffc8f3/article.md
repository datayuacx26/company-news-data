---
schema_version: "1.0.0"
document_id: "2224f2938b5eb551f89815e3b24abcfebceb33d87b8df05d4248a39c26ffc8f3"
company_key: "yc-pibit-ai"
company: "Pibit.ai"
source_id: "yc-pibit-ai-news-import-b491cbbe82b1"
canonical_url: "https://pibit.ai/blog/bordereau-ingestion-automation"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-22T08:59:11.961361+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:09c206efb212fa0919ae8a928c55eca25d6cdeefa796deedfa79518bc10a14a7"
---

# Bordereau Ingestion Automation: Turning MGA files into one schema

Bordereau ingestion automation is the process of parsing the monthly or quarterly files that coverholders send to a capacity provider, reading every row of premium, claims, and policy detail across each MGA's own format, and mapping all of it into one consistent account-level schema the carrier can query. Done manually, it is a spreadsheet reconciliation job that grows linearly with every new program. Done as software, it turns a stack of inconsistent files into a single portfolio view without adding headcount. The point of this piece is the mechanics: what actually happens between the file landing in an inbox and the data being usable, and why that pipeline is harder than it looks.


We have already made the case for why carriers need this: a delegated book is invisible until the bordereau arrives, and by then the carrier has been on-risk for weeks. This article assumes you accept that problem and focuses on the fix, the mechanics of turning those files into usable data.


## What is bordereau ingestion automation?


A bordereau is the periodic file in which a coverholder reports what it bound on the carrier's paper: premium, policies, endorsements, and often loss activity. Bordereau ingestion automation is the layer that takes those files and produces structured, validated, account-level data on a schedule, without a person retyping figures into a master workbook.


The distinction that matters is between a report and a dataset. A bordereau received as a PDF or a loosely formatted spreadsheet is a report: it states totals and is designed to be read, not queried. Automated ingestion turns that report into a dataset, where each policy is a record and the whole book can be filtered, aggregated, and compared against appetite.


A bordereau as a report The same bordereau as a dataset


States totals, built to be read Each policy is a record, built to be queried


One aggregate premium line Premium split by class, geography, and coverholder


Answers arrive next reporting cycle Answers arrive the same day


Appetite drift is invisible until it compounds Appetite drift is a filter you can run


You trust the coverholder's math You reconcile premium against bound policies


For a primer on the underlying arrangement, see the glossary entry on[delegated underwriting authority](https://www.pibit.ai/insurance-knowledge/delegated-underwriting-authority) .


## Why do bordereau files resist a single schema?


If every MGA filed the same template, this would be a solved problem and no one would write about it. They do not. The friction is structural, and it compounds with every coverholder a carrier adds.


Consider what varies across a portfolio of programs:


Source of variance What it looks like in practice


Column naming One file labels a field "Gross Written Premium," another "GWP," another "Prem (100%)." Same number, three headers.


Layout Data starts on row 1 in one file and row 9 in another, under a merged title block and a logo.


Date and currency formats Effective dates as MM/DD/YY, DD-MM-YYYY, or Excel serial numbers. Premium with and without currency symbols and thousands separators.


Class and coverage codes Coverholder-specific class codes that do not map cleanly to the carrier's own rating classes.


Granularity Some files report per policy, others per transaction, others rolled up to program totals that cannot be decomposed.


Completeness Missing endorsement records, blank claim fields, and policies that appear one month and vanish the next without a cancellation row.


None of these is hard in isolation. The problem is volume and drift. A carrier running thirty programs is reconciling thirty formats, and those formats change quietly when a coverholder updates its own system. Template-trained tools that were tuned to last quarter's layout break the moment a header moves. That is why the durable approach reads the meaning of a field rather than its fixed position, so a moved column or a renamed header does not stop the pipeline.


## How does the ingestion pipeline actually work?


A production-grade bordereau ingestion pipeline runs in five stages. The sequence is deliberate, because each stage protects the one after it.


Stage What happens What it catches


1. Intake and classification Identifies the coverholder, program, period, and file type (premium, claims, or combined) as the file lands, usually by email Misfiled and duplicate submissions, before they pollute the data


2. Extraction Reads every row and field from a spreadsheet, a converted PDF, or an embedded[loss run](https://www.pibit.ai/insurance-knowledge/loss-run) Data trapped in mixed formats and in documents traveling with the file


3. Schema normalization Maps every field to one canonical schema, standardizes dates and currency, translates coverholder class codes to rating classes Thirty formats that disagree on field names and layout


4. Validation Checks typed fields against rules and history, with a person reviewing the exceptions the model flags Mapping errors and impossible month-over-month swings


5. Account-level output Writes validated records to the system of record, queryable by class, geography, coverholder, and concentration A book you could otherwise see only as one aggregate line


The economics of getting stage four right are not subtle. The exceptions a pipeline surfaces are the bill you actually pay in underwriter and operations time, so the vendor question worth asking is not how fast the SLA is but how few items escalate at that SLA. A short diligence list keeps the conversation honest:


- What is your field-level accuracy, and is it contractual?
- What share of files clear without a human touching them, and what is the escalation rate at your stated SLA?
- Do you normalize coverholder class codes to our rating classes, or hand back raw labels?
- Can you read PDFs and embedded loss runs, or only clean spreadsheets?
- Do you sit on top of our policy administration system, or ask us to move to yours?


## What does account-level output change for the carrier?


A bordereau read as a report tells you what a coverholder says happened, in aggregate, after the fact. The same bordereau ingested as data lets an underwriting team do the things the summary never allowed: test appetite drift before it compounds, catch a geographic or class concentration mid-quarter, and reconcile reported premium against bound policies rather than trusting a total.


This is the difference between monitoring a delegated book and merely receiving statements about it. When the underlying data is structured and current, portfolio steering becomes a routine query instead of a quarterly forensic exercise. The same intake discipline that carriers now apply to direct[submission intake](https://www.pibit.ai/blog/submission-intake-automation-insurance) extends to the delegated channel, which is where a growing share of premium now sits and where visibility has historically been weakest. Premiums placed through MGAs and other delegated underwriting authorities reached roughly $89.9 billion in 2024, a fourth consecutive year of double-digit growth ([AM Best](https://news.ambest.com/newscontent.aspx?refnum=266436) ). Broader estimates that add Lloyd's business put the figure near $114.1 billion ([Conning](https://www.prnewswire.com/news-releases/conning-us-mga-premiums-climb-16-to-114-billion-in-2024-302502760.html) ). A book that large should not be the least visible one a carrier holds.


## How do carriers adopt this without replacing the PAS?


This is where a modular platform matters, and it is the practical answer to the objection that stops most delegated-data projects before they start: the fear of a multi-year systems replacement.


Pibit.AI is built as a set of independent modules rather than a monolith. Document and loss-run extraction sit in DocumentCURE, clearance and routing in ClearCURE, the underwriting workbench in WorkflowCURE, with RiskCURE and ResearchCURE available when a program is ready for scoring and external enrichment. A bordereau is built from exactly the documents DocumentCURE already handles at scale, the loss runs and the premium and policy schedules, so the same template-agnostic extraction that reads a standalone loss run reads the file that reports a delegated book. Each module can run on its own. A carrier can start with document ingestion for a single high-volume program, prove the accuracy and the time saved, then extend into clearance and workflow at its own pace.


Crucially, none of this requires ripping out the policy administration system. The platform sits on top of an existing environment such as Guidewire, Duck Creek, or Insurity and feeds validated data into it, which means the first program can go live in weeks rather than the twelve to eighteen months an internal build would take. Modularity is what makes bordereau ingestion a starting point instead of a transformation program. You buy the piece that hurts most today and add the next piece when the return is proven.


## How accurate is automated bordereau ingestion at volume?


Accuracy is the reason to automate and the reason automation fails when it is done carelessly. Pibit.AI operates to a 99.9% contractual field-level accuracy standard, delivered through AI extraction paired with a managed human-in-the-loop review team that validates the data before it reaches the carrier. In delegated business that standard is not a vanity metric. A single mismapped premium or class field flows straight into the aggregate a carrier uses to judge a program, so the cost of an error is measured in portfolio decisions, not just rekeying.


The volume case is the one that convinces operations leaders. In one workers' compensation MGA program, Pibit.AI processed 1,053 loss runs and 626 submissions in a single month with zero errors. That is the shape of the problem bordereau ingestion is built for: high, repeating volume where manual reconciliation degrades as the book grows and automated ingestion holds its accuracy as volume rises.
