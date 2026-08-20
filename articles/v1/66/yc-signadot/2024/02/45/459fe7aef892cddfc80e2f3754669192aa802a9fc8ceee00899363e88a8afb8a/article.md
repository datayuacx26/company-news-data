---
schema_version: "1.0.0"
document_id: "459fe7aef892cddfc80e2f3754669192aa802a9fc8ceee00899363e88a8afb8a"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/why-testing-must-shift-left-for-microservices/"
published_at: "2024-02-24T23:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:0a8747a1a3382946f35f94cc17ed47ed3c2fae0993c9b1f61f2c6f3c491785e2"
---

# Why Testing Must Shift Left for Microservices

‍_Integrating testing earlier in software development finds bugs earlier, accelerates feedback loops and speeds deployment to production._


What’s the best path to releasing code? A process with no spikes, no putting out fires, no desperate rush to add a quick feature to meet an enterprise customer’s requirements? When everything is going right, the process looks like this:


A decade ago, project managers derided waterfall implementations of the software development lifecycle (SDLC), where phases were rigidly defined, work on the planning phase never overlapped with development and testing started only after development ended. This fixed process meant that releases were infrequent, and it took a long time to get user feedback. Waterfalls were particularly ill-suited to software delivered over the internet, where agile methodologies could release software daily and reflect user feedback within a couple of weeks.


While agile methodologies allow these phases to overlap and emphasize delivery speed, the phases are still waterfall-based, and traditional methods of developing, building and testing are not well suited to the modern microservices-based environment.


## Two Major Problems with Testing Today


Although more and more workloads are moving to[microservices](https://www.signadot.com/blog/how-microservices-communicate-with-each-other/) , testing has failed to keep up with modern development needs. Here are two reasons why.


### QA Is Supposed to Find Regressions, Not Regress to the Waterfall Era


While agile methodologies are closely associated with the rise of online software delivery, another component that made waterfalls obsolete was automation and democratization of quality assurance (QA). With automated testing and QA more integrated with development teams, it’s unusual for testing to wait for development to be complete. The modern process defines many fine gradations of testing, from unit to end-to-end testing, with constant feedback as developers write code and connect services.


Microservices have somewhat broken this paradigm, reopening the door to a waterfall world. Broadly, the issue is interdependence. Microservices are[so reliant on other services](https://www.signadot.com/blog/the-struggle-for-microservice-integration-testing/) that it’s very hard to get an accurate testing picture before your service is deployed and interacting with our other components and third-party APIs. Often, a QA or operations team is the first to find serious issues with microservices code.


The result of this broken paradigm is that feedback comes in very late in the cycle and requires taking a release back to the earliest phases of development. While this sometimes happens after code is in production, too often, initial deployments to testing fail to catch problems that show up in later stages, or final canary tests find integration problems that should have shown up far earlier in the process. The real process looks more like this:


The most common solution offered for these issues is to build unit tests, stubs and mocks to simulate all the other components, but this strategy is rarely fully successful. A test suite that can simulate a complex cluster either requires QA to be highly sophisticated with every service in the stack or that each team is willing to devote serious time to maintaining tests on their service *and* accurately simulating the other services.


### Testing Is Too Slow for Developers


When trying to simulate an entire cluster for testing, the result is unacceptably slow. As you have to run the entire test suite on a testing environment, it can take from 20 minutes to a few hours to run all the tests and get results. Even 10 or 20 minutes is long enough that developers won’t sit and wait for all tests to run several times during the day. It’s generally acknowledged that developers don’t frequently run integration testing, where the updated service is working with the rest of your cluster; instead, they wait to run it later in the deployment lifecycle.


Since many bugs are discovered late in the deployment cycle, there’s another process issue that feels reminiscent of the waterfall days: When an engineer on another team discovers a bug, the process of diagnosing, reporting and fixing the problem becomes cumbersome. Operations and QA engineers are tasked with filing bug reports for every integration issue, and developers are asked to fix problems out of band.


## Shift Left to Fix Testing and Development


To fix the process of developing and testing code,[shift left](https://www.signadot.com/blog/shift-left-testing-in-kubernetes/) : Test code earlier in the cycle, and deliver feedback directly to developers. Shifting left is a cultural and practice shift, but it also includes technical changes to how a shared testing environment is set up.


### Make Smaller Changes More Frequently


In an ideal SDLC for microservices, the focus is on integrating testing early and often, starting right from the development phase. This approach emphasizes the importance of small, incremental code changes. By keeping changes limited in scope, developers can more easily understand and test the impact of their modifications. This granularity not only speeds up the verification process but also makes testing more precise.


In this model, developers take ownership of both the development and testing of their code. This ownership clarifies responsibilities and makes quality a priority from the outset. The approach scales effectively[across engineering teams](https://www.signadot.com/blog/how-to-be-an-effective-platform-engineering-team/) , as each team or developer can work independently on their respective services or features, thereby reducing dependencies. While this is great advice, it can feel hard to implement in the current development environment: If the process of releasing code to a shared testing cluster takes too much time, it doesn’t seem feasible to test small incremental changes. It’s better to implement a shared testing environment where developers can test out small changes.


### Get Faster Feedback


The feedback loop in this model is fast. Since developers test as they go, many potential issues are addressed immediately, often before they are identified as bugs in a traditional sense. The difference between finding bugs as a user and finding them as a developer is massive: When an operations or site reliability engineer (SRE) finds a problem, they need to find the engineer who released the code, describe the problem they’re seeing and present some steps to replicate the issue. If, instead, the original developer finds the problem, they can cut out all those steps by looking at the output, finding the cause and starting on a fix. This proactive approach to quality reduces the number of bugs that need to be filed and addressed later in the development cycle.


Culturally, this SDLC model fosters a culture of CI/CD, where code changes are integrated, tested and delivered rapidly and reliably. This not only accelerates the development process but also enhances the overall quality of the software. Though CI means “continuous integration,” in the context of microservices, CI tools optimally provide *continuous testing* to let developers know early the real-world issues they’ll face when trying to deploy microservices code.


### Space for Testing, with Integration


Integrating a system for previewing code changes is a key component because it allows immediate feedback on how changes will behave in a live environment. Such previews are invaluable for developers as well as other stakeholders such as project managers and QA teams. The[technical challenges are significant](https://www.signadot.com/blog/transforming-kubernetes-developer-environments-the-shift-to-request-level-isolation/) , and there’s no “drop-in” solution to create a very accurate copy of production where every developer can test frequent changes.


In brief, the basic requirements for any such system are:


- A realistic copy of the production environment, with all of the required dependencies and the many microservices maintained by other teams.
- An easy path to deploy new, small code changes to this shared environment.
- A way to prevent collisions so that your experimental code deployed to your service won’t interrupt another developer’s performance of the cluster.


In general, solutions that promise to stand up a copy of your entire cluster only when it’s time to test are not satisfactory. Instead, developers need to make small, incremental changes, sometimes deploying more than once a day. The time it takes to stand up a whole cluster once things get really complex will disincentivize the goal of shifting left.


## Request Isolation for Shifting Left


A fast and accurate testing environment for developers must be native to the[Kubernetes](https://roadmap.sh/kubernetes) space to dynamically allow updating and testing in a shared cluster that uses the systems that run production environments. Many large enterprise teams have implemented a model called[request isolation](https://www.signadot.com/blog/environment-replication-doesnt-work-for-microservices/) that allows a test service to run as part of a cluster without interrupting other services. Teams including[Uber and DoorDash](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/) can push out a test version of a service, even to production, that will only handle test requests but can still make requests to all the other services it depends on.


Signadot allows development teams to use the request-isolation technique on their cluster. By leveraging a service mesh, engineering teams can direct only test requests to the updated version of their service. When a service is updated with a test version, the base version of the service remains available to other teams, so they can use the same testing cluster.


The result enables teams to make small and incremental changes and test them against a real cluster. Developers find problems themselves, vastly decreasing feedback time and accelerating development.
