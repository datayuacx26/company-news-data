---
schema_version: "1.0.0"
document_id: "7869fddca629903546b96014684c76b40180386049bcb4c31afe68a4f63a0533"
company_key: "yc-cofactr"
company: "Cofactr"
source_id: "yc-cofactr-news-import-06c16db4e4eb"
canonical_url: "https://www.cofactr.com/articles/a-practical-guide-to-itar-compliance-for-manufacturers-and-engineers"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-27T08:34:13.149498+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:83104cec3bd6b69d415620b08cf825022b47409fe4ac091ed598ee41c1186603"
---

# A Practical Guide to ITAR Compliance for Manufacturers and Engineers

##


Buying parts, placing boards, and sending build packages around for aerospace, defense, or space hardware brings ITAR into the conversation earlier than most teams expect. You can order resistors without becoming an export lawyer. The useful skill is spotting when a drawing, part, assembly, supplier, or employee access request could create a regulated export.


## What ITAR Is


ITAR stands for the International Traffic in Arms Regulations. In plain English, it is the U.S. export-control rulebook for defense articles, defense services, and related technical data listed on the United States Munitions List, usually shortened to USML. The regulations live in 22 CFR Parts 120-130, with the[USML itself in 22 CFR Part 121](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M/part-121) .


The basic rule is blunt: technical information about an article on the USML cannot be disclosed to a non-U.S. person unless the transfer is authorized.


ITAR generally creates two categories of export risk:


- Export: sending controlled hardware or technical data to another country. Emailing CAD files to a supplier in the UK, uploading controlled files to an overseas engineering office, or shipping a defense-related assembly overseas all fall into this category.
- Deemed export: releasing controlled technical data to a non-U.S. person inside the United States. The file can stay in the building, the employee can sit on your payroll, and the disclosure can[still count as an export](https://www.govinfo.gov/content/pkg/CFR-2018-title22-vol1/pdf/CFR-2018-title22-vol1-part120.pdf#:~:text=,con%02sulate%2C%20in%20the%20United%20States) under ITAR.


Two definitions carry a lot of weight here:


- U.S. person: generally a U.S. citizen, lawful permanent resident, protected individual, or U.S. entity.
- Non-U.S. person: often called a foreign person in the regulations, this includes foreign individuals, foreign companies, foreign governments, and people in the United States on many common work visas.


Disclosure can be an email, a shared CAD model, a drawing review, a screen-share, a supplier portal login, or a hallway conversation that gets too specific. ITAR does not need a shipping label to care.


Once technical data is identified as ITAR-controlled, the handling burden shifts onto the company sharing it. Controlled files are typically marked with export-control notices or ITAR legends, access restrictions are documented, and transfers are limited to authorized recipients. Those controls become important evidence that the company recognized the data as controlled and communicated handling requirements to downstream parties.


Manufacturers and engineering teams usually run into trouble around technical data.[ITAR technical data](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M/part-120) includes information required to design, develop, produce, manufacture, assemble, operate, repair, test, maintain, or modify a defense article. CAD models, drawings, schematics, source code, manufacturing notes, test procedures, and repair instructions can all qualify.


An engineer screen-sharing a controlled PCB layout with a foreign supplier can create an export event. A cloud drive synced to an overseas engineering office can create an export event. A non-U.S. person or employee accessing controlled files from inside your U.S. office can create deemed exports.


## What ITAR Is Not


The government does not issue an “ITAR certification.”


A company may be registered with[DDTC](https://www.state.gov/bureaus-offices/under-secretary-for-arms-control-and-international-security-affairs/bureau-of-political-military-affairs/directorate-of-defense-trade-controls-pm-ddtc) , the Directorate of Defense Trade Controls. Registration tells the government who is in the defense trade business, while exports still require their own authorization.


Registration simply means the company has identified itself to the State Department as a manufacturer, exporter, temporary importer, or broker of defense articles or defense services. Registration allows the company to[apply for export licenses and agreements](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M/part-122) .


That distinction matters because people throw around phrases like “ITAR certified warehouse” or “ITAR certified manufacturer” all the time. Usually what they mean is:


- The company is DDTC registered
- They have some export compliance procedures
- They probably have restricted access controls
- Hopefully somebody there knows what a DSP-5 is


Those details are useful due-diligence signals, but compliance depends on the actual controls, records, authorizations, and day-to-day behavior. Every export, reexport, technical-data transfer, or defense-service activity still needs proper authorization unless an exemption applies.


Read More:[Should Your 3PL Be ITAR/EAR Compliant?](https://www.cofactr.com/articles/should-your-3pl-be-itarear-compliant)


## What ITAR Registration Actually Means


Under 22 CFR Part 122, U.S. companies involved in manufacturing defense articles or furnishing defense services generally must register with DDTC, even if they never export anything.


That surprises a lot of engineering teams.


Domestic manufacturing of controlled hardware can trigger registration requirements before any international shipment enters the plan.


Electronics companies can run into ITAR scope through:


- Military electronics
- Spacecraft hardware
- Ruggedized aerospace assemblies
- Certain RF systems
- Defense-related sensors
- Controlled software
- Specially designed components for USML systems


The “specially designed” language in ITAR is broad enough to make experienced compliance people sweat a little. A commercial component modified specifically for a military system can become ITAR-controlled[depending on the application and classification](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M/part-120) .


Registration also requires appointing an[Empowered Official](https://www.ecfr.gov/current/title-22/chapter-I/subchapter-M/part-120/subpart-C/section-120.67) , usually somebody authorized to certify export-license submissions and bind the company legally.


Once registered, companies can apply for licenses and agreements such as:


- DSP-5 permanent export licenses
- Technical Assistance Agreements (TAAs)
- Manufacturing License Agreements (MLAs)
- Temporary export/import authorizations


Those approvals govern the actual transfer activity. Registration only gets you into the system.


## How ITAR Is Enforced


DDTC enforcement has real teeth.


Violations can trigger civil penalties, criminal penalties for willful misconduct, seizure actions, and debarment from export activity.


The recent enforcement actions are large enough to get everyone’s attention:


- [Boeing](https://2021-2025.state.gov/u-s-department-of-state-concludes-51-million-settlement-resolving-export-violations-by-the-boeing-company/) resolved alleged AECA and ITAR violations with a $51 million settlement.
- RTX resolved 750 alleged violations with a $200 million settlement.
- GE Aerospace resolved alleged violations with a $36 million settlement.
- Precision Castparts agreed to a $3 million civil penalty plus compliance obligations.


The Boeing case is particularly educational because many violations involved technical-data access and classification failures. The real-world version of export control trouble is usually much more mundane than a spy thriller.


In real life, ITAR problems usually look like routine process failures. An engineer uploads files to the wrong server. A supplier gets copied on an email thread without proper authorization. Somebody assumes EAR99 because the hardware “looks commercial.” Then six months later legal finds out during due diligence.


Voluntary disclosures matter here. DDTC specifically encourages companies to self-report violations promptly, and cooperation can reduce penalties substantially.


Voluntary disclosure can turn a terrible problem into a more manageable one.


## Common Ways Teams Accidentally Violate ITAR


Most ITAR mistakes begin inside ordinary engineering workflows: a supplier quote, a shared folder, a design review, a quick Slack message, or a contractor who needs access to finish the job. The practical risk lives in the spots where controlled technical data can leak during normal work.


### Emailing CAD Files to Foreign Suppliers


Sending controlled drawings or CAD files to a foreign supplier without authorization is an export, regardless of whether the supplier ever manufactures the part.


That includes suppliers in allied countries such as the UK, Canada, or NATO partner nations. Friendly countries may qualify for specific exemptions, agreements, or streamlined licensing paths, but ITAR still treats the transfer as controlled activity that requires authorization analysis before the files move.


### Giving Non-U.S. Person Employees Access


Employee status and ITAR status are separate questions. A person can work for your company, sit in your U.S. office, and still be a non-U.S. person for ITAR purposes. H-1B employees are a common example.


Controlled technical data needs access controls based on ITAR authorization, not just job title or project need. Giving a non-U.S. person employee access to controlled drawings, CAD files, source code, test data, or manufacturing instructions can create a deemed export violation.


### Cloud Storage Problems


Cloud systems are a compliance minefield when configured poorly.


Non-U.S. person access to ITAR-controlled files in Slack, GitHub, Jira, PLM systems, shared drives, Teams, or cloud storage can create export or deemed export risk.


The practical control is pretty simple: ITAR files should stay out of general-purpose collaboration systems unless that system, or a clearly separated part of that system, is configured and managed as ITAR-controlled.


AWS GovCloud is a common example of a cloud environment designed to support ITAR-controlled workloads, but the platform choice only helps when the tenant, permissions, logging, and operating procedures are configured correctly. That means access is limited to authorized users, permissions are reviewed, audit logs are available, and the company can demonstrate that non-U.S. persons cannot access the controlled workspace.


Teams often assume “the server is in the U.S.” solves the problem. In many situations, access control matters more than the physical rack location.


### Third-Party and Supply Chain Risk


ITAR risk follows the data into the vendor base. A U.S.-based supplier, CM, warehouse, 3PL, SaaS vendor, MSP, or engineering contractor can still create exposure if its employees, support teams, subcontractors, or offshore affiliates can access controlled technical data.


The practical question is: who can see the files, where can they see them from, and what prevents unauthorized access? A supplier NDA helps, but it is only paperwork. The operating controls matter more: written ITAR handling requirements, permission boundaries, subcontractor restrictions, audit logs, user reviews, data-location controls, and a clear process for incident reporting.


This is also where “sloppy vendor” risk becomes very real. If a company sends controlled technical data to a supplier with weak controls, broad internal access, unmanaged subcontractors, or overseas support teams, regulators are going to ask hard questions about vendor diligence and oversight. Marking files correctly and communicating handling requirements help establish that the data was treated as controlled, but companies are still expected to exercise reasonable care over where the data goes and who can access it.


This is especially important with supplier portals and PLM access. A supplier may be physically located in the United States while relying on overseas quoting teams, IT administrators, CAM engineers, or customer-support staff. Controlled drawings in that environment can create export or deemed export risk unless access is restricted to authorized users.


### AI Tools and Design Assistants


AI tools deserve the same suspicion as cloud storage, plus a little extra side-eye. Pasting controlled drawings, source code, test results, failure-analysis notes, or manufacturing instructions into an AI chatbot can disclose technical data to the tool provider or its systems. AI-enabled CAD, EDA, PLM, ticketing, quoting, and documentation tools raise similar questions when they ingest or process controlled files.


AI meeting assistants and automatic note-taking tools deserve attention too. Zoom, Teams, Meet, and similar platforms increasingly generate transcripts, summaries, action items, searchable notes, and AI-generated recaps automatically. A meeting discussing controlled technical details can create ITAR exposure if those transcripts, recordings, summaries, or embeddings become accessible to non-U.S. persons, overseas support teams, or AI training systems.


Before ITAR data goes into an AI system, confirm the basics: who can access the input and output, whether the provider uses customer content for model training, where the data is stored and processed, whether support personnel can view it, how logs are retained, and whether the environment is contractually and technically restricted for ITAR-controlled work. Enterprise privacy terms may address training and ownership, but ITAR review still needs recipient, access, retention, admin, subcontractor, and data-location controls.


A safe default is to keep ITAR technical data out of general-purpose AI tools unless the tool has been approved for controlled data by your export-compliance owner. The phrase “AI design assistant” should make compliance people lean forward in their chairs.


### “We Didn’t Ship Hardware”


This misconception causes endless trouble.


ITAR controls technical data and defense services in addition to physical hardware. A Zoom call discussing controlled engineering details with foreign attendees can create an export event.


A conference presentation can create an export event.


A screen-share session can create an export event.


### Misclassifying ITAR Items as EAR-Controlled


This is one of the classic disaster paths.


A team assumes a part is[EAR99](https://www.trade.gov/eccn-and-export-administration-regulation-ear99) or ordinary dual-use commercial hardware when it actually falls under a USML category. EAR99 is the catch-all classification for many low-risk commercial items regulated under the Export Administration Regulations, or EAR, rather than ITAR. Engineers sometimes see “commercial-looking” hardware and assume EAR99 automatically applies. That assumption can become very expensive when the item is actually USML-controlled and requires State Department authorization before export.


Most classification questions should be handled through a documented internal review with export counsel when needed. A[Commodity Jurisdiction](https://www.pmddtc.state.gov/ddtc_public?id=ddtc_kb_article_page&sys_id=249f7c0adb6cf7007ede365e7c9619fd) request is the formal government process for deciding whether something belongs under ITAR or EAR, but it is uncommon in routine procurement work and usually reserved for genuinely hard boundary cases.


Read More:[BOM Scrub Explained](https://www.cofactr.com/articles/bom-scrub-explained)


## Practical Next Steps for Engineering and Operations Teams


Most teams can start with basic operational discipline rather than a giant export-compliance bureaucracy on day one.


Start with classification. Determine whether the hardware, software, drawings, or manufacturing data fall under the USML. USML-covered items and data are ITAR-controlled. Items outside the USML still need EAR review under the[Commerce Control List](https://www.pmddtc.state.gov/sys_attachment.do?sys_id=d7dfbbfe1bbd10942b6ca932f54bcb5d#:~:text=Do%20you%3A%20o%20Manufacture%2C%20repair%2C,hyperlink%20to%2022%20CFR) .


### Internal Controls


Once technical data is identified as ITAR-controlled, the first job is containing access internally.


That usually means:


- Restricting access to authorized U.S. persons
- Segmenting controlled projects and repositories
- Marking ITAR technical data clearly
- Controlling cloud, SaaS, and AI-tool access
- Reviewing permissions and audit logs
- Training employees on export-sensitive workflows


The operational goal is straightforward: know who can access controlled technical data and demonstrate that unauthorized users cannot.


### External Exposure Controls


The next job is preventing controlled data from leaking through external parties and external systems.


That includes suppliers, CMs, contractors, customers, cloud vendors, collaboration tools, supplier portals, PLM access, AI systems, and support teams that may sit behind those vendors.


Before any external party can touch ITAR-controlled technical data, confirm the control environment:


- Mark the data clearly as ITAR-controlled
- Limit access to authorized users only
- Confirm whether non-U.S. persons, overseas support teams, or subcontractors can access the files
- Put ITAR handling requirements into supplier agreements or purchase terms
- Restrict onward sharing and subcontractor access
- Use controlled portals or repositories instead of broad email distribution
- Review permissions and audit logs periodically
- Require prompt notice if the vendor suspects unauthorized access


The main point is containment. A U.S.-based supplier, friendly-country partner, or commercial-looking design package can still create ITAR exposure if controlled data can become visible to unauthorized users.


### Keep ITAR Boring


The teams that handle ITAR well usually make it part of the normal engineering and operations workflow. They classify the data early, mark it clearly, restrict access, review vendors before sharing files, and keep records that explain why each decision was made.


That is the real goal: make controlled technical data boring, traceable, and hard to accidentally leak. ITAR trouble tends to grow in the gaps between engineering speed and compliance visibility, especially when drawings move faster than anyone can track.


A little structure up front is much cheaper than discovering an export violation during an acquisition diligence review. Those meetings have terrible vibes.


**Ready to let Cofactr handle sourcing, negotiations, storage, kitting, and delivery while your team focuses on building products? It’s free to get started with Cofactr today.**


## Frequently Asked Questions


**What is ITAR and why does it matter for manufacturers?**


ITAR regulates defense articles, defense services, and related technical data listed on the United States Munitions List. Manufacturers handling aerospace, defense, RF, or spacecraft hardware often face ITAR obligations earlier than expected.


**What counts as ITAR technical data?**


ITAR technical data includes CAD models, schematics, source code, manufacturing instructions, test procedures, repair guidance, PCB layouts, and other information needed to design or produce defense-related hardware.


**What is a deemed export under ITAR?**


A deemed export occurs when ITAR-controlled technical data is disclosed to a non-U.S. person inside the United States. File access, design reviews, or screen sharing can trigger violations without any overseas shipment.


**Who qualifies as a U.S. person under ITAR?**


A U.S. person generally includes U.S. citizens, lawful permanent residents, protected individuals, and U.S. entities. Many common work visa holders are treated as non-U.S. persons under ITAR rules.


**Can an internal engineering meeting create an ITAR violation?**


Yes. A Zoom call, Teams meeting, or screen-share session discussing controlled technical details with unauthorized foreign participants can qualify as an export of technical data under ITAR.


**Does ITAR only apply when shipping hardware overseas?**


No. ITAR also controls technical data and defense services. Emailing drawings, sharing CAD files, or giving unauthorized access to controlled repositories can create export violations without physical shipments.


**What does DDTC registration actually mean?**


DDTC registration identifies a company to the State Department as participating in defense trade activities. Registration allows license applications but does not authorize exports, technical-data transfers, or defense services automatically.


**Is there an official ITAR certification?**


No. The government does not issue an official ITAR certification. Companies may be DDTC registered and maintain internal compliance controls, but every controlled transfer still requires proper authorization analysis.


**Why do engineering teams struggle with ITAR classification?**


Commercial-looking hardware can still fall under the USML when specially designed for military or aerospace applications. RF systems, ruggedized electronics, spacecraft assemblies, and modified components frequently create classification confusion.


**What is the United States Munitions List?**


The USML is the list of defense articles and related technical data controlled under ITAR. It appears in 22 CFR Part 121 and defines which items fall under State Department jurisdiction.


**Can cloud storage platforms create ITAR exposure?**


Yes. Shared drives, Slack workspaces, PLM systems, Git repositories, and collaboration tools can create export risk when non-U.S. persons or overseas support teams gain access to controlled technical data.


**Does storing ITAR files on a U.S.-based server solve the problem?**


Not necessarily. ITAR focuses heavily on access control. A server located in the United States still creates compliance problems if unauthorized foreign users can access controlled data remotely.


**What controls should companies apply to ITAR technical data?**


Common controls include export-control legends, restricted repositories, permission reviews, audit logging, supplier restrictions, controlled collaboration systems, and documented limits on non-U.S. person access.


**Can AI tools create ITAR compliance problems?**


Yes. Uploading controlled schematics, source code, manufacturing notes, or test data into AI systems may disclose technical data to providers, support personnel, or overseas infrastructure without proper authorization.


**Are AI meeting assistants risky for ITAR-controlled discussions?**


AI-generated transcripts, summaries, and searchable meeting notes can expose controlled technical data if recordings or embeddings become accessible to unauthorized users, offshore support teams, or AI training environments.


**What should teams verify before using AI tools with controlled data?**


Teams should review data retention policies, model-training terms, support access, storage locations, subcontractor access, logging controls, and whether the environment is contractually restricted for ITAR-controlled workloads.


**Can a supplier in the United States still create ITAR risk?**


Yes. U.S.-based suppliers may rely on overseas quoting teams, offshore IT administrators, subcontractors, or foreign support staff. Controlled drawings remain regulated if unauthorized users can access them.


**Why are supplier portals and PLM systems high-risk areas?**


Supplier portals often expose drawings, models, and manufacturing packages to broad user groups. Weak permission controls, unmanaged subcontractors, and overseas engineering support increase export and deemed export risk.


**What is an Empowered Official under ITAR?**


An Empowered Official is an authorized company representative responsible for certifying export-license submissions and legally binding the organization in export-control matters with the State Department.


**What licenses are commonly used under ITAR?**


Companies frequently use DSP-5 export licenses, Technical Assistance Agreements, Manufacturing License Agreements, and temporary export authorizations for controlled defense-related transfers and technical-data sharing activities.


**What are common examples of accidental ITAR violations?**


Frequent violations include emailing CAD files abroad, granting unauthorized repository access, misclassifying hardware as EAR99, sharing controlled designs during meetings, and using unsecured cloud collaboration environments.


**Can H-1B employees access ITAR-controlled data automatically?**


No. Employment status does not override ITAR restrictions. H-1B employees are often considered non-U.S. persons and may require authorization before accessing controlled technical data.


**Why is EAR99 misclassification dangerous?**


Assuming hardware is EAR99 without documented classification review can trigger major enforcement problems if the item actually belongs on the USML and requires State Department authorization before transfer.


**What is a Commodity Jurisdiction request?**


A Commodity Jurisdiction request asks the government to determine whether an item falls under ITAR or the Export Administration Regulations. Companies typically reserve this process for difficult classification disputes.


**How does DDTC enforce ITAR violations?**


DDTC enforcement can involve civil penalties, criminal penalties, debarment, seizure actions, mandatory compliance measures, and extensive oversight obligations following investigations or voluntary disclosures.


**Why do voluntary disclosures matter in ITAR enforcement?**


Prompt self-reporting and cooperation can substantially reduce penalties. DDTC encourages voluntary disclosures when companies discover unauthorized exports, technical-data transfers, or compliance failures.


**What operational steps help reduce ITAR risk?**


Teams typically start with classification reviews, controlled repositories, restricted access, export-control markings, vendor diligence, audit logging, employee training, and documented handling procedures for technical data.


**How should companies handle ITAR-controlled supplier relationships?**


Companies should restrict onward sharing, define handling rules in contracts, review vendor access controls, confirm subcontractor limitations, and require incident reporting procedures for unauthorized access events.


**What industries commonly encounter ITAR obligations?**


Aerospace, defense electronics, spacecraft manufacturing, military communications, RF systems, sensors, ruggedized hardware, and defense software development frequently involve ITAR-controlled components or technical data.


**Why is early classification important during product development?**


Early classification prevents uncontrolled file sharing, improper supplier engagement, and accidental exports. Waiting until production or diligence reviews often exposes years of undocumented technical-data handling mistakes.
