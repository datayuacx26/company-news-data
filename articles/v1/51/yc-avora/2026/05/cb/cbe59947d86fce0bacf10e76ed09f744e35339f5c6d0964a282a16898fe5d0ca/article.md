---
schema_version: "1.0.0"
document_id: "cbe59947d86fce0bacf10e76ed09f744e35339f5c6d0964a282a16898fe5d0ca"
company_key: "yc-avora"
company: "Avora"
source_id: "yc-avora-rss-c62bcb629b1f"
canonical_url: "https://getavora.ai/blog/periodontal-chart"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:14.966450+00:00"
fetched_at: "2026-07-28T20:50:41.166574+00:00"
content_hash: "sha256:ee855da1070145b587006930e48b61381764f2a4056d1ba6dccedf88cd7c6446"
---

# Periodontal charting: codes, measurements, and the case for going hands-free

The perio chart is the densest document in a dental visit. Six probing depths per tooth across 28 teeth is **168 measurements before you count bleeding, recession, mobility, or furcation** — and every one of those numbers is both a clinical signal and a reimbursement signal. Miss them and an SRP claim gets downgraded to a prophy. Capture them inconsistently and a D4910 periodontal-maintenance claim looks indefensible to the payer who reads it.


This is a practical guide to what to capture, what gets denied, and why a[voice perio agent](https://getavora.ai/voice-perio-charting) changes the rhythm of the hygiene exam without forcing the hygienist to memorize a script.


## How to read a perio chart


A perio chart is a per-site snapshot of periodontal health, recorded tooth-by-tooth. The fields you'll see in any PMS perio screen:


- **Probing depth (PD).** Distance in millimeters from the gingival margin to the base of the sulcus, measured at **six sites per tooth** — DB, B, MB, ML, L, DL. Depths of **4mm and above** are the alarm line; 5–6mm is moderate periodontitis territory; 7mm+ is severe.
- **Bleeding on probing (BOP).** Marked site-by-site. The most reliable inflammation indicator — a 4mm pocket with BOP is a different finding from a 4mm pocket without it.
- **Recession.** Distance from the cemento-enamel junction to the gingival margin. Subtracted from PD gives **clinical attachment loss (CAL)** , the actual periodontal-bone-loss measurement payers want to see.
- **Mobility.** Scored 0 (none), 1, 2, or 3 — Class 3 is moving in all directions and signals serious bone loss or trauma.
- **Furcation involvement.** Class I (palpable groove), II (probe enters but doesn't pass through), III (probe passes through). Critical for prognosis on molars.
- **Mucogingival junction (MGJ), plaque, suppuration.** Smaller fields but they matter for D0180 completeness and SRP defense.


Color coding varies by PMS, but the structure of the data is the same everywhere. A complete chart is **non-negotiable for D0180** , and the foundation for every periodontal code below.


## The perio codes that drive what you get paid


Five codes carry most of the perio reimbursement weight, and four of them depend directly on the probing depths in this visit's note:


- **[D0180 — Comprehensive periodontal evaluation](https://getavora.ai/templates/d0180-comprehensive-periodontal-evaluation) .** Requires a full-mouth probing record by definition. Payers reject D0180 when the chart shows fewer than six sites per tooth or partial-mouth probing.
- **[D4341 — SRP, four or more teeth per quadrant](https://getavora.ai/templates/d4341-srp-four-plus) .** Requires documented probing depths (typically 4mm+ with bleeding, radiographic bone loss, or CAL) and a periodontal diagnosis. The note has to show medical necessity in this visit's chart, not last year's.
- **[D4342 — SRP, one to three teeth per quadrant](https://getavora.ai/templates/d4342-srp-one-to-three) .** Same documentation requirements as D4341. Quadrant must have fewer than four teeth meeting the periodontal-disease criteria — otherwise it's D4341 or it gets recoded.
- **[D4346 — Scaling in the presence of inflammation](https://getavora.ai/templates/d4346-scaling-inflammation) .** The boundary code between prophy and SRP. Generalized moderate or severe gingival inflammation **without bone loss or attachment loss** . Easily confused with D1110 and D4341; the chart-note has to be explicit about which applies and why.
- **[D4910 — Periodontal maintenance](https://getavora.ai/templates/d4910-periodontal-maintenance) .** Three things in the chart: disease history, prior active periodontal treatment, and **current** probing depths. Missing any one of those and the claim bounces.


If you're looking for the[full template library](https://getavora.ai/templates) , Avora ships 260+ CDT-organized templates with documentation guidance, including the boundaries between these five codes.


## What a full perio appointment actually looks like


A full-mouth chart is **168 probing sites** . Walk it tooth-by-tooth at one site per few seconds and you're calling out — and someone is recording — roughly one number every four seconds for ten minutes straight. Add bleeding flags, recession, mobility, and furcation and you're closer to three signals per site.


The traditional workflows for capturing that data are all bad in their own way:


- **Assistant types while hygienist calls out.** The assistant can't do anything else during the exam — no room turnover, no sterilization, no patient escort. Two people billing time for one task.
- **Hygienist self-charts.** Gloves off, type, gloves on. The exam stops, the patient watches the clock, and the rhythm of the appointment breaks.
- **Old-school voice perio (command syntax).** Strict prefixes — "tooth eight mesial three" — disrupt the way hygienists actually talk during an exam. Adoption stalls within weeks because the system fights the workflow.


Voice perio built for natural language is the version that holds up. The hygienist talks the way they would to an assistant; the chart fills in.


## Why the typical perio workflow is harder to defend than it looks


Audit defense isn't about whether the visit was clinically correct. It's about whether **the chart and the narrative agree** , in writing, by the end of the day.


The places that get teams in trouble:


- **Skipped sites.** When the rhythm breaks — gloves off, keyboard, gloves on — sites get filled in from memory or carried forward from the prior visit. That's the moment a chart becomes hard to defend.
- **Inconsistent capture between providers.** When three hygienists each chart the same patient differently, the periodontal diagnosis drifts, and SRP gets billed where prophy belongs (or vice versa). Standardizing perio across a[multi-location DSO](https://getavora.ai/ai-dental-notes-for-dsos) is one of the highest-leverage things a clinical director can do.
- **Chart-note narrative that doesn't match the chart.** D4341 with no probing depths in the note. D4910 with no disease history. D0180 with partial-mouth probing. These are routine reasons for denial — and they're all documentation problems, not clinical ones.


A voice perio agent that captures **both the chart and the narrative in the same pass** removes most of these failure modes by construction.


## How Avora connects voice perio to the rest of the visit


Avora's[voice perio agent](https://getavora.ai/voice-perio-charting) isn't a separate appliance — it's the perio-specific behavior of the same workflow that handles the rest of the visit's documentation.


- **Natural-language capture.** Hygienists call out the way they already talk: *"three two three, bleeding mesial, recession one, furcation class one buccal."* No prefix syntax to memorize.
- **In-place corrections.** *"Scratch that, tooth 14 mesial is four."* The agent re-reads, updates the chart, keeps going. No script restart.
- **Native PMS write-back.** Six-point depths, BOP, recession, mobility, and furcation land in the perio chart in **Dentrix, Open Dental, Eaglesoft, Denticon, and Dentrix Ascend** — not a pasted summary block.
- **Chart-note in the same pass.** The[chart-note narrative](https://getavora.ai/blog/ai-scribe-dental-clinical-notes) for the D0180, D4341, or D4910 visit is drafted from the same conversation that filled the perio chart. Diagnosis, depths, BOP map, and recommended treatment all agree by construction.
- **Timestamped citations.** Every line of the generated note links to the moment in the recording it came from. Audits and provider review take seconds.
- **Hygienist-first ergonomics.** Built around the way[hygiene actually documents](https://getavora.ai/ai-notes-for-dental-hygienists) — prophy, SRP, OHI, recall, and perio in one flow.
- **HIPAA, BAA, and encryption.** Avora signs a BAA, encrypts PHI in transit and at rest, and never trains models on recordings without opt-in — see the[HIPAA-compliant scribe](https://getavora.ai/hipaa-compliant-ai-dental-scribe) page for the full posture.


> The unlock isn't faster typing. It's removing the typing step from the perio exam entirely — and letting the same conversation produce both the chart and the defensible note.


## Frequently asked questions


### What is voice perio charting, and how is it different from older voice systems?


Voice perio charting captures probing depths, bleeding, recession, mobility, and furcation from natural speech and writes them into the PMS perio chart live. Older voice-perio systems require strict command syntax ("tooth eight mesial three") that disrupts the rhythm of the exam. Avora works with natural callouts like *"three two three, bleeding mesial, recession one"* and tracks tooth, surface, and corrections without making the hygienist memorize a script.


### Which perio codes need probing depths in the note?


D4341 and D4342 (SRP) both require documented probing depths and a periodontal diagnosis to defend medical necessity. D4910 (periodontal maintenance) needs current depths plus disease history and prior active treatment. D0180 (comprehensive periodontal evaluation) requires a full-mouth probing record by definition.


### Can a hygienist correct a measurement mid-exam without restarting?


Yes. Avora supports natural corrections — *"scratch that,"* re-reading a tooth, changing surfaces, or restating a measurement — without forcing the hygienist to stop the exam or repeat a script. The agent tracks where you are in the mouth and updates the chart in place.


### Will the perio chart write directly into Dentrix or Open Dental?


Yes. Avora writes back natively into Dentrix, Open Dental, Eaglesoft, Denticon, and Dentrix Ascend perio charts — full six-point depths, BOP, recession, mobility, and furcation, not a pasted summary. Other PMS coverage varies and should be verified before rollout.


### What measurements does Avora capture?


Six-point or four-point probing depths, bleeding on probing, suppuration, recession, clinical attachment loss, mobility, furcation, mucogingival junction, and plaque scores. That covers everything a defensible D0180, D4341, D4342, or D4910 note needs to carry.


### Is voice perio charting HIPAA-compliant?


Avora signs a BAA, encrypts PHI in transit and at rest, and never trains models on recordings without explicit opt-in. Voice perio captures patient identifiers, clinical findings, and audio, so any vendor's security posture should be reviewed in writing before rollout.


## Where to go next


- [Voice Perio Charting](https://getavora.ai/voice-perio-charting) — the pillar overview of Avora's perio agent.
- [AI Dental Scribe — feature overview](https://getavora.ai/ai-dental-scribe) — ambient capture, SOAP output, PMS write-back.
- [AI notes for dental hygienists](https://getavora.ai/ai-notes-for-dental-hygienists) — prophy, SRP, OHI, recall, and perio in one workflow.
- [AI dental notes for DSOs](https://getavora.ai/ai-dental-notes-for-dsos) — standardizing perio diagnosis across locations.
- [HIPAA-compliant AI dental scribe](https://getavora.ai/hipaa-compliant-ai-dental-scribe) — BAA, encryption, and retention detail.
- [AI scribe for dental clinical notes](https://getavora.ai/blog/ai-scribe-dental-clinical-notes) — the documentation side of the same workflow.
- [Best AI dental charting software (2026)](https://getavora.ai/blog/best-ai-dental-charting-software) — vendor-by-vendor reviews with PMS and HIPAA matrix.
- [Dental chart note templates](https://getavora.ai/templates) — 260+ CDT-organized templates, including the perio codes above.


Bring a tricky quadrant to a demo and we'll chart it live.[Book a demo →](https://getavora.ai/demo)
