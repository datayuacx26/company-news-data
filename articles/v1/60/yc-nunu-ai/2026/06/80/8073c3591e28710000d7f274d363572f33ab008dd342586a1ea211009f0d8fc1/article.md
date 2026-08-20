---
schema_version: "1.0.0"
document_id: "8073c3591e28710000d7f274d363572f33ab008dd342586a1ea211009f0d8fc1"
company_key: "yc-nunu-ai"
company: "nunu.ai"
source_id: "yc-nunu-ai-news-import-9ab4e6dc8961"
canonical_url: "https://nunu.ai/blog/how-nunu-works"
published_at: "2026-06-11T12:18:00+00:00"
first_seen_at: "2026-07-24T11:21:30.712503+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:ef7611b6faa7bab572703547a81af739af9dd4f686303909a2f59352d9e19a15"
---

# How to Test Your Game or App with AI Agents: A Complete Guide to Automated QA

## **How to Test Your Game or App with AI Agents: A Complete Guide to Automated QA**


**AI QA testing with nunu works like this: you describe a test in plain English ("complete the tutorial," "complete the signup flow"), and an AI agent executes it on a real device by looking at the screen and interacting like a human user. No test scripts, no engine integration, no SDK required to start.**


This guide walks you through running your first AI-powered test with nunu.ai's web portal for automated game and app testing, and shows you how to write tests that actually catch bugs.


#### **TL;DR**


- Upload your build (APK, IPA, or ZIP) to nunu.ai
- Type what to test in plain English
- Hit Play, review the recording


First test running in ~5 minutes. The rest of this post is how to do it well.


### **What is AI QA testing?**


Think of nunu as ChatGPT for your game or app. You tell it what to test in natural language, and you can be as vague or as specific as you like. Behind the scenes, AI agents interact with your software on real devices and perform the testing automatically.


Four things make this different from traditional test automation:


- **Vision-based.** The agent sees rendered frames, exactly like a user would. It doesn't need access to your code or engine.
- **Adaptive.** When your UI changes, the agent adapts. It's not matching pixels or replaying recorded coordinates, it's reasoning about what's on screen. Traditional automation scripts break on every UI change; **AI agents don't.**
- **No integration required.** The agent operates a real device out of the box. An optional lightweight SDK adds deeper hooks like error logs and application state.
- **Cross-platform.** Works on mobile (iOS and Android) and Windows PC software, whether that's a game, a consumer app, or internal tooling.


### How do I set up my first AI test?


Getting your first test running with nunu takes only minutes:


**1. Upload your build** (APK, IPA, or ZIP) to Build Storage. Mobile builds run on real devices in nunu's infrastructure. For PC, point the agent at your Windows build.


**2. Create a test** in Tests → Repository. Start with a Discovery test (explained below).


**3. Hit Play** , choose your deployment config and build, and confirm.


**4. Watch the agent live** or come back later.


**5. Review the run** in History → Runs, including the full screen recording.


No integration work is needed for your first test. If you later want deeper access, the[SDK](https://docs.nunu.ai/home/next-steps) is optional. Roboto Games integrated it in under five hours of dev time and now runs 400+ automated tests on their survival game Stormforge, reclaiming around 160 QA hours per month.


### The Three Types of Tests


Nunu has three agent types, and choosing the right one matters:


**Discovery -** Exploratory testing. Give the agent a scope and let it loose.


**Verification -** Regression and smoke testing with structured steps.


**Task -** Automation beyond testing: docs, research, workflows.


**If you're new, start with a[Discovery test](https://docs.nunu.ai/writing-tests/types/discovery) .** Minimal setup: define a scope like "explore the main menu and settings," hit run, and the agent investigates on its own, flagging anything broken along the way.


Once you know your critical flows, **[Verification tests](https://docs.nunu.ai/writing-tests/types/verification)** turn them into a repeatable regression suite that runs on every build.


### **How do I write a good AI test case?**


A test case is a natural language description of what the agent should do and what success looks like. Think of it as instructions you'd give a new QA tester on their first day, with one key difference: **the agent has no assumptions** . Anything obvious to you must be stated explicitly.


A weak test case:


Test the shop.


A strong test case:


Open the shop from the main menu. Purchase the cheapest item using soft currency. Verify the item appears in the inventory and the currency balance decreased by the item price. If a confirmation popup appears, accept it. The test FAILS if the purchase does not complete or the inventory does not update.


The same logic applies to any app. "Test login" is weak. "Log in with the test account credentials, verify the dashboard loads, and FAIL if an error message appears or loading takes longer than 30 seconds" is strong.


Four rules from nunu's[best practices guide](https://docs.nunu.ai/writing-tests/best-practices) that consistently produce better tests:


- **Be precise.** Define clear endpoints: "Reach level 3," not "play for a while." "Complete checkout," not "try the shopping flow."
- **Iterate.** Write → run → observe → refine. Treat test cases like living documents.
- **Use strong language.** MUST, NEVER, ALWAYS. Use caps for critical steps so the agent knows what's non-negotiable.
- **Handle variations.** Real software is messy. If a popup can appear, tell the agent what to do: "If a Terms of Service popup appears, tap I Agree."


Writing a test down takes two minutes, and then it can be reused forever.


### What results do AI agents give you?


Every run produces a full evidence package, which is what makes bugs reproducible:


- A **screen recording** of the entire run
- The **agent's step-by-step reasoning** , so you can see why it did what it did
- The **exact inputs** performed (touch, keyboard, mouse)
- **Error logs** (with the SDK connected)
- A **test report** with the verdict
- Even **full PDF reports or digests** (with screenshots) if prompted


Instead of "checkout is broken sometimes," your developers get a video, the exact action sequence, and the agent's observation of what went wrong.


## How do I scale AI testing across my pipeline?


Once your first tests pass, here's where to go next:


- [Scheduled Tests](https://docs.nunu.ai/home/next-steps) : set up triggers to run your suite overnight automatically
- **CI/CD Integration** : auto-test on every new build, so regressions get caught in hours instead of days
- **Knowledge Base** : give your agent context about your game or app so it tests smarter
- **Bug Review & Tracking** : reduce noise and false positives over time
- **SDK Integration** : expose debug functions and application state for advanced testing


## Will AI testing replace my QA team?


No, and that's not the point. AI agents remove the repetitive 80% of QA work so your team can focus on what actually needs humans: exploratory testing, UX judgment, game feel and balance, and edge cases that require creativity.


## FAQ


**Do I need to integrate an SDK to use AI testing?** No. nunu agents work fully black-box by operating real devices through vision and simulated input. The SDK is optional and adds deeper hooks like error logs and application state access.


**Does this only work for games?** No. Nunu tests any mobile or Windows software: games, consumer apps, and internal tools. The agents interact through the screen, so anything a human can operate, they can test.


**What platforms does nunu.ai support?** iOS, Android, and Windows PC, with console support on the roadmap. Mobile builds run on real devices in nunu's infrastructure.


**Do the tests break when my UI changes?** No. Unlike scripted automation, the agents reason about what's on screen rather than matching pixels or replaying coordinates, so they adapt to UI changes automatically.


**How long does setup take?** Your first test can run within minutes of uploading a build. Full SDK integration, if you want it, has taken teams under five hours of dev time.


## Get Started


Create an account at[nunu.ai](https://nunu.ai/) , upload a build, and run your first Discovery test today. Full guides at[docs.nunu.ai/home/quickstart](https://docs.nunu.ai/home/quickstart) .


[<< back to blog](https://nunu.ai/blog)
