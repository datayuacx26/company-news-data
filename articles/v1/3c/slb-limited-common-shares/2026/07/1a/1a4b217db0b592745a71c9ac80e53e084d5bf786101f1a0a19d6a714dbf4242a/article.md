---
schema_version: "1.0.0"
document_id: "1a4b217db0b592745a71c9ac80e53e084d5bf786101f1a0a19d6a714dbf4242a"
company_key: "slb-limited-common-shares"
company: "SLB Limited Common Shares"
source_id: "slb-limited-common-shares-news-import-99bb013bef99"
canonical_url: "https://www.slb.com/insights/a-framework-for-managing-cyber-supply-chain-risk-in-the-energy-sector"
published_at: "2026-07-24T13:57:00+00:00"
first_seen_at: "2026-07-25T02:18:49.163179+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:bbfe671f5fe4481339cd2e0719996490c44274449d40c90c40ec11b79964f718"
---

# A framework for managing cyber supply chain risk in the energy sector

## Key takeaways


- Supplier risk should be measured by operational consequence, not by spend, category, or procurement importance.
- Cyber resilience is strongest when it’s built into contracts, architecture, access controls, and onboarding before a supplier ever connects to critical systems.
- Static vendor questionnaires are no longer enough; organizations need continuous telemetry that can detect exposed services, leaked credentials, vulnerable dependencies, and concentration risk.
- Effective supply chain security depends on clear decision rights, joint incident exercises, and cross-functional governance that can act quickly when supplier risk becomes operational risk.


Digital supply chains have become one of the most important—and least contained—sources of cyber risk for global energy companies. Critical operations now depend on a broad ecosystem of software providers, cloud platforms, managed service partners, equipment vendors, subcontractors, and upstream technology dependencies. As these ecosystems become more interconnected, a weakness in one supplier can quickly become an operational, regulatory, financial, or safety issue for the entire enterprise.


Traditional supplier risk management in oil and gas wasn’t designed for this reality. Annual questionnaires, static assessments, and procurement-led reviews can provide a baseline, but they rarely offer the visibility, speed, or accountability needed to manage cyber risk across dynamic digital supply chains. Organizations need a model that reflects how supplier relationships actually function, which is continuously, operationally, and across multiple layers of dependency.


A more effective approach starts by[moving cyber supply chain risk out of isolated compliance processes](https://www.slb.com/insights/whats-a-sustainable-energy-transition-without-cybersecurity) and into the core of business resilience. That means identifying which supplier-enabled services matter most, embedding security expectations before contracts are signed and systems go live, continuously monitoring for material risk signals, and preparing jointly for incidents before they occur. The following four-pillar framework provides a practical blueprint for doing exactly that.


## Pillar 1: business criticality mapping


*Identify what truly matters, and why.*


Instead of classifying suppliers by spend or category, organizations should map them by operational impact using a simple, repeatable process that asks the following questions:


- Would production stop?
- Would safety be affected?
- Would critical data flows break?
- Would regulatory obligations be breached?


Once the operational impact has been clearly defined, the next step is to implement a structured approach to managing supplier risk. This begins with an inventory of critical supplier-enabled services and the associated data flows (not just vendor names). From there, the impact should be assessed across key dimensions such as availability, safety, regulatory exposure, and recovery time.


Based on the assessment, each supplier or service can be assigned a tier along with a corresponding set of required controls, including expectations for evidence depth, monitoring rigor, and exercise cadence. These tiers should be reviewed on a quarterly basis and updated whenever there are changes to system architecture, scope, or access.


The ultimate objective is to enable risk-tiering that reflects operational reality, ensuring deep scrutiny is applied where failure has the greatest consequence.


## Pillar 2: resilience by design


*Bake cyber into contracts, architecture, and onboarding, rather than adding it after.*


The second framework pillar involves defining resilience requirements upfront and then enforcing them by implementing:


- Mandatory contractual controls
- Secure-by-design architectural expectations
- Defined access patterns (least-privilege, time-bound, monitored)
- Preapproved subprocessors
- Software bill of materials (SBOM) visibility for software providers.


Key to effective implementation is the use of a standard contract annex that includes non-negotiable security clauses for critical suppliers.[These contractual requirements should then be translated into concrete technical guardrails](https://www.slb.com/insights/making-record-performance-repeatable-through-alternative-contract-models) —such as defined access patterns, network segmentation, and logging—before any system goes live.


At onboarding, be sure to require clearly named control owners and supporting evidence to ensure accountability, including who approves access, who validates logging, and who tracks exceptions. Finally, organizations should establish a formal exception process that includes documented risk acceptance, defined expiry dates, and appropriate compensating controls for managing deviations transparently without allowing them to persist unchecked.


Some specific considerations with respect to the SBOM include:


- **Require signed SBOMs—** Specify SBOM format (e.g., Software Package Data Exchange or CycloneDX) and require cryptographic signing tied to the supplier’s release process.
- **Bind SBOMs to artifacts—** Require build provenance/attestations that link the SBOM to an immutable artifact hash (and, ideally, to identity-driven continuous integration or delivery).
- **Validate completeness—** Check that direct and transitive dependencies are present, versions are pinned, and licensing and source fields are populated.
- **Verify the delivery chain—** Confirm software is distributed through controlled channels, with code-signing certificates managed and monitored.
- **Continuously reconcile—** Compare SBOM components to vulnerability intelligence; require updated SBOMs for major releases and critical patches.


## Pillar 3: continuous telemetry and dependency visibility


*Move beyond static questionnaires.*


Supplement periodic checks with continuous assessments that can be operationalized, such as external attack-surface monitoring, vulnerability and configuration[telemetry](https://glossary.slb.com/en/terms/t/telemetry) , credential leak monitoring, dependency mapping beyond Tier 1, and concentration risk analysis.


During the process, define what “material supplier risk” means in metrics (e.g., exposed services, critical vulnerabilities and exposures, and leaked credentials). Organizations can then set alert thresholds and escalation paths with response service-level agreements (SLAs), integrate signals into vendor performance reviews and renewal decisions, and maintain a living dependency map for critical services, including nth-party and concentration-risk hot spots.


Practical steps for performing a concentration risk analysis include:


- **Define the unit of concentration—** This could be a shared cloud region, identity provider, managed service provider, code-signing service, niche original equipment manufacturer (OEM), or critical open-source maintainer.
- **Build a dependency graph—** For each critical service, list Tier 1 suppliers and the key upstream services they depend on (from contracts and architecture reviews to SBOMs and incident postmortems).
- **Quantify exposure—** Count how many critical services share the same upstream dependency; estimate impact using recovery time and point objectives (RTOs and RPOs), safety exposure, and revenue and production loss.
- **Rank “single points of systemic failure”—** Prioritize upstream nodes with high criticality and low substitutability.
- **Mitigate—** Diversify providers where feasible, require region redundancy, prenegotiate surge support, and add compensating controls (e.g., segmentation, kill switches, and staged rollouts).
- **Operationalize—** Review quarterly and whenever a vendor changes subprocessors, regions, or privileged access patterns.


The goal is to shift away from annual questionnaires toward a supply chain detection and response (SCDR) that provides continuous visibility. Just remember that turning signal into action requires integrating the SCDR tooling into existing workflows. Some recommendations for achieving a successful transition are to:


- **Start with use cases—** Exposed supplier services, leaked supplier credentials, critical vulnerabilities and exposures in shared components, and abnormal update and signing behavior.
- **Integrate data sources—** Vendor inventory and tiers, external attack-surface monitoring, vuln intel, SBOM repositories, identity logs, and ticketing.
- **Normalize ownership—** Map each supplier to an internal owner (procurement or vendor manager) and a technical owner, such as a system, application, or operational technology (OT) asset owner.
- **Define playbooks—** What happens when a signal triggers (i.e., triage, evidence request, access restriction, compensating controls, leadership notification).
- **Close the loop—** Route findings into tickets with SLAs, track remediation, and feed results into renewals, exceptions, and supplier scorecards.


## Pillar 4: integrated governance and joint incident readiness


*Prepare for the bad day, together.*


The fourth pillar of the framework is to operationalize joint readiness so that supplier-related incidents can be contained quickly and handled with transparency.


This includes establishing joint incident response playbooks, conducting quarterly resilience reviews for critical suppliers, and running cross-functional exercises that involve legal, procurement, information and operational technology, and operations. Business continuity plans should also be aligned with vendor recovery capabilities, supported by cross-functional vendor risk councils that ensure coordination and accountability.


To implement this, organizations should clearly define notification triggers, evidence expectations, and decision rights—including when to suspend supplier access. Tabletop exercises with critical suppliers should be conducted at least annually, formally tracking actions, owners, and due dates.


It’s also important to pre-stage secure communication channels for incident response and forensic data exchange. Finally, readiness should be measured through metrics—such as exercise completion rates, time to notify, and time to contain—and reported alongside broader supplier performance indicators.


“Operationalizing OT-specific supplier controls requires translating policy into enforceable technical and procedural safeguards.”


– Francesco De Lucia


Governance committees should be structured to drive decisions, not just discussions. A practical approach is a two-tier model, with an operational working group meeting weekly or biweekly and an executive steering group convening monthly or quarterly.


Clear decision rights are also essential. Define who has authority to accept exceptions, approve go-live, suspend supplier access, and trigger contractual remedies. Meetings should follow a disciplined, standing agenda focused on top supplier risks and signals, open remediation actions, upcoming renewals, expiring exceptions, and lessons learned from incidents.


The outputs must be tangible and actionable, including a prioritized risk register, a remediation tracker with defined owners and deadlines, and an executive dashboard highlighting key risk indicators, readiness metrics, and areas of supplier concentration risk.


Supplier access should be segmented and brokered through controlled pathways such as jump hosts, with unidirectional gateways used where appropriate, and identity-backed access secured with multifactor authentication, thereby eliminating direct inbound connections to control networks.


Privileged access should be timebound and closely monitored using just-in-time (JTI) provisioning, session recording, and controls on commands and file transfers. All changes must follow strict governance, including defined maintenance windows, tested rollback plans, and staged deployments for updates.


Organizations should also pre-stage incident response actions—such as “kill switch” procedures to disable vendor accounts, revoke certificates, or block update channels—and routinely validate that these controls function as intended. Recovery dependencies must be verified to ensure suppliers can support restoration during outages, including access to spare parts, field engineers, and license keys.


Finally, to ensure readiness, joint exercises should simulate OT-specific scenarios, such as loss of visibility or control, safety system interactions, and manual operations.


“Cross-functional collaboration underpins the entire security model and should be treated as a competitive advantage rather than an administrative burden.”


– Francesco De Lucia


Overall, enterprise risk frameworks, like those promoted by the National Institute of Standards and Technology (NIST), emphasize that clearly defined and distributed accountability is what enables faster, more controlled decision making. When these functions operate in alignment, organizations can move with greater speed and confidence, reducing risk while avoiding unnecessary bureaucracy.


## Focusing effort where it matters most


Grounded in current guidance from the World Economic Forum,[NIST’s Cyber Supply Chain Risk Management (C-SCRM)](https://csrc.nist.gov/projects/cyber-supply-chain-risk-management) framework, and the supply chain security models enforced by the European Union Agency for Cybersecurity, the four-pillar framework described above provides a practical and operational blueprint for governing cyber risk across increasingly interconnected supplier ecosystems.


Unlike traditional approaches built around annual questionnaires and siloed assessments, the model[integrates cybersecurity into every phase of the supplier life cycle](https://www.slb.com/insights/cybersecurity-the-next-frontier-of-safety-in-oil-and-gas) , from early criticality mapping and secure-by-design onboarding to continuous telemetry and joint resilience planning. This allows organizations to focus resources where dependencies pose the greatest operational impact, aligning risk management with the realities of today’s digital supply chains.
