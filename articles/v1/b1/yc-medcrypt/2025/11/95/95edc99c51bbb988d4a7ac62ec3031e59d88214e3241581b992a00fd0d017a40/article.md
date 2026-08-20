---
schema_version: "1.0.0"
document_id: "95edc99c51bbb988d4a7ac62ec3031e59d88214e3241581b992a00fd0d017a40"
company_key: "yc-medcrypt"
company: "MedCrypt"
source_id: "yc-medcrypt-news-import-56918f8bbce9"
canonical_url: "https://www.medcrypt.com/blog/jsp-risk-and-supplier-management-the-neighborhood-that-keeps-your-house-safe"
published_at: "2025-11-03T00:00:00+00:00"
first_seen_at: "2026-07-22T03:50:59.950031+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:ea3f66e9305a6b5e68b366ebe3d3610afc262d54218f6a431c2f2833e2b65d5d"
---

# JSP Risk & Supplier Management: The Neighborhood That Keeps Your House Safe

You’ve poured the foundation during the Concept phase, framed and wired during Design & Development, passed inspection in V&V, and set up a maintenance plan to keep everything in working order. But your house doesn’t stand alone. It sits within a neighborhood - one with codes, inspectors, suppliers, and utility providers.


In product security, that neighborhood represents Risk Management and Supplier Management - the guardrails and relationships that ensure your cybersecurity “home” stays safe and functional over its entire lifecycle. These two processes extend beyond individual projects. They define how an organization thinks about cybersecurity risk, how it manages third-party dependencies, and how it proves that every component of its system is trustworthy.


## **The Building Codes That Keep Every Home Standing: Risk Management (JSP2 Section B)**


Just as every city enforces construction codes to ensure structural integrity, Risk Management sets the standards your cybersecurity house must meet from design through decommissioning. Section B of the JSP outlines how manufacturers identify, evaluate, and control cybersecurity risks throughout the product lifecycle - distinguishing between design risks and known vulnerabilities, linking cybersecurity to patient safety, and documenting how all those risks are ultimately managed and approved.


The following three subsections - B.1 through B.3 - walk through the critical phases of this process: from assessing design risks and integrating security into safety, to compiling the final residual risk story that supports regulatory submission.


## **Inspecting the Framework: Security Design Risk Assessment (JSP2 Section B.1)**


Every neighborhood has building codes that keep homes structurally sound. For product security, those codes are your Risk Management processes, ensuring that cybersecurity risks are identified, evaluated, and controlled from concept through end of support.


At the core of this effort is the Security Design Risk Assessment, where you identify architectural weaknesses before they turn into structural failures. The JSP distinguishes two key risk categories: Security Design Risks and Known Vulnerabilities.


Security Design Risks are like hidden design flaws - an undersized beam or misrouted wiring that could compromise stability. These are discovered through threat modeling, architecture reviews, and design analysis, and are controlled through defined requirements and verification. Known Vulnerabilities, in contrast, are the product recalls of the cybersecurity world - publicly known issues with software or hardware components that could be exploited if left unaddressed.


Treating these categories separately matters. Each requires a distinct method of discovery, evaluation, and documentation. Regulators expect manufacturers to show that distinction clearly across their risk files, ensuring both kinds of risks are properly controlled.


In cybersecurity, attackers aren’t random failure modes, so traditional safety probability models don’t apply. Instead, risk is evaluated through exploitability (how feasible an attack might be) and impact (what happens if it succeeds). Together, these provide a more realistic view of security risk than probability and severity alone.


Throughout design, your threat model and Security Design Risk Assessment should evolve alongside the product. Each identified risk must trace to specific requirements and verification activities, ensuring what was planned gets validated. Think of it as keeping your blueprints aligned with what’s actually built.


When design changes occur - or when new vulnerabilities are discovered - the assessment should be updated. This ensures every potential weakness is managed with the same rigor as safety risks and that no new “cracks in the foundation” appear as development progresses.


## **Wiring Security Into Safety: Security Integration Into Safety Risk Assessment (JSP2 Section B.2)**


If the Security Design Risk Assessment is your building inspection, Security Integration into Safety Risk Assessment is your electrical wiring check - making sure all systems are connected safely and function as one.


The JSP makes it clear that cybersecurity and safety risks are not independent; they are deeply interconnected. Any cybersecurity issue that could lead to patient harm must flow into the safety risk management process.


For example, a denial-of-service attack that could prevent therapy delivery isn’t just a cybersecurity concern - it’s a safety hazard. In this context, the likelihood of occurrence is represented by exploitability, not traditional probability. This ensures that the unpredictable nature of human-driven attacks is accurately captured within the safety framework.


The integration is bidirectional. When new safety controls are introduced, they must be reassessed through a cybersecurity lens to ensure they don’t introduce fresh vulnerabilities. This linked process ensures consistency and prevents gaps between two disciplines that regulators increasingly expect to see merged.


A mature integration process means your risk files tell a single, cohesive story: how cybersecurity threats are identified, how they could impact patient safety, and how mitigations are implemented and verified across both systems.


## **Signing Off on the Build: Security Risk Management Summary and Approval (JSP2 Section B.3)**


By the time your house is ready for inspection, you’ve documented every code compliance, passed every test, and confirmed every connection. In the JSP, this milestone is represented by the Security Risk Management Summary and Approval.


This report consolidates your entire risk management story - from threat model through testing and mitigation - into a single, cohesive narrative. It includes the final residual risk determination for the product, combining the outcomes of your Security Design Risk Assessment, Known Vulnerability analysis, and verification results.


Leadership approval of this artifact is critical. It demonstrates that the organization understands the remaining cybersecurity risks and has formally accepted them before market submission. The FDA and auditors view this summary as the executive snapshot of your device’s security posture - clear, concise, and traceable to every supporting document.


Strong documentation here doesn’t just streamline submission - it ensures that lessons learned inform future products, strengthening the organization’s long-term security maturity.


## **Supplier Management: The Infrastructure That Powers the Neighborhood (JSP Section C)**


Even if your home meets every building code, it still relies on external utilities and quality materials to function. The same is true for medical devices. Section C of the JSP focuses on Supplier Management, describing how manufacturers manage cybersecurity risks introduced by third-party hardware, software, and service providers.


It outlines a lifecycle approach to Supply Chain Risk Management (SCRM) - starting with supplier evaluation and contracting (C.1), and extending into ongoing monitoring and performance management (C.2). Together, these activities ensure that the “materials” entering your house and the “utilities” keeping it running remain secure, supported, and trustworthy throughout the device’s lifespan.


## **Choosing Quality Lumber: Supplier Purchasing Process (JSP2 Section C.1)**


Even the strongest house can fail if the lumber is substandard or the utilities are unreliable. The Supplier Management process ensures every component - whether hardware, software, or service - is vetted for security and reliability before it ever enters your product.


Supplier evaluation starts early. Manufacturers must assess each supplier’s security posture and ensure they follow a secure development lifecycle, handle vulnerabilities responsibly, and maintain transparent communication channels. Contracts and SLAs should clearly define security deliverables: how and when vulnerabilities are disclosed, how incidents are reported, and how SBOMs are maintained and shared.


Privacy and compliance obligations, like Business Associate Agreements (BAAs) and Data Processing Agreements (DPAs), should be built into every contract where sensitive data may be accessed. For open-source or third-party software, licensing, export control, and intellectual property checks must be completed to prevent downstream issues.


Beyond initial assessment, manufacturers should maintain an Approved Supplier List (ASL) and a Supplier Risk Management Plan that outlines how suppliers are reviewed, monitored, and re-evaluated. Key triggers such as major software updates, ownership changes, or end-of-life notices should initiate formal re-assessments to ensure no weak links are introduced into the supply chain.


This proactive oversight creates accountability on both sides - ensuring that every supplier contributing to your cybersecurity house upholds the same standards you do.


## **Keeping the Lights On: Supplier Performance Management (JSP2 Section C.2)**


After your house is built, you don’t stop paying attention to your utilities - you monitor performance and respond when something fails. The Supplier Performance Management process ensures that vigilance continues throughout the device’s operational life.


This involves continuous monitoring of supplier vulnerabilities and advisories, verifying that updated SBOMs are provided with each release, and ensuring that new vulnerabilities are promptly disclosed and addressed. Supplier notifications should flow directly into your Product Security Incident Response Team (PSIRT) so that response actions can be taken quickly and efficiently.


Periodic supplier reviews verify that partners are meeting contractual obligations, maintaining secure practices, and communicating transparently. This also extends to tools and services used in the development environment - CI/CD platforms, code repositories, cloud infrastructure, and build systems. Any weakness in these can compromise the integrity of your product.


Strong supplier performance management ensures that your supply chain remains secure and reliable, turning vendor relationships into long-term security partnerships rather than transactional exchanges.


## **Who’s in the Room**


Risk and Supplier Management require collaboration across the organization. On the risk side, Product Security, Systems and Safety Engineering, Regulatory, Quality, and program leadership work together to align cybersecurity and safety documentation. On the supplier side, Procurement, Legal, Product Security, PSIRT, Quality, DevOps, and Product Owners coordinate to evaluate vendors, negotiate security terms, and monitor performance. Together, they form the neighborhood watch that keeps your cybersecurity house safe.


## **Why It Matters**


Getting these phases right creates three major benefits. First, regulatory clarity- showing that cybersecurity risks are distinct, traceable, and properly linked to safety risks. Second, operational resilience, ensuring predictable patch cycles, efficient incident handling, and less disruption when new vulnerabilities appear. And third, customer trust - providing confidence to hospitals and clinicians that your products are built and maintained with security at every level of the supply chain.


## **Designed for Life**


By the time you complete the Risk and Supplier Management phases, you should have a living, traceable record of how cybersecurity risks are identified, evaluated, and controlled. Your Security Design Risk Assessment should capture architectural weaknesses and mitigation strategies, your Known Vulnerability records should document exploitability and impact, and your Security Risk Management Summary should be signed off by leadership.


For suppliers, your ecosystem should be transparent and verifiable - contracts that set clear expectations, ongoing monitoring of supplier vulnerabilities, current SBOMs for every release, and evidence that supplier performance is actively reviewed.


These artifacts together form the unseen framework of trust that supports your entire cybersecurity house.


## **Securing Your Legacy**


Risk Management is your building code - the set of rules ensuring your structure is sound and safe. Supplier Management is your materials and utilities assurance - the confidence that what enters your home is reliable and secure. Keep both active and aligned, and your cybersecurity house won’t just stand - it will thrive in a complex, interconnected neighborhood.
