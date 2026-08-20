---
schema_version: "1.0.0"
document_id: "087bdfa76a2df00e94bc33c143e5d5dc487472ae2b479a841372e1cf72fbc4ed"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2026-02-18-how-a-learning-project-became-our-modern-mobile-test-framework/"
published_at: "2026-02-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:19:43.789522+00:00"
content_hash: "sha256:edb80ebe323c6d66069dbb3a0970bf00bb2ba552f8c0e5a4f5a99520f993751a"
---

# How a Learning Project Became Our Modern Mobile Test Framework

## **Introduction**


About six years ago, our mobile automation setup was showing its age. It was a small, homegrown framework that had worked “well enough” for a long time, until we tried to upgrade Java and a few core dependencies. That’s when things started breaking in ways we couldn’t reasonably patch around. The framework simply wasn’t built to move forward.


Rather than trying to keep it alive with one off fixes, one of our engineers used the moment to start a learning project with Appium 1. The goal wasn’t to replace everything overnight, it was to explore whether we could build something stable, maintainable, and shared across iOS and Android. As the proof of concept grew, it became clear the approach was solid. That “learning project” eventually turned into our primary test framework for years.


## **The Appium 2 (and later 3) Transition**


When Appium 2 arrived, it changed some fundamental assumptions. Most notably for us, the old MobileElement pattern we had leaned on was deprecated in favour of W3C WebDriver standards and platform specific drivers. That forced a decision: keep bolting compatibility layers onto an aging design, or take what we’d learned and rebuild the framework the right way?


We chose the rewrite this time, designed intentionally around Appium 2’s driver model and extensibility. We leaned into modularity, separated platform concerns cleanly, and built the framework so future changes in Appium wouldn’t require another ground up rebuild. When Appium 3 followed we were set up to move forward, not stuck reworking things.


## **What We Did Differently in the Appium 2/3 Framework**


Rewriting the framework on top of Appium 2 (and later Appium 3) was not just a migration, it was an opportunity to rethink several core architectural and collaboration decisions. Below are some of the most impactful changes we introduced in the latest generation of the framework.


-


**From Parallel Forks to Parallel Threads**
Previously, our parallel execution relied on multiple forks (separate JVM processes). This gave strong isolation by default, but it increased resource usage because each fork paid its own JVM startup and memory overhead.
In the new framework, we run scenarios in parallel threads within a single JVM. This reduces JVM startup cost and overall memory consumption. The practical outcomes were:
• Lower resource overhead
• Faster startup and execution


-


**Plugin Based Architecture with Full Lifecycle Access**
Previously, a lot of framework logic was tightly coupled. Platform specific behaviour, infrastructure concerns, and test utilities often lived too close together, which made changes risky and extensions painful.
The new framework introduces a plugin architecture, where:
• Each plugin can hook into any phase of the test lifecycle
• Platform specific behaviour (iOS / Android) lives in dedicated plugins
• Cross cutting concerns (logging, reporting, configuration, session management) are reusable and isolated.
This architecture made the framework easier to extend, safer to evolve, and much more explicit about responsibilities.


-


**Bringing App Developers into the E2E Loop**
One of the biggest improvements wasn’t just technical, it was cultural.
As the framework matured, app developers started treating E2E testing (and Appium constraints) as part of normal development instead of something QA “handles later.” In practice, that meant:
• New UI elements consistently include accessibility IDs / resource IDs
• Testability is considered during feature design, not retrofitted
• QA and app engineers share a common language for identifiers and UI structure
Over time, this reduced flakiness and lowered the cost of maintaining tests.


-


**Using E2E Tests to Enforce Accessibility (a11y)**
Once accessibility identifiers became standard, we went further and used automation to protect accessibility itself.
We now use E2E tests to:
• Validate accessibility labels and values
• Catch regressions in screen reader related attributes
• Ensure iOS/Android behaviour stays consistent
That way, our E2E tests do two jobs: they check the features still work, and they make sure we don’t break accessibility as we ship changes


## **Conclusion**


This wasn’t a straight line “migration.” It was a series of evolutions:
We started with an outdated framework that couldn’t keep pace with modern tooling.
A small Appium 1 learning project proved we could unify iOS and Android testing in a reliable way, and it grew into our main framework.
Appium 2 (and later Appium 3) gave us the push to redesign instead of patch, moving to a standards based, driver centric approach with a modular architecture.
Today, our framework reflects nearly a decade of lessons: it’s faster to run, easier to extend, more resilient to Appium changes, and better integrated with how our mobile teams actually build products.


## **Closing Thought**


The Appium 2/3 rewrite wasn’t just a response to breaking changes. It was a chance to apply years of production experience and rebuild around what actually works: clear boundaries, modularity, strong lifecycle control, and shared ownership of testability. The result is a modern framework that’s faster, more maintainable, and far better aligned with how mobile apps evolve.
