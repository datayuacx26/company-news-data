---
schema_version: "1.0.0"
document_id: "cc8522f4fb5184182ab46327b78bd190c7137ac6ae1a7017b30d2c1a52779ab8"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/why-shift-testing-left-part-2-qa-does-more-after-devs-run-tests/"
published_at: "2024-05-27T20:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:648221fc3ac64a6d14da2282a97facaade7a4375dd3941d0d2c29e47ac825565"
---

# Why Shift Testing Left Part 2: QA Does More After Devs Run Tests

*Originally posted on*[TheNewStack](https://thenewstack.io/why-shift-testing-left-part-2-qa-does-more-after-devs-run-tests/) *. This is the second of two parts. Read*[Part 1](https://www.signadot.com/blog/why-we-shift-testing-left-a-software-dev-cycle-that-doesnt-scale/) *.*


In my previous article, I talked about how the software development life cycle (SDLC) hasn’t scaled as we’ve moved over to[microservice architectures](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) . The result has been a more waterfall-like process where everyone is waiting on a QA process, and feedback only gets to developers slowly. The proposed solution is to “shift left:” empower more developers to run and maintain tests.


## QA Still Has a Role


It’s reasonable to ask[whether shifting testing left](https://www.signadot.com/blog/we-need-a-new-approach-to-testing-microservices/) means that QA engineers will be out of a job. Nothing could be further from the truth. When highly technical experts on your team have part of their responsibilities shifted because of design changes, the result is that those experts end up doing more, not less, with higher expectations for what they can enable.


### **“Who Here Still Configures Email Servers?”**


A few years ago I was listening to a talk by Kelsey Hightower, where he advocated for moving to serverless and public cloud-managed hosting tools over directly managed virtual machines. Someone in the audience asked: “Do you want sysadmins to lose their jobs?”


Kelsey had an insightful answer. “Everyone in this room,” he said, “raise your hand if you used to configure email servers, their certificates and authentication.”


About 80% of the room raised their hands.


“Now keep your hand up if you still do work maintaining this configuration for email servers.”


No one kept their hand up. I’m paraphrasing here but this was the crux of what he said next:


“All of you have a ton of experience administering systems. And when a service comes along that removes some of that work, the normal result isn’t that you all work less; it’s that we expect more. Instead of requesting a new server be spun up and waiting at least a day for you to deploy it, now we expect virtual machines to appear automatically with configuration changes. Don’t get me started on automatic cluster scaling.”


The point was well taken: when work by a qualified team is eliminated, the result is higher expectations, not reduced workload. It’s the arbitrary heavy lifting that gets eliminated, not the people.


Think about what happened to many sysadmin workers in the past few years: where once they were charged with manually setting up and managing dozens of servers, now the same people orchestrate hundreds or thousands of containers. While no one configures email servers anymore, our expectations for what those same people can do have only increased.


### **QA As Strategic Leader**


The goal of[shifting testing left](https://www.signadot.com/blog/why-testing-must-shift-left-for-microservices/) isn’t to remove QA, it’s to let QA be a strategic leader that helps standardize culture around testing, maintains automation so things can run smoothly, and picks frameworks and standards for testing so that results can be shared. Think about a QA team that can:


- Help select and keep updated the testing frameworks for different areas of the application
- Define testing strategy and which new features require new tests
- Monitor “cruft” like longstanding failing tests or tests with poor feedback and work on the technical debt that will improve every developer’s experience
- Add test coverage for known edge cases, security risks and privacy failures to make sure your code stays compliant.


Without constant failures to merge, QA can focus on finding emergent behavior and regressions that automated testing would have missed. QA can also take the time to learn the types of testing that dev teams most need, define practices that make test writing take the least time and optimize test runners to make test execution take less time, speeding up the developer’s inner feedback loop.


### **Embedded QA: Always Worth a Thought**


At my first tech job, every language team had a QA specialist and a documentation specialist. I remember this because the only exceptions were Ruby and Java. Ruby had no QA because “Rails doesn’t require QA.” Java had no tech writer because “Java is self-documenting.” What fools we were! But the basic idea of embedding QA throughout your teams is still a powerful one.


All those years ago the QA specialists used some automation but were also masters of exercising their product manually to find failures. In retrospect, we underestimated the necessity of QA and documentation across all languages. The belief that Ruby on Rails didn’t require QA or that Java was self-documenting seems naive now.


These days it’s even more crucial to consider embedded QA, specifically software development engineers in test (SDETs). The idea is to embed SDETs into the product teams that can own automation. SDETs are solely focused on writing automated tests and by being embedded in each product team, they are also domain-aware and can write meaningful tests.


## Conclusion: Shifting Left Is a Return to the Good Old Days


The motivation for shifting left primarily stems from the desire to[enhance development velocity](https://www.signadot.com/blog/improve-developer-velocity-by-decentralizing-testing/) and reduce the frequency of bugs reaching production.


For companies experiencing growth, the complexity of systems, especially with microservices, adds another layer of challenge. The traditional method of developers writing code and relying on QA to catch issues later does not scale efficiently as the company grows.


The option to shift testing left offers a solution by empowering developers to perform more comprehensive tests — including integration and even end-to-end tests — early in the development process. This change reinstates a sense of ownership and satisfaction among developers as they can verify their work immediately and submit pull requests with more confidence.


This shift does not diminish the role of QA, but transforms it. QA becomes more about leading strategy, defining and maintaining automation frameworks and ensuring quality throughout the development process. Shifting testing left is not just a return to more efficient development practices reminiscent of earlier times when systems were simpler and could be tested more straightforwardly, but it’s a necessary adaptation to the complexities of modern software development. This shift aims to improve the speed and quality of software releases, essential for companies looking to scale effectively without compromising on quality.


### Connect with Signadot


At Signadot, we’re trying to build the tools that your team needs to let[every developer test](https://www.signadot.com/blog/multitenancy-is-fundamental-to-shift-left-testing/) their code in a highly accurate cluster with fewer surprises at deployment time.[Check out how we enable high-fidelity testing](https://www.signadot.com/) .
