---
schema_version: "1.0.0"
document_id: "71fc90a258f2ac04b7ebca829962213b80c903791a0d758c164f0490597272b2"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/rfi-vs-rfp-vs-rfq"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-24T10:12:39.972373+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:9e8d67dffa7b31a3a6203834e3fe003ec4e364cc16ad899270b0a0d22975dce0"
---

# RFI vs RFP vs RFQ: When to Use Each (And When to Skip One)

A category manager we talked to last year ran a full RFP for a batch of cold-rolled steel brackets. Eleven-page document, weighted scoring rubric, three rounds of clarification calls. The suppliers all came back with roughly the same thing: a price and a lead time. Which is exactly what an RFQ would have gotten her in a quarter of the time. She'd spent three weeks formalizing a decision that hinged on one number.


That's the core of the RFI vs RFP vs RFQ question. The three documents look similar from a distance, and procurement people use the terms loosely, but they're built for completely different situations. Pick the wrong one and you either waste your own team's weeks or you waste your suppliers' goodwill. Both cost you later.


## What's the Difference Between RFI, RFP, and RFQ?


An RFI (request for information) gathers capabilities and qualifies suppliers before you have a defined need. An RFP (request for proposal) asks suppliers to propose a solution to a complex or open-ended requirement, with price as one factor among many. An RFQ (request for quotation) asks for price, lead time, and terms on a fully defined scope where you already know exactly what you want. RFI explores, RFP evaluates, RFQ buys.


That's the snippet version. It's correct and it's also where most articles stop, which is why most articles aren't useful. The interesting part is what happens when these run in the real world, in actual ERP systems, with suppliers who don't want to fill out your forms.


## The Comparison Table


Document


What it asks for


When to use it


RFI


Supplier capabilities, certifications, capacity, general background


You're qualifying a new category and don't yet know who can do the work


RFP


A proposed solution to an open-ended need, with pricing as one input


Scope is complex or undefined and you want suppliers to bring approach


RFQ


Price, lead time, and terms on a fixed, fully specified scope


You know exactly what you want and you're choosing mostly on number


RFx


Umbrella term for all three


When someone wants to sound like they read the procurement glossary


One scope, three jobs. The RFI sizes the field. The RFP shapes the work. The RFQ closes the price. If your situation doesn't clearly map to one of those three jobs, you're probably reaching for the wrong document.


## When Should You Use an RFQ Instead of an RFP?


Use an RFQ when the only real variable left is price. You've got the part number, the drawing at the current revision, the tolerances, the annual volume, the quality requirements. There's nothing for a supplier to "propose." They just need to quote it. A machined housing, a run of injection-molded parts, a commodity fastener, a known chemical at a known grade: all RFQ territory.


Use an RFP when there's genuine room for suppliers to differ in approach, not just in price. A new packaging line. A contract manufacturing relationship where the supplier owns process decisions. A custom test fixture where you've described the problem but not the design. In those cases the cheapest bid can be the worst outcome, and you want suppliers to show their thinking.


Here's the take that gets pushback: most manufacturers over-formalize. They run RFPs for parts that just need an RFQ because the RFP feels more rigorous, more defensible, more like "real" procurement. It isn't. It's slower. A 12-page RFP for a stamped bracket signals that the buyer doesn't trust their own spec. If you can write a complete drawing package, you don't need a proposal. You need three quotes. For the full anatomy of doing that well, here's[what actually goes into an RFQ packet](https://lumari.ai/blog/rfq) .


The flip side is just as real. Sending an RFQ for something that genuinely needs design input gets you three confident quotes for three different things, none of them comparable, and a buyer who finds out at PO time that "they all quoted it differently." If the scope isn't nailed down, an RFQ just hides the ambiguity instead of resolving it.


## Can You Skip the RFI?


Usually, yes. And for direct materials, you almost always should.


The RFI made sense in a world where you didn't have a way to find out who could machine a titanium part to AS9100 without writing to each shop and asking. That world is mostly gone. Your engineering team already knows the qualified shops. Your approved supplier list already exists (or should, see[why your approved supplier list keeps rotting](https://lumari.ai/blog/approved-supplier-list) ). Trade databases, ThomasNet, a Google search, and a 20-minute capability call will tell you more, faster, than a formal RFI that takes a supplier two hours to fill out and you a week to score.


So here's the contrarian position: the RFI is mostly dead for direct materials. It survives in two places. One, regulated and public-sector buying where the RFI is a procedural requirement, not a discovery tool. Two, genuinely new categories where you're sourcing something your company has never bought and you can't name a single qualified supplier. Outside those, an RFI is a form you make suppliers fill out so you can feel thorough.


Where the RFI does earn its keep is at scale and in services. If you're standing up a new plant in a region where you have zero supplier relationships, an RFI to forty local fabricators is a reasonable way to thin the herd before you spend real time. For indirect and services categories, where capability and fit matter more than a part print, the RFI does actual work. The difference comes down to direct vs indirect, which is its own rabbit hole:[direct and indirect procurement run on different logic](https://lumari.ai/blog/direct-vs-indirect-procurement) .


## Where These Documents Actually Live (The System Reality)


The textbook diagram shows a clean RFI-then-RFP-then-RFQ funnel inside one sourcing platform. That isn't how it works for most teams.


In a sourcing suite like Jaggaer or Coupa, all three exist as event types, and the supplier is expected to log into a portal to respond. The catch is that small and mid-size suppliers, the machine shops and the regional fabricators, won't reliably do that. Coupa's supplier portal has an onboarding flow most small shops abandon partway through. So the buyer creates the RFQ event in the system, the supplier ignores the portal invite, and the whole thing falls back to email anyway. The "event" in the platform becomes a record of a process that's actually happening in Outlook.


In an ERP, RFQ functionality is buried in a transaction most buyers route around. SAP has the RFQ document type (it lives in the same purchasing flow as the PO). NetSuite has a request-for-quote record tied to vendors. Epicor and Oracle have their versions. They technically work. But they assume structured supplier responses, and supplier responses come back as a PDF, an Excel sheet, or three prices typed into the body of an email. So the buyer copies the quote out of the email and pastes it into the ERP by hand, or more often, into a comparison spreadsheet that lives outside the ERP entirely.


That gap, between the formal document the system expects and the messy reply the supplier actually sends, is where most RFQ time disappears. We did a[breakdown of where the hours actually go](https://lumari.ai/blog/manual-rfq-time-study) , and it's almost never the part anyone planned for. It's the chasing and the retyping.


## How Direct-Materials Sourcing Bends the Rules


Most RFI/RFP/RFQ advice on the internet was written for indirect spend and services: buying a marketing agency, an IT system, a facilities contract. That's where the RFP shines and the RFI does real qualifying work. Direct materials behave differently, and the standard advice misleads.


For direct materials, the spec usually already exists. Engineering produced a drawing, a BOM, a material callout. The "what" is decided before procurement gets involved. That collapses the funnel: you rarely need an RFP because there's nothing to propose, and you rarely need an RFI because your qualified suppliers are a known set. You mostly need RFQs, run well and run often.


It also changes what "comparing responses" means. In an RFP you're weighing approach, references, risk, and price across a rubric. In a direct-materials RFQ you're normalizing apples to slightly-different apples: one supplier quoted at 1,000 pieces, another at 1,500, one bundled tooling into piece price, one quoted tooling separately, one used a different Incoterm so the landed cost isn't even visible in the number they gave you. The comparison is arithmetic, but it's tedious, error-prone arithmetic that happens at 9pm before a stage gate. Doing it cleanly is a real skill, and[normalizing quotes for a true comparison](https://lumari.ai/blog/quote-comparison) is most of the job.


One more wrinkle specific to direct materials: the RFQ isn't always competitive. Plenty of direct buying is single-source by design, because of a qualified tool, a sole supplier of a custom part, or a long-term agreement. In that case the "RFQ" is really a price refresh on a relationship, and dressing it up as a competitive event fools nobody, least of all the supplier who knows they're the only option.


## What Teams Get Wrong Most Often


A few patterns show up again and again.


Running an RFP when an RFQ would do. The most common and the most expensive in calendar time. If your spec is complete, you don't need proposals. You need quotes.


Running an RFQ when the scope isn't actually fixed. The opposite error. You get three quotes that aren't comparable because each supplier filled the gaps in your spec differently, and you discover the mismatch after you've already picked a winner.


Treating the RFI as a hoop instead of a tool. Sending a long capability questionnaire to suppliers you've already worked with, or to a category where you already know the players, burns supplier goodwill. Suppliers remember which buyers waste their time, and they price that in, or quietly deprioritize your next RFQ.


Skipping qualification entirely and RFQ'ing strangers. The opposite of over-RFI'ing. Sending a real RFQ to a shop you've never vetted, getting a great price, and then learning at first article that they can't actually hold the tolerance. The RFI exists for exactly this case, the truly unknown supplier in a truly new category. Skipping qualification there is how you end up with a cheap quote and a quality escape.


And the quiet one: confusing the document with the outcome. The RFI/RFP/RFQ is paperwork. The outcome is a good supplier at a fair price who actually delivers. We've watched teams run flawless RFP processes that produced a supplier who missed every date. The process is not the point. The decision is.


## A Simple Decision Rule


If you can write a complete spec and the choice comes down to price and delivery: RFQ.


If the scope is open enough that suppliers will solve it differently and you want to weigh approach, not just cost: RFP.


If you genuinely don't know who can do the work and need to thin a field of unknowns before you spend real time: RFI. Otherwise, skip it.


Most direct-materials teams will land on RFQ four times out of five, occasionally RFP, and rarely RFI. If your mix looks heavier on RFPs and RFIs than that, the question to ask isn't "which document," it's "why are we formalizing decisions that don't need it." Speed is a procurement advantage that nobody puts on a scorecard.


## Where the Real Work Hides


Whichever document you send, the work that eats the week is the same: chasing the suppliers who didn't respond, parsing the replies that came back in five different formats, normalizing them into something comparable, and keeping your system of record current while you do it. The RFI/RFP/RFQ distinction decides how you start. It does nothing for the slog in the middle.


[Lumari runs that middle for procurement teams](https://lumari.ai/) : it sends the RFQ, follows up with the suppliers who go quiet, pulls pricing and lead times out of whatever format the quotes arrive in (PDF, Excel, email body), and hands the buyer a clean comparison instead of a 10pm spreadsheet. Pick the right document for the situation, then stop spending your evenings doing the data entry behind it.
