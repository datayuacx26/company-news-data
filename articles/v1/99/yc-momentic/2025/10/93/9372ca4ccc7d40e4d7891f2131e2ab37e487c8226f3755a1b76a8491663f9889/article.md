---
schema_version: "1.0.0"
document_id: "9372ca4ccc7d40e4d7891f2131e2ab37e487c8226f3755a1b76a8491663f9889"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/swes-are-the-new-qa"
published_at: "2025-10-07T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-07-23T16:54:58.821228+00:00"
content_hash: "sha256:7cf7ea19b259c25363c0eae2ff9d92ce8725881ccfdd3f5e16292adaf654b7f1"
---

# SWEs Are the New QA (And They Will Hate It Too)

A couple of weeks ago, Sahil Lavingia, the[doyen of Doge](https://sahillavingia.com/doge) and CEO of Gumroad, posted:


Cute, but wrong. It’s backwards. QA isn’t the new SWE; SWEs are the new QA. And engineers are about to find out that owning quality is every bit as frustrating, political, and high‑stakes as the feature work they cherish.


## Why QA isn’t the new SWE


The point Lavingia is making is an important one: there is no longer any delineation between engineering and testing. In an AI-first environment, what does it mean to code? It means to generate code with AI. What does it mean to test that code? It means to generate tests with AI.


The idea of keeping two separate teams for these two functions is ridiculous. Thus, Lavingia says, *QA is the new SWE* .


What’s the steelman case for QA to become SWE?


- AI code generation removes the syntax barrier. QA engineers with deep domain knowledge can now write production code, not just test it.
- QA professionals think in edge cases and failure modes by default. This mindset yields more robust code than that of developers who treat testing as an afterthought.
- They already own the test infrastructure. In many orgs, QA wrote the test harnesses, simulators, and automation frameworks that keep systems running.
- Domain expertise now matters more than coding fluency. A QA who has spent five years in payments understands payment flows better than a developer who has never worked in fintech.
- In regulated industries, QA already maps features to compliance requirements. Having them implement those features maintains the audit trail.
- Cross-training existing QA staff is more cost-effective than hiring new developers, especially since they already understand your systems.
- The market is already moving in this direction with hybrid roles like "Software Engineer in Test" becoming standard.


I don’t know if Lavingia had thought through all these points, but there is definitely proof in his pudding. So, why do I disagree?


## Why SWE now owns QA


The problem with "QA is the new SWE" is that it misses the shift happening. We're not elevating QA to engineering status; we're distributing QA responsibilities across the entire engineering org.


Think about what's changing. AI doesn't just help QA write code; it allows developers write tests at crazy speed. The bottleneck was never technical capability. It was ownership.


When you separate building from testing, you create a moral hazard.


Phase Who “owns” it Typical outcome


Code Devs (optimize for speed) Corner‑cutting, fragile assumptions


Test QA (optimize for coverage) Late discovery, frantic bug triage


Deploy Ops (optimize for uptime) Friday‑night rollbacks


Developers optimize for feature velocity because someone else will catch their mistakes. QA optimizes for finding bugs because that's how they prove value. Neither optimizes for shipping quality code.


This is role absorption. Modern engineering teams are eliminating the handoff entirely. You write the feature, you write the tests, you own the quality. No more throwing code over the wall.


The case for developers owning quality:


- **Shift-left ownership.** The cheapest place to find a defect is in the same commit where it was introduced. When developers test their own code, bugs never leave their local environment.
- **Tool-chain parity.** Modern IDEs generate test scaffolds as naturally as they autocomplete code. The person with full context (the developer) can refine those tests in seconds, not hours.
- **On-call accountability.** The engineer who gets paged at 2 a.m. has every incentive to write reliable, self-validating code. Personal pain drives quality better than any process.
- **End-to-end flow efficiency.** Eliminating the dev→QA→ops relay cuts cycle time from weeks to hours. No more context switches, no more finger-pointing, just continuous flow.
- **Career reality.** Engineering promotion rubrics now measure both velocity and stability. Writing good tests is how you level up.
- **Strategic QA evolution.** As developers handle routine testing, QA specialists move upmarket into risk modeling, security testing, and exploratory scenarios. Quality expertise scales with complexity, rather than being stuck in repetitive gatekeeping.


When the same person who architects a system also designs its test strategy, quality becomes a positive constraint from day one.


So yes, QA professionals can become developers. However, the bigger shift is that developers are becoming responsible for quality.


## Why SWEs will hate wearing the QA hat


Engineers joined this field to build things, not to write assertions about things they already built. There's a fundamental identity clash here: we see ourselves as creators, not validators. Testing feels like janitorial work compared to architecting new systems. AI can now generate tests, but you still need to review, debug, and own them.


The fear runs deeper than ego. In most organizations, feature count remains the primary yardstick on roadmaps. Spending half your sprint on test coverage looks like reduced output, even when it prevents future fires. Your manager says quality matters, but the promotion committee counts shipped features. AI might write your tests 10x faster, but you're still the one explaining why you "only" shipped three features this quarter.


Then there's the skills gap nobody talks about. Even with AI assistance, writing deterministic, maintainable tests is its own discipline:


- Mocking complex dependencies without creating brittle tests
- Debugging flaky tests that pass locally but fail in CI
- Designing test data that catches edge cases without bloating the suite
- Knowing when to test implementation vs behavior


The accountability shift hits even harder. When you own quality, you own every production incident. The pager becomes your personal shame bell. That 2 a.m. alert is explicitly your failure. Ship ten features flawlessly, and nobody notices. Miss one edge case, and everyone knows your name. AI might have written the test that missed the bug, but guess whose name is on the commit?


Perhaps worst of all is the maintenance drag. Tests aren't write-once artifacts. They break with every refactor, every dependency update, every requirement change. It's code that never ships anything new but constantly needs fixing. While your peers are building the next big feature, you're updating assertions because someone renamed a CSS class. AI can help regenerate tests, but you're still the one who has to figure out which failures are real bugs versus which are just outdated assumptions. You're still the one running the test suite locally, waiting for it to finish, investigating the failures.


## Why an AI Agent Should Own QA


Here's the escape hatch that makes this transition bearable: moving from AI-assisted testing to agentic testing. The difference matters.


Today's AI coding tools are test *assistants* . They generate tests faster, suggest edge cases, or even update simple assertions. But developers still review every test, debug every failure, and own every outcome.


True AI testing agents operate differently:


- Run exhaustive test suites 24/7 without human oversight
- Explore input permutations no human would enumerate
- Auto-fix broken tests by understanding intent, not just syntax
- Spin up environments, reproduce bugs, and verify fixes independently


We want to remove humans from the testing loop entirely. You define the quality strategy: what needs testing, what risks matter, and what user journeys are critical. AI agents handle everything else.


Instead of reviewing AI-generated tests, you're setting quality objectives. Instead of debugging why the AI's test is flaky, you're reviewing coverage reports that matter to the business. Instead of updating assertions after a refactor, the agent understands your refactor and adapts automatically. *You can’t hate a job you don’t have.*


We're reaching that point. Tools like Momentic represent the shift from AI-assisted to AI-autonomous testing. Independent quality agents who own the[full testing lifecycle](https://momentic.ai/blog/the-software-testing-life-cycle) .


Developers no longer have to own the tests. The AI agent does. You define quality objectives and success criteria. The agent handles test creation, execution, maintenance, and evolution. When tests break, the agent fixes them. When requirements change, the agent adapts.


SWEs become QAs, but at a much higher level. In reality, AI becomes QA, and (as yet) they have no propensity for job dissatisfaction.
