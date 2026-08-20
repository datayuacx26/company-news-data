---
schema_version: "1.0.0"
document_id: "2d2b6bc296b6b4306b4b945f879fac3b009e68cfcda40918080e4416d4b0b965"
company_key: "yc-o11"
company: "o11"
source_id: "yc-o11-news-import-e61ea6fbe134"
canonical_url: "https://o11.ai/blog/how-o11-builds-a-living-data-model-from-enterprise-systems"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-17T12:10:05.697433+00:00"
fetched_at: "2026-08-17T12:10:07.459125+00:00"
content_hash: "sha256:d6ca3b0653796b9b58399d7f3ac6367d88f59139aad9ddb3fb03a37a069400ff"
---

# How o11 Builds a Living Data Model from Enterprise Systems

The short answer is that o11 builds a living data model by connecting approved enterprise sources, indexing their contents, extracting useful structure, and maintaining relationships as the source landscape changes. The result is not a static spreadsheet or a one-time export. It is a permission-aware institutional record that can support search, research, analysis, drafting, and supervised action.


The word “living” is important. Enterprise information changes constantly: a CRM record is updated, a new diligence document arrives, an email changes the status of a relationship, and a financial file is replaced by a later version. A useful model must reflect those changes without forcing a team to rebuild its entire context layer by hand.


o11’s public[Memory product page](https://o11.ai/solutions/atlas) describes continuous indexing across approved sources including DealCloud, Salesforce, Outlook, calendars, email, files, data rooms, market data, ERP, notes, templates, and systems of record. It also describes source permissions, institutional recall, and a continuously updated record at terabyte scale. This article explains the architecture in practical terms and makes clear where human governance remains necessary.


## Start with sources, not a blank schema


Most enterprises already have many systems that function as partial sources of truth. The challenge is not a lack of data. It is that each system sees only one part of the business.


Source type Typical information Why context is lost when isolated


CRM Companies, contacts, pipeline, activity Relationship history may sit in email and notes


Email and calendar Decisions, commitments, timing, participants Important facts are difficult to find later


Files and data rooms Contracts, diligence, financials, presentations Versions and related entities are hard to reconcile


ERP and systems of record Transactions, operations, master data Operational facts lack surrounding narrative


Market data Prices, benchmarks, public-company context External evidence is separate from internal work


Templates and prior work Institutional standards and precedent New teams repeat work already completed elsewhere


o11’s first design choice is therefore source-oriented. Instead of asking users to manually load every fact into a new application, the product connects the systems and content that already define how the enterprise works. The exact source set depends on the deployment and the organization’s approvals.


## The four layers of a living model


A useful model can be explained as four layers. They are not a promise that every source has identical structure. They are a way to reason about how raw material becomes usable context.


### 1. Ingestion and indexing


The first layer makes approved source content available for processing and retrieval. Documents, messages, records, and other items need stable identities, metadata, and status. Indexing allows the system to find relevant content without requiring a person to remember which system contains it.


The public product description uses the phrase “continuously indexes.” That means the model is intended to stay synchronized as approved sources change. It does not mean every source updates at exactly the same moment or that a connector can overcome a source system outage. Freshness should be monitored and communicated like any other data-quality property.


### 2. Structure and entities


The next layer identifies useful structure. An enterprise may refer to the same company by a legal name, a shorthand name, an account ID, or a portfolio label. A deal may be represented by a CRM opportunity, a data-room folder, a calendar series, and a set of emails. A person may appear as an email address, a contact, and a meeting participant.


The model can connect these references when the evidence supports the relationship. This is the difference between searching for a phrase and asking for a coherent record of a company, transaction, or workstream.


Entity resolution is not magic. Ambiguous names, stale records, and conflicting identifiers require conservative matching and review. A well-designed system should preserve uncertainty rather than silently merge unrelated entities.


### 3. Relationships and provenance


The third layer captures how information relates. A document may describe a company. An email may explain a decision about that company. A calendar meeting may include the people responsible for the next step. A model may contain an assumption whose support appears in a source document.


Relationships make it possible to retrieve context rather than isolated snippets. Provenance makes it possible to inspect why a relationship or claim was returned. For professional work, both are essential. A result without source context may be fast but difficult to trust.


### 4. Permissions and application


The final layer determines what a person or workflow is allowed to retrieve and how the context is applied. o11’s public product language emphasizes preserving source access controls so each person and workflow can retrieve only the firm context they are authorized to use.


This is not an optional filter added after search. Permissions shape what should be indexed for a tenant, what can be returned to a user, and what can be used in a generated output. The application layer can then put context to work in research, decisions, drafts, analysis, and supervised actions.


## A practical flow from source to answer


Consider a request from a deal team: “Prepare a review of the target’s customer concentration and identify the assumptions that changed since the last committee meeting.” A living model supports a workflow like this:


Step What the system needs to do What the reviewer should see


Connect Reach the approved CRM, data room, email, meeting notes, and financial files Which sources were in scope


Find Locate relevant target, customer, and committee records The matching entities and time period


Relate Link the current financials to prior materials and decisions The supporting records and relationships


Compare Identify changed values, assumptions, and dates A difference view with source references


Apply Draft a reviewable memo or update package Evidence-backed text and unresolved items


Govern Enforce access and approval rules Who can view, edit, or approve the result


The output is not automatically the committee’s decision. It is a better starting package for a human review process.


## Why this is different from a file search box


Keyword search is valuable, but enterprise questions often require more than finding matching words. “What changed in the target’s customer concentration?” may require understanding which files refer to the same target, distinguishing a current schedule from an old one, and connecting a number to the source table or note that explains it.


o11’s[file-search tooling](https://o11.ai/solutions/atlas) and source-backed workflow are intended to reduce that context switching. A user can begin with a business question rather than a file path. The system still needs the user to specify scope when the question is ambiguous, and the user should verify important conclusions.


## Keeping the model current


A model becomes stale when it has no way to incorporate change. Common change patterns include:


- new documents added to an existing matter;
- revised files replacing prior versions;
- new CRM activity or relationship notes;
- records changing status;
- permissions changing when people join, leave, or change teams; and
- external data being updated on a different schedule from internal data.


A living model needs update detection, indexing status, version awareness, and operational monitoring. It should be possible to distinguish “no evidence of change” from “source was not available” and “source has not been indexed yet.” Those states have different implications for a decision.


The product promise is to reduce manual maintenance. It should not be interpreted as a guarantee that all source updates are instantaneous or that a connector can repair an incorrect upstream record. Teams still need freshness expectations for critical workflows.


## Where human judgment belongs


Automation is most useful when it handles repetitive discovery and organization while people retain control over decisions and exceptions. In a living data model, human review remains important for:


1. defining which sources and spaces are approved;
2. resolving ambiguous entities or conflicting records;
3. setting the time period and business scope of a question;
4. reviewing citations and source quality;
5. approving client-facing or investment-sensitive outputs; and
6. correcting a source system when the underlying record is wrong.


This division of labor is especially important in financial services. An AI system can assemble evidence for an investment memo or pitchbook workflow, but the deal team owns the recommendation, positioning, and final judgment. See the dedicated[private equity](https://o11.ai/industry/private-equity) and[investment banking](https://o11.ai/industry/investment-banking) pages for examples of that supervised framing.


## What teams should measure


Teams evaluating a living data model should measure more than retrieval speed. Useful measures include:


- source coverage: which approved systems are connected;
- freshness: how quickly relevant changes appear;
- entity precision: how often records are matched correctly;
- citation coverage: how often important claims have inspectable support;
- permission correctness: whether users see only authorized context;
- review time: how long it takes a person to validate an output; and
- exception rate: how often a workflow needs human correction.


These measures make the product operational. They also expose where the system should not be trusted without review. A fast answer with the wrong entity or stale source is not a successful data workflow.


## Common implementation mistakes


Three mistakes appear often when teams attempt this work.


### Treating every source as equally authoritative


An email, a signed agreement, and a stale spreadsheet may all mention the same value. They should not automatically receive the same weight. Teams need source precedence and date rules for important workflows.


### Ignoring permissions until launch


Permissions are part of the data model, not a final user-interface check. Model source scopes and access changes early, test them with representative users, and keep an audit trail for sensitive workflows.


### Starting with a universal ontology


Trying to model the entire enterprise before delivering one useful workflow creates long delays. Start with a recurring question, define the entities and evidence needed for it, then expand coverage as the model proves useful.


## Frequently asked questions


### What is a living data model?


It is an organized, continually updated representation of enterprise entities, records, relationships, and source context. It changes as approved systems and content change, rather than remaining a one-time export.


### Does o11 replace a data warehouse?


Not necessarily. o11 can sit alongside existing warehouses, databases, and systems of record. Its focus is connecting and applying enterprise context across approved sources, including information that may not already be modeled in a warehouse.


### Which systems can o11 connect?


The public Memory page lists DealCloud, Salesforce, Outlook, calendars, email, files, data rooms, market data, ERP, notes, templates, and systems of record. Available sources depend on the deployment and approvals.


### How does o11 handle conflicting records?


Conflicts should be surfaced for review rather than silently resolved. Teams should define source precedence and time-period rules for workflows where a discrepancy can change a decision.


### How does the model respect permissions?


o11’s product description says source access controls are preserved so users and workflows retrieve only context they are authorized to use. Organizations still need to configure and test source permissions correctly.


### Is the model fully automatic?


The goal is to automate repetitive connection, indexing, and context assembly. Source governance, ambiguous entity decisions, output review, and business judgment remain human responsibilities.


## Sources and further reading


- [o11 Memory: connected enterprise context](https://o11.ai/solutions/atlas)
- [o11 Enterprise](https://o11.ai/enterprise)
- [o11 private equity workflows](https://o11.ai/industry/private-equity)
- [o11 investment banking workflows](https://o11.ai/industry/investment-banking)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST Big Data Interoperability Framework](https://www.nist.gov/itl/big-data-interoperability-framework) , for a public reference on data ecosystems, structures, and interoperability.


The product description and claims in this article were reviewed against o11’s public product pages on 2026-08-14.
