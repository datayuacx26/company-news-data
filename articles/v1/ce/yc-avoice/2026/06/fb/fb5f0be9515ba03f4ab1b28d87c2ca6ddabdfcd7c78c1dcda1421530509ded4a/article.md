---
schema_version: "1.0.0"
document_id: "fb5f0be9515ba03f4ab1b28d87c2ca6ddabdfcd7c78c1dcda1421530509ded4a"
company_key: "yc-avoice"
company: "Avoice"
source_id: "yc-avoice-news-import-1664a7132626"
canonical_url: "https://www.avoice.co/blog/why-qa-qc-is-the-most-undervalued-stage-of-specification-writing"
published_at: "2026-06-20T00:00:00+00:00"
first_seen_at: "2026-08-09T19:33:57.958959+00:00"
fetched_at: "2026-08-09T19:33:58.487003+00:00"
content_hash: "sha256:742e0600f0b6a2d476ab28082d7108a438a16e99b5ab0e48b05c142183b44524"
---

# Why QA/QC is the most undervalued stage of specification writing

Most practices treat the specification review process as a formality. The document gets written, someone gives it a quick read, and it goes out with the tender package. That habit is quietly expensive. The hours you save by not checking properly come back later as RFIs, variations, and the occasional awkward call with your professional indemnity insurer.


### The stage everyone skips


Quality control on a specification isn't glamorous. It doesn't win competitions or impress a client at a design review. But it's the step that decides whether your carefully resolved drawings actually get built the way you intended. Skip it, and you're trusting that nobody on site reads the spec too closely.


The work gets cut for a simple reason. It sits right at the end of RIBA Stage 4, when the tender date is fixed and the design has changed a dozen times since the spec was first drafted. Checking is the one task with no client chasing it. So it shrinks to a quick skim, and the document goes out carrying contradictions nobody looked for.


### Where the cost actually lands


A single request for information costs a project somewhere between a few hundred and a few thousand pounds once you count the design team's time, the contractor's query handling, and the delay while everyone waits for an answer. A mid-sized project can generate two or three hundred RFIs. A meaningful share of those trace back to a spec that contradicted a drawing, named a discontinued product, or left a performance requirement vague enough to argue about.


Variations are worse. When the spec calls for one thing and the drawing shows another, the contractor prices the cheaper reading and claims the difference as a variation when you correct it. You absorb the cost or you argue about it. Either way you've spent money and goodwill on a problem that a careful check would have caught for nothing.


Then there's the insurance angle. A large share of professional indemnity claims against architects come down to documentation, not design. A spec that promised a fire rating the installed product couldn't meet, or a clause that quietly transferred a risk you never meant to accept, is exactly the sort of thing that surfaces as a claim three years later. Good QA is the cheapest premium you'll ever pay.


The schedule of quantities is where these small errors compound. A contractor prices what's written, not what you meant. If the spec under-describes a system, the price comes in low and the shortfall lands on you as a variation when reality catches up. If it over-describes, you've paid for belt and braces nobody needed. Either way, the review that would have squared the spec against the schedule was the cheapest fix available, and it's the one that got cut.


### What a real specification review process looks like


A proper review checks three things that rarely get checked together. First, internal consistency: does every clause agree with every other clause, and does the spec match the drawings and schedules. Second, completeness: is every specified system actually documented, and does every drawing reference point to a clause that exists. Third, correctness: are the products current, the standards the right edition, and the performance requirements achievable.


Most reviews only manage the third, and only partly. Checking that your window schedule matches your specification, line by line, across forty openings, is the kind of task people do badly. It's repetitive, it's detailed, and it happens at the end of a project when everyone is tired and the deadline is tomorrow. That's not a criticism of anyone. It's just how the work is shaped.


### The coordination gap between spec and schedules


The hardest errors to catch live in the gaps between documents. Your specification names one glazing unit, your window schedule names another, and your thermal calculations assumed a third. Each document was right when it was written. They drifted apart as the design changed and nobody reconciled them.


This is where Part L compliance quietly breaks. The U-value in the spec, the product in the schedule, and the figure in the SAP or SBEM calculation all need to agree. When they don't, you either find out during building control review or you find out on site. Neither is a good moment to discover it. The error wasn't a lack of knowledge. It was a lack of coordination between three documents that nobody read side by side.


### How this plays out on a real project


Picture a modest school extension at the end of Stage 4. The architect specifies an FD30S fire door to a corridor, with an intumescent seal and a self-closer, classified neatly under the right CAWS section. Three weeks earlier, during a value engineering exercise, the door schedule was updated to a cheaper leaf that only carries an FD30 rating without the smoke seal. Nobody told the spec. Both documents looked finished. Both went out with the tender.


On site, the contractor installs what the schedule says, because the schedule is what they price and order from. Building control picks it up at inspection, the door has to come out, and a four week lead time on the correct set turns into a programme delay with a cost attached. The architect knew the right answer the whole time. The spec even said it. The two documents simply never got read against each other, and that one unchecked line became the most expensive item on the project that month.


This is the texture of most specification failures. Not a dramatic mistake, just a small disagreement between documents that each looked complete in isolation. A review whose only job is to catch that disagreement would have flagged it in seconds. The reason it didn't happen is the same reason it usually doesn't. There was no time, and no spare reviewer, at the exact moment it mattered.


### Why the review keeps getting skipped


The honest reason QA gets cut is that it sits at the worst point in the programme. Stage 4 runs late, the tender date is fixed, and review is the one task with no external deadline policing it. Nobody chases you for a thorough spec check the way a client chases you for drawings.


There's also a skills question. The person who wrote the spec is often the worst person to review it, because they read what they meant rather than what they wrote. A fresh pair of eyes helps. In a small practice there often isn't a spare pair to offer, so the spec gets marked by the same person who drafted it, at eleven at night, the day before tender.


### How AI changes the economics of review


This is the part of the specification review process where the maths has changed. Cross-checking a spec against schedules and drawings is exactly the kind of structured, rule-bound comparison software handles well. Tools like[Avoice](https://www.avoice.co/) read a firm's specifications, schedules, and drawings together and flag where they disagree, before the package goes out rather than after it comes back.


What makes this useful rather than gimmicky is the grounding.[Avoice](https://www.avoice.co/) generates and checks specifications against recognised standards, with output classified under Uniclass and CAWS, drawing on a practice's own documentation rather than a generic clause library. A check that knows your window schedule, your material library, and the right edition of the standard can catch the mismatch between a specified fire door rating and the product actually scheduled. That's the error that costs you on site.


None of this removes the architect from the loop. It removes the tedium that makes review get skipped in the first place. When the line-by-line comparison takes minutes instead of an afternoon, the review actually happens, on every project, not just the ones with slack in the programme.


### What QA can and can't catch


It's fair to be clear about the limits. A review, automated or not, can tell you that your spec contradicts your schedule. It can't tell you that your design intent was wrong in the first place. Judgement about whether a build-up suits the context, whether a product will weather well, whether a detail is right for the site, stays with you. That's the work only an architect can do.


What QA catches is the gap between what you decided and what you documented. That gap is where most specification errors actually live. They're rarely failures of knowledge. They're failures of coordination, the small contradictions that creep in as a project changes and the documents fall out of step. A platform like[Avoice](https://www.avoice.co/) that ingests your past projects, your standard clauses, and your schedules can check the spec against the way your practice actually works, not against a template that was never yours.


### Where this leaves your practice


Treating QA as the undervalued stage is really an argument about when you pay. You can pay a little now, in review time, or a lot later, in RFIs, variations, and risk that lands on your insurance record. The practices that work this out tend to be the ones that have already felt the cost of getting it wrong.


The interesting question isn't whether the specification review process matters. Everyone knows it does. It's whether the review is cheap enough to do every time, on every project, even when the tender date is tomorrow. That's the part that's changing. If you want to see how a spec check that runs in minutes rather than hours fits into the way you already work, it's worth looking at how[Avoice](https://www.avoice.co/) handles it.


See how Avoice replaces the busywork.


Book a demo and see how the platform handles specifications, QA/QC and construction admin without adding tools to your stack.


[Book a demo](https://www.avoice.co/demo)
