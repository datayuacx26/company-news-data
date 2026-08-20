---
schema_version: "1.0.0"
document_id: "904657a4714e69d9c33d5a7a00039fabcfd1f5c5d85191d2de03e7d6d26aecc0"
company_key: "yc-fresco"
company: "Fresco"
source_id: "yc-fresco-news-import-11bc7e0494a7"
canonical_url: "https://fresco.build/blog/extract-door-hardware-schedule-087100-spec-excel"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-25T05:48:27.827048+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:4529fc42e6bd57345c42c32dc5712db5a42167e2296f23a2999b6b11bbd2222a"
---

# How to Extract a Door Hardware Schedule from an 087100 Spec into Excel

Workflow


# How to Extract a Door Hardware Schedule from an 087100 Spec into Excel


July 17, 2026 · 12 min read


The bid is due Thursday. The 087100 spec carries twenty hardware sets, and every table you pull into Excel lands with the quantity and the product description crushed into one cell.


A Division 8 door and hardware supplier asked r/estimators for “a (near) fool proof method of extracting door hardware schedules from 087100 specs as tables into Excel.” The thread pulled 59 comments. Estimators traded OCR settings, Excel import paths, and AI experiments, and nobody landed on a method that actually held up. If you’ve ever tried to extract a door hardware schedule from a spec PDF and ended up rebuilding the table by hand, that thread reads like your own bid week.


Extraction keeps failing because a hardware schedule inside an 087100 spec isn’t really a table. It’s text formatted to look like a table. Most hardware schedules are written in Word, exported to PDF, and stripped of their structure on the way out. The rows and columns you see on the page don’t exist in the file, which is why every tool that tries to read them (OCR software, Excel, ChatGPT) keeps producing broken output.


## Why the 087100 Spec Breaks Every PDF Table Tool


The supplier who started that thread diagnosed the root cause himself: “The lack of visible lines at cell edges/columns & rows really hurts every software solution’s ability to properly figure out the table layout.” Hardware consultants author schedules in a word processor, and the PDF conversion throws away the cell boundaries. What’s left is positioned text that only looks tabular to a human eye.


Scale compounds the problem. A 20-set hardware schedule means twenty or more discrete tables in one PDF, and every one of them has to extract consistently for the output to be usable. Formats shift from one hardware consultant to the next, so a workflow tuned to one architect’s spec breaks on the next architect’s spec.


Asking the GC for the source file rarely helps. As the same estimator put it: “The GC often doesn’t have a non-PDF version anyways... Gotta work with what I got, ya know?” The PDF is the input. Any workflow that depends on cleaner input is solving the wrong problem.


The short answer to why extraction fails: the table structure was destroyed before your software ever opened the file. It’s a document problem first and a tooling problem second, and the PDF is usually the only version you’ll ever get.


## The Four Failure Modes to Expect


Across that thread and the wider Division 8 community, extraction goes wrong the same four ways on nearly every schedule:


- Merged columns. The quantity column and the product description (the Short Description, if you’re a Comsense user) collapse into one cell, so every row needs manual surgery before it can price.
- Broken door number lists. Comma-separated door numbers in a single cell get scattered across cells, or one opening’s row splits into fragments.
- Wrapped-text rows. Long product descriptions wrap in the source document, and the tool reads each wrapped line as a new row.
- Shattered multi-page tables. A schedule that runs twenty-plus pages comes back as disconnected pieces; Excel’s own PDF import produces one workbook per page with no consolidation.


Most extraction errors aren’t random. They’re the same four failures repeating: merged columns, broken door number lists, wrapped-text rows, and tables shattered across page breaks. If your output looks wrong, it’s almost always one of these patterns, which means you can check for them systematically instead of eyeballing the whole sheet.


## What Actually Works When You Extract a Door Hardware Schedule Today


Here’s the honest state of the tool options, drawn from what practicing estimators report rather than what any vendor claims:


**ABBYY FineReader —** the community’s “least worst” option: predictable, trainable, handles most layouts. It still merges columns on borderless tables, and per-schedule cleanup never fully goes away.


**Excel (Get Data > From File > From PDF) —** free and already installed, workable on short schedules. But it produces one workbook per page with no multi-page consolidation.


**Bluebeam —** strong for plan markups and takeoff notes, but unreliable on hardware spec tables specifically.


**ChatGPT —** fast for one small, clean table, but a documented failure loop on real schedules: it returns headers or the first rows, claims to fix it, then repeats the error.


**Gemini (paid) —** the best LLM result in one estimator’s structured comparison, but it dropped columns, and the output still needs full verification.


**Tabula —** free and open source, but effectively abandoned; it wouldn’t even load for the estimator who tried it.


The ChatGPT loop deserves its own warning, because most estimators try it first. In the original poster’s words: “It kept returning headers only, I’d explain that was wrong, it would tell me it understood the problem and its cause and fix it, then it would do the exact same thing.” A second estimator in the same thread hit the identical wall independently: he could only ever get the first thirteen or so rows back before concluding he was using the wrong tool for the job.


So the current least-bad workflow is unglamorous: extract with something predictable (ABBYY for long schedules, Excel’s PDF import for short ones), budget real cleanup time per schedule, and verify against the source before anything touches a price. No generic tool removes the verification step; the good ones just shrink the cleanup.


## What to Check Before You Trust an Extracted Table


The sharpest line in the thread wasn’t about any tool. It was about risk: “AI makes me nervous that they’re going to mutate or fill in some data point and I’m not going to catch it.” That’s the real bar for extraction. The dangerous extraction error isn’t the one that breaks your spreadsheet. It’s the one that passes a visual scan and lands in a material quote.


And the estimator carries that error alone. Same thread: “No company takes ownership of that kind of error except the idiot (me) who put a price out based off bad data. There’s always checking involved... checking it is basically inevitable by default.” Since checking is inevitable, the job is to make it fast and targeted. It’s the same discipline that protects the number once AI enters the takeoff itself; we covered that side in


[how Division 8 estimators review AI takeoffs and keep the sign-off](https://fresco.build/blog/ai-takeoff-review-sign-off) . Before an extracted schedule feeds a bid, verify four things:


- Row counts per set. Set 12 should have the same number of line items in Excel as it has in the spec. A dropped row is invisible once the table looks clean.
- Door totals against the door schedule. Total the doors assigned across hardware sets and reconcile against the schedule’s opening count, in both directions.
- The money items, line by line. Electrified hardware deserves a full read: there’s a “huge price swing between a Grade 1 cylindrical lock and an Exit Device with electric latch retraction,” as the thread put it, and the electrified item drags power transfers, raceways, power supplies, and activation hardware along with it.
- Spec-versus-schedule conflicts. A set referenced by the door schedule but missing from the 087100 spec (or the reverse) isn’t an extraction error. It’s a document error, and it belongs in an RFI early, while answers still change the bid.


And when the hardware spec is missing or incomplete entirely, extraction stops being the problem; price exposure is. Estimators on r/estimators report pricing more and more projects where the spec is “wrong, incomplete, or missing all together,” and GC-side rules of thumb in the few-hundred-dollars-per-opening range collide with commercial hardware that can run thousands per leaf. Document your assumptions on the bid, qualify what the spec never defined, and treat the allowance like any other moving number; we covered that side in


[keeping door hardware pricing current for Division 8 estimators](https://fresco.build/blog/door-hardware-pricing-current) .


What to check, in one line: row counts per set, door totals both directions, electrified hardware line by line, and spec-versus-schedule conflicts. Those four checks catch nearly everything the extraction step can break.


## Where the Hardware Schedule Workflow Goes From Here


Every tool in the rundown above shares one assumption: the schedule is a picture of a table, and the job is to read it. The newer approach in Division 8 doesn’t read the table at all: it reconstructs it. AI built specifically for construction documents knows what a hardware set is, what belongs inside one, and how the 087100 spec relates to the door schedule and the floor plans, so it rebuilds the schedule from that domain knowledge and then cross-checks it against the rest of the set the way an estimator would.


That’s the approach we build at Fresco: hardware sets extracted from the spec, checked against the door schedule, conflicts flagged for the estimator to resolve (a door assigned to one set in the schedule and a different set in the spec gets surfaced, not silently “fixed”) and the output lands as clean Excel or Comsense-ready import files. The posture matters as much as the horsepower. Estimators told us the bar plainly: a system like this “would have to be insanely good” before they’d trust it with takeoffs. So the output is structured for review. It’s a better starting point to check against, never a black box asking to be believed.


Extraction is also only half the job. Once the sets are clean, they still have to get into your estimating system, and that entry step is its own documented time drain. We covered it in


[how Division 8 estimators are getting hardware sets into Comsense faster](https://fresco.build/blog/hardware-sets-into-comsense-faster) . The two pieces bracket the full workflow: schedule out of the PDF, sets into the pricing system. On the large institutional work (the healthcare set with a 100-plus page hardware schedule) the two steps together are the difference between bidding the job and passing on it.


Where this lands: generic tools read pixels and guess; Division 8-specific tools reconstruct the schedule from domain knowledge and cross-check it against the full document set. That shift turns extraction from a rebuild into a review.


## When This Advice Doesn’t Apply


- A four-door schedule the GC emails over doesn’t need a workflow. Read it, type it, move on; any tool costs more time than it saves at that size.
- If your manufacturer or distributor partner already supplies hardware data digitally, you’re consolidating data, not extracting it, and the checks above matter more than the tooling.
- If you can actually get the native Word file from the hardware consultant, table conversion is a copy-paste. It’s rare, but it’s worth one ask on a big job.


The threshold is simple: extraction workflows start earning their keep around the multi-set commercial schedule, and they stop being optional on the 100-page institutional one.


## Frequently Asked Questions


### Why do the quantity and description columns merge when I copy a hardware schedule from a PDF into Excel?


Because the schedule was authored in Word and converted to PDF, the cell boundaries don’t exist in the file. Extraction tools guess the layout from character spacing, and two adjacent columns with no visible border between them read as one. It’s the single most reported failure mode for 087100 extraction, and it’s a property of the document, not your settings.


### Can ChatGPT or Gemini extract a door hardware schedule from an 087100 spec?


Not reliably. Estimators independently report the same ChatGPT loop: it returns the headers or the first rows, acknowledges the problem, then repeats it. In one estimator’s structured comparison, paid Gemini came closest (“exactly what I needed” apart from dropped columns), but dropped columns in a hardware schedule are exactly the silent failure that ends up in a material quote. Anything from a general-purpose LLM needs line-by-line verification before it prices.


### What’s the most reliable way to extract a door hardware schedule without specialized software?


ABBYY FineReader remains the community’s “least worst” pick for long schedules: its failures are at least predictable. For a short schedule, Excel’s built-in Get Data > From File > From PDF path is workable if you’re willing to consolidate one workbook per page. Either way, budget cleanup time and run the four verification checks before the data touches your bid.


### How should I price a Division 8 bid when the hardware spec is missing or incomplete?


Treat it as a scope-exposure problem, not an extraction problem. Qualify the bid explicitly around what the spec never defined, document the assumptions behind your hardware allowance, and get the RFI in early. The gap between a placeholder allowance and real commercial hardware (a few hundred dollars per opening versus thousands per leaf on institutional work) is too wide to absorb silently.


### How do Division 8-specific AI tools handle extraction differently from PDF converters?


A PDF converter tries to read the table’s visual layout. A Division 8-specific tool like Fresco reconstructs the schedule from domain knowledge (it knows what a hardware set is and how the spec relates to the door schedule and plans) and then cross-checks the result across documents and flags conflicts for the estimator to resolve. The output arrives as clean Excel or a Comsense-ready import file, structured for review rather than blind trust.


## Key Takeaways


- A hardware schedule in an 087100 spec is text formatted to look like a table. Extraction fails because the structure was destroyed in the Word-to-PDF conversion, not because you picked the wrong OCR settings.
- Expect four failure modes: merged columns, broken door number lists, wrapped-text rows, and multi-page tables that won’t consolidate.
- To extract a door hardware schedule with generic tools today, ABBYY FineReader is still the least-worst option; ChatGPT’s headers-only loop is documented by multiple estimators independently.
- Checking is inevitable by default. Verify row counts per set, door totals in both directions, electrified hardware line by line, and spec-versus-schedule conflicts before anything prices.
- Division 8-specific AI reconstructs the schedule from domain knowledge and cross-checks it against the full document set, turning extraction from a rebuild into a review.


If the 087100 spec is where your bid week goes to die, run one of your own project sets through Fresco and see what the extraction and cross-checks look like on your documents. Book a demo at fresco.build.


See what Fresco can do on your next project.


[Get a free takeoff](https://fresco.build/)
