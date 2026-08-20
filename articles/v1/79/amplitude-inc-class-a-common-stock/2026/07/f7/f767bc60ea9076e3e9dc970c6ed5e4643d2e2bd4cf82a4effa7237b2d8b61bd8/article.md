---
schema_version: "1.0.0"
document_id: "f767bc60ea9076e3e9dc970c6ed5e4643d2e2bd4cf82a4effa7237b2d8b61bd8"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/hybrd-agent-evals-retention-signal"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T18:31:55.615982+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:130a95cee4291927790522411e1e8a1314c76d23554325aa580c2e5169997d89"
---

# How HYBRD Turned Agent Evals into a Retention Signal

A good coach costs about $500 a month, the kind who actually reworks your plan as your life changes. Almost no one pays that. Most people train off a PDF they downloaded once, or an app that calls itself adaptive until you add a cross-training day and the plan breaks.


[HYBRD](https://www.hybrd.com/) (Y-Combinator F2024) Brain is our answer: a coach you talk to. Tell it your goal, your schedule, and how your legs feel today, and it builds the plan around you. It reads your wearables and fills in what they miss, moving before you ask.


The whole point is that Brain adapts to the individual. Two athletes ask it the same thing and need opposite answers, one rebuilding after an injury, the other eight weeks from a race. That range is what makes quality so hard to measure. The same answer is right for one of them and wrong for the other, and the average across them tells you nothing.


So how do you measure something that individual? For a long time, we didn't really know.


## **When only engineers could grade the coach**


We started the way most engineering teams do. Our own eval framework, written in code, with test cases we wrote by hand and scoring logic we maintained ourselves. It worked. That also meant every eval lived in the codebase, so an engineer had to build, review, and deploy each one before anyone else could act on it.


The process was slow. A product teammate would spot something strange in Brain's behavior on a Tuesday, and the eval to confirm it might not ship for a week, if we chose to pull an engineer off the roadmap for it. We wanted anyone at HYBRD to write an eval, test a hunch about a new behavior, and see whether a change moved quality, without a deploy and without an engineer in the loop.


## **From code to a question anyone could ask**


##### Setting up Agent Analytics was easy in a way I did not expect. We pointed our coding agent at the docs, it wrote the instrumentation, and we were live.


No pipeline to run, no schema to design first. What it gave us was a different question. The old evals could tell us whether Brain passed a test we had written. This let us ask whether Brain was actually good at the job, which is fuzzier and the thing we really care about.


## **Telling a real bug from a bad day**


When someone tells us Brain nailed it, or blew it, the first job is working out what actually happened. Is it a real bug in the agent, a broken tool, or the model just off on that one request? A screenshot and an irritated email will not tell you.


One example. A few users said Brain sometimes needed a second nudge to run a tool: ask once, nothing, ask again, then it fires. Could have been three unlucky people, could have been most of them, and from the outside I could not tell which.


We built an evaluator in the Amplitude Global Agent to flag any conversation where someone had to ask twice for a tool to run, pointed it at the past week of sessions, and let it go. It came back at 25%, one in four sampled conversations, too many to write off as a fluke. The fix, mostly prompt changes to Brain, went out the same day, and the rate dropped.


Start to finish, the whole thing took a day, and nobody wrote a line of test code to do it.


## **The 4x we didn't expect**


Because Brain's activity lands in Amplitude as ordinary events, we could put agent quality beside the numbers we actually run on, retention and conversion.


##### Athletes who work with HYBRD Brain retain and convert at more than 4x the rate of athletes who don't.


We figured talking to the coach helped. Four times was not what we had in mind. That number changed what we work on. Improving Brain still matters, but getting a new athlete into a real exchange with Brain in the first few minutes now matters just as much, because that is when they decide whether to stay. So Brain went into onboarding, a notification nudges you to plan the week with it, and adjusting a workout runs through it by default. The opening minutes of the app steer you toward the coach now.


## **What comes next**


We keep giving Brain more to do. At first it could only build plans from blocks we'd defined, blocks built on the training methods of coaches like[Alex Viada](https://www.instagram.com/alex.viada/?hl=en) , who coined the term "hybrid athlete." As the toolset got sharper and we trusted the output more, we let it write individual workouts, then whole training blocks, inside the limits we set.


Each time, the same question comes up, the one we started with. Is the coach any good? Now we can answer it with a number, the same day we think to ask.


The goal is to give everyone the coaching that used to cost $500 a month. That is only worth anything if the coaching is right for the person actually following it, one athlete at a time. Being able to measure that, fast, is what lets us stand behind it.


See how Agent Analytics tells real bugs from noise →[Explore Amplitude Agent Analytics](https://amplitude.com/agent-analytics)
