---
schema_version: "1.0.0"
document_id: "2acdbb770369ea2081e5610fe02d77d8e9c38d22a5cedc98baff463f2c45862c"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/why-we-shift-testing-left-a-software-dev-cycle-that-doesnt-scale/"
published_at: "2024-05-20T15:27:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:45fa58515017bc2b54a7930481f2c570ff6efff33f32f0991ead0057c89e73ed"
---

# Why We Shift Testing Left: A Software Dev Cycle That Doesn’t Scale

*Originally posted on TheNewStack. This is the first of a four-part series.*


Shift left testing is the practice of moving testing earlier in the development process. In a traditional development cycle, testing often occurs after the completion of a feature or even at the end of the development phase.[Shift left testing](https://www.signadot.com/blog/why-testing-must-shift-left-for-microservices/) challenges this by integrating testing activities from the beginning and throughout the development process. For how shifting left fits the rest of the lifecycle, see our complete guide to[microservices testing](https://www.signadot.com/the-complete-guide-to-microservices-testing-from-local-development-to-production/) .


Let’s start by discussing why we chose the option to shift left, as it’s not the only way to solve scaling problems with your software development life cycle (SDLC).


## **A Tale as Old as Time: An SDLC That Doesn’t Scale**


Before we decide to move more testing over to the development side of the process, let’s discuss why leaders would make this decision in the first place.


There’s a cynical read of “shift left” that is “make people do more work but pay them the same.” A stressed-out dev team might rebel at the thought of adding more concerns to their docket. But the real motivation for shifting testing left is a frustration with a software release cycle in deadlock, developers forced to context switch and never allowed to get into a flow state and a glacial development velocity.


### **A Scaling Company Whose Testing Process Isn’t**


Imagine you’re at a company that’s scaling up past the startup stage. You all remember a time when features went out in weeks or even days, but those days are long gone. Now multiple teams are trying to release branches at the same time, and QA is stressed trying to find problems before they release to production.


Worse, the complexity of the microservice architecture means that developers can’t effectively do integration tests before shipping code off for QA. There’s a lot of talk about improving contract testing, but again, things are scaling quickly and the estimates for writing clear intra-service contracts and refactoring to match would take months. In the meantime, developers are stuck writing unit tests and creating crude mocks for outside services like payment providers and even the services of other teams. This leads to more surprises when code is deployed to pre-release environments like staging.


### **The Choice to Shift Testing Left**


So our engineering leaders are faced with a choice: How to[improve developer velocity](https://www.signadot.com/blog/improve-developer-velocity-by-decentralizing-testing/) while also shipping fewer bugs to production? You have two options:


Increase the QA team, and expand the resources for their testing/staging environment, so that they can do more tests faster.


This isn’t a terribly scalable choice. Already teams are occasionally conflicting over who gets to deploy to staging, and how long they need to have the environment reserved. Further, even with more QA engineers, you’re still making developers write code, run limited tests, push and then wait for a human being to write back with what problems were detected. The result is an “outer loop” feedback cycle where devs either have to pause all work until they get QA feedback or context switch between two branches as they try to work on the next feature while switching back and finding problems with the last thing they pushed. Even with an unlimited budget for the QA team, this definitely won’t scale to hundreds of developers.


### **Growing QA Isn’t an Alternative to Shifting Left**


QA has a major role in the strategic vision and architecture decisions for testing as an engineering team shifts testing left. But the quest to ensure that more releases are successful isn’t perfectly served by just expanding the QA team. Why is that?


As devs are only doing basic testing before merging code, it’s likely that QA discovers trivial integration bugs.


Is that list of attributes an array or an object? And is that locator 0-indexed or not? These small integration errors can block a release when QA is testing them. Instead of a developer finding small problems with their own code and quickly fixing the errors, QA is now required to document and communicate back the issue. Even worse is when QA doesn’t know who to communicate with:


QA teams typically write end-to-end tests that test for business/user flows. If any of these tests fail, it’s not clear who to assign the issue to, as the flow could touch several frontend and backend services.


As a developer you make updates to` user_state_controller` , but as a QA you may be testing things like “user is able to update their login.” This isn’t a mismatch, as the teams have different goals. But when QA finds errors in a user flow, they don’t necessarily know who in development would be the best person to talk to. This causes further delays.


The cadence in which to run the tests is not clear. Should QA run tests daily? After every pull request is merged? Since the suite of tests is likely to be large, it’s impractical to run the whole suite for every commit.


Without a team of engineers to determine the best set of tests to run, QA ends up running tests at a reduced cadence and only finding problems in large groups.


Devs experience very slow debugging cycles when tests fail in the QA phase. When there are multiple commits that are being tested together, it’s not clear which one is the offending commit.


This is a basic breakdown that I think warrants more discussion: Running a large test suite by a QA team is an appealing way to make sure that everything is working as designed end to end. But high-fidelity tests also need to be high frequency. If not, we lose the basic benefits of source control: Without tests running for every single commit, we lose easy rollbacks and fixes. This is one of many ways that “shift left” testing is a return to an older standard: Developers should be able to tell right away if their current group of changes broke something. Relying on QA to find problems the first time means you’re picking through a long list of failures trying to determine which were caused by which commit.


This process then becomes more of a waterfall model where dev and QA are distinct, sequential phases. This is the opposite of an agile approach that has obvious benefits.


Again, we’re talking about a regression from the coding and delivery standards of 2005. The waterfall model makes it impossible to do true continuous delivery to production.


One essential idea here is that QA being the only finders of bugs is disempowering to your engineers.


## **Empowering Your Engineers**


Even without the leadership-level concerns about the impact to velocity, you can imagine that a setup where most feedback comes on the outer loop can be frustrating and painful for engineers. My own ego aside, I don’t want to push code that I have little confidence in. This brings us to option two:


Shift testing left, give devs the power to run integration or even end-to-end tests within minutes of writing their code and ensure nothing is being QA’ed for pre-release without 95% certainty that everything is working.


In this scenario, breaking things as a developer can feel fun again: We’re experimenting, seeing what the other services can support and only pushing what we know works. QA still has a major role in this design, but their role is less in manually testing and finding individual bugs. Rather, the QA team becomes a strategic leader in defining automation strategy, selecting testing frameworks and enabling developers to have more confidence in their code. With testing available to every engineer every day, there will be many more tests being written and run, but their feedback goes straight to developers on an inner loop of feedback.


## **Shifting Testing Left Is a Return of an Older, Better System**


In a recent discussion in a platform engineering lean coffee group, one idea stuck with me, mentioned by a few participants:


“Microservices present a lot of advantages, but microservices are much more difficult to test. This problem is so severe that many teams don’t see real benefit from microservices.”


I’ve[written about this before](https://www.signadot.com/blog/prezi-eventbrite-teams-testing-on-kubernetes/) but to summarize: In the era of monoliths, there was often a way to run the whole system, or a facsimile, on your laptop with minimal friction. This meant significant testing could happen right as you were developing.


So really, when we talk about shifting testing left, we’re talking about shifting it *back* . We still have a role for QA teams, but we want to put the first rounds of testing back in the hands of the engineers writing the feature.


## **QA Still Has a Major Role**


In a[follow-up to this article](https://www.signadot.com/blog/why-shift-testing-left-part-2-qa-does-more-after-devs-run-tests/) , I’ll write about the role that QA has and will continue to have as you shift the responsibility for running testing over to your development teams. In short, QA still has a major role to play and will be doing a lot more, not less, when developers are running more of your tests. If you’d like to discuss this article and the ideas of shift left testing,[join us in the Signadot Slack.](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) We’d love to hear your ideas!


‍
