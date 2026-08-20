---
schema_version: "1.0.0"
document_id: "5f3351b8eb51952c6b23a1a7be60909d8b8377e4928e7f0036199ef3851f1455"
company_key: "yc-onefin"
company: "OneFin"
source_id: "yc-onefin-rss-7ca07195f9bf"
canonical_url: "https://www.onefin.in/post/inside-rbi-s-proposed-model-risk-management-framework"
published_at: "2026-07-15T10:44:30+00:00"
first_seen_at: "2026-07-20T23:20:39.909173+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:252c1af4867f200a4d3a433a38ffb4816f1b160435745c2eee13dd58bb40d3e5"
---

# Inside RBI's Proposed Model Risk Management Framework

For most lenders, model risk management till now has meant credit scoring. A bureau-based scorecard, some NPA prediction analytics, periodic risk team review. The regulatory anchor was a single chapter in RBI's 2002 Credit Risk Management guidance. That chapter predates machine learning in Indian lending.


The draft "


[Guidance on Regulatory Principles for Model Risk Management, 2026](https://rbidocs.rbi.org.in/rdocs/Content/PDFs/DRAFTGUIDANCE24062026FF12A4FF7BC84E8887009D5C5365F8BF.PDF) ," released on June 24, changes this baseline. It is open for public comment until July 24. The proposed framework covers all models used by regulated entities across the full model lifecycle. It applies to all NBFC layers: Base, Middle, Upper, and Top. Banks, cooperative banks, AIFIs, ARCs, and credit information companies are also in scope. For lenders that have digitized rapidly, the operational implications run deeper than a policy update.


##


**What Counts as a Model**


The definition is the starting point. A model, under the draft, is not limited to formal ML systems or credit algorithms. The guidance defines it as any system that materially influences business or operational decision-making. This includes algorithms, analytics, interfaces, applications, and decision-based rules.


The guidance provides a specific illustration. A spreadsheet-based loan pricing calculator takes inputs such as borrower type, tenor, credit score, and collateral value. It applies processing logic like interest rate grids and margin formulas. It produces a lending rate that shapes business decisions. Under this framework, it is a model. The label the lender attaches to it does not change that.


The practical scope is wide. All these qualify as models:


-


Informal pricing tools


-


Cutoff-based rule engines


-


Analytically-driven collections prioritization logic


Most NBFCs will find their actual model inventory is much broader than current records show.


##


**The Governance Framework**


The proposed framework requires a Board-approved Model Risk Management Framework (MRMF). This is an operational architecture, not a policy document. It covers governance structure, risk tiering, inventory standards, and lifecycle policies. These span development, validation, deployment, monitoring, change management, and decommissioning.


Governance operates at 3 levels:


-


The Board approves the MRMF. It sets the risk appetite for model risk. Risk appetite must reflect scenario analysis and stress testing. It also approves the model tiering policy.


-


The Risk Management Committee of the Board (RMCB) reviews validation reports. It approves deployment of all high-risk models. It oversees third-party models and AI models with enhanced scrutiny, regardless of risk tier. Tiering classifications are reviewed at least annually.


-


Senior Management operationalizes the MRMF. It allocates resources, maintains the model inventory, and reports to the RMCB.


Three lines of defense apply below Board level. Model owners form the first line. An independent validation function forms the second, separate from development teams. A robust internal audit function forms the third.


##


**Third-Party Models: Accountability Cannot Be Outsourced**


These governance requirements apply in full to third-party models. Paragraph 45 of the draft is direct: an RE using third-party models at any stage is accountable for their outcomes. No vendor certification substitutes for the lender's own independent validation. RMCB oversight is required regardless of risk tier.


Before acquiring a third-party model, due diligence must cover:


-


The provider's credibility and the model's methodological soundness.


-


Model limitations and the quality of data used.


Contracts with third-party providers must include:


-


Technical records sufficient to validate the model against the lender’s own MRMF standards.


-


Audit rights for the lender and its supervisory authority.


-


Continuity and exit arrangements.


For material AI models, the draft flags concentration risk. Reliance on a limited number of providers creates supply chain exposure. It also limits independent validation. Provider-driven updates can alter model behavior without the lender’s control.


##


**Inventory, Lifecycle, and the 10-Year Trail**


Every model must be inventoried. This covers active models, inactive models (including those under development), and decommissioned models. No model may be used or relied upon unless it is inventoried.


The inventory must capture owners, developers, validators, and approvers. It must record risk tier, intended use, and upstream and downstream dependencies. Key observations from validation, monitoring, and audit must also be included.


The decommissioning clause deserves attention. Retired models must stay in inventory for at least 10 years. The clock starts from the date of decommissioning. Or it starts when the model ceases to serve as a backup reference, whichever is later. Records must be maintained in parallel.


Across the lifecycle, the draft imposes structured disciplines:


-


Documented rationale is required before development begins.


-


Independent validation applies before and after deployment, following modifications, and regularly per the MRMF.


-


Validation reports must reach the RMCB within 3 months of completion.


-


A defined threshold for "material change" triggers full re-validation and re-approval.


-


Business continuity planning must cover model unavailability, performance degradation, and fallback mechanisms.


##


**AI and ML: The Additional Layer**


The draft does not treat AI and ML as a separate category. It applies the base MRMF and adds further requirements.


Key AI-specific provisions:


-


Explainability thresholds must be defined for all AI models. Higher thresholds apply to models used in material decision-making or with significant customer impact. Where full explainability is not achievable, compensating controls apply. These include enhanced validation, output corroboration before use, usage restrictions, and more frequent re-validation.


-


Bias and fairness assessments are required for models that influence customer outcomes. Recalibration or redesign is required where discriminatory outputs are identified.


-


Models must be tested for overfitting using out-of-sample data. Data drift and concept drift must be monitored on an ongoing basis.


-


Structured challenge processes, including red-teaming, apply to models with customer interaction or generative capabilities.


-


Auto-updating models require enhanced controls. The scope of automatic updates must be defined. Monitoring must be more stringent.


For customer-facing AI, deployment controls must include safeguards against prompt injection, user disclosure of AI interaction, and an option to switch to human assistance.


Human oversight is mandatory. Kill-switch mechanisms and override arrangements are required. Regular human review of model-driven decisions is also required. The draft explicitly names automation bias and decision fatigue as risks the oversight mechanism must address. This signals that RBI is not just mandating governance structures. It is addressing how humans actually behave around model outputs in practice.


##


**What This Means for Lenders**


Several implications of the proposed framework deserve attention.


**A.** The inventory exercise will surface tools lenders do not currently classify as models. Pricing calculators, eligibility rule engines, and collections logic all qualify under the definition. The scope audit is likely the first significant compliance task.


**B.** Existing third-party contracts almost certainly do not meet the proposed requirements. The draft mandates technical records access, audit rights, and exit arrangements. Most agreements predating this guidance will lack these provisions. Renegotiating them is a procurement and legal action, not a compliance memo.


**C.** The organizational build required is substantial. An independent validation function and dedicated model owners are structural requirements. So is internal audit with genuine model risk expertise. For NBFCs with lean analytics teams, this cannot be addressed by reassigning existing staff.


**D.** AI deployment is explicitly conditional under the draft. AI and ML models should only be deployed where the NBFC can manage risks effectively. This is a governance gate on adoption, not just an oversight overhead.


OneFin's LOS and LMS platform supports model monitoring, validation audit trails, and change records across the loan lifecycle. This is the operational infrastructure the proposed framework requires.


##


**Priorities & Next Steps**


The final circular will carry the implementation timeline. But lenders that wait for finalization will find the gap harder to close.


Near-term actions worth initiating now:


-


**Scope audit:** Catalog every tool that materially influences a business decision and apply the guidance definition broadly. The inventory will be wider than expected.


-


**Third-party contract review:** Assess existing vendor agreements for records access, audit rights, and exit provisions. Flag gaps for renegotiation.


-


**Governance gap assessment:** Map current model ownership and validation arrangements against the three-lines requirement. Identify where dedicated functions are absent.


-


**Model tiering:** Classify inventoried models by materiality and complexity. High-risk models will require RMCB approval under the proposed framework.


-


**AI deployment review:** For deployed AI models, assess whether explainability thresholds are defined and compensating controls are in place.


RBI has been consistent in this direction across its recent regulatory work. Governance must keep pace with analytical capability. Lenders that have built model-dependent operations now face the harder task. The stack must be legitimate before the framework is finalized.


To know more about OneFin


,


[schedule a Demo](https://www.onefin.in/request-demo) .
