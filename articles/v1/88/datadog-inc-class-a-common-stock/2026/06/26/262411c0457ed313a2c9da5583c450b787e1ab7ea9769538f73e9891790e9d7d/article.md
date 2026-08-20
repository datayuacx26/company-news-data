---
schema_version: "1.0.0"
document_id: "262411c0457ed313a2c9da5583c450b787e1ab7ea9769538f73e9891790e9d7d"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/bits-testing-test-coverage/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:0eb5957870dad29b6b61661de79f4f8d8d2ef182cee8b1de18fb88294353c1c4"
---

# Automate synthetic test coverage with Bits Testing

Younes Berradia


Product Manager


Marc Papazian


Natasha Silva


Technical Content Writer


Writing test scenarios and keeping them up to date can be time-consuming. When new user journeys or interface changes ship, test scripts can break and leave gaps in coverage. Critical flows such as checkout, signup, login, and booking can then reach production unvalidated.


Bits Testing, now[available in Preview](https://www.datadoghq.com/product-preview/bits-testing/) , automates synthetic test generation and maintenance. To generate tests, all you need to do is provide a URL or describe a goal in natural language. The agent explores the application, identifies critical user journeys, and creates test suites for them.


This post covers how Bits Testing helps you:


-


Discover and generate synthetic tests automatically


-


Create Goal-Based tests for dynamic applications


-


Combine deterministic and nondeterministic testing strategies


## Discover and generate synthetic tests automatically


Bits Testing uses discovery runs to explore applications autonomously and generates synthetic tests for critical user journeys. Instead of requiring you to define every click, form submission, or navigation event manually, the agent interacts with the application directly and records viable user paths as runnable test suites.


For example, you can point the agent at an ecommerce application and ask it to identify purchase-related workflows. The agent explores the site, navigates product listings, adds items to a cart, and proceeds through checkout flows to generate a collection of tests that represent real customer journeys. You can then review, accept, and schedule the generated tests like any other[Synthetic Monitoring test](https://docs.datadoghq.com/synthetics/) . This expands coverage without the engineering time needed for manually scripting and testing updates.


## Create Goal-Based tests for dynamic applications


Traditional tests are deterministic. They define an exact sequence of actions that must occur during every run. Deterministic tests work well for workflows where the precise execution path matters, such as validating API contracts, testing checkout sequences, or verifying authentication flows. These tests provide consistent coverage for known application flows and infrastructure dependencies.


Some workflows do not follow one fixed path. AI-powered applications, recommendation systems, inventory-based workflows, and conversational interfaces can produce multiple valid ways to reach the same outcome. In these cases, deterministic scripting becomes fragile because even minor interface updates can invalidate tests.


Bits Testing introduces Goal-Based tests for these dynamic environments. Instead of storing a fixed sequence of steps, a Goal-Based test stores the intended outcome in natural language. During execution, the agent determines viable paths to achieve that goal.


For example, you can define a goal such as “Complete a flight booking for a round-trip itinerary” or “Generate a support answer from the AI assistant.” At runtime, the agent navigates the application and adapts dynamically to accomplish the goal.


Goal-Based tests are especially useful for AI-powered applications where outputs and navigation paths may vary between runs. Rather than failing because a button moved or the interface changed slightly, the agent finds a new path while still validating the intended user experience.


## Combine deterministic and nondeterministic testing strategies


Deterministic and nondeterministic testing approaches solve different problems. Most teams can benefit from using both. Deterministic browser, API, and network tests generated via discovery runs validate workflows where execution order and exact behavior matter.


Goal-Based tests complement those workflows by handling scenarios where interfaces or execution paths change frequently. For example, a retail platform can use deterministic tests to validate payment processing APIs and transaction workflows, while using goal-based tests to verify that customers can still complete purchases after frontend redesigns or via utilization of a chat or recommendation engine.


Using both test types together helps teams maintain stable coverage for critical backend workflows while also testing evolving and nondeterministic experiences that would otherwise require constant maintenance.


## Get started with Bits Testing


Bits Testing automates synthetic test coverage for both stable and dynamic user journeys. Scheduled discovery runs and Goal-Based tests reduce manual maintenance and extend coverage to AI-powered and frequently updated interfaces.


To start automating your test coverage, join the[Bits Testing Preview](https://www.datadoghq.com/product-preview/bits-testing/) .


If you’re new to Datadog,sign up for a free 14-day trial .


-
-
-
