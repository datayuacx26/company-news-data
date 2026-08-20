---
schema_version: "1.0.0"
document_id: "631127539a2ac6f9a22def3279fb418f8d5d22b0c20fd37bcf46a4331a29915b"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/shifting-end-to-end-testing-left-on-microservices/"
published_at: "2024-10-11T20:30:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:8c24ed15093205f6cbf0eedc280ccca0cecc7d528fc089a6d4f14b1c3965be1b"
---

# Shifting End-to-End Testing Left on Microservices

Image from Gorodenkoff on Shutterstock.


*Originally posted on*[The New Stack](https://thenewstack.io/shifting-end-to-end-testing-left-on-microservices/) *.*


**Learn how Uber and fintech company Earnest have approached end-to-end testing that’s fast, reliable and scalable.**


Recently I took to Reddit to ask platform engineers and developers: “Who should be actually running tests and looking at the output? QA specialists or developers?” The answers surprised me! The most upvoted response was: “Developers shouldn’t have to run tests.”


When I talk about shifting testing left, I usually write from the assumption that we all agree that[developers should be getting test feedback sooner](https://www.signadot.com/blog/why-we-shift-testing-left-a-software-dev-cycle-that-doesnt-scale/) . We may not agree on the best method to reach that outcome, but I thought the goal was universal. It turns out, shifting left is still a debatable topic!


What development team leads know, what their managers know, what good CTOs know, is that the sooner you can get test feedback to developers, the faster the “inner loop” of development will be. Developers will write code, see how it works and then rewrite their code faster if they can look at the results of testing sooner.


In a microservice world, developers are often writing code that they have no way of running realistically without a test suite; interdependence means that only the most basic unit tests can run on our[microservices](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) without having other dependencies available. That means developers need to be able to run complete tests early, there should be no extra set of tests that a QA team is running before merging to staging, and end-to-end testing needs to shift left.


## Earnest and the Value of End-to-End Tests Early


At fintech company Earnest, end-to-end tests covered the critical flows through their application:


“Basically an integration test goes through the flow of any of our products and emulates a user interaction. There are tests to emulate every critical part of our flow, such as document signing. There’s a test to create a \[loan\] application, and check to see if the applicant was approved or not, whatever it’s expecting. Then a separate test that will go into an existing application, see that an offer has been made, and verify that you get a PDF and that you can sign it.”


A word on terminology: The Earnest team refers to these as “integration” tests whereas some teams would call a browser-based test that goes all the way to downloading and signing a form an “end-to-end” test. While the classical[testing pyramid](https://www.signadot.com/blog/is-the-testing-pyramid-broken/) makes a clear distinction between end-to-end tests and integration tests, the two terms often vary in their definition by organization.


Whatever term you use, if you’re talking about having a user sign in, fill out a loan application and sign a PDF, there’s no way to cover all that with unit tests. Even contract testing will be insufficient. There’s no mock you can write that successfully simulates a visual PDF signing tool.


While any team leader would agree such tests are necessary, what if I told you that Earnest lets every[developer run these tests](https://www.signadot.com/blog/the-struggle-to-test-microservices-before-merging/) at any time, and it takes just a few minutes for these tests to complete? That’s surprising. At many orgs, the end-to-end tests like these — with synthetic users clicking around and interacting with the site — take hours to complete. At Earnest, the use of extreme parallelization and sandboxing make it possible. ([Read the case study](https://www.signadot.com/case-studies/how-earnest-empowers-developers-for-early-testing/) .)


## How Uber Shifts End-to-End Testing Left


Uber realized that this early detection is essential for scaling its vast microservice architecture, especially as it operates in a fast-paced, high-availability environment. Traditional[approaches to testing](https://www.signadot.com/blog/we-need-a-new-approach-to-testing-microservices/) often fail to handle the interconnected complexity of microservices, so Uber developed the Backend Integration Testing Strategy (BITS) to address this challenge.


## Key Strategies Behind Uber’s Approach


### Infrastructure Isolation and Sandboxing


To prevent test environments from contaminating production, Uber uses isolated sandboxes. These environments isolate the traffic intended for these test versions of services, while allowing the sandbox to rely on the many microservices that don’t need to be forked. The BITS architecture includes smart routing mechanisms, tenancy-based data filtering and sandboxed Kafka integrations, ensuring tests mirror production as closely as possible while keeping things separated.


### Automated and Composable Testing Frameworks


Uber’s Composable Testing Framework (CTF) allows developers to build modular test flows. These can simulate complex scenarios like ride-sharing routes or payment processing. The flexibility of CTF reduces maintenance overhead and keeps tests aligned with real-world use cases.


### Advanced Test Management and Analytics


Uber has implemented a sophisticated test management UI that tracks test health, endpoint coverage and failure patterns. By continuously monitoring test performance, it can automatically quarantine unreliable tests, minimizing disruptions to CI/CD pipelines.


### Reliability and Speed Improvements


A common criticism of E2E testing is its fragility and slowness. Uber tackles this by running placebo tests in parallel and incorporating retry mechanisms, achieving test pass rates above 99%. This stability debunks the myth that E2E testing can’t scale in large systems.


### Collaborative Architecture


Rather than fitting into the traditional “testing pyramid,” Uber’s microservices and collaborative development style naturally led to a more comprehensive E2E approach. The success of this strategy stems from aligning testing directly with Uber’s service-oriented architecture and recognizing that cross-service interactions often need to be tested together.


### The Results


By integrating these practices, Uber reduced incidents by 71% per 1,000 code changes in 2023. This significant improvement underscores that testing isn’t just about technology; it’s also about fostering collaboration and communication across teams.


The lessons from Uber’s shift-left strategy remind us that when testing is done right, it improves both speed and quality, helping developers ship features more confidently while avoiding nasty surprises in production.


## The Right Tools To Shift Testing Left


It’s a truth universally acknowledged that E2E testing is hard with microservices.


In “[Why E2E testing will never work in Microservice Architectures](https://www.mkarkowski.com/why-e2e-testing-will-never-work-in-microservice-architectures/) ,” software consultant Michal Karkowski argues that end-to-end (E2E) testing is impractical in microservice architectures due to the complexity and variability introduced by independent service deployments. As microservices are developed and deployed autonomously, the required number of testing environments for each possible service version combination becomes unmanageable, making E2E testing inefficient and unreliable in such contexts. We need specialized tooling for testing in this environment.


Uber’s approach demonstrates that early and integrated testing isn’t just about good processes; it’s about using tools that facilitate fast, reliable and scalable testing.


[Signadot](https://www.signadot.com/) , a platform that provides developers with lightweight sandboxes for early testing, allows them to spin up microservice replicas without heavy infrastructure overhead. This approach empowers developers to test in realistic conditions sooner, catching potential issues before they reach production.
