---
schema_version: "1.0.0"
document_id: "9af4f7a34869e84c131d709905ae950bb4382af86b4f25cd9501dbcef98741bc"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/is-the-testing-pyramid-broken/"
published_at: "2024-08-23T20:28:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:b7823df63cb59f196f487dec9a06c03c96407059f4fe7e8490919cd0c985c561"
---

# Is The Testing Pyramid Broken?

*Originally posted on*[The New Stack .](https://thenewstack.io/is-the-testing-pyramid-broken/)


**Problems in testing software with microservices have blocked many productivity gains. Here’s how to get back to confident releases in production.**


The benefits of new technology aren’t always apparent right away. In 1851,[Charles Babbage](https://en.wikipedia.org/wiki/Charles_Babbage) stated:


“One of the inventions most important to a class of highly skilled workers (engineers) would be a small motive power — ranging perhaps from the force of from half a man to that of two horses, which might commence as well as cease its action at a moment’s notice.”


Twenty years before the first practical electric generator, Babbage saw the need for the electric motor. And as electrification took off starting in 1900, everyone envisioned major improvements to industrial productivity.


Those improvements, though, didn’t appear right away. Factories that swapped steam drive systems for electric motors saw almost no immediate increase in productivity. The reasons are complex, but the principle is general: Even when an improvement has been sought after, planned for and universally praised, its benefits don’t come until we rethink how we work.


The testing and QA process are a final step of a huge automated process for releasing modern software. And while[microservices architecture](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) promises huge benefits for software systems, problems in testing production software have blocked many of those[productivity gains](https://www.signadot.com/blog/the-wrong-way-to-use-dora-metrics/) . Let’s look at how we used to test software, how microservices architectures have broken that process and how we can get back to a world where we confidently release working code to production, without abandoning the benefits of microservices.


## The History of the Testing Pyramid


Multiple explanations of testing as a pyramid, with fewer more expensive tests at the top and many small cheap tests at the bottom, have been proposed, but most recognize the simple three layer pyramid from Mike Cohn’s “[Succeeding with Agile](https://www.amazon.com/Succeeding-Agile-Software-Development-Using/dp/0321579364) .”


## The 3 Layers of the Testing Pyramid, and When to Use Them


Each layer serves a distinct purpose and should be used at different stages of the development process.


### 1. Unit Tests


**Description:** Unit tests form the foundation of the testing pyramid. They focus on individual components or functions within the software, ensuring each part works correctly in isolation.


**When to Use:** Unit tests should be run frequently, ideally as part of the continuous integration process. This frequent execution helps catch issues early, ensuring that changes do not break existing functionality. Example: In a project management tool, unit tests might verify that a single method works in the code of the back-end API, for instance, the method to create a task, update a task with a new description, or check off a task. Each test isolates and checks the logic specific to these features, using mock dependencies to simulate the broader system. Notably, the task management UI wouldn’t be covered, and the methods will not update any datastore, relying on simple mocks to pretend to update a datastore when a task is updated.


### 2. Integration Tests


**Description:** Integration tests sit in the middle of the pyramid and test how different components or modules of the application work together. These tests are fewer in number and more complex than unit tests.


**When to Use:** Integration[tests should be run](https://www.signadot.com/blog/why-shift-testing-left-part-2-qa-does-more-after-devs-run-tests/) at key points during the development cycle, such as after significant changes or features have been added. They ensure that individual units interact correctly within the system.


**Example:** In the same project management tool, integration tests might check how the task management system interacts with the calendar. They ensure that new tasks appear on the correct dates and that updates to tasks are reflected accurately in the calendar. While the datastore might be updated as part of an integration test, we might not test the UI or third-party services like a payment provider to create a subscription.


### 3. End-to-End (E2E) Tests


**Description:** E2E tests, at the top of the pyramid, validate the entire application flow from the user’s perspective. These tests are comprehensive and simulate real user scenarios.


**When to Use:** E2E tests should be run at significant milestones, such as before a major release. Due to their complexity and resource demands, they are executed less frequently but provide critical validation of the application’s overall functionality.


**Example:** For the project management tool, E2E tests might involve creating a project, adding team members, managing tasks and ensuring the entire project workflow functions correctly. Tests would be run using an automated browser controlled with a test framework like Playwright. These tests mimic real-world usage to verify that the application meets user expectations, and everything from visual updates of buttons to complete authentication, account updates and complex application interactions would be tested.


## How the Testing Pyramid Is Broken for Microservices


Microservices have come to be the dominant architecture for production services, in general offering the benefits of:


- Clear separation of concerns
- Smooth scaling — demand on single services is easy to identify, and services can be replicated without scaling roadblocks
- Faster debugging of failures — isolated to a single service, it’s easier to find a root cause
- Easier maintainability of code — a small team becomes an “expert” on their few microservices


These benefits are irrefutable, and this article doesn’t seek to advocate for older-style monolithic software. However, the process of testing in a microservice architecture is broken in a few key ways.


Before I dive in, let me say that these breaking issues involve the way microservices are implemented in the real world_._ In an ideal world, some of the problems below wouldn’t exist, but I’d ask the reader to ask whether some or all of the product architectures they have seen had these issues. In the order of pyramid layers:


**Unit testing** tends to break in microservice architectures for one reason: interdependence. When developers are tasked with creating unit tests, they tend to create tests that are easy to implement, and by design, they don’t create tests that require any other services. In the example above I mentioned a simple backend action like “creating a new task.” However, in reality, even such a simple task involves at least two services.


*Unit tests focusing on a single service*


Often developers end up writing tests that exercise every method in their code, without any actual certainty that common tasks will work as described. Unit testing isn’t “broken” as much as it’s entirely insufficient to know whether our code will really work after integrating with other services.


**Integration testing** tends to break in a microservices architecture due to issues of test reliability. With many components like the frontend and third-party services represented with mocks, it becomes hard to know whether our new code will work in a real environment. It takes real work to keep these mocks updated to reflect the current state of the services they’re covering, and when these fall out of sync, reliability of testing suffers. Inevitably, more and more failure cases are left until end-to-end testing is found, until teams start skipping this stage entirely.


*Extensive mocking of outside components*


**End-to-end testing** breaks in two significant ways: cost and quality of feedback. In order to run complete end-to-end tests, there are significant costs in terms of time and infrastructure costs for a high-quality environment and test runner to store results. Quality of feedback is another issue, partly as an outgrowth of costs: first off, a failure observed from the frontend doesn’t have a clear pointer to where the request actually failed, then if QA teams are running E2E[tests on multiple pull requests](https://www.signadot.com/blog/collaborative-pre-merge-testing-for-multi-pr-features/) at once, it’s very hard to localize the root cause. In the testing phase, engineers shouldn’t be struggling to replicate a failure.


As a result of these shortcomings with existing end-to-end testing environments, teams will try multiple solutions: creating many test environments, which increases costs and creates the risk that my end-to-end testing environment doesn’t perfectly reflect the other versions’ state, or using something like feature flags to test new code directly on production. Neither solution scales to large teams, and so by this point all layers of the testing pyramid are rendered unreliable.


## A Reminder of the Benefits of the Testing Pyramid


What then, is the core benefit of the testing pyramid? The idea is simple:


Different levels of testing should catch different types of failures, starting with the simple and common, and moving up to the subtle and complex.


The idea, then, is that unit testing should catch every simple typo and off-by-one error that every programmer makes in her daily work, integration testing will find all the times you didn’t read the spec correctly or sent misformatted JSON, and end-to-end testing finds only the subtlest problems. With the testing pyramid, you should be surprised when the last layer of testing fails, and after end-to-end testing succeeds, we should have high confidence that our code works on production.


We still need all the layers of the testing pyramid to work as designed, but we must work out how to make these steps work in a complex microservice architecture.


## How to Fix the Testing Pyramid


The solution needs to enable developers to test early on an environment that looks a whole lot like production. No level of restrictive policies, unit tests specs or detailed QA review will take the place of empowering developers to use an accurate environment early in their cycle. An ideal solution will have the following characteristics:


- A distributed, decentralized approach to testing
- Support for API, contract and integration testing scoped to each microservice versus solely relying on UI testing
- Tests need to be run early, before the code merge. This makes testing fast and debugging quick.
- Testing needs to be effective. Mocks and other simulations that stray away from the production-like environment are not ideal. Testing in a live environment is desirable.
- It needs to be easy to adopt across engineering — infrastructure and provisioning concerns need to be abstracted away from developers in their day-to-day work.


Any system that meets these requirements will effectively allow engineers to use every layer of the testing pyramid.


## Join the Signadot Slack Community!


At Signadot, we aim to simplify testing at every stage of the software development life cycle. By joining our Slack community, you can connect with like-minded professionals, share insights and stay updated on the latest testing advancements. If you’re interested in learning more,[join us on Slack](https://signadotcommunity.slack.com/join/shared_invite/zt-1estxm8pv-qfiaNfiFFCaW~eUlXsVoEQ#/shared-invite/email) to meet our community.
