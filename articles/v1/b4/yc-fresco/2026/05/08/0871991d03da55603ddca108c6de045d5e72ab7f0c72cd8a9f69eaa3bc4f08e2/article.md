---
schema_version: "1.0.0"
document_id: "0871991d03da55603ddca108c6de045d5e72ab7f0c72cd8a9f69eaa3bc4f08e2"
company_key: "yc-fresco"
company: "Fresco"
source_id: "yc-fresco-news-import-11bc7e0494a7"
canonical_url: "https://fresco.build/blog/fake-door-schedule-problem"
published_at: "2026-05-14T00:00:00+00:00"
first_seen_at: "2026-07-25T05:48:27.827048+00:00"
fetched_at: "2026-07-28T21:55:55.482044+00:00"
content_hash: "sha256:55c53e4824fc5c1ecf9130a8a9681878d7ba1fe1a2db39a279e80d43615ca7f6"
---

# The Fake Door Schedule Problem: Why Division 8 Takeoffs Are Harder Than They Should Be

A door schedule is supposed to be the single source of truth on a Division 8 project. Every door in the building, listed once, with every attribute accounted for: size, material, fire rating, hardware set, frame type. In theory, you read the schedule, cross-reference the floor plans, match the hardware spec, and you’ve got your takeoff.


In practice, the door schedule lies.


Not intentionally. Architects are managing thousands of details across hundreds of drawings, and the door schedule’s one of the last documents to get reconciled. The result is a document that references doors that don’t appear on the floor plans, omits doors that do, assigns hardware sets that contradict the hardware specification, and lists door counts that disagree with the elevations. For a Division 8 estimator, the takeoff isn’t a matter of reading the schedule. It’s a matter of figuring out which parts of the schedule are real.


## What Actually Goes Wrong


The inconsistencies between a door schedule and the rest of a bid package aren’t random. They follow predictable patterns that every experienced Division 8 estimator recognizes.


**Doors appear on the floor plans but not in the door schedule.** The architect drew the opening, assigned it a room, but never added it to the schedule. If the estimator trusts the schedule alone, those doors are missing from the bid.


**Doors appear in the schedule but not on the plans.** The architect created a line item — maybe during an earlier design phase — but the room was later redesigned or removed. The schedule still references a door that no longer exists in the building.


**Door counts disagree.** A door code appears four times in the schedule and six times on the floor plans. Or the schedule lists it once, but it appears across three separate enlarged plans, and the estimator has to determine whether those are the same opening drawn at different scales or three distinct openings.


**Hardware sets contradict each other.** The door schedule assigns a door to hardware set 107.3. The hardware specification assigns the same door to 107.4. A door doesn’t get two hardware sets, but the documents say otherwise.


These aren’t edge cases. On more than 80% of commercial projects, the architectural documents contain inconsistencies that require an estimator’s judgment to resolve. That statistic comes from analyzing thousands of takeoffs across the United States and Canada. The problem isn’t that architects are careless. The problem is that coordinating a set of construction documents across dozens of consultants, hundreds of pages, and multiple revision cycles is genuinely difficult, and the door schedule is where those coordination failures surface.


## Why This Makes Division 8 the Hardest Takeoff


Every trade in construction does takeoffs. Concrete counts cubic yards. Electrical counts outlets and runs conduit. Plumbing counts fixtures. These are complicated, but the unit of measurement is relatively straightforward.


Division 8 is different. A single door isn’t a single item. It’s a door leaf, a frame, a set of hinges, a lockset, a closer, a seal, a kick plate, and potentially a dozen other components — each of which must be sized, specified, and priced according to the characteristics of that specific opening. A 3070 hollow metal door with a cylindrical prep in a 90-minute rated frame with a specific closer arm length is a fundamentally different line item than a 3070 hollow metal door in a non-rated frame with a different prep. Multiply that specificity by 300 or 400 openings on a mid-size commercial project, and the combinatorial complexity is enormous.


Now layer in the document inconsistency problem. The estimator isn’t just counting doors. The estimator’s simultaneously cross-referencing four or five documents — the door schedule, the floor plans, the hardware specification, the architectural specification, and the door and frame elevations — looking for places where those documents disagree. The takeoff is a reconciliation exercise as much as it’s a counting exercise.


This is why a 400-opening hospital project can take an experienced estimator a full week. Not because counting is slow, but because finding and resolving discrepancies is slow.


## What Most Estimators Actually Do About It


Every Division 8 estimator has a version of the same workflow. Open the door schedule. Open the floor plans. Start marking off doors in Bluebeam or on paper. When a door appears on the plans but not the schedule, flag it. When a door appears on the schedule but not the plans, flag it. When the hardware set in the schedule doesn’t match the hardware set in the spec, flag it. Build a list of discrepancies. Send RFIs to the architect. Wait for responses. Adjust the takeoff. Send the bid.


The process works. It’s worked for decades. But it’s almost entirely manual, and the time it takes is directly proportional to the number of discrepancies in the documents — which the estimator can’t predict until they’re already deep into the takeoff. A clean set of drawings might take two hours. A messy set with the same door count might take two days. The variable isn’t the project size. The variable’s the drawing quality.


General-purpose takeoff software has attempted to speed this up. Tools like Togal and Stack offer AI-assisted quantity counting across multiple trades. But Division 8 estimators who’ve used these tools consistently report the same experience: the software counts doors, but it doesn’t understand doors. It double-counts at doorways. It measures behind cabinets that are out of scope. It requires manual tagging of every door type. One estimator who used a general-purpose platform for over a year described it as “basically a PDF reader.” PDF readers are helpful. They’re not doing the estimator’s job.


## The Architectural Error Flip


The question Division 8 estimators ask most often when they first see an AI takeoff tool is: what happens when the drawings are wrong?


It’s a reasonable question. If the architect’s door schedule contains errors — and on 80% of projects, it does — then any AI system that reads the schedule and produces a takeoff will inherit those errors. A bad schedule in, a bad takeoff out.


This is where the assumption breaks. The thing estimators expect to be the platform’s failure is exactly what a Division 8-specific AI platform is designed to address.


Fresco, a Division 8 AI takeoff tool built by a team that’s processed thousands of commercial takeoffs, doesn’t just read the door schedule and produce a finished count. It reads the door schedule, the floor plans, the hardware specification, and the elevations simultaneously — the same way a human estimator does. Where those documents agree, the takeoff proceeds automatically. Where they disagree, Fresco flags the discrepancy and presents it to the estimator for resolution.


Instead of reviewing 400 doors on a project, the estimator reviews the 5 or 10 where there’s a mismatch. The 90% of the takeoff that involves no contradictions — the rote, mechanical work of pulling information off plans and specs — is handled automatically. The 10% that requires judgment stays with the human.


The result isn’t just a faster version of the old process. It’s a structurally different process. A 400-opening project that would take a week manually takes less than an hour. Not because the AI is faster at reading. Because the AI eliminates the reconciliation work that consumes 90% of the estimator’s time.


## What This Means for Division 8 Estimators in 2026


The Division 8 takeoff has been a manual reconciliation exercise for as long as door schedules have existed. The tools have changed — from paper to Bluebeam to Excel — but the workflow hasn’t. The estimator still cross-references documents by hand, flags discrepancies by hand, and resolves them by hand.


That workflow is changing. AI systems trained specifically on Division 8 construction drawings — not general-purpose models, but models built on millions of door, frame, and hardware drawings — can now perform the cross-referencing and flagging that accounts for most of the takeoff time. The estimator’s judgment is still the final authority. But the estimator’s time is no longer consumed by the mechanical work of finding problems. It’s focused on solving them.


The fake door schedule problem isn’t going away. Architects will continue to produce documents with inconsistencies, because coordinating a complex set of construction drawings is genuinely hard. What’s changing is how Division 8 estimators deal with it. The estimators who are getting through takeoffs in a fraction of the time aren’t working faster. They’re working on a different layer of the problem.


## Frequently Asked Questions


### How long does a Division 8 takeoff typically take on a mid-size commercial project?


It depends almost entirely on drawing quality, not project size. A 400-opening hospital project with clean, well-coordinated documents might take an experienced estimator two to three days. The same project with a messy set — doors missing from the schedule, hardware set conflicts between the spec and the door schedule, count discrepancies between the plans and the elevations — can stretch to a full week. The unpredictability is one of the most frustrating parts of Division 8 estimating: you don’t know how long the takeoff will take until you’re already in it.


### Why can’t general-purpose takeoff software handle Division 8?


General-purpose takeoff tools like Togal and Stack are built for quantity counting across multiple trades — counting outlets, fixtures, linear feet of conduit. Division 8 isn’t a quantity counting problem. A single door opening involves a door leaf, a frame, hinges, a lockset, a closer, seals, and potentially a dozen other hardware components, each specified differently depending on fire rating, prep type, and frame configuration. These tools count doors, but they don’t understand the relationships between a door schedule, a hardware specification, and a floor plan. That’s where the actual estimating work happens.


### What happens when an AI takeoff tool encounters errors in the architectural drawings?


This is the most common concern Division 8 estimators raise, and it’s the right question. A Division 8-specific AI platform like Fresco doesn’t just read the door schedule and produce a finished takeoff. It cross-references the door schedule, floor plans, hardware specification, and elevations simultaneously. Where the documents agree, the takeoff proceeds automatically. Where they disagree, the platform flags the discrepancy for the estimator to resolve. The AI catches the conflicts; the estimator applies the judgment.


### Do I still need to send RFIs if I’m using an AI takeoff tool?


Yes. Document discrepancies still need to be resolved with the architect, and RFIs are still the mechanism for that. What changes is how quickly you identify which RFIs need to be sent. Instead of manually cross-referencing every door across four or five documents to find mismatches, the platform surfaces the conflicts for you. Your RFI list gets built in minutes instead of hours.


### Is AI going to replace Division 8 estimators?


No. The mechanical work of cross-referencing documents and flagging discrepancies is being automated. The judgment work — deciding how to resolve a conflict between the door schedule and the hardware spec, knowing when to call the architect versus when to make an assumption, pricing a non-standard opening that doesn’t match any template — stays with the estimator. The role shifts from spending most of your time finding problems to spending most of your time solving them.


Fresco is a Division 8 AI takeoff tool used by more than 70 companies across the United States and Canada. See it in action at fresco.build.
