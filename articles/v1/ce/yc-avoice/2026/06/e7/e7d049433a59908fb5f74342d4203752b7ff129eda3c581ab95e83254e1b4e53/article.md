---
schema_version: "1.0.0"
document_id: "e7d049433a59908fb5f74342d4203752b7ff129eda3c581ab95e83254e1b4e53"
company_key: "yc-avoice"
company: "Avoice"
source_id: "yc-avoice-news-import-1664a7132626"
canonical_url: "https://www.avoice.co/blog/specification-coordination-architect-structural-engineer"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-08-09T19:33:57.958959+00:00"
fetched_at: "2026-08-09T19:33:58.487003+00:00"
content_hash: "sha256:facd5f877082a2e3a036e6f3d99c4bb94150b99d22c5cf4b82bcd20175b63bc3"
---

# Specification coordination between architect and structural engineer: closing the gaps

On most UK projects, the architectural and structural specifications are written by different people, in different offices, working to different deadlines. They are meant to describe the same building. They often don't. Specification coordination is the quiet discipline that catches those disagreements before a contractor finds them first, and on a busy job it's the first thing to slip.


Think about how a typical job is staffed. The architect writes the architectural spec, usually in NBS, structured around Uniclass or CAWS. The structural engineer writes a separate structural spec, often to their own house style. Both describe the same concrete, the same fixings, the same junctions. Neither reads the other's document line by line. That's where the trouble starts.


### Where the two specifications actually overlap


More of the building sits in the overlap than most people assume. Concrete is the obvious one. The engineer specifies the structural grade, the cover, and the reinforcement. The architect specifies the finish, the formwork quality, and sometimes the admixtures for colour or water resistance. If those two descriptions of the same pour disagree, the contractor has to guess, raise an RFI, or pour what's cheapest and argue later.


Fixings are another. So are movement joints, waterproofing at the substructure, fire protection to steel, and the tolerances where cladding meets frame. Each of these belongs to both disciplines and fully to neither. The architect assumes the engineer has it. The engineer assumes the architect has named the product. Sometimes both specify it, in conflicting terms. Sometimes neither does. And the more a project leans on performance specifications rather than prescriptive ones, the easier it is for two reasonable descriptions of the same element to quietly drift apart.


And this is before the mechanical and electrical engineer, the fire engineer, and the facade consultant add their own documents to the pile. On a mid-sized job you can easily have five or six specifications, each written in isolation, all describing parts of one building. The architect sits at the centre of that web by default, because the architect is usually the one the contractor calls when two documents disagree. Coordination, then, isn't a courtesy. It's a liability question.


### The gaps nobody owns


The dangerous items aren't the conflicts. Conflicts get caught, eventually, because two clauses say different things and someone notices. The real risk is the gap, the item that falls between the two specifications because each discipline thought it belonged to the other.


Fire protection to structural steel is the classic example. The engineer specifies the section sizes and may note a required fire rating. The architect specifies finishes. So who specifies the intumescent coating, its thickness, the approved applicator, and the inspection regime? On a coordinated job, clearly one of them. On a rushed one, the answer turns out to be nobody, and it surfaces during the Building Safety Act sign-off when there's no time and no budget left to fix it the easy way.


The schedule of quantities is where these gaps turn into money. A quantity surveyor prices what the documents say, and if the documents are vague or contradictory, the QS prices the cautious reading or carries a provisional sum. Either way the client pays for the uncertainty. A gap that would have cost nothing to close at Stage 4 becomes a line item, then a variation, then a claim. The cost of poor coordination is rarely a single dramatic failure. It's a slow leak of contingency across the whole job.


### Why drawing coordination doesn't close the specification gap


Most practices coordinate drawings well. Models get federated, clashes get detected, and the architect and engineer sit in a coordination meeting to resolve the geometry. That work is real and it matters. But it solves a different problem.


A clash detection tool tells you that two objects occupy the same space. It says nothing about whether the clauses describing those objects agree. You can have a perfectly coordinated model where the architectural spec calls for a 60 minute fire door and the door schedule says 30, or where the structural spec demands a concrete grade the architectural finish clause makes impossible. The geometry is clean. The words are at war. And it's the words that get built from.


### What specification coordination looks like in practice


Good specification coordination is mostly about reading, cross-referencing, and asking awkward questions early. At RIBA Stage 3, the job is to agree the boundary: who specifies what, written down, before either spec is drafted in detail. A short responsibility matrix that names every shared element and assigns it to one discipline is worth more than any amount of clever clause writing later.


At Stage 4, the work shifts to checking. Every shared element gets read across both documents and against the drawings and schedules. Does the fire rating in the spec match the door schedule and the compartmentation drawing? Does the concrete grade in the structural spec match what the architectural finish assumes? Does the waterproofing system named in one document contradict the substructure detail in the other? This is slow, unglamorous work, and on a tight fee it's the first thing to get skipped.


It helps to be clear about who owns this. The principal designer role under the CDM regulations covers coordination of health and safety information, and people sometimes assume that captures specification coordination too. It doesn't. A project can have a diligent principal designer and still ship two specs that disagree about a concrete grade, because the CDM duty and the technical reconciliation of clauses are different jobs. Someone has to own the second one explicitly, or it falls to whoever the contractor phones first.


That's the honest problem. Specification coordination is known to be necessary, and quietly known to be under-resourced. Most senior architects can name a project where a coordination gap cost real money. Few can point to the hour in their programme that was actually paid for finding it.


### Where AI changes the economics


This is the kind of task that suits a machine. Reading two long documents side by side and flagging every place a value in one contradicts a value in the other is tedious for a person and well within reach for software. Tools like[Avoice](https://www.avoice.co/) are built for exactly this. It ingests a firm's existing specifications, schedules, and drawings, then flags inconsistencies between them before they reach site, rather than waiting for a contractor's RFI to surface the problem on someone else's terms.


The useful part is that it works on the documents you already have.[Avoice](https://www.avoice.co/) reads the architectural spec, the structural spec, the door and window schedules, and the project drawings as one connected set, and surfaces where a fire rating, a concrete grade, or a product reference appears in two places with two different answers. Because it structures its output around recognised standards like Uniclass and CAWS, a flagged item points to where in the specification the conflict actually lives, not a vague warning you then have to chase down by hand.


None of this removes the architect's judgement. The machine doesn't decide whether the 60 minute door or the 30 minute door is correct. It makes sure you're looking at both before the door is ordered. The decision stays with you. The hunting, which is the expensive and error-prone part, doesn't.


### A coordination routine for Stages 3 and 4


If you want a practical starting point, build the discipline around three habits. Agree the responsibility boundary in writing at Stage 3, naming every shared element, concrete, fixings, fire protection, waterproofing, movement joints, and tolerances, and assigning each to one party. Treat the specification and the schedules as a single checkable set at Stage 4, not as separate documents that happen to describe the same building. Then run a cross-document check before every issue, by hand or with a tool, so the conflicts and gaps surface in the office rather than on site.


The firms that do this consistently aren't the ones with the biggest teams. They're the ones who've made the check routine instead of heroic. A platform like[Avoice](https://www.avoice.co/) helps because it makes that routine cheap enough to run on every project, not just the ones where someone happened to remember.


### The gap closes in the office or on site


Every coordination gap gets resolved eventually. The only question is where, and at what cost. Caught at Stage 4, a contradiction between the structural spec and the architectural finish is a five minute conversation and a clause edit. Caught on site, it's a variation, a delay, and a contractor who now holds the upper hand in the conversation about who pays. The work is the same. The price isn't.


Specification coordination has always rewarded the careful and punished the rushed. What's changed is that the careful part no longer has to be the slow part. If you want to see how that works on a real set of project documents,[Avoice](https://www.avoice.co/) runs demos tailored to the way your practice writes specs. The gap between your architectural and structural specifications is going to close one way or another. Far better in the office than on site.


See how Avoice replaces the busywork.


Book a demo and see how the platform handles specifications, QA/QC and construction admin without adding tools to your stack.


[Book a demo](https://www.avoice.co/demo)
