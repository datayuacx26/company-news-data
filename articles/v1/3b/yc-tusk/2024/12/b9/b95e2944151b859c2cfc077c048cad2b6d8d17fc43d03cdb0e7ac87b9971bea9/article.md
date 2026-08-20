---
schema_version: "1.0.0"
document_id: "b95e2944151b859c2cfc077c048cad2b6d8d17fc43d03cdb0e7ac87b9971bea9"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/tusk-launch-generate-and-run-tests-with-ai"
published_at: "2024-12-18T09:00:00+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:f20a9751953796c9800ce978c7bbb9e85244bfc4893657e3ecdd8b15ae235b41"
---

# Tusk Launch: Generate and Run Tests With AI

# The Era of AI-Powered Testing


Customers have told us a bunch that they want Tusk to write unit tests along with its code changes, just like a software engineer would.


We went off and spoke to over 100 CTOs and EMs at high-growth consumer and B2B SaaS companies. What we found was that this was a big enough problem to warrant building a separate AI agent -- one that uses both business requirements and codebase context to generate unit and integration tests for PRs. So that's what we did.


## Balancing Quality and Speed


Your browser does not support the video tag.


*Demo of Tusk catching an edge case in a PR in strict mode (only the highest-impact tests shown)*


We're coming up to the end of the year, and companies are rushing to cut a release before people go out-of-office for the holidays.


Any engineer who has had to hastily meet product deadlines knows that writing tests was the last thing on their mind. Their main priority was to ship the feature as fast as possible.


But the issue down the line is that customers will experience and complain about a bug in the feature, which puts the blame on Engineering for not caring enough about quality.


We know how painful this experience is as engineers at our previous jobs. We built[Tusk Tester](https://usetusk.ai/) to provide EMs and senior engineers in charge of quality efforts a solution to this perennial problem. Our test generation agent creates unit and integration tests for your PRs with codebase context, allowing teams to ship fast while increasing feature coverage.


CTOs and EMs have overwhelmingly told us that LoC code coverage as a metric incentivizes teams to write tests for the sake of writing tests. No good. Which is why Tusk is optimized to catch edge cases that aren't already covered in your engineers' tests.


## How It Works


Tusk Tester is a non-blocking PR check that generates unit and integration tests and catches failing test cases. Read[our documentation](https://docs.usetusk.ai/automated-tests/overview) for more details.


1.


When a commit is pushed to a PR, Tusk will look at the code changes, PR details, any existing tests, and your codebase context to generate unit and integration tests for the happy path and edge cases.


2.


Tusk will run these new tests in an **isolated sandbox** .


3.


If any tests fail and Tusk determines that the failure is due to an issue with the test, Tusk will auto-iterate on the test case.


4.


Once the check is complete, Tusk will display the test cases created in the check’s details.


5.


You can view the code for each test case, the reason why it was created, whether it passed or failed, and the test execution logs.


6.


Click **Incorporate tests** to add the tests to the PR. If you’d like to add only a subset of the Tusk tests, hover over the code block, click to copy the test to your clipboard, and add it to the test file(s) in your IDE.


7.


You will see a new commit in the PR with the Tusk tests added. Good to go!


## Human-in-the-Loop


Having built AI agents from the moment the concept was conceived, we know that steerability plays a big part in creating the ideal developer experience.


Tusk puts the engineer in control by allowing them to accept/decline the generated tests and customize configs to meet your team's testing best practices (mock vs. stub, avoid specific directories, etc.).


LLMs are uniquely suited to preventing bugs from slipping into prod because of their ability to simulate a massive number of test scenarios within a short amount of time. Let Tusk take over this testing burden, while your engineers focus on high-level decision making.


## Interested?


Tusk Tester is now in private preview for select customers.[Book a demo with us](https://cal.com/team/tusk/demo) to learn about how Tusk can help your team ship faster but safer.
