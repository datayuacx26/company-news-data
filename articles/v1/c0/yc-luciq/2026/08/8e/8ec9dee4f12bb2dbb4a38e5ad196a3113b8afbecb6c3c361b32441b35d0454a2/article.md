---
schema_version: "1.0.0"
document_id: "8ec9dee4f12bb2dbb4a38e5ad196a3113b8afbecb6c3c361b32441b35d0454a2"
company_key: "yc-luciq"
company: "Luciq (formerly Instabug)"
source_id: "yc-luciq-news-import-4fda431b7a4e"
canonical_url: "https://www.luciq.ai/blog/ten-years-mobile-observability"
published_at: "2026-08-10T15:21:01.139+00:00"
first_seen_at: "2026-08-10T00:49:47.550665+00:00"
fetched_at: "2026-08-10T00:49:48.393459+00:00"
content_hash: "sha256:def0a45acf03ad94ad0ead38102c7005b0cb0284a764b4771e28702f7c160ce1"
---

# Ten Years In Mobile, Ten Years of the Same Question

*In August 2016 I signed an offer letter with a company called Instabug. It is called Luciq now, and almost nothing we sell would be recognizable to the person who signed that letter. The problem I walked in to solve is the one I worked on this morning.*


Before Instabug I built mobile apps at a software house. Client work. We took a spec, built against it, handed over a signed build, and the client owned everything downstream: the store listing, the release, the users, the support inbox. I never met a single person who used what I made.


Everything I learned about how the app behaved in the world came back as email. A client would send three sentences and a screenshot. The app froze when I tried to check out, can you fix it today. That was the whole bug report. No device model, no OS version, no idea whether they were on wifi or two bars of 3G in a parking garage, no notion of what they tapped in the ninety seconds before it broke.


So I would write back asking. They would reply the next day, half answering. I would install the build on every device on my desk and try to make the app fail on command. Usually it would not. I once spent two days reproducing a bug that took eleven minutes to fix.


I want to be precise about this, because it runs through everything below.


‍ ***The work was never the fix. The work was reconstructing what the user had already experienced.***


[Debugging](https://www.luciq.ai/blog/luciq-ai-debugging) was archaeology performed on a JPEG. That is the problem I joined Instabug to solve. Not a market I had analyzed. A thing that had personally wasted years of my life.


## 2016: Putting the Debugger in the User's Hand


The product I joined ran on an idea that sounds obvious now and was strange then. The person holding the phone is already holding the most complete debugging environment you will ever have. They have no way to hand it to you.


So we let them shake it.


Shake the phone, a reporting sheet appears, the user annotates the screenshot and writes one line. What arrives on the other end is the device, the OS, the free memory, the network state, the console logs, the app version, and the steps that led there. The user made one gesture. We did the archaeology.


It worked. Within a few years you could shake some of the largest apps in the world and watch a reporting sheet slide up. That was a strange and excellent feeling.


The lesson I took from it had nothing to do with the gesture. The industry had quietly accepted a whole category of wasted work as the cost of doing business. Nobody was asking for shake to report, because nobody believed reproduction was solvable. It was just the job. The best products delete a step everyone has stopped questioning.


## 2016 – 2018: Everybody Wanted the Crash


The center of gravity in 2016 was[crash reporting](https://www.luciq.ai/blog/top-mobile-crash-reporting-tools) . If you ran a mobile team, capturing crashes was the most sophisticated thing you could do. If you sold tools, it was the entire roadmap: symbolication, grouping, deduplication, alerting.


It did something important. It gave engineering leaders a number, crash-free sessions, and quality stopped being a feeling you had about your app. It became something you could put in a board deck.


Then it got commoditized. Crash capture went free, bundled into the platforms, table stakes inside two years. That was the first time I watched a category compress in real time, and the lesson stuck. A signal everyone can capture stops being a differentiator the moment everyone captures it. The value moves to what you do with it.


There was a deeper problem that took the industry years to admit. You could sit at 99.5% crash-free and still have users quietly hating your app. The six-second screen, the button that did nothing, the flow that dead-ended, all of it invisible. We had built a good instrument for measuring one kind of failure and then mistaken it for a measure of quality.


Crash-free is not the same as working. It took most of a decade for that to become common sense.


## 2019: Slow Is a Bug


By 2019 apps had gotten good, so expectations had gotten brutal. Users stopped grading mobile apps against other mobile apps and started grading them against the best app on their phone.


The alternative to measuring performance in production was measuring it on your desk, on a flagship device, on office wifi, against a staging server two milliseconds away, with a warm cache. Everything is fast on your desk. Production is a four-year-old Android on a congested network in a country you have never visited, and it behaves like a different app.


So we built[APM](https://www.luciq.ai/blog/mobile-application-performance-monitoring-for-mobile-teams) for mobile. Not backend APM pointed at a phone, but mobile-native: app launch, screen loading, network calls, the things a user physically waits for. Only the sharper teams cared at first. Performance felt like a luxury concern, something you got to once the crashes were handled.


They were right to care early. Slow never files a ticket. It shows up in your retention curve a quarter later.


## 2020 – 2023: The Stack Trace Stopped Explaining Things


The failures changed shape. As apps grew more dynamic, with remote config, experiments, personalized flows, and half the UI decided at runtime, the interesting problems stopped being deterministic. A stack trace tells you where the process died. It tells you almost nothing about why the user got there, or what they were looking at when they did.


A growing share of real failures produced no stack trace at all. A button rendered behind a banner throws nothing, because the code ran perfectly. A Continue that routes to the wrong screen throws nothing either, and it is a working app failing at the only thing the user wanted. To your telemetry, both sessions look healthy.


That is why session replay arrived on mobile when it did. Teams needed to see. Mobile made it hard. You cannot ship a recorder that eats battery, and you cannot move a user's personal data off their device to satisfy your curiosity. Masking had to happen on the device, before anything left it. Solving that properly took years, and it is the least glamorous work I have ever been proud of.


Replay showed us what one session looked like. It still could not tell us how many people fell out of checkout on Tuesday, or why. That is a different shape of question and it needed different instruments. Funnels, to see where a flow leaks at population scale. Surveys, to hear the reason from the person who left. A funnel tells you eleven percent abandon at the payment step. A survey tells you they did not trust the card form. Neither is an error. Both are the product failing.


That was the point where the tooling stopped being about failures and started being about outcomes. A crash is a defect. An eleven percent drop at payment is a number with revenue attached, and once you can see the two side by side, somebody has to decide which one deserves the sprint.


Look at the shape of the decade. Every few years we added a dimension. What broke, then how fast, then what it looked like, then where people fell out and why. We were not collecting more data for its own sake. We added dimensions because the previous set had stopped explaining the failure.


Ten years of tooling, read as accumulation rather than replacement. Nothing on this chart was ever retired. Each band was added because the ones below it had stopped explaining the failure.


## 2022 – 2026: Building Got Cheap. Nothing Else Did.


Late 2022 moved the ground. By 2026, with agentic coding tools in every serious workflow, writing code is no longer the constraint on shipping. A team that shipped one meaningful change a week ships five. That is real, and it broke two things at once. Both of them are the problem I had in 2015 with that screenshot.


### The first is context


An agent is an extremely fast engineer with no memory of your product and no access to production. Ask it to fix a crash, hand it a stack trace, and you have recreated my 2015 inbox with better grammar. It will guess. It will guess confidently, produce a plausible diff, and you will find out next release whether the guess was any good.


Give it the full state, what rendered, what the user tapped, which build, which device, which network call quietly failed with a 200, and it stops guessing and starts reasoning. Same model, same fix, completely different reliability. The bottleneck was never the model's intelligence. It was the poverty of what we were feeding it.


Same model on both sides. The only variable is what it was handed.


That is the part I still find funny. We spent ten years building an instrument to help humans reconstruct a session, and it turned out to be the exact fuel autonomous systems needed. Nobody planned it. It is what a decade of unglamorous mobile work compounds into.


### The second is what to build


This one gets less attention and I think it matters more.


When building was expensive, prioritization corrected itself. You got a few shots a quarter, so you argued hard about each one, and the cost of being wrong was visible before you started. When building is cheap you can be wrong five times a week, at speed, with a very green dashboard. Velocity without judgment is a faster way to ship the wrong thing.


We had a release where a crash showed up on under one percent of sessions and a checkout step was leaking double digits. Engineering instinct said fix the crash first, because a crash is a defect and defects are our job. The funnel said the leak was worth an order of magnitude more. Both got done that quarter, because building is cheap now. The order was the whole decision, and the only way to argue about the order was to put both things in the same unit.


So the scarce resource moved. It used to be whether we could build the thing. Now it is whether the thing is worth building, and whether I can prove it. A crash on three percent of checkout sessions has to stop being an engineering ticket and become what it actually is, a conversion problem with a dollar figure attached, weighable on the same scale as the feature three people are lobbying for. Fixes and features have to compete in one currency, and that currency is business outcome.


## What Actually Changed: The Qestion, Not the Signals


The signals changed and the categories came and went. What really moved was the question the tooling was expected to answer.


Solid is the window where answering the question was a differentiator. Pale is table stakes. Every question on this chart got about three years.


Three years each, roughly, before the answer became something everybody could produce. If you are building in this space, that is the only forecast I would make with any confidence. Whatever signal feels advanced today is table stakes by 2029, and the value will have moved one rung up.


What does not commoditize is the judgment layer. Anyone can capture a crash. Ranking what matters against what it costs the business, and then handing that judgment to a system with enough context to act on it, is much harder to copy. It gets built out of ten years of knowing what mobile actually does when it breaks.


The job itself has not changed. In 2015 I was trying to understand what one frustrated user experienced, and I was doing it by squinting at a JPEG. Today the same question gets answered across millions of sessions, ranked by[what it is costing the business](https://www.luciq.ai/blog/mobile-observability-organization-types) , with a fix drafted before anyone opens a laptop.


Same question. Ten years of better answers.
