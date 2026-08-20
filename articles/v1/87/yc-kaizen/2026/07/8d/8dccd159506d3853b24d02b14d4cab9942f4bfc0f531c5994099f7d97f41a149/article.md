---
schema_version: "1.0.0"
document_id: "8dccd159506d3853b24d02b14d4cab9942f4bfc0f531c5994099f7d97f41a149"
company_key: "yc-kaizen"
company: "Kaizen"
source_id: "yc-kaizen-news-import-1021911e948a"
canonical_url: "https://www.kaizenautomation.com/blog/psv-credentialing"
published_at: null
first_seen_at: "2026-07-24T01:13:36.362674+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:46918b602a5e72898f4909177fc3843a3eb618a3f866556a893de3d898911f44"
---

# PSV Credentialing: How It Works + 6 Bottlenecks to Avoid

## What is PSV credentialing?


PSV credentialing is the process of **verifying a healthcare provider's credentials** directly with the institution that issued them (state licensing boards, certifying bodies, medical schools, and employers) rather than relying on copies, third-party databases, or self-reported documents.


It's the verification standard **required by NCQA, The Joint Commission, CMS, and URAC** , and files fail audits without it.


## What PSV credentialing verifies


PSV credentialing covers the full credential stack for a healthcare provider, across three categories.


### Training and qualifications


- **Medical school diploma:** verified with the issuing university, or through ECFMG for internationally trained physicians
- **Residency and fellowship training:** verified directly with the training program or sponsoring hospital
- **Board certifications:** confirmed through the relevant specialty board or ABMS
- **State medical licenses:** verified with each state licensing board where the provider holds a license


### Compliance and sanctions


- **NPDB queries:** pulled to surface malpractice payments and disciplinary actions on file
- **OIG and SAM.gov exclusion checks:** run to confirm the provider isn't excluded from federal healthcare programs
- **DEA registration:** confirmed through the DEA's Registrant Validation Toolset, which now requires multi-factor authentication on every check
- **Malpractice insurance coverage:** confirmed directly with the insurer


### Behavioral health add-ons


- **BCBA and BCaBA certifications:** verified through BACB
- **State counseling and therapy licenses:** for LCSWs, LPCs, LMFTs, and LMHCs, verified with each state's licensing board
- **Psychology licenses:** verified through state psychology boards
- **NPI taxonomy codes:** confirmed against the provider's actual license type and specialty


## Who requires PSV credentialing?


PSV credentialing is required by the organizations that oversee hospital privileging, health plan credentialing, Medicare participation, and CVO accreditation.


- The Joint Commission requires primary source verification of licensure, relevant training, and current competence as part of the appointment and privileging process in hospitals.
- NCQA requires PSV in credentialing and also expects ongoing monitoring between recredentialing cycles. Its 2025 standards include monthly monitoring, and the verification window depends on the credential type.
- CMS enforces PSV through deemed-status accrediting bodies (NCQA, The Joint Commission). For hospitals, the Conditions of Participation require credentialing and privileging before privileges are granted, with direct-source verification of licensure, education, and board certification expected at survey.
- URAC includes PSV and credentialing time frames in its CVO standards. If a CVO wants URAC accreditation, direct-source verification is part of the process.


## How PSV credentialing works: Step by step


Each step has a defined scope, a responsible party, and a specific output. Miss any of them and the file stalls.


### 1. Receive self-reported credentials


Most providers self-report their credentials through **CAQH ProView:** degrees, training, licenses, certifications, and work history. PSV begins once the credentialing team has that file in hand and starts verifying each item independently against the original issuing source.


### 2. Identify what needs PSV


The scope depends on the **accreditation body and provider type** . The Joint Commission requires PSV for medical school diplomas, specialty training, and current licensure. **NCQA expands the scope** to include board certification, DEA registration, and sanctions history.


**Getting the scope wrong** is one of the most common audit findings we see. The file looks complete, but a required check was never run.


### 3. Verify each item with the source


The team checks **each credential at the source:** state licensing board portals, ABMS, NPDB, BACB, OIG, and SAM.gov. **Portal access** returns results in hours. **Fax or written correspondence can take weeks** , and this is where most manual workflows lose the most time.


### 4. Log the results and flag exceptions


Each verified credential gets logged with its **source, date, and status** . Anything that comes back **expired, sanctioned, or inconsistent** with the self-reported file gets flagged for review before it moves forward.


CAQH's PSV product, for example, **categorizes each verified data element in a file:** A (valid and current), B (potentially irregular), and C (very irregular). Files containing B or C elements get routed immediately to the review team. Other CVOs use **their own classification** , but the principle is the same: **clean files keep moving** , exceptions get pulled out.


### 5. Send the file for committee review


The file moves to the committee only **after every PSV is complete** . One missing check holds the entire file, and committees typically meet **on a fixed cadence** , so a **single delayed verification** can push a provider's start date back by weeks.


### 6. Run continuous monitoring after approval


[NCQA's July 2025 standards](https://wpcdn.ncqa.org/www-prod/2025-HPA-Policy-Updates_7-28-25.pdf) require **monthly tracking of license expiration** and **every-30-day checks** against OIG, SAM.gov, and Medicare/Medicaid exclusion databases for every credentialed provider in the network.


NPDB monitoring runs separately through **Continuous Query enrollment** , which sends alerts within 24 hours of any new report. **Source-verification work** that used to run once per credentialing cycle now runs continuously across the full provider roster.


## The biggest PSV bottlenecks


These are the bottlenecks that show up in nearly every **manual PSV workflow** , and the ones most likely to add weeks to a file that should have closed already.


### Closed institutions


Medical schools that have shut down, residency programs that no longer exist, and dissolved professional associations create **verification dead ends** . The team has to **locate successor institutions, archived records, or state-level documentation.** A single closed institution can add weeks to an otherwise clean file.


### International credentials


For internationally trained physicians, domestic PSV channels don't apply. **ECFMG handles verification of foreign medical credentials** for U.S. purposes, but the timeline is longer and the documentation requirements are more complex than for domestic graduates.


Files involving ECFMG verification routinely **run past the 90-day mark** , even when everything else moves cleanly.


### Slow-responding state boards


State licensing board portals vary significantly. Some provide **real-time online lookup** , while others still require **written requests, faxed forms, or mailed verifications** that take **weeks to process** . Manual teams lose more time waiting on slow boards than on any other PSV step.


### Secondary sources used in place of PSV


Checking a provider's license on a **third-party data aggregator** is not PSV, even if the aggregator looks authoritative. The verification must come **from the issuing body itself** . Organizations using aggregator lookups as a substitute are not meeting NCQA or Joint Commission standards, and won't find out until an audit.


### CAQH attestation lapses


Providers must re-attest their CAQH profile **every 120 days** . When a profile lapses, it blocks PSV workflows for every health plan connected to that provider. For a 50-provider group, one lapsed profile can **stall credentialing across an entire payer panel** , and the lapse is rarely caught until a payer flags the file as incomplete.


### Monthly monitoring at scale


Under the old biannual model, manual checks were manageable. At a monthly frequency, a 50-provider group runs **2,400 monitoring events per year** across license expiration, OIG, SAM.gov, and Medicare/Medicaid checks, before NPDB or state sanction databases get added in.


Manual workflows can't sustain that without **errors or staff burnout** .


## Manual vs. automated PSV credentialing


Manual PSV credentialing on a complex file runs **90-120 days** . For files involving international credentials, multiple state licenses, or behavioral health certifications, that timeline is the starting point. Some files run longer.


[CAQH PSV](https://www.caqh.org/solutions/psv-faqs-health-plans) returns **98% of initial files within 8-14 days at 98.5% accuracy** . The gap shows up across the full workflow: turnaround, error rates, audit trails, scalability.


Manual PSV Automated PSV


Turnaround 90-120 days for complex files 8-14 days with CAQH PSV


Error rate High data entry, missed expirations Low — direct source connection


Monthly monitoring Not sustainable at scale Built-in, continuous


Audit trail Inconsistent, file-dependent Timestamped, automated


NCQA 2025 compliance Difficult to maintain manually Designed for it


Staff burden High, prone to burnout Redirected to exceptions only


## How Kaizen automates portal-based PSV


Most credentialing teams run those checks manually, one provider at a time, with **a coordinator spending 3 to 5 hours** per provider on portal navigation. We've automated the portal-based steps of that workflow **without requiring a payer API** or a custom engineering build.


Your ops team describes the **workflow in plain English** . We turn it into a deterministic automation that executes the same way every time, no code required.


**In the PSV credentialing workflow, we handle:**


- **State licensing board checks:** Verifying license status directly on state board portals across multiple states simultaneously, replacing the manual tab-by-tab process that slows most credentialing teams.
- **CAQH profile management:** Monitoring re-attestation cycles and submitting updates before profiles lapse, preventing the blockages that stall PSV for entire provider rosters.
- **OIG and SAM.gov exclusion scans:** Running monthly checks across both databases for your full provider list, meeting the 2025 NCQA monitoring requirement without manual effort.
- **NPDB queries:** Accessing the NPDB portal to pull malpractice and disciplinary history as part of the initial credentialing workflow.
- **Ongoing monitoring cycles:** Running automated status checks monthly across all required PSV sources and surfacing alerts in Slack or Google Sheets when a license status changes.


Like all portal automation, Kaizen can hit edge cases when portals glitch or required fields are missing. The self-healing agents handle most of these, but **your ops team handles exceptions** .


Kaizen is built for **HIPAA-compliant workflows** and is **SOC 2 certified** , which matters when running PSV checks on sensitive provider data across multiple portals.


**The credentialing committee review, payer contract strategy, and escalations** still belong to your ops team. What Kaizen removes is the portal execution work, so a small team can run monthly PSV monitoring across **100+ providers without adding headcount** .


> Run PSV credentialing without the portal work — keeping up with PSV credentialing across 50, 100, or 200 providers is where manual workflows break down. Kaizen helps make that work manageable so your team can spend less time chasing status updates and more time moving files forward.[Book a call](https://calendly.com/kaizenautomation/sales-call) to see how Kaizen works against your current provider volume.


## Frequently asked questions


### What is the difference between PSV and secondary source verification?


The main difference between PSV and secondary source verification is that **PSV contacts the original issuing body** directly like the state board, medical school, or certifying organization. Secondary source verification uses **copies, third-party databases, or self-reported documents** . NCQA and The Joint Commission require primary source verification.


### How long does PSV credentialing take?


Manual PSV credentialing takes **90-120 days for complex files** , especially those involving multiple state licenses, behavioral health certifications, or international credentials. Automated PSV through systems like CAQH returns **98% of initial files within 8-14 days** .


### What credentials require primary source verification?


PSV is required for medical licenses, board certifications, medical education, residency training, DEA registration, NPDB malpractice history, and OIG and SAM.gov exclusion status. **For behavioral health providers** , PSV also covers BCBA and BCaBA certifications through BACB, state counseling and therapy licenses, and psychology licenses.


### Is monthly PSV monitoring required?


Yes, monthly PSV monitoring is required for **NCQA-accredited and certified organizations** . Under NCQA's updated credentialing standards effective July 1, 2025, **monthly monitoring of license status, OIG exclusions, SAM.gov exclusions, and sanctions is mandatory** . Annual or biannual checks no longer meet NCQA compliance.


### Can PSV credentialing be automated?


Yes, PSV credentialing can be automated. **Kaizen automates the portal-based steps** like state licensing board checks, CAQH profile management, OIG and SAM.gov exclusion scans, NPDB queries, and monthly monitoring cycles.
