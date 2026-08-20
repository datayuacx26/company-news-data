---
schema_version: "1.0.0"
document_id: "4f1c02f30b76d370ce644d8794d00336dca4d3c7cc1c11a76bf9006d6b69f335"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/testing-is-now-your-core-competency-dont-outsource-it"
published_at: "2026-03-02T00:00:00+00:00"
first_seen_at: "2026-07-22T04:45:46.439360+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:651bcb4d8bb7b778a2c4e409ee6f6753fbf85e9cb03b7ade57dc1af05549476c"
---

# Testing Is Now Your Core Competency. Don’t Outsource It

For the last decade, the smart move was to outsource your testing. You had limited engineering headcount, testing was repetitive and expensive, and some vendors could do it cheaper. It made sense.


It doesn't anymore. The fundamental shift in how software is built means testing has moved from a support function to a core competency. If you're still outsourcing it, you're now outsourcing the critical primitives of your product.


## Tests are now the most valuable artifact in your codebase


At the end of February, one Cloudflare engineer with $1,100 in AI tokens and a dream[reimplemented 94% of the Next.js API surface in one week](https://blog.cloudflare.com/vinext/) . The output passes over 1,700 unit tests and 380 end-to-end Playwright tests.


The reason this worked is simple: Next.js has a comprehensive public test suite. Those tests function as a machine-readable specification. The AI didn't need to understand the framework's intent or philosophy. It was implemented against a contract, ran the tests, and iterated until they passed. Define a task, generate code, run the suite, merge or fix. Repeat.


This isn’t a million miles away from the[Claude C Compiler](https://www.anthropic.com/engineering/building-c-compiler) . Claude was able to build a C compiler in a few days and with $20k of tokens, exactly because it could test what it was building. As Anthropic researcher Nicholas Carlini put it:


> “Claude will work autonomously to solve whatever problem I give it. So it’s important that the task verifier is nearly perfect, otherwise Claude will solve the wrong problem. Improving the testing harness required finding high-quality compiler test suites, writing verifiers and build scripts for open-source software packages, and watching for mistakes Claude was making, then designing new tests as I identified those failure modes.”


SQLite has operated this way for years. The database itself is in the public domain. The most comprehensive test suite,[TH3](https://sqlite.org/th3.html#th3_license) , is proprietary. The core library is about 156,000 lines of C. The test code is 590 times larger. What looked like an unusual business decision now looks like the smartest IP strategy in open source.


The pattern is clear. Comprehensive tests have become a precise, machine-executable description of what your software does.[The test is the truth](https://momentic.ai/blog/the-test-is-the-truth) . That makes them either your greatest asset or your biggest vulnerability, depending on whether you're the one using them.


### Tests are what make AI-generated code trustworthy


AI generates code fast. Without tests, that code is plausible-looking text and nothing more.


The vinext workflow made this explicit. One engineer defined tasks, pointed AI at the codebase, and let it write implementations. But the speed was only useful because[tests existed to verify the output at every step](https://paddo.dev/blog/vinext-test-suites-are-specs/) . Remove the test suite and the entire loop breaks. You're left with generated code that looks right but has no mechanical way to prove it works.


This applies whether you're reimplementing a framework or shipping a feature. The team with good tests can use AI to move fast because they have a verification layer that catches mistakes automatically. The team without good tests can't trust what AI produces. They end up manually reviewing every line of generated code, which eliminates most of the speed advantage.


Tests are the correction mechanism. They're what turn “AI can write code” from a party trick into an actual productivity gain. Without them, AI-assisted development is fast but unreliable. With them, it's fast and verified.


## Why you can't outsource this anymore


When testing was just about catching bugs before release, outsourcing made sense. You wrote the code, someone else verified it, and you shipped. That division of labor was clean enough.


That's not what testing is anymore. When tests are the specification layer for AI-assisted development, the verification loop for every AI-generated change, and the most precise description of what your software actually does, **testing is the core of how software gets built** .


Outsourcing your testing is now functionally the same as outsourcing your development.


### Testing is now central to the SDLC


The common response here is "we outsource execution, not strategy. We decide what to test, and the vendor runs it." That distinction made sense when testing was a discrete phase. Write the test plan, hand it over, and get results back.


But in an AI-assisted workflow, execution and strategy aren't separable. Every test run generates information that should immediately influence what you test next, how you interpret failures, and what you prioritize. The feedback loop between writing code and verifying it is now measured in minutes, not days. You can't outsource one half of a loop that tight.


The old workflow was linear: develop, then test, then ship. Testing was a gate at the end. You could hand it off because it happened after the real work was done.


In an AI-assisted workflow, testing happens continuously. An engineer describes a task; AI generates code; tests verify it; and the cycle repeats dozens of times per feature. Tests aren't a gate at the end. They're the feedback loop that drives development forward.


Outsourcing that feedback loop means introducing a handoff into the middle of your development process. Different timeline, different context, often a different time zone. Bug reports arrive days after the code was written, by which time the engineer has already moved on. You cannot run a tight AI-assisted development loop when your verification step sits outside your organization.


Shift-left, the industry consensus that testing should move earlier in the development cycle,[is structurally incompatible with outsourcing](https://momentic.ai/blog/qa-test-plan) . You can't shift testing left when the people doing the testing are waiting for a handoff from the right.


### Tests are how you understand your own system


Over time, a team that owns its tests accumulates deep knowledge of failure modes, edge cases, customer-impacting scenarios, and architectural weak points. This knowledge makes testing faster and more effective with each release. It also makes every other engineering decision better, because the people building the product understand where it breaks.


When testing is outsourced, this knowledge never forms inside your organization. The outsourced team may develop it, but it lives in their heads, in their documentation, and in their processes. Cancel the contract, and you have nothing. Ask yourself: if you ended your QA vendor relationship tomorrow:


- What would you know about your own system's failure modes?
- Which critical user flows have reliable test coverage, and which don't?
- Which parts of your codebase are fragile, and why?
- What breaks when you ship a redesign?


If you can't answer these confidently, that tells you how much quality knowledge actually lives inside your org.


The only durable option is distributed ownership: your team writes the test strategy, defines what matters, interprets the results, and builds institutional knowledge over time. The tooling can be external, but the understanding can't be.


### Testing is about quality, and quality is the thing you need to care about


There's a broader context here. The barrier to producing code has dropped to near zero, but the standard to producing good code hasn't moved.


What is now the upper bound for LOC/day? 100,000? 1 million? Depends on how many agents you have running. At this scale, the code itself can no longer be the differentiator. What differentiates is judgment: knowing what to build, how it should behave, and what "good" looks like for your users. *Taste* , as it is now called (overused, but directionally true).


Engineering leaders who develop that *taste* and encode it into their systems will build better products than those who simply generate more code faster.


Tests are how you encode that taste. A test that says "when a user changes their subscription mid-billing-cycle, the prorated amount should round in the customer's favor" isn't a QA artifact. It's a product decision made permanent. A test suite full of decisions like that is a compounding record of everything your team has learned about what your software should do and how it should behave.


Without that, you're relying on individual engineers to remember what "good" means across every feature, every refactor, every AI-generated change. That doesn't scale. Tests scale.


The companies that win in this environment won't be the ones that produce the most code. They'll be the ones with the clearest definition of quality and the discipline to enforce it. Testing is the enforcement mechanism.


## What competency looks like


If testing is a core competency, it has to behave like one. That starts with one thing: **strategy lives inside your org** . What to test, what to skip, what matters most. These are product decisions, not vendor deliverables. No outsourced team can make them for you, because no outsourced team understands your risk surface the way your engineers do.


Everything else follows from that:


- **Tests as product artifacts** . If tests encode your team's judgment about how the product should behave, treat them that way. Review them like code. Debate them like product decisions. Maintain them with the same rigor. A test that encodes a product decision is worth more than the implementation behind it, because implementations change, and the decision shouldn't.
- **Testing as an embedded flywheel** . The SDLC argument above has a practical corollary: if any handoff exists between "writing code" and "verifying code," you've already lost the speed advantage AI gives you. Tests run on every PR. Results are visible in GitHub, Slack, and the IDE. Failures block merges for critical flows. This is the loop that makes AI-assisted development work, and it has to be tight.
- **Testing as signal** . None of the above matters if you're tracking the wrong things. "Number of tests" and "coverage percentage" are vanity metrics that let teams declare victory without improving quality. Track what actually matters: time from bug introduction to detection, production incidents per release, and engineer confidence in deploying. A team with 200 tests and zero production incidents is in better shape than a team with 2,000 tests and weekly outages.
- **Testing as strategy (tooling as commodity)** . Buy a platform that handles execution, maintenance, and infrastructure. You own your deployment strategy even though you use Vercel. You own your observability strategy even though you use Datadog. Own your quality strategy the same way. (We wrote about how to evaluate build vs. buy here.)


## The split that's coming


Every engineering team now has access to the same AI tools. The same models, the same code generation capabilities, the same speed.


The differentiator is what you do with that speed. Teams that own their testing competency will use AI to ship faster, catch regressions automatically, and compound quality with every sprint. Teams that outsource it, underinvest in it, or treat it as someone else's problem will produce more code and worse software.


Own yours.
