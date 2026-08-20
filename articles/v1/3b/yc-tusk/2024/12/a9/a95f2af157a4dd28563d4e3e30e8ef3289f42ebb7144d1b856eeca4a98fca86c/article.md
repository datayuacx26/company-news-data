---
schema_version: "1.0.0"
document_id: "a95f2af157a4dd28563d4e3e30e8ef3289f42ebb7144d1b856eeca4a98fca86c"
company_key: "yc-tusk"
company: "Tusk"
source_id: "yc-tusk-rss-fc043d74cc9e"
canonical_url: "https://blog.usetusk.ai/blog/evaluate-your-unit-tests-with-this-quick-exercise"
published_at: "2024-12-10T16:02:00+00:00"
first_seen_at: "2026-07-26T03:21:03.026732+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:6db4b05e9f142fa7fc6cfc6c312a1cd0d8ebcd6bb120e80a649942e6f2a399a6"
---

# Evaluate Your Unit Tests With This Quick Exercise

"Eat your veggies" of the programming world


Writing unit tests is like maintaining a clean diet and exercise.


Everyone knows you should do it regularly because it’s good for your health, yet it is extremely easy to put off because of how tedious it can be.


Today's engineers can make use of AI copilots to write tests but it still feels like a chore to cover all edge cases in a feature, especially when they've already done manual testing locally.


As an engineering leader, how do you determine if your team is writing **meaningful unit tests** ?


## Is Tracking Code Coverage a Solution?


Some engineering leaders believe that enforcing code coverage requirements as a blocking check in their teams' PRs is the solution.


This solution, however, often leads to unintended consequences. For starters, implementing such a blocking check means that you run into a classic **principal-agent problem** between engineering leadership and individual contributors (ICs).


If you tell your software engineers that they need to meet code coverage before merging a PR, you incentivize them to write tests for the sake of writing tests. We see this a lot with engineering teams that use **LoC test coverage metrics** for their unit testing.


To extend the health analogy, it’s kind of like gifting someone a gym membership only for them to sit around on the bench scrolling through Instagram.


Other code coverage metrics like branch coverage and paths coverage are better alternatives but they are not immune to the same problem. Though to steelman this approach, code coverage going up is usually an indicator that the state of testing is going in the right direction, even if not entirely accurate.


## Goodhart's Law in Action


Goodhart's Law: "When a measure becomes a target, it ceases to be a good measure." Source: XKCD


One of the more extreme versions of code coverage tracking leading to counterproductive behaviors is an anecdote I once heard from an experienced Salesforce developer.


**Brief background:** Apex is a strongly typed, OOP programming language with Java-like syntax that runs on Salesforce's platform.


In order to deploy Apex or package it for Salesforce AppExchange, Salesforce requires that you cover **75% of your Apex code with unit tests** . Moreover, those unit tests must pass.


An unintended consequence of this code coverage requirement is that Salesforce developers in a rush to deploy their code changes will sometimes write placeholder files with print statements and then write unit tests for the dummy code. Ta-dah - 75% code coverage!


As developers ourselves, I think it's safe to say that developers are going to find their way around hard requirements. In cases like this, enforcing the requirement just leads to wasted time without the benefit of quality engineering.


## 3-Step Exercise


Instead of using LoC test coverage as a measure for the health of your unit tests, I recommend trying out this quick-and-dirty exercise with your engineering team:


1. Pick one of your recent unit tests.
2. Make an intentional change in the logic it’s supposed to cover.
3. Does the test break? Or does it pass silently?


If the unit test **still passes** despite the logic change, it signifies that the test provides a *false sense of security* –– it isn't testing anything meaningful in your code.


This is a cause for concern and could lead to a **high change failure rate** .


Follow this exercise consistently when reviewing PRs and you'll be able to identify and address these gaps with your ICs. Ultimately, the goal is to foster a healthy testing culture in your organization, so that engineers are proactive about writing meaningful tests.


---


At[Tusk](https://usetusk.ai/) , we’ve built an **AI test generation agent** that suggests edge case tests for your team's PRs since you can’t realistically review every test case.


Grab time on[our calendar](https://cal.com/team/tusk/demo) if you want to learn more about how we can help you improve your feature coverage with high-impact unit tests.
