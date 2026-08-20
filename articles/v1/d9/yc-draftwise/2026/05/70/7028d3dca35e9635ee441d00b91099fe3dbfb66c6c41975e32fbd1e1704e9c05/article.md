---
schema_version: "1.0.0"
document_id: "7028d3dca35e9635ee441d00b91099fe3dbfb66c6c41975e32fbd1e1704e9c05"
company_key: "yc-draftwise"
company: "Draftwise"
source_id: "yc-draftwise-news-import-f9e9b1f848aa"
canonical_url: "https://www.draftwise.com/blog/what-to-ask-before-deploying-legal-ai-a-security-guide-for-law-firms"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-21T16:59:12.101501+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:bf231aa106ddf38fcbf6dc9faa4b2f171d9fe3519898a51784898eca51eac5ef"
---

# What to Ask Before Deploying AI Contract Review Software | Draftwise

Legal AI platforms process your most sensitive data: client files, deal terms, negotiation strategy, and work product. Before onboarding a new platform or tool, your firm needs answers to a specific set of security questions. Most vendor evaluations don't ask them.


This guide covers what to ask and why it matters.


## Start With Ownership, Not Features


Vendor demos lead with productivity. Your security conversation should lead with something more fundamental: Does your firm retain complete, sole ownership and control of its data at all times?


The details matter here. The vendor must commit, contractually and architecturally, that your documents, client files, and institutional knowledge will never be used to train AI models, never disclosed to third parties, and never surfaced as aggregated intelligence that could benefit competing firms or their clients. That last point is easy to overlook: insights derived from your data are still your data, even after anonymization or aggregation.


Before any other conversation, ask for three documents: the vendor's Privacy Policy, Information Security Policy, and Data Retention Policy. These should be published, specific, and shareable. If a vendor can't produce them, that's your answer.


## Ask About the LLM Layer — Most Firms Don't


Legal AI platforms don't process your data entirely in-house. They route it through external large language model providers, the underlying AI engines that power document analysis, drafting suggestions, and contract intelligence. This is standard practice. What varies significantly between vendors is what happens to your data at that layer.


The risk: a vendor can maintain strong internal security practices while their LLM provider quietly retains your inputs, logs your prompts, or reserves the right to use that content for model training.


Ask directly: Does the vendor have explicit zero-data-retention agreements with every LLM provider in their stack? Not standard API terms — enterprise contracts specifying that inputs and outputs are not stored, not logged, and not used for training. Can they demonstrate ongoing compliance monitoring? What happens if a provider changes its data handling policies?


Most firm security questionnaires don't yet cover this layer. Getting specific here will surface meaningful differences between vendors.


## What Certifications Actually Tell You


Certifications are third-party verification of claims the vendor would otherwise make on their own behalf. They're not sufficient on their own, but their absence is a red flag.


The baseline for a law firm deploying AI on client data:


- **SOC 2 Type II:** The most common enterprise security certification; audits controls around security, confidentiality, and privacy over a sustained period. Type II is substantially stronger than a point-in-time Type I audit.
- **ISO 27001:** The international standard for information security management. Signals a systematic, audited approach to security risk.
- **ISO 42001:** The international standard for AI management systems. Covers AI governance, risk, and lifecycle controls, and signals that a vendor treats AI risk as its own discipline **.**
- **GDPR Compliance:** Required for any client work touching European data.


Ask to see the actual reports or certificates, not a checkbox on a product page. Then ask something that doesn't appear on any certification checklist: has the vendor's engineering team built secure systems for organizations with comparably sensitive data, large enterprises, financial institutions, or government agencies? Certifications reflect past audits. Team experience predicts future security decisions.


## Encryption: The Follow-Up Questions That Matter


Most vendors will confirm their platform is encrypted. These follow-up questions are what separate a serious security posture from a talking point:


- Is data encrypted both at rest and in transit? Both is the standard; either alone is insufficient.
- Which protocols? AES-256 at rest and TLS 1.2 or 1.3 in transit are the current benchmarks.
- Who holds the encryption keys, your firm or the vendor?
- Are comprehensive audit logs maintained with anomaly detection, and for how long are they retained?


Audit logs matter for a related reason: they create an immutable record of who accessed what and when, which is essential not only for incident response but also for meeting professional responsibility obligations regarding client data.


## A Checklist for Your Vendor Evaluation


Before any legal AI platform handles client data, confirm each of the following:


- The firm retains complete ownership of all data, with no vendor right to use it for training, aggregation, or any other purpose.


- The vendor has published enforceable policies covering privacy, information security, and data retention.


- The vendor holds explicit zero-data-retention agreements with every LLM provider in their stack, not just standard API terms.


- The vendor is SOC 2 Type II certified. ISO 27001 and ISO 42001 certifications are strong additional signals, particularly ISO 42001 for AI-specific governance.


- Data is encrypted at rest (AES-256 or equivalent) and in transit (TLS 1.2+).


- Your firm has the option to hold its own encryption keys.


- Audit logs are maintained in near-real-time, with anomaly detection, and retained for at least two years.


- The platform supports deployment configurations that fit your firm's infrastructure and compliance requirements.


- The vendor's security team has experience building secure systems for organizations handling equivalently sensitive data.


## The Bottom Line


Firms that engage with legal AI thoughtfully will be better positioned than those who either rush in or hold back entirely. Security evaluation doesn't slow that process down — it just needs to happen in parallel with the feature evaluation, not after the contract is signed.


Vendors that have built their platforms on a foundation of security will welcome these questions. The ones who haven't will struggle to answer them specifically. That distinction is usually apparent within a single conversation.


*For a closer look at how Draftwise enforces zero data retention at the LLM provider layer, read:*[Our Commitment to Zero Data Retention](https://www.draftwise.com/blog/our-commitment-to-zero-data-retention)


*Further reading:*[draftwise.com/privacy-policy](https://www.draftwise.com/privacy-policy) *|*[draftwise.com/information-security-policy](https://www.draftwise.com/information-security-policy) *|*[draftwise.com/data-retention-policy](https://www.draftwise.com/data-retention-policy)
