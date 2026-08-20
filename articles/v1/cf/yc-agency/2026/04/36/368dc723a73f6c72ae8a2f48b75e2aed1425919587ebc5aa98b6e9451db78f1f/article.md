---
schema_version: "1.0.0"
document_id: "368dc723a73f6c72ae8a2f48b75e2aed1425919587ebc5aa98b6e9451db78f1f"
company_key: "yc-agency"
company: "Agency"
source_id: "yc-agency-news-import-0d31f4b059c0"
canonical_url: "https://blog.getagency.com/articles/sbir-ato-guide"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-21T05:05:48.945011+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:ae5c40409032f3ef060fa434d68eca42e70bdabcbb783debb994165c34b80e93"
---

# SBIR Grants: How to Secure Authority to Operate (ATO)

*SBIR and STTR programs have produced some of the most innovative technologies in the federal space — but innovation means little if your product cannot operate in a government environment. For a growing number of SBIR awardees, securing an Authority to Operate is the gate between a successful prototype and actual agency adoption.*


The Small Business Innovation Research (SBIR) and Small Business Technology Transfer (STTR) programs inject substantial funding annually into small businesses developing cutting-edge technologies for federal missions. What many awardees discover during Phase II or Phase III is that their technology must meet the same security authorization requirements as any other federal information system. An[Authority to Operate](https://blog.getagency.com/articles/what-is-an-ato) is not optional — it is the formal approval that allows your system to process government data in a production environment.


This guide walks SBIR and STTR awardees through the ATO process, from determining whether you need one to selecting the right baseline, leveraging cloud infrastructure, and working with your sponsoring agency to reach authorization.


## Why SBIR Companies Need an ATO


The need for an ATO typically emerges when an SBIR project transitions from research to operational deployment. During Phase I (feasibility study) and early Phase II (prototype development), your work may involve only simulated data or operate in isolated lab environments. But as your technology matures and moves toward integration with agency systems, the security requirements escalate dramatically.


You will need an ATO if your SBIR-funded product:


- **Connects to government networks** — Any system that interfaces with a .mil or .gov network requires authorization from the network's Authorizing Official
- **Processes federal data** — If your system ingests, stores, analyzes, or transmits data classified as federal information (including CUI, PII, or PHI), it requires an ATO
- **Deploys in a federal environment** — Systems installed on government infrastructure, even as pilot programs, require authorization
- **Provides cloud services to agencies** — Cloud products used by federal employees fall under FedRAMP requirements


What we tell clients: if you are unsure whether you need an ATO, ask your Contracting Officer's Representative (COR) or the agency's Information System Security Officer (ISSO) during Phase II kickoff. Discovering the ATO requirement six months before your Phase II ends leaves insufficient time and budget.


### The Phase II to Phase III Transition


The most critical ATO moment for SBIR companies is the transition from Phase II to Phase III. Phase III is where your technology enters production use and generates revenue beyond the SBIR grant. Federal agencies will not deploy your product in production — or award Phase III contracts — without a valid ATO.


In our experience, the SBIR companies that handle this transition successfully are those that begin security engineering in Phase II rather than treating it as a Phase III problem. Building security into your architecture from the start is dramatically cheaper than retrofitting it after the fact.


## Determining Your Security Baseline


The first step in the ATO process is determining the appropriate FIPS 199 security categorization for your system. This categorization — Low, Moderate, or High — drives the number of security controls you must implement and directly affects your timeline and cost.


Impact Level Control Count (approx.) Typical SBIR Scenario


**Low** ~125 controls Public-facing analytics tool, non-sensitive data visualization


**Moderate** ~325 controls System processing PII, CUI, or connecting to agency networks


**High** ~421 controls System handling law enforcement data, critical infrastructure


Most SBIR products fall into the Low or Moderate category. High-impact systems are rare for SBIR companies and typically involve national security or law enforcement applications.


### How to Confirm Your Categorization


Do not assume your categorization. The sponsoring agency determines the FIPS 199 categorization based on the types of data your system will process and the potential impact of a security breach. Here is the process:


1. **Identify data types** — Document every category of data your system will process, store, or transmit. Include data received from government systems, data generated by your system, and metadata.
2. **Map to NIST SP 800-60** — Use NIST SP 800-60 (Guide for Mapping Types of Information and Information Systems to Security Categories) to identify the provisional impact levels for each data type.
3. **Engage the agency** — Present your data mapping to the sponsoring agency's ISSO or security team. They will confirm or adjust the categorization based on agency-specific policies and mission context.
4. **Document the decision** — Record the agreed categorization in your System Security Plan. This becomes the foundation for all subsequent control selection.


## Leveraging Cloud Provider Inherited Controls


One of the most effective strategies for SBIR companies to reduce ATO scope and cost is deploying on a[FedRAMP-authorized](https://blog.getagency.com/articles/fedramp-authorization-explained) cloud service provider. When your system runs on infrastructure that already holds a FedRAMP authorization, you can inherit a significant portion of the required security controls rather than implementing them yourself.


### How Control Inheritance Works


Security controls fall into three responsibility categories:


- **Fully inherited** — The cloud provider is entirely responsible. Physical security (PE family), data center environmental controls, and infrastructure-level network protections typically fall here. You document these as inherited and reference the provider's FedRAMP authorization.
- **Shared responsibility** — Both you and the cloud provider contribute to meeting the control. For example, access control at the infrastructure layer is the provider's responsibility, but application-level access control is yours.
- **Customer responsibility** — The control is entirely your obligation. Application security, data classification, user training, and incident response planning at the application layer are your responsibility regardless of the underlying infrastructure.


### Impact on SBIR ATO Scope


For a typical SBIR application deployed on AWS GovCloud, Azure Government, or Google Cloud for Government, you can expect to inherit roughly 30 to 40 percent of Moderate-baseline controls. This reduces both the implementation effort and the assessment scope, which translates directly to timeline and cost savings.


Deployment Model Controls Inherited (approx.) Controls Remaining Impact on Timeline


**FedRAMP-authorized IaaS** 30–40% 195–230 controls Saves 3–5 months


**FedRAMP-authorized PaaS** 40–55% 145–195 controls Saves 4–7 months


**FedRAMP-authorized SaaS (as platform)** 50–65% 115–160 controls Saves 5–8 months


What we tell clients: choose your cloud platform before you start the ATO process, and choose one with FedRAMP authorization at your target impact level. Migrating platforms mid-process is one of the most expensive mistakes we see SBIR companies make.


## Working with the Sponsoring Agency's Authorizing Official


The Authorizing Official (AO) is the senior government executive who will ultimately sign your ATO. For SBIR companies, this is typically someone within the sponsoring agency — the agency that awarded your SBIR contract. Understanding how to work effectively with the AO and their team is critical to a successful authorization.


### Key Agency Contacts


- **Authorizing Official (AO)** — The decision-maker. You may interact with the AO only at key milestones, but every deliverable is ultimately reviewed against their risk tolerance.
- **Information System Security Officer (ISSO)** — Your primary day-to-day contact for security matters. The ISSO reviews your documentation, validates control implementations, and advises the AO on authorization readiness.
- **Contracting Officer's Representative (COR)** — Manages the contract relationship. The COR can help you understand timeline expectations and may facilitate introductions to the security team.
- **Information System Security Manager (ISSM)** — Oversees the agency's security program. The ISSM may set agency-specific policies that go beyond the standard NIST baseline.


### Tips for Effective Agency Engagement


1. **Initiate security conversations early** — Contact the ISSO during Phase II kickoff, not at Phase II closeout. Ask what the agency expects for authorization and whether they have specific documentation templates or requirements.
2. **Use agency templates when available** — Many agencies provide SSP templates, control implementation guides, and POA&M formats. Using these demonstrates that you understand the agency's processes and reduces review friction.
3. **Schedule regular checkpoints** — Propose monthly or bi-monthly security reviews with the ISSO. These catch misalignments early and keep your ATO timeline on track.
4. **Be transparent about gaps** — If you cannot implement a specific control, say so early and propose a compensating control or POA&M item. Authorizing Officials respond poorly to discovering undisclosed weaknesses during assessment.
5. **Understand the AO's risk appetite** — Some AOs are conservative and expect near-zero findings. Others accept a reasonable POA&M. Your ISSO can help you calibrate expectations.


## The FedRAMP Pathway for SBIR Cloud Products


If your SBIR-funded product is a cloud service and you intend to sell it to multiple federal agencies, pursuing[FedRAMP authorization](https://blog.getagency.com/articles/fedramp-authorization-explained) rather than a single-agency ATO may be the better long-term strategy. A FedRAMP authorization — whether a P-ATO through the Joint Authorization Board or an Agency ATO submitted to the FedRAMP PMO — creates a reusable authorization package that any federal agency can leverage.


### When FedRAMP Makes Sense for SBIR Companies


FedRAMP is a significant investment, but it makes sense when:


- Your product is a multi-tenant cloud service (SaaS, PaaS, or IaaS)
- You have identified demand from two or more federal agencies
- Your Phase III business plan depends on broad federal adoption
- You are willing to invest in continuous monitoring infrastructure for the long term


For SBIR companies building a specialized tool for a single agency's mission, a traditional agency ATO is faster, cheaper, and sufficient.


### FedRAMP Costs for SBIR Companies


FedRAMP authorization is more expensive than a traditional ATO due to the 3PAO assessment requirement and the rigor of the FedRAMP PMO review. Costs vary significantly based on system complexity, impact level, and the chosen path to authorization. SBIR companies should budget for both the initial assessment and ongoing continuous monitoring. For more detailed cost breakdowns, see our[FedRAMP cost guide](https://blog.getagency.com/articles/fedramp-cost) . These costs can be partially funded through SBIR Phase II and Phase III contracts, as security engineering and compliance are allowable costs.


### How a P-ATO Opens Doors


A Provisional ATO from the Joint Authorization Board is particularly powerful for SBIR companies because it signals to every federal agency that your cloud service has been vetted at the highest level of FedRAMP scrutiny. Agencies can issue their own ATOs based on the P-ATO package with minimal additional assessment, dramatically shortening the sales cycle.


In our experience, SBIR companies with FedRAMP authorization convert federal prospects to customers three to five times faster than those requiring each agency to conduct its own assessment.


## Budgeting for ATO Within SBIR Funding


One of the most common mistakes SBIR awardees make is failing to budget for security and compliance within their grant proposals. ATO-related costs are allowable under SBIR contracts, and the most successful companies plan for them from the start.


### Recommended Budget Allocation


For a typical Phase II SBIR contract, we recommend allocating:


- **10–15% for security engineering** — Building security controls into the product architecture, implementing encryption, access controls, logging, and vulnerability management
- **5–10% for documentation and compliance** — System Security Plan development, policy writing, POA&M management, and continuous monitoring setup
- **5–10% for assessment preparation and 3PAO engagement** — If pursuing FedRAMP, or for pre-assessment readiness reviews for a traditional ATO


This level of allocation may seem high, but it is far less expensive than discovering compliance gaps after Phase II funding has been spent.


### Using Phase III to Complete Authorization


If Phase II funding is insufficient to complete the full ATO process, structure your Phase III proposal to include authorization completion as a funded activity. Many agencies recognize that SBIR companies need additional support to transition from prototype to authorized production system, and Phase III contracts can include explicit ATO milestones.


## Common Pitfalls for SBIR Companies


### Treating Security as a Phase III Problem


The most expensive mistake we see is SBIR companies that build their entire product during Phase II with no consideration for federal security requirements, then discover during Phase III that their architecture cannot support required controls. Retrofitting encryption, access controls, audit logging, and network segmentation into an existing application can cost more than building them in from the start.


### Underestimating Documentation Volume


A Moderate-baseline System Security Plan describes every security control implementation in detail. For a typical SBIR application, this means 200 to 400 pages of technical documentation. Start writing your SSP as you implement controls — do not leave it as a monolithic task at the end.


### Ignoring Continuous Monitoring Requirements


An ATO is not a finish line. Once granted, you must maintain continuous monitoring: monthly vulnerability scans, ongoing POA&M management, configuration change tracking, and annual assessments. Budget for this ongoing cost in your business model, not just in your SBIR proposal.


### Not Engaging the ISSO Early Enough


The agency ISSO is your most important ally in the ATO process. They know the AO's expectations, the agency's specific requirements, and the common reasons authorization packages are rejected. Engaging them at the start of Phase II rather than the end can save months of rework.


## How Agency Supports SBIR Companies


At Agency, we work with SBIR and STTR awardees to build security into their products from Phase II onward. Our approach includes gap assessments against the target security baseline, architecture reviews that optimize for control inheritance from FedRAMP-authorized cloud providers, SSP development that aligns with agency templates and assessor expectations, and continuous monitoring automation that minimizes the ongoing compliance burden.


Whether you need a single-agency ATO or are positioning for[FedRAMP authorization](https://blog.getagency.com/articles/fedramp-authorization-explained) , the key is starting early, engaging the right agency contacts, and treating security as a product feature rather than a compliance burden. The SBIR companies that internalize this approach are the ones that successfully transition from grant-funded research to sustainable federal products. For more on[CMMC requirements for small businesses](https://blog.getagency.com/articles/cmmc-requirements-for-small-business) , see our dedicated guide.
