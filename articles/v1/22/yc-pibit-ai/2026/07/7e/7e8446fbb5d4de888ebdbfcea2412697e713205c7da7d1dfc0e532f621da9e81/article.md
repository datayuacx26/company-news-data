---
schema_version: "1.0.0"
document_id: "7e8446fbb5d4de888ebdbfcea2412697e713205c7da7d1dfc0e532f621da9e81"
company_key: "yc-pibit-ai"
company: "Pibit.ai"
source_id: "yc-pibit-ai-news-import-b491cbbe82b1"
canonical_url: "https://pibit.ai/blog/build-vs-buy-underwriting-automation"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-23T21:01:24.489812+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:bda4a7cd3fd782226810f4ac1f1360e1efa8be855e9506ed1cfefd763c0b534d"
---

# When both teams say build, they are usually describing different things

An engineer on your team spends two weeks with modern tooling and comes back with something that reads a loss run cleanly. It pulls the carrier, the policy period, the claim counts, the paid amounts and the reserved amounts, and it puts all of that into a table that looks finished. Everyone in the room is impressed by the demonstration, and they have good reason to be impressed. Somebody then asks the question that had already occurred to everyone else in the room. Why are we paying a vendor for something our own team just built in a fortnight?


That question is reasonable and it deserves a real answer instead of a defensive one. The answer has less to do with technology than with how the word build gets used inside a carrier.


## The word itself is the problem


The word build carries two meanings, and both of them get used in the same meeting. When someone from the business side says we should build this, they are describing an outcome they want to have. They picture submissions arriving in one place, information already extracted, underwriters moving through files faster, quotes going back to brokers sooner. The eighteen months of engineering work between the demonstration and that outcome sits outside the picture entirely.


When someone from engineering says we should build this, that same sentence carries a completely different set of meanings. It means headcount assigned for a long period, a roadmap that now has one very large item sitting on it, ongoing maintenance long after the first version ships, and a list of other projects that will wait their turn. Engineering is describing the work itself and the cost of committing to that work.


Both sentences use the same words and both people are being honest when they say them. The two meanings diverge, and the gap between them is where underwriting programs lose a year.


## A prototype supports both readings at once


A working prototype is the thing that allows both sides to hear agreement where none exists. The business looks at the screen and sees the outcome almost arrived already. Engineering looks at the same screen and sees a promising start on a road that runs for a while yet. Nobody discovers the mismatch during that meeting, because the demonstration genuinely supports both readings of it.


The mismatch surfaces around month fourteen, when the business asks why quotes are not moving faster yet and engineering explains what is still outstanding. By that point the money has been spent and the roadmap has been rearranged around a decision nobody examined properly.


## The question is not whether your team can do it


Start from the assumption that your engineers can build document infrastructure that holds up in production. The question worth asking has nothing to do with their capability.


Is submission intake where you want the next eighteen months of that team's capacity to go? While intake is being built, the pricing model refresh waits, the broker portal work waits, the rating engine migration waits, and the reporting rebuild waits behind all of them. None of that appears in the business case for the build, because delayed work does not get a line item anywhere.


Your position in the market comes from pricing, from risk selection, from appetite discipline, from how quickly you return a quote, and from the relationships your team has built with brokers. Extraction is necessary work that sits underneath all of it, and it is rarely the reason an account gets placed with you.


## What the eighteen months actually contain


The work between a prototype and a production system takes a long time and produces very little to demonstrate along the way. It becomes visible once real submission volume starts arriving through the system every day. Broker formats change without anyone telling you, scanned documents arrive at poor quality, values disagree across two documents inside the same submission, exceptions need defined handling rules that somebody has to write down, and every number that feeds a bound policy has to stay explainable to an auditor years afterwards.


Your team will work all of that out eventually, because the problems are tractable and your people are competent. The real question is how many months it takes them, and what does not get built during those months. We have written elsewhere about why[template-dependent extraction struggles once real submission volume arrives](https://pibit.ai/blog/why-ai-alone-wont-fix-submission-intake) , and the same pattern shows up at carriers of different sizes.


## You do not have to settle the argument to start


Here is the part of the argument that these conversations skip over. You do not have to resolve the build question before you start doing something useful about intake. You will not know whether building was the right call for at least another year, and nobody sitting in that room today knows it either.


The useful move is to lower the cost of being wrong, which is an easier problem to work on. Bring a modular intake system into production now, and let the build continue alongside it if the team still wants to build. Modular matters here because it means you can adopt one capability, leave the rest alone, expand later if the first capability earns it, and remove the whole thing without a migration if it does not.


The build team then stops working from assumptions and starts working from live submissions moving through a real system every day. That is a genuine improvement to the build itself, and it gets missed in these arguments. Meanwhile the business stops waiting for month eighteen and starts[returning quotes inside the broker's decision window](https://pibit.ai/blog/submission-turnaround-time-commercial-pc) long before then. By the time the original argument comes back around, it will have answered itself with evidence that nobody had access to before.


## What makes it reversible


For any of that to hold, the system you put into production has to have a few specific properties. It has to sit alongside your existing systems instead of replacing them, so that adopting it is not itself a migration project. It has to be adoptable in pieces, so the first commitment you make stays small and stays reversible. The structured data it produces has to remain yours and stay portable, so that leaving is a migration you can actually perform later. The commitment period itself has to be short enough that reversing the decision stays practical for you.


This is how we have built the CURE™ platform, and it is worth being direct about what is involved. There is real onboarding of two to three weeks, spent working through your underwriting guidelines and configuring the models against them. After that configuration we own the accuracy, at 99.9% at field level, and we maintain the engine as broker formats change and new lines get added.


## One thing worth doing this week


If a build conversation is currently live inside your organisation, there is one exercise worth running before the next meeting. Have the business side and the engineering side each write down separately what they mean by build this, in as much detail as they can manage, and then compare the two documents. The two descriptions will diverge, and finding that out this week costs less than finding it out in month fourteen.
