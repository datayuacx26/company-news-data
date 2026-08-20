---
schema_version: "1.0.0"
document_id: "36e12473bfb7210fbb81085e89c2bb4e1b99b6f0f3c303dc7e8942feadaa44a3"
company_key: "bentley-systems-incorporated-class-b-common-stock"
company: "Bentley Systems Incorporated"
source_id: "bentley-systems-incorporated-class-b-common-stock-news-import-dc7c98e657e9"
canonical_url: "https://blog.bentley.com/insights/managing-infrastructure-in-the-dark-and-what-it-takes-to-turn-the-lights-on/"
published_at: "2026-06-16T15:32:54+00:00"
first_seen_at: "2026-07-24T19:29:36.034394+00:00"
fetched_at: "2026-07-28T21:23:18.688239+00:00"
content_hash: "sha256:8fb3441cddcdf9ec7238dcd6486518853a8261ea46bb80ff711e31e105f41f79"
---

# Managing Infrastructure in the Dark — And What It Takes to Turn the Lights On

“What you don’t know CAN hurt you…”


There is a cost that does not appear on your project budgets. It does not show up in your maintenance accounts. It is not captured in your risk registers. And yet it compounds, quietly, with every capital project your organization executes, every modification your operations team makes, and every year that passes without deliberate intervention.


It is the cost of not knowing what you have, where it is, and whether the information describing it can be trusted.


This is the problem of engineering information debt on brownfield infrastructure assets — and it is significantly larger than most executive teams appreciate.


## The World Is Already Built


Infrastructure organizations in mature economies spend the overwhelming majority of their capital and operational budgets not building new things, but maintaining, expanding, and upgrading things that already exist. Pipelines laid in the 1970s. Substations commissioned in the 1980s. Water treatment plants that have been modified so many times that the original design intent is buried under decades of redlines, workarounds, and tribal knowledge.


This is the brownfield reality. And it presents an information challenge that is categorically different from building on a greenfield site.


On a greenfield project, information is created in parallel with the physical asset. Drawings are generated, reviewed, and approved before anything is built. Equipment is procured to a specification. A model or drawing set reflects the design intent with reasonable fidelity because nothing has yet diverged from it. The information environment starts clean.


On a brownfield asset, information is supposed to describe a physical reality that has been accumulating complexity for decades. Every modification, every emergency repair, every temporary fix that became permanent, every project that was delivered without proper documentation handover — all of it has widened the gap between what the drawings say and what actually exists in the field.


That gap is where your risk lives. It is also where a very large and poorly understood portion of your project cost overruns originates.


## What the Gap Actually Costs


The consequences of poor engineering information management are rarely attributed to information quality because the causal chain is long and indirect. But consider how these scenarios present themselves in practice:


A capital project commences on a live water treatment facility. The engineering team requests the current drawing set. What arrives is a collection of PDFs, some scanned from hand-drawn originals, others exported from CAD software three versions out of date. The team conducts a brief walkdown and proceeds with design. Three months into construction, the excavation contractor strikes an uncharted pipe. An RFI is raised. A variation is issued. The project absorbs an unbudgeted cost and a schedule delay — and the root cause is recorded as a “site condition.” It was not a site condition. It was an information failure.


A maintenance team isolates a section of pipeline for inspection based on the current P&ID. The isolation is not complete because a bypass installed during a previous project was never reflected in the drawing. An incident occurs. The investigation identifies “inadequate isolation” as the cause. The deeper cause — that the P&ID did not accurately represent the installed configuration — is noted but not structurally addressed.


A new project engineer joins the team and asks what the current operating pressure of a particular system is. The P&ID says one thing. The CMMS record says another. A spreadsheet maintained by a recently retired operator says a third. Nobody knows which is correct, so the engineer designs to the most conservative value — which results in oversizing, unnecessary cost, and a capital expenditure that could have been avoided with reliable data.


None of these costs are small in isolation. Cumulatively, across a portfolio of brownfield assets and a pipeline of capital projects, they represent a material and largely invisible drag on organizational performance.


## The Information Environment Most Organizations Actually Have


On a typical brownfield asset, engineering and operational information is fragmented across a constellation of repositories that were each implemented to solve a specific problem at a specific point in time, with no overarching architecture connecting them.


There is a maintenance management system — SAP, Maximo, or similar — that contains equipment hierarchies and work order history, but whose records have not been systematically updated to reflect physical changes made over the years. There is a GIS that knows approximately where the assets are, but not to the engineering accuracy needed for design. There is a document management system, or a SharePoint site, or a network drive, containing drawings of variable vintage and unknown currency. There is a process historian containing continuous operating data of extraordinary richness that the engineering team has no practical access to. And there are spreadsheets — an enormous number of spreadsheets — maintained by individuals, containing critical asset data that exists nowhere else and that will be partially or entirely lost when those individuals retire.


Multiple maintenance management systems— SAP, Maximo, SharePoint, GIS—hinder engineering teams ability to make quick and informed design decisions.


These systems do not talk to each other. The same physical asset has a different identifier in each one. Nobody knows, for any given data attribute, which system contains the authoritative value. And capital projects, rather than improving this situation at handover, typically make it incrementally worse — delivering documents rather than data, transmitting rather than integrating, and departing before the information problem they have inherited and amplified is resolved.


This is the information environment within which your engineering teams are making design decisions, your operators are making safety-critical decisions, and your executives are making capital allocation decisions. The question is not whether this costs money. The question is how much.


## What Aerospace and Defense Figured Out — And Infrastructure Hasn’t


There is an industry that solved a version of this problem decades ago, under conditions far more demanding than most infrastructure operators face. It is instructive to understand how, and why.


Boeing, Airbus, NASA, and SpaceX operate in environments where information failure is not a cost overrun. It is a catastrophe. An aircraft that has been built to a drawing that does not match the physical configuration is not a maintenance liability — it is a safety emergency. A launch vehicle assembled from components whose specifications have drifted from their digital records does not deliver a late project — it delivers a mission failure.


Product Lifecycle Management (PLM) consists of the physical asset, design requirements, and documentation at every stage of that asset’s life.


These organizations developed, over many decades and at enormous investment, a discipline called Product Lifecycle Management — PLM. At its core, PLM is built on a deceptively simple proposition: the digital description of a physical asset must be authoritative, current, and trusted at every stage of that asset’s life, from initial design through to retirement.


In practice this means several things that infrastructure organizations have not yet adopted at scale:


**A single, unique identifier for every component and assembly** , enforced across every system — design, manufacturing, maintenance, inspection — so that any question about any asset can be answered by querying a known, consistent source.


**A structured model of the asset that is maintained as a living representation** , updated when the physical asset changes, not retrospectively reconstructed from paper records after the fact.


**Formal version control on engineering information** , with the same rigour applied to a drawing revision or a data record change as a software engineer applies to a code commit. Changes are tracked, attributed, and reversible. The current state is unambiguous.


**A discipline of branching and merging** — when a modification is planned, the affected portion of the information environment is formally checked out, modified under controlled conditions, verified against the physical result, and checked back in. The as-operated record and the as-designed record are kept deliberately separate during the modification, and reconciled at a defined milestone.


This last concept deserves particular attention because it maps directly onto the most common and most costly failure mode in brownfield capital project information management.


## Branching and Merging: The Concept Infrastructure Needs to Borrow


When a software development team wants to build a new feature without breaking the working application, they branch the codebase. They take a controlled copy of the current state, work on it in isolation, test it, and then merge it back into the main trunk when it is ready. The main trunk continues operating during this process. The branch tracks its divergence from the trunk. The merge is a deliberate, governed act — not an accident.


The same logic should govern engineering information on a brownfield capital project. When a project commences, the relevant portion of the asset’s information environment — its drawings, its P&IDs, its equipment records, its data — should be formally branched. The project works from this branch, developing the design and tracking changes against the baseline. The operating asset’s master information environment continues to reflect the as-operated state, updated as operational changes occur. The two are maintained as parallel, simultaneous representations of the same physical systems, with a formal interface governing changes that cross the boundary between them.


Then, as the project moves through commissioning, the branch is progressively merged back into the asset’s master information environment — system by system, milestone by milestone, with verification at each step that the information being returned accurately reflects the physical reality that has been constructed. This is not a single handover event at the end of the project. It is a sequenced, scheduled activity that runs in parallel with the commissioning program and is treated with the same rigour as physical completion milestones.


Where this discipline is applied well, each capital project improves the asset’s information environment. The as-built record is better than what existed before. The data repositories are more complete and more accurate. The operating team takes handover with confidence in the information they have been given.


Where it is not applied — which is most brownfield capital projects today — each project adds physical scope while leaving the information environment in a state of greater fragmentation than it found it. The next project starts from a worse baseline. The cost of that degradation is paid in every project that follows.


## The Digital Twin Is a Business Strategy, Not a Software Product


The term “digital twin” has been colonized by vendors selling visualization platforms and dashboard tools. This is worth addressing directly, because the vendor framing obscures what is actually valuable about the concept and leads organizations to make technology investments that do not solve the underlying problem.


A digital twin, properly understood, is not a piece of software. It is a business strategy for maintaining a trusted, current, connected digital representation of a physical asset across its entire operational life — including through the capital projects that modify it. The technology is an enabler. The governance, the data standards, the organizational commitment, and the process discipline are what make it work.


The digital twin workflows lifecycle.


The organizations that have genuinely realized the value of this approach share a common characteristic. They did not start by buying a platform. They started by establishing what information they actually need to operate and maintain their assets reliably, what quality that information needs to be, and who is responsible for maintaining it. They built data standards and enforced them. They created a connected data environment — not a single monolithic system, but a governed set of repositories linked by common asset identifiers and maintained by named custodians with real authority. And they embedded information quality requirements into their capital project contracts with the same rigor as physical completion requirements.


The technology investment followed the governance investment, not the other way around. This sequence matters enormously. A sophisticated platform built on poor data and absent governance will faithfully and expensively replicate the fragmentation it was purchased to solve.


## What Management Needs to Do Differently


The path from the current state to a materially better information environment is not primarily a technology problem. It is a leadership, governance, and investment problem. Three shifts are required at the executive level:


**Recognize information quality as a balance sheet issue, not an administrative overhead.** The engineering information describing your assets is itself an asset. It has value — in reduced project costs, in faster and safer operations, in better capital allocation decisions. It also degrades if not maintained, and the cost of that degradation is real. Treating information management as a back-office function rather than a strategic investment is a decision with quantifiable financial consequences.


**Establish an empowered information management function within the asset-owning organization.** Not a project-funded role that disappears at handover. A permanent function with technical credibility, authority over data standards, and a seat at the table when capital projects are scoped and contracted. The organizations that have solved this problem have an information management capability that participates in projects from inception — not one that receives documents at the end.


**Change how capital project contracts are written.** Information quality requirements need to be specified with the same rigor as physical scope. Acceptance of a project should be conditional not only on physical completion but on verified information handover — data ingested into the asset’s repositories, as-builts verified against field reality, CMMS records updated, and the information branch formally merged back into the asset’s master environment. Until there are contractual and commercial consequences for information handover failure, the incentive structure will continue to favor physical delivery at the expense of information quality.


## The Compounding Return on Getting This Right


The ROI case for better engineering information management is not complex, though it requires thinking across project cycles rather than within individual project budgets.


When the information environment is reliable, projects start from a better baseline — less time spent verifying existing conditions, fewer unknowns, fewer RFIs and variations driven by information gaps. When data is structured and connected, operational decisions are better — maintenance is more targeted, capacity is better understood, failure risks are better anticipated. When handover is treated as a merge rather than a transmittal, each project builds on the value created by the last rather than compounding the debt.


The aerospace industry did not develop PLM discipline out of an abundance of caution. It developed it because the cost of information failure — in rework, in delays, in liability, and ultimately in safety — exceeded the cost of maintaining the information environment properly. Infrastructure organizations are operating under similar dynamics, at a different risk profile, and most have not yet reached the same conclusion.


The organizations that reach it first will have a structural cost and performance advantage over those that continue to absorb the hidden cost of information debt — one project, one variation, one information failure at a time.


The aerospace industry developed a PLM discipline to mitigate the cost of information failure.


*The conversation about how to implement these practices — the governance models, the technology stack, the contractual mechanisms, and the remediation roadmap for assets already in a poor information state — is the logical next step. The starting point is acknowledging that the problem is real, that it is costing money today, and that the tools and disciplines to address it already exist. They have existed, in adjacent industries, for decades.*


*This article was originally published on[Medium](https://medium.com/@hilmarretief) .*


## FAQ:


**


**


What can the infrastructure industry learn from aerospace organizations like NASA and Boeing?


Aerospace and defense companies operate in extreme environments where an information failure results in a mission catastrophe rather than just a late project. To prevent this, they pioneered Product Lifecycle Management (PLM), a discipline ensuring the digital description of an asset remains authoritative, strictly version-controlled, and fully trusted from initial design through retirement.


**


**


What is the concept of "branching and merging" in capital project management?


Borrowed from software development, branching and merging involves taking a formal, controlled copy (a “branch”) of an asset’s master information environment when a new project begins. As the project moves through commissioning, the updated, verified data is formally “merged” back into the main system, ensuring that every project actually improves the master record instead of leaving it more fragmented.


**


**


How should organizations approach digital twins and information handover in their contracts?


A digital twin should be properly understood as a business strategy for maintaining trusted, current data, rather than just a visualization software product. To implement this strategy effectively, organizations must change how capital project contracts are written, making final project acceptance contingent on the handover of rigorous, verified information alongside physical completion.
