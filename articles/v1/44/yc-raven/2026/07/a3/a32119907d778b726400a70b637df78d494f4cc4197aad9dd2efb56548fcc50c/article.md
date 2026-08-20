---
schema_version: "1.0.0"
document_id: "a32119907d778b726400a70b637df78d494f4cc4197aad9dd2efb56548fcc50c"
company_key: "yc-raven"
company: "Raven"
source_id: "yc-raven-news-import-5e16082028b5"
canonical_url: "https://startraven.com/blogs/context-rebuilding-tax-brownfield-plants"
published_at: null
first_seen_at: "2026-07-24T11:20:36.993056+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:35b461953e0a9c62880dd03fd751f20397163a00df0a7ca0b0c120866ae665d6"
---

# The Context-Rebuilding Tax at Brownfield Plants

The week before a HAZOP looks the same at most brownfield plants. The study leader pulls the P&IDs for the unit — scanned PDFs, a revision cloud from 2011 still sitting on sheet 4 — and starts building context by hand. Which safeguards protect this node? The safeguard list from the last study is nine years old. Did we ever have an incident on this type of exchanger? There was one, people think, in another unit, before the current team's time. Where is the relief valve datasheet? In a folder, somewhere, probably.


None of this is analysis. It is retrieval — assembling facts that already exist in the plant's own documents so that the actual work can begin. And when the session finally runs, part of it goes to debating facts that were written down somewhere all along.


## The same tax on every workflow


Call it the **context-rebuilding tax** . Every workflow at a brownfield plant pays it. A HAZOP starts with days of P&ID review and precedent hunting. An MOC review starts by finding the affected SOPs, interlocks, and past incidents — usually from memory. A shutdown work pack starts by reassembling the same equipment context that was assembled last shutdown, by someone else, who has since changed roles. An investigation starts with the timeline and the history hunt. A parts order starts with a 40-page scanned vendor datasheet and a planner's recollection of which model variant is actually installed.


The tax is invisible because it hides inside other jobs. Nobody's timesheet says "context reassembly." It shows up as HAZOP prep, permit preparation, work-pack planning, investigation. And because it never appears as its own line item, it never becomes a project, never gets a budget, and never goes away on its own.


## "But we have a DMS"


The plants that pay this tax most heavily are not short on systems. There is a DMS, an EAM, SharePoint, a historian, and folders of vendor documents. The documents exist. What does not exist is the connection between the documents and the place where work actually starts: **the drawing** .


A process plant runs on its P&IDs. Isolation boundaries are traced on them. HAZOP nodes are marked on them. The permit references them. But in most brownfield sites the P&ID is a scan — a picture of plant intelligence, not the thing itself. You cannot click a pump on a picture. So every job begins with a person acting as the join between the drawing and the ten systems that hold its context.


That person is usually senior. They are the ones who know where things are, which revision is current, and what happened on that line in 2014. Which means the most experienced engineers at the plant spend a real share of their week doing retrieval — the one part of the job that does not need their judgment.


## What it costs


Three things, in increasing order of severity. **Hours** — mostly senior-engineer hours, paid again on every review, permit, shutdown job, and investigation. **Variance** — the quality of an isolation plan or a HAZOP pre-read depends on who prepared it and how much time they had; night shift gets a thinner version than day shift, and the thirtieth permit of a turnaround gets less scrutiny than the third. **Silence** — the worst cost is what never surfaces: the precedent incident nobody found, the safeguard that was decommissioned but still appears in the documentation, the outstanding inspection finding on the exchanger that got opened anyway. Most of these stay consequence-free. The ones that don't become incident reports.


## Convert the drawings once


The fix is not another repository, and it is not a chatbot over the file shares. A chatbot answers questions; it does not know that PSV-201 protects V-201, or that the SOP it just retrieved references a valve that was removed two MOCs ago. The fix is to make the drawing itself smart — once.


Converting scanned P&IDs into smart, connected drawings means extracting every pump, valve, line, and instrument as an object, with its connectivity, and linking each tag to the documents that describe it: datasheets, SOPs, manuals, MOCs, incidents, inspection and maintenance history, spares. One information page per asset, validated by the plant's own engineers, every link traceable to its source.


After that, the tax stops compounding. The HAZOP pre-read assembles itself from the drawing and gets *verified* instead of built. The isolation boundary is marked on the P&ID, not traced across sheets by hand. The investigation starts from the equipment's actual history instead of a folder hunt.


## The first workflow takes effort. The next ten don't.


This is what separates conversion from a redraw service. A manual redraw produces CAD files — accurate the day they ship, decaying from the first MOC after. Smart P&IDs with asset pages are infrastructure: every linked document, every validated correction, every completed workflow makes the base richer. The second workflow reuses the objects, source links, and review model the first one built. By the tenth, the work is being drafted from the asset base and reviewed by an engineer — instead of assembled by one.


That compounding is also the honest test of any plant-AI purchase: if the second use case costs as much as the first, you bought a service, not a system.


## How to see the tax at your plant


You don't need a study. Ask three questions about the last month. On the last MOC or HAZOP, how long did assembling the evidence take — and who did it? On the last shutdown, how much of the work-pack prep had been done before, by someone else, for a previous shutdown? When the last near-miss was investigated, was the relevant precedent found before or after the conclusions were written?


If the answers are "days," "most of it," and "after," the tax is running — and it has been for years. What's new is that converting the drawings and connecting the plant's information around them is now a matter of weeks, not a multi-year services project. That is the work Raven does. If you want to see it on one of your own drawings, it takes 15 minutes.


About the author


**Priyansh Srivastava** is co-founder and CEO of Raven (YC S22). Raven converts old, scanned P&IDs into smart, connected drawings and builds one information page per asset on top — deployed in production at a 3M TPA ammonia–urea complex, where 1,000+ users work from the same validated drawings.


Smart P&IDs


Asset Information


Brownfield Plants
