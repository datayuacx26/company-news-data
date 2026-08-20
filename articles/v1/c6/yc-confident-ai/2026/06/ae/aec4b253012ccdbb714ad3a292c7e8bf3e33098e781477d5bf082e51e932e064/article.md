---
schema_version: "1.0.0"
document_id: "aec4b253012ccdbb714ad3a292c7e8bf3e33098e781477d5bf082e51e932e064"
company_key: "yc-confident-ai"
company: "Confident AI"
source_id: "yc-confident-ai-news-import-3974c9e901d3"
canonical_url: "https://www.confident-ai.com/blog/launch-week-q2-2026-day-3-annotation-forms"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-21T14:44:45.986766+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:a9f4720240e7e608f616ec9b7de57f61eebc32ad1a1d335e35b4b3ec712c96a4"
---

# Introducing Annotation Forms: Capture any human feedback without leaving Confident AI

Blog


# Introducing Annotation Forms: Capture any human feedback without leaving Confident AI


Jun 24, 2026


·


5 min read


Jeffrey Ip


Co-founder @ Confident AI. Creator of DeepEval & DeepTeam. Building an unhealthy LLM evals addiction. Ex-Googler (YouTube), Microsoft AI (Office365).


Today we're launching **Annotation Forms** on Confident AI — a configurable set of fields that defines exactly what reviewers capture when they annotate your data. Annotations aren't new on the platform — thumbs up/down, 5-star ratings, and expected outputs have been there for a while — but those are a fixed set of fields, so SMEs and PMs kept exporting data to finish the real review in a spreadsheet. Annotation Forms close that gap: define your own fields and every annotation comes back structured, on the platform, and ready to feed back into your evals.


## What are Annotation Forms?


An annotation form is the set of questions a reviewer answers when they open an item in an annotation queue. You design it once in **Project Settings → Annotation** , and from then on every reviewer working that queue sees the same fields, in the same order, capturing the same data.


Each form is a list of **fields** , and each field has three parts:


- **Field** — the question the reviewer is asked, e.g. *"Describe how good the AI response is."*
- **Type** — how the answer is captured (more on the types below).
- **Description** — optional guidance shown to the reviewer, e.g. *"Focus on characteristics such as response length, and conciseness."*


Any field can be marked **Required** , so reviewers can't submit until the questions that matter are filled in.


app.confident-ai.com


Build an annotation form field by field in Project Settings on Confident AI.


## The problem: review always ended up in a spreadsheet


You could already annotate on Confident AI — leave a thumbs up or down, give a 5-star rating, write an expected output. That covers the common cases, but it's a fixed set of fields. The moment a team needed to capture anything beyond them, they ran into a wall.


So what we kept seeing was this: the SME or PM does the part the product supports, then **exports the data and does the rest in an Excel sheet** . The severity rating, the failure category, the three checkboxes for what went wrong, the rubric score their team actually cares about — all of that lives in a spreadsheet next to the export, disconnected from the traces it describes.


That's the worst of both worlds:


1. **The real review lives outside the platform.** The judgments that matter most are in a file on someone's laptop, not attached to the traces, datasets, or evals they're about.
2. **Nothing flows back.** A score in a spreadsheet can't become a golden expected output, alignment data for an LLM judge, or a filterable signal in your dashboards. Someone has to re-import and re-code it by hand — so usually no one does.
3. **No consistency across reviewers.** Every SME builds their own columns, their own scale, their own definition of "good." There's no shared schema, so the results can't be compared or aggregated.


Annotation Forms fix this at the source: define the exact fields your reviewers need — once, in the product — so the entire review happens on the platform and the spreadsheet detour disappears.


## Field types


When you add a field, you pick how its answer is captured. Forms support seven types, so you can match the question to the right input instead of forcing everything into text:


- **Text** — a free-form text answer. Best for open-ended explanations and qualitative notes.
- **Number** — a whole number answer.
- **Decimal** — a decimal number answer, for fractional scores and measurements.
- **Yes / No** — a simple yes/no toggle for binary judgments like *"Did the AI answer the question?"*
- **Single choice** — pick exactly one option from a list, ideal for categories like severity or sentiment.
- **Multiple choice** — pick one or more options, for tagging every issue that applies to a response.
- **Criteria** — score a single annotation criterion, capturing a structured rating against a definition you set.


Mix and match these across a single form — a Yes/No for "is it correct," a single choice for severity, a Criteria score for helpfulness, and a Text field for notes — to capture exactly the dimensions you care about.


## Building a form


Forms are built in a drag-and-drop editor:


1. Open **Project Settings → Annotation** and create a **New Form**
2. Click **Add** to append a field
3. Write the question, pick a **Type** , and optionally add a **Description** to guide reviewers
4. Toggle **Required** on the fields that must be answered
5. Reorder fields by dragging the handle on the left of each field
6. **Save**


Switch to the **Preview** tab at any time to see the form exactly as a reviewer will — so you can sanity-check the wording and ordering before it goes live.


## From annotations to evals


The point of structured annotations isn't just tidy data — it's that structured data flows directly back into the rest of Confident AI. Because every answer has a known type:


- **Scores and criteria** become quantitative signals you can filter, chart, and threshold on, just like automated metrics.
- **Choice and yes/no fields** become labels you can slice your traces by in the Observatory and Dashboards.
- **Reviewed items** become aligned data for your LLM judges and golden expected outputs for your datasets.


Human judgment stops being a comment nobody reads and becomes a first-class input to your evaluation workflow.


## Get started


Annotation Forms are live on Confident AI now.


- Read the[annotation documentation](https://www.confident-ai.com/docs/llm-evaluation/annotation) for the full reference on every field type and option
- Open **Project Settings → Annotation** in your project to build your first form


And keep an eye on the blog this week — we've got more shipping.


---


Do you want to brainstorm how to evaluate your LLM (application)? Ask us anything in our[discord](https://discord.com/invite/a3K9c8GRGt) . I might give you an "aha!" moment, who knows?


## Standardize AI Quality for the entire org, not just individual teams


Give all AI use cases the same quality bar with all-in-one evals, observability, and red teaming, and enforce them at scale.


AI evals for product teams, not just engineers.


Observability for production traffic.


Red teaming for security and safety.


AI governance for multiple projects at once.


[Book a Demo](https://www.confident-ai.com/book-a-demo?utm_source=blog&utm_medium=content&utm_campaign=q2_2026_launch_week&utm_content=product)[Or sign up](https://app.confident-ai.com/auth/signup?utm_source=blog&utm_medium=content&utm_campaign=q2_2026_launch_week&utm_content=product)


In this story


- What are Annotation Forms?
- The problem: review always ended up in a spreadsheet
- Field types
- Building a form
- From annotations to evals
- Get started
