---
schema_version: "1.0.0"
document_id: "c569e8bdf5aedf5688fffda34cb4b7e6a8b671a484ce796e1cbd0ca6a8a7c970"
company_key: "yc-clearly-ai"
company: "Clearly AI"
source_id: "yc-clearly-ai-news-import-066e8c806377"
canonical_url: "https://clearly-ai.com/blog/why-we-started-clearly-ai"
published_at: "2026-02-03T00:00:00+00:00"
first_seen_at: "2026-07-23T05:49:00.156409+00:00"
fetched_at: "2026-07-28T21:26:59.511520+00:00"
content_hash: "sha256:0d0443565a7d8900368aa1312bf8c063cabf044af36c5ecb5ba3ce14686bb995"
---

# Why we started Clearly AI

##### **I have a confession: I started Clearly AI because I had a love/hate relationship with my career.**


I loved the problem solving of working as a product security engineer, and I loved the goal: make sure all products are secure upon launch, and catch security problems before they hit production.


But I hated what the job actually ended up looking like day-to-day. I couldn't imagine spending the rest of my career answering basic questions from engineers and fighting with engineering leaders about the impact of a major security flaw. Especially when involving the security team from Day 1 would have turned that major issue into a minor fix.


After living this day in and day out, I started to see a pattern. The boundaries between security teams and engineering teams are where things fall apart. As both a software engineer and security engineer, I knew this was a problem I had to fix.


## The information gap


The manual back-and-forth work of a security engineer starts with an asymmetry of information. Security engineers know a lot about security. Software engineers know a lot about the system they built. Before a security engineer can assess whether something is secure, they have to figure out what it *is* .


That often means parachuting into unfamiliar corners of the company, chasing down documentation, and sitting through meetings where even the developers disagree on how their own system works. Reviews quickly turn into coordination overhead: setting up calls, pulling in the right people, reconciling conflicting explanations just to get to a shared understanding. One customer told us they used to ask developers 325 questions just to understand what they're building.


The inconsistency runs deeper than just the information gap. Security expertise is often split across domains like identity, encryption, and cloud infrastructure. No single security engineer can confidently cover everything. (If you don’t believe me, go to DEFCON. There is so much variation in the security domain!)


From a developer's perspective, that meant the quality and depth of a review could vary depending on who they were assigned. Not because anyone was less capable, but because each of us brought different expertise and different context. Developers would get frustrated - why are you calling out this risk when no one else has surfaced it previously?


## Existing approaches don’t help


For the past decade, security tooling has tried to solve the problem of “are our products and applications secure?” with proxy metrics. SAST, SCA, DAST, secret scanners, ASPMs, runtime scanners. Different names, different roles, all attempts to get some glimpse in through one window. Runtime tools tell you what's happening but not why. Code scanners find patterns but miss intent. Design-time tools give advice but nobody ensures it's followed through in implementation or production.


None of these tools solve the actual problem. They each require knowledgeable security professionals to operate, interpret, and act on the results, and they often bring more noise than signal. The real work of product security, understanding a system end-to-end and making sure it's built securely, still falls entirely on people.


Before LLMs, proxy metrics were the best we could do. Now, for the first time, it's possible to ingest the full context of a system (design docs, code, configuration, organizational knowledge) and reason over it the way a security engineer would. That changes everything.


Security doesn't break down because of bad tools or bad engineers. It breaks down when the right information doesn't reach the right people at the right time. Clearly AI was built to fix that. Not to give security teams another dashboard to monitor. To actually do the work.


Our mission is to solve the whole thing: ensure that every product you build and launch is secure. We actually perform your technical controls analysis, highlight where controls are missing, and tell developers exactly how to remediate it. We go beyond surfacing risk. We focus on the "so what," with the visibility and control to let your team decide how much to automate and when to stay hands-on.


Point Clearly AI at design docs, code repositories, and the sources that matter, and we synthesize that information into threat models, data flow diagrams, and security assessments. Depending on how your team configures it, Clearly AI can triage and surface what needs attention or provide full-depth analysis on any system in your environment.


This works across the full lifecycle. Not just at design time, but through implementation and into production. Advice is meaningless if nobody follows up on it. We care about every part of the SDLC because that's what it takes to actually keep products secure.


When your technical controls are consistently evaluated and your baseline security expectations are met automatically, security engineers can focus on the work that actually requires human judgment: understanding system-level risk, evaluating how components interact, making business risk reduction tradeoffs, and reasoning about complex attack paths.


## Clearly AI is built by security engineers for security engineers


We built Clearly AI because I lived these challenges as a security engineer. The hardest parts of the job were never finding vulnerabilities. They were navigating broken communication and inconsistent reviews to find out who was responsible for fixing them, and whether they were even "compromisable" in the first place. A scanner flags a high CVE score, but does it actually mean customer data is at risk? If you can't tell the whole story, you're just handing developers a number and hoping they figure it out. And when they push back, it turns into an escalation game that wastes everyone's time.


Clearly AI creates the conditions where expertise gets applied where it matters most, bringing clarity, consistency, and shared understanding to security reviews so teams can stop escalating over numbers that lack context and start doing the work that actually requires their judgment.


If this reflects your experience, we built Clearly AI for you. Jump on a call with me to learn more!
