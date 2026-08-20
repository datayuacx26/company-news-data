---
schema_version: "1.0.0"
document_id: "0f9bff3e5c53af95b92ba8e0b47320b5bf6fd5459be393251969472a32260020"
company_key: "yc-ballerine"
company: "Ballerine"
source_id: "yc-ballerine-news-import-9376e18e0cef"
canonical_url: "https://ballerine.com/blog/aml-screening-for-merchant-portfolios-pep-sanctions-and-transaction-monitoring-for-acquirers"
published_at: "2026-05-14T13:31:21.638+00:00"
first_seen_at: "2026-07-23T02:58:11.390770+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:78325ac375a7edc2cd624e96bdfebb44649034cfd09f7b6a627ea2344a5f548a"
---

# AML Screening for Merchant Portfolios: PEP, Sanctions, and Transaction Monitoring for Acquirers

Acquirers, PayFacs, PSPs, and banks with acquiring programs face a compliance challenge that consumer-focused financial institutions rarely encounter at the same scale: AML risk is distributed across an entire portfolio of business relationships, not just individual customer accounts. Merchant AML programs must account for the legal entities being boarded, the individuals behind them, and the transaction patterns those merchants generate, often across thousands of active relationships.


‍


PEP and sanctions screening, combined with ongoing transaction monitoring, form the two pillars of a merchant AML program. In practice, most programs were built on consumer AML frameworks and adapted imperfectly for the merchant context. The gaps that result are not theoretical; they are the basis for regulatory findings, card scheme fines, and enforcement actions. This guide addresses what acquirer AML programs specifically require, where they typically fall short, and what a complete merchant portfolio monitoring program looks like.


‍


‍


‍


## PEP and Sanctions Screening in a Merchant Context


‍


PEP screening in consumer AML typically means checking an individual's name against political exposure lists at account opening. In a merchant context, the target of that check is a legal entity, but the underlying obligation is the same: identify whether the people who own or control that business are politically exposed or subject to sanctions.


‍


For acquirers, this means pep and sanctions checks extend beyond the merchant applicant itself. Beneficial ownership structures must be traced to the natural persons who hold controlling interests, consistent with applicable FinCEN guidance and mirrored in EU AML frameworks. Depending on jurisdiction, this typically means identifying and screening individuals who own above a defined ownership threshold, as well as authorized signatories, directors, and officers with material operational authority over the entity.


‍


Sanctions coverage for merchant portfolios must address the relevant lists for each geography in which the acquirer operates. These typically include the OFAC Specially Designated Nationals and Blocked Persons List, the EU Consolidated List of Persons, Groups and Entities Subject to EU Financial Sanctions, the UN Security Council Consolidated List, and the UK HM Treasury Financial Sanctions Targets list. Acquirers operating across multiple jurisdictions must maintain coverage across all relevant lists, not only the primary regulatory framework of their home market.


‍


Where merchant-level PEP screening diverges from consumer AML is in the layered nature of the check. A consumer screen is largely a one-to-one match against an individual's name. A merchant screen requires mapping a legal entity to its principals, normalizing data across jurisdictions, and applying pep screening solutions to every named individual with a material relationship to the business. Corporate structures with multiple ownership layers, nominee directors, or beneficial owners in high-risk jurisdictions add complexity that individual consumer checks rarely surface. The population of individuals who must be screened for a single merchant can be substantially larger than it appears at the entity level.


‍


Merchant AML control room


### Every merchant signal needs to be read in context


#### Ownership map


Who owns, controls, or operates the merchant.


#### PEP & sanctions


List exposure across relevant jurisdictions.


#### Adverse media


Early-warning signals from public sources.


risk signal


#### Merchant baseline


The reference point that turns raw signals into explainable AML decisions.


#### Transaction behavior


Volume, velocity, refunds, and ticket size.


#### Geography shifts


Cross-border activity outside the approved profile.


#### MCC fit


Whether payment activity still matches the business model.


Alert


Decision record


Audit trail


‍


‍


## Business Transaction Monitoring vs. Customer Transaction Monitoring


‍


Customer transaction monitoring, as deployed by retail banks and consumer payment services, focuses on individual behavioral patterns: unusual transfer volumes, geographic anomalies, structuring indicators, and changes in account behavior relative to a customer's profile. The signals are individual, and the baseline is personal financial behavior.


‍


Business transaction monitoring in a merchant portfolio operates on different signal types. The relevant unit of analysis is the merchant, not the cardholder. Acquirers monitor aggregate transaction volumes relative to the merchant's approved business model, refund and chargeback rates, the geographic distribution of card-present and card-not-present transactions, average ticket size relative to the merchant category code, and transaction velocity patterns that deviate from the merchant's established norms.


‍


‍


What triggers review at the merchant portfolio level is a deviation from expected business behavior, not from personal financial behavior. A merchant processing three times its historical monthly volume without a corresponding change in business circumstances warrants investigation. A sudden increase in cross-border transactions for a merchant approved for domestic sales is another signal. A refund rate that rises while transaction volume remains flat may indicate dispute absorption intended to avoid chargeback thresholds. These are business risk signals, and monitoring logic must be calibrated to merchant-level baselines, not population-level averages.


‍


This distinction matters for acquirers evaluating transaction monitoring solutions. Platforms designed for consumer AML will not surface the right signals for a merchant portfolio. Effective business transaction monitoring requires merchant-specific behavioral models, configurable thresholds by MCC and business type, and alert logic that connects current transaction data to the merchant's original underwriting profile. Without that connection, analysts evaluate alerts without the context needed to distinguish genuine risk from normal business variation.


‍


‍


‍


## Common Gaps in Acquirer AML Programs


‍


The most common structural gap in acquirer AML programs is point-in-time screening. A merchant is screened at onboarding, cleared, and not revisited until periodic review, which in many programs means annually or less frequently. Sanctions lists are updated continuously, sometimes multiple times within a single day. A merchant principal who was not a PEP at onboarding may become one following a political appointment. A business relationship that was clean when approved may become impermissible within months. Without a defined re-screening cadence tied to ongoing monitoring, these changes go undetected until identified by a regulator or card scheme.


‍


A second gap is the separation between monitoring systems and underwriting data. Many acquirers built their monitoring tools independently of, or well before, their underwriting infrastructure. The result is that monitoring alerts are evaluated without context about what was known at boarding. A transaction volume spike that looks suspicious in isolation may be entirely consistent with the merchant's seasonal business profile, documented in the underwriting file but inaccessible to the monitoring team. When business transaction monitoring is siloed from underwriting data, analysts either over-alert on benign activity or lack the context to properly triage genuine risk signals.


‍


The third gap is the absence of adverse media integration. PEP and sanctions screening checks individuals and entities against structured lists. Adverse media screening checks the broader information environment: news sources, court records, regulatory actions, and public databases for negative coverage associated with the merchant or its principals. Acquirers without adverse media integration will miss the reputational and criminal risk signals that precede list inclusion. By the time a business or its owner appears on a sanctions list, adverse indicators have typically been visible in public sources for an extended period. Adverse media screening does not replace peps and sanctions checks, but its absence creates a meaningful early-warning gap.


‍


Merchant AML lifecycle


### Merchant AML risk evolves after onboarding, which is why continuous monitoring matters


Step 1


#### Merchant onboarding


The merchant is initially approved based on documents, ownership structure, MCC, and underwriting assumptions.


Step 2


#### Continuous monitoring


Merchant activity is continuously monitored for geography shifts, sanctions exposure, adverse media, and transaction anomalies.


Step 3


#### Risk escalation


Behavior mismatch detected


When merchant behavior no longer matches the original approved risk profile, the case requires escalation and review.


Step 4


#### Resolution


Case resolved


Risk profile updated


Decision recorded


Audit trail saved


The case is closed with a documented decision, the merchant risk profile is updated, and the full audit trail is preserved for regulators.


‍


‍


‍


## Regulatory Requirements


‍


In the United States, acquirer AML obligations are shaped primarily by the Bank Secrecy Act and FinCEN regulations, interpreted through the[FFIEC BSA/AML Examination Manual](https://bsaaml.ffiec.gov/manual) . The manual addresses payment processors and acquirers directly, establishing that institutions with acquiring relationships are expected to conduct customer due diligence on the merchants they board, monitor those relationships for suspicious activity, and file Suspicious Activity Reports where warranted. Expectations for third-party payment processor relationships are further addressed in agency guidance from the FDIC and OCC, referenced within the examination manual.


‍


Card scheme rules impose an additional layer of requirements independent of regulatory frameworks. Visa's and Mastercard's portfolio monitoring programs set specific expectations for acquirers related to merchant activity thresholds, chargeback management, and portfolio-level risk controls. Failure to meet scheme standards can result in fines, remediation obligations, or loss of acquiring privileges, separate from any regulatory enforcement action.


‍


For PSPs and PayFacs operating in European markets, the EU AML framework applies. The EU's Anti-Money Laundering Regulation and the 6th Anti-Money Laundering Directive, which member states must transpose by July 2027, establish harmonized customer due diligence and monitoring obligations for payment service providers across the EU. The Anti-Money Laundering Authority (AMLA), operational since July 2025, introduces direct supervisory oversight of the highest-risk financial actors in coordination with national supervisors. PSPs operating across multiple EU jurisdictions should track both the centralized requirements under the AMLR and country-level transposition for the directive components, as variation across member states will persist during the implementation period.


‍


One practical implication of this multi-regulatory environment is that acquirer AML programs cannot be designed around a single framework. An acquirer that is BSA-compliant but lacks adequate PEP and sanctions screening coverage for EU-exposed merchants, or a PayFac that meets card scheme monitoring thresholds but cannot produce audit-ready documentation of its screening decisions on demand, is exposed in the dimensions it has not addressed.


‍


‍


‍


## What Merchant Portfolio AML Monitoring Must Include


‍


A program that addresses the regulatory requirements above and closes the structural gaps identified in the previous section requires four components working in coordination.


‍


Continuous re-screening is the foundation. Merchant principals, beneficial owners, and authorized signatories must be re-screened against sanctions lists and PEP registries on an ongoing basis, not only at the point of onboarding. The frequency should be defined by the acquirer's risk appetite and the volatility of the lists being monitored. For high-risk merchant categories or complex ownership structures, more frequent screening intervals are appropriate. Re-screening must be documented with timestamps and decision records.


‍


Adverse media integration must be embedded into the ongoing monitoring workflow, not treated as an occasional manual task. Adverse media checks should follow the same rigor as structured list screening: defined source coverage, documented methodology, and an audit trail for each check. At portfolio scale, this requires tooling that runs adverse media checks continuously and surfaces relevant signals with enough context for an analyst to triage without reviewing every underlying source in full.


‍


Business transaction monitoring must use explainable alert logic tied to each merchant's individual baseline. When an alert fires, the reviewer needs to understand what changed, relative to what established pattern, and why it meets the threshold for review. Explainable alert logic is also a regulatory expectation: examiners reviewing an acquirer's AML program will examine how monitoring alerts are generated and how analysts determine whether to escalate or close them. Generic, population-level thresholds do not satisfy this standard.


‍


Audit-ready documentation must cover every stage of the program. Screening decisions, alert dispositions, SAR filing determinations, and re-screening records must be preserved in a format that can be produced to a regulator or card scheme without additional reconstruction. This means timestamped records, clear reason codes for each disposition, and traceable linkage between the monitoring alert and the underlying merchant file.


‍


Ballerine's[Merchant Monitoring](https://ballerine.com/solutions/merchant-monitoring) product provides continuous re-screening across adverse media, web signals, and behavioral transaction data, with configurable alert logic tied to each merchant's underwriting profile. The[Policy Fit](https://ballerine.com/compnay-fit-policy) product extends coverage to acceptable use policy compliance, enabling acquirers to monitor merchants against their own defined risk standards rather than static rule sets. Both products generate structured, audit-ready documentation built for regulatory review, card scheme reporting, and internal oversight.


‍
