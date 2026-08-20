---
schema_version: "1.0.0"
document_id: "53613ebe3c3c687bfde7b991434c60cfb7505c3c04a3762caca104e737ef4dde"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/reducto-classify-endpoint-api"
published_at: "2026-05-21T12:00:00+00:00"
first_seen_at: "2026-07-23T23:11:50.013054+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:38e9b72c88ae241813a3b8adf4f089c1749714598a2ea814387b7b7b9b974130"
---

# Announcing Reducto’s Classify Endpoint: Route Documents Before Processing

When you're building a document agent, you don't get to control what users upload. One session it's a W-2, the next it's a scanned handwritten note, the next it's a 40-page insurance binder.


**Today we're launching our Classify endpoint** , a new lightweight endpoint that tells you what kind of document you're looking at so you can act on it correctly, whether that's routing it to the right pipeline or tagging it in your systems with a class.


Reducto’s Classify endpoint was specifically designed with users in mind who want finer control over document routing at a fraction of the cost and latency.


### Why document type matters


Not all documents need the same treatment.


While our default A medical intake form requires a different extraction schema than a passport scan. A signed contract requires different handling than a draft with a VOID watermark. A purchase order with line items needs a different schema than the cover letter attached to it.


When you send them all through the same pipeline, you're either over-engineering for too many documents or under-investing on the ones you care about. The right approach is to classify first, then route to the appropriate pipeline.


## **How Classify works**


You call /classify with a document and a list of categories you define. Each category has a name and a set of criteria describing what makes a document belong to it. Classify reads the document, matches against your criteria, and returns the best-fitting category.


You don’t need to match against fixed templates. You define what each category looks like in plain language, which means Classify adapts as your document types do.


## **What you can classify**


You define your own categories, so Classify is as flexible as your documents. A few patterns that come up often:


**Document type routing.** Separate invoices from contracts from receipts before passing them downstream. Different types often need different schemas and extraction settings.


**Form variant routing.** Even within the same document category, variants often have different structures and need different extraction configs. Classifying by variant before extraction means each one gets a schema built for it, not a compromise that tries to handle all of them at once.


**Processing characteristics.** Some documents need agentic enhancement before extraction; others don't. You can classify on whether a document appears handwritten or is a low-resolution scan, then configure accordingly.


**Content-based routing.** Mixed document batches don't have to be pre-sorted before they hit your pipeline. More importantly, knowing what you're looking at before extraction means your schemas can be narrow and precise rather than broad enough to cover everything, and that's where accuracy actually improves.


This means Classify can also answer questions that have nothing to do with document type:


- Does this document contain a table?
- Is the text handwritten or machine-generated?
- Does this document have a watermark?
- Is this a signed or unsigned contract?
- Is this a single-page document or multi-page?
- Is this a scanned image or a native digital PDF?


And it can also answer document type questions:


- Is this a W-2, a 1099, or a pay stub?
- Is this an ACORD 25 or a declarations page?
- Is this an invoice, a purchase order, or a receipt?


The criteria are natural language descriptions you write. No training a model, no maintaining a custom classifier. If you can describe what you're looking for, Classify can detect it.


## **Example Snippets**


Each Classify call contains a “classification_schema” which should have a list of categories along with their criterias. The output result


```text
python  from reducto import Reducto


client = Reducto()
upload = client.upload.local(file_path="document.pdf")


response = client.classify.run(
input=upload.file_id,
classification_schema=[
{
"category": "W-2",
"criteria": [
"Wage and Tax Statement issued by an employer",
"Contains boxes for wages, federal income tax withheld, and Social Security wages"
]
},
{
"category": "1099-NEC",
"criteria": [
"Nonemployee Compensation form",
"Reports payments to independent contractors",
"Does not include Social Security or Medicare withholding"
]
},
{
"category": "pay stub",
"criteria": [
"Issued by an employer to an employee",
"Shows pay period earnings, YTD totals, and deductions",
"Not an official IRS tax form"
]
}
]
)
```


The response returns the matched category plus confidence scores and per-criterion reasoning. Unlike free-form model reasoning, the output is structured: every category gets a numeric confidence score and every criterion gets an individual confidence rating, so you can parse the results programmatically rather than interpreting natural language explanations.


Here’s an example of an output where Classify was called to classify documents based on what it contained:


```text
json  {
"job_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
"result": {
"category": "contains_table"
},
"response_confidence": {
"categories": [
{
"category": "contains_table",
"confidence": 1.0,
"criteria_confidence": [
{ "criterion": "Document contains one or more data tables", "confidence": "high" },
{ "criterion": "Tables have rows and columns with structured data", "confidence": "high" },
{ "criterion": "Could be financial data, line items, schedules, or any tabular format", "confidence": "high" }
]
},
{
"category": "handwritten",
"confidence": 0.0,
"criteria_confidence": [
{ "criterion": "Text is written by hand, not typed or machine-generated", "confidence": "low" },
{ "criterion": "Irregular letter spacing and inconsistent formatting", "confidence": "low" },
{ "criterion": "Does not have a clean printed or digital appearance", "confidence": "low" }
]
},
{
"category": "watermarked",
"confidence": 0.0,
"criteria_confidence": [
{ "criterion": "Document contains a watermark, stamp, or overlay text", "confidence": "low" },
{ "criterion": "Common watermarks include DRAFT, CONFIDENTIAL, COPY, or VOID", "confidence": "low" },
{ "criterion": "Watermark may be faint or diagonal across the page", "confidence": "low" }
]
}
]
},
"duration": 1.1
}


```


The confidence field on each category is a 0–1 score. The criteria_confidence breakdown tells you which specific criteria fired and which didn't, which is useful when you're iterating on your category definitions. If a criterion consistently shows low confidence on documents you'd expect it to catch, that's a signal to tighten the description.


Classify is faster and more lightweight than a full Parse or Extract call, and is subsequently cheaper in credits as well.


**What this looks like in a real pipeline**


Once you know the document type, you can route to the right configuration.


Here's a healthcare intake example. A clinic processes both printed patient forms and handwritten clinical notes through the same upload endpoint. Before Classify, every document went through the same extraction call, and the accuracy on handwritten notes was unreliable.


With Classify in front, the document gets the right configuration before extraction starts:


```text
python  classification = client.classify.run(
input=upload.file_id,
classification_schema=[
{
"category": "handwritten_note",
"criteria": [
"Handwritten text, not machine-generated",
"Clinical or medical in nature",
"Likely contains medication names, dosages, or symptom descriptions"
]
},
{
"category": "printed_form",
"criteria": [
"Machine-generated or typed text",
"Structured form layout with labeled fields"
]
}
]
)


if classification.result.category == "handwritten_note":
result = client.extract.run(
input=upload.file_id,
instructions={
"schema": patient_schema,
"agentic": [{
"scope": "text",
"prompt": "This is a handwritten medical note. Pay close attention to medication names and dosages."
}]
}
)
else:
result = client.extract.run(
input=upload.file_id,
instructions={"schema": patient_schema}
)


```


## **Where Classify fits**


To understand how Classify relates to the rest of the API: Parse reads documents and converts them to structured text. Extract pulls specific fields using a schema. Split divides a multi-document file into logical sections. Classify could run before all of these, or even be used as a standalone call. It tells you what you're working with so you can decide how to process it.


This is especially useful inside pipelines. You can chain a Classify call at the front, branch on the result, and send each document type to a different downstream configuration. Mixed document intake, handled cleanly, without requiring pre-sorting before documents hit your system.


We’ve also seen customers build new products and features around Classify – automated tagging, sorting of folders, and renaming of files.


## **Get started**


Classify is available now. Read the[full documentation](https://docs.reducto.ai/classify/overview) for the complete request schema, response format, and configuration options.


If you’d like to talk to our team or get a custom demo, reach out[here](https://reducto.ai/contact?utm_source=blog) .
