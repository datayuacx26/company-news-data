---
schema_version: "1.0.0"
document_id: "61d3cb977b14ac6de691904aff4b3e25bedb56dc9f49e5146aa2c11e3754f34a"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/paying-down-1-500-definitions-of-taxonomy-debt-in-a-two-day-project"
published_at: null
first_seen_at: "2026-07-30T15:27:01.907546+00:00"
fetched_at: "2026-07-30T15:27:03.483070+00:00"
content_hash: "sha256:6b594865424a7e6f735a4a6460eb7de5a32a3a0eefa55d6884f9cfd6c8a537a5"
---

# Paying down 1,500 definitions of taxonomy debt in a two-day project

Summer is the one time of year when the roadmap loosens up a bit. Fewer launches, fewer urgent asks, more room for the kind of deep work that never wins a prioritization meeting. This July, I used that room to pay down a debt our data team had been stepping around for years: the missing definitions in our Amplitude taxonomy.


The numbers made it clear why nobody had done it by hand. Our taxonomy holds about 6,400 items: roughly 1,200 events, close to 3,800 event properties, around 1,500 user properties, and a handful of group properties. More than 5,000 of them had a definition that was missing, a stub that restated the name, or a description too weak to trust. We do have a clean process for new events now (a Claude skill enforces our naming convention at creation time; see[this article](https://www.photoroom.com/inside-photoroom/amplitude-event-governance-slack-bot-and-ai) , even though the skill has evolved a lot since then!) but that only stops the debt from growing. The historical pile was still there, and everyone at Photoroom who opens Amplitude reads from it.


So instead of documenting thousands of items myself, I set up a division of labor: Claude Code did the digging, through dozens of small sub-agents patrolling our codebases, and I stayed in the role of the gate. Nothing was written to Amplitude without a human reading it first. Less than 24 hours later, 1,330 definitions were live, 102 properties were newly governed, and 32 pieces of junk were deleted.


This post is not a tutorial. It's the handful of practices that made the project work, in the hope that they're useful to other data teams sitting on a similar pile.


## **Give the agent all the code**


The single prerequisite for everything else: the agent needs to see every repository where tracking calls live. For us that meant cloning the iOS, Android, web app, and backend repositories side by side, so a sub-agent asked "where does this event fire?" could actually go look.


*One parent folder, every repo cloned next to each other: wherever an event actually fires, the agent can go look.*


This matters because the truth about an event is scattered. The generated Ampli catalogs (` Ampli.swift` ,` Ampli.kt` ,` index.ts` ) turned out to be the real schema: exact property names, enum value sets, sometimes doc comments. Backend events traced back to Python payload keys. Our richest single source was one line of backend code that dumps entire raw Stripe objects into the analytics payload, which let 300+` stripe_data.*` properties map cleanly onto documented Stripe fields.


One caveat we kept repeating to ourselves: code tells you *where* an event fires, not always what it *means* . That gap is exactly where the human review comes in, and it shaped the whole design.


## **Before executing anything: "challenge me"**


The first prompt of the project was not "do this". It was "before doing anything, **challenge me.** " What followed was a real back-and-forth between Claude and me, objections on its side, decisions on mine, and that conversation surfaced, before a single token was spent on execution, the constraints that ended up defining the project:


-


A wrong definition is worse than a blank one. At least blank is honest. Wrong looks authoritative, and misleads every future reader.


-


Many ingested names don't exist as string literals in the code (enums, interpolation, generated catalogs), so naive grepping would report false "not found"s.


-


"Missing" was underspecified. We had to decide on an explicit quality bar (we settled on a five-level rubric: missing, stub, weak, ok, good).


-


The biggest risk was never running out of context. It was writing bad text into a shared production taxonomy the whole company reads. Conclusion: the agent proposes, only a human applies.


We also got a **cheap second opinion** : before running the plan, I opened a fresh session with a new model and asked it to critique the plan too. Its most useful move was that it started by reading the environment instead of executing. It noticed an exploratory attempt at this problem from a few weeks back (when I had first started looking into filling all the definitions automatically but didn't yet have access to all the repositories), and correctly recommended treating it as context rather than a base to build on. It also upgraded several plan steps from "invent an approach" to "reuse a pattern the team already has", like our existing Amplitude branch-creation scripts. An agent that reads the room before executing catches that the ground has shifted before spending anything on the wrong plan.


## **Put the plan in a Markdown file, then execute it with sub-agents**


The pattern at the heart of the project: separate planning from execution, and make a Markdown file the contract between the two. It's a pattern we trust enough to use elsewhere; our[Insights Factory](https://www.photoroom.com/inside-photoroom/the-insights-factory-how-we-run-deep-data-investigations-with-llm-agents?_storyblok=198326574241393&_storyblok_c=PageInternalBlogPost&_storyblok_version=&_storyblok_lang=default&_storyblok_release=0&_storyblok_rl=1784109174229&_storyblok_tk%5Bspace_id%5D=191576&_storyblok_tk%5Btimestamp%5D=1784109174&_storyblok_tk%5Btoken%5D=3d5dd6349cb62eb5570cf646ca71b31d1d263642) runs on the same idea.


Concretely, the project ran in five phases:


1.


fetch the full taxonomy to disk


2.


score and classify every item


3.


write the plan


4.


execute it


5.


after human review, apply the results to Amplitude.


The contract sits in the middle. The first three phases produced an inventory and an` execution-plan.md` , then stopped: nothing ran until I had read and amended the plan. Execution, launched as a separate prompt, produced proposals from the approved plan and nothing else. And applying anything to Amplitude waited for a second human gate, the review described two sections down.


Execution itself never relied on one big context window. The 2,500 in-scope items were sliced by a small deterministic script into 41 batch files, and each batch went to a fresh sub-agent that read exactly two things: one shared briefing file with the rules, and its own input slice. We ran them in seven waves. After every wave, mechanical checks verified the output (schema respected, row counts reconciled, every "high certainty" row backed by a real` file:line` citation), and a progress bar in the plan file got bumped and committed.


*The contract itself: phases with checkmarks, a progress bar, and a status line reminding everyone that nothing writes to Amplitude without approval.* *


Two properties of this setup did a lot of quiet work.


1.


The shared briefing file kept 41 agents consistent without restating the rules 41 times. And because the batches lived on disk with progress tracked in the plan, the run was resumable at any point.


2.


No single agent ever held more than a sliver of the problem, and the plan file always knew where we were.


## **Classify before you dig**


The most valuable thing the agent did was not writing definitions. It was proving that half of them didn't need to be written.


Before any code archaeology, we had the agent classify all 5,015 items needing work into families based on their names: real business properties, backend payload keys, Amplitude system fields, marketing parameters, third-party SDK events, and so on. One family dominated: 2,515 properties were auto-generated Amplitude Experiment exposure flags, all following the same` \[Experiment\] <key>.variant` pattern.


The agent's instinct was to template them, since one description keyed on the flag name would cover all 2,515. My call as the human was different: they're auto-generated noise and don't belong in a curated taxonomy at all. Don't document them.


That decision took about thirty seconds and erased half the workload. And it's the point I'd most want another data team to take away: **the agent's job is to make expensive decisions legible and cheap to make.** On my own, I would have faced 5,015 undifferentiated rows. What I actually faced was seven named families and a handful of accept/reject calls. The human still owns "what belongs in the taxonomy"; the agent owns "here's what's in it and what each option costs."


## **Meet the reviewer in their medium**


Everything the agent proposed passed through human review, so the ergonomics of that review decided whether the project was pleasant or miserable. The obvious format was CSV, since the pipeline ran on CSVs anyway. Reviewing spreadsheet cells row by row is a chore, though, and a chore is exactly how a review gate dies.


We kept the CSVs as the machine source of truth but generated Markdown review files on top, sorted by stakes (overwrites of existing text first, then new high-certainty proposals, then medium). Checkboxes were the decision: tick the box, the proposal ships. I read the rendered pages with[mdopen](https://github.com/dahbiahmed/mdopen) , a small tool that opens a Markdown file as a clean page and lets you comment inline, roughly like a Notion page.[Ahmed](https://www.linkedin.com/in/dahbiahmed/) , our former VP Product, happened to share his package the very day of the cleanup, providential timing, and it made reviewing hundreds of proposals genuinely fast.


*The review surface, rendered with mdopen: one checkbox per decision, under each definition the code citation that grounds it, and inline comments on the side. I highlight, comment, and copy the notes straight back to Claude.*


The human review caught things I found more interesting than bugs:


-


**It caught over-explaining.**
The first two things I deleted were both things the agent had added to be helpful: a "generated by Claude" provenance tag on every description, and an enumeration of each event's properties ("…, recording A, B and C"). Both read as clutter on the actual review surface, and the property list is worse than clutter: attachments change over time and the prose silently rots. Provenance belongs in git, not stapled to every string.


-


**It caught what the code cannot say.**
My favorite example: the agent faithfully described our subscription tiers as "Pro, Business, Free, Ultra", because the code emits a tier literally called` business` . But that plan is called **Max** in the product. The team named it` business` in code deliberately, because` max` is a reserved function name in nearly every language, and renames it in Amplitude to match the product. The correct display name exists nowhere in any repository. Only in people's heads.


-


**It directed effort.**
On two refund events, I asked "is this Stripe-specific? If so, say so." That one question sent the agent a hop deeper into the backend, where it confirmed the refund path only exists for Stripe, and the definitions came back both more precise and better grounded. Verifying everything to that depth would have been wasteful across 2,500 items. The reviewer's eye is the targeting signal for where deeper digging pays off.


## **What the agent refused to guess**


Of the 2,500 in-scope items, the agent proposed grounded definitions for 1,621 (65%), each tagged high or medium certainty with its evidence. For the other 879 it wrote` no proposal` and a reason.


35% may sound disappointing, but is a number I'm comfortable defending. Those items are ingested into Amplitude but have no emitter anywhere in our repositories: leftovers from concluded experiments, telemetry from native vendor integrations, ad-campaign strings mis-ingested as property names, even a few literal spam keys. An agent optimizing for coverage would have produced 879 plausible-but-wrong definitions, and each would have sat in the taxonomy looking exactly as authoritative as the real ones. When you're guessing, blank wins.


I'll spare you the full apply-phase war stories, but honesty requires saying the writes were not as clean as the plan. The taxonomy we **read** (everything Amplitude has ever observed) and the tracking plan we **write to** (only what's formally governed) are different surfaces, and 393 names initially fell through the gap between them. The casing duplicates the audit had flagged as cosmetic debt (` Invite Id` vs` Invite ID` ) came back to break actual API payloads by colliding into a single JSON key. We logged a dozen or so traps like these in an internal playbook. If you attempt something similar, budget real time for the apply phase; that's where the platform's edges live.


## **What we kept**


At the end, we asked what actually deserved to be versioned, and landed on an ordering we now apply more broadly: keep the **why** (rationale, decisions), keep the **how** as prose (patterns, API traps, with the scripts stamped as dated reference code), and don't version the **what** (the definitions themselves, which Amplitude owns). The project folder shrank from about 10 MB to 250 KB: one playbook a future run reads first, plus a terse log. The durable asset from an automation project is not the automation. It's the distilled knowledge of how the system actually behaves.


The final tally: roughly 6,400 items audited, 1,330 definitions now live in Amplitude, 102 properties newly governed, 32 junk items deleted, and 879 honest blanks routed to human owners. Elapsed time, less than two days in total, most of which was review and decisions rather than waiting on agents.


Nothing in this setup is specific to us. If your team has a taxonomy (or a dbt project, or a metrics catalog) with the same slow-rotting documentation debt, the recipe is portable: give the agent all the code, ask it to challenge you before it executes, put the plan in a file and farm the work out to small sub-agents, classify before you dig, and keep a human gate whose review surface you've actually made pleasant. The quiet weeks of summer turn out to be exactly the right time.
