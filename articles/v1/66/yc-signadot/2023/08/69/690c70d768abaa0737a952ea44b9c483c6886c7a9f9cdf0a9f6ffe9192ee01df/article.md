---
schema_version: "1.0.0"
document_id: "690c70d768abaa0737a952ea44b9c483c6886c7a9f9cdf0a9f6ffe9192ee01df"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/the-struggle-for-developers-to-collaborate-on-microservices/"
published_at: "2023-08-25T06:08:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:b5ee14daaf16c30ad7eb6910ced2740a467197bf30627a167992e27b2b9f8c3e"
---

# The struggle for developers to collaborate on microservices

In conversation with[Kostis from Codefresh](https://www.signadot.com/webinar-and-podcast/with-codefresh-is-the-developer-experience-around-microservices-lagging/) last month, one thing he said really stood out to me:


Kubernetes and containers have made the operations side of the equation far simpler and more consistent. However, the developer experience has not really caught up. Developers are still trying to figure out the ideal software development lifecycle. They need to have environments at their disposal to be able to develop fast, test their code fast, and collaborate with other developers that may be working on other microservices that they could depend on.


There’s no doubt that microservices has drastically improved the operational excellence of large software teams. When is the last time that a service you relied on was down for more than a few hours? The establishment of clear contracts between microservices helps developers work confidently, with simple tests showing whether any changes continue to meet said contract. Our complete guide to[microservices testing](https://www.signadot.com/the-complete-guide-to-microservices-testing-from-local-development-to-production/) covers how those contracts get validated across the lifecycle.


But microservices represent challenges for the developer experience as your microservice architecture grows in scale. It is difficult to test how your code works in a complex microservice architecture, and collaborating on larger or more complex features can be made doubly difficult in these environments.


## Previewing features in a microservice space can be tricky


Before microservices, developers could have the entire application (front end, middleware, database) on their local workstation, making it easy to develop and test. There were of course limitations to this approach: architecture differences meant that some things ‘worked on my machine’ and didn’t work in production, but the basic map of the system could be entirely mapped on a local workstation.


After microservices, with the splitting of the monolith into potentially hundreds of microservices, it becomes impossible to have all services running on a local workstation. This means you’ll have to devise a different solution to preview how your code will work with dozens of other microservices.


## Different approaches to providing preview environments


Several extant strategies exist for creating a preview environments. They can be broadly defined as:


- **Subset Solution** : Developers might choose to run only a subset of microservices locally, mocking or stubbing out the rest. This requires careful selection and can lead to inconsistencies between local and production environments.
- **Docker Compose** : Some might use Docker Compose to run a bunch of services locally. This approach may not align with production if Kubernetes is used there, leading to a “Delta” between development and production.
- **Kubernetes Namespaces** : Others might standardize on Kubernetes for development, spinning up namespaces for the services needed. This approach mimics the production environment but requires more setup and understanding of Kubernetes. It can also lead to issues as these environments fall out of sync or aren’t updated frequently enough.
- **Other ‘Ephemeral Environment’ Tools:** There are a hundred different tools available to let you spin up a container very quickly, often touted as ‘environments as a service.’ These lightweight environments can be the worst of both worlds: a pale imitation of the real cluster of services, while also requiring constant maintenance work and significant infrastructure costs.


💡I recenly heard an anecdote about a large enterprise team: Earlier than most teams they ran into the need for some kind of Sandbox for previewing code. However as these sandboxes were needed frequently, they began to gobble resources. Therefore an entire team was dedicated to working as ‘sandbox killers’ to detect unused/uneeded sandboxes for downscaling.


All of these solutions, even with the drawbacks listed, will work at smaller scales, but they will all pose significant challenges to collaboration between engineers.


## **Collaboration Challenges with Microservices**


What are the situations where microservice teams need to collaborate? Shouldn’t each small team just worry about their microservice and fulfilling the contracts between them and the others? Picture a simple feature like collecting user birthdays and adding a ‘happy birthday’ message to users on their profile page. Which microservice team can deliver this feature? No microservice team can deliver a feature like this. From user signup, to user records handling, to the profile display, multiple microservices will be involved. This is true of ***most features*** . Simple requests lead to more and more requirements for teams to work together to make sure every aspect works.


The solutions listed in the section above prove problematic for collaboration between teams. The issue, broadly, is isolation between developers and the separation of operational aspects from developer control.


**Isolation Between Developers** : Using namespaces in Kubernetes provides isolation between different developers, but it also means that collaboration requires more coordination and understanding of the shared environment.


**Operational Divisions** : The provisioning of namespaces might be handled by a platform or DevOps team, separating the operational aspects from development. While this can be an advantage, it also means that developers must rely on other teams for certain aspects of their environment, potentially slowing down the development process.


Let’s talk through our process of pushing out a ‘user birthday’ feature with the tools we have available:


1. Frontend, User Data, and Backend teams are all tasked with working together to add user birthday messages. They define an API contract to fulfill to pass this user data around.
2. All three teams work in their own environments to implement the feature, with stubs for the missing components. These environments comprise the ‘subset’ solution.
3. When it’s time to test changes to all three services at once, the teams want a new namespace to try out the changes. But Operations hasn’t prioritized this feature, causing delays.
4. After testing in their own environments, and performing code review, and waiting for Operations to set up a new namespace, all updates are pushed to one namespace. This causes unexpected interactions, and things don’t work as planned. Each team returns to their own environments to implement fixes.
5. Fixes are implemented, and changes are tested on the shared namespace. Now the team has to merge these updates to production.
6. However, in the time since this namespace was created, other teams have made changes to other services that introduce unexpected interactions.
7. For the team to move forward, they either have to fix their problems live, or update their shared namespace with changes made by other teams. Again, they have to engage operations to update the namespace
8. after all the work done to build and then update the shared namespace, these three teams aren’t tasked with working together for the next few months. As such the namespace isn’t updated, and there’s even a chance it runs up infrastructure costs for some time before anyone notices to shut it down.


### The criticality of shifting testing left


The concept of microservices shift left testing emphasizes the importance of testing early and often in the development lifecycle. By shifting testing to the left, developers can catch issues sooner, reducing the time and effort required to fix them. This approach aligns well with the dynamic and distributed nature of microservices development, where collaboration and communication between team members are essential.


### The conundrum of microservice collaboration


The transition from a monolithic architecture to a microservice architecture introduces new complexities in the development environment. While solutions like Docker Compose and Kubernetes namespaces can mitigate some challenges, they also introduce new collaboration difficulties. The need to manage multiple services, ensure consistency between environments, and coordinate with other teams can make collaboration more challenging in a microservice architecture.


## High Level Solution Design


How can two teams collaborate on features with extensive interdependencies? Let’s go over the requirements:


- Ability to Sandbox services that are being changed
- Access to shared resources - for resources not under experimentation, we want an up-to-date shared cluster that we can experiment with.
- No need to ‘fork’ the rest of the cluster - the operational overhead is too high to keep these forks up to date.
- Easily configurable by microservice teams - Ops support is of course necessary, but we shouldn’t have to wait for for ops just to have a new experimentation space. Ideally something like a GitHub action to create the space rather than an Ops migration.
- Ability to share experiments/branches - critical to collaboration with other teams, there needs to be a way to let others access and experiment with our branch. This requires the ability to route traffic dynamically to Sandboxed services.
- Ability to combine Sandboxes for collaboration


In essence we require something that easily lets us define zones where we can test and experiment with new code, a ‘sandbox’ that can safely interact with the rest of our cluster.


### Possible solutions: service mesh & request isolation


In order to enable on-demand sandboxes that can connect with the shared cluster when necessary, many solutions to allow a sort of ‘tunnel’ between a local service and the cluster, seems like a solution. This, however, really struggles when we think about collaboration. How would this ‘tunnel’ allow for a request to hit multiple test services, running on disparate local workstations? It’s not well supported by that architecture.


Rather we need something that isolates the requests of our sandbox and can control routing across the entire cluster. For this, a tool like Istio or linkerd, running a service mesh that controls intra-service communication, is a crucial missing piece.


## Conclusions: the path to a collaborative microservice world


This piece is more about defining the problem and the hazards along the way than a fixed solution to issues of collaboration with microservice architectures. The interdependent world of microservices has made local replication difficult, and existing stop-gaps for smaller microservices clusters stop working beyond a certain scale. The result has been a much slower path between first writing code and sending it to production, with added friction as changes conflict with each other. When we imagine features that rely on multiple changes across teams, it’s possible that developer velocity can slow to zero.


However the same technologies that brought about this impasse also offer solutions: in a modern containerized and orchestrated cluster of microservices, tools like service mesh can make it possible to collaborate fluidly with multiple test sandboxes.


‍
