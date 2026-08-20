---
schema_version: "1.0.0"
document_id: "44f3f5c8ba3425413f2d6ec5195f054a571f8eb4e2dee60c835c300fc9216a33"
company_key: "yc-invofox"
company: "Invofox"
source_id: "yc-invofox-news-import-19c8879c7fc9"
canonical_url: "https://www.invofox.com/en/blog/gemini-ocr/"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T06:23:05.042843+00:00"
fetched_at: "2026-08-06T06:23:06.950975+00:00"
content_hash: "sha256:1760e265b775271d7f9589859e4d339414a43de65fc1928e08656f91572a3460"
---

# Gemini OCR: How Well It Works and How to Use It (2026)

Table of contents


- Disclaimer
- Can Gemini do OCR?
- How to OCR a PDF with Gemini, step by step
- Two different jobs hide behind the word “OCR”
- What Gemini OCR does well
- Where Gemini OCR breaks at scale: six issues by design
- When Gemini OCR is enough
- How to evaluate Gemini OCR on your documents
- When you need this to run at scale
- Final thoughts


## Disclaimer


I’m Head of Product at Invofox, where we build a managed document extraction platform. I have a horse in this race and you should read what follows with that in mind. As with our[Google Cloud Document AI teardown](https://www.invofox.com/en/blog/the-problems-youll-run-into-using-google-document-ai/) , I’ve kept the body of this post to what Google’s own documentation says and to behaviour you can reproduce yourself, and I’ve kept the comparison with managed alternatives in one clearly labeled section at the end.


Every limit and mechanism cited is taken from the official Gemini API documentation as of August 2026, linked inline throughout.


## Can Gemini do OCR?


Yes — and for a growing number of teams it’s the first thing they try. Gemini is a multimodal model: it accepts images and PDFs natively and reads the text in them, no separate OCR engine involved. Point it at a scanned invoice and ask for the total, and it will usually answer correctly. Ask for JSON and it will return JSON.


What Gemini is **not** is an OCR product. There’s no fixed schema, no per-field confidence, no coordinates, no validation, no processing pipeline — there is a model, a prompt, and whatever you build around them. That distinction is the whole story of this post: the demo takes ten minutes, and the gap between the demo and a production document workflow is where all the interesting decisions live.


If you’re evaluating “Gemini OCR” in 2026, you’re really evaluating two things: how well the model reads documents (genuinely well) and how much infrastructure you’re prepared to build around a raw model (more than most teams expect).


## How to OCR a PDF with Gemini, step by step


The[document understanding docs](https://ai.google.dev/gemini-api/docs/document-processing) cover the mechanics; here is the shortest path from a PDF to structured JSON, in Python, Node.js and raw HTTP.


All three use the[Interactions API](https://ai.google.dev/gemini-api/docs/interactions) , which Google made generally available and now recommends for new development. The older` generateContent` method still works — the migration guide says it “remains fully supported” — but a pipeline you’re building today should start on the surface Google is steering toward. You’ll need an API key from Google AI Studio.


### Gemini OCR in Python, Node.js or curl


```text
pip   install   -U   google-genai   pydantic
```


```text
from   google   import   genai
from   pydantic   import   BaseModel


# This schema IS the field list — it's what you're asking the model for.
class   LineItem  (  BaseModel  ):
description:   str
quantity:   float
unit_price:   float
amount:   float


class   Invoice  (  BaseModel  ):
invoice_number:   str
issue_date:   str
supplier_name:   str
supplier_tax_id:   str   |   None
total_amount:   float
currency:   str
line_items: list[LineItem]


PROMPT   =   (
"Extract the invoice fields defined in the schema. "
"Use null for fields that are not present. Do not guess values you cannot read."
)


client   =   genai.Client()    # reads GEMINI_API_KEY from the environment


# PDFs up to 50 MB / 1,000 pages go through the Files API
doc   =   client.files.upload(  file  =  "invoice.pdf"  )


interaction   =   client.interactions.create(
model  =  "gemini-3.6-flash"  ,    # swap for whichever model is current — see Issue #4
input  =  [
{  "type"  :   "text"  ,   "text"  :   PROMPT  },
{  "type"  :   "document"  ,   "uri"  : doc.uri,   "mime_type"  : doc.mime_type},
],
response_format  =  {
"type"  :   "text"  ,
"mime_type"  :   "application/json"  ,
"schema"  : Invoice.model_json_schema(),
},
)


invoice   =   Invoice.model_validate_json(interaction.output_text)
print  (invoice.invoice_number, invoice.total_amount)
```


```text
npm   install   @google/genai
```


```text
import   { GoogleGenAI }   from   "@google/genai"  ;


// The schema IS the field list — it's what you're asking the model for.
const   invoiceSchema   =   {
type:   "object"  ,
properties: {
invoice_number: { type:   "string"   },
issue_date: { type:   "string"   },
supplier_name: { type:   "string"   },
supplier_tax_id: { type: [  "string"  ,   "null"  ] },
total_amount: { type:   "number"   },
currency: { type:   "string"   },
line_items: {
type:   "array"  ,
items: {
type:   "object"  ,
properties: {
description: { type:   "string"   },
quantity: { type:   "number"   },
unit_price: { type:   "number"   },
amount: { type:   "number"   },
},
required: [  "description"  ,   "quantity"  ,   "unit_price"  ,   "amount"  ],
},
},
},
required: [  "invoice_number"  ,   "total_amount"  ,   "currency"  ],
};


const   prompt   =
"Extract the invoice fields defined in the schema. "   +
"Use null for fields that are not present. Do not guess values you cannot read."  ;


const   ai   =   new   GoogleGenAI  ({});   // reads GEMINI_API_KEY from the environment


const   doc   =   await   ai.files.  upload  ({
file:   "invoice.pdf"  ,
config: { mime_type:   "application/pdf"   },
});


const   interaction   =   await   ai.interactions.  create  ({
model:   "gemini-3.6-flash"  ,
input: [
{ type:   "text"  , text: prompt },
{ type:   "document"  , uri: doc.uri, mime_type: doc.mimeType },
],
response_format: {
type:   "text"  ,
mime_type:   "application/json"  ,
schema: invoiceSchema,
},
});


const   invoice   =   JSON  .  parse  (interaction.output_text);
console.  log  (invoice.invoice_number, invoice.total_amount);
```


No SDK, for when you’re wiring this from a language Google doesn’t ship a client for. Upload the file to the Files API first, then reference its URI — the API key travels in the` x-goog-api-key` header:


```text
curl   -X   POST   "https://generativelanguage.googleapis.com/v1/interactions"   \
-H   "x-goog-api-key:   $GEMINI_API_KEY  "   \
-H   "Content-Type: application/json"   \
-d   '{
"model": "gemini-3.6-flash",
"input": [
{ "type": "text", "text": "Extract the invoice fields defined in the schema." },
{ "type": "document", "uri": "FILE_URI_FROM_UPLOAD", "mime_type": "application/pdf" }
],
"response_format": {
"type": "text",
"mime_type": "application/json",
"schema": { "type": "object", "properties": { "invoice_number": { "type": "string" }, "total_amount": { "type": "number" } } }
}
}'
```


### What the pieces actually do


A few mechanics worth knowing, all from the documentation:


- **The schema is the request; the prompt is the behaviour.** The fields you want are declared in` response_format.schema` — that’s the contract, and it’s what the model receives as the field list and types. The text prompt carries only how to behave on ambiguity (“use null”, “don’t guess”). Resist listing the fields in both places: two sources of truth drift the moment someone edits one.
- **Size limits.** Gemini supports PDFs up to **50 MB or 1,000 pages** . Small files can be passed inline in the request; anything bigger goes through the Files API, where uploads are kept for 48 hours.
- **Pages are tokens.** Each document page is equivalent to **258 tokens** of image input, and pages are rescaled internally (down to a maximum of 3072×3072, up to at least 768×768). This is the unit your bill is denominated in — more on that in Issue #6.
- **The schema constrains shape, not truth.** It guarantees syntactically valid JSON matching your model. It does not guarantee the *values* are right — which is why the Python sample validates the response rather than trusting it. Google’s[structured output docs](https://ai.google.dev/gemini-api/docs/structured-output) say it plainly: *“always validate values in your application.”*


That’s the whole tutorial. It works, it’s impressive, and you can build a demo with it before lunch. Now for what the demo doesn’t show.


## Two different jobs hide behind the word “OCR”


Before going further, it’s worth separating the two things people mean when they ask whether Gemini can do OCR, because they have different success criteria:


- **Read the whole document.** Turn a PDF into structured Markdown or JSON with the layout, tables and reading order preserved, so it can feed a search index, a RAG pipeline or your own extraction step. Success here means *nothing was lost or reordered* . This is the job our[Document Parsing API](https://www.invofox.com/en/document-parsing/) does.
- **Pull out specific fields.** Get` total_amount` ,` invoice_number` and the supplier’s tax ID as typed, validated values you can post to an ERP. Success here means *this number is right* . This is the job our[OCR API](https://www.invofox.com/en/ocr-api/) does.


The quickstarts above are the second job — that’s what the schema is for. Gemini will do either one, and it does both surprisingly well on a good document — which is exactly why it’s worth being precise about what happens when the document isn’t good. **The six issues below apply to both jobs** , because they come from the shape of a general-purpose model, not from which output you asked it for.


## What Gemini OCR does well


Credit where it’s due — on several axes, a frontier multimodal model beats every classic OCR engine we’ve ever benchmarked:


- **Clean documents read almost perfectly.** Digitally-generated PDFs and good scans of standard business documents come back with very few character-level errors.
- **Handwriting and stamps.** Handwritten notes, signatures crossing text, stamps over fields — cases where template-based OCR collapses — often come back usable.
- **Layout diversity without templates.** A weird invoice format it has never seen is just another prompt. There is no per-vendor template to build, which is the single biggest workflow difference versus legacy OCR.
- **Languages and mixed scripts.** Multilingual documents, including mixed-language line items, are handled in one pass.
- **Tables.** Ask for a table as Markdown or as a JSON array and the reading order is usually respected — something we test explicitly in our[GPT-4o vs Claude vs Invofox benchmark](https://www.invofox.com/en/blog/document-parsing-using-gpt-4o-api-vs-claude-sonnet-3-5-api-vs-invofox-api-with-code-samples/) .


If your problem is “read this document once, roughly” — Gemini OCR mostly solved it. The issues start when the problem is “read 50,000 documents a month and be right about the numbers.”


## Where Gemini OCR breaks at scale: six issues by design


None of these are bugs. They’re consequences of using a general-purpose generative model as an extraction engine, and they don’t go away with a better prompt.


### Issue #1: The output depends on the prompt


The same document with two slightly different prompts returns different field names, different date formats, different treatments of an ambiguous line. Your “schema” lives in natural language, so every prompt tweak is an untested deploy to production. Structured output narrows this — the *shape* becomes stable — but which value lands in` total_amount` when the document shows three candidate totals is still the model’s call, made independently on every request. There is no configuration surface where correct behaviour can be pinned; there is only prose.


### Issue #2: No confidence scores


Traditional OCR engines return a confidence signal per word; extraction products return one per field. The Gemini API returns generated text, and nothing else. You cannot ask the pipeline “how sure are you?” and get a measurement back. You *can* ask the model to self-rate — and it will produce a plausible-looking number, generated exactly the way it generates everything else. It’s not an observation of the model’s internal state, and building a human-review routing rule on top of it means building on fiction. Without confidence, every document is either fully trusted or fully reviewed; there is no middle tier.


### Issue #3: No validation — failures are silent


When a classic parser can’t read a field, it fails loudly. When Gemini can’t read a field, it answers anyway. Feed it a rotated or half-cropped photo of an invoice and you don’t get an error — you get a fluent, well-formatted JSON with values that look right. Totals that don’t equal the sum of the line items. A tax ID with a transposed digit. An issue date pulled from the due-date box. Nothing in the response distinguishes these from correct output; syntactically, they’re identical.


The whole-document job fails just as quietly. A two-column page merged into one reading order, a table row silently dropped because a cell was empty, a footnote spliced into the paragraph above it — the Markdown that comes back is perfectly well-formed either way. Downstream, a RAG pipeline will happily embed and retrieve the mangled version, and the failure surfaces months later as an answer nobody can trace back to a page.


This is the same[3pm-on-a-Friday test](https://www.invofox.com/en/blog/the-problems-youll-run-into-using-google-document-ai/) we apply to every extraction stack: a customer reports that` total_amount` is wrong on a batch of 200 invoices — what does the fix look like? With a raw model the answer is “edit the prompt and hope,” because there is no feedback mechanism, no per-field rule to adjust, and no way to guarantee the edit doesn’t degrade a different document type. Silent failure plus no remediation loop is the combination that disqualifies raw-model OCR for most finance-grade workflows.


### Issue #4: The model underneath you keeps changing


The` gemini-3.6-flash` in the quickstarts above[launched in July 2026](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-6-flash-3-5-flash-lite-3-5-flash-cyber/) ; a year ago the equivalent line was 2.5, and versions retire on a schedule you don’t control. The API surface moves too — the code above uses the Interactions API precisely because the method most tutorials still show,` generateContent` , is already the previous generation. Every model update is a silent change to your extraction behaviour: fields that parsed one way parse another, edge cases shift, and your pipeline’s accuracy moves without a single line of your code changing. We wrote up what[Gemini’s deprecation cadence means for production AI](https://www.invofox.com/en/blog/gemini-just-updated-what-it-means-for-ai-in-production/) — the short version is that pinning a version buys you months, not stability, and re-validating your entire document mix on every migration becomes a recurring engineering tax.


### Issue #5: Rate limits are tiered, and the ceiling finds you


API quotas are organized in[spend-based tiers](https://ai.google.dev/gemini-api/docs/rate-limits) — Free, then Tier 1 through 3 as your billing history grows — measured in requests per minute, input tokens per minute and requests per day. Past your limit, the API returns` 429 RESOURCE_EXHAUSTED` , which means a Monday-morning backlog of 5,000 scanned documents doesn’t get processed at your speed; it gets processed at your tier’s speed, behind backoff-and-retry logic you now own. Per-model numbers live in Google AI Studio rather than on a public table, so capacity planning starts with an empirical test.


### Issue #6: Pricing is per token, and pages are tokens


Gemini is[priced per token](https://ai.google.dev/gemini-api/docs/pricing) , and for documents that unit works against predictability. Each page bills as image-modality input (~258 tokens), the prompt bills as text, and the extracted JSON bills as output — which for a long invoice is not small. Re-runs multiply everything: a retry after a` 429` , a second pass with a refined prompt, a re-validation after a model update — each is a full-price pass over the same document. Per-token pricing is excellent for chat and genuinely awkward for “process every page exactly once at a known cost,” which is how document operations think about budgets.


## When Gemini OCR is enough


An honest list — several of these describe us at Invofox, too, because we prototype with raw models constantly:


- **Prototypes and feasibility checks.** Nothing gets you from “can this document be read at all?” to an answer faster.
- **Low volume with a human in the loop.** If someone reviews every result anyway, silent failures are caught by design.
- **One-off digitization projects.** A box of archive scans that needs to become a spreadsheet once, not a pipeline that runs every day.
- **Internal tools with soft failure costs.** If a wrong value costs an eye-roll rather than a mis-payment, the validation gap may never bite.
- **Teams that already planned to build the surrounding stack.** If evaluation harnesses, verification rules and review routing are on your roadmap anyway, a raw model is a legitimate foundation.


If most of your workload looks like that list, stop reading and go ship — Gemini OCR is a fine choice, and the free tier makes it a cheap one.


## How to evaluate Gemini OCR on your documents


Whatever you do, don’t decide based on a benchmark — ours included. Method, not numbers:


1. **Build a ground-truth set from your real traffic.** 50–100 documents, hand-verified. Include the ugly tail: rotated photos, low-DPI faxes, stamps over amounts, multi-column pages, multi-document PDFs — the tail is where document stacks are actually decided.
2. **Score at the granularity you’ll depend on.** If you’re extracting fields, score per field: “95% of documents mostly right” and “the total is right 95% of the time” are radically different claims for an AP workflow. If you’re parsing whole documents, score structure — tables reconstructed intact, reading order preserved, nothing dropped — not a character-level diff that averages away the one table that broke.
3. **Count silent errors separately.** The metric that matters isn’t accuracy — it’s how many *wrong outputs arrived looking right* . That’s the number your downstream systems will eat.
4. **Re-run the same set on every model version.** Issue #4 makes this a recurring job, not a one-time gate.
5. **Price the full loop.** Tokens for inputs, outputs, retries and re-validations — not the single-pass estimate.


This is the same discipline we apply to our own models — we’ve written up how we think about[evaluation and accuracy](https://www.invofox.com/en/evaluation-and-accuracy/) if you want the deeper version.


## When you need this to run at scale


Everything in the issues list comes down to one structural fact: a raw model gives you generated output, and production document workflows need *output you can trust without reading it* . Closing that gap means building schema enforcement, verification, routing to human review, a feedback loop for corrections, evaluation across model updates, and queueing that survives rate limits. That surrounding layer — not the model — is most of the engineering.


That layer is the product Invofox sells, so weigh this section accordingly. Which of the two doors applies depends on the job you’re doing:


- **Whole document.** Our[Document Parsing API](https://www.invofox.com/en/document-parsing/) returns the full document as structured Markdown or JSON with layout, tables and reading order preserved — the input format a RAG or search pipeline needs, without the silent reordering.
- **Specific fields.** Our[OCR API](https://www.invofox.com/en/ocr-api/) runs extraction behind validation: every field is checked against your schema and business rules before it reaches you, corrections feed back per tenant, and the contract stays stable when the models underneath change.


The trade-off is the mirror image of the raw-model path in both cases: less direct control over prompts and models, more of the operational surface handled for you.


The honest way to compare isn’t a feature table — it’s a[performance report](https://www.invofox.com/en/performance-reports/) on your own documents: same ground-truth set, measured accuracy and silent-error rate. If you’re weighing Gemini OCR against a managed API right now,[that’s a test we’ll happily run with you](https://www.invofox.com/en/sign-up/) .


## Final thoughts


Gemini reads documents better than any general-purpose model before it, and the ten-minute demo is real. But “the model reads well” was never the hard part of document automation at scale — the hard part is knowing *which* outputs to trust, catching the ones that fail silently, and keeping behaviour stable while models churn underneath you. That’s true whether you’re pulling six fields off an invoice or turning a 200-page policy into Markdown. Gemini gives you none of it, by design; it gives you a brilliant reader and a blank page where the pipeline should be.


Prototype with it. Ship low-stakes workflows with it. And when the numbers start mattering — measure before you trust.
