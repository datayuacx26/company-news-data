---
schema_version: "1.0.0"
document_id: "9efb573d5ba7ff5bf65480effb9e71c4076bc9385a042d7b39a638eaf55d9058"
company_key: "yc-medcrypt"
company: "MedCrypt"
source_id: "yc-medcrypt-news-import-56918f8bbce9"
canonical_url: "https://www.medcrypt.com/blog/why-penetration-testing-isnt-enough-fda-cybersecurity"
published_at: "2025-09-29T00:00:00+00:00"
first_seen_at: "2026-07-22T03:50:59.950031+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:c8a47f5fd20da13322ac6752d927e703695c82d42d40164f2a3cce54130641a3"
---

# Why Pen Testing Isn’t Enough (Especially If You Want a Smooth FDA Review)

Penetration (pen) testing is an effective (and expected) security practice and serves a specific purpose, but is not a one stop shop for Cybersecurity Testing. By the time a pen test happens, architectural decisions are locked in, coding and integration are complete yet timelines are tight and you’re hoping there aren’t any surprises. Regulators don’t just want to see that you hired a skilled hacker for two weeks - they want evidence that security has been engineered throughout the lifecycle and that you can maintain that posture in the field. That’s the gap: pen testing is a snapshot; regulators expect a movie.


Below is a practical playbook for going beyond pen testing - what to add, when to add it, and how it maps to what FDA reviewers look for.


## **What regulators actually want to see**


FDA’s current guidance expects a **Secure Product Development Framework (SPDF)** with traceable security risk management, a **maintainable update process** , and an **SBOM with vulnerability monitoring** . Section 524B of the FD&C Act (PATCH Act) makes those expectations explicit for “cyber devices.” Pen testing can contribute objective evidence, but it cannot stand in for SPDF, SBOM discipline, or postmarket vulnerability handling.[U.S. Food and Drug Administration+2U.S. Food and Drug Administration+2](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/cybersecurity-medical-devices-quality-system-considerations-and-content-premarket-submissions)


**Bottom line:** A clean pen test report is helpful, but it won’t satisfy expectations without lifecycle artifacts (threat models, security requirements, architecture and control rationale, verification evidence during the development process, update/patch plans, and vulnerability management procedures).[U.S. Food and Drug Administration](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/cybersecurity-medical-devices-quality-system-considerations-and-content-premarket-submissions)


## **Why pen testing alone falls short**


1. **Timing & leverage:** Pen tests happen late; fixing architectural and coding issues then is costly (or impossible).


2. **Volume of Findings:** If preceding verification testing and code review activities did not happen or were insufficient, pen tests will produce a large number of findings that may be difficult to attribute and remediate.


3. **Coverage:** Pen tests probe what’s exposed; but scope limitations and missing technical documentation may lead to latent weaknesses in components not surfaced at an interface.


4. **Reproducibility & traceability:** Poorly scoped pen tests can result in findings which do not trace cleanly to requirements and risks unless you’ve established a traceability matrix linking each risk to a requirement, the control that addresses it, and the test evidence that verifies it.


5. **Sustainability:** Regulators also care about how you’ll handle vulnerabilities *after* launch—pen testing is a point-in-time activity but is expected to be repeated periodically after product market release.[U.S. Food and Drug Administration](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/postmarket-management-cybersecurity-medical-devices)


## **The full toolkit: security testing types (and where they shine)**


Here’s a concise overview you can drop into internal FAQs or reviewer-facing documentation. (This expands on the summary you provided.)


Relying on pen testing as the predominant method of identifying vulnerabilities will result in incomplete findings, results that are difficult to attribute to the specific defective code, and a high volume of findings that will be costly to remediate at this late stage.Other testing activities that are more appropriate during early development and integration stages should shoulder the heavy lifting at the appropriate time when code defects can be easily identified and remediated. Pen testing should only be the final confirmation and be considered a second (independent) opinion that would produce only limited findings.


Other testing methods that should be applied and will provide beneficial results at the respective development and integration stages:


- **Code Review:** A peer-level cross-review of software code to help identify potential violations or coding errors. Very effective in the early stages of development as it provides another set of eyes.


- **Static Analysis (SAST):** Analyzes source code for insecure patterns and coding-rule violations during build. Great at catching classes of defects early (e.g., hard-coded passwords); won’t be able to analyze binaries of third-party code.


- **Dynamic Analysis (DAST):** Exercises the running application and flags unexpected behavior. Good at finding runtime misconfigurations and logic issues; may not pinpoint the exact code location.


- **Automated interface testing (incl. fuzzing, boundary testing):** Interrogates interfaces with malformed/out-of-range inputs to trigger undefined behavior such as crashes or memory leaks.


- **Vulnerability scanning:** Interrogates exposed services and interfaces to identify known-vulnerable components. Useful for OS/middleware; limited to what the device reveals at the perimeter. Some scanners may deploy agents that provide deeper analysis.


- **SBOM analysis:** Matches *all* declared components (including embedded libs) to known CVEs - goes deeper than surface scanning; requires accurate identification & version mapping.


- **Penetration testing:** Independent experts emulate adversaries to chain weaknesses into plausible exploit paths. Best as a capstone to validate assumptions and controls - and to test “defense in depth.”


**Key idea:** Use each method where it has the most leverage (early vs. late), and tie results to risk controls, requirements, and verification evidence. That’s how you turn “tests we ran” into “assurance we can prove.”


## **Map testing to lifecycle (shift left, then close the loop)**


- **Concept & Architecture:** Threat modeling; define security requirements; choose secure crypto & identity patterns; partitioning; update/rollback strategy. (Standards like IEC 81001-5-1 help you systematize this).[ISO](https://www.iso.org/standard/76097.html?utm_source=chatgpt.com)[Regulatory knowledge for medical devices](https://blog.johner-institute.com/iec-62304-medical-software/iec-81001-5-1/)[Intertek](https://www.intertek.com/blog/2025/03-11-iec-81001-5-1/?utm_source=chatgpt.com)
- **Implementation:** SAST, secrets scanning, dependency policy; unit tests for security-critical code; reproducible builds.


- **Integration:** Fuzzing of interfaces/protocols; DAST on representative deployments; hardening baselines; logging/telemetry validation.


- **Verification & Submission:** Pen test as a **capstone** ; SBOM completeness check; vulnerability scan of the built image; traceability from risks → controls → tests → results → residual risk. Consider UL 2900 if you want a recognized, testable evaluation path.[Shop UL Standards](https://www.shopulstandards.com/ProductDetail.aspx?productId=UL2900-1_2_S_20231213&utm_source=chatgpt.com)[UL Solutions](https://www.ul.com/insights/medical-device-cybersecurity-standards-and-services)
- **Launch & Postmarket:** Vulnerability monitoring against your SBOM, intake/triage process, coordinated disclosure, and patch/update mechanics—explicitly required under FDA’s postmarket guidance and 524B.[U.S. Food and Drug Administration+1](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/postmarket-management-cybersecurity-medical-devices)


## **“Regulator-ready” checklist (use this to frame your submission)**


1. **SPDF evidence:** Process docs showing how security is baked in (not stapled on).[U.S. Food and Drug Administration](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/cybersecurity-medical-devices-quality-system-considerations-and-content-premarket-submissions)
2. ‍ **Threat model & security requirements:** Linked to risks and, as needed, to hazards, harms, and clinical context.
3. ‍ **Control rationale:** Why your chosen controls reduce risk (defense in depth, least privilege, crypto, secure boot, update/rollback, logging).


4. **Traceability matrix:** Verification and validation testing (SAST/DAST/fuzz/scan/SBOM analysis/pen test) mapped to identified risks and respective controls; include objective evidence and residual-risk disposition.


5. **SBOM + policy:** Complete, normalized SBOM; process for monitoring CVEs and evaluating exploitability in context.[U.S. Food and Drug Administration](https://www.fda.gov/medical-devices/digital-health-center-excellence/cybersecurity-medical-devices-frequently-asked-questions-faqs)
6. **Update plan:** How you deliver timely, safe patches (incl. authentication, rollback, clinical considerations).[U.S. Food and Drug Administration](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/postmarket-management-cybersecurity-medical-devices)
7. **Vulnerability handling & disclosure:** Intake → triage → remediation → communication; metrics to demonstrate responsiveness.[U.S. Food and Drug Administration](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/postmarket-management-cybersecurity-medical-devices)


## **How to use pen testing the *right* way**


- **Repeat at meaningful changes: Major architecture, networking, crypto, or update-system changes warrant re-testing.** ‍


- **The finishing touch:** Pen testing should only be undertaken when all other defined tests have been conducted at the appropriate development stage and all findings have been remediated and dispositioned.


- **Scope to risks:** Derive test objectives from your threat model and architecture—not a generic “scan everything.”


- **Independence matters:** Use a tester with no commercial interest in the product.


- ‍ **Chain-of-attack focus:** Ask for exploit paths (how issues combine), not just a list of CVEs.


- ‍ **Evidence of exploitation: Document** evidence (e.g., screenshots) of successful attack path in the penetration test report to allow for internal verification of weakness.


- ‍ **Tie findings to evidence:** Every finding should be documented as risk (or mapped to already identified risks) and either be mitigated or documented as residual risk.


## **Put it together: efficient, regulator-friendly assurance**


Think of pen testing as the **final exam** , not the course. If you’ve built the syllabus (SPDF), studied consistently (SAST/DAST/fuzz/scan/SBOM analysis), and kept your notes organized (traceability), the exam confirms what you already know—and your FDA review goes faster because your story is coherent, evidence-backed, and maintainable post-launch.[U.S. Food and Drug Administration+1](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/cybersecurity-medical-devices-quality-system-considerations-and-content-premarket-submissions)


### **References you can cite in submissions**


- FDA, *Cybersecurity in Medical Devices: Quality System Considerations and Content of Premarket Submissions* (updated June 27, 2025; supersedes Sept 27, 2023).[U.S. Food and Drug Administration+1](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/cybersecurity-medical-devices-quality-system-considerations-and-content-premarket-submissions?utm_source=chatgpt.com)[Federal Register](https://www.federalregister.gov/documents/2023/09/27/2023-20955/cybersecurity-in-medical-devices-quality-system-considerations-and-content-of-premarket-submissions)[‍](https://www.federalregister.gov/documents/2023/09/27/2023-20955/cybersecurity-in-medical-devices-quality-system-considerations-and-content-of-premarket-submissions?utm_source=chatgpt.com)


- FDA, *Cybersecurity in Medical Devices – FAQs* (Section 524B timing & expectations).[U.S. Food and Drug Administration](https://www.fda.gov/medical-devices/digital-health-center-excellence/cybersecurity-medical-devices-frequently-asked-questions-faqs)[‍](https://www.fda.gov/medical-devices/digital-health-center-excellence/cybersecurity-medical-devices-frequently-asked-questions-faqs?utm_source=chatgpt.com)


- FDA, *Postmarket Management of Cybersecurity in Medical Devices* (sustaining activities).[U.S. Food and Drug Administration](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/postmarket-management-cybersecurity-medical-devices)[‍](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/postmarket-management-cybersecurity-medical-devices?utm_source=chatgpt.com)


- IEC 81001-5-1:2021 - *Health software and health IT systems safety, effectiveness and security - Part 5-1: Security - Activities in the product lifecycle* .[International Electrotechnical Commission](https://webstore.iec.ch/en/publication/63293) ‍


- UL 2900-2-1:2017 (R2022) - *Software Cybersecurity for Network-Connectable Products, Part 2-1: Particular Requirements for Network Connectable Components of Healthcare and Wellness Systems* .[UL Standards & Engagement](https://www.shopulstandards.com/ProductDetail.aspx?productId=UL2900-2-1_1_S_20170901&)


‍
