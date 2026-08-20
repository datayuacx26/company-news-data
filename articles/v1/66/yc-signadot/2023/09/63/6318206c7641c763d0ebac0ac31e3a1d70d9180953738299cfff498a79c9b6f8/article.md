---
schema_version: "1.0.0"
document_id: "6318206c7641c763d0ebac0ac31e3a1d70d9180953738299cfff498a79c9b6f8"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/why-developers-shouldnt-write-mocks-a-guide-to-modern-testing/"
published_at: "2023-09-26T07:59:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:db35636e1111fea9ef984976634019e9d299a99941759f5f7909fca89b246591"
---

# What's the real ROI of writing Mocks?

Mocking, it’s got a long and complex history within real-world development. But is mocking required? Do we really need to write mocks?[Is mocking a joke](https://www.youtube.com/watch?v=B81U7Vunhuc&ab_channel=SaturdayNightLive) ? Let’s take a step back to the era of test-driven development.


## Everyone should always do test-driven development, except when they shouldn’t, which is most of the time.


Test-driven development was often promoted as the ‘right’ way to program, starting almost[20 years ago](https://web.archive.org/web/20141222180731/http://nparc.cisti-icist.nrc-cnrc.gc.ca/npsi/ctrl?action=shwart&index=an&req=5763742&lang=en) . I never actually followed this practice when working as a developer in high-throughput production environments. In the end I felt like the recommendation was a bit like 8 glasses of water & an hour of meditation a day: It’s easy to claim amazing benefits to practices that almost no one is able to do.


While the process of defining a test, writing a test, and then producing code to that test sounds like a way to keep clear focus as you work through a coding challenge, the reality doesn’t align with this vision. **Often the added time of writing an initial test has little clear benefit for the writer, and the test we produced isn’t useful for unit testing later.**


As our monolithic applications were replaced by clusters of microservices, the argument for TDD got more difficult, as code that passed a simple unit test was not particularly more likely to actually work when deployed to a cluster of other services.


While you might think that Test-Driven Development and the unit tests that were its starting point has passed out of favor, in fact unit tests have only increased in popularity. The reason is another aspect of our lived Microservice lives: the time it takes to deploy code and see results on our cluster.


Before we go any further, let’s define our terms


### Defining our terms: Unit Tests, Integration Tests, Mocks, and Stubs


**1. Unit Tests
** Unit tests are small, focused tests that verify the correctness of individual units or components of a software application. These tests isolate a specific piece of code, like a function or method, and check if it behaves as expected under different conditions. Unit tests are very fast, should almost always run on a developer’s workstation, often running every time source files are updated.


**2. Integration Tests
** Integration tests assess the interactions and compatibility between different components or modules within a software system. These tests ensure that various parts of the software can work together seamlessly and that data flows correctly between them. Recently, integration tests have become extremely slow to run for large microservice clusters, often not running until there’s been a full code review.


**3. Mocks
** Mocks are objects or components used in testing to simulate the behavior of real, external dependencies. They allow you to isolate the code you’re testing by providing controlled responses to interactions, helping to mimic the behavior of the actual dependencies without relying on them during testing. Mocks are usually simulating other dependencies within the product, but can also be used to simulate[third-party dependencies like payment APIs](https://www.signadot.com/blog/mocking-and-testing-3rd-party-apis-with-sandboxes/) . Mocks range from very very simple code (e.g. one that replies with whatever object you just sent it confirming it was written to a database) to very complex (for example a mock that runs a temporary DB table in memory, simulates latency, errors, and other real world scenarios).


**4. Stubs
** Stubs are similar to mocks in that they are used in testing to replace real components or services. However, unlike mocks, stubs provide predefined, hardcoded responses rather than simulating dynamic behavior. Stubs are often used when you want to isolate a piece of code and ensure it behaves predictably in a controlled environment.


### Unit tests are more important than ever


The microservice model is supposed to imply a separation of business concerns into atomized teams. Technical domains should cover a particular business concern with the team becoming intimately familiar with the business requirements and technical backround of a certain group of problems.


For these small teams, the process of going from coding on their workstation to running their code on a production cluster is a big leap from their comfort zone to an environment full of unpredictable latency, un-documented updates, and surprise side effects.


In essence, unit testing has increased in importance because later stages of integration testing have gotten significantly slower to implement. Instead of a compile>deploy process taking minutes, in large teams developers are waiting more than a quarter of an hour to see their code running within a cluster. The need, therefore, to write detailed and re-usable unit tests has increased.


## If unit tests are important, why shouldn’t we create mocks?


Mocking is the solution when your unit test would call a service that’s outside of your domain. A small snippet of code that responds in a way that credibly simulates another service, database, etc. It sounds like a necessary addition if we want to use unit tests extensively. Why do I recommend against it? Let’s start by clarifying the problem we’re trying to solve.


### Inter-dependence in services leads to a call for mocking


In microservice world, mocks have become standard advice for a complex problem related to poor separation of concerns in most organizations. Services that are controlled by separate teams and separated into separate pods can often be so closely interlinked that you can’t test one without the other. Some have mentioned, quite correctly, that this is a failure of domain-driven design: it *should* be possible to separate services, test the contracts between them, and be confident that they’ll play together perfectly.


Again, the advice at the start of the microservice revolution was consistent: If two (or more) microservices need to collaborate a lot with each other, they should probably be the same microservice. But this often isn’t possible, take a streaming news service and a user profile service: the two might communicate constantly to access and update a user’s preferences as she upvotes and downvotes news items. The two services are closely interlinked, but their actual functions are so different it’s unlikely any organization would put them both into one service.


### Mocking simulates too much


Let’s say we have a simple user profile we want to load with our microservice. Every request for a user profile includes a call to our auth service, so the natural solution is to mock that auth. It might take a little while, again we’re not writing a stub so we don’t want the response to always be “approved,” we’ll need some logic to simulate requests not being authorized. Then it’s time to test our service.


Sure enough, our updates to our service work great with our mock, and when we deploy it to staging… it fails. Why? Some possibilities:


- Our user profile service was changing userID character encoding for display on the page, and this re-encoded version was accidentally passed to the auth service.
- The auth service was updated last week to be more restrictive about whitespace in requests. What worked last week doesn’t now.
- Our redesign of the profile service is less tolerant of latency, and only works if the auth service replies in < 5ms.


In all of these cases, the failure was obvious when the code was running with a real authentication service, but would be extremely hard to detect beforehand. In order to solve all three scenarios while still using mocks, the complexity of the mock would have to increase (e.g. being more restrictive about accepted inputs, updating to reflect the recent updates to the real service, and adding Sleep() to simulate latency). Without this added complexity, we find that **mocking simulates outside services, and simulates passing unit tests, without accurately representing either** .


One proposal to resolve the issues with mocks not reflecting the real service is to have mocks be the responsibility of the team that maintains that service. Who else but the auth team could write a great mock that simulates the auth service? One issue here is that it’s not standard practice now, so most developers aren’t in the habit of writing mocks of their own service. Another issue is that these developers have already produced something that responds like their microservice was: the microservice. Once we’re adding string validation, and simulated latency to our mock, we find that we’re now writing a whole new fake service to look like our real service.


### Costs matter when contemplating mocking


The above service shows how often mocks are insufficient to offering real confidence in testing, and we get surprised that unit-tested code fails integration testing. Surely the fact that ‘sometimes mocks lead to false results’ isn’t a reason to never use them, is it?


The second factor that makes mocks (almost) always a bad idea is the cost. Mocks are not inexpensive to write and maintain. And since they often don’t do what they’re supposed to, accurately warn us of pending integration problems, it doesn’t make sense to spend our precious time writing mocks.


### What mocks can’t capture, and the hybrid that fixes it


---


Mocks are genuinely good at some things: testing negative cases, reproducing specific edge cases, and validating isolated logic with controlled inputs. The trouble starts when teams treat them as high-fidelity stand-ins for whole systems. As a microservices ecosystem grows, mocks alone cannot:


- **Capture real-world interactions.** Dependency chains and network effects are hard to simulate faithfully.
- **Adapt as APIs evolve.** Every change to a real service means another mock to update, and the drift between the two is exactly where bugs hide.
- **Surface integration issues early.** Without real dependencies, most integration failures stay invisible until they reach staging.


The fix is not to ban mocks, it is to stop over-indexing on them. Use mocks for edge cases and controlled inputs, and use a real environment to validate integration flows, complex API behavior, and performance against actual dependencies. That hybrid is what “shift left” should have meant all along: meaningful feedback earlier, not more simulation.


Historically the catch was that real, production-like environments were expensive to stand up, operationally complex, and contended when shared across teams. That is the problem sandboxes solve. Each change gets its own isolated slice of a shared baseline, so you get real-dependency feedback on every pull request without duplicating environments or queuing for staging.


Function purity and testing


In previous sections it was strongly implied that unit tests and mocks go hand in hand. What I’d submit is that they shouldn’t. Think about the difference between pure and impure functions:


Side effects are the definition of an impure function


Pure functions only consult their internal logic, and just have inputs and outputs. Impure functions either emit side effects or bring in outside state, these side effects are, generally, where mocks come in. And the thing is **Pure functions can easily be tested with unit tests, and impure functions require so much support to run unit tests, it’s not worth doing.**


Impure functions need to wait for integration testing to be tested. What do you do if all your functions have side effects, is unit testing impossible? Not at all! you need to separate out the pure from the impure functions \[insert a joke here about panning for gold\]


This function can’t easily be unit tested, but we can break it up like:


‍


‍


This function is, much more obviously, one that really needs integration testing. A separate function can be a pure function and is easy to unit test:


‍


‍


The result is a more testable code base, and has the side benefit of producing better code with functions whose overall purpose is more clearly defined.


This is the portion we can unit test


‍


In our contrived example we could always separate off impure functions completely, but what about something like an auth service? That might be called with every single action, how can we get this working? For that, it’s fine to use a stub.


## Stubs are fine


As we see more complex testing situations pushing the line between a mock and the actual service, I’ll remind you that it always makes sense to write a stub: an entirely static return is fine for unit testing, its consistent, and most importantly developers ***don’t*** expect a stub to simulate any processing that happens in our dependency. If your testing uses an authentication that just always returns authorized , then it’s always clear that testing *with* the dependency is a whole separate step, and any data parsing errors are more predictable.


This distinction matters when we discuss dependencies like datastores: if our test works fine with a static return value from a get then a stub can cover the use case. But if a test requires multiple reads and writes with state, now we’re in mock territory and it doesn’t make sense for a modern developer to spend that amount of time creating something that fundamentally only gives us a false sense of security.


So what are we to do when our stubs no longer cut it and a mock feels like the only way to have accurate testing? There are a few options.


### Solution 1. Local Replication


By this point, it’s possible some readers, like know-it-alls watching Jeopardy, have been screaming at the screen ‘just use local replication!’


And folks, yes, correct, local replication is the most common solution. But really if we were even considering writing mocks, there’s probably some reason that local replication isn’t practical. For most of us, it’s that our dependencies will no longer run easily on our laptops. For smaller clusters, the issue is more often that our dependencies include datastores with a state of some kind. By the time we’re rewriting local deployment scripts to populate our local replica of a datastore, this has stopped feeling like an easy solution.


Mocks shouldn’t be used by Go devs for unit tests. Anything involving a replica of a service, even if it’s a highly attenuated replica, is really an integration test. Integration testing, and related acceptance testing, can happen in a lot of ways, and local replicas are part of the solution.


### limitations of local replication


Much like Mocks falling out of sync with the actual service, local replicas also imply a significant workload to keep these replicas up to date. Even if a loyal force of platform engineers keeps a dependency list updated, developers will still need to grab the updates before they start working every day, which in large teams can take an hour or more to download, build, and locally deploy their dependencies.


Here a solution may be to create a shared environment that is kept up to date constantly, and is always available for curious developers. To do this right, we’ll need to let developers try out changes without impacting others using the cluster, and we want to make the whole process easy and fast.


## Solution 2. Shared resources with Isolation During Development and Testing


From[Uber](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/) to[Razorpay](https://www.signadot.com/blog/how-lyft-and-razorpay-share-development-environments-with-hundreds-of-devs/) , large enterprise teams have figured out ways to make integration testing almost as fast and easy as unit testing. While unit tests still cover pure functions, a shared staging environment with some kind of isolation for systems under test, can make Integration testing quick and easy for impure functions that can’t be tested without testing their side effects.


## What this means now that AI is writing the tests too


Since this was written, something changed that makes the case against over-mocking sharper, not softer. A large share of code, and the tests that go with it, is now written by AI coding agents like Claude Code, Codex, and Cursor. Ask an agent to make a failing test pass and it will often do the most direct thing available: mock the dependency that is getting in the way. The test goes green, the pull request looks finished, and the code has never once run against the real service it depends on.


That is the false sense of security described above, now produced at machine speed. An agent optimizes for the signal it is given. If the signal is “the mocked test passes,” it will happily hand you code that breaks the moment it meets a real auth service, a real schema, or real network latency. Mocks no longer just hide integration bugs from people. They hide them from the agent that could have caught and fixed them in the first place.


The way to give an agent honest signal is to let it test against real dependencies. A per-pull-request sandbox does exactly that: the agent runs only the service it changed against a live, production-like baseline, so an integration failure shows up while the agent is still in the loop and able to fix it. On top of that environment, a validation layer turns the result into something an agent can act on. Signadot’s[SmartTests](https://www.signadot.com/docs/concepts/smart-tests-and-jobs) compare the real baseline against the change and flag meaningful behavioral differences,[Jobs](https://www.signadot.com/docs/reference/jobs) run the test suites you already trust against the sandbox, and[Plans](https://www.signadot.com/docs/reference/plans) tie them into one repeatable check. The longer version of that argument is in[validating AI-generated code on Kubernetes](https://www.signadot.com/validate-ai-generated-code-kubernetes/) .


None of this means mocks are useless. As before, they are the right tool for edge cases and inputs that are hard to reproduce any other way. It means the default should flip: real environments for integration, mocks only where they genuinely earn their keep. That was good advice when humans wrote the code. Now that agents write most of it, it is the difference between a pull request that looks tested and one that actually is.


## Conclusion: We must make integration tests faster and more available.


As integration tests got slower and harder to run, we’ve put more and more pressure on our developers to simulate the other services in their cluster. This is a timesink that doesn’t reliably predict whether code will run right on production. The solution is to move integration tests earlier, and make them more available for developers. Right now we’re penalizing developers for ‘breaking staging,’ and otherwise writing code that fails at the integration test stage. Failing at integration testing should be as common as breaking unit tests.


Not all testing is created equal, and the type of testing to employ should be context-specific. For pure functions, unit tests are straightforward and effective. For impure functions, which often involve external dependencies and side effects, the road is less clear. Mocks may not be the answer, but neither is avoiding testing altogether.


Local replication and shared resources with isolation during development and testing emerge as viable alternatives. These approaches offer a more reliable and realistic testing environment compared to mocks. They also align well with the modern microservice ethos of simplicity and efficiency.


The goal is to achieve a balance between thorough testing and development velocity. Spending the time now to evaluate our testing and development framework, and the architecture that supports it, is the pragmatic path forward for developers navigating the intricate world of software testing.


‍
