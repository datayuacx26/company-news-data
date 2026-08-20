---
schema_version: "1.0.0"
document_id: "d0779a0650fc55cf4b0f45561d6ffd65b434ad284d38521cb1ce6a89c6bdd85d"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/teamfeepay-case-study"
published_at: "2025-05-20T09:00:01+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:93116c227468c7492966ea2d30f20392b25c9d0e5c2b2c1f3c719cbee9f98152"
---

# TeamFeePay Increases Unit Test Coverage 30% in 3 Weeks

59.6%


Tusk test suites incorporated into MRs


887


Tests added to codebase in 2 months


28


Bugs prevented in 2 months


## Introduction


[TeamFeePay](https://teamfeepay.com/?utm_source=tuskblog) is a comprehensive club development software platform designed to help football clubs thrive. Created by a team of experienced coaches and sports administrators, their solution goes beyond basic financial management to support all areas of football club development, with UK-based experts providing tailored plans for each organization's specific needs.


## Problem


The TeamFeePay engineering team approached[Tusk](https://usetusk.ai/?utm_source=blog&utm_source=teamfeepay-case-study) as they were searching for a new solution to improve unit test coverage across their large codebase. Taking a quality-first approach to development was especially important because they handle payment processing and fee management.


Stephen Houston, CTO at TeamFeePay, identified that while their team had strong testing practices for new features, their legacy codebase had **inconsistent unit test coverage** . This created risk areas in their application where bugs could potentially slip through undetected.


"As a platform supporting vital club operations and finances, reliability isn't just a nice-to-have—it's mission-critical," explained Stephen. "We needed a solution that could help us systematically increase test coverage without slowing down our development cycles." With a distributed engineering team where some software engineers placed more emphasis on testing than others, Stephen was looking for a tool that could serve as a **safeguard for code quality** across the entire organization.


## Solution


"Tusk has contributed to about three quarters of our recent test coverage increase, so it's been a helpful boost to test coverage, particularly given the size of our codebase as well."


**Stephen Houston** – Chief Technology Officer at TeamFeePay


TeamFeePay integrated Tusk's AI test generation agent into their GitLab CI/CD pipeline for their primary **Ruby on Rails** codebase.


The onboarding process was straightforward, with Stephen completing the initial GitLab authorization in minutes, after which the Tusk team handled the configuration of the test environment required for Tusk to self-execute its generated tests. Within 24 hours, Tusk was operational, and TeamFeePay engineers began receiving AI-generated test suggestions directly within their merge requests (MRs).


Stephen, when reviewing his engineering team's MRs, would view Tusk's test output to get an understanding of whether the MR's changes met the technical requirements. If there were failing tests, he would use Tusk's suggestions for potential fixes as a starting point to make sure the function under test worked as expected before merging.


Because Stephen opted to use Tusk without enabling "Strict Mode", Tusk would provide a **backfill of unit tests for untested functions** that were modified in the MR, even if the tests were only adjacent to the scope of the MR's changes. By doing so, the engineering team increased code coverage even in legacy parts of the codebase. Moreover, this piecemeal approach enabled engineers to add tests while already having context of the system under test.


## Results


"Unit test coverage was in the high 30s before we started using Tusk. Coverage has certainly picked up a good bit since then. So it's heading in the right direction."


**Stephen Houston** – Chief Technology Officer at TeamFeePay


The impact of implementing Tusk was both immediate and substantial. Within just 3 weeks of deployment, TeamFeePay saw a 30% increase in unit test coverage across their large codebase, with three quarters of that attributable to Tusk alone. This was a significant improvement that would have required months of dedicated effort to achieve manually.


### In the first 2 months


- **59.6% of Tusk's test suites** were incorporated by engineers into MRs
- **887 tests** were added to the codebase
- **28 bugs** were prevented before they could reach production


The TeamFeePay team was impressed by Tusk's ability to identify potential issues in club management features and payment processing edge cases that could have resulted in membership discrepancies, operational disruptions, or failed transactions if left unaddressed. When asked about his enthusiasm to continue using Tusk, Stephen responded with an emphatic "100%."


---


## Curious?


Tusk is an AI agent that generates unit and integration tests for PRs. Backed by Y Combinator. If you’d like to use Tusk to increase code coverage and prevent regressions,[book a demo](https://cal.com/team/tusk/demo) with us.


For football club development software that helps your organization thrive, visit[TeamFeePay](https://teamfeepay.com/?utm_source=tuskblog) to learn more about how their platform and expert team can help boost membership, streamline operations, and achieve financial security with a bespoke plan.


---


TRY TUSK NOW


## AI test generation with codebase context for quality-obsessed teams.


Cover your blind spots, catch verified critical bugs, merge PRs 25% faster with peace of mind.
