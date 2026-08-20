---
schema_version: "1.0.0"
document_id: "92dcc983032df9bbdc032069bc8f748f3faa62e634416793eb8e6d375088d2ad"
company_key: "yc-jiga"
company: "Jiga"
source_id: "yc-jiga-news-import-07676ebb04d8"
canonical_url: "https://jiga.io/articles/itar-compliance-manufacturing/"
published_at: "2026-08-18T15:38:23+00:00"
first_seen_at: "2026-08-20T01:58:45.018415+00:00"
fetched_at: "2026-08-20T01:58:46.131739+00:00"
content_hash: "sha256:c11167e4a81a5e4facb57ee588d76e98f99f86ed3715aacdd892644c7db96a76"
---

# Maintaining ITAR compliance when sourcing custom metal components

Your CNC supplier may be ITAR registered.


So may your sheet metal fabricator.


Your heat treater, plating vendor and inspection house may all appear qualified.


Yet your program can still suffer an ITAR violation.


For many defense and


[aerospace manufacturing programs](https://jiga.io/aerospace-and-defense) , compliance does not fail because a supplier lacks registration. It fails because controlled technical data, manufacturing responsibility or traceability breaks somewhere between engineering release and final delivery.


Perhaps a controlled CAD model is emailed to an overseas applications engineer for manufacturing advice. A plating process is subcontracted to a vendor outside your approved network. A design revision is shared through an unmanaged cloud folder. Material certificates become separated from production records during a supplier change.


None of these failures originate with the machined component itself. They originate when control over technical data, supplier responsibility, engineering configuration, or


[DFMA](https://jiga.io/articles/dfma-techniques) is lost.


They occur because compliance is a process that must be maintained across an entire


[manufacturing qualification](https://jiga.io/articles/?u) supply chain.


That distinction is largely missing from current guidance. Most articles explain what ITAR is or provide directories of ITAR-registered manufacturers. Few explain how engineers and supply chain teams preserve compliance once a controlled drawing passes through multiple companies, subcontractors and production operations.


This article focuses on that operational reality. It explains where compliance most commonly breaks, why registration alone is not enough, how ITAR differs from DFARS and other quality standards, and how maintaining a single supplier of record can help preserve compliance throughout the manufacturing lifecycle.


This CNC machined Aluminum mounting point and bearing pillow is referenced throughout the following article, to illustrate the ITAR compliance process in a real part.


ITAR compliance begins with tight control of technical data.


## Registration is not certification


The first misunderstanding appears before manufacturing even begins.


Many supplier websites proudly advertise themselves as


**“ITAR Certified.”**


That term has become so widespread that even AI-generated answers and procurement articles repeat it.


It is also incorrect.


There is


**no government-issued ITAR certification.**


Under the International Traffic in Arms Regulations (ITAR), manufacturers producing defense articles listed on the United States Munitions List (USML) register with the


**U.S. Department of State’s Directorate of Defense Trade Controls (DDTC)** in accordance with


**22 CFR Part 122** . Registration is required for manufacturers of USML-controlled articles, even if they never export a single component.


Registration enables an organization to participate in regulated defense work and apply for export authorizations where required.


It does


**not** certify that the company has mature compliance systems, secure handling of technical data or effective control over subcontractors.


Those responsibilities remain with the manufacturer.


Just as importantly, they remain with you as the engineering organization selecting and managing that supplier.


There are many ways in which non compliant actions can be present in a production process, so high vigilance is required in order to avoid downstream audit surprises, component recertifications and potentially damaging and costly component recalls.


## What "ITAR registered" actually tells you


An ITAR registration provides useful information.


It demonstrates that the organization has:


- Registered with DDTC.


- Appointed an Empowered Official.


- Accepted responsibilities under ITAR.


- Become eligible to participate in controlled defense work where appropriate.


It does


**not** tell you:


- who has access to your CAD files,


- how subcontractors are managed,


- whether technical data is encrypted,


- whether engineering revisions remain controlled,


- whether inspection records are retained correctly,


- or whether every supplier in the manufacturing chain is equally compliant.


Those questions determine whether compliance survives production.


Registration simply establishes the starting point. Registration answers whether a supplier may participate in controlled defence work. It does not answer whether they can manage your programme compliantly.


### Compliance is a supply chain activity


Many engineers naturally think about ITAR as applying to finished hardware.


In practice, the more valuable asset is often the technical data.


- A STEP file.


- A drawing.


- A tolerance stack.


- A GD&T scheme.


- A manufacturing process specification.


- A heat treatment procedure.


In many defence programmes, these files are more valuable than the finished component because they define how identical hardware can be reproduced. Each may constitute controlled technical data under ITAR depending on the program and classification.


That means the compliance chain begins long before the first chip is cut.


The engineering release itself often represents the most sensitive point in the manufacturing process.


**CTA**


Before releasing a controlled drawing for quotation, verify not only that the intended supplier is ITAR registered, but also that every planned manufacturing operation and data transfer follows the same compliance controls.


## Where compliance actually breaks in a sourced supply chain


Most export violations do not begin with malicious intent.


They begin with ordinary manufacturing activities carried out without recognising their compliance implications.


The highest-risk points are usually the least dramatic.


They occur during quoting, subcontracting, revision management and day-to-day engineering communication rather than during shipment of the finished component.


### Deemed exports through technical data


One of the most common compliance failures involves the concept of a


**deemed export** . For example, sharing a controlled CAD model during a Teams call with an overseas applications engineer may constitute a deemed export depending on the programme.


Under ITAR, releasing controlled technical data to a non-U.S. person within the United States can constitute an export, even though the information never physically leaves the country. Controlled data includes CAD models, engineering drawings, manufacturing instructions, inspection plans and test procedures.


Common examples include:


- emailing controlled drawings to an overseas engineering office,


- sharing CAD through an unmanaged cloud drive,


- granting supplier-portal access without nationality controls,


- screen-sharing controlled geometry during technical support,


- allowing unrestricted access to revision-controlled manufacturing documentation.


In many organisations these activities happen every day.


Without appropriate controls, they can unintentionally create export violations despite the hardware never leaving the facility.


### Flow-down to sub-tier suppliers


A machined component rarely remains within a single facility.


After machining it may require:


- heat treatment,


- anodising,


- plating,


- painting,


- passivation,


- NDT,


- precision grinding,


- laser marking,


- dimensional inspection.


Each operation introduces another organisation into the compliance chain.


If one subcontractor lacks appropriate controls, the integrity of the entire manufacturing process may be compromised.


This is particularly important because subcontractors are often selected by the primary machine shop rather than by the customer.


Unless those flow-down requirements are actively managed, engineering organisations may not even know which companies handled their controlled hardware or technical data.


One of the most valuable functions of a supplier of record is ensuring these downstream manufacturing steps remain within a qualified and auditable network rather than becoming invisible parts of the sourcing process.


### Traceability and record retention


Even when technical data remains secure, compliance can still fail if records cannot demonstrate what happened during production.


Documentation should allow the complete reconstruction of the complete manufacturing history, years after production, not merely satisfy today’s receiving inspection.


ITAR requires records associated with regulated activities to be retained for


**at least five years** . That requirement extends beyond shipping documents to include export authorizations where applicable, technical data transfers, manufacturing records and supporting documentation.


For custom metal components, an audit-ready production record typically includes:


- Revision-controlled engineering drawings


- Material certificates


- Certificates of Conformance (CoCs)


- Heat treatment and plating certifications


- First Article Inspection Reports (FAIRs) where applicable


- Lot and serial number traceability


- Approved supplier records


- Records of engineering changes and disposition


The value of these documents is cumulative. A material certificate without revision-controlled drawings, or a


[FAIR](https://jiga.io/articles/first-article-inspection) without traceability to the production lot, leaves gaps that complicate compliance during customer audits or government reviews.


Maintaining those records across multiple suppliers is considerably more difficult than maintaining them within a single manufacturing facility. This is one reason many defence programs prefer a single supplier of record responsible for managing documentation throughout the production lifecycle.


### Engineering changes can introduce new compliance risks


Compliance is not frozen when the first revision of a drawing is released.


Engineering changes often create entirely new compliance obligations. The bearing mount used as an example can become ITAR-controlled after adding a defence-specific mounting feature or mission-specific tolerance requirement.


Examples include:


- adding a defence-specific feature to a previously commercial component,


- introducing controlled manufacturing instructions,


- modifying a commercial design so it becomes


**“specially designed”** under ITAR definitions,


- changing manufacturing processes that require additional controlled technical data,


- introducing new subcontractors to support production capacity.


Each revision should therefore be evaluated not only for manufacturing impact, but also for export-control implications.


A seemingly minor geometry change may alter how technical data is classified or who may legally access it during production.


Configuration control becomes both an engineering discipline and a compliance discipline.


## ITAR, DFARS and the standards engineers commonly confuse


Another source of confusion is the number of standards that appear together during supplier qualification.


Many organisations request ITAR registration, AS9100 certification, CMMC compliance and DFARS clauses simultaneously.


Although these requirements often apply to the same supplier, they address very different risks.


Understanding those differences helps avoid unnecessary supplier qualification issues and ensures the correct questions are asked during sourcing.


Requirement Controls


ITAR Technical data & export


DFARS Defence procurement & materials


AS9100 Quality system


CMMC Cybersecurity


NADCAP Special processes


### ITAR


ITAR regulates the manufacture, handling and export of defence articles and controlled technical data under the Arms Export Control Act and 22 CFR Parts 120 to 130.


Its primary concern is controlling who can access defence-related products and technical information.


Typical questions include:


- Is this component listed on the USML?


- Who has access to the CAD model?


- Has technical data been released to a non-U.S. person?


- Are export licences required?


ITAR focuses on technical data and export control, not manufacturing quality.


### DFARS


DFARS is frequently mentioned alongside ITAR, but its scope is different.


For manufacturing engineers, one of the most relevant provisions is DFARS 252.225-7009, covering specialty metals for defence procurement.


Unlike ITAR, DFARS can restrict the source of raw materials used in defence hardware.


For example, a component may satisfy every ITAR requirement yet still fail DFARS compliance if the specialty metal originates from an unapproved source.


The two requirements overlap operationally but solve different problems.


ITAR controls information.


DFARS may control material sourcing.


Treating them as interchangeable can lead to costly procurement errors.


### AS9100D


AS9100D is a quality management standard for aerospace manufacturing.


It establishes systematic controls for production, risk management, corrective action and continuous improvement.


An AS9100D-certified machine shop may have an excellent quality system while having no authority to manufacture ITAR-controlled components.


Conversely, an ITAR-registered supplier is not automatically AS9100D certified.


One governs quality.


The other governs export control.


### NIST SP 800-171 and CMMC


Modern defence manufacturing increasingly depends on digital engineering data.


NIST SP 800-171 establishes cybersecurity controls for protecting Controlled Unclassified Information (CUI), while the Cybersecurity Maturity Model Certification (CMMC) provides a framework for verifying those controls across the defence industrial base.


These standards complement ITAR by reducing the likelihood that controlled technical information is exposed through insecure IT systems.


They do not replace export-control obligations.


### NADCAP, FAIR and PPAP


Other standards frequently appear in defence manufacturing, each serving a specific purpose.


- NADCAP accredits special processes such as heat treatment, welding and chemical processing.


- AS9102 First Article Inspection (FAIR) verifies that the first manufactured component conforms to engineering requirements.


- PPAP demonstrates production readiness, primarily within automotive manufacturing, but sometimes adopted elsewhere for structured process approval.


Each contributes to manufacturing confidence.


None substitutes for maintaining ITAR compliance throughout the sourcing process.


#### A practical qualification checklist


When evaluating suppliers for controlled defence work, consider asking:


- Can you show me an example of your technical-data access controls?


- Describe how obsolete revisions are withdrawn.


- Are you currently registered with DDTC under ITAR?


- Who controls access to customer technical data?


- How are sub-tier suppliers approved and monitored?


- How are engineering revisions distributed and withdrawn?


- What records are retained, and for how long?


- Can you demonstrate full material and process traceability?


- How are controlled CAD files stored, transmitted and archived?


- Who remains accountable if production moves between qualified shops?


These questions reveal considerably more about operational compliance than a registration number displayed on a website.


**CTA**


During supplier qualification, review compliance workflows with the same discipline used for DFM, capability studies and quality planning. Strong manufacturing outcomes depend on strong information control as much as machining capability.


## A sourcing workflow that maintains ITAR compliance


Maintaining ITAR compliance is not a single approval step.


It is a controlled workflow that begins when engineering releases a drawing and continues until production records are archived.


Every transition introduces potential risk:


- A drawing released for quotation.


- A CAD model transferred to manufacturing.


- A machining operation subcontracted for heat treatment.


- A revised drawing replacing an obsolete revision.


- A finished component moving to inspection and shipment.


The objective is not simply to prevent violations.


It is to ensure that every movement of technical data and every manufacturing process remains visible, controlled and auditable.


An effective sourcing workflow typically follows six stages.


### 1. Qualify the manufacturing network


Qualification begins before a request for quotation is issued.


Confirm that suppliers possess the capabilities required for both manufacturing and compliance.


Depending on the programme, this may include:


- DDTC registration where required


- AS9100D certification


- NADCAP accreditation for special processes


- NIST SP 800-171 or CMMC implementation


- Demonstrated traceability systems


- Secure handling of controlled technical data


Capability should be assessed alongside compliance.


An excellent five-axis machine shop is not automatically an appropriate supplier for controlled defence work.


### 2. Control technical data access


Engineering information is often more sensitive than the finished component.


Every release should answer questions such as:


- Who receives the CAD model?


- Which revision is current?


- Who may download manufacturing files?


- Are obsolete revisions withdrawn?


- Are suppliers prohibited from forwarding data without authorisation?


Many organisations focus heavily on controlling hardware while overlooking digital information.


Increasingly, the greater compliance risk lies in uncontrolled electronic distribution.


### 3. Control the manufacturing chain


Production rarely finishes where it starts.


Typical secondary operations include:


- heat treatment,


- passivation,


- anodising,


- painting,


- grinding,


- coating,


- non-destructive testing,


- laser marking.


Each introduces another organisation that may require access to controlled technical data.


Those organisations should be approved before work begins rather than discovered during an audit.


Maintaining an Approved Vendor List (AVL) for both primary and sub-tier suppliers helps ensure compliance remains intact throughout the production process.


A controlled manufacturing supply chain means every partner, every transfer, every action is fully controlled, traceable and documented for ITAR compliance


### 4. Maintain complete traceability


Every manufacturing event should remain connected to the finished part.


That includes:


- material heat numbers,


- process certifications,


- inspection reports,


- Certificates of Conformance,


- First Article Inspection Reports,


- engineering revisions,


- approved deviations,


- serial or lot numbers.


If one document becomes disconnected from the production history, demonstrating compliance becomes significantly more difficult. Every document should identify not only what happened, but who performed the work, when it occurred and which engineering revision governed the operation.


Traceability should therefore be viewed as an engineering requirement rather than simply an administrative exercise.


### 5. Audit continuously


Supplier qualification is not permanent.


- Manufacturing capability evolves.


- Personnel change.


- Subcontractors change.


- Cybersecurity requirements develop.


Audit programmes should periodically verify that suppliers continue to operate in accordance with programme requirements.


Typical audit topics include:


- technical data handling,


- revision control,


- subcontractor approval,


- records retention,


- corrective actions,


- cybersecurity controls.


Continuous verification is considerably more effective than assuming compliance remains unchanged after initial qualification. Compliance should be treated as a continuously verified process rather than a one-time supplier qualification exercise.


### 6. Retain records


The final production shipment does not conclude compliance responsibilities.


Records supporting regulated activities generally must remain available for the required retention period, allowing future audits or investigations to reconstruct exactly what occurred during manufacturing.


A production package should be sufficiently complete that another qualified engineer can determine:


- which


[drawing revision](https://jiga.io/articles) was manufactured,


- which suppliers performed each process,


- which materials were used,


- which inspections were completed,


- and how every controlled manufacturing step was authorised.


That level of documentation protects both the customer and the manufacturer.


Central to ITAR compliance is the control of access to part data, as many aspects will contain state and client/application trade secrets and protected capabilities. Unauthorized alterations and document substitutions are extreme barriers to compliance


## AUKUS and allied-country manufacturing


AUKUS reduces friction for certain defence trade activities, but it does not remove programme-specific export-control obligations.


Recent amendments supporting AUKUS defence cooperation, including changes to 22 CFR 126.7 and 126.18 that became effective on 30 December 2025, have streamlined certain defence trade arrangements among the United States, Australia and the United Kingdom.


These changes are significant because they recognise the increasingly integrated nature of allied defence manufacturing.


However, they should not be interpreted as eliminating export-control responsibilities.


Programme classification, technical data controls, licensing requirements and customer-specific obligations still determine how information and hardware may be transferred.


For engineering organisations, the practical lesson remains unchanged:


Every programme should be evaluated against its own regulatory requirements rather than assuming all allied-country sourcing follows identical rules.


## One point of accountability beats a directory of logos


Many sourcing teams begin with a directory of suppliers.


Each shop displays impressive credentials:


- ITAR registered.


- AS9100D certified.


- NADCAP approved.


- ISO 9001 certified.


Yet no single organisation owns the compliance record once production moves between machining, finishing, inspection and assembly.


That fragmented responsibility creates risk.


Every additional supplier relationship increases the number of technical-data transfers, engineering revisions, quality records and subcontracting decisions that must be managed.


A supplier of record changes that model.


Instead of managing multiple independent suppliers, the engineering team works through one accountable manufacturing partner responsible for coordinating a qualified network while maintaining visibility across the complete manufacturing chain.


The customer retains engineering authority, Approved Vendor List requirements and audit rights.


The supplier of record assumes operational responsibility for ensuring approved manufacturers, controlled technical data, production records and traceability remain connected throughout the programme.


For complex defence hardware, that continuity is often more valuable than simply locating another qualified machine shop.


**CTA**


Jiga operates as a supplier of record for custom metal components, coordinating qualified manufacturing partners while maintaining engineering accountability, traceability and compliance across the production lifecycle.


## Conclusion


Maintaining ITAR compliance is an operational discipline, not a registration status.


While DDTC registration establishes a manufacturer’s eligibility to participate in controlled defence work, it does not by itself protect engineering data, control subcontractors or maintain traceability through production.


Those responsibilities persist every time a drawing is released, a supplier is added, a manufacturing process changes or a component moves between facilities.


For mechanical engineers and supply chain leaders, successful compliance depends on controlling the complete manufacturing chain:


- qualified suppliers,


- controlled technical data


- approved sub-tier processors,


- documented engineering changes,


- complete traceability,


- audit-ready records.


As defence supply chains become increasingly distributed, maintaining one accountable organisation responsible for coordinating those activities becomes just as important as selecting capable manufacturers.


The objective is straightforward:


In defence manufacturing, compliant hardware is only half the deliverable. The other half is demonstrable control over every piece of technical data, every manufacturing step and every record that proves the component was produced in accordance with ITAR.


## Frequently Asked Questions


Is there an ITAR certification?


No. The U.S. government does not issue an ITAR certification. Manufacturers register with the Directorate of Defense Trade Controls (DDTC) under


**22 CFR Part 122** . Registration should not be confused with certification.


What is a deemed export?


A deemed export occurs when controlled technical data is released to a non-U.S. person, even if the disclosure takes place within the United States. CAD models, engineering drawings, manufacturing instructions and test procedures may all constitute controlled technical data depending on the programme.


Does ITAR control where raw materials come from?


Not generally. ITAR regulates defence articles and technical data. Requirements relating to the country of origin of specialty metals are typically governed by


**DFARS** , particularly clauses such as


**DFARS 252.225-7009** , rather than ITAR itself.


How long must ITAR records be retained?


Records associated with regulated activities generally must be retained for


**at least five years** , enabling future audits and demonstrating compliance throughout the manufacturing process.


Why use a supplier of record for ITAR-controlled manufacturing?


A


[supplier of record](https://jiga.io/) provides a single point of operational accountability across a qualified manufacturing network. This helps maintain technical-data control, supplier flow-down, traceability and documentation throughout the production lifecycle, reducing the risk of compliance gaps as components move between multiple manufacturing operations.
