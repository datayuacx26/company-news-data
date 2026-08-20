---
schema_version: "1.0.0"
document_id: "16dfa4eca811d1538de24d3f747f0d37f3e28b262ecc0b242ee5dfda445dd087"
company_key: "yc-lumari"
company: "Lumari"
source_id: "yc-lumari-news-import-140e3221891d"
canonical_url: "https://lumari.ai/blog/procurement-workflow-examples"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-08-09T23:40:56.159161+00:00"
fetched_at: "2026-08-09T23:40:57.548506+00:00"
content_hash: "sha256:522cf5558fbe122c1b4fdfe668167a2474a44101d805c3a1407db654126f160c"
---

# Procurement Workflow Examples: 5 Real Flows From Manufacturing Teams

Most published procurement workflow diagrams are useless. They show six tidy boxes: Identify Need, RFQ, Quote, PO, Receive, Pay. Arrows go one way. Nothing branches. Nobody has to chase a supplier for a ship date on a Friday afternoon while production is screaming about a line-down at 4pm.


If you're trying to document or redesign your team's procurement workflow, the textbook version is a bad reference point. Below are five real procurement workflow examples from manufacturing teams we've talked to over the last year. Each one shows what the workflow actually looks like (named systems, named roles, real branches), where it breaks, and what the team automated or wishes they could. Anonymized, but specific.


## Why the Textbook 6-Step Procurement Workflow Is Useless


A real manufacturing procurement workflow has 40 steps, four loops, two off-system handoffs, and at least one Slack message that reads "wait did anyone actually send the PO?"


The diagram everyone publishes shows a process. What actually runs is a switchboard: emails from suppliers, RFQs going out, planners flagging late shipments, engineering tossing in a revision mid-bid, finance asking why the GL doesn't match the PO. The buyer routes all of it, and the workflow is whatever it takes that week to keep the line running.


Don't start from the six-box version. Start from what your buyers do on a Tuesday. Here's what that looks like at five shops.


## Workflow 1: New Part Sourcing for a Custom Electromechanical Assembly


**The shop:** 200-person EMS company in the Midwest, ~$80M revenue, makes industrial control assemblies for ag and rail. Runs NetSuite. Three buyers, one sourcing manager.


Engineering releases a new BOM for a control panel. Custom aluminum casting on it: $14K annual spend, no current supplier. Anna, the buyer, needs three quote-ready suppliers in ten business days so the project doesn't slip its phase-gate review.


What she does:


1. Pulls the print and spec from Arena PLM as a revision-stamped PDF.
2. Opens her "preferred castings" tracker. Excel file on the shared drive, ~60 shops by region. Last meaningful update: 14 months ago. Two contacts have left their companies.
3. Picks five candidates. Two in Indiana, two in Wisconsin, one in Ohio.
4. Drafts the RFQ email in Outlook. Copies the standard NDA from a Word doc. Attaches print, spec, NDA. Sends individually so suppliers don't see each other. BCCs procurement@.
5. Three suppliers respond in 48 hours with clarifying questions. Heat treatment, alloy substitution, machining bundle. Anna routes the alloy question to engineering via Slack. Engineering takes two days.
6. Quotes come back over a week. Two PDFs, one Excel, one in the body of an email. One supplier ghosts after two follow-ups.
7. Anna re-keys quotes into a comparison sheet. Unit price, tooling, lead time, MOQ, terms, FOB. Calculates landed cost manually with a ~6% logistics adder.
8. Two finalists. Sample request. Three weeks. One passes, one has porosity issues.
9. Winner picked. Creates the item master in NetSuite. Sets up the supplier as a vendor (or asks AP, depending on whether they've ever paid them). Issues the PO.


**Where it breaks:** The Excel supplier tracker. Stale, lives on one desktop, doesn't reflect who's quoting fast right now. When Anna was out a week, the backup buyer sourced the same kind of part from three of the wrong shops.


**What they automated:** An Outlook rule that BCCs procurement@ on every RFQ. That's it.


**What 2-3 hours per week would buy them:** A real supplier database with quote history, hit rate, and OTD by part family. Anna spends ~4 hours a week just deciding who to ask. Half of that is detective work she shouldn't have to do.


## Workflow 2: Annual RFQ Refresh Across 200 Approved Parts


**The shop:** Industrial equipment maker in the Southeast, $250M revenue, runs Epicor Kinetic. Five buyers. Once a year they refresh quotes on a portfolio of ~200 high-volume parts to keep cost data fresh and renegotiate where they can.


A slog. The whole event takes ~11 weeks from kickoff to last quote loaded.


The sourcing lead, Marcus, pulls the part list from Epicor with a saved search filtered by annual spend > $5K and quote age > 12 months. Output: 200 lines with part number, description, current supplier, price, annual qty.


He splits the list by commodity (machined, sheet metal, plastics, fasteners, electronics) and assigns chunks to buyers. Most parts are dual-sourced, so each goes to two or three suppliers. 500+ outbound RFQ emails over a two-week window.


Quotes come back in every format imaginable. Big distributors send Excel that almost matches the original sheet. Smaller shops send PDFs. A couple still fax. (Yes, in 2026.) The fax lands in a shared inbox as a TIFF that someone manually transcribes.


Buyers re-key everything into a master comparison sheet. Flag price increases over 5%, ask for justification, negotiate. Then update standard cost in Epicor part by part. 200 manual updates because nobody built the bulk-import workflow.


**Where it breaks:** Format chaos and re-keying. They estimate 60-70% of the 11 weeks is data movement. The negotiation work, the strategic part, is maybe 3 weeks of real thinking.


**What they automated:** A Power Automate flow that takes Excel responses and dumps them into a SharePoint list. Works for ~40% of responses. The other 60% (PDFs, email bodies, the fax) still get re-keyed.


**What would help:** Quote extraction from the messy 60%. If a tool read inbound PDFs and emails and normalized them into one schema, Marcus thinks the cycle goes from 11 weeks to 6. He's not wrong. We've seen this exact pattern at teams who automated quote comparison.


## Workflow 3: Expediting Late Shipments Mid-Production


**The shop:** Contract manufacturer in the Pacific Northwest, ~$45M revenue, mixed mode (some build-to-stock, some build-to-order). Runs Plex. Two planners, two buyers.


Different workflow shape. This one runs hot and fast, sometimes daily.


Tuesday morning. Production manager pings the planner: line 3 is short on a connector and a machined bracket, both promised for delivery yesterday. Without parts by Thursday, they short-ship a Tier 1 auto OEM on Friday. This matters.


The planner pulls Plex, checks PO history. Connector supplier had a confirmed ship date of last Friday. Bracket supplier last Wednesday. Neither updated since. She opens Outlook and drafts two expedite emails.


Connector supplier responds in 20 minutes: shipped Monday, UPS tracking, arrives Wednesday. Crisis 1 averted.


Bracket supplier goes dark. She calls the rep at lunchtime. Voicemail. Calls the main line. Receptionist says the rep is "out today." She pings the buyer: any other contact? Buyer digs through old emails, finds a production manager's direct line from a 2024 quality issue. She calls. The production manager says the part went on hold yesterday for a tooling problem, new ship date is next Tuesday, "didn't anyone email you?" Nobody emailed anyone.


Now she needs an alternate. No second source for this bracket. She calls the engineering manager: can a similar part with minor mods substitute? Two hours of back-and-forth. Yes, with a deviation, signed off by the customer quality contact. Another half day.


By end of Wednesday she's placed a rush PO with a backup machine shop quoting 48-hour turn at 2.3x normal price. Bracket arrives Friday morning. Line 3 ships, barely.


**Where it breaks:** No automatic notification when a ship date changes. The bracket shop didn't email because their hold system doesn't email customers. A 4-day slip became a Tuesday panic instead of a Friday FYI.


**What they automated:** Plex sends a daily exception report showing POs past confirmed ship date by more than 2 days. Helpful, but only after the miss has happened.


**What would actually fix it:** Automatic supplier follow-ups going out 3, 5, and 7 days before a confirmed ship date, asking for re-confirmation. Teams that have built this (some manually, some with software) catch ~70% of slips a week early. The planner here estimates that saves ~3 hours a week of triage and kills most of the rush-buy premium. Full pattern in stop chasing suppliers.


## Workflow 4: Onboarding a New Supplier to the AS9100 AVL


**The shop:** Aerospace machine shop in the Southwest, ~150 people, ~$60M revenue. AS9100 certified. Custom precision parts for primes and tier 1 aerospace. Runs SAP B1 plus a separate quality management system. Two buyers, one quality engineer, one supply chain manager.


Heavy because AS9100 requires it to be heavy. Approving a new supplier for production parts is a 6 to 8 week effort, not a one-day form.


The supply chain manager identifies a candidate (small CNC shop in Arizona). She sends a supplier qualification package: capability questionnaire, quality survey, ITAR self-cert, financial stability statement, AS9100 or ISO 9001 cert request, insurance certificate. Email with 5 PDF attachments and a Word form.


The supplier returns the package in pieces over two weeks. Some via email, some via a secure file transfer set up for ITAR documents. Cert and insurance land in one shared-drive folder. The questionnaire in another. No single source of truth for "where is supplier X in onboarding."


Quality engineer reviews, flags two issues (insurance limit too low for their tier, AS9100 cert expires in 4 months). Email exchanges. Insurance gets bumped. Supplier provides registrar documentation showing recert is in progress.


Quality engineer schedules a remote audit. 4-hour video call. Reviews process control docs, calibration records, FAI procedure, non-conformance handling. Writes findings.


Then PPAP. First article kit goes out (drawing, GD&T spec, sample fixture). Supplier runs samples, submits FAIR per AS9102. Quality reviews and approves. Only then does the supply chain manager add the supplier to the AVL, set payment terms in SAP B1, and let the buyer issue a PO.


**Where it breaks:** Document tracking. No system says "Acme Machining is at step 4 of 9, waiting on revised insurance cert." The supply chain manager keeps an Excel file. The quality engineer keeps a different Excel file. They occasionally reconcile.


**What they automated:** Almost nothing. AS9100 audit pressure makes them risk-averse about anything that isn't paper-trail-friendly. They did standardize the qualification package as a single PDF kit instead of 8 separate documents.


**What 2-3 hours per week would buy them:** A status tracker for in-flight onboardings. Not a portal (the suppliers won't log in). Just one internal place to see where each candidate is. The supply chain manager estimates ~5 hours a week lost to "where are we with \[supplier\]?" conversations. More in[supplier onboarding](https://lumari.ai/blog/supplier-onboarding) .


## Workflow 5: Tariff-Driven Supplier Switch


**The shop:** Mid-size industrial equipment maker in the Northeast. $180M revenue, runs Oracle ERP Cloud. Three buyers. They source a meaningful chunk of mid-volume cast and machined parts from suppliers in China.


A new Section 301 update bumps duty on a category of their parts from 7.5% to 25%. On ~$2.4M annual spend, that's roughly $400K of unbudgeted landed cost hitting next quarter.


The buyer assigned to the category (Kevin) gets the brief from the CFO: find US or Mexico alternatives for the top 30 parts by spend, in 60 days. Quote them, compare landed cost, get engineering sign-off, recommend a switch plan.


Kevin exports the 30 parts from Oracle with current price, qty, annual spend, China supplier. Cross-references his predecessor's "domestic alternates" list (Excel, last updated 2023). 12 of 30 have a known US source. The other 18 he finds from scratch via Thomasnet, OEM application engineers, and a couple of industry Slack groups. ~40 candidate shops in a week.


Sends RFQs. Same pattern as Workflow 1, at scale. He has to explicitly request landed cost with FOB origin so he can compare apples to apples with the Chinese delivered prices.


Quotes come back over 3-4 weeks. Kevin builds a master comparison sheet in Excel. Chinese price + 25% tariff + logistics + customs broker = new landed. US/MX quoted price + domestic freight = new landed. Messy because the Chinese cost basis itself moves with exchange rates and freight.


For each part, Kevin flags "switch makes sense." 14 of 30 come in lower or equal landed cost domestically once the tariff applies. Another 8 are close enough to switch for risk reasons. The remaining 8 are still cheaper from China, mostly because the domestic shops won't commit to MOQs as low as the Chinese suppliers will.


Engineering signs off on the 22 switches. Each new supplier needs material certs and a sample run, which kicks off another 30-day loop. Some switches bounce back on quality. Total project: 4 months from CFO ask to first domestic POs flowing.


**Where it breaks:** The landed cost math. Kevin maintains it manually in a 30-line spreadsheet. When the tariff rate changed (twice, during the project), he re-ran everything. Two "switch makes sense" decisions reversed.


**What they automated:** Excel template with tariff rates as a named range so a rate change only updates one cell. Better than nothing.


**What would help:** Live landed cost comparison pulling current quotes, current tariffs, current freight, and flagging the breakeven. A few teams have built spreadsheet versions. None love them. The pattern of tariff-driven nearshoring is showing up across almost every manufacturer we talk to right now.


## What These 5 Workflows Have in Common


Five different companies, five ERPs, five commodity types. The workflows look nothing alike. But they share a few things worth saying out loud.


**Email is the actual workflow engine.** Every one of these processes runs on email. RFQs out as email. Quotes back as email attachments. Expedites as email. Onboarding documents as email. Tariff responses as email. The ERP is the system of record. Email is the system of work. Anyone documenting a procurement workflow who doesn't start there is missing the actual workflow. We wrote about this in procurement email management.


**The data is in the inbox, not in the system.** Across all five shops, buyers and planners can answer most questions ("did supplier X confirm the ship date? what was the original lead time? did we get the revised insurance cert?") only by searching their inbox. The ERP knows the PO exists. The inbox knows what's actually happening with it.


**The handoffs are the breakage points.** Buyer to engineering. Buyer to quality. Buyer to planner. Buyer to AP. Every workflow above has 2 to 5 handoffs, and almost all the time leakage happens at those seams. The single-discipline work isn't where time goes. The cross-team coordination is.


**Nobody's process matches the diagram.** Not one of these shops has a published workflow diagram that resembles what they actually do. They have old diagrams from a Lean consultant or an ISO audit. Nobody references them. The actual process lives in people's heads and Outlook folders.


**Automation happens at the edges, not the center.** None of these shops automated their procurement workflow wholesale. They automated specific seams: an Outlook rule, a Power Automate flow, an Excel template, a saved search. Big platform replacements mostly fail (we wrote about why ERP wasn't built for procurement). The targeted micro-automations stick.


## How to Map Your Own Workflow


If you're using this post to design or document your own procurement workflow, watch one of your buyers for a day. Real day, no special instructions. Sit next to them or screen-share. Count how many times they switch between Outlook, the ERP, Excel, and Slack. Count emails sent only because a system didn't auto-notify the right person. Count fields re-keyed from one place to another.


Then count the handoffs. To engineering, quality, finance, the planner. Each one is where the workflow either flows or stalls. Document the failure modes at each seam, not just the happy path.


Then ask: of everything on this list, what would be easier if email was the system instead of a side channel?


That's the question we keep coming back to. The procurement workflows we've watched all share a connective tissue, and it's email. Most software tries to replace that. The ones that work tend to live inside it.[Lumari](https://lumari.ai/) reads incoming supplier email, pulls out quotes, ship dates, and confirmations, and keeps your ERP and your buyers in sync without anyone re-keying or chasing. If the workflows above looked familiar, that's the thread we'd pull first.
