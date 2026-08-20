---
schema_version: "1.0.0"
document_id: "b8f99cbb02ee89509c43192ba3d977aade8acfb315b1b074fb55836bf2e27cce"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/rfq"
published_at: "2026-05-09T00:00:00+00:00"
first_seen_at: "2026-08-09T23:40:56.159161+00:00"
fetched_at: "2026-08-09T23:40:57.548506+00:00"
content_hash: "sha256:01010d95da4338e80a7a7a998e25f0fe92ab1908087e8fedb5254f89f99e9369"
---

# What Is an RFQ?

A request for quotation, or RFQ, is a formal request a buyer sends to one or more suppliers asking for a price, lead time, and terms on a specific, defined scope of goods or services. Suppliers respond with a quote. The buyer compares quotes, picks a winner, and converts the winning quote into a purchase order.


That's the textbook version. In practice, an RFQ is rarely just a price ask. It's a packet that includes part numbers, drawings or specs, target quantities, delivery terms, payment terms, quality requirements (often AS9100 for aerospace, IATF 16949 for automotive, FDA 21 CFR Part 820 for medical devices), and sometimes a non-disclosure agreement. The supplier needs every piece of that to quote accurately. Send a half-built RFQ and you'll get a half-built quote back, or no response at all.


## RFQ vs RFP vs RFI: The Distinction Buyers Actually Care About


These three get conflated constantly. The difference matters because using the wrong one wastes everyone's time.


An RFI, request for information, is exploratory. You're asking suppliers to tell you about themselves and their capabilities. No commitment, no scope, no pricing. RFIs are useful when you're qualifying a brand new category and don't know which suppliers can even do the work.


An RFP, request for proposal, is for complex scope where you want suppliers to propose a solution. Software implementations, engineering services, multi-year programs. The supplier brings creativity to the table.


An RFQ is the opposite. The scope is fixed. You know exactly what you want. You're asking for price and delivery on something specific. If you're sending an RFP for a stamped bracket, you're doing it wrong. Send an RFQ. If you're sending an RFQ for a custom enterprise software platform, you're also doing it wrong. Send an RFP.


## What Goes Into a Real RFQ Packet


A serviceable RFQ has more than most buyers think.


The bill of materials or part list, with revision numbers. Drawings or 3D models, current revision, with title block visible. Tolerances and material specs called out. Annual usage volume and a target order quantity. Required lead time, or at least a "needed by" date. Quality requirements, including any industry certifications and PPAP level for automotive. Packaging and labeling requirements. Inspection and acceptance criteria. Delivery point and Incoterms. Payment terms (Net 30, Net 60, 2/10 Net 30). Tooling expectations: amortized into piece price, paid upfront, or supplier-owned.


The RFQs that come back fast and accurate are the ones where the supplier didn't have to ask a single follow-up question. The RFQs that drag on for three weeks are the ones where the buyer left out tolerances or didn't include packaging spec, and the supplier had to ping back twice before they could quote.


If you've ever wondered why one supplier returns a quote in 3 days and another takes 17, the difference is usually the packet, not the supplier.


## Why RFQ Cycles Drag


The "send the RFQ, get a quote back" model sounds linear. It almost never is.


Most buyers send the same RFQ to 4 or 5 suppliers and then spend the next two weeks chasing them. Three respond on time. One responds with questions that the buyer should have answered in the original packet. One ghosts entirely until the buyer follows up three times, at which point they say they can't quote because they're at capacity and didn't want to say so.


When the quotes do come in, they're in different formats. One supplier sends an Excel sheet. One sends a PDF with a hand-typed table. One emails the prices in the body of the email with no document at all. One quotes in pieces of 1,000 when the RFQ asked for pricing at 500, 1,000, and 5,000. Now the buyer is rebuilding all of it into a comparison spreadsheet, manually, often at 10pm before a stage gate review.


The teams we talk to who run a lot of RFQs (machine shops sourcing castings, contract manufacturers sourcing components, EMS firms running NPI) say RFQ admin eats 30 to 60 percent of a buyer's week. That's not the strategic part. That's email triage and copy-paste.


## The Hidden Cost of a Mid-Tier RFQ


There's a reason senior buyers hate RFQs for one-off, low-dollar parts. The fully loaded cost of running a competitive RFQ (buyer time, engineering review, supplier capacity used up, comparison work, negotiation) is often higher than the savings the RFQ produces on a small part.


The opinion: most companies RFQ too aggressively on parts under $10K annual spend. The buyer time would be better spent on the top-50 spend categories. Run a Pareto on your spend. The 80/20 isn't a cliche here, it's the actual answer.


When RFQs do matter (high-spend parts, new programs, supplier consolidation, tariff-driven resourcing), they really matter. Those are the ones to over-invest in.


## How Companies Run RFQs Today


Three common setups, all flawed in different ways.


Email and Excel. The buyer maintains an RFQ tracker in a workbook with a tab per supplier, manually pastes quotes in, and emails follow-ups one at a time. This is what 70 percent of procurement teams under 200 people are doing. It works until the buyer leaves and takes the supplier knowledge with them.


ERP-based RFQ modules. SAP, NetSuite, Oracle, Epicor, JDE all have an RFQ module of some kind. They mostly work but suppliers hate them. NetSuite's vendor portal in particular requires the supplier to log in, which most small machine shops will not do. Result: buyers fall back to email and the ERP module sits unused.


Sourcing platforms (Coupa, Jaggaer, Ariba). Capable but heavy. Coupa's supplier onboarding is a multi-step flow that small suppliers abandon. Jaggaer's RFQ workspace is powerful but the learning curve eats months. The platforms make sense for enterprise teams running 200+ RFQs a quarter. For a 10-person procurement team running 30 RFQs a month, the implementation cost is bigger than the value.


The new wave is AI-driven RFQ tools that meet suppliers where they already are: in email. The supplier responds in their preferred format, the system parses the quote, normalizes it, and feeds it into a comparison view. It's a different bet: instead of forcing suppliers into a portal, automate the buyer's side of the email back-and-forth.


## Where Lumari Fits


Lumari runs RFQs for procurement teams without forcing suppliers into a portal. It sends the RFQ, follows up on a cadence, parses the quotes that come back as PDF, Excel, or email body text, and pushes a normalized comparison into the buyer's hands. Suppliers keep working how they already work. The buyer stops spending Tuesday night rebuilding spreadsheets.


If your buyers are spending more time chasing quotes than evaluating them,[see how Lumari runs RFQs end-to-end](https://lumari.ai/) . It's the same playbook a senior buyer would run, minus the manual parts.
