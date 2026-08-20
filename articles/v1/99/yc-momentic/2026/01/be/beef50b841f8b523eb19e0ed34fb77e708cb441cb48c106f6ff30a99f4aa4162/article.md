---
schema_version: "1.0.0"
document_id: "beef50b841f8b523eb19e0ed34fb77e708cb441cb48c106f6ff30a99f4aa4162"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/vibe-testing"
published_at: "2026-01-07T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:91c0db3190d9e518a6a50581b1fbe383995e6919c354e49ac88d52bea9cedc0a"
---

# Embrace the Exponentials of Vibe Testing

At Momentic, we're all about the vibes.


So[vibe coding](https://x.com/karpathy/status/1886192184808149383) resonates. And leads to the obvious question: If you can vibe code, can you vibe test?


Absolutely. We'd argue that vibe testing has been around longer than vibe coding. Vibes are essential to robust testing. Testing *has* to include a certain amount of intuition-users aren't QA automatons, they're humans who interact with software based on feeling and instinct. They use your product on vibes; thus, you must add some vibes to your testing process.


But vibe coding is obviously taking vibes to the max. So, if we were to follow the Karpathy framework, what would vibe testing look like?


## Vibes, Exponentials, and True No-Code


Here's Karpathy's concept of vibe coding:


There are three core elements to vibe coding in that first sentence that we can translate to testing.


### 1. Give into the vibes


The vibes are calling, and in testing, they beckon you away from rigid methodologies toward organic exploration. This principle transforms testing from a mechanical checkbox exercise into an intuitive journey guided by your instincts and observations.


Instead of writing detailed test plans, you explore the application organically, following your instincts about where issues might lurk.


- You might notice something feels off about the login flow and dive deeper, or sense that a particular edge case needs examination.
- You intentionally try strange combinations of actions that a structured test plan would never include, discovering a critical bug when rapidly switching between user roles.
- You follow your gut feeling about which features new users might misunderstand, revealing serious UX issues that weren't apparent in scripted testing.


### 2. Embrace exponentials


Exponentials represent the explosive scaling potential when AI amplifies your testing capabilities beyond human limitations. This approach leverages automation to generate and execute test scenarios at a scale no manual tester could achieve, embracing quantity as a pathway to quality.


- Let AI generate dozens/hundreds/thousands of test scenarios you wouldn't have thought of. You no longer care about organization; you can create and kill these as you wish.
- Feed user stories to your LLM assistant and have it brainstorm edge cases. Use generative testing to create thousands of input combinations rather than the handful you'd manually design.
- Have your AI assistant monitor production logs in real time, identifying unusual patterns and automatically generating new test cases based on actual user behavior.


### 3. Forget the code exists


This principle invites you to liberate yourself from implementation details and experience the product with fresh, user-focused eyes. By maintaining deliberate ignorance about how the system works internally, you position yourself to discover the same surprises and frustrations your users might encounter.


Don't get bogged down in how features were implemented-focus on whether they work for users. Test the application as if you had no idea how it was built, because that's how your users will experience it.


- Test the checkout flow by deliberately making unusual payment selections and discovering a bug where discount codes aren't correctly applied to bundled items.
- Test the search functionality by entering queries as a non-technical user would, with typos, natural language, and unexpected formatting, revealing gaps in the search engine's robustness.


## The Strong and Weak Version of Vibe Testing


There are two ways to look at vibe testing.


The first is the strong version. Strong vibe testing is testing at its most chaotic and intuitive extreme-the pure embodiment of Karpathy's original vision. You completely abandon test plans, documentation, and methodologies. You simply open the application and follow whatever draws your attention, with zero structure or methodology.


When you encounter issues, you copy the error message directly into an LLM and implement whatever solution it suggests without questioning the underlying causes. You don't maintain test cases, so why bother when AI can regenerate scenarios on demand? You don't track bugs systematically because your AI assistant will remember them (it won't).


Testing becomes a continuous stream of consciousness: click here, try that, ask AI, implement fix, repeat. You might have the AI generate hundreds of test variations and run them without reviewing them first. When stakeholders ask about test coverage, you vaguely gesture toward the AI and assure them, "the vibes are solid." The product ships when it feels right, not when specific criteria are met.


Strong vibes might look like this:


- A startup gives their new app to an AI-equipped tester who opens it without reading the requirements, clicks randomly through the UI while asking ChatGPT to "find bugs in this kind of app," and pastes screenshots of anything strange-looking.
- When they encounter an error message, they copy-paste it directly to their AI assistant and implement the suggested fix without reviewing the code or understanding the issue.
- They run 500 AI-generated test cases overnight without reviewing them first, then declare the app "feels solid" based on the 72% pass rate.
- During a demo, they discovered a critical bug in a core feature but assured stakeholders that "the AI didn't flag it as high priority" before launching anyway.
- Their test documentation consists entirely of AI-generated summaries of testing sessions, with no reproducible steps or verification methodology.


But there is also the weak version-perhaps better called the practical version-that integrates vibes into a structured framework. You start with traditional test planning but leave space for intuitive exploration. Your test documentation exists but is augmented by AI rather than replaced by it.


You still create formal test cases for critical paths, but use AI to expand them with edge cases you might have missed. When exploring the application, you document insights from your intuitive sessions and incorporate them into your test suite. Error messages are analyzed with AI assistance, but you validate the suggestions against your understanding of the system.


The AI becomes an intelligent collaborator rather than a replacement for your judgment. It helps you scale your testing through intelligent generation of test scenarios, priority suggestions based on risk analysis, and automated test maintenance. But you remain the curator of the test strategy, incorporating vibes as an enhancement rather than surrendering to them completely.


This might be a weak vibe workflow:


- A testing team creates a core test plan covering critical user journeys, then uses AI to expand this with possible edge cases and automated tests for repetitive scenarios.
- Testers conduct exploratory sessions, following their intuition through the app but documenting meaningful bugs they find in a structured tracking system.
- They use AI to analyze unusual patterns in production telemetry, generating targeted test cases for areas where users are experiencing unexpected behavior.
- When encountering errors, they use AI to help analyze root causes but validate all fixes against their understanding of the system architecture.
- They let non-technical team members use the product and observe their natural interactions, incorporating these insights into formal test cases that preserve the "vibes" while adding structure.


This balanced approach recognizes that both structure and intuition have their place, using AI to bridge the gap between methodical coverage and the unpredictable ways users will interact with your product in the real world.


## Grown-Up Vibes: Testing With Tabs


There is already a backlash against vibe coding. It's seen, perhaps reasonably, as an option only for the non-serious, either in terms of coder or product. You can't build robustly on vibes alone.


Instead, the AI answer for serious programming is[Cursor tab coding](https://x.com/ThePrimeagen/status/1903638369491435633) :


[Cursor tab](https://www.cursor.com/blog/tab-update) allows coders to stay in control of what the AI model is doing, but still lets the AI model do the work. It revolves around these concepts:


- The developer writes initial code, then the AI highlights logical next steps that can be accepted with a simple tab press
- The AI maintains awareness of the entire codebase and can predict necessary changes across multiple files
- It handles "zero-entropy" edits-predictable, mechanical tasks that follow naturally from the developer's intent
- The developer focuses on architecture and design decisions while the AI handles implementation details


Unlike vibe coding, tab coding provides full visibility and understanding of the generated code. A version of this for testing would be ideal: guided, intentional automation while keeping the developer in control. Tab testing would likely look something like this:


1. **Predictive test generation** : As you write code, the AI highlights areas that need testing and suggests appropriate test cases when you press tab. For example, after you implement a function, pressing tab might generate a basic unit test for it.
2. **Zero-entropy test actions** : The system would identify predictable test patterns based on your codebase and conventions, automatically implementing them while giving you control over acceptance.
3. **Cross-component test coordination** : The AI would understand your entire dependency graph and suggest tests that cover integration points when you modify code that affects multiple components.
4. **Incremental test evolution** : Instead of generating a complete test suite upfront (vibe testing), tab testing would build tests incrementally alongside your code, maintaining quality while keeping you engaged in the process.
5. **Test-fix cycles** : When tests fail, the AI might highlight the issue and suggest a fix when you press tab, but you maintain control over implementing the solution.


Unlike "vibe testing," where you might just let AI generate a bunch of tests without understanding them, tab testing would keep you in the loop, preserving your agency while eliminating the tedious parts of writing test boilerplate. Tab testing, like tab coding, would be collaborative and intentional rather than surrendering control completely to the AI's "vibes." You'd maintain understanding of your test coverage while accelerating the testing process itself.


## The Vibeaissance


Should you be going fully with the vibes when testing? Probably not. But these extreme ideas always contain a kernel of truth. In testing, the balance has shifted too much towards frameworks, structure, and rigidity.


Even as just a thought experiment, vibe testing throws up some interesting ideas-embracing intuitive exploration, leveraging AI to generate test scenarios at extreme scale, and approaching software with fresh eyes, unbiased by implementation knowledge. Doing this, alongside strong frameworks and testing knowledge, creates more comprehensive, user-focused test strategies that catch the bugs that matter most.


The future of testing isn't abandoning structure entirely but finding the sweet spot where human vibes and AI speed amplify each other for better-quality code.


## FAQs


1. **What is vibe testing?**
Vibe testing is an intuition-led testing approach that prioritizes organic exploration of the product the way real users behave.
2. **How is vibe testing different from traditional QA?**
Traditional QA follows structured test plans; vibe testing encourages following instincts, trying odd user behaviors, and exploring “what feels off.”
3. **How does AI amplify vibe testing?**
AI can generate large volumes of edge cases and scenarios, helping you test far beyond what a human would manually design.
4. **What’s the risk of “strong” vibe testing?**
It can become chaotic and non-reproducible, with weak coverage tracking, unreliable bug reporting, and over-trusting AI outputs.
5. **What’s the practical way to apply vibe testing in real teams?**
Keep structured tests for critical flows, then use vibe-driven exploratory sessions plus AI-generated scenarios to expand coverage and capture user-centric issues.
