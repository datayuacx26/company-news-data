---
schema_version: "1.0.0"
document_id: "bbff4930bbc50885614983ee7bf11b053c58d551d3fc875e65e1155fa23713f9"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/the-struggle-for-microservice-integration-testing/"
published_at: "2023-11-10T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:e448f07749f0245a05a47891a3fec4530a90a0ee557bcd81d1e93a81daa47439"
---

# The Struggle for Microservice Integration Testing

When I began my career in development, I remember staging my first production deployment. I was horrified when automated testing returned 232 failing tests on my pull requests. I messaged my mentor right away that a number of tests were failing. Her question surprised me: “How many tests failed?” I felt embarrassed but admitted it was over 200. “How many exactly?” she asked. More confused than ever, I said it was 232. “Oh that’s fine,” she said, “a bunch of tests are broken, so we just check that it’s always exactly 232 that fail. That means everything is fine.”


Later I realized that our automated testing was essentially bankrupt. It produced unreliable output so often that all developers had gotten used to manually checking that everything actually worked on staging before pushing out changes. This manual testing, or “ClickOps,” was the only real way[we knew that our code was working](https://www.signadot.com/blog/why-we-shift-testing-left-a-software-dev-cycle-that-doesnt-scale/) before it hit production. What we were doing, essentially, was manual integration testing. Our unit tests couldn’t simulate a request moving between multiple services. For where integration testing fits in the full lifecycle, see our complete guide to[microservices testing](https://www.signadot.com/the-complete-guide-to-microservices-testing-from-local-development-to-production/) .


Let’s look at the challenges of automated integration testing for modern microservice architectures and consider a few possible solutions.


## Integration Testing Is a Key Concern for Microservices


[Integration testing](https://www.signadot.com/the-complete-guide-to-microservices-testing-from-local-development-to-production/) is crucial for microservices architectures. It validates the interactions between different services and components, and you can’t successfully run a large architecture of isolated microservices without integration testing. In a microservices setup, each service is designed to perform a specific function and often relies on other services to fulfill a complete user request. While unit tests ensure that individual services function as expected in isolation, they don’t test the system’s behavior when services communicate with each other. Integration tests fill this gap by simulating real-world scenarios where multiple services interact, helping to catch issues like data inconsistencies, network latency and fault tolerance early in the development cycle.


Integration testing provides a safety net for[CI/CD pipelines](https://www.signadot.com/blog/your-ci-cd-pipeline-wasnt-built-for-microservices/) . Without comprehensive integration tests, it’s easy for automated deployments to introduce regressions that affect the system’s overall behavior. By automating these tests, you can ensure that new code changes don’t disrupt existing functionalities and that the system remains robust and scalable. This is particularly important in[microservices architectures](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) where services are often developed by different teams and can be deployed independently.


## Integration Testing Is an ‘Unsolved’ Problem


On a recent reddit thread, user r/[brangtown](https://www.reddit.com/r/softwaretesting/comments/160dqlg/microservices_automated_integration_testing/) asks how others are handling integration testing between microservices:


*“I’m curious as to how others are handling integration testing of microservices. For example, I’d like to be able to run some API tests to ensure service A, B and C still happily talk to each other. Someone makes a change to service A, the pipeline builds a new container image and deploys it to the k8s cluster. How can I then ensure the release container images for service B and service C are deployed?”*


The problem starts off sounding fairly technical and specific, but I believe the problem is quite general: What’s an automatic path to testing that changes to one service haven’t introduced blocking problems with the interaction between services?


## Integration Testing Presents a Problem of Scale


While your organization is small, this shouldn’t be a significant problem: You can check in with your teams manually, say via Slack, to ask if they’ve got updates to their service that should be tested together. You might even run tests manually (ClickOps) just to make sure things are working as expected.


As your team expands, though, the shared environment used before release (often called test or staging) will start to get crowded, and multiple versions of other services running on the cluster can make automated integration tests unreliable. If a new version of service A is being tested, the best case would be to have the release version of services B and C deployed at the same time for integration testing.


## More Scale Issues on the Horizon


In the scenario above, the problem is the chance that two dependent services (B and C) will have a version deployed to the test environment that won’t pass integration tests. On a moderate developer team, it will be hard to coordinate changes such that integration tests always pass, and we don’t want to get false alarms because an unfinished version of service B isn’t working the way the release version of service B does.


While this problem would be solved by deploying release versions of services B and C for integration testing, this strategy can cause more problems as we scale: As we sync up our testing environment with release versions of all our services, we will effectively block other teams from using said environment. Again this isn’t a problem with just a few dozen developers, who would have to wait while automated testing runs to test their release candidates.


## Solution 1: Contract Testing


Contract testing offers a focused approach to verifying interactions between microservices, serving as an alternative or complement to automated integration testing. In contract testing, each service defines a “contract” that outlines the expectations for both its requests and responses. These contracts act as a formal agreement between consumer and provider services, specifying what each can expect from the other. By validating that both parties adhere to the contract, you can achieve a high level of confidence that the services will interact correctly when deployed. This method is generally faster and less resource-intensive than full-scale integration tests, as it isolates the scope of testing to the contract itself rather than the entire system.


The advantage of contract testing lies in its modularity and efficiency. When a service is updated, you only need to run contract tests related to that specific service, rather than executing a comprehensive suite of integration tests. This makes it easier to identify and fix issues at the service level before they propagate into the larger system. Additionally, contract tests can be versioned along with the service contracts, allowing for better governance and backward compatibility. This is particularly useful in a microservices architecture where services may be developed and deployed independently by different teams.


Contract testing has one extreme drawback, which consigns it to ideas like[full-time pair programming](https://www.reddit.com/r/ExperiencedDevs/comments/10rgn4w/does_anyone_find_full_time_pair_programming/) and[Holacracy](https://en.wikipedia.org/wiki/Holacracy) : It promises advantages, but no one has been able to make it work at scale. There are very, very few talks from large enterprise teams about how they use contract testing successfully across their product teams. The reason, doubtless, is that it requires a change to all developers’ work processes, as they need to both follow and update their service’s contracts extremely closely. The expense of creating and maintaining contracts is borne by developers, when it’s often operational engineers who see the benefits in modular and reliable testing.


## Solution 2: Multiple Environments


Reddit user r/[asmodeanreborn](https://www.reddit.com/r/softwaretesting/comments/160dqlg/comment/jxm29v9/?utm_source=share&utm_medium=web2x&context=3) does a great writeup of one solution, involving multiple testing/staging environments. I’m going to quote their response at length since it gives a very nice tour of the needs for, and solutions to, integration testing woes on a microservice architecture.


*“We have a lot of Go microservices in addition to our other products (which are written in .NET, React, React Native, and/or Angular depending). Whenever we make changes, we manually deploy them to one of our development or test domains (of which there are about a dozen).”*


This number is critical: The solution of multiple environments is workable as long as the number of environments is tens not hundreds (and each environment is a manageable size). This low number makes it possible to make sure that updates are kept up to date with the versions of other services that our service needs. As indicated in the next paragraph:


*“Each domain has its own data, its own set of microservices, and its own versions of all the products. Each product has its own set of tests, from Unit Tests to Integration, Component, and E2E \[end-to-end\] tests (depending on the product, obviously). The tests that need an environment to run in are run whenever a deployment happens to one or more products/services.”*


User r/asmodeanreborn makes clear why this kind of testing is so necessary: There are always surprises, big or small, at the integration testing stage:


*“This means that if there’s a messup in gateway configs, gRPC settings, or just newly introduced issues, they’re usually found fairly early and problems with a deployment only affect a very limited number of Developers/QA.”*


Testing of integration can still remain a manual process at this scale: A verification that most routes work as expected (often as simple as checking for 200 status codes) along with manual testing can show that services are integrating as expected.


*“We still don’t have many ‘good’ API tests (outside of verifying appropriate status codes) and generally handle endpoint updates/additions with manual testing. After passing testing and stakeholder reviews, we typically let API updates ‘bake in’ on multiple environments before going to production with them too. The reason this is even feasible is that we rarely touch them, so we can afford to release them slowly.”*


This solution lacks extremely clear versioning of API updates, preferring to let changes happen gradually as multiple environments are updated. As the size and complexity of the project expands, one limitation will be testing multiple service updates at the same time: Each team’s test environment is isolated so updated versions of other services would have to be manually added, with the chance of integration surprises.


## A New Solution: Integration Testing at the PR Phase


The wholesale replication of environments for integration testing does make the process of integration testing easier, but entails a large amount of new operational work to keep multiple environment replicas up to date. What if there were a way to do integration testing much earlier in the development cycle? If we could deploy every pull request (PR) into our shared development cluster, while also isolating this new code, it should be possible to run automated integration tests against our updated service without interrupting the service of the shared cluster for other teams.


Signadot’s solution is request-level isolation. When developers create a pull request, a small “sandbox” is created that can be hosted on the same cluster as the shared development environment. With a service mesh, requests can be tightly scoped to go only to this sandbox as part of the testing process. Each new sandbox has exposed URLs for automated testing. This means that instead of waiting for a penultimate testing phase on staging, or contract testing, or creating a whole replica environment, we can test every single PR for how it integrates with the entire service.


By working on a large shared “baseline environment” for the majority of our services, we reduce operational load since only one shared environment needs to be kept up with every update. We also reduce infrastructure costs compared to multiple environments.


The benefits of request-level isolation are manifold. It enables faster, scalable testing by setting up “sandboxes” in seconds, allowing rapid testing against all dependencies. This high-fidelity environment eliminates the need for mocks or fakes, providing developers with the confidence to test changes in staging or production before merging. It also offers a collaborative environment at scale without the high infrastructure and maintenance costs associated with traditional namespace or cluster-based approaches. By isolating changes made by one developer from another, it prevents issues of cross-dependency and unpredictable staging environments, making it a robust solution for integration testing in complex microservices architectures.


## Conclusion: Navigating Integration Testing


Integration testing is a necessary tool for modern microservice architecture. Further, this testing can and should be automated, not requiring “ClickOps” to find out if everything works. While traditional methods like contract testing and multiple environments offer some solutions, they come with their own sets of limitations. Contract testing, although efficient, requires a cultural shift in development practices and has yet to prove itself at scale. Multiple environments, on the other hand, can become cumbersome and expensive to maintain as the number of services and teams grows. Newer approaches like request-level isolation offer a promising avenue for scalable, efficient integration testing. Allowing for high-fidelity testing environments at the pull request stage minimizes operational overhead and maximizes developer confidence, making it a strong candidate for future-proofing integration testing in microservices architectures.


The struggle for effective integration testing in a microservices environment is far from over, but emerging solutions are showing promise. The key is to find a strategy that not only ensures robust testing but also scales efficiently with the growing complexity of services and teams. Whether you’re a developer or an operations engineer, keeping an eye on these advancements can help you make informed decisions that contribute to more resilient, efficient and maintainable systems.
