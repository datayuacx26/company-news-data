---
schema_version: "1.0.0"
document_id: "d74ed6d4b5ee8600f0d570275add2fa65822f7d7c4ef604e874ed000729573ab"
company_key: "yc-avoice"
company: "Avoice"
source_id: "yc-avoice-news-import-1664a7132626"
canonical_url: "https://www.avoice.co/blog/how-to-build-a-qa-qc-checklist-for-architectural-specifications"
published_at: "2026-06-13T00:00:00+00:00"
first_seen_at: "2026-08-09T19:33:57.958959+00:00"
fetched_at: "2026-08-09T19:33:58.487003+00:00"
content_hash: "sha256:9cdee5be34f5b89598be6af3d71b98dc303d8162d210aa583859237a7f5cfbbf"
---

# How to Build a QA/QC Checklist for Architectural Specifications

Most specification errors aren't dramatic. A fire rating reads one way on the drawing and another in the spec. A Uniclass code gets copied from last year's project. A performance clause sits untouched in a template that nobody edited. None of it looks like much on screen, and all of it costs money on site. A QA/QC checklist for architectural specifications is the cheapest insurance a practice can buy against exactly this kind of quiet mistake.


The trouble is that most checklists are either too vague to help or so long that people stop using them by the third project. The aim here is a checklist an architectural technologist can actually run before issue, in an hour or two, and trust. That means knowing what to check, in what order, and why each item earns its place.


### Why a checklist beats a careful read


A careful read finds the errors you're looking for. It misses the ones you've stopped seeing. Anyone who has written specs for a few years develops blind spots around their own habits, and a fresh read of a familiar document tends to confirm what you expect rather than catch what's wrong.


A checklist works differently. It forces a fixed set of questions regardless of how confident you feel about the document. The same logic underpins the surgical checklists that cut complication rates in hospitals and the pre-flight checks pilots run even after thousands of hours. The expert doesn't need reminding of the basics. The expert needs protecting from the one time the basics slip.


Specification quality control is the same problem in a different setting. Your checklist isn't there to teach you how to write a spec. It's there for the Friday afternoon when a deadline is looming and three packages are going out at once.


### What a specification QA/QC checklist actually checks


Start with completeness. Every system shown on the drawings needs a corresponding clause, and every clause needs a system that uses it. Orphan clauses are as dangerous as missing ones, because a contractor will price a specified item whether or not it appears anywhere on the job. Check that performance criteria are present and measured, not just named. "High performance glazing" means nothing. A U-value, a g-value, and an acoustic rating mean something.


Then check substitutions and proprietary references. A spec that names a single product without an "or equal approved" route can box a contractor into a sole supplier, and a spec that names a product discontinued two years ago will generate an RFI before the first pour. Material references should be live, current, and traceable to a real datasheet.


Workmanship and execution clauses deserve a line of their own, because they're the ones most often left generic. A clause that says "install in accordance with manufacturer's instructions" without naming tolerances, testing, or inspection hold points gives a contractor little to build to and gives you little to hold them to. Check that the spec says who inspects what, when, and against which standard.


### Coordination between the spec and the drawings


This is where most of the costly errors live. Your window schedule says one thing about an opening size. Your spec implies another through its reference to a standard frame depth. The schedule of quantities counts forty doors. The door spec describes thirty eight. None of these contradictions are visible if you read the spec on its own, which is exactly why a coordination check has to be a deliberate step.


Fire is the area that deserves its own line. Fire ratings should match across the drawings, the spec, and the schedules, and they should reflect the actual compartmentation strategy rather than a default carried over from a previous job. Part B coordination errors are the ones that surface late, cost the most to fix, and carry the heaviest professional risk. A spec review that doesn't cross reference fire ratings against the drawings isn't finished.


Acoustic and thermal performance follow the same pattern. Part E and Part L values in the spec need to agree with what the drawings and calculations assume. When they drift apart, the contractor builds to one figure and building control assesses against the other, and somebody pays to reconcile the difference.


It helps to remember what a coordination error becomes downstream. Most of them arrive back at the practice as an RFI, then a variation, then a delay, and the cost grows at every step. An inconsistency that takes two minutes to fix in the spec can take two weeks to resolve once a contractor has priced it, ordered against it, and started building. The whole point of the check is to catch the thing while it's still cheap.


### Classification, references, and standards


Classification is where a lot of UK specs quietly fall apart. If you're working to Uniclass 2015, every section needs the correct code, and the codes need to be consistent with the way the rest of the project is structured. A mix of Uniclass and old CAWS references in the same document confuses everyone downstream, especially on BIM projects where the classification carries information through to the model. Check that British Standards and Approved Document references are current versions, because standards get withdrawn and superseded more often than most templates get updated.


Citing a withdrawn standard is one of the most common errors in older specs, and it's invisible unless someone checks the reference against the current list. The same goes for clause numbers carried across from a prior project. They look authoritative. They might point at nothing.


### How to build the checklist so people actually use it


A checklist nobody runs is worse than no checklist, because it creates a false sense of process. Keep it to the errors that actually happen on your projects rather than every error that could theoretically occur. Most practices find that a list of around twenty checks covers the overwhelming majority of real problems, and that list will look slightly different for a housing specialist than for a practice doing schools or labs.


Write each item as a question with a yes or no answer, not a vague instruction. "Are all fire ratings consistent across spec, drawings, and schedules?" beats "Check fire." Group the questions by theme so the person running the check moves through completeness, coordination, classification, and compliance in a logical order. Assign the check to someone who didn't write the spec, because the value of specification quality control comes largely from a second set of eyes that doesn't share the author's assumptions.


Keep a record of what each check finds. Over a few projects, the pattern of errors tells you where your templates are weak, and that's far more useful than fixing the same mistake quietly every time. Treat the checklist as a living document too. When a new failure mode bites you on a job, add the question that would have caught it, and retire the questions that never find anything. A list that grows with the practice stays trusted. A list that ossifies gets ignored.


### Where automation changes the maths


A manual checklist depends on a person having the time and focus to run it properly, and that's the part that breaks under deadline pressure. This is where AI-powered specification tools start to change the calculation. Platforms like[Avoice](https://www.avoice.co/) can read a specification against the drawings and schedules and flag the contradictions automatically, so the coordination check that used to take an afternoon happens in the background as you work.


The useful thing isn't that the software replaces the checklist. It's that it runs the mechanical parts of the checklist continuously and without fatigue.[Avoice](https://www.avoice.co/) ingests a practice's own sheets, schedules, specs, and material libraries, then checks new output against that body of work, so a fire rating that disagrees with the drawing or a Uniclass code that doesn't match the rest of the project gets surfaced before issue rather than after. The standards references stay current because the system structures output around recognised classifications like Uniclass and CAWS rather than whatever happened to be in the last template.


That frees the human part of the review for the judgement calls that genuinely need an architect: whether a performance specification suits the procurement route, whether a substitution clause carries acceptable risk, whether the design intent actually survives the way it's been written down.


### Making the checklist part of the workflow


A QA/QC checklist only works if it runs at the right moment, which is before issue and ideally at each RIBA stage gateway rather than once at the very end. Build it into the point where a package gets signed off, so it becomes a condition of issue rather than an optional extra. The earlier a contradiction surfaces, the cheaper it is to fix, and a check at Stage 4 costs a fraction of the same error found on site.


Whether you run the check by hand or let a tool like[Avoice](https://www.avoice.co/) handle the mechanical parts, the principle holds. The errors that cost the most are rarely the ones that look difficult. They're the small inconsistencies that survive because everyone assumed someone else had checked. A good checklist makes sure someone did.


If you want to see how much of that checking can run automatically against your own documents, the[Avoice](https://www.avoice.co/) AI Spec Agent is built to do exactly this kind of coordination and compliance checking on real project files. Start with one live project, point it at the spec and the drawings, and see what it finds.


See how Avoice replaces the busywork.


Book a demo and see how the platform handles specifications, QA/QC and construction admin without adding tools to your stack.


[Book a demo](https://www.avoice.co/demo)
