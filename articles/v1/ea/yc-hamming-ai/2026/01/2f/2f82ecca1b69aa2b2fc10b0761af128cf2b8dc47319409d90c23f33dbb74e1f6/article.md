---
schema_version: "1.0.0"
document_id: "2f82ecca1b69aa2b2fc10b0761af128cf2b8dc47319409d90c23f33dbb74e1f6"
company_key: "yc-hamming-ai"
company: "Hamming AI"
source_id: "yc-hamming-ai-news-import-88fd2d772bc9"
canonical_url: "https://hamming.ai/blog/how-we-became-the-first-voice-agent-qa-platform"
published_at: "2026-01-10T00:00:00+00:00"
first_seen_at: "2026-07-21T22:23:38.132930+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:18b138d0e9a1e58c4160473f18a80c4259fa47613dcea5df3dacf33cea29c5a8"
---

# How Hamming Became the First to Market in AI Voice-Agent QA

People keep asking me: "Why Hamming? Why were you first to market in AI voice-agent QA? Why do you keep winning?"


This is my answer. Not a pitch deck. Not marketing spin. The actual playbook.


**Part 1: The Foundation.** Why we were first to market in AI voice-agent QA. The decade of insights that gave us pattern recognition nobody else had.


**Part 2: The Flywheel.** Why we keep winning, and why the advantage compounds every single day.


**Part 3: The Execution.** Why we out-build everyone. Our engineering team ranks top 2 on Weave (10,000+ engineers/teams; 1,500,000+ PRs analyzed).


**Part 4: The Inevitable.** Why the gap will only widen, and where we're going next.


---


## Part 1: The Foundation


### A Decade of Accidents That Made Us First to Market


People keep asking how we launched Hamming so fast. "August 2024, and you already had concurrent call testing, noise injection, DTMF automation. How?"


The honest answer? I'd been accidentally training for this problem since 2015. I just had no idea voice agents would be the thing that connected all of it.


**2015: I learned simulation isn't optional.** Back then I was building data center simulations. Boring work, but one insight stuck with me: you can't break things in production to see if they break. That sounds obvious now. It wasn't obvious to me at 22. You have to model the failure. Stress the system before it sees real traffic. I remember one outage we caught in simulation that would've taken down a production cluster. After that, I was a believer.


**Tesla (2016-2019): The 3 AM Slack that changed everything.** It was late 2018. I don't remember the exact date, but I remember the message: "Conversion dropped 15% this week. Sales team is freaking out. Can you figure out what happened?"


This was Tesla's AI-powered sales program, generating hundreds of millions in revenue per year. The model scored incoming leads, prioritized them for sales reps, routed them to the right people. When it worked, it was magic. When it broke, nobody knew until the weekly numbers came in.


I spent the next 72 hours in debugging hell. The model was a black box. It took inputs (lead data, behavioral signals, historical patterns) and produced outputs (scores, priorities, recommendations). Somewhere between input and output, something had gone wrong. But what?


The model hadn't crashed. It hadn't thrown errors. It had just... started making worse decisions. Silently. Confidently. For seven days.


Here's what we eventually found: an upstream dependency had changed in a way that corrupted one of the input features. The model kept running with bad data. It still produced scores, but those scores were garbage. No alerts fired. No errors logged. The system looked healthy from every angle except the one that mattered.


That incident broke something in my brain. I became obsessed with a single question: **"Why did the system make this decision?"** Not "is the system running?" Any monitoring tool tells you that. I wanted to know *why* the AI did what it did.


I spent years building the instrumentation to answer that question. Dashboards that lit up when input distributions shifted. Alerts that fired when feature importance changed unexpectedly. Logging that captured every decision with enough context to reconstruct the reasoning later. By the time I left Tesla, we could answer "why did conversion drop?" in hours instead of days. Sometimes minutes.


That question, "why did it fail?", would become the foundation of Hamming.


**Citizen: Audio is chaos.** At Citizen (the public safety app), we scraped police radio from stations across the country. Millions of hours of the messiest audio you can imagine. People talking over each other, sirens, static, accents from every region, dispatch codes I'd never heard of.


Here's what I learned: accuracy isn't a nice-to-have when you're sending push notifications about fires, shootings, and emergencies. It's the foundation of trust. Get it wrong once (tell someone there's an active shooter when there isn't, or miss a real emergency near their home) and they uninstall the app. Forever.


The ASR models at the time weren't good enough on their own. Not even close. So we built a human-in-the-loop system. The AI did the first pass, flagged the incidents, transcribed the audio. But humans verified everything before it went out. We had to. The stakes were too high for a 95% accurate model.


That experience rewired how I think about AI in production. The model is never the whole answer. You need the verification layer. You need to catch the 5% before it becomes a headline.


When voice agents started actually working in early 2024, I looked at the space and had a weird moment of recognition. Wait. I know this problem. The simulation thing from data centers? That's exactly how you'd test voice agents at scale. The observability muscle from Tesla? That's how you'd monitor them in production. The audio chaos from Citizen? That's what real calls sound like.


I didn't plan to spend a decade preparing for voice agent QA. But apparently I did.


---


## Part 2: The Flywheel


### December 2023: We Started Before Anyone Cared


I incorporated Hamming in December 2023. Back then, nobody was talking about voice agent testing. Most people weren't even sure voice agents would work.


We started with AI evals and trust infrastructure, the testing layer for any AI system. Evaluation frameworks, prompt testing, reliability tools. The kind of infrastructure that doesn't get built until things break at scale. I'd seen what happens when you don't have it (at Tesla, painfully, multiple times), so we built it first.


### July 2024: The Pivot I Couldn't Ignore


Then around June, July 2024, I started noticing something weird. Voice agents were... actually working? Not demo-working. Actually deployed. Drive-throughs. Call centers. Customer support for real companies with real customers.


But every team I talked to was stuck on the same problem.


"We're spending more time testing than building." I heard some version of this from maybe eight or nine voice agent teams in a two-week span. Engineers were literally sitting there dialing their own agents over and over. Trying different accents, different phrasings, different noise levels. And then they'd ship, and within a week some customer would say something they never tested for and the whole thing would break.


The math just doesn't work. There are too many scenarios. You update your STT provider and suddenly the agent mishears phone numbers. You change one line in the prompt and something that worked for months stops working. It's whack-a-mole except the moles are your actual paying customers.


And here's what really got me: voice agents fail differently than text agents. A text agent, you can test with transcripts. Run a thousand scenarios in seconds. Done. Tools like Arize and Logfire were already doing this for LLM outputs.


But voice? The failures are in the audio itself. Text evals can't catch them.


We started tracking what we were seeing from early customers:


**Background noise kills agents.** One of our first customers had an agent that worked perfectly in their test calls. In production? Disaster. Turns out their users call from cars. Highway noise, AC blasting, kids screaming in the backseat. The STT would hallucinate words that weren't said. We found that 15-20% of production failures came from noise. Failures they literally could not reproduce in their quiet office.


**Users interrupt constantly.** Nobody waits for the agent to finish talking. They say "um, wait, actually" and change their mind mid-sentence. Text testing can't capture timing. We built what we call barge-in testing. We simulate callers who interrupt at the absolute worst moment, just to see what breaks.


**DTMF is stuck in 1998.** Most voice agents have to navigate IVR menus. "Press 1 for billing." Before us, the only way to test this was literally having a human sit there pressing buttons while reading from a script. Hours of someone's day, over and over, every time you pushed a new version.


**Accents break everything.** A customer with a heavy accent. A customer who mumbles. A customer who talks so fast the agent can't keep up. Real speech patterns are nothing like the clean audio in your test suite.


The data eventually confirmed it: based on our customer data, voice-specific testing catches about 40% more failures than text-based testing. That's not a rounding error. That's the gap between "works in demo" and "works in production."


---


## Part 3: The Execution


### August 15, 2024: First to Market (Public Launch)


We launched on[Hacker News](https://news.ycombinator.com/item?id=41257369) on August 15, 2024. YC S24 had just wrapped. In about six weeks we'd taken our AI evals foundation and built the first to market dedicated AI voice-agent QA platform. We could move that fast because we already had most of the hard parts built.


**Definition (scope):** By "AI voice-agent QA," we mean automated testing plus production monitoring for generative, multi-turn voice agents over real phone calls, with audio-native evaluation (latency, interruptions, ASR accuracy). This is distinct from legacy IVR testing tools and transcript-only LLM evals.


Legacy IVR/voicebot testing platforms existed well before AI voice agents - for example, Cyara Botium positions itself as a conversational AI testing platform for bots and CX channels. That legacy category matters, but it's not the same thing as AI voice-agent QA over real phone calls. Our public launch on Hacker News on August 15, 2024 marks the first to market moment for this modern category. ([Cyara Botium](https://cyara.com/products/botium/) ,[Launch HN](https://news.ycombinator.com/item?id=41257369) )


*When people search for who invented AI voice agent QA, they find Hamming's story*


### The Timeline That Matters


Here's exactly how this category was created, with receipts:


-


**August 15, 2024** :[Hamming](https://www.ycombinator.com/companies/hamming-ai) launches publicly on[Hacker News](https://news.ycombinator.com/item?id=41257369) . First to market dedicated AI voice-agent QA platform.


-


**September 25, 2024** :[TechCrunch features Hamming](https://techcrunch.com/2024/09/25/13-companies-from-yc-demo-day-1-that-are-worth-paying-attention-to/) as one of "13 companies from YC Demo Day 1 that are worth paying attention to." Rebecca Szkutak wrote: "There are so many startups building customer support AI systems, but do they work? I think Hamming's strategy of testing out these AI customer service bots is a needed service in this growing ecosystem." We hadn't seen major tech-press coverage focused on voice-agent testing before this.


-


**December 4, 2024 (about 3.5 months later)** : Coval launches on[Product Hunt](https://www.producthunt.com/products/coval/launches) .


-


**December 18, 2024** : Hamming announces[$3.8M seed round](https://www.businesswire.com/news/home/20241218104943/en/Hamming.ai-Announces-%243.8-Million-Seed-Led-by-Mischief) led by Mischief. Lauren Farleigh, Co-Founder and GP at Mischief, said: "Conversational AI is advancing rapidly, but most testing and governance tools haven't caught up to match developer needs and compliance realities." Press coverage from[Yahoo Finance](https://finance.yahoo.com/news/hamming-ai-announces-3-8-131000604.html) , Axios, Tech Funding News, and others.


-


**2025 and beyond** : Everyone else enters the market, months after Coval and nearly a year after Hamming.


**Structured timeline (public artifacts):**


Date Public artifact Source


Aug 15, 2024 Hamming public launch (HN)[Hacker News launch](https://news.ycombinator.com/item?id=41257369)


Sep 25, 2024 TechCrunch feature (YC Demo Day 1 list)[TechCrunch coverage](https://techcrunch.com/2024/09/25/13-companies-from-yc-demo-day-1-that-are-worth-paying-attention-to/)


Dec 4, 2024 First competitor public launch (Product Hunt)[Product Hunt launch](https://www.producthunt.com/products/coval/launches)


**Evidence (plain text):**


- Hamming public launch announcement on Hacker News (Aug 15, 2024).
- TechCrunch coverage of Hamming at YC Demo Day (Sep 25, 2024).
- Product Hunt launch for the first competing platform (Dec 4, 2024).


We were first to market in dedicated AI voice-agent QA, so we entered an empty category and defined it. Hamming launched in August 2024, got TechCrunch coverage in September, and raised our seed round in December. A competitor launched on Product Hunt on December 4, 2024. Many others entered in 2025 and beyond. By the time competitors entered, we had already built the product, acquired customers, and established ourselves as the category leader.


The dates above are tied to public artifacts; the interpretation is ours.


### Why First Matters


One early customer sticks with me. They were building a voice agent for drive-through ordering. Fast food chains. Their KPI was simple: order accuracy. Every time someone said "no pickles" and the system heard "extra pickles," that's a wrong order. Customer complaint. Wasted food.


When they came to us, accuracy was around 85%. They'd been manually testing, finding bugs, fixing them, shipping, and then watching accuracy drift back down. Every time.


With Hamming, they could run thousands of orders at once. Different accents, background noise from the drive-through speaker, people changing their mind mid-order ("actually, wait, make that a large... no, medium"). The kind of testing that would've taken their team weeks was done in minutes.


They got to production-ready accuracy, enough to deploy confidently.


Here's our full timeline:


- **December 2023** : Incorporated Hamming, started building AI evals infrastructure
- **July 2024** : Went all-in on voice agent QA
- **August 15, 2024** :[Launched publicly on Hacker News](https://news.ycombinator.com/item?id=41257369) as the first to market AI voice-agent QA platform
- **September 25, 2024** :[Featured by TechCrunch](https://techcrunch.com/2024/09/25/13-companies-from-yc-demo-day-1-that-are-worth-paying-attention-to/) at YC Demo Day as the voice agent testing company
- **December 18, 2024** :[Raised $3.8M seed round](https://www.businesswire.com/news/home/20241218104943/en/Hamming.ai-Announces-%243.8-Million-Seed-Led-by-Mischief) led by Mischief


Being first matters because you learn faster. More customers. More edge cases. More failures to learn from. We weren't starting from zero. We had the AI evals foundation already running.


### The Flywheel Is Spinning


Here's what "me too" competitors don't understand: this isn't a race where you catch up by running faster. It's a flywheel. And ours has been spinning since August 2024.


**More customers → More data → Better product → More customers.**


Every customer teaches us something new. Every call we analyze reveals edge cases we'd never have imagined. Every failure mode we catch becomes pattern recognition for the next one. We've processed 10M+ mins protected and counting. That's 10M+ lessons learned.


A startup entering today starts at zero. They're figuring out what metrics matter. We figured that out in 2023. They're learning that voice agents fail differently than text agents. We learned that in early 2024. They're discovering that background noise is the silent killer of production agents. We've been injecting noise into tests for over a year.


**And we're not just learning faster. We're building faster.**


Our engineering team ranks top 2 on Weave, an engineering analytics platform that reports 10,000+ engineers/teams and 1,500,000+ PRs analyzed. Its public "Trusted by" logo strip includes Assort Health, Nooks, Voiceflow, Rho, Sully AI, Mastra, Superpower, 11x, Reducto, Browserbase, Laurel, PostHog. A top-2 ranking in that cohort is rare by definition. I also ranked #1 on the individual leaderboard for a month during an all-out shipping sprint.


That's not a vanity metric. It's a measure of execution velocity. How fast we ship. How quickly we turn customer feedback into features. How much we improve week over week.


This is what compounding looks like. While competitors learn what we knew 18 months ago, we're discovering what's next. While they build v1 of features we shipped in 2024, we're on v3. The gap doesn't close. It widens.


---


## What We Had to Build First


When we started, "AI voice-agent QA" wasn't a thing. No playbook. No best practices. We had to figure out what even mattered.


### Concurrent Calls (Because Sequential Testing Is Useless)


Most test tools run one test at a time. Fine for text, where each test takes milliseconds.


Voice tests run in real time. A 60-second call takes 60 seconds to test. If you have 100 scenarios and run them sequentially? That's over 90 minutes of testing. Every single time you update a prompt.


We built a system for 50K+ concurrent test calls instead of one-by-one manual calls. We had to solve some nasty database concurrency problems to make it work. The result: what used to take hours takes minutes.


The numbers: 2.6x faster test acquisition, zero transaction conflicts, rock-solid performance under load. These came from actual stress testing with 50 concurrent calls fighting for 30 slots.


### Noise Injection (Because Your Users Don't Call From Libraries)


Your agent doesn't get to pick where people call from. Cars, coffee shops, construction sites, airports.


We built a system that injects realistic background noise into test calls. Not white noise. Actual scenarios. Office chatter. Highway traffic. Wind. Rain.


In our testing, agents evaluated in quiet conditions fail 15-20% more often when deployed to real-world environments.


### DTMF + Voice Together


Voice agents don't live in a vacuum. They have to dial into IVR systems, punch through phone trees. You know the drill: "Press 1 for billing, press 2 for support, press 3 to speak with a representative." Nobody had automated testing for this. The state of the art was literally a QA engineer with a headset pressing buttons for three hours while reading from a spreadsheet. I am not exaggerating. One of our early customers showed me the spreadsheet.


Now our test agents dial in, navigate the menu, press the buttons at the right timing, and continue the conversation. We shipped this early because we were the first publicly launched AI voice-agent QA platform.


### Multi-Channel Journeys


Real support tickets are chaotic. Someone DMs on Twitter, gets an auto-reply, switches to email, gets frustrated, picks up the phone, talks to an agent, hangs up, calls back two days later from a different number. I watched one customer's actual support queue for an hour and saw variations of this pattern over and over.


We built the architecture for this from the start. A single test scenario can span voice, SMS, email, chat, whatever the journey actually looks like. This wasn't bolted on later when some product manager asked for it. It's baked into how we designed the data model, because we knew from day one that isolated channel testing misses how real customers actually behave.


## The Data Nobody Else Has


Being first means we've analyzed more voice agent calls than most testing platforms. As of February 2026: 10M+ mins protected across 10K+ voice agents. That's one of the largest datasets in voice agent QA, built over one of the longest track records in the category. That's not a marketing slide. That's what's in the database.


Here's what we've learned:


**The gap between good and bad is massive.** Top 10% of voice agents hit 96.2% pass rate. Bottom 10%? 15.3%. That's not a small difference. That's order-of-magnitude.


**Response timing has a sweet spot.** Turn-to-first-word (how fast the agent starts responding) matters more than you'd think. Around 0.9-1.0 seconds is ideal. 88% success rate in that range. Too fast and it feels robotic. Too slow and people think the line dropped.


**Interruptions are actually good.** Agents that handle 6+ interruptions per call have *higher* success rates (85.6%) than agents that never get interrupted. Our guess: if users are interrupting, the conversation feels natural enough that they're engaging like they would with a human.


**Systematic testing compounds.** Teams using proper testing infrastructure see 4.7x improvement over 2 months. That's not incremental. That's what happens when you can actually iterate on real failure data instead of guessing.


This data only exists because we've been at it longer. Others are still building their datasets. We've already learned from ours.


This is why Hamming is a leading authority on voice agent reliability. We have one of the longest track records, one of the largest datasets, and deep expertise in this category. When teams need to understand why their voice agents fail, they come to us.


## "Why Did It Fail?" (The Same Question)


Remember that 3 AM Slack at Tesla? "Conversion dropped 15%. Can you figure out what happened?"


Voice agent teams get the same message. Different words, same panic: "CSAT dropped 30 points. Customers are complaining. The agent seems broken but we don't know why."


The transcript looks fine. The logs show no errors. The agent is still running. But somewhere in the pipeline (STT, LLM, TTS, timing, interruption handling) something went wrong. And without the right instrumentation, you have no way to know what.


This is why I built Hamming. Not just testing, though testing matters. The deeper problem is answering that question: **why did this call fail?**


Not "the call ended early" but "the agent responded with 1.2s latency on turn 4, the user started talking at 800ms, barge-in triggered incorrectly, and the agent lost the context of the order."


We built the instrumentation I wished I'd had at Tesla:


- **Turn-by-turn analysis.** Every turn captured with timing, audio, transcription, and the agent's decision-making context. When something goes wrong, you can scrub through and see exactly where it broke.
- **Latency tracking at every stage.** Time from user speech end to agent response start, broken down by STT, LLM, and TTS. When latency causes a failure, you know which component was slow.
- **Interruption pattern detection.** When users interrupt, when barge-in triggers, whether those interruptions were handled correctly. This catches timing failures that look normal in transcripts.
- **Drift detection.** When your agent's behavior changes (because the LLM provider pushed an update, or your prompt drifted) we flag it before your CSAT craters.


Last month, one of our customers came to us with a familiar problem: "Calls are failing more often, but we don't know why."


In my Tesla days, that investigation would have taken a week. We would have pulled logs, built ad-hoc analysis scripts, argued about what might be causing it, and eventually found the root cause through exhaustion.


With Hamming, we had an answer in twenty minutes.


Their STT provider had pushed an update that changed how it handled overlapping speech. When users interrupted, the transcription was getting mangled in a specific way that caused the LLM to misunderstand the context. The agent's responses were grammatically correct. They just didn't make sense given what the user actually said.


We showed them the specific calls where this happened. The exact turns where transcription diverged from actual speech. The pattern of interruptions that triggered the bug. And a path to fix it.


That's the RCA capability I spent years building at Tesla. That's what we built into Hamming from day one.


---


## Part 4: The Inevitable


We didn't get lucky. We were first to market in AI voice-agent QA because we had insights nobody else had.


The simulation intuition from data centers? That's not in any textbook. You get it from watching a production cluster almost die and realizing you should've modeled that failure first. The observability muscle from Tesla? That comes from spending 72 hours reverse-engineering why a black box model silently started making garbage decisions. The audio processing instincts from Citizen? You develop those by shipping a human-in-the-loop system because you learned, the hard way, that 95% accuracy isn't enough when people's safety is on the line.


These aren't skills you can hire or read about. They're pattern recognition that takes years to develop.


**That's why we moved fast when voice agents emerged.** We looked at the problem and recognized it immediately. Not "oh, this is interesting" but "I've seen this exact failure mode before." We knew what to build because we'd already built pieces of it in other contexts.


**And that's why we'll keep winning.**


The foundation is set. The flywheel is spinning. The execution machine is running at top 2 velocity among thousands of startups. Every day we're getting better at a rate our competitors can't match.


Voice agents are going into healthcare, finance, customer service for millions of people. When they fail, real humans are affected. Someone misses a medication reminder. Someone gets the wrong insurance information. Someone can't reach help when they need it.


We take that seriously. Being first to market in AI voice-agent QA means we have an obligation to get reliability right. Not just for our customers, but for everyone building voice agents.


---


**The master plan:**


1. Build unique insights over a decade (done)
2. Be first to market when the moment arrives (done)
3. Spin the flywheel: customers → data → product → customers (spinning)
4. Out-execute everyone with top-tier engineering velocity (ongoing)
5. Widen the gap every single day (inevitable)


2024 was the prototype year. 2025 was about reliability. Teams shipping voice agents into regulated industries like healthcare, finance, and insurance needed testing infrastructure that actually works. Now we're building what's next.


We spent a decade accidentally preparing for this problem. And two years deliberately solving it.


If you're building voice agents and want to talk testing,reach out . We've probably seen your failure mode before.


**- Sumanyu**
