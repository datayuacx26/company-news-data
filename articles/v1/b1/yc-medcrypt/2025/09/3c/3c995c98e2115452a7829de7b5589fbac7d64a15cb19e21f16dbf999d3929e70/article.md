---
schema_version: "1.0.0"
document_id: "3c995c98e2115452a7829de7b5589fbac7d64a15cb19e21f16dbf999d3929e70"
company_key: "yc-medcrypt"
company: "MedCrypt"
source_id: "yc-medcrypt-news-import-56918f8bbce9"
canonical_url: "https://www.medcrypt.com/blog/legacy-medical-device-cybersecurity-fda-guidance"
published_at: "2025-09-29T00:00:00+00:00"
first_seen_at: "2026-07-22T03:50:59.950031+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:70a9b0fadbb700b89beba655b4d750fd52f568167b330e16a1cf08390fd47601"
---

# ICYMI: The Legacy Dilemma - Security, Responsibility & Regulation for Connected Medical Devices

**Couldn’t join us live?** We unpacked how regulators and standards bodies define “legacy” devices, how this may differ from healthcare providers’ view, what the FDA’s latest guidance means for end-of-life (EOL) and end-of-support (EOS) planning, how risk responsibility shifts from manufacturers to operators (such as healthcare providers), and the practical steps healthcare organizations can take today to reduce future legacy burden. Along the way, we drew lessons from high-profile incidents like WannaCry and explored the future of post-quantum cryptography.


Here’s what you missed.


## **FDA’s Updated Stance: A New Normal for Cyber**


The FDA is raising the bar on cybersecurity expectations. Under the FD&C Act, Section 524B, manufacturers must now provide a reasonable assurance that devices and related systems remain cybersecure - which includes documenting support windows and end-of-support dates for all software components.


This makes EOL/EOS planning not just a best practice, but a regulatory obligation. For device makers, it means proactively communicating how long software and third-party components will be supported. For providers, it means adjusting procurement and replacement planning to account for security risks across the full lifecycle.


## **What “Legacy” Really Means**


The term “legacy” is often thrown around loosely, but different stakeholders define it in very different ways:


- **IMDRF:** A legacy device is one that *cannot be reasonably protected* against current threats.
- **IEC 62304:** A legacy product is software developed before the standard came into effect.
- **ISO/IEC 81001-5-1:** Does not use “legacy, " instead refers to “transitional” health software, created under older assumptions of risk.


The nuance matters. Misapplying these definitions can lead to either overestimating security posture or under-investing in protection strategies.


‍


## **Risk Transfer: End of Life ≠ End of Use**


When vendor support ends at EOS, risk doesn’t disappear - it shifts. But responsibility only truly transfers when three things are in place:


1. **Documentation** of what the risks are and how to mitigate them.
2. **Training** for those who will own ongoing security.
3. **Technical capability** for the healthcare delivery organization (HDO) to actually apply external controls, network segmentation, or mitigations.


This distinction between **end-of-support** and **end-of-use** is critical. Devices often stay in service well past official support timelines, and without a structured risk transfer process, HDOs inherit risk by default - often unknowingly.


## **Real-World Impact: WannaCry’s Lesson**


The 2017 WannaCry ransomware outbreak drove this point home. In the UK, **81 of 236 NHS** hospitals were affected, with 20,000+ cancellations of appointments and procedures.


Why? Legacy systems running unpatched, popular software stacks created an outsized attack surface. As the webinar highlighted, attackers are economically rational: when vulnerabilities scale across many organizations, compromise is only a matter of time, even if, as in the NHS case, an organization is not specifically targeted.


## **Don’t Skip Decommissioning**


Another overlooked risk? Improper device retirement. Too often, equipment leaves service with PHI, clinical trial data, or even user or Wi-Fi credentials still stored locally. These artifacts can surface later in secondary markets, exposing sensitive data long after the device’s “official” life is over.


Effective decommissioning should include data purging, credential resets, verification that no sensitive information can be recovered, and documentation of these activities.


## **Looking Forward: Procurement, PQC, and Beyond**


Securing legacy devices isn’t just about end-of-support and end-of-use planning. Organizations should:


**Healthcare Delivery Organizations (HDOs):**


- Bake cybersecurity requirements into procurement and contracts to ensure vendors are accountable from day one.
- Segment and monitor networks to contain the inevitable risks of older devices still in service.


‍


**Medical Device Manufacturers (MDMs):**


- Follow secure lifecycle processes and “secure by design” principles.
- Plan for post-quantum cryptography (PQC): Current public-key algorithms may fall within 5–10 years. Considering the long life of medical devices, migration paths must be charted now and design changed or updated to be future-ready.


## **Top Takeaways**


1. **Plan early for EOL/EOS** : document and communicate support windows as part of FDA readiness.
2. **Define “legacy” precisely** : don’t rely on a single standard; understand the nuances across IMDRF, IEC, and ISO and how different stakeholders, based on their context, may use “legacy” differently.
3. **Treat risk transfer as a process** : make sure risk is actually transferrable and ensure HDOs are equipped with training, documentation, and tools.
4. **Don’t neglect decommissioning** : protect PHI, sensitive data, and credentials beyond the device’s usable life.
5. **Think ahead to PQC** : crypto agility must be on the roadmap today.


## **Watch the Full Webinar**


This recap only scratches the surface. To hear the full discussion - including practical strategies, regulatory insights, and expert Q&A - watch the on-demand webinar:


**Watch the recording here:**[https://youtu.be/IvW248JuFko](https://youtu.be/IvW248JuFko)


‍
