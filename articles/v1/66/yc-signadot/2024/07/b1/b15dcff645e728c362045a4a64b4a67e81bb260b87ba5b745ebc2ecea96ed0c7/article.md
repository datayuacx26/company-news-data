---
schema_version: "1.0.0"
document_id: "b15dcff645e728c362045a4a64b4a67e81bb260b87ba5b745ebc2ecea96ed0c7"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/multitenancy-is-fundamental-to-shift-left-testing/"
published_at: "2024-07-05T08:41:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:182b87ef19c2712b105aaabb6e87feadc2311b78a5cc59950e3a145e58db62f5"
---

# Multitenancy Is Fundamental to Shift-Left Testing

*Originally posted on*[The New Stack](https://thenewstack.io/can-tests-only-be-fast-or-good/) *.*


There was a time when smoke tests were literal. When every startup started by buying their own racks of server hardware, the initial smoke test of a new hardware configuration involved a technician standing in the server closet to make sure smoke didn’t start pouring out of any of the components.


Then, as now, the process of testing software was a balance between the complexity of testing and speed.[QA teams](https://www.signadot.com/blog/who-should-run-tests-on-the-future-of-qa/) that confidently built massive suites of unit tests, integration tests and end-to-end tests ended up with test run times in hours or days. As such, this thorough testing became more and more infrequent. The basic tests that every developer runs while writing code are by definition limited in scope, and their run time is always tightly controlled, as a test suite that takes too long to run after every code change will irreparably damage the developer’s workflow.


But the choice between test quality and speed is a false one. It’s possible to create testing systems that are both fast and high quality.


## **Testing Phases and Quality — The False Dichotomy**


Testing’s various phases are sometimes described as a pyramid or triangle: As we approach production, the tests become more thorough with slower feedback. The relationship is something like the diagram below:


*Source: Signadot*


The lower environments like local workstation unit tests and contract tests run as part of continuous integration (CI) and offer faster feedback but sacrifice quality — they are not a good replica of production. As you go to higher environments you get higher-quality feedback, but the speed of feedback is quite slow.


Largely, the right side of this flow makes sense: Right before a production release, we should be running tests that take quite a bit longer to complete, offer more feedback and find (almost) every problem or possible degradation of performance. Things like synthetic user testing and load testing are slow by definition. It’s not possible to run tests like those every time you make a small code change as a developer, before you even submit a pull request.


However, what forces the tests run on the left part of this diagram to be “low quality”? Why should the tests we run 10 times a day offer only partial evidence that our code actually works? Moreover, I feel that if you ask developers whether the tests on their local workstation are reliable, their experience with such tests have gotten *worse* in the last five years. Why are local tests so low in quality?


## **Testing Solutions at Multiple Scales**


In a recent Lean Coffee chat for platform engineers, more than one engineering manager described a situation where many of the middle stages of testing were no longer occurring. Here are a few quotes from that discussion (these quotes are unsourced by agreement with the organizers):


“\[It would be great to have\] contract testing, but those tests would be extremely expensive to maintain in our cluster.” Keep in mind that as[microservices](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) proliferate, contract testing gets more costly to implement and difficult to maintain.


“Right now testing consists of unit testing, which are each dev’s own responsibility, and then something like synthetics testing as the final stage right before going to staging.”


In this case the reference to “synthetics” is a highly[automated version of end-to-end testing](https://www.signadot.com/blog/shifting-end-to-end-testing-left-on-microservices/) , where a browser or synthetic user makes a complete request on a highly accurate copy of the production cluster. The question of how we make an accurate environment to run these end-to-end tests brings us to a solution that many try with less-than-satisfactory outcomes.


## **A Solution That’s Neither Good nor Cheap**


The more general version of this tradeoff of quality and speed adds cost as a factor. Traditionally the “cost” of testing was wrapped up in the time spent by engineers. However our modern microservice environments have thrown up a solution that manages to be expensive in time and infrastructure costs as well: the duplicated developer environment.


As soon as the staging cluster got too large and complex to run on a developer’s workstation, a proposed solution was to create a copy of this cluster for developers to test on. Since developers’ changes to this cluster would often clash, this cluster would be further replicated for every developer who wanted to test changes.


This approach has several significant drawbacks:


- Running multiple environments all the time, just to wait for someone to need them for testing, will incur significant infrastructure costs.
- These multiple clusters will inevitably fall out of date with staging and production environments, leading to lower quality testing.
- You either shut down these clusters when they are not in use, or have them refresh their dependencies each time before they run tests.


## **Quality Matters in Testing**


While we want tests that run quickly and efficiently, I must bring back our focus, first and foremost, to the quality of tests. The change in the last decade in the complexity of our architectures has led to testing performed by developers that is significantly less accurate than before.


Speaking to a group of developers last month at a meetup, I asked if they had ever shipped code to staging that they had no idea would work. More than half the room responded saying “yes.” That’s disturbing. However we change our process, it must produce higher-quality testing information. Having high-quality, production-like data will provide high-quality testing signals and greatly increase testing effectiveness.


### **The Rise of the Third-Party Service and the Challenge of Test Quality**


A huge issue with any solution that creates many environments for developers or other testers is the specter of third-party services. All these atomized testing environments can’t provide any kind of accurate test version of a third-party service. While a service like Stripe has a way to simulate transactions for testing, these don’t scale out to many “test” clients. Third-party services are another reason we want to establish high-quality environments that are shared between engineers.


## **Multitenancy in Test Environments**


Rather than continuing to expect large middle phases of testing, like contract testing, teams like[Uber and DoorDash](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/) have implemented shared testing environments where developers can run true integration and end-to-end tests early in their development cycle.


### **Multitenancy Is a Powerful Idea With Many Examples**


Various systems, including CPUs, operating systems and applications, progress toward multitenancy to make more efficient use of hardware and infrastructure. CPUs use[multithreading](https://en.wikipedia.org/wiki/Multithreading_(computer_architecture)) to more efficiently use other systems available to the central processor. In the operations world, virtual machines are a multitenant solution on top of physical compute hardware, and even applications support concurrent threads of execution, another form of multitenancy. In the world of cloud architecture, it’s rare that any compute system actually uses dedicated hardware spaces for tenants. For seamless scaling and efficiencies of scale, multitenancy is standard.


### **A Multitenant, Highly Accurate Shared Testing Environment**


To share a staging or preproduction cluster with a large dev team, additional work with a tool like Signadot is required to make sure that developers’ tests and experiments don’t collide with each other. Sometimes called “[sandboxes](https://www.signadot.com/blog/transforming-kubernetes-developer-environments-the-shift-to-request-level-isolation/) ,” a request isolation system can make sure that only the right requests are passed to the services running a test.


## **Learn More With Signadot**


At Signadot we’re pursuing better ways to perform testing on a shared, multi-tenant environment. We’ve got a few case studies from[enterprise users like Brex](https://www.signadot.com/case-studies/brex-uses-signadot-to-scale-developer-testing-across-100s-of-engineers/) , and articles that study how enterprise teams like[Uber](https://www.signadot.com/blog/large-engineering-teams-testing-on-k8s/) and[Eventbrite](https://www.signadot.com/blog/prezi-eventbrite-teams-testing-on-kubernetes/) solve the challenge of high-quality shared testing environments.
