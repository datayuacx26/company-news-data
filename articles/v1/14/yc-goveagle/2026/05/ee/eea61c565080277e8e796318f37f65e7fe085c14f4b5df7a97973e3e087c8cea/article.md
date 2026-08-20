---
schema_version: "1.0.0"
document_id: "eea61c565080277e8e796318f37f65e7fe085c14f4b5df7a97973e3e087c8cea"
company_key: "yc-goveagle"
company: "GovEagle"
source_id: "yc-goveagle-news-import-c64731ce0af9"
canonical_url: "https://www.goveagle.com/blog/itar-complete-guide"
published_at: "2026-05-18T00:00:00+00:00"
first_seen_at: "2026-07-21T21:57:49.307964+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:9d1c7a5c24728480b12250d6c5038b3c301699c483793cd2cd93cbee1352f6b8"
---

# International Traffic in Arms Regulations (ITAR): Complete Guide for May 2026

Managing ITAR requirements for employees across multiple proposals means you're constantly pulling the same assets from different repositories. Your facility certifications sit in one SharePoint site, your approved security language lives in another, your past performance references that show[ITAR compliance](https://www.goveagle.com/) are scattered across proposal archives, and your Technology Control Plan summaries need legal review every time you adapt them to a new solicitation. Let's walk through what ITAR actually controls, the documentation requirements that matter for proposals, and how to organize your compliance assets so they surface when your team needs them instead of three days after the bid ships.


**TLDR:**


- ITAR governs defense articles, services, and technical data on the USML, enforced by the State Department's DDTC.
- Violations carry civil penalties up to $1.3M per violation, criminal fines to $1M, and potential debarment from federal contracting.
- Registration through DECCS is required for manufacturers and exporters, even without physical exports, and must be renewed annually.
- Technical data controls require limiting access to U.S. persons or other authorized recipients, proper marking, and five-year recordkeeping for required ITAR export records.
- AI-driven proposal tools automate ITAR compliance matrices and retrieve security documentation from existing repositories with FedRAMP Moderate Equivalent controls.


## What ITAR Is and Why It Matters for Government Contractors


ITAR stands for International Traffic in Arms Regulations, a set of federal rules administered by the U.S. Department of State's Directorate of Defense Trade Controls (DDTC). Codified under[22 CFR Parts 120-130](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M) , ITAR governs the export and import of defense articles, defense services, and related technical data.


The goal is national security. These regulations keep sensitive defense technologies out of foreign adversaries' hands. If your company manufactures, exports, brokers, or handles data tied to items on the United States Munitions List (USML), ITAR applies to you. For government contractors, compliance reaches into hiring decisions, facility access, software choices, and how your team shares documents across every program.


## ITAR vs. EAR: Understanding the Key Differences


Both ITAR and the Export Administration Regulations (EAR) govern U.S. export controls, but they cover distinct categories of items and carry different compliance obligations.


ITAR, administered by the State Department's DDTC, covers defense articles, defense services, and technical data on the U.S. Munitions List. EAR, administered by the Commerce Department's Bureau of Industry and Security, covers dual-use goods and technologies with both commercial and military applications.


The core distinction is intent and control list. If an item appears on the USML, ITAR applies. If it falls under the Commerce Control List, EAR governs it.


Factor ITAR EAR


Governing agency State Department / DDTC Commerce Department / BIS


Control list U.S. Munitions List (USML) Commerce Control List (CCL)


Item focus Defense articles and services Dual-use goods and tech


License type DSP-5 and others Commerce export license, when required


Penalty severity Generally stricter Substantial but comparatively lighter


When an item transitions off the USML, it often moves to EAR99 or receives an Export Control Classification Number. Understanding which regulation applies is the first step in any export control review.


## Who Must Comply with ITAR Regulations


ITAR applies to any U.S. person who manufactures, exports, temporarily imports, or brokers defense articles, or furnishes defense services. That scope covers prime contractors, subcontractors, staffing firms, and software vendors handling controlled data.


Flow-down is where teams often get caught off guard. When a prime contract involves ITAR-controlled items, compliance requirements extend to every subcontractor and supplier in that chain. You cannot pass the obligation through contract language alone.


Registration may also be required without a single physical export. If your company manufactures defense articles or provides defense services domestically, DDTC registration may still apply regardless of whether anything leaves the country.


## ITAR Registration Requirements and Process


ITAR registration runs through the Defense Export Control and Compliance System (DECCS) portal, where companies file a DS-2032 Statement of Registration with the DDTC.


The trigger threshold is low. Companies engaged in the business of manufacturing, exporting, temporarily importing, or furnishing defense services generally must register with DDTC. Fees vary based on registration type and category and are assessed annually.


Registration is valid for one year. A lapsed registration is its own[ITAR violation](https://www.goveagle.com/blog/disqualified-govcon-compliance-errors) , independent of any other compliance issues on your program.


## The United States Munitions List: What's Controlled Under ITAR


The USML is divided into 21 categories, each covering a distinct class of defense articles. Category placement depends on an item's primary function, not whether a commercial equivalent exists.


For government contractors, these categories appear most frequently:


Category Coverage


IV Launch vehicles, guided missiles, and rockets


VIII Aircraft and related articles


XI Military electronics, including radar and EW systems


XII Fire control, laser, and optical equipment


XV Spacecraft and related articles


The "specially designed" standard is where teams frequently miscalculate. A component does not need to be a complete weapons system to qualify as ITAR-controlled. If it was designed or modified for a USML application, it falls under ITAR regardless of commercial availability. Teams working programs in any of these categories should verify classification before treating associated technical data as unrestricted.


## ITAR Compliance Requirements for Government Contractors


Government contractors working with defense-related items, services, or technical data face a specific set of ITAR compliance requirements that go beyond standard export control awareness.


### Registration


Manufacturers, exporters, temporary importers, and providers of defense services register under 22 CFR Part 122. Brokers have separate registration requirements under ITAR brokering rules. Registration is not optional, even if no exports have occurred.


### Employee and Visitor Controls


- Contractors must screen employees and visitors to prevent unauthorized access by foreign persons, as defined under 22 CFR 120.
- Visitor logs, access restrictions, and nationality verification are standard requirements.


### Technical Data Controls


Contractors must restrict access to ITAR-controlled technical data to U.S. persons or other authorized recipients covered by a license, agreement, or applicable exemption.


### Technology Control Plans


Many contracts require a written Technology Control Plan (TCP) outlining how the contractor will[protect ITAR-controlled items and data](https://www.goveagle.com/blog/compliance-matrix-automation-tools) throughout performance.


### Recordkeeping


Under 22 CFR 122.5, contractors must retain export-related records for[five years](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M/part-122/section-122.5) . Incomplete recordkeeping is one of the most cited causes of ITAR violations during audits.


## ITAR Penalties and Enforcement Actions


Civil penalties reach[$1,271,078 per violation](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M/part-127) , or twice the value of the underlying transaction, whichever is greater. Criminal charges can add fines up to $1 million and up to 20 years in prison per violation.


Debarment is a separate consequence and arguably the worst for contractors. It can bar a company from participating in ITAR-controlled defense trade and may create major federal contracting consequences until eligibility is restored.


DDTC may consider a voluntary disclosure as a mitigating factor when determining administrative action. Proactive disclosure is worth serious legal consideration if a potential violation surfaces internally.


## Common ITAR Violations and How to Avoid Them


ITAR enforcement actions reveal patterns that repeat across industries. Knowing where companies go wrong is the fastest way to build a defensible compliance posture.


Common violations include:


- Exporting or transferring defense articles to foreign nationals without the required license or applicable exemption, including employees working on-site.
- Sharing ITAR-controlled technical data via uncontrolled channels like personal email, consumer cloud storage, or unapproved collaboration tools.
- Missing or expired registrations, since ITAR registration must be renewed annually with DDTC.
- Failing to screen visitors, vendors, or subcontractors against the proscribed countries list before granting access to controlled areas or data.
- Inadequate recordkeeping, as ITAR requires retaining export-related records for five years.


Penalties for ITAR violations can reach[$1 million per violation](https://www.pmddtc.state.gov/) criminally and $1.3 million civilly, along with debarment from future government contracting.


## ITAR Technical Data and Documentation Requirements


Under ITAR, technical data includes information required for the design, development, production, manufacture, assembly, operation, repair, testing, maintenance, or modification of defense articles, such as blueprints, drawings, photographs, plans, instructions, and documentation.


Handling requirements apply at every touchpoint:


- Documents must be clearly marked as ITAR-controlled to put all handlers on notice of their obligations.
- Storage must restrict access to U.S. persons only, with access logs maintained for audit purposes.
- Transmission must occur through secured, approved channels that meet encryption standards.


The ITAR 120.54 carveout applies directly to how proposal teams store and share controlled content during development. It permits sending, taking, or storing unclassified ITAR technical data when the data is secured using end-to-end encryption, protected with compliant cryptographic modules, and is not intentionally sent to a person in or from a prohibited country or stored in a prohibited country. For GovCon teams using shared proposal workspaces, any provider access to plaintext data voids the exemption entirely, a risk worth confirming with your IT and legal team before an RFP drops.


## Building an Effective ITAR Compliance Program


A written compliance program is the foundation of any defensible ITAR posture. A documented compliance program may help mitigate enforcement risk if a violation surfaces, and prime contractors increasingly require documented programs before awarding subcontracts.


Core elements to build out:


- Risk assessment across your supply chain and active programs
- Written policies covering employee access, visitor controls, and technical data handling
- An appointed Empowered Official with direct DDTC authority
- A Technology Control Plan for each ITAR program
- Regular internal audits to catch gaps before enforcement does


## Managing ITAR Compliance in Government Proposals


Defense solicitations frequently ask contractors to show ITAR compliance within the proposal itself. Registration status, facility security infrastructure, and personnel citizenship qualifications all factor into source selection before technical evaluation begins.


Teaming decisions carry real ITAR exposure. A partner with a lapsed DDTC registration or inadequate nationality controls can compromise a proposal regardless of technical strength. Verify every teammate's registration status before submitting past performance references tied to ITAR programs.


For bid/no-bid decisions, ITAR readiness is a hard go/no-go. Missing facility certifications or citizenship gaps among proposed key personnel represent compliance costs worth calculating before committing pursuit resources.


## Simplifying ITAR Compliance Documentation with GovEagle


[ITAR documentation in proposals](https://www.goveagle.com/solutions/proposals) draws from many sources at once: past performance references, approved security language, facility certifications, and TCP summaries.[GovEagle's semantic search](https://www.goveagle.com/solutions/knowledge-management) retrieves those assets directly from existing SharePoint or Box repositories without data migration, so the right content surfaces in seconds instead of buried search sessions.


The[automated compliance matrix](https://www.goveagle.com/blog/compliance-matrix-guide-how-to-build) captures ITAR-specific requirements from Sections L and M, reducing the risk of citizenship controls or facility certifications going unaddressed before submission. Built-in access controls and audit logs support ITAR recordkeeping obligations throughout the proposal lifecycle, while FedRAMP Moderate Equivalent infrastructure on AWS GovCloud keeps controlled content protected at every stage of development.


## FAQs


### How do I verify if my subcontractor is ITAR compliant before teaming?


Request proof of DDTC registration directly from the subcontractor and screen the company against applicable restricted-party lists. A lapsed registration or missing facility controls can disqualify your entire bid regardless of technical strength.


### What are ITAR requirements for employees working on defense programs?


Employees must be U.S. persons or otherwise authorized to access ITAR-controlled technical data. You must maintain access logs, verify nationality before granting access, implement visitor controls for foreign persons, and retain export-related records for five years per 22 CFR 122.5.


### What's the fastest way to catch ITAR compliance gaps in my proposal before submission?


An automated compliance matrix that extracts citizenship requirements, facility certifications, and TCP obligations from Sections L and M prevents missing ITAR-specific mandates during review cycles. Semantic search across past ITAR proposals retrieves approved security language and facility certifications in seconds instead of manual document reviews.


## Final Thoughts on ITAR Compliance in Proposals


ITAR requirements for employees apply to your programs, but showing compliance in every proposal is where time disappears. Section L compliance matrices,[ITAR compliance](https://www.goveagle.com/) write-ups, and TCP summaries repeat across solicitations, yet teams rebuild them from scratch instead of reusing vetted content. GovEagle pulls that documentation directly from your existing repositories and maps it to each solicitation's requirements, so your team is ready when the RFP drops, not scrambling after submission. See a demo of how proposal managers retrieve ITAR language in seconds, not search sessions, while keeping everything audit-ready.
