---
schema_version: "1.0.0"
document_id: "aaa2e47da594c0af888b8b0ae5ad1dce5dfb767916ed65a7cd277fdfb473d1c4"
company_key: "yc-pulse-3"
company: "Pulse"
source_id: "yc-pulse-3-news-import-f90f167021ce"
canonical_url: "https://www.runpulse.com/blog/capturing-what-the-pen-says-handwritten-mark-detection-in-pulse"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-24T09:39:59.698897+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:b9b58d601b9694f85929947eec200e99d27953d65aaa3541cf97d750331e4a84"
---

# Capturing what the pen says: Handwritten Mark Detection in Pulse

Document extraction has a problem that sits in plain sight on millions of forms processed every day, and it has nothing to do with tables, handwriting, or degraded scans but with the pen mark itself. A checkmark drawn next to an option, a circle around a response, or a line struck through a choice that was reconsidered are how people communicate decisions on paper.


Today Pulse is closing that gap with the launch of Selection Mark, a model built specifically to read the marks people make when they fill out a form.


## **The Problem the Research Confirms**


The Snowflake CheckboxQA benchmark is the first rigorous evaluation of selection detection on real-world forms. The authors tested a range of frontier models, including GPT-4o and Claude, on the specific task of determining whether checkboxes are selected. The findings are unambiguous: large language models perform poorly at this task, and the failure modes are predictable: marks placed outside a printed checkbox boundary are missed, non-standard mark types such as circles, underlines, crosses, and strikethroughs are frequently ignored, and checkboxes embedded in table columns are handled inconsistently. The models are not confused by unusual documents; they are confused by the most ordinary act a person performs when filling out a form.


This is not a benchmark curiosity but the daily reality for any workflow that depends on form data, whether a compliance audit form where the inspector circled "Yes" rather than ticking the box, a medical intake questionnaire where a patient underlined their insurance type, or a government survey where a respondent crossed out an option and marked the one next to it. In all of these cases, a VLM-only extraction pipeline returns the pre-printed state of the form, as if nobody had touched it.


## **Why the Obvious Fixes Do Not Work**


The first instinct is to crop each option region from the page and send each crop to a vision model with a prompt asking whether a mark is present. This approach works in a narrow range of conditions and fails in the ones that matter most.


The latency problem alone rules it out at production scale, because a typical form page has dozens of selectable options, and processing each one as a separate model call adds tens of seconds to every extraction job, which is not a budget any customer-facing workflow can absorb.


Beyond latency, the cropping approach is limited to options the extraction system has already identified. If the mark falls outside that set, whether an inline "Yes / No" on a single line, a checkbox column in a table, or a handwritten annotation in the margin, the loop never reaches it. The architecture of the fix reproduces the blind spot of the original problem.


## **A Different Approach**


The Snowflake paper frames mark detection as a fundamental limitation of current language models, and it is right. The way to close the gap is not to ask a language model to try harder at a task it was not built for. It is to train a model specifically for this problem, on data that represents the full range of ways people actually mark forms.


Pulse trains on a large corpus of real and synthetically generated examples across a broad range of document types, form layouts, and marking styles: checkmarks, crosses, circles, underlines, strikethroughs, and brackets across thousands of real scanned forms. A model fine-tuned this way sees ink on paper as the primary signal rather than as noise around the text it is trying to read. It learns to find marks where general-purpose models see nothing, and to distinguish real marks from the printed characters and form elements that surround them.


The result is a pipeline that reads what the pen said, not just what the printer produced, adding under 20 milliseconds of processing time per page with no additional API calls and no per-option round trips.


## **Trained to See What Others Miss**


This level of accuracy is the direct result of training a model specifically on this problem at a scale that general-purpose systems are not designed to match.


Pulse's detection model was fine-tuned on tens of thousands of document pages spanning a wide range of form types, layouts, and handwriting styles. Roughly half of that training corpus consists of real scanned documents with marks annotated by human reviewers. The other half is synthetically generated: blank forms with programmatically drawn marks that simulate the physical properties of real handwriting, including pen tremor, ink fading, stroke discontinuities, and the natural variation in how different people draw a checkmark or a circle. Together, they expose the model to the full range of marking conventions it will encounter in production.


Training at this scale on this specific signal is what produces a model that finds marks general-purpose systems treat as invisible. A frontier VLM has seen handwritten marks in its training data, but they represent a vanishingly small fraction of billions of training examples. Pulse's model has seen little else, and that specialisation is what makes the difference.


## **Live in the Pulse API**


Mark detection runs as a feature of the standard Pulse extraction API, enabled by a detect_selections parameter. When enabled, Pulse returns the enriched extraction result with selected: true annotated on matched elements. The rest of the document structure is unchanged, and downstream systems receive the same JSON they always did, with selection state added where marks were found.


Read the docs[here](https://docs.runpulse.com/api-reference/endpoint/extract) !


## **Seeing It in Practice**


The two examples below are drawn from real forms and illustrate the range of mark styles Pulse handles correctly.


The first is a standard multiple-choice question from a standardised test. Question 28 has option F underlined and question 31 has a cross beside option A. These are two different mark styles on the same page, neither of which falls cleanly inside a printed checkbox, yet Pulse identifies both correctly.


The second is considerably harder, a science passage with numbered steps that has large sweeping marks drawn across multiple lines of text, encircles that span entire sentences and cross over printed content. These are exactly the kind of marks that break general-purpose models: large, irregular, not confined to any checkbox boundary, drawn over complex mixed-content layouts with embedded figures and tables, yet Pulse detects all of them.


‍


*Note: the marked options shown are for illustrative purposes only and do not represent the correct answers to these questions.*


Figure 1. A few examples of the various selection styles that Pulse detects.


## **What This Unlocks**


The practical effect is that a category of form data that was previously invisible to automated extraction becomes readable, from compliance checklists where inspectors mark findings by hand and insurance claims where adjusters circle applicable conditions to healthcare intake forms where patients tick, cross, or circle their answers and surveys where respondents annotate in any of a dozen different ways. Every one of those workflows has until now relied on a person reading the original document and reconciling it against whatever the extraction system returned, and Selection Mark removes that step by making the extraction output reflect what the document actually says, including what the pen said.


Selection Mark is available today as part of the standard Pulse extraction API.
