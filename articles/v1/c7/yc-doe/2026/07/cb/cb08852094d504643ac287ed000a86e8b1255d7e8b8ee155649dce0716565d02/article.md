---
schema_version: "1.0.0"
document_id: "cb08852094d504643ac287ed000a86e8b1255d7e8b8ee155649dce0716565d02"
company_key: "yc-doe"
company: "Doe"
source_id: "yc-doe-news-import-a3f9325ff0ec"
canonical_url: "https://doe.so/blog/human-time-returned"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-21T16:44:00.145230+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:c011cda02408ac0b79ca5060cbfcca4ed51f744aa8b7c25a55b9daa89c155442"
---

# Time as the Unit of AI Value

In 1911, Frederick Winslow Taylor published The Principles of Scientific Management and changed the texture of industrial life. His instrument was the stopwatch. His subjects were workers. His premise was that human labor could be decomposed into measurable motions, timed, optimized, and standardized. Taylorism produced enormous gains in output, and it produced something else too: a century of unease about what happens when measurement is pointed at people. The stopwatch became a symbol of surveillance, of the manager hovering over the shoulder, of work stripped of judgment and reduced to seconds.


A century later, the stopwatch is back. But this time it is pointed in the other direction. The most honest way to measure what artificial intelligence is worth is not tokens generated, not benchmark scores, nor parameter counts. It is time. Specifically, it is human time returned: the hours a person would have spent on a task that they no longer have to spend, because a machine did the work instead. And when you measure AI this way, something interesting happens. The entire apparatus of scientific management, the timing, the decomposition of tasks, the relentless optimization, gets applied to the machine rather than the human. Taylorism inverted: measurement liberates the person instead of constraining them.


## The wrong denominators


The AI industry has settled into a handful of default metrics, and nearly all of them describe the machine rather than the outcome. We count tokens because tokens are what the meter measures and what the invoice itemises. We publish benchmark scores because benchmarks are legible, comparable, and satisfying to rank. We track context windows, throughput, and latency percentiles because these are the quantities engineers can actually instrument.


None of these numbers answers the question a buyer, a builder, or a skeptic actually cares about: did this thing give me my afternoon back? A model that produces ten thousand tokens of elegant prose that nobody needed has created no value. A model that scores in the ninetieth percentile on a reasoning benchmark but takes longer to supervise than the task would have taken to do by hand has created negative value. Token counts measure activity. Benchmarks measure capability under laboratory conditions. Neither measures what economists would call the consumer surplus of the exchange, which in the case of AI-performed work is almost entirely denominated in hours.


This is not a pedantic complaint about dashboards. Units of account shape behaviour. When the industry prices and evaluates AI in tokens, it optimises for verbose output and impressive demos. When it evaluates in benchmark deltas, it optimises for leaderboard positions that correlate loosely, at best, with usefulness. If instead we accounted for AI in units of human time returned, we would optimise for something much closer to the actual point: finishing real work faster than a person could, reliably enough that the person can walk away.


## Taylor's stopwatch, reversed


Taylor's method had three moves. First, decompose work into discrete, observable tasks. Second, time each task precisely. Third, restructure the work so that each unit of time yields more output. The moral problem with Taylorism was never the measurement itself; it was the asymmetry. The manager held the stopwatch, and the worker absorbed the consequences. Measurement flowed downhill onto the least powerful person in the room, and the gains flowed uphill.


Applied to AI agents, the same three moves lose their sting and keep their power. Decompose the work: an agent session is already a naturally bounded unit of task execution, with a beginning, an end, and a result. Time it: every agent's session has a wall-clock duration measured to the second, with no ambiguity and no resentment, because software does not experience being timed. Restructure for efficiency: when a class of sessions runs long, you improve the system, not the disposition of a tired human being.


The asymmetry that made Taylorism corrosive simply is not present. The entity being measured is a machine. The entity being liberated is a person. Every optimisation pressure that once bore down on the worker's body now bears down on infrastructure. The worker, for the first time in the history of scientific management, is the beneficiary of the stopwatch rather than its target. Taylor believed, sincerely if naively, that scientific management would create prosperity for worker and employer alike. He was wrong about the mechanism because his mechanism required timing people. Point the stopwatch at the machine and his arithmetic finally works the way he promised.


## What the ledger actually says


This argument would be idle philosophy without numbers, so here are ours, drawn from Doe's own production ledger. Across our platform, a cohort of more than 40,000 agents has completely roughly 22,000 tasks. That work was estimated to take a human 5,951 hours to do manually. The agents themselves spent just 1,655 hours of wall-clock time doing it. That is about a 3.6x time leverage: for every hour of machine time expended, roughly three and a half hours of human time came back.


Stated in the currency of a working life, nearly 6,000 hours is close to three person-years of full-time labor. That labor did not disappear; it was performed, completed, and delivered. What disappeared was the requirement that a human being sit through it. This is the figure that token counts and benchmark scores are incapable of expressing, and it is the figure that matters.


The per-task view sharpens the picture. Our production data shows that the average task would have taken a person around 2,425 seconds, which is about 40 minutes of sustained attention. The average agent session that completed the same work ran 438 seconds. A forty-minute errand of the mind, the kind that fragments an afternoon, compressed into roughly seven minutes of machine time that the person did not have to attend to at all.


Forty minutes is a revealing number in its own right. It is too long to be trivial and too short to be a project. It is precisely the size of task that populates the interstitial tissue of knowledge work: the report to reformat, the dataset to reconcile, the correspondence to draft, the research question to run down. These tasks are rarely anyone's job description, but in aggregate they are nearly everyone's actual day. That is where the leverage lands, not on the rare heroic project but on the dense, unglamorous middle of the workweek.


## The shape of returned time


Averages flatter and averages deceive, so it is worth looking at the distribution. In our production data, the median agent session runs 170 seconds, while the average runs 438 and the 95th percentile stretches to 1,652 seconds. The distribution is heavily right-skewed: most sessions are brisk, under three minutes, while a minority of long, complex sessions pull the mean upward. The same skew shows up in the work itself: the median task involves 92 recorded actions, the average 467, and the 95th percentile more than 2,100.


This shape is itself a finding. It says that agent work behaves like a portfolio, not like an assembly line. Taylor's factory floor rewarded uniformity; every deviation from standard time was a defect to be engineered away. Agent workloads reward the opposite. The half of sessions that finish in under three minutes are the quick wins, and the long tail out at 1,652 seconds and beyond represents genuinely hard tasks, the ones where twenty-seven minutes of machine time may be substituting for a human day. A uniform distribution would suggest the system only handles one size of problem. A skewed one suggests it flexes across the real diversity of work.


The distribution also carries a design lesson. If the median session is only a couple of minutes, then the experience of delegating to an agent is usually the experience of a short wait, short enough to stay in flow, long enough to start something else. The occasional long session changes the interaction contract: the person should be able to hand off the task and genuinely leave. Systems built around time as the unit of value learn to signal which regime a task belongs to, because the value of returned time depends on the person actually being able to spend it elsewhere. Time that is returned but spent watching a progress indicator has not really been returned.


One more number belongs in this section: the average agent turn processes for 446 seconds. A turn is a unit of sustained, uninterrupted machine effort, and seven and a half minutes of it is longer than most humans can hold undivided attention on a tedious task without checking a phone, refilling a coffee, or losing the thread. The machine does not lose the thread. Part of the leverage, in other words, comes not from raw speed but from the absence of the friction that surrounds human effort: the warm-up, the context reload, the fatigue tax. Taylor spent his career trying to engineer those frictions out of people. They do not need to be engineered out of software; they were never there.


## Leverage that compounds


A single measurement is a snapshot; the trajectory is the story. Monthly actions in our system, the individual agent activity events we record as work done, grew from roughly 7,400 in November 2025 to more than 2.7 million in June 2026. That is not a marketing curve; it is a behavioural one. Nobody mandates that people delegate work to agents. Every one of those actions is a concrete step a machine took on someone's behalf, inside a task that a person judged, based on accumulated experience, was the better use of the next forty minutes of their life.


Growth of that shape suggests the economics are being felt, not just claimed. Time is the one commodity whose scarcity every knowledge worker experiences viscerally and daily. When a tool reliably returns it, usage does not need to be evangelised; it spreads the way any good trade spreads, because both sides of the exchange keep coming back. An expansion of that magnitude reads less like the adoption of a technology and more like the discovery of a favorable exchange rate in one's own time.


There is a compounding effect hiding inside the aggregate as well. Returned time is not consumed once; it is reinvested. The person who recovers forty minutes does not typically spend them idle. They spend them on the work that was being crowded out, some of which generates more tasks worth delegating. Time leverage feeds its own demand curve, which is exactly what that growth looks like from the outside.


## What time-denominated accounting changes


Adopting human time returned as the unit of account is not merely a reporting preference. It reorders priorities in at least three practical ways.


First, it changes what improvement means. Under token accounting, a system improves when it produces more output per dollar. Under time accounting, a system improves when the gap widens between what a task would have cost a human and what it costs the machine, and when the person can trust the handoff enough to fully disengage. Those are different engineering targets, and the second one is the one users experience.


Second, it changes how value is communicated. A time-leverage claim measured across millions of recorded actions is auditable in a way that qualitative promises about productivity never are. Every action is logged; every task has an estimable manual cost. The ledger is checkable, and because it is checkable, the hours convert cleanly into the language a finance committee actually speaks: net hours returned times a loaded cost per hour, set against the all-in cost of the tool, produces a payback period rather than a leap of faith. Taylor's deepest insight, that management should argue from measurement rather than anecdote, survives fully intact; only the target of the measurement has moved.


Third, it changes the social meaning of the technology. The recurring fear about automation is that it does to knowledge workers what Taylorism did to machinists: fragments the work, times the fragments, and squeezes. Time-denominated accounting makes the opposite claim, and makes it falsifiable. If the hours returned are real, they show up in the data as a widening spread between manual estimates and machine wall-clock. If they are not real, the same accounting exposes the shortfall. A metric that can embarrass its own proponents is worth far more than one that cannot.


## The honest caveats


Time accounting has failure modes, and pretending otherwise would repeat Taylor's mistake of overclaiming for a method. Manual-time estimates are estimates; they can be generous, and any serious accounting should be transparent about how they are produced. Wall-clock time understates the human cost when a person must review or redo the machine's work, so returned time should be netted against supervision time, not reported gross. And not all hours are equal: forty minutes recovered from drudgery is not the same as forty minutes carved out of work someone found meaningful, even if the stopwatch cannot tell the difference.


But notice that every one of these caveats is an argument for better time accounting, not for retreating to tokens and benchmarks. The critiques all live inside the time frame: net the hours honestly, weight them sensibly, audit the estimates. That is what it looks like when a unit of account is the right one; even its critics denominate their objections in it.


## The wager


Frederick Taylor died in 1915, stopwatch in pocket, convinced that the precise measurement of work would end the antagonism between labor and management. It did not, because measuring people is not a neutral act. Measuring machines is. The inversion matters morally as much as it matters economically.


The numbers behind all of this sit a few paragraphs up, and they read as more than a tally: behind each line item is a person who got a piece of their day back and, judging by the growth curve, came back for more.


If that ledger reads as a claim rather than a proof, the remedy is small and cheap. Take one narrow, high-frequency task, measure the honest hours it costs a person today, run it through agents for four weeks with someone reviewing every output, and compare. Report the hours returned net of that review, alongside the error rate and the loaded cost. Doe keeps exactly this ledger by default, which is why the numbers above could be pulled from production rather than assembled for a pitch. The wager costs four weeks and a rounding error; what it buys is the first line of your own version of the table above.


The industry will keep publishing token counts and benchmark tables, and they will remain useful the way an engine's RPM gauge is useful: as an instrument reading, not as a destination. The destination is measured in hours. When the history of this technology is written, the interesting number will not be how many tokens the machines produced. It will be how much time the humans got back, and what they chose to do with it. That is the unit of account worth optimising, the metric worth auditing, and the promise worth holding the whole field to: not artificial intelligence measured by its own output, but human time, returned with interest.
