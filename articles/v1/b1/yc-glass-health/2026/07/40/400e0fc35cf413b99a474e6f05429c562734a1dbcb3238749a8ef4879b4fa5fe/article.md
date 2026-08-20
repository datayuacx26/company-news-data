---
schema_version: "1.0.0"
document_id: "400e0fc35cf413b99a474e6f05429c562734a1dbcb3238749a8ef4879b4fa5fe"
company_key: "yc-glass-health"
company: "Glass Health"
source_id: "yc-glass-health-news-import-5021dfc91ecf"
canonical_url: "https://glass.health/resources/clinical-ai-api-ehr-integration"
published_at: null
first_seen_at: "2026-07-23T10:30:56.315957+00:00"
fetched_at: "2026-08-09T22:41:06.044449+00:00"
content_hash: "sha256:36df904ddedc4dff9206119b26c08431b9e12681857aadd99953a6f926e16a07"
---

# Clinical AI with EHR Integrations

Glass brings clinical AI into supported EHR workflows using authorized chart context. That lets clinicians draft notes, generate differentials, build assessment-and-plan output, and ask cited clinical questions in one workflow.


The important framing is clinical AI with EHR integrations, not a generic "API integration." If you are embedding Glass into your own software product, start with[/for-developers#features](https://glass.health/for-developers#features) . If you are evaluating Glass inside an EHR workflow, this page is the right starting point.


## What we mean by EHR integration


For Glass, EHR integration means chart-context clinical AI. The EHR remains the system of record. SMART on FHIR authorizes access to the patient context needed for the clinician’s session, and Glass uses that context to support ambient scribing plus clinical decision support.


That is why this is more than a generic API connector story. The point is not to bolt a chatbot beside the chart. The point is to make the chart context useful for real clinical work: documentation, differential diagnosis, assessment-and-plan drafting, chart summarization, and cited Q&A.


## How SMART on FHIR fits into the workflow


SMART on FHIR is one of the standards-based patterns healthcare teams use to connect applications with FHIR-based data systems. In practice, the buyer question is simple: can the workflow bring the right chart context into the clinician’s review path without forcing the clinician to restate the case manually?


The workflow is straightforward:


1. The implementation authorizes the supported EHR workflow.
2. Glass uses the authorized patient context needed for the session.
3. Glass uses that context to support ambient documentation and clinical reasoning.
4. The clinician reviews the output inside the workflow they are already running.


The practical value is not technical elegance for its own sake. It is that the same patient context can support both documentation and clinical reasoning during the encounter.


## What to confirm about chart context


Glass can incorporate authorized chart context into supported workflows. During evaluation, confirm which patient context is in scope, how clinicians review generated output, what the pilot covers, and how the workflow behaves in the EHR environment your team actually uses.


That matters because a clinical AI workflow is much stronger when it starts from relevant patient context instead of a blank text box.


## What clinicians can do once the chart context is in Glass


Once authorized patient context is available in Glass, the workflow can support the same clinical jobs Glass Health supports across its product pages:


- ambient note drafting from the encounter conversation
- chart summarization
- differential diagnosis with next-step support
- assessment-and-plan drafting
- cited clinical Q&A grounded in medical literature and guidelines


This is the core product distinction. Glass is not just listening and transcribing. It is using chart context plus encounter context to support both documentation and clinical reasoning.


## Supported EHR workflows


Our current[EHR workflow](https://glass.health/for-clinicians#ehr-integration) covers:


- Epic
- eClinicalWorks
- athenahealth


Across our supported Max workflows, we also support Elation clinical workflows.


If your team uses another EHR, contact us to review fit, available chart context, and rollout scope for that environment.


## Security, HIPAA, and BAA review


Glass is designed to support HIPAA-compliant use of PHI in AI-powered clinical decision support. For Developer API deployments, teams should confirm the current BAA path before sending production PHI through the Developer API ([/api-documentation](https://glass.health/api-documentation) ). EHR-connected deployments should confirm the exact BAA path and implementation scope directly with Glass.


For EHR-connected deployments, the right diligence sequence is simple:


1. confirm the supported EHR setup and authorized data scope
2. confirm the BAA and PHI handling path
3. test the review workflow your clinicians will actually use
4. validate the exact pilot scope before broader rollout


The BAA is necessary, but it is not the whole implementation story. You still need the right workflow, the right chart context, and the right review path.


## How to scope a pilot well


The fastest way to evaluate Glass inside an EHR is to keep the pilot narrow and concrete.


Start with one clinician cohort and one real workflow. Define which outputs matter most. For some teams that will be ambient notes and chart summaries. For others it will be differential support, cited Q&A, or assessment-and-plan drafts. Then confirm the supported EHR prerequisites, the patient-context scope, and the review process clinicians will follow before the output is finalized.


If your organization needs a deeper product-embedding conversation, pair this page with[/for-developers#features](https://glass.health/for-developers#features) . If the main question is clinician workflow fit, pair this page with[/for-clinicians#ambient-scribing](https://glass.health/for-clinicians#ambient-scribing) and the EHR workflow pages linked above.


## FAQ


### How does Glass connect to an EHR?


Glass uses authorized chart context from supported EHR workflows so clinicians can use patient context for documentation plus CDS inside the Glass workflow.


### What chart data does Glass pull into the workflow?


Confirm the current chart-context scope directly with Glass during implementation. The practical goal is to bring the relevant patient context into the clinician’s documentation and CDS review workflow.


### Is this an API integration?


For Glass, the better description is clinical AI with EHR integrations. The point is not just that data can move between systems. The point is that authorized chart context helps clinicians use ambient scribing and CDS in one workflow.


### Does Glass require a BAA for PHI workflows?


Yes. For Developer API deployments, teams should confirm the current BAA path before sending production PHI through the Developer API. For EHR-connected deployments, confirm the exact BAA and implementation scope during contracting.


### Which EHR workflows does Glass support today?


Glass supports Epic, eClinicalWorks, athenahealth, and Elation workflows. Non-Epic workflows, including Elation, should be confirmed directly with Glass during setup.


### Can Glass work outside the EHR too?


Yes. Glass Health’s Ambient CDS page describes browser-based use as well as EHR-connected workflow support. That is useful for teams that want flexibility across clinics, remote work, or phased deployment.


### What should a team validate in a pilot?


Validate the supported EHR setup, the chart-context scope you need, the clinician review flow, the note and CDS outputs that matter most, and the BAA/security path for the exact deployment.


### What if my EHR is not listed on this page?


If your team uses another EHR, contact us to review fit and rollout scope directly. Glass supports Epic, eClinicalWorks, athenahealth, and Elation workflows today, and broader evaluation should start with your EHR, implementation needs, and review workflow.
