---
schema_version: "1.0.0"
document_id: "9382b0bc29fa9cb2ca1a0dbb0d6c197d564154712b382c97e1581f3ea8f999de"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/why-the-sdlc-workflow-is-broken-for-many-developers/"
published_at: "2023-08-21T05:33:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:99153fe1e2cf2178d65ef2f21d9af3c6d8aeece5f9e4fa9a9bd46d59da960a22"
---

# Why the SDLC Workflow is Broken with many Microservices

I want to talk about the unique challenges faced by a developer who owns a few microservices among many, especially with the constraint of relying on shared staging environments for testing.


## Introduction


In the modern microservices architecture, a developer often handles multiple individual services. In cases where there are numerous microservices, bringing up a local environment for all services is often not feasible. The typical SDLC workflow in this scenario requires reliance on shared staging environments for testing, which brings its complexities and challenges. This piece won’t cover why especially large teams and end up adopting shared clusters for testing and experimentation, that’s covered in more detail in my[exploration of testing in large architectures on Kubernetes](https://www.signadot.com/blog/large-engineering-teams-testing-on-k8s/) series.


### Stages of the Process


#### 1. **Write Code**


- **Time Consumption** : Moderat
- **Details** : Developers first write the code for their specific microservices. This step includes designing, coding, and using local tools to ensure the code functions correctly.


#### 2. **Basic Local Testing**


- **Time Consumption** : Varies
- **Details** : Developers perform initial local testing to ensure the microservice is working as expected. Since the entire environment can’t be replicated locally, this testing can be limited and less comprehensive.


#### 3. **PR Phase - Code Review**


- **Time Consumption** : Considerable
- **Details** : The code must go through a rigorous peer review process. The peers must ensure that the code adheres to standards, is maintainable, and fits well within the architecture. Delays can occur if changes are requested.


#### 4. **Run CI/CD**


- **Time Consumption** : Moderate to High
- **Details** : Continuous Integration and Continuous Deployment (CI/CD) pipelines automate the building, testing, and deployment. This phase can become complex and time-consuming if problems are encountered due to dependencies on other microservices. While it’s true that in small architectures or on your dev projects, CI/CD tools should take minutes or even seconds to run, the reality of larger architectures means this kind of speed isn’t generally possible. A critical point here is that these automated tools always take long enough that the developer leaves their desk, or has to switch tasks, waiting for it to complete. This is not a ‘save and hit refresh’ process, it involves the dreaded[task switching](https://www.apa.org/pubs/journals/releases/xhp274763.pdf) .


#### 5. **Test on Staging**


- **Time Consumption** : High
- **Details** : This phase involves testing in a shared staging environment, mimicking the production environment as closely as possible.
- **Debugging Difficulties** : Debugging test failures in staging is often complicated and time-consuming due to the co-mingling of commits from multiple developers.
- **QA Challenges** : If Quality Assurance (QA) is involved in testing here, the process becomes even more intricate. The entire bug filing workflow has to be followed, leading to delays and potential miscommunications.
- **Blocking others:** Whether or not QA are involved in your process on staging, if you commit breaking changes to your shared cluster, you will inevitably block the work of other developers who want to use staging for the same purpose.


## What can we do to fix our Software Development Lifecycle?


The workflow described above highlights the complex and multifaceted nature of developing and maintaining microservices within a larger system. The reliance on shared staging environments adds a layer of complexity that can significantly impact the efficiency and effectiveness of the process.


The solution to this conundrum is a better process for letting developers experiment with your shared cluster. In recent pieces we’ve talked about how[large teams often evolve their approach](https://www.signadot.com/blog/how-uber-and-doordash-enable-developers-to-test-in-production/) as they resolve to allow faster and better[experimentation for developers](https://www.signadot.com/blog/how-lyft-and-razorpay-share-development-environments-with-hundreds-of-devs/) .


### Three steps to fixing your Software Development Lifecycle


1. **Shift left testing and experimentation to *before* the slow stages of the SDLC** - The more your developers can ‘play around’ with changes before a code review/QA process, the more they’ll learn and the faster they can ship features
2. **Give devs the fastest possible feedback on changes** - There’s always a need for a QA process, but do the work to let developers both try out changes and share those changes with other teams before a formal release process
3. **Foster collaboration and prevent breakage of shared environments** - Following up on 2., while we want to be able to *decide to share* our changes with other teams, having Team B access a shared cluster and discover that Team A has moved things around unexpectedly will have the opposite effect: teams won’t trust each other and the shared environment will become subject to the same heavy release controls


There are a number of Developer Experience tooling choices that can help make these goals a reality. For smaller teams it may work to have[multiple staging instances or namespaces,](https://www.signadot.com/blog/namespace-based-environments-for-testing-pros-and-cons/) for larger teams consider[sandboxes and request isolation](https://www.signadot.com/blog/transforming-kubernetes-developer-environments-the-shift-to-request-level-isolation/) .


‍


## Microservices Architecture vs. Monolithic Architecture


One of the questions that arises when discussing the implementation and lifecycle difficulties of the standard SDLC is that, if things are so much more complex within a microservice architecture, why not abandon the model altogether? If testing and tracing were easier with a monolith, and it was often possible to replicate the monolith in your own environment, why not return to it? Let’s talk about the differences and why you probably don’t want to move away from microservices.


Microservices architecture involves breaking down the entire application into smaller services, each performing a specific function. Monolithic architecture, on the other hand, builds the entire system as a single unit or application.


### Advantages of Microservices:


-


Scalability: Microservices can scale independently, offering flexibility and efficiency. Companies like Netflix have leveraged this architectural style to meet demand.


-


Fault Tolerance & Resilience: A failure in one service doesn’t bring down the entire system.


-


Technology Diversity: Different services can employ different technologies, allowing for experimentation and the use of best tools for specific tasks.


-


Continuous Delivery & Integration: Facilitates continuous integration and continuous delivery (CI/CD) pipelines, enhancing collaboration and efficiency.


-


Challenges with Microservices:


-


Data Consistency: Ensuring consistency across different services can be complex.


-


Security Vulnerabilities: Implementing consistent security controls and Access Control can be challenging.


-


Operational Costs: More infrastructure and resources might be required.


-


Monolithic System Considerations:


-


Simplicity: Easier to develop and test in the initial stages.


-


Challenges in Scaling: Scaling a monolith might involve scaling the entire application, leading to higher costs and complexity.


### Development Process Tooling in Microservices


While the SDLC predates microservice architecture, the tooling and techniques are significantly different. Emphasizing best practices, the SDLC in microservices involves several phases and steps.


- Development Phase & Design Phase: Involves defining use cases, requirements, and creating a design. Development teams often employ microservices frameworks to speed up this part of the project.
- Integration Testing: Tools like Docker are often used to create a test environment, enhancing the quality and resilience of microservices applications.
- Deployment & Maintenance: Leveraging Amazon Web Services (AWS) and containers, teams can automate deployment and streamline maintenance. Third-party providers can also be utilized.
- Development Project Considerations:
- Small Teams & Large Applications: Small teams working on complex applications benefit from the independent services approach.
- Communication & Collaboration: Effective communication between different teams and business units is crucial.
- Cultural Shift: Moving from monolithic system to microservices environment requires a significant cultural change within the engineering team and organizations.


### Microservices are a clear choice


Microservices architecture represents a substantial shift in the way software systems are built and maintained. From enhancing scalability and fault tolerance to introducing challenges in Data Consistency and security, this approach is multifaceted.


Leveraging platforms like AWS, tools like Java virtual machines for isolation, and adopting agile methodology, teams can create robust and flexible systems. However, the journey requires careful planning, understanding of risks, alignment with business logic, and a focus on user experience and security.


Companies like Amazon and Netflix have demonstrated the powerful capabilities of this approach. As we look forward to the coming years, microservices will likely continue to evolve, with new features, methods, libraries, and solutions to overcome existing challenges, reflecting the dynamic and innovative nature of modern software development. Whether for smaller services or large applications, microservices are an exciting trend shaping the future of technology, reflecting the complex yet elegant nature of modern software development and operations.


It’s unlikely that monoliths, however reasonable they are for smaller-scale operations, will come to be a significant trend in Enterprise of even SMB operations in the near future. A significant shift in the economics of large-scale SaaS would have to occur for Monliths to re-emerge.


## Conclusion: Maintain a Clean Path for Feedback with Developer Experience Tooling


The complexity of developing and maintaining microservices within a larger system, especially with the reliance on shared staging environments, necessitates a refined approach to the Software Development Lifecycle (SDLC). A solution lies in a three-step process: firstly, shifting left testing and experimentation to allow developers more freedom before the slower stages of the SDLC; secondly, providing developers with the fastest possible feedback on changes, balancing the need for QA with the ability to try out and share changes before formal release; and thirdly, fostering collaboration while preventing breakage of shared environments to maintain trust among teams. Developer Experience tooling choices, such as multiple staging instances or namespaces for smaller teams, and sandboxes and request isolation for larger teams, can facilitate these goals, leading to a more efficient and effective development process.


To go deeper, smaller teams may want to try[multiple staging instances or namespaces,](https://www.signadot.com/blog/namespace-based-environments-for-testing-pros-and-cons/) for larger teams consider[sandboxes and request isolation](https://www.signadot.com/blog/transforming-kubernetes-developer-environments-the-shift-to-request-level-isolation/) .


‍
