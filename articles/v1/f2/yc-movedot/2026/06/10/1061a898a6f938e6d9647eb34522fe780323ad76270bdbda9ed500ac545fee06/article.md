---
schema_version: "1.0.0"
document_id: "1061a898a6f938e6d9647eb34522fe780323ad76270bdbda9ed500ac545fee06"
company_key: "yc-movedot"
company: "MOVEdot"
source_id: "yc-movedot-news-import-b09513d212fd"
canonical_url: "https://movedot.ai/blog/post-test-road-load-data-analysis"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-25T16:12:42.390551+00:00"
fetched_at: "2026-07-28T21:46:31.447870+00:00"
content_hash: "sha256:b4dfff006c7c2d577a7c4770dd9b51053c7a73f4de721ef8ed584aa51b870f42"
---

# How an autonomous agent saved a multi-week durability campaign from being wasted | MOVE. Blog

A real durability workflow: one prototype SUV, four terrain blocks, 93 channels. Data came in Monday evening, and the full post-test analysis (QC, damage, exceedance, verdict) was ready Tuesday morning.


Post-test analysis


90%


less engineer time


From


2 days


to


Overnight


Scrapped rig campaign


$250K+


saved per catch


From this project. Time: the full post-test analysis (QC, damage, exceedance, verdict) is about two days of focused engineer effort — usually stretched across a week — and ran overnight in 8 minutes of compute. Cost: catching the under-scoped rig load schedule before the rig was booked avoids repeating an invalid accelerated campaign — rig time, re-instrumentation, and often a fresh prototype — which runs well into six figures.


On a durability program, the bottleneck is rarely the test. The vehicle runs, the sensors record, and by the end of the day there is a clean pile of road load data sitting on a drive. Then the real work starts: ingest the files, check every channel, count cycles, build the damage matrix, compare against the rig's load schedule, find the severe events, scale it to customer life, and write it all up. A good engineer has scripts for most of these steps, but the scripts break on each new program, the steps live in different tools, and someone has to carry the data from one to the next by hand. So it rarely happens in one sitting. It gets picked up between other work and stretches across the better part of a week, and the slow steps are the first to be shortened or skipped when the schedule is tight.


This is a walkthrough of a real post-test analysis we ran with an agent doing that legwork. One prototype SUV, a mixed-terrain durability day, 93 instrumented channels. The data was ingested Monday evening. By Tuesday morning the full analysis was done and ready for engineering review. To keep it concrete we scoped it to the front suspension, but the same pipeline runs across the rest of the vehicle.


First, the time, since it is the thing everyone asks about. With the scripting a competent durability team already has, this post-test work is roughly a day and a half of focused effort, and in practice it stretches across most of a week because the slow steps keep getting deferred. The agent did all of it overnight, on every channel, including the ones that usually get skipped.


Post-test turnaround


manual (scripted)


agent-assisted


Data ingest + channel mapping


1–2 hrs → 3 min


~40×


Sensor QC across 93 channels


1–1.5 hrs → 30 sec


~150×


Rainflow + damage matrix


1.5–2.5 hrs → 20 min


~7×


Load exceedance vs rig target


1–1.5 hrs → 3 min


~30×


Top events + driver correlation


2–3 hrs → 8 min


~20×


Customer equivalence


0.5–1 hr → 2 min


~30×


Report drafting


3–5 hrs → 15 min


~20×


Total engineer time


~13 hrs → ~3 hrs


~5×


Rig re-test avoided


18% load gap caught before the rig was booked


weeks of rig time


Engineer hours assume the imperfect scripting a real team already has, the kind that breaks and needs per-program fixups. Agent compute was about 8 min. The ~3 hr figure is the engineer's review and sign-off.


But the hours are the least interesting part. What matters is that running every step overnight, on every channel, catches things a rushed week misses, and this run caught two. The rig's load schedule under-tested the severe tail that does most of the damage, by enough to have invalidated the entire accelerated test. And a front-left wheel-force sensor had quietly saturated, which floors every damage number it reports. Neither is exotic. Both are exactly the kind of thing that gets skipped when the schedule is tight. The rest of this post is what happened in those eight minutes of compute.


## The test day


The vehicle is a stock SUV prototype carrying wheel-force transducers, body and unsprung IMUs, damper position and load sensors, the steering system, and the full vehicle bus. The day is split into four terrain blocks that compress the 90th-percentile customer duty cycle into ten and a half minutes of driving.


Mixed-terrain duty cycle


customer life


test time


B1


Smooth highway


100 kph


65%


28.6%


B2


Belgian block


35 kph


20%


28.6%


B3


Unimproved trail


50 kph


12%


28.6%


B4


Severe rock garden


30 kph


3%


14.3%


B4 takes 14.3% of test time but only 3% of customer life. The severe terrain is deliberately amplified to accumulate fatigue inside a finite test window.


The important detail is the rock garden. It takes 14.3% of the test time but represents only 3% of customer life. That over-representation is deliberate: severe terrain is amplified to accumulate meaningful fatigue inside a finite test window, then scaled back to customer-equivalent distance in post-processing. Holding onto that 3%-versus-86% relationship is what most of this analysis is really about.


## Overnight, unattended


The pipeline ran on its own overnight and finished in eight minutes of compute. Nothing here is novel as a set of steps. Every durability engineer knows them. What changes is that they all run, in sequence, on every test, without a person feeding files from one tool to the next.


Overnight processing · unattended


8 min 14 s


04:31


Data ingested


Complete


4 MF4 files · 570 MB · 93 channels mapped to durability short names, written to clean parquet per block.


04:32


Sensor QC


3 flagged


4 automated checks (drift, saturation, dropout, flatline) across all 93 channels.


04:33


Rainflow cycle counting


Complete


ASTM rainflow on all WFT_FZ channels × 4 blocks. 12,983 cycles on the front-left campaign. Pseudo-damage Σ(range⁵) per channel × block.


04:35


Load exceedance vs rig target


Gap +18%


Cumulative exceedance curve compared against the 329 LT rig schedule. Gap detected in the high-load tail.


04:36


Top 5 severe events


5 events


Peak detection above the 90th percentile, one per corner, cross-referenced against the driver radio transcript (22 timestamped comments).


04:37


Customer equivalence


~800 km


Damage-per-km weighted by the 90th-percentile mission profile, scaled to a full 6-hour test day.


04:39


Report drafted


Ready


All sections assembled, verdict computed, delivered to the shared drive.


By the time anyone arrived Tuesday, the report was already on the shared drive with a verdict attached. The value is not just the speed. It is that the slow, easily-skipped steps, like the driver radio correlation and the rig schedule comparison, got done at all.


## Sensor QC comes first


No damage number means anything if the sensor that produced it was lying. So the first gate is a health check on all 93 channels for drift, saturation, dropout, and flatline.


Channel health check


93 channels


Pass


75


Flatline


15


Drift


1


Saturation


1


Dropout


1


WFT_FZ_FL


saturation


Clipped at ±6,800 N on B2, B3, B4. Sensor undersized, so damage on this channel is underestimated.


WFT_FZ_FR


drift


Slow bias growth, +151 N by end of B4 (~3% of static load). Within spec; monitor next test.


Damper_Jounce_FL


dropout


25-second zero-flat window on B1 (t = 95–120 s). Connector intermittent or logger buffer overflow.


Most channels pass. Fifteen flatline, which is expected, since brake torque, tie-rod, and bump-stop channels simply are not exercised in a straight-line duty cycle. Three are flagged, and one of them matters a lot: the front-left vertical-force sensor saturated at its ±6,800 N limit on the three rough blocks. It was undersized for this prototype, which means the damage it reports is a floor, not the truth. That single fault propagates all the way to the verdict.


## Peak loads


With the channels triaged, the first quantitative pass is peak vertical force per corner per block, expressed as a dynamic factor against the static load. Anything meaningfully above 1.10× is worth a look for ultimate-stress margin.


Peak vertical wheel-center force


per block · corner


Highest single event: 7,347 N front-left in B4 (1.14×). Rear-left reaches 1.19×, the largest dynamic factor of the day. Numbers in red on the front-left are floored by sensor saturation, so the true peaks are higher.


The largest single event is 7,347 N on the front-left in the rock garden, a 1.14× factor. The rear-left reaches 1.19×, the highest of the day. None of these are alarming on their own, but peaks are not what kills a component. Fatigue is about the whole spectrum of cycles, not the single largest one.


## Where the damage actually is


So we count cycles. ASTM rainflow on every wheel-force vertical channel across every block, with pseudo-damage computed as Σ(range⁵). That is the standard slope exponent for welded steel, and a damage proxy that ranks correctly without committing to a specific S-N curve.


Pseudo-damage by block


Σ(range⁵) · % of campaign


WFT_FZ_FL


WFT_FZ_FR


WFT_FZ_RL


WFT_FZ_RR


B4 (rock garden) is 86.2% of total Fz pseudo-damage despite 14.3% of test time and 3% of customer life. The rear-left corner alone is 37.96%, the single most-damaged channel. This is the range⁵ exponent at work, not a quirk of the test.


The result is stark. The rock garden block produces 86.2% of the total damage from 14.3% of the test time, and the rear-left corner alone accounts for 38% of the campaign. This is not a quirk of the test design. It is the range⁵ exponent at work. A single 1,849 N cycle from the rock garden does roughly 580× the damage of a single 317 N highway cycle. Large events dominate fatigue completely, which is exactly why the next step matters.


## The rig was about to run the wrong loads


Every accelerated rig test runs to a load schedule, the recipe that tells the rig how hard to push and how many times to do it. The one queued up for this program was the 329 LT schedule, and on paper it looked like a perfectly reasonable default. There was just one problem nobody had caught. It was written for the previous generation of this vehicle, which carried less mass over the front axle. So the agent ran the comparison that, on a tight program, almost never gets done in time. It took the 12,983 cycles it had just counted, built the cumulative exceedance curve, and laid it straight over what the rig was actually going to apply.


Load exceedance vs 329 LT rig schedule


test


rig target


Low-to-mid loads match the rig schedule exactly. The whole gap sits in the severe tail above 1,000 N, the regime that drives 86% of the damage. Run the schedule as it stands and the part is under-tested in the one place it actually fails.


For most of the range, the two curves sit right on top of each other. The rig handles the everyday loads exactly as it should. Then you get to the tail, the severe events above 1,000 N, and a gap opens up. There the rig under-applies by roughly 18%. That sounds like a rounding error until you remember where the damage actually comes from, because that same tail is responsible for 86% of it. The rig, in other words, was set to faithfully test the part everywhere except the one place it actually breaks.


Leave that alone and you get the worst outcome durability can hand you. The part passes on the rig, the report signs it off, and the real weakness sails through into production, where it shows up as warranty claims and a recall instead of a line item in a test report. The fix is almost insulting in how small it is: raise the severe-event counts by 15 to 18% before the rig starts. The hard part is timing. A finding like this is only worth something if it lands before someone hits go on the rig. Catch it the following Friday, the way the old workflow would have, and you have already burned weeks of rig time running the wrong test.


## The severe events, in context


Peaks are also where the human record lives. The agent pulled the top five events, one per corner, and matched each against the driver's radio transcript within a thirty-second window.


Top 5 severe events


peak Fz · driver radio


1


B4 / FL


00:09:17


7,347 N


“Peak hit on FL. Felt that one through the seat.”


2


B4 / FR


00:09:05


7,251 N


“Rock garden. 30 kph. Hold on.”


3


B3 / FR


00:07:47


6,973 N


“Peak hit on FR. Felt that one through the seat.”


4


B3 / FL


00:06:21


6,911 N


“Trail block. 50 kph through rough.”


5


B2 / FL


00:05:57


6,835 N


“Peak hit on FL. Felt that one through the seat.”


Events 1 and 2 land within 12 seconds of each other at the start of the rock garden. The driver call at event 2 is the block-start callout, not a reaction to the hit. Radio comments are matched on a ±30 s window, never treated as exact event markers.


This is the kind of correlation that gets dropped first under deadline pressure, and it pays for itself immediately. It also shows why the matching has to be fuzzy: the driver's "Rock garden. 30 kph. Hold on." at event 2 is a block-start callout, not a reaction to that specific hit. Radio comments are context, not timestamps.


## Back to customer life


Finally, everything is weighted by the 90th-percentile mission profile and scaled to a full six-hour test day. That ties the abstract damage numbers back to something a program manager can act on.


Where the life goes vs where the damage goes


~800 km equivalent


Customer life


B1 · 65%


B2 · 20%


B3 · 12%


Test damage


B3 · 11%


B4 · 86%


Highway


1×/km


Belgian block


340×/km


Trail


920×/km


Rock garden


27,900×/km


The rock garden is 27,900× more damaging per kilometer than highway driving. That is why a 3% slice of customer life produces 86% of the fatigue damage, and why severe terrain has to be weighted, not just sampled.


A full test day at this terrain mix is worth roughly 800 km of customer use. The inversion is the whole point: customer life is dominated by the highway, but damage is dominated by the rocks, because the rock garden is 27,900× more damaging per kilometer. A duty cycle that just *sampled* terrain in proportion to use would never accumulate meaningful fatigue. Severe terrain has to be weighted, and then carefully scaled back, which is exactly what this pipeline does.


## The verdict


All of it rolls up into a single call, with the caveats kept attached rather than buried.


Component verdict


Conditional Go


Conditional Go · Medium confidence


Front suspension likely survives. Two actions required before final sign-off.


Peak dynamic factor of **1.14×** on the front-left corner (B4) is within the design envelope. Damage is dominated by the rock garden block (86.2%), consistent with expectations for this terrain mix, and the load spectrum matches the design target in the low-to-mid range.


Confidence is medium rather than high for two reasons: the rig schedule must be updated before the accelerated test is valid, and the front-left WFT sensor saturated on three of four blocks, meaning true damage on that channel is higher than measured.


Action 1: Update 329 LT rig schedule.


Increase severe-event cycle counts by 15 to 18% in the >1,000 N range before rig start. Without it, the accelerated test under-tests the regime responsible for 86% of damage.


Action 2: Re-instrument WFT_FZ_FL.


Replace the front-left vertical-force sensor with a higher-range unit before the next run. Current damage on that channel is underestimated on B2, B3, and B4.


Monitor: WFT_FZ_FR drift.


3% bias growth across the campaign. Within acceptable range today; check sensor calibration before the next test.


This is the part we are deliberate about: the agent does not decide whether the part is good. It assembles the evidence, computes the verdict against the methodology the engineer defined, and surfaces the two things that would otherwise be easy to miss: the load gap and the saturated sensor. The sign-off stays with the engineer.


## Why it matters


None of these steps are new. Every durability engineer knows how to count cycles, sanity-check a sensor, or hold a load history up against the rig schedule. What changes when an agent runs the whole pipeline is that all of them actually happen, every time, on every channel, instead of just the ones that survive the deadline. The load gap is the clearest example. It is exactly the check that gets quietly dropped in a busy week, and it is also the one that would have cost the most to miss.


So it is worth being concrete about that cost. An accelerated durability campaign is weeks of near-continuous rig time. Run it against the wrong loads, realize later, and the whole test is invalid. You do it again: the rig time, the re-instrumentation, and often a fresh prototype, all of it. That runs comfortably into six figures before you count a single day of schedule, and on a program approaching start of production, the slip usually hurts more than the re-test does. The agent caught this one in eight minutes, before the rig was even booked. You only bank that on the runs where the gap would have slipped through, but for a check this easy to skip, it does not have to happen often to pay for itself many times over.


---


*Data note. The telemetry in this study was generated with VI-CarRealTime 2026 on a stock SUV configuration across rough-road profiles at varying scale factors. Tools: VI-CarRealTime 2026, Python (asammdf, pandas, numpy, scipy), and the MOVEdot agent platform.*


If you run durability programs and recognize the week that disappears into post-processing, we would like to talk. Get in touch:founders@movedot.com , or[www.movedot.ai](https://www.movedot.ai/) .
