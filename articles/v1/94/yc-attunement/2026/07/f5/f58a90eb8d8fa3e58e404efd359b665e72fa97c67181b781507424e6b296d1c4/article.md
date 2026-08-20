---
schema_version: "1.0.0"
document_id: "f58a90eb8d8fa3e58e404efd359b665e72fa97c67181b781507424e6b296d1c4"
company_key: "yc-attunement"
company: "Attunement"
source_id: "yc-attunement-news-import-6e718227cfff"
canonical_url: "https://attunementclinical.com/blog/ai-can-get-things-wrong-confidently"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-08-09T19:27:56.570754+00:00"
fetched_at: "2026-08-09T19:27:58.301535+00:00"
content_hash: "sha256:e3ab0e6b5fb937cb1a6120696f968f03c32b09698252a0d4989db639a3bd6f45"
---

# AI can get things wrong, confidently.

The first time I watched a clinician review an AI-generated chronology of a patient’s chart, what stood out was how much time it took for her to check its work.


She would read a line, stop, and go find it in the chart. Then the next line. Nothing looked wrong, but there was no way to tell where the statements came from without checking all of them.


That is the part of AI in clinical work that is a real blocker. The risk isn't that a model produces something obviously wrong. It's that everything it produces reads *confidently fine,* which leaves the entire burden of sorting with the person who signs the report.


And that person is you. Whether the report is read by a colleague, questioned by a payer, challenged by a licensing board, or read aloud in a deposition, you are expected to stand behind every statement in it.


## What makes clinical records challenging for AI


A patient's chart rarely arrives as a clean document. It's thousands of pages of faxes, scans of scans, handwritten notes, duplicate intakes, and records that flatly contradict each other about the same event. The patient’s charts span decades, a dozen providers, no consistent format. Somebody has to turn that into a coherent history that holds up under scrutiny, and today that somebody is usually a clinician with a highlighter. It takes weeks.


So the appeal of handing it to AI is obvious. What's less obvious is what happens when you get it back.


## Confident output but…is it right?


Language models read medical records well, and they're getting better very fast.
A model can extract a real date of injury, a real medication, a real prior diagnosis and pull it from *the wrong document* in the file. Nothing about the resulting sentence looks different from the sentences around it.


But… one date to the wrong year and the reading of everything downstream shifts with it. Getting something confidently wrong and having to check its work is worse than doing it by hand.


And when someone eventually asks where does that come from? The clinician is back in the chart trying to reconstruct how the report got written in the first place.


## You can't review what you can't see


Most tools hand you a polished narrative and keep the construction hidden. A single report might contain hundreds of factual claims: dates, medications, diagnoses, providers, imaging findings, symptom history, and you have no realistic way to trace each one back.


So review turns into guesswork. The clinician checks what catches their eye, but did that really save them time? In my experience the line that deserves a second look is almost never the one that reads strangely. It's the one that reads like everything else.


## So…trust AI less?


We didn't build Attunement to convince clinicians to trust AI. That's the wrong ask to make of someone whose license is attached to the output.


Trust has to be unnecessary because of how the technology is designed.


We built it to cut the double-checking. Every statement stays connected to the page it came from, so verifying one takes seconds instead of a trip back through three thousand pages. The report doesn't ask for your faith. It shows its work.
My dream is to unload cognitive burden with a breakthrough technology. If you want to see how it works on your own records, you can[schedule a demo here](https://calendly.com/attunementai/attunement-demo?utm_source=attunementclinical&utm_medium=blog&utm_campaign=deid_launch&utm_content=ai_can_get_things_wrong_confidently) .


*Angelia Muller is the founder of Attunement, which turns medical records into source-traced histories with every fact linked to the page it came from. Attunement is based in San Francisco.*
