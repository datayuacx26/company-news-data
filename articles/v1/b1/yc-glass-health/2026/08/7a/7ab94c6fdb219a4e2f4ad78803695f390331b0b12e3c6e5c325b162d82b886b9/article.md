---
schema_version: "1.0.0"
document_id: "7ab94c6fdb219a4e2f4ad78803695f390331b0b12e3c6e5c325b162d82b886b9"
company_key: "yc-glass-health"
company: "Glass Health"
source_id: "yc-glass-health-news-import-5021dfc91ecf"
canonical_url: "https://glass.health/resources/ai-symptom-triage-api"
published_at: null
first_seen_at: "2026-08-09T22:41:21.143594+00:00"
fetched_at: "2026-08-09T22:41:22.527486+00:00"
content_hash: "sha256:01f98155d19db63f18d372d45a246bbdba5b815dc286c0df8da7b232a3f0993c"
---

# AI Symptom Triage API 2026 — Patient + Clinician Compared

AI symptom triage APIs split by end-user. Glass Health belongs on the clinician side of the category, where triage means chart-aware reasoning, differential diagnosis, workup support, planning, and documentation for nurses and physicians. Patient-facing teams usually review dedicated symptom-checker products such as Isabel Healthcare, Microsoft Azure Health Bot, or EndlessMedical. Those are different jobs, so this page organizes the market by who is actually using the system.


## What an AI symptom triage API actually does


Most buyers picture a chatbot asking about fever and cough. That is only one slice of the category. An AI symptom triage API is usually a workflow that starts with symptoms or risk factors, asks follow-up questions, estimates likely causes, suggests the next care step, then hands the case to a human or another system.


[Isabel Healthcare Symptom Checker](https://www.isabelhealthcare.com/products/symptom-checker) frames the same job around speed. The page says it lets “patients to get triage advice in under a minute asking just 11 questions,” with “7 standard questions about the onset, severity, and duration.” That is classic digital front door logic. The API is not trying to draft a full clinician plan. It is trying to keep the patient engaged long enough to get to a useful routing decision.


Some products widen the input before they route. In the public[Microsoft triage and symptom checking documentation](https://learn.microsoft.com/en-us/azure/health-bot/bot_docs/triage_symptom_checking) , the returned result object can include “User details,” “Symptoms,” “Risk factors,” “Questions and responses,” “Possible causes (and ICD10 codes),” “Triage results (and Telemedicine recommendation),” and “Messages (such as suggested care).” That is more than a symptom list. It is a handoff object that another system or human can use.


[EndlessMedical](https://endlessmedical.com/) goes broader still on input breadth. Its public page says it “processes 2000+ data points” and can process “review of systems,” “physical examination,” “history and allergies,” “imaging results,” “blood work,” “urine tests,” “genetic testing,” “biopsies results,” and the “patient’s medications.” That means some triage APIs are not just symptom-to-condition mappers. They are broader clinical-input engines that can sort by urgency or specialty using more context.


Clinician-facing triage looks different again. The[Glass Health Developer API](https://glass.health/for-developers#features) lists “triage” as a use case, but the surrounding features are built for clinicians: “Patient Data Summarization,” “Differential Diagnosis Generator,” “Treatment Plan Generator,” “Evidence-Based Q&A,” and “Documentation Generation.” In practice, that means Glass Health can support triage when the user is a nurse or physician who needs help thinking through the case, not only when a patient needs care navigation.


Condition ranking is where the category starts to split. Isabel Healthcare says it “accurately suggests the correct condition in its list of suggested conditions provided back to your clinicians in 96% of cases.” Microsoft Azure Health Bot returns “Possible causes (and ICD10 codes).” Glass Health supports AI-ranked differentials and diagnostic next steps for clinician-side workflows. Similar language, different jobs. A patient-facing API usually needs enough reasoning to guide routing. A clinician-facing API needs reasoning that can survive chart review, workup planning, and documentation.


Then comes disposition and handoff. A good patient triage API does not stop at likely causes. It moves the user somewhere useful: self-care, appointment scheduling, telemedicine, a call center, urgent evaluation, or the ER. Isabel Healthcare says “Up to 57% of patients completing Isabel Self-Triage select an appointment scheduling option.” Microsoft Azure Health Bot includes “Triage results (and Telemedicine recommendation)” plus “Messages (such as suggested care).” EndlessMedical says it “triages by acuity,” “triages by severity,” and “triages by specialty.”


Clinician-side handoff is different. Glass Health can “Generate concise, structured summaries of EHR data,” produce “AI-ranked differentials and diagnostic next steps,” “Craft guideline-aligned management plans tailored to patient context,” and “Create comprehensive clinical documentation—progress notes, discharge summaries, referrals, prior authorization letters, insurance appeals, and more.” So if you strip away the marketing terms, an AI symptom triage API does five things: it takes input, asks the next question, narrows the possibilities, routes the case, and hands the information off in a form another human or system can use. The right product depends on who that human is.


## Patient-facing vs clinician-facing triage APIs — these are different purchases


This is the part that saves buyers from wasting a quarter on the wrong pilot. “Triage API” sounds like one category. It is not. A patient-facing symptom checker API and a clinician-facing clinical triage API may share some words, but they solve different problems, land in different workflows, and get bought by different teams.


Start with who is at the keyboard. In patient-facing triage, the user is the patient, member, caregiver, or sometimes a non-clinical agent. The product has to keep that person moving through the interview without confusing or scaring them. That is why public patient-facing pages talk about “11 questions,” “under a minute,” “completion rate,” “digital symptom checker,” “chatbot,” “webpage,” “medical app,” “call center,” and “white labeled, virtual health assistant.” Those are consumer access and routing problems.


Clinician-facing triage is a different purchase because the user is trained. The nurse, NP, PA, or physician already knows how to question the case. What they need is context, synthesis, differential structure, next steps, and documentation support. The Glass Health Developer API supports patient record summarization, diagnostic support, treatment planning, evidence-based Q&A, and automated documentation around clinician-side triage. That is not a symptom checker homepage. It is a clinician workflow.


The second difference is output. Patient-facing products need to produce care navigation. Microsoft Azure Health Bot publishes a result object with “Triage results (and Telemedicine recommendation)” and “Messages (such as suggested care).” Isabel Healthcare ties self-triage to appointment scheduling behavior. Those outputs are meant to move a patient to the next step safely and clearly.


Clinician-facing outputs are heavier. Glass Health can return chart summaries, AI-ranked differentials, treatment plans, evidence-cited answers, and documentation. That is what a clinician needs when the case is no longer just a routing question. EndlessMedical sits in an in-between zone from the public material because its page says it is both “a medical API” and “a symptom checker,” and it emphasizes broad data processing plus triage by acuity, severity, and specialty. The cited public page does not fully pin down whether the primary user is a patient, agent, or clinician, which means you need to ask early in diligence.


The third difference is safety model. Patient-facing systems need careful wording, clean escalation paths, and a route to a human when the case is uncertain or high-risk. They also need to handle drop-off well because a half-finished triage interview is a broken handoff. Clinician-facing systems can assume human review, but they need to make their reasoning inspectable. For Glass Health, that means “markdown-formatted responses with in-text citations.” The clinical user can review and edit the output. That is a different safety posture from a patient seeing “suggested care” on a screen.


The fourth difference is the buyer. Patient-facing digital front door tools usually get evaluated by product, access, call-center, payer navigation, or conversational AI teams. They care about channel coverage, interview length, routing objects, multilingual support, and scheduling handoff. Clinician-facing tools usually get pulled by clinical operations, CMIO teams, urgent care leaders, nurse triage programs, tele-triage groups, or digital health teams building internal workflows. They care about chart context, reasoning quality, workup support, documentation, and whether the output can live inside a clinical workflow.


The fifth difference is deployment surface. Patient-facing docs talk about webpage, medical app, call center, browser, websites, conversational AI, non-clinical agent call centers, and white labeled virtual assistants. The Glass Health Developer API provides a RESTful interface with secure authentication and structured endpoints for clinical queries. Separately, EHR-connected Glass app workflows are assisted Max implementations, not part of Developer API product scope. For API use, the customer application sends the clinical data it wants Glass to process.


This also explains why Glass Health should be judged honestly. Glass Health is not a consumer or patient-facing symptom checker. Glass Health does not list a triage endpoint or a triage dispositions schema the way buyer teams often expect from a symptom checker API. If your RFP says, “patient enters symptoms and receives a patient-facing care route object,” Glass Health is the wrong first fit. If your RFP says, “clinician intake needs DDx, workup support, plan, evidence, and documentation,” Glass Health becomes very relevant.


A lot of buyer confusion comes from trying to compare everything on one axis. If you only ask which API does “triage,” the rows blur together. If you ask who the end user is, what output the workflow needs, where it gets deployed, and how a human receives the handoff, the shortlist becomes much cleaner. Read this guide that way, and the category makes sense. If you are exploring clinician-side workflows beyond triage, pair this page with Glass Health’s[Ambient CDS](https://glass.health/for-clinicians#ambient-scribing) overview and our broader[best healthcare AI API](https://glass.health/resources/best-healthcare-ai-api) guide.


## How to compare the 4 APIs


This comparison separates patient-facing symptom checkers from clinician-facing reasoning APIs. End-user fit comes first. A patient-facing symptom checker should be judged on plain-language intake, follow-up questions, likely causes, routing, and handoff. A clinician reasoning API should be judged on whether it helps a nurse, physician, or clinical team reason through the case and document next steps.


Clinical grounding is evaluated from each vendor’s public language. Isabel Healthcare is assessed from its machine-learning, completion, and condition-list claims. EndlessMedical is assessed from the breadth of data it says it can process. Microsoft Azure Health Bot is assessed from its built-in triage, result-object detail, language detail, and clinical-content governance. Glass is assessed from its evidence-based Q&A, chart summarization, AI-ranked differentials, treatment plans, citations, and medical-knowledge retrieval.


Deployment surface matters because many failed pilots are deployment mismatches. Isabel Healthcare names API, browser, websites, conversational AI, and non-clinical agent call centers. Microsoft Azure Health Bot is a cloud platform for white-labeled virtual health assistants. Glass exposes a RESTful API for clinical queries.


Region, language, safety, and governance details are included only when the vendor publishes them. If a table cell says a detail is not publicly stated, that is a documentation note, not a claim that the vendor cannot support the requirement.


## Quick comparison: 4 AI symptom triage APIs


The table is organized by user and job. A patient-facing symptom checker has to manage plain-language intake and care routing. A clinician-facing API has to help a clinical team reason through a case, plan next steps, and document the result.


Product Primary user Core workflow Deployment and evidence


Glass Health Clinicians and clinical operations teams Chart summarization, clinical Q&A, differential diagnosis, treatment planning, and documentation REST API with cited clinical answers; 50+ languages; teams can review and accept a click-through BAA in API settings before sending production PHI


Isabel Healthcare Symptom Checker Patients, with results available for clinician handoff Short symptom interview, likely-condition list, and care navigation API or browser deployment; vendor publishes completion, condition-list, channel, and language claims that buyers should validate in their own population


EndlessMedical Intended user is not clear in the public material Broad clinical input processing, acuity and specialty triage, and documentation Public page describes broad data inputs but leaves deployment, language, and privacy details for direct diligence


Microsoft Healthcare Agent Service Digital-health product teams and patients using a branded assistant Built-in symptom checking, triage, structured results, and extensible routing Azure platform with documented result objects, extension points, and multilingual content; NLP is documented as English-only


Three patterns stand out. First, Isabel Healthcare and Microsoft Azure Health Bot publish clear patient-facing routing language. Isabel Healthcare is explicit about short self-triage and scheduling behavior. Microsoft Azure Health Bot is explicit about white labeled virtual assistants and the structured result object that comes back from triage.


Second, EndlessMedical publishes the broadest input story on its homepage but the least public deployment detail. That can still be a fit, especially for technical teams that want more than a symptom interview, but it means the live demo matters more.


Third, Glass Health looks different because Glass Health belongs on the clinician side of the map. The Developer API lists triage as a use case, but the surrounding stack is chart summarization, differential diagnosis, treatment planning, evidence-backed Q&A, and documentation. If you compare Glass Health to patient symptom checkers as if every row solves the same job, the table will feel uneven. If you compare by user and job, the category becomes much easier to buy.


## Detailed per-vendor reviews


The table is short by design. Buying these tools is not. The notes below explain where each product fits, what the public materials make easy to evaluate, and which buyer archetypes should start with each option.


### Glass Health — Best fit for clinician-side triage reasoning


The[Glass Health Developer API](https://glass.health/for-developers#features) lists “triage, patient record summarization, diagnostic support, treatment planning, and automated documentation” as API use cases. That quote is the reason Glass Health belongs in this guide. It also tells you where Glass Health sits in the category. Glass Health is not a consumer symptom checker built to ask a patient a short sequence of questions on a website. Glass Health is relevant when triage happens on the clinician side, where the user is a nurse, physician, or clinical operations workflow.


Glass Health makes that distinction clearer in the product itself. Evidence-Based Q&A lets clinicians ask clinical questions and receive answers backed by guidelines, trials, and medical resources. Patient Data Summarization generates concise summaries of EHR data such as notes, labs, imaging, medications, and history. The Differential Diagnosis Generator expands diagnostic thinking with ranked differentials and diagnostic next steps. The Treatment Plan Generator supports guideline-aligned management plans tailored to patient context. Documentation Generation creates clinical documentation such as progress notes, discharge summaries, referrals, prior authorization letters, and insurance appeals. Put together, that is a clinician triage stack.


That matters because real clinician triage rarely stops at symptoms. In urgent care, tele-triage, nurse review, and hospital intake, the hard part is moving from the first complaint to an organized assessment, a safe workup, and a usable handoff. Glass Health can support that flow because it can summarize chart context, answer evidence questions, generate AI-ranked differentials with next steps, draft treatment plans, and produce documentation in the same pipeline. Patient-facing symptom checker APIs usually stop earlier because their job is care navigation. Glass Health can help later in the chain, where the clinical user still has to decide what to do next.


Glass Health’s clinical grounding is also different from most front-door symptom checkers. The developer page says the knowledge base includes “more than 38 million peer-reviewed articles and comprehensive FDA-approved drug information covering 154,000+ compounds.” It describes the system as a “large language model-based AI agent powered by sophisticated medical knowledge retrieval.” Output is “markdown-formatted responses with in-text citations.” That matters for a clinician-facing triage API because the output can be reviewed, challenged, and copied into downstream workflows instead of being treated as a black box.


From an engineering angle, the Developer API provides a RESTful interface with secure authentication, structured endpoints for clinical queries, and real-time progress updates during processing. Published reliability figures describe platform operations, not clinical accuracy. Clinical workflows still require validation and human review.


Procurement is where buyers need to stay disciplined. Glass Health lists Developer API pricing in its API documentation: the subscription starts at **$250/month** and usage above that floor is billed by token ([API documentation](https://glass.health/api-documentation) ). The same API settings let teams review and accept a **click-through BAA** before sending production PHI. That means buyers can evaluate both pricing and the BAA path directly from Glass documentation rather than guessing from the clinician subscription tiers.


Glass Health positions triage as part of a broader clinician workflow, not as a stand-alone patient symptom-checker widget. That matters if your requirement doc expects a patient to type symptoms, receive a route such as home care or urgent evaluation, and get patient-ready copy from a triage object. If that is your job, a patient-facing vendor such as Isabel Healthcare or Microsoft Azure Health Bot is the better category to start with.


If your workflow is clinician-facing, the picture changes fast. Glass Health lets API teams review and accept a click-through BAA before sending production PHI. Buyers should still confirm current terms, data retention, and controls for the exact implementation they are evaluating.


The buyer who should move Glass Health to the top of the shortlist is not launching an AI self triage widget on a homepage. It is the clinical operations team building clinician intake, nurse review, tele-triage, ED or urgent care workflows, or hospital command-center logic where the user is a clinician and the output needs to support reasoning and action. If that is you, start with the[Developer API](https://glass.health/for-developers#features) , then look at how Glass Health’s clinician workflow extends into[Ambient CDS](https://glass.health/for-clinicians#ambient-scribing) . For broader context, read this alongside our[best healthcare AI API](https://glass.health/resources/best-healthcare-ai-api) roundup and[HIPAA compliant AI API](https://glass.health/resources/hipaa-compliant-ai-api) checklist. If your brief says patient self-triage, buy another category. If it says clinician intake with DDx, workup, plan, evidence, and documentation, Glass Health is the right kind of API to evaluate.


### Isabel Healthcare Symptom Checker — Best fit for short self-triage flows


The public[Isabel Healthcare Symptom Checker page](https://www.isabelhealthcare.com/products/symptom-checker) is built around one sentence: “Self-Triage without endless questions.” That line tells you what Isabel Healthcare thinks the product wins on. The goal is not to run the longest branching interview. It is to keep the patient moving through a short, low-friction self-triage flow that still gathers enough signal to guide the next step.


The company says the product allows “patients to get triage advice in under a minute asking just 11 questions.” It also says the flow uses “7 standard questions about the onset, severity, and duration.” If you own a digital front door, those details matter more than they may seem. A symptom checker API does not only need medical logic. It needs a question design that patients will actually finish when they are sick, worried, or on a phone. Isabel Healthcare’s public positioning is clear that speed and completion are part of the product, not a side benefit.


The page is unusually explicit about engagement. Isabel Healthcare says it has a “97% completion rate minimizing drop off.” It also says, “User drop off rates for Isabel are less than 5% compared to 60-80%.” That is vendor-published language, not independent benchmarking, but it still tells you what the company believes it sells: not only triage, but a high-completion patient experience. The same page says, “Up to 57% of patients completing Isabel Self-Triage select an appointment scheduling option,” and “92% patients said they would use again.” If your business KPI is not just triage completion but downstream scheduling behavior, that matters.


Isabel Healthcare also makes an accuracy claim on the public page. It says the product “accurately suggests the correct condition in its list of suggested conditions provided back to your clinicians in 96% of cases.” Read that line carefully. It is a claim about whether the correct condition appears in the returned list, and it says the suggestions are “provided back to your clinicians.” That points to a workflow where patient self-triage feeds clinician review or another clinical handoff. It does not mean the top suggestion is always correct or that the API should stand in for medical judgment. Still, as public copy, it is more specific than many symptom checker pages.


Deployment is another strong point. Isabel Healthcare says a “comprehensive API or native browser version enables rapid deployment.” If you are looking specifically for an Isabel Healthcare API, that line is the public answer. The page also lists channels that include “websites, conversational AI, non-clinical agent call centers, global health information sites, find a doctor sites.” It says the product is “built to work with conversational virtual assistants” and “Can be deployed in any channel and workflow.” For buyers choosing between a website flow, a browser deployment, a call-center tool, or a conversational assistant, that breadth is useful.


Language coverage is also public and specific. Isabel Healthcare says the symptom checker is “available in English, Spanish, German, French, Dutch, Portuguese (Brasil), Russian, Romanian, Tagalog, Arabic, Chinese (Simplified), Malayalam, Urdu and Hindi.” It also says, “All ages covered from new-born to senior.” For multi-market consumer access teams, that specificity helps. You know what to test before a pilot starts.


Where Isabel Healthcare is the better choice is pretty clear. Buy it when you want a patient self-triage flow that is short, channel-flexible, and designed to move people into scheduling or another next step. If your team is trying to reduce abandonment on a consumer health site, if your “find a doctor” journey needs symptom capture before routing, or if you want a short flow that can sit inside conversational AI, Isabel Healthcare is a strong fit.


As with the other non-Glass Health vendors in this article, you still need live diligence for privacy and contracting. The public page cited here does not spell out a HIPAA or BAA path. The buyer persona is usually a consumer experience lead, appointment-access lead, or virtual-assistant team that values completion and conversion as much as clinical logic. If the real user is a clinician doing intake and differential reasoning, Glass Health is closer to that job. If the real user is the patient and the business problem is a short, high-completion self-triage flow, Isabel Healthcare belongs near the top of the shortlist.


### EndlessMedical — Best fit for broad clinical input capture


The public[EndlessMedical homepage](https://endlessmedical.com/) is thinner than the other pages in this guide, but it still says enough to justify a look. The company describes itself as “Artificial intelligence powered pre diagnosis, triage and more.” It also says it “is a medical API” and “is a symptom checker.” Those three lines put EndlessMedical in the symptom-triage conversation, while also hinting that the product is trying to do more than a simple front-door symptom form.


The clearest differentiator on the page is input breadth. EndlessMedical says it “processes 2000+ data points.” It then lists the kinds of information it can handle: it “processes review of systems,” “processes physical examination,” “processes history and allergies,” “processes imaging results,” “processes blood work,” “processes urine tests,” “processes genetic testing,” “processes biopsies results,” and “processes patient’s medications.” That is a much wider data surface than most people imagine when they hear symptom checker API. Even from a sparse public page, it is clear that EndlessMedical wants to be evaluated as a broad medical API, not only as a symptom box.


The public page also states that EndlessMedical “objectively evaluates the symptoms,” “triages by acuity,” “triages by severity,” and “triages by specialty.” It also says it “creates documentation.” That is an interesting mix because it spans both decision support and downstream workflow. A buyer who wants an engine that can ingest structured clinical data, sort by urgency or specialty, and return documentation output will at least want to see a demo.


Contracting is harder to evaluate from the public page because it does not spell out deployment channels, a language list, a HIPAA or BAA path, or the primary end user. That does not establish that EndlessMedical lacks these capabilities. It means buyers need to confirm them directly before relying on the product in a production workflow.


That ambiguity matters in category placement. EndlessMedical is easy to include in a symptom checker API search because the page literally says it “is a symptom checker.” It is harder to drop into a clean patient-facing or clinician-facing bucket because the same page leans into clinical data types like imaging, blood work, genetic testing, biopsies, and medications. If your intended workflow includes richer clinical data than a patient could provide in a homepage interview, EndlessMedical may be more relevant than it first appears. If your intended workflow is a pure consumer digital front door with a highly specified interview and routing object, Isabel Healthcare or Microsoft Azure Health Bot give clearer public evidence for that use case.


EndlessMedical moves up the shortlist when the project needs more than symptom-to-condition mapping and less than a full clinician reasoning workspace. It is most relevant for technical teams or innovation groups that want one medical API to process broad data inputs, triage by acuity or specialty, and create documentation. Buyers should ask early who the intended end user is, what the handoff object looks like, how the API is deployed, and what privacy terms are available. The buyer persona here is usually a technical evaluator comfortable doing more diligence because the “2000+ data points” promise is interesting enough to justify it.


### Microsoft Azure Health Bot / Healthcare Agent Service — Best fit for white labeled virtual health assistants


Microsoft’s public[Healthcare agent service page](https://learn.microsoft.com/en-us/azure/health-bot/) makes clear that this product is bigger than a symptom checker API. Microsoft says, “Healthcare agent service is a cloud platform that empowers developers in Healthcare organizations to build and deploy compliant, AI-powered virtual health assistants.” That matters because you are not only buying triage logic. You are also buying a platform for a branded assistant that can host triage, symptom checking, and related workflows.


The same landing page says, “The healthcare agent service enables you to build an intelligent, white labeled, virtual health assistant that can perform sophisticated tasks such as triaging, checking symptoms or answering questions about medical conditions and symptoms.” In buying terms, this puts Azure in a different spot from a pure symptom checker API. It is a fit for teams that want white-label control, enterprise cloud governance, and room to add steps around the triage itself.


The[triage and symptom checking documentation](https://learn.microsoft.com/en-us/azure/health-bot/bot_docs/triage_symptom_checking) explains how Microsoft does that. It says, “Built-in triage and symptom checking are packaged and included as part of the healthcare agent service solution so that you can easily incorporate them within your own healthcare agent service instance.” It also says teams can tailor the triage behavior through service configurations. For many buyers, Azure is a platform layer for a white labeled virtual assistant with Microsoft extension points. That is a different operating model from buying a standalone symptom checker.


One of the strongest parts of the public docs is the handoff object. Microsoft says the builtin triage scenarios return a result object that includes “User details,” “Symptoms,” “Risk factors,” “Questions and responses,” “Possible causes (and ICD10 codes),” “Triage results (and Telemedicine recommendation),” and “Messages (such as suggested care).” That level of public specificity is useful because triage only works in production if another system or human can receive and act on the output. Microsoft also says “built-in triage can easily be extended to add steps before or after the triage assessment,” which makes Azure a good fit for teams that want to wrap triage with their own intake, identity, or routing logic.


The clinical grounding language is also explicit enough to be useful. Microsoft says the service “uses multiple credible medical content providers that author and validate clinical content such as triage protocols and information about conditions or medications.” For buyers, that gives you a clearer mental model than many cloud pages do.


Language detail is another strength. The triage docs list content in English (en-US), Chinese (Simplified) (zh-Hans), French (fr-FR), German (de-DE), Italian (it-IT), Spanish (es-ES), Arabic (ar-SA), Portuguese (pt-PT), Portuguese – Brazilian (pt-BR), Russian (ru-RU), Dutch (nl-NL), Estonian (et-EE), Polish (pl-PL), Slovak (sk-SK), Turkish (tr-TR), and Ukrainian (uk-UA). There is an important nuance in the same doc: the NLP layer is available only in English. If multilingual support matters, that sentence should go straight into your test plan.


The regulatory note is worth reading exactly as written. Microsoft says, “The healthcare agent service isn’t a medical device and therefore, in the EU or UK its triaging functionality doesn’t provide any diagnostic functionality.” That statement is specific to this service and those jurisdictions, but it is useful because it reminds buyers that triage, diagnosis, and regulatory status are not the same thing. Your legal review has to match the deployment and the market.


Microsoft Azure Health Bot is the better choice when your buyer is an enterprise developer team or Microsoft-heavy organization that wants a white labeled virtual assistant with built-in triage and extension points. It is also a good fit when the structured result object and platform control matter more than buying a standalone engine. If you want clinician-side DDx, workup, and documentation, Glass Health is a different kind of tool. Buyers should still run live privacy and contracting diligence before sending production PHI through any triage workflow.


## When a BAA is not enough: safety review for triage APIs


A BAA answers one question: how protected health information is handled between parties. It does not answer the harder triage question, which is whether the system routes the right patient to the right level of care and whether the handoff survives the real workflow. Buyers mix those up all the time.


Start with severity. A symptom checker that ranks conditions reasonably well but under-calls red flags is still unsafe in practice. Test the edge cases that matter in your setting: chest pain, shortness of breath, sepsis-like symptoms, stroke symptoms, pregnancy complaints, pediatric fever, and immunocompromised patients. Microsoft Azure Health Bot returns “Triage results (and Telemedicine recommendation)” and “Messages (such as suggested care).” EndlessMedical says it triages by acuity, severity, and specialty. Those are the parts that need review, not just whether the demo feels polished.


Next is handoff integrity. If the output lands in scheduling, a call center, telemedicine, or a clinician queue, the receiving person needs the same facts the triage flow collected. Microsoft Azure Health Bot is strongest in public documentation here because it spells out a result object with user details, symptoms, risk factors, questions and responses, possible causes, and suggested care. Isabel Healthcare ties self-triage to appointment scheduling behavior, which means scheduling handoff is part of the product story. On the clinician side, Glass Health’s strength is different: the output can move from intake into evidence-cited answers, differentials, treatment plans, and documentation that a clinician can review.


Clinician review still matters. Even patient-facing tools need visible paths to a human when the case is high-risk, unclear, or outside the intended flow. Clinician-facing tools need active review because evidence citations and AI-ranked differentials help, but they do not remove judgment. Glass Health’s “markdown-formatted responses with in-text citations” are useful because they make the reasoning inspectable. That is part of a safety case, not the whole thing.


The practical safety review is simple to state and hard to skip. Ask who the end user is. Ask what exact triage object comes back. Ask how that output reaches a human. Ask what happens after hours or in high-risk cases. Ask what the public privacy language actually says. Buy the BAA, yes. Just do not mistake the BAA for the clinical safety review.


## Real triage API use cases


The easiest way to choose a triage API is to picture the workflow, not the vendor category page. The five common scenarios below show where each type of tool fits.


### Patient-facing digital front door


When the patient is on your website or app and the goal is care navigation before an appointment exists, start with a patient-facing symptom checker API. Isabel Healthcare is the better fit when completion rate, short questioning, and scheduling behavior are the business problem. Microsoft Azure Health Bot makes sense when the triage flow is one part of a broader white labeled virtual assistant.


Glass Health is not the right first pick here because Glass Health is not a patient-facing symptom checker and Glass Health does not list a triage-dispositions schema. EndlessMedical can stay on the longlist if you want to explore broader data capture, but the public deployment detail is thinner than the other front-door products.


### Payer member portal


Payer portals often need symptom capture plus member routing, telemedicine, or appointment guidance. Microsoft Azure Health Bot is attractive when your portal strategy already uses Microsoft tooling and you want a structured triage result object that includes telemedicine recommendation inside a branded assistant. Isabel Healthcare is worth a look when your next step is scheduling or provider selection, because the product page ties self-triage to appointment behavior.


Glass Health is less of a front-door fit in this scenario, but Glass Health can still matter downstream if escalated cases land with a clinician who needs chart summary, differential diagnosis, treatment planning, and documentation rather than only a routing suggestion.


### Nurse triage call center


In call centers, the first question is whether the interaction is patient-facing or clinician-facing. Isabel Healthcare names “non-clinical agent call centers,” so it belongs in a self-service or scripted interview flow. Microsoft Azure Health Bot works when the call-center experience is part of a white labeled virtual assistant and you want the same structured result object to flow into downstream systems.


If the person on the line is a licensed nurse or physician who needs reasoning support, Glass Health becomes much more interesting. The Glass Health API supports triage, patient record summarization, diagnostic support, treatment planning, and automated documentation, which is closer to clinician triage than to a patient symptom form. EndlessMedical is also worth a look if your call center has access to richer clinical inputs such as medications, imaging, or lab results.


### Clinician intake at ED or urgent care


In ED and urgent care intake, a patient-facing symptom checker often stops too early. The clinician needs a compressed chart, a working differential, a workup plan, and clean documentation. That is where Glass Health fits best in this list. The developer API brings together chart summarization, evidence-based Q&A, AI-ranked differentials with diagnostic next steps, treatment planning, and documentation. That is a practical clinician triage stack.


EndlessMedical also deserves attention here because its public page says it processes review of systems, physical examination, imaging, blood work, urine tests, genetic testing, biopsies results, medications, and triages by acuity, severity, and specialty. Isabel Healthcare and Microsoft Azure Health Bot can still play a front-door role before the clinician sees the case, but they are not the primary match for the clinician’s intake workspace.


### Hospital-at-home remote triage


Remote triage programs usually split into two layers. Layer one is patient or caregiver self-report from home. That is where Isabel Healthcare or Microsoft Azure Health Bot can make sense, depending on whether you want short self-triage or a broader virtual assistant. Layer two is command-center review by a nurse or physician. That is where Glass Health fits, because the same case can move from symptom report into clinician reasoning, treatment planning, and documentation.


EndlessMedical may also be relevant if the remote program feeds in labs, imaging, or broad clinical data, but the public page does not say enough about channels to place it more precisely. The buying mistake here is trying to force one patient-facing tool to do both layers when the work is really split across patient routing and clinician decision support.


## Decision guide — which API by buyer archetype


If you know your buyer archetype, the shortlist gets smaller fast.


### Consumer digital front door product lead


If your success metric is self-service completion on web or mobile, start with Isabel Healthcare. It is the cleaner fit when your team obsesses over short flows, low drop-off, and scheduling behavior. Use Microsoft Azure Health Bot instead when the symptom checker is part of a broader white labeled virtual assistant program.


### Payer navigation lead


For payer member portals, Microsoft Azure Health Bot is a strong starting point when the member experience already sits on Microsoft infrastructure and the workflow needs a structured triage result object plus telemedicine recommendation inside a branded assistant. Isabel Healthcare is worth a look when provider or appointment selection is the main next step.


### Call center operations lead


If callers interact with a scripted symptom interview before reaching a human, Isabel Healthcare has clear public call-center language around non-clinical agent call centers and a short self-triage flow. If your call center is staffed by licensed nurses or clinicians who need reasoning help rather than patient-facing interview logic, Glass Health is the better fit because Glass Health can support triage, chart summarization, differential diagnosis, treatment planning, and documentation in one workflow.


### Virtual assistant architect on Azure


If your organization already buys heavily from Microsoft and you want one cloud platform for a white labeled virtual health assistant, start with Microsoft Azure Health Bot. The built-in triage content, extension points before and after the triage assessment, and structured result object make it easier to build a fuller assistant rather than a single-purpose symptom checker. This is the clearest choice when engineering control and Microsoft governance matter more than buying a standalone triage engine.


### ED or urgent care clinical operations lead


If the real user is a clinician at intake, do not buy a patient symptom checker by mistake. Glass Health is the shortlist item here because the Developer API supports triage, patient record summarization, diagnostic support, treatment planning, and automated documentation. That is the workflow an ED, urgent care, or tele-triage team actually needs. EndlessMedical is the other interesting name if you want broad data ingestion across exam, labs, imaging, and medications, but you will need more diligence because the public page is sparse.


### Hospital-at-home or remote triage lead


Remote triage programs often need two tools, not one. For patient or caregiver self-report, use Isabel Healthcare or Microsoft Azure Health Bot depending on channel, language, and assistant requirements. For command-center clinician review, Glass Health is a better fit because the output is built around reasoning and action, not only routing. If you no longer need patient self-triage, you may actually be shopping for a clinician triage API.


## FAQ


### What is an AI symptom triage API?


An AI symptom triage API is software that takes symptoms and other patient data, asks follow-up questions, estimates likely causes, and routes the case to the next step. In patient-facing products, that next step is usually care navigation such as home care, appointment booking, telemedicine, or urgent escalation. In clinician-facing products, the next step may be a differential diagnosis, workup, treatment plan, or note. That is why Isabel Healthcare and Microsoft Azure Health Bot look different from Glass Health. They are all in the triage conversation, but they are not solving the same workflow.


### Patient-facing vs clinician-facing — how do I tell which I need?


If the patient or member is typing symptoms into a website, app, chatbot, or virtual assistant, you need a patient-facing product. Isabel Healthcare and Microsoft Azure Health Bot both publicly describe that kind of workflow. If a nurse or physician is the user and the output needs to support intake, differential diagnosis, workup, plan, and documentation, you need a clinician-facing API. Glass Health fits that second bucket. EndlessMedical sits between buckets in the cited public material because it calls itself a symptom checker and medical API and describes broad clinical data processing, but the public page does not clearly state the primary end user.


### How accurate are AI triage APIs?


Accuracy depends on the task you mean. Isabel Healthcare publicly says it suggests the correct condition in its returned list in 96% of cases. Glass Health provides evidence-backed Q&A and AI-ranked differentials, but Glass Health does not publish a triage-disposition accuracy metric. When you evaluate any clinical triage API, ask for task-specific numbers: correct condition in list, top-rank accuracy, route accuracy, high-acuity recall, and performance by age, language, and setting. A single headline percentage rarely tells you whether the API is safe for your workflow.


### Is a triage API a medical device?


It depends on the product, jurisdiction, and deployment. Microsoft’s public docs say the healthcare agent service “isn’t a medical device” and that, in the EU or UK, its triaging functionality “doesn’t provide any diagnostic functionality.” That statement is specific to that service and those markets. You should not assume the same answer for every symptom checker API or clinician triage API. The practical step is to treat medical-device status as a legal and regulatory workstream, not a marketing claim. Ask the vendor how it describes the product in your market, then review that answer with counsel and your compliance team before go-live.


### Does HIPAA apply to triage APIs?


If the API will receive, store, or transmit protected health information, HIPAA usually matters. A BAA addresses how data is handled between parties, but it does not tell you whether the triage logic is clinically safe. Glass Health supports BAA-backed workflows and notes HIPAA compliance documentation, and buyers should confirm current terms for their implementation. Public competitor pages in this article do not spell out a HIPAA or BAA path, so you should ask directly during diligence. In practice, buyers need both pieces: privacy terms and a separate safety review of routing logic, handoff, logging, and clinician oversight.


### Can I use Glass Health for triage?


Yes, for clinician-side triage. The[Developer API](https://glass.health/for-developers#features) lists “triage” as a use case alongside patient record summarization, diagnostic support, treatment planning, and automated documentation. That makes Glass Health relevant when a nurse, physician, or tele-triage clinician is the user. It is not the same thing as a consumer symptom checker. Glass Health is not a patient-facing self-triage product, and Glass Health does not list a triage endpoint or a triage-dispositions schema. If you need patient self-triage on a website or member portal, start with Isabel Healthcare or Microsoft Azure Health Bot instead.


### What languages do triage APIs support?


Public language detail varies a lot. Microsoft lists Azure triage content in English (en-US), Chinese (Simplified) (zh-Hans), French (fr-FR), German (de-DE), Italian (it-IT), Spanish (es-ES), Arabic (ar-SA), Portuguese (pt-PT), Portuguese – Brazilian (pt-BR), Russian (ru-RU), Dutch (nl-NL), Estonian (et-EE), Polish (pl-PL), Slovak (sk-SK), Turkish (tr-TR), and Ukrainian (uk-UA), with the NLP layer only in English. Isabel Healthcare lists English, Spanish, German, French, Dutch, Portuguese (Brasil), Russian, Romanian, Tagalog, Arabic, Chinese (Simplified), Malayalam, Urdu and Hindi. The cited Glass Health and EndlessMedical pages do not publish language lists.


### How do I handoff from triage to human?


Build the handoff as part of the triage purchase, not as an afterthought. The receiving human should get the symptoms, risk factors, follow-up answers, likely causes, and suggested route in a structured format. Microsoft’s public documentation is the clearest example here because it describes a result object with user details, symptoms, risk factors, questions and responses, possible causes, triage results, telemedicine recommendation, and suggested care messages. Isabel Healthcare ties self-triage to scheduling behavior. Glass Health works well when the human is already a clinician, because the output can move straight into differential diagnosis, treatment planning, and documentation. Whatever product you choose, test after-hours escalation, dead ends, and override paths.


## Bottom line


The shortlist gets simple once you stop treating every triage API as the same product. If a patient or member is doing self-triage on a website, app, chatbot, or virtual assistant, start with Isabel Healthcare or Microsoft Azure Health Bot. Add EndlessMedical when broad data ingestion and documentation are part of the brief, but plan for heavier diligence because the public page is thin. If the user is a nurse or physician and the workflow needs chart summary, DDx, workup, plan, and documentation, evaluate Glass Health first.


Before you buy, ask four things. Who is the end user? What exact output object comes back? How does the handoff reach a human? What public privacy terms can the vendor support? If clinician-side triage is your use case, start on the[developer API](https://glass.health/for-developers#features) page and the[Ambient CDS](https://glass.health/for-clinicians#ambient-scribing) page. For broader API market context, see our[best healthcare AI API](https://glass.health/resources/best-healthcare-ai-api) guide, our live[best AI medical scribe](https://glass.health/resources/best-ai-medical-scribe) resource, and the[HIPAA compliant AI API](https://glass.health/resources/hipaa-compliant-ai-api) checklist.
