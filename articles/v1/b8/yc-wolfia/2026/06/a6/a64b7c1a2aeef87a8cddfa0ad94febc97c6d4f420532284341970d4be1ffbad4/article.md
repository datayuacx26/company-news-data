---
schema_version: "1.0.0"
document_id: "a64b7c1a2aeef87a8cddfa0ad94febc97c6d4f420532284341970d4be1ffbad4"
company_key: "yc-wolfia"
company: "Wolfia"
source_id: "yc-wolfia-news-import-63b7007a854b"
canonical_url: "https://wolfia.com/blog/we-mapped-all-261-caiq-v4-questions-by-domain"
published_at: "2026-06-25T00:00:00+00:00"
first_seen_at: "2026-07-22T23:41:27.201440+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:a051fbc97e98b938be1ecbb079024f0af8e81e8ab46b9ff18c4f4c8049a5121d"
---

# We mapped all 261 CAIQ v4 questions by domain

**TL;DR**


- CAIQ v4 has 261 questions organized across 17 domains and 197 control objectives.
- The five heaviest domains (DSP, CEK, DCS, IAM, HRS) hold 111 questions, or 42.5 percent of the total.
- CAIQ v3.1 had 310 questions; v4 cut 49 of them, a 16 percent reduction driven by control consolidation.
- Logging and Monitoring (LOG) is the one domain added new in v4.
- The three lightest domains (GRC, A&A, IPY) account for just 25 questions combined, under 10 percent of the questionnaire.


## What the CAIQ actually is


The Consensus Assessment Initiative Questionnaire is the Cloud Security Alliance's standardized self-assessment format. Buyers use it to evaluate cloud vendor security posture, and it maps directly to the[CSA Cloud Controls Matrix (CCM)](https://cloudsecurityalliance.org/research/cloud-controls-matrix) . The CCM defines the control objectives; the CAIQ is the questionnaire layer built on top of them.


When a prospect sends you a CAIQ, they are asking whether your environment satisfies each CCM control objective. The format is consistent enough that, unlike custom questionnaires, the questions are predictable. You know what domains will appear, you know roughly how many questions each domain carries, and you can build reusable answer sets before the next questionnaire lands.


CAIQ v4 is the current version. It replaced v3.1, which had been the standard for several years. If you have answered a CAIQ recently, it was almost certainly v4.


## How v4 changed from v3.1


The shift from v3.1 to v4 was not cosmetic. CAIQ v3.1 had 310 questions. CAIQ v4 has 261, a reduction of 49 questions, about 16 percent. CSA consolidated overlapping controls and tightened the mapping so each question connects more directly to a specific control objective. That is why v4 has 197 control objectives while v3.1 had more diffuse coverage, averaging roughly 1.3 questions per control across the full framework.


The other structural change: Logging and Monitoring (LOG) became its own dedicated domain in v4. In prior versions, logging controls were distributed across several other domains. Giving LOG its own home reflects how central observability has become to cloud security reviews. Before v4, logging-related answers lived in multiple places and were prone to going inconsistent when one domain was updated and another was not. A standalone LOG domain fixes that.


The net effect for GRC teams: v4 is slightly shorter and better organized than v3.1, but the domains that were heavy before are still heavy. Data security, cryptography, and identity remain the most demanding sections.


## All 17 domains, question by question


These counts come directly from the CAIQ tab of the official CCM v4.0.8 workbook. The sum across all 17 domains is 261.


Domain Code Questions


Data Security and Privacy Lifecycle Management DSP 24


Cryptography, Encryption, and Key Management CEK 23


Data Center Security DCS 23


Identity and Access Management IAM 21


Human Resources Security HRS 20


Business Continuity Management and Operational Resilience BCR 18


Logging and Monitoring LOG 18


Supply Chain Management, Transparency, and Accountability STA 15


Universal Endpoint Management UEM 15


Infrastructure and Virtualization Security IVS 14


Threat and Vulnerability Management TVM 12


Application and Interface Security AIS 11


Change Control and Configuration Management CCC 11


Security Incident Management, E-Discovery, and Cloud Forensics SEF 11


Governance, Risk, and Compliance GRC 9


Audit and Assurance A&A 8


Interoperability and Portability IPY 8


## Which five domains account for 42 percent of the questionnaire


DSP, CEK, DCS, IAM, and HRS together hold 111 of 261 questions, 42.5 percent of the total. If your answers in those five domains are current, detailed, and accurate, you have handled nearly half the questionnaire before you touch anything else.


Expand to the eight heaviest domains, everything down through the supply chain domain, and you have covered 162 of 261 questions, or 62 percent. The remaining nine domains share the other 38 percent, with the three lightest sitting at 8 or 9 questions each.


This distribution matters for how you prioritize. GRC teams answering CAIQs for the first time often treat all domains as equally urgent. Building strong, sourced answers for DSP, CEK, DCS, IAM, and HRS first gets you through the most review-intensive part of every CAIQ you will receive. Teams that spread effort evenly across all 17 domains typically spend more time on lower-stakes sections and then scramble on the ones buyers care about most.


## Why data security and cryptography sit at the top


DSP's 24 questions cover the full data lifecycle: classification, retention, disposal, data loss prevention, privacy notices, and data handling agreements. Buyers focus here because a single data mishandling incident in a cloud vendor can trigger GDPR fines, breach notifications, or contract violations on their side. The depth of DSP questions in a CAIQ tracks directly with enterprise risk appetite for data residency and handling. A vague DSP section often prompts follow-up questions or a separate custom addendum.


CEK's 23 questions cover encryption at rest, encryption in transit, key generation, rotation, escrow, and destruction. With SOC 2 and ISO 27001 both requiring documented cryptographic policies, buyers have become more specific here. Questions asking what algorithm standards your organization enforces for data in transit are harder to answer generically than simple yes/no encryption questions. CEK rewards teams that have turned vague policies into specific documented practices with named standards.


DCS covers physical and logical data center controls: access logs, environmental safeguards, shared tenancy isolation, and media disposal. Cloud vendors running on AWS, Azure, or GCP typically inherit much of this from their providers, but DCS still requires showing what controls sit on top of the provider's baseline. Buyers in regulated verticals want to understand your controls, not just your provider's.


## What changed when CSA added the LOG domain in v4?


Before v4, logging and monitoring controls appeared across the CCM but had no dedicated section. In v4, CSA carved out LOG as its own 18-question domain, covering audit log generation, monitoring, alerting, retention, and log protection.


The practical effect for GRC teams: LOG answers now live in one coherent section and can be maintained as a set. Before, logging controls were scattered across IVS, IAM, and other domains, making it easy for answer sets to drift when one domain was updated and another was not. A security team updating their SIEM documentation would fix the IAM logging reference but miss the IVS one.


The 18-question count also signals how seriously buyers weigh observability. Logging and monitoring used to be a footnote in most questionnaires. As vendor risk programs have matured, buyers want specifics: what events are logged, how long logs are retained, who has access to them, and what alerting exists on anomalies. LOG at 18 questions puts it on par with BCR and just below the five heaviest domains.


## The three lightest domains and what buyers actually ask there


GRC (9 questions), A&A (8), and IPY (8) together account for 25 of 261 questions, under 10 percent. Light question counts do not mean easy answers.


GRC questions ask about your governance structure: risk program ownership, policy review cadence, and compliance program scope. Buyers with mature third-party risk programs read GRC answers carefully because they reveal whether security is a documented program or an informal practice. A weak GRC section can flag a vendor as high-risk even if the technical domains look solid.


A&A questions cover audit scope, internal audit frequency, and how findings are tracked. The answers here feed directly into vendor risk scorecards. Auditors sometimes use A&A follow-up questions to probe the basis for answers given in heavier domains, so weak A&A documentation has a way of surfacing problems elsewhere in the review.


IPY questions address data portability, exit planning, and interoperability standards. These matter most to buyers in regulated industries who need assurance they can move data out of a vendor's environment in a structured way. A buyer's legal team often flags IPY during contract review.


## How do you answer CAIQ without rebuilding answers each time?


The short answer: you do not rebuild them. You maintain a live knowledge base that updates when your controls change, and you draw answers from it each time a CAIQ arrives rather than researching each question from scratch.


The problem most teams run into is not answering the CAIQ the first time. It is the second, third, and fourth time a CAIQ arrives after your encryption library changed, your data center shifted to a new provider, or your key rotation policy was updated. Manually tracking which CAIQ answers need refreshing after an infrastructure change is where teams fall behind and where response timelines stretch from days to weeks.


On the questionnaires that GRC teams see most often, the recurring choke point is not the initial build of answers; it is keeping DSP and CEK current as the underlying controls evolve. A data classification policy gets updated, and three months later someone sends a new CAIQ before anyone has updated the DSP answer set. The answer goes out stale, the buyer notices a discrepancy with your SOC 2 report, and the deal stalls on a follow-up.


For[teams answering security questionnaires without a dedicated security function](https://wolfia.com/blog/answer-security-questionnaires-no-security-team) , the compounding nature of this maintenance burden is what pushes cycle times out. CAIQ is predictable enough to build answers once and keep them current, but only if the knowledge base is connected to the actual controls rather than a static document library that no one has time to groom.


## How Wolfia handles CAIQ's heaviest domains


Wolfia is built for GRC teams handling exactly this workflow. When a CAIQ arrives through a portal like OneTrust or ServiceNow, Wolfia's Chrome extension reads the questions directly in the portal and generates answers pulled from your knowledge base, with a source citation on every response. The team reviewing the output can see which document or policy each answer came from before approving it.


For the heavy domains where answer quality matters most, DSP, CEK, and IAM, the knowledge base auto-updates when your underlying documentation changes. You do not tag answers to domains manually or maintain a separate CAIQ answer library. The knowledge base pulls from your policies, audit reports, and prior questionnaire responses and surfaces the relevant answer when the question comes in.


On domains where answers need compliance or legal review before going out, Wolfia can auto-route specific responses to the right reviewer for approval. That matters for CEK answers covering key escrow practices or DSP answers touching data residency commitments, where legal wants eyes on the language before it leaves.


Source citations are attached to every answer Wolfia generates, so reviewers can trace any CAIQ response back to the specific document section it drew from. This is particularly useful for A&A and GRC domains, where auditors sometimes ask follow-up questions about the basis for an answer.


The[knowledge base that maintains itself](https://wolfia.com/blog/build-a-questionnaire-knowledge-base-that-maintains-itself) is the structural difference between answering CAIQ v4 in two days and two weeks. Wolfia's self-maintaining approach means the heavy domains do not require a manual review cycle every time something in your environment changes. For teams looking at how to scale responses across CAIQ, SIG, and other frameworks,[security questionnaire automation at scale](https://wolfia.com/blog/scale-security-questionnaire-responses) covers how the workflow fits together end to end.


## Final Thoughts


CAIQ v4's 261 questions are not distributed evenly. DSP, CEK, DCS, IAM, and HRS hold 42.5 percent of the total, and the three lightest domains hold under 10 percent. Knowing the distribution before a questionnaire arrives changes how you prioritize answer-building and where you invest in documentation depth.


The shift from v3.1 to v4 also points to where cloud security reviews are heading: logging and monitoring now have their own 18-question domain, cryptography has become more granular, and data security remains the single largest surface area a buyer will probe. GRC teams that maintain well-sourced answers in the heavy domains go into each review cycle with a meaningful head start. Teams that rebuild from scratch each cycle spend that time catching up rather than improving.
