---
schema_version: "1.0.0"
document_id: "c70a7e45a1dee5ca2f3a425d73921f736bef48f26387c06d87a8ebc8c1b6e942"
company_key: "yc-karate-labs"
company: "Karate Labs"
source_id: "yc-karate-labs-news-import-eb3f9bfe6418"
canonical_url: "https://karatelabs.io/blog/test-pyramid-widening"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-08-01T00:21:22.043213+00:00"
fetched_at: "2026-08-01T00:21:23.410549+00:00"
content_hash: "sha256:9138e48fc778339871662e44431bbf50a91376a33f87c7ddbd522defcb2da2eb"
---

# The Test Pyramid Isn’t Inverting. It’s Widening.

Some news first: I’m joining the Karate Labs team as UI Product Strategy & AI Advisor.


For those who don’t know it,[Karate](https://karatelabs.io/) is one of the open-source test automation frameworks I respect most: API and UI testing in a single tool, executable Gherkin, clean design, and a large, mature community built over years by its creator, Peter Thomas.


This didn’t start with a job offer. It started in 2023, when I published[a hands-on comparison of Selenium and Karate](https://lab.scub.net/selenium-versus-karate-a-concrete-comparative-approach-2699ad6195b8) , a concrete one, code and CI/CD included, because the theoretical comparisons were already legion. The Karate team read it; Peter shared it; conversations with Peter and co-founder Kapil Bakshi followed, and we ended up running a webinar together. The exchange never really stopped. Three years later, it turns into a role.


The fair question remains: why would a CTO who spends his days on production AI systems invest part of his time in a testing framework, now?


The short answer: because I believe end-to-end testing is quietly changing economic status, and I want to be where that shift is actually being built, not just commented on.


The long answer is this article. And because extraordinary claims deserve evidence, I’ll back each step of the argument with recent research rather than vibes.


## Full disclosure, before anything else


I’m now part of the team. That means I’m no longer a neutral observer when I write about Karate, Karate Agent, or AI in testing. I’d rather say it myself, plainly, than have someone point it out in the comments.


What doesn’t change: I’ll keep writing the way I always have. Measured claims, numbers when I have them, and explicit limits. If anything, being on the inside raises the bar. **An insider who overhypes is worse than a marketer who does.**


## The thesis: the pyramid widens, it doesn’t invert


You’ve probably seen the takes claiming AI “inverts the test pyramid”: fewer unit tests, everything end-to-end. I think that’s wrong, and conveniently wrong for people selling E2E tooling. In fairness, that group now includes my own team, and the more moderate thesis I’m about to defend serves them too. Which is exactly why every step below leans on independent research: judge the evidence, not the messenger.


Swipe the diagram to see both states →


AI lowers the cost of authoring and maintaining tests at every layer. Because the top was the most cost-constrained, it gains the most, without the base losing its reason to exist.


The pyramid, from Mike Cohn’s original formulation to[the canonical write-up on martinfowler.com](https://martinfowler.com/articles/practical-test-pyramid.html) , was never narrow at the top because E2E tests were hard to write. It was narrow because they are slow to execute, expensive in CI infrastructure, and coarse in their feedback. And here I have to be precise, because I don’t buy the lazy version of my own argument. AI is starting to move some of these constraints, and I say that as someone now helping build agents designed to do exactly that. Feedback is becoming less coarse on two fronts, and I can say both are workstreams we take seriously at Karate. A model can localize what diverged and why, instead of leaving you with a red cross and a stack trace. And self-healing absorbs the noise of UI drift, the broken locators and cosmetic changes, so the failures that survive are far more likely to be real behavioral divergences. When the red you see actually means something, the feedback stops being coarse. Suite-level economics shift too, when models help select which scenarios to run and triage flakiness. But the hard core remains: a real browser journey against a real system takes the time it takes, CI still bills by the browser-minute, and putting a model in the runtime adds an inference cost line that didn’t exist before. Some criteria move; not enough to invert the pyramid. Unit tests still give you fast, isolated feedback that nothing else provides.


The cost of executing an E2E test barely moves,
but the value of having it explodes


.


AI changes the ROI equation, from both ends at once.


Numerator


AI-assisted development means dramatically more code shipped, and the data here is unambiguous. GitClear’s longitudinal research, with[211 million changed lines analyzed for the 2025 report](https://www.gitclear.com/ai_assistant_code_quality_2025_research) and[623 million changes through 2026](https://www.gitclear.com/the_ai_code_quality_maintainability_gap) , documents record code output alongside deteriorating structural signals: block duplication up 81% versus 2023, refactoring moves down 70%, and 2024 as the first year on record where copy/paste exceeded refactored code. More code shipped, and code that is structurally worse on average, means more behavior that needs validating at the level where it actually matters: the user’s journey through the system.


+81%


Block duplication vs 2023


−70%


Refactoring moves


834M


Changed lines & changes analysed across both GitClear reports


~5,000


Professionals surveyed in DORA 2025


Google’s[DORA 2025 report](https://dora.dev/dora-report-2025/) , based on nearly 5,000 technology professionals, completes the picture: AI adoption now correlates positively with delivery throughput and negatively with delivery stability. Their own framing:[“AI accelerates software development, but that acceleration can expose weaknesses downstream.”](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report) The control systems DORA names as the remedy? Strong automated testing and fast feedback loops. I couldn’t ask for a better summary of why the top of the pyramid matters more now, not less.


Denominator


Writing and maintaining E2E tests was always the painful part: brittle selectors, endless glue code, suites that rot faster than they’re written. This is precisely where LLM-based approaches are advancing fastest. A[2026 peer-reviewed synthesis of 55 studies](https://peerj.com/articles/cs-3695/) (January 2023 to July 2025) on LLMs in GUI testing documents the shift from rigid script generation toward intent-driven exploration and self-healing maintenance. It also notes that effective agents have converged on combining visual screenshots with DOM structure to ground intent into precise actions. That convergence, incidentally, is exactly the architectural bet behind approaches like[Karate Agent](https://karatelabs.io/agent) .


And the two levers compound. AI-assisted authoring turns readable intent into executable scenarios; agentic self-healing keeps them alive as the UI evolves. A suite that is continuously repaired instead of slowly rotting stays trustworthy, and a trustworthy suite is one teams actually run and act on. Reliability is not a nice-to-have here; it is a third multiplier on the ROI, after authoring cost and maintenance cost.


More value to validate, at a fraction of the historical cost. E2E scenarios that were prohibitively expensive to justify become trivial to justify. The top of the pyramid, historically starved, densifies. So do the other layers:[AI makes unit and integration tests cheaper too](https://karatelabs.io/ai-test-automation) . The whole pyramid grows. But the relative change is largest at the top, because that’s where the cost was most prohibitive.


## The objection you should raise


If you’ve been reading carefully, you should now object: “You just cited evidence that AI degrades application code. Why on earth would AI-written tests be any better?”


Fair. Two answers.


Answer one


### A closed vocabulary can’t sprawl


There is essentially one way to express “click this, expect that.” You cannot accumulate clever glue code in a language designed to have none.


Answer two


### AI proposes, humans dispose


Generated scenarios arrive as readable diffs a person approves, because the DSL keeps them readable by people who aren’t automation engineers.


First, an application codebase and an E2E test codebase are not the same object. The GitClear degradation signals (duplication sprawl, dying refactoring) emerge in general-purpose code, where there are a thousand ways to express the same thing and unbounded room for structural drift. A deliberately small, high-level DSL like Karate’s changes that geometry: there is essentially one way to express “click this, expect that,” the vocabulary is closed, and the output is short, declarative, and readable line by line. You cannot accumulate clever glue code in a language designed to have none. Constraining the generation space is not a side effect here; it’s the point.


Second, review stays structural, not optional. The autonomous test-repair case study I cite below shows what happens without it. The sane pattern, and the one I’ll defend, is AI proposing, humans disposing: generated scenarios and healed locators arrive as readable diffs that a person approves, precisely because the DSL keeps them readable by people who aren’t automation engineers.


## The tautology problem


There’s a second argument for the top of the pyramid, and I think it’s the stronger one.


When a human writes the code, unit tests verify “I built what I intended to build.” That’s meaningful: human intent and human implementation genuinely diverge.


When an AI writes both the code and its unit tests, those tests largely verify “the AI understood what the AI meant.” That’s close to a tautology. And the research increasingly backs the intuition. An[empirical study presented at ASE 2024 (Yang et al.)](https://arxiv.org/abs/2406.18181) evaluated LLM-generated unit tests against Defects4J and found their defect detection ability weak: for the large majority of defects, no valid test was generated at all, and even among detectable defects, roughly half went undetected. A[systematic review of 115 publications](https://arxiv.org/abs/2511.21382) (2021 to 2025) reaches the same conclusion: weak fault detection remains the open challenge of LLM-based test generation. And a[2026 mutation-driven study from Virginia Tech and CMU](https://arxiv.org/abs/2603.23443) asks the uncomfortable question directly: do LLM-generated tests reflect genuine reasoning about program behavior, or do they reproduce superficial patterns from training?


Let me be honest about what these findings imply, because they cut both ways: a naively AI-generated E2E test inherits the same weakness. Generation alone guarantees nothing. What breaks the tautology is not the generator; it’s the anchor. A test anchored on user-facing behavior, written in a language the business can read and validate, sits outside the AI’s self-referential loop in a way that a generated unit test never can. **The oracle is a human who knows what the product should do, not the model that wrote the code.**


Which is why I believe executable BDD, meaning Gherkin scenarios that actually run against the real system, is becoming the standard for the top of the pyramid in an AI-assisted world. BDD’s original sin was always the gap between the scenario written by the three amigos and its automation, which fell back on developers and QA as thankless glue work. AI is closing precisely that gap. Some in the testing community have started calling the LLM the “fourth amigo.” I think the framing holds.


And it reshapes the team, not just the tooling. For twenty years we’ve split QA into two castes: manual testers who know the business but don’t automate, and automation engineers who automate but drift from the business. If generation becomes cheap and the scenarios stay readable, that wall stops making sense. The manual profile moves to where its knowledge was always most valuable: expressing and validating business intent, driving generation. The automation profile moves to guaranteeing the quality behind it: reviewing what was generated, hardening the technical layer, owning the pipeline. Not two castes; one team, reunified around the tool. That reunification is, to me, the most underrated consequence of this whole shift.


## Why Karate, specifically


Follow the thesis to its conclusion. If the top of the pyramid densifies, and executable BDD becomes its standard, then the frameworks that matter are the ones positioned exactly there: open source, Gherkin-native, covering API and UI in one coherent model, with a real community rather than a demo video.


That’s not a description I reverse-engineered to flatter my new team. It’s the market reading that led me to write that comparison in 2023, long before any role existed. Karate’s API testing foundation is solid and widely adopted. The frontier now is UI, and the honest, hard question of where AI fits into it (a question that goes well beyond[the Selenium-versus-Karate comparison](https://karatelabs.io/compare/selenium) I started with). That’s the work I’m joining for.


## What I’ll actually be doing


My focus is UI product direction, and one architectural question in particular: **where should the LLM sit in the lifecycle of a UI test?**


There are at least three schools of thought emerging in the industry.


Lowest autonomy


### Authoring-only


AI generates tests but stays out of the runtime.


Middle ground


### Embedded safety net


The runtime can recover when the UI drifts.


Highest autonomy


### Full runtime pilot


An agent drives the browser end to end.


Each has real trade-offs in determinism, cost, and trust, and the industry has not settled the question. A recent[industrial case study of a fully autonomous multi-agent test repair system](https://arxiv.org/abs/2605.01471) is instructive here: under high autonomy in a realistic enterprise UI environment, the system exhibited severe failure modes, including a phase where planning appeared healthy while test generation had silently collapsed. **Autonomy is not free.** Working through exactly those trade-offs, with a framework that has actual production users, is the kind of problem I enjoy.


I’ll share what we learn as it ships. Measured, and with numbers when I can.
