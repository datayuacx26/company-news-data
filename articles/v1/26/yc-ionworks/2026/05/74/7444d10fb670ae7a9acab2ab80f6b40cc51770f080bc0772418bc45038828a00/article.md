---
schema_version: "1.0.0"
document_id: "7444d10fb670ae7a9acab2ab80f6b40cc51770f080bc0772418bc45038828a00"
company_key: "yc-ionworks"
company: "Ionworks"
source_id: "yc-ionworks-news-import-f340ab61a31b"
canonical_url: "https://ionworks.com/blog/generate-cycler-protocols-from-natural-language"
published_at: "2026-05-11T00:00:00+00:00"
first_seen_at: "2026-07-22T00:33:30.651124+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:17c977fef36814499f390b9ae9770f814ac5ae4f1b4517154d264db1690d4272"
---

# Generate cycler protocols from natural language

A characterization protocol is a program. It runs on expensive hardware against expensive cells, it loops through hundreds of steps, and it writes out data that[downstream models](https://ionworks.com/solutions/train) depend on. And yet most teams still author it by hand, in MITS Pro or the Maccor builder, step by step, line by line.


That is the part of a test campaign that has aged the worst. The DOE, the cell selection, the temperature matrix all get scrutiny. The schedule file that implements them gets typed up in an afternoon and trusted because "it built." Errors only surface weeks later, when someone notices the data does not say what the test was meant to ask. We have written before about why[the protocol is the most under-tested part of a test campaign](https://ionworks.com/blog/simulate-your-test-protocol-before-you-run-it) ; this is the other half of the same story.


The Ionworks protocol generator closes that gap. You describe the test in plain language. You get a validated cycler schedule, in your vendor's native format,[simulated against a physics-based model](https://ionworks.com/solutions/battery-protocol-simulator) before any cell is loaded.


## What you type, what you get


The input is the kind of description an engineer would scribble in a notebook before sitting down to write the schedule:


> "Run an HPPC characterization at 25 °C. 1C discharge and charge pulses, 10 s each, 10 min rest between pulses. 2C pulses, same pattern. Then a 1C discharge for 3 min to step SOC down ~5%. Repeat 20 times. Export for Arbin."


About thirty seconds to write.


The output is a complete` .sdu` schedule, eleven step blocks resolved into the full 200-step program with repeats, voltage limits, end conditions, and metadata. It opens in MITS Pro, runs on the channel, and produces data with the structure the description implied. The same prompt exports cleanly to Maccor, Neware, BioLogic, or Novonix. The protocol is the asset; the cycler vendor is a setting.


## Where the time goes


Hand-authoring a multi-step characterization is not difficult work. It is slow, repetitive work. Most of an engineer's time on a new protocol is spent on three things:


The first is mechanical translation. Pulling a description from a paper or a previous campaign and turning it into the specific syntax your cycler expects: control type, step labels, end conditions, loop counters, formula-driven limits. None of this is engineering judgement. It is data entry against a manual.


The second is fact-checking. Every line gets read twice. Did the C-rate convert correctly to amps for this cell? Is the rest long enough for the relaxation you care about? Did the loop block close where you meant it to close? Errors here are not exotic. They are the ones that pass syntax validation and produce the wrong data anyway.


The third is the rebuild when the lab picks a different cycler. A protocol perfected in MITS does not survive intact when the same campaign moves to a Neware bench. The translation is manual every time.


Generating from language collapses all three. The mechanical translation is automatic. The fact-checking moves earlier in the workflow, where it is cheap. The vendor format is a final step, not a rewrite.


## The benefits, concretely


**Hours become seconds per protocol authored.** A characterization that took half a day of careful typing and proofreading takes the time it takes to write a paragraph. The compounding effect is larger than it sounds: most labs run dozens of protocol variants per quarter, and the time freed is the time you wanted to spend on analysis.


**Errors get caught before a cell is loaded.** The generator's output is checked against a fast cell model. A wrong cutoff appears as a wrong cutoff. A missing end condition appears as a flat line. A drive cycle that quietly hits Vmax because the previous step left the cell at the wrong SOC fails[in simulation](https://ionworks.com/solutions/predict) , not on channel. The cost of a protocol mistake stops being weeks of cycler time.


**One source, any cycler.** The same description exports to Arbin, Maccor, Neware, BioLogic, or Novonix with full round-trip fidelity. A protocol that lives in one place does not drift between vendors. A team that adds a new bench does not rewrite its library.


**Cycler software upgrades stop breaking your library.** The same property holds across cycler control software revisions. A MITS Pro 8 to MITS Pro 10 upgrade, a Maccor controller swap, a Neware BTS update: the UCP description is the source of truth, and only the translation layer changes. Re-export the library against the new version, run the simulation check, and the protocols that worked before still work, in syntax the new software accepts. The migration project that used to take a week of manual rewrites stops being a project.


**The non-experts can author.** A modeler who needs a parameterization run, a project manager who wants a sanity-check formation, a new hire who has never opened MITS — all of them can describe the test in language and get something the lab can run. The cycler interface stops being the bottleneck on who can move work forward.


**The expert reviews instead of writes.** Senior cell engineers stop spending their afternoons translating ideas into schedule syntax. They review the generated protocol, adjust what needs adjusting, and move on. The skill that matters — judging whether the test will answer the question — gets applied to the question, not the translation.


## Why this is not just an LLM with a cycler manual attached


The hard part of generating a cycler schedule is not language understanding. It is correctness. A schedule that "looks right" but loops the wrong way, or off-by-ones a counter, or sets a current limit that violates your cell's safe operating envelope, is worse than no schedule at all — it produces data that no one knows is wrong.


The generator is built around a small set of design choices that exist to make the output trustworthy.


A description is first translated into UCP, the Ionworks Universal Cycling Protocol — a structured intermediate representation that captures the test as data, not vendor syntax. UCP is where validation happens: step ordering, voltage and current limits against the cell specification, end-condition completeness, loop integrity. The model that generates UCP also checks UCP, and corrects what it can before any export runs.


The export from UCP to a vendor schedule (` .sdu` for Arbin,` .000` for Maccor, the corresponding format for Neware, BioLogic, or Novonix) is deterministic. Not AI. A given UCP produces the same` .sdu` every time, and that mapping is testable, version-controlled, and reviewable. The only place language models are in the loop is the front end, where they are good — turning intent into structure. The conversion to executable code is plain software.


Finally, every generated schedule is simulated against a[parameterized cell model](https://ionworks.com/solutions/train) before it is handed to the lab. Predicted voltage, current, and capacity traces are produced for the full program. If a step pushes the cell into a place it should not go, the trace shows it. The check is not "did the model agree with itself" — it is "would this protocol, run on this cell, do what was asked." That is the question the lab actually has.


## How it works


Four stages, each one carrying a different burden. The language model handles intent. UCP handles structure. The exporter handles vendor compatibility. The simulator handles physics. None of them is asked to do a job another tool does better, and the validation loop happens twice — once inside UCP generation, once after simulation.


The pink stage is the only place a language model touches the output. The green stages — UCP validation and the export to a vendor schedule — are deterministic, version-controlled software with tests of their own. The blue stages run[physics-based simulation](https://ionworks.com/models/types/electrochemical) against a[parameterized cell model](https://ionworks.com/blog/battery-parameter-estimation) . The two grey stages are where the engineer is in the loop: writing the description and reviewing the simulated trace before approving the schedule for the lab.


The validator can return the draft to the LLM for correction; the simulator can return the whole protocol to the engineer if a trace surfaces an issue the validator could not catch on structure alone. Neither loop is silent — every revision is logged against the description it came from.


## What this changes


The protocol stops being the most fragile part of a test campaign and starts being its most reproducible artifact. A description in a project doc generates the same schedule a year later. A new cycler in the lab does not require a translation project. A junior engineer can author a formation protocol without a senior engineer reading every line.


The longer-term shift is that the schedule file stops being the source of truth. The description is the source of truth, the UCP is its validated form, and the vendor file is a build artifact. That is how every other engineering discipline treats its programs, and it closes one of the[handoffs that traditionally break a battery simulation workflow](https://ionworks.com/blog/why-batteries-break-traditional-simulation-workflows) . Battery test protocols are catching up.


## Try it


Protocol generation from natural language is available in Ionworks today, alongside the[protocol simulator](https://ionworks.com/solutions/battery-protocol-simulator) that validates whatever schedule — generated or hand-written — your team is about to put on channel. If you want to see it on a protocol you already run,[book a working session](https://ionworks.com/demo) . Bring a description; we will run it end to end and show you what comes out the other side.
