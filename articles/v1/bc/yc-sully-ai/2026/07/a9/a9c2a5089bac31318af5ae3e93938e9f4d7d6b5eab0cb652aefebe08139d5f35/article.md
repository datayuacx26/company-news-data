---
schema_version: "1.0.0"
document_id: "a9c2a5089bac31318af5ae3e93938e9f4d7d6b5eab0cb652aefebe08139d5f35"
company_key: "yc-sully-ai"
company: "Sully.ai"
source_id: "yc-sully-ai-news-import-101ec319ffc2"
canonical_url: "https://www.sully.ai/blog/how-to-add-an-ai-scribe-to-altera-ehr"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-26T02:26:44.431773+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:435249eece770437d08fc35fe4fc2621e41c55cb35b58503d85c545bdee5efb3"
---

# How to Add an AI Scribe to Altera EHR in 2026

You run a health system on Altera, the enterprise EHR family from Altera Digital Health, formerly Allscripts, whether that is Sunrise in your hospitals, Paragon in a community site, or TouchWorks on the ambulatory side. Documentation burden is driving clinician burnout at scale. Physicians spend close to two hours on the EHR and deskwork for every hour of direct patient care \[1\], and family doctors log about 86 minutes of nightly "pajama time" finishing charts \[2\].


Here is the fact worth knowing up front: Altera now has a native ambient scribe, Sunrise Thread AI, so the real decision is no longer whether to add a scribe. It is whether to stop at an ambient note or also automate the work that happens after it. This guide covers how a scribe connects to Altera, a step by step rollout, and the options worth shortlisting.


## Key Takeaway


Altera now has a native ambient AI scribe, Sunrise Thread AI, that documents the visit inside the Sunrise workflow. That covers the note, but not the work after it. Enterprise ambient tools like Abridge, Nuance DAX Copilot, and Ambience also document the visit. To go further, you add a workforce platform:[Sully.ai](https://www.sully.ai/products) integrates with Altera and adds the[AI Coder](https://www.sully.ai/agents/medical-coder) that submits clean claims, plus reception and triage, across Altera and more than 20 other EHRs.


## What an AI Scribe Adds to the Altera Workflow


An[AI scribe](https://www.sully.ai/medical-scribe) listens to the visit and turns the conversation into a structured note. The clinician talks to the patient like normal. The scribe drafts the note in seconds, and the clinician reviews it instead of typing it.


Health systems on Altera feel the charting load across every service line, and it follows clinicians home. The numbers below are the burden a scribe is meant to remove.


The difference on Altera is that a native scribe already exists. So the decision shifts from "do we add a scribe" to "do we stop at the note, or automate the work that comes after it."


### Ambient Documentation Versus Manual Charting


The old flow is typing in Sunrise or TouchWorks during or after the visit. Either way the clinician is still doing data entry.


The ambient flow is listen, draft, review, sign. The impact is measurable: in a multi-site randomized study, ambient scribes cut documentation time meaningfully \[4\]. At enterprise scale, the question that decides real value is what happens to that note after it is written, because the note is only the first step in the administrative chain.


### Altera Has a Native Scribe, So the Question Is What Comes Next


Altera introduced Sunrise Thread AI, a native ambient scribe and note-generation assistant built directly into the Sunrise workflow. Clinicians review and edit in their native workflow, and it keeps the visit inside Sunrise \[3\].


It documents, and it does that well inside Sunrise. So the decision is not native versus nothing. It is whether documentation alone is enough, or whether you also want coding, claims, scheduling, and triage handled. That framing drives the rest of this guide.


## How AI Scribes Connect to Altera


There are three routes: the native Sunrise Thread AI, an enterprise third-party ambient scribe, or a workforce platform. The depth decides how much of the administrative load a tool actually removes.


The key distinction at enterprise scale: documenting the visit and automating the work around it are not the same thing. A tool can write an excellent note and still leave coding, claims, and scheduling to your staff.


Integration type


Documents the visit


Beyond the note


EHR reach


Connection


Native scribe (Sunrise Thread AI)


Yes, in-workflow


None


Altera only


Built in


Enterprise ambient scribe


Yes


Limited


Major EHRs, deepest in Epic


Embed or integration


Workforce platform


Yes


Claims, reception, triage


Altera plus 20 more


Deep integration


### The Native Option, Sunrise Thread AI


The zero-friction path for documentation. Sunrise Thread AI is built into Sunrise, needs no new login, keeps data in the facility-controlled environment, and lets clinicians review and edit in the native workflow \[3\].


This is a strong fit for systems that want an ambient note inside Sunrise and nothing more. The honest limit is scope: it documents, and it does not code the claim, schedule the follow-up, or run triage.


### Enterprise Ambient Scribes


Abridge, Nuance DAX Copilot, and Ambience are enterprise ambient documentation platforms, contract-priced and deepest in Epic \[5\]\[6\]\[7\]. They document well across large clinician populations.


The tradeoff is the same one the native scribe has. These are documentation tools, not a connected workforce, so they do not submit clean claims or staff the front desk.


### Workforce Platforms


The broadest tier. A workforce platform integrates with Altera and adds agents across the visit.


Sully.ai[integrates with Altera](https://www.sully.ai/integrations) once, and then its agents share that connection: the AI Scribe documents, the AI Coder submits the claim, the AI Receptionist handles scheduling, and the[AI Triage Nurse](https://www.sully.ai/agents/triage-nurse) runs intake. This is the option that goes past the note, and it also covers the EHRs your system runs beyond Sunrise.


## How to Add an AI Scribe to Altera Step by Step


Five steps take you from decision to a validated note in the chart.


### 1. Confirm Your Altera Footprint and Goals


Confirm which Altera products you run, Sunrise, Paragon, or TouchWorks, and identify the clinical-informatics owner. Decide what you want: an ambient note inside Sunrise, where the native Sunrise Thread AI is the quick path, or the work after the note, coding into claims, scheduling, and triage, which points to a workforce platform.


That goal determines the route.


### 2. Choose Your Integration Method


Map the methods to that goal. The native scribe is the fastest in-workflow note. An enterprise ambient scribe documents across major EHRs. A workforce platform handles claims, reception, and triage through one integration and covers the EHRs beyond Sunrise.


The decision rule: if you want an ambient note inside Sunrise, the native scribe gets you there. If you want the work after the visit handled, the claim submitted plus scheduling and triage, across every EHR you run, you need a workforce platform.


### 3. Connect the Scribe


For the native option, enable Sunrise Thread AI in the Sunrise workflow \[3\]. For an enterprise or workforce tool, have the vendor integrate with your Altera environment \[6\]\[7\].


Whichever route you pick, two things are non-negotiable before any patient audio is recorded: a signed BAA and HIPAA-compliant data handling, reviewed against your system-level data-governance policy.


### 4. Map Notes and Coding to Altera


Configure how each note section maps to the correct Sunrise or TouchWorks fields, and how codes attach to each problem instead of landing as one block. A native or ambient scribe focuses on the note; a workforce platform also[writes coded, claim-ready data](https://www.sully.ai/help-center/articles/how-does-sully-ai-s-two-way-integration-with-ehrs-work) .


Verify it against the templates and specialties you actually run, not a demo.


### 5. Pilot, Measure, and Scale


Pilot on a high-volume service line. The hardest, busiest setting is the one that reveals real value.


Confirm the note lands in the right Altera sections, measure the[time saved](https://www.sully.ai/help-center/articles/what-is-the-roi-of-an-ai-medical-scribe) , which is well documented for ambient scribes \[4\], and measure the work after the note: are claims cleaner, is scheduling handled. Validate governance before a system-wide rollout.


## What to Look For in an Altera AI Scribe


Four criteria separate the options on the shortlist.


### Native Workflow Versus Added Capability


The native Sunrise Thread AI wins on in-workflow simplicity for the note, since it lives inside Sunrise \[3\]. The question is whether you also need coding, claims, scheduling, and triage, which a native documentation scribe does not cover.


Match the tool to how much of the administrative load you actually want removed, not just to the note.


### Depth of Work After the Note


Does the tool turn the visit into linked ICD-10 and CPT codes and a clean claim, and handle scheduling and follow-up, or just document? Native and enterprise ambient scribes document. Workforce platforms go further.


This is where the real value sits for a system carrying its own billing operation. Sully pairs its[AI Scribe](https://www.sully.ai/agents/scribe) with an AI Coder that extracts every ICD-10 and CPT code and submits clean claims, plus front-desk and triage agents.


### EHR Breadth Across the System


Health systems rarely run one EHR. A native scribe covers only Altera, and an enterprise ambient tool is deepest in Epic. A workforce platform that integrates with Altera and more than 20 other EHRs covers the whole system through one approach \[6\]\[7\].


Weigh coverage across your real footprint, not just the EHR in front of you today.


### Security, Governance, and HIPAA Compliance


The non-negotiables at enterprise scale:[HIPAA compliance](https://www.sully.ai/help-center/articles/are-ai-scribes-hipaa-compliant) , a signed BAA,[encryption and de-identified PHI handling](https://www.sully.ai/trust) , MFA, SSO, role-based access, and clear data-governance and retention controls.


The native scribe emphasizes facility-controlled data \[3\]. Hold any third party to the same bar, and ask where audio is processed, how long it is retained, and who can access transcripts before the pilot starts.


## Best AI Scribes for Altera


Five options worth shortlisting. Altera has a native scribe, so it earns a place on the list, credited for what it does.


### 1. Sully.ai


Sully.ai starts as a strong ambient scribe and extends into a coordinated AI workforce that integrates with Altera through a single integration. The AI Scribe documents the visit, then the AI Coder extracts ICD-10 and CPT codes and submits clean claims, the AI Receptionist handles calls and scheduling, and the AI Triage Nurse runs intake and follow-up.


Three things set it apart on Altera. First, it goes past the note: the native scribe and the enterprise ambient tools document, while Sully also codes the claim and staffs the front desk and triage. Second, it is one integration and a connected workforce, not a stack of point tools bolted together. Third, breadth and proven scale: Sully integrates with Altera and also with Epic, Cerner, Meditech, Athenahealth, and more than 20 other EHRs, across[5,000+ providers](https://www.sully.ai/customer-stories/:WxcUfLIZH) and 50M+ hours of AI work delivered. Each AI role costs 80 to 90% less than the equivalent human role.


Best fit: health systems on Altera that want documentation plus the administrative work after the note, across every EHR they run.


### 2. Sunrise Thread AI (Altera native)


Altera's native ambient scribe and note-generation assistant, built directly into the Sunrise workflow with no new app or login \[3\].


Standout: zero-friction in-workflow documentation for Sunrise. Best fit: Sunrise systems that want an ambient note inside their existing workflow and do not need coding, claims, or front-desk automation. The honest limit is scope: it documents.


### 3. Abridge


An enterprise ambient documentation platform used widely across health systems, deepest in Epic and contract-priced per system \[5\]\[8\].


Standout: proven enterprise documentation at scale. Best fit: large systems wanting an ambient documentation deployment, after confirming Altera Sunrise fit.


### 4. Nuance DAX Copilot


Microsoft and Nuance's enterprise ambient documentation, embedded in major EHR mobile apps and typically contract-priced per provider \[6\].


Standout: established enterprise documentation with deep Microsoft backing. Best fit: systems standardized on Microsoft and Nuance tooling, after confirming Altera fit.


### 5. Ambience


An enterprise ambient documentation platform deployed in large hospital environments \[7\].


Standout: enterprise-grade documentation with broad specialty coverage. Best fit: large systems wanting an enterprise documentation rollout, after confirming Altera fit.


## Move From a Single Scribe to a Full AI Workforce


Altera now documents the visit natively with Sunrise Thread AI, and that is a real win on the note. But documentation is one slice of the administrative load.


Coding into a submitted claim, scheduling, intake, and follow-up all still happen, and for every physician seeing patients there are roughly ten administrative staff behind them. The average hospital already runs on about 800 different software tools, and point-solution AI just adds more silos instead of a workforce.


Sully takes the connected path. One integration with Altera, then a team of AI employees, the AI Scribe, the AI Coder, the AI Receptionist, and the AI Triage Nurse, sharing context across the visit. The Scribe writes the note, the AI Coder extracts every ICD-10 and CPT code, and a clean claim goes out before a denial can occur. Meanwhile the[AI Receptionist](https://www.sully.ai/agents/receptionist) books the follow-up before the provider finishes the next visit, across Altera and the other EHRs the system runs. Each AI role costs 80 to 90% less than the equivalent human role, proven across 5,000+ providers and 50M+ hours of AI work.


If you are weighing an AI scribe for Altera, it is worth seeing what the full team looks like in action.


## FAQ


**Q: What is an Altera AI scribe?**


An Altera AI scribe is an ambient documentation tool that listens to the patient visit and drafts a structured clinical note in an Altera EHR such as Sunrise or TouchWorks, so the clinician reviews and signs instead of typing. Altera now offers a native scribe, Sunrise Thread AI, and third-party platforms like Sully.ai integrate with Altera to add coding, claims, scheduling, and triage on top of documentation.


**Q: Does Altera have its own AI scribe?**


Yes. Altera introduced Sunrise Thread AI, a native ambient scribe and note-generation assistant built into the Sunrise workflow, generally available as of late 2025 \[3\]. It documents the visit in-workflow. It does not code claims, schedule, or triage, which is where a workforce platform adds value.


**Q: How do I add an AI scribe to Altera?**


Decide whether you want documentation only or the work after the note. For an in-workflow note, enable the native Sunrise Thread AI \[3\]. For coding, claims, scheduling, and triage, integrate a workforce platform such as Sully.ai, confirm your Altera footprint, connect it, map notes and coding to your Sunrise or TouchWorks fields, and pilot before scaling.


**Q: Should we use the native Sunrise scribe or a third-party platform?**


Use the native Sunrise Thread AI if you only need an ambient note inside Sunrise \[3\]. Choose a workforce platform if you want the administrative work after the note handled and coverage across the other EHRs your system runs. Sully.ai integrates with Altera and adds an AI Coder that submits clean claims, plus reception and triage.


**Q: Does an Altera AI scribe handle coding and claims?**


The native Sunrise Thread AI and the enterprise ambient scribes document the visit but do not submit claims. To turn the visit into a submitted clean claim plus scheduling and follow-up, you need a workforce platform. Sully.ai's AI Coder extracts every ICD-10 and CPT code and submits the claim through one integration with Altera.


## Sources


\[1\] **Annals of Internal Medicine (Sinsky et al.)** — Allocation of Physician Time in Ambulatory Practice: A Time and Motion Study in 4 Specialties. https://pmc.ncbi.nlm.nih.gov/articles/PMC5593724/


\[2\] **American Medical Association (AMA)** — Family doctors spend 86 minutes of "pajama time" with EHRs nightly. https://www.ama-assn.org/practice-management/digital-health/family-doctors-spend-86-minutes-pajama-time-ehrs-nightly


\[3\] **Altera Digital Health** — Altera Introduces Sunrise Thread AI, an Ambient Scribe and Note Generation Assistant. https://www.alterahealth.com/newsroom/altera-digital-health-introduces-sunrise-thread-ai/


\[4\] **JAMA Network (ambient AI scribe randomized study)** — Ambient AI Scribes in Clinical Practice. https://pmc.ncbi.nlm.nih.gov/articles/PMC12768499/


\[5\] **American Hospital Association (AHA)** — Health Systems Enhancing Care Delivery with Ambient AI Scribes. https://www.aha.org/aha-center-health-innovation-market-scan/2026-04-14-6-health-systems-enhancing-care-delivery-ambient-ai-scribes


\[6\] **Microsoft / Nuance** — Dragon Copilot clinical workflow. https://www.microsoft.com/en-us/health-solutions/clinical-workflow/dragon-copilot


\[7\] **Ambience Healthcare** — Ambient AI platform. https://www.ambiencehealthcare.com/


\[8\] **Abridge** — Generative AI for clinical documentation. https://www.abridge.com/
