---
schema_version: "1.0.0"
document_id: "7ff8da3db45552bd3cb772da9cff68702bea52bdd0edd5315e89de7773059a66"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/prezi-eventbrite-teams-testing-on-kubernetes/"
published_at: "2023-05-30T10:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:f30ebb952ab3b8c177fa0ecdd6912dd46a02cf6dbe90bb70cff41ae01573f5bd"
---

# Exploring Kubernetes Testing Evolution: Deep-Dive Case Studies from Eventbrite and Prezi

In a[previous story](https://www.signadot.com/blog/large-engineering-teams-testing-on-k8s/) we explored, in general terms, how large teams were adopting testing on Kubernetes. These were drawn from multiple teams’ writeups on their systems and evolution. In this piece we’ll zoom into the evolution of solutions at two large engineering teams, at Eventbrite and Prezi.


## Case 1: Eventbrite


In the[beginning at Eventbrite](https://blimpup.medium.com/why-eventbrite-runs-a-700-node-kube-cluster-just-for-development-e5a0df95a45c) , the system was similar to what you’d expect at a succesful startup: from a monolith, the increasing complexity motivates the product team to add new features as microservices, and the original monolith runs in a container in production but not locally. Eventually this gets too much to run on a single developers laptop, so the Eventbrite team built **yak** to give users control of which containers they wanted to work with. **yak** moved Eventbrite’s development environment into the cloud.


### Hurdles on the path


There were scaling issues as the team worked to adopt a development cluster


> *Our infrastructure was running on one EKS cluster originally. At one point, we had 700 worker nodes, and 14,000 pods running. We ran into performance and rate-limiting issues that made us reconsider this single-cluster approach. Over time, we switched to a multi-cluster architecture where each cluster had no more than 200 nodes.*


While this sounds like a drawback, there’s some hidden benefit here: in trying to replicate a true production environment, you’re going to face some of the real scaling issues and end up creating an experimentation setup that’s closer to a real-world environment. This multi-cluster structure will help you preview the sync and connectivity issues that your prod environment will suffer.


### Dogfooding your own DevOps, DevOps-fooding


DevOps is a design goal: it aims to break down the silos between development and operations teams, and to create a more collaborative and efficient workflow. By using Kubernetes to test and experiment during development, you are dogfooding your own DevOps practices, and building a culture of collaboration and experimentation that will ultimately lead to better products and services.


Another great benefit of **yak** was how it kept things simpler for developers to dip a toe into the Kubernetes experience:


> *We kept it as minimal as possible and the configuration files are plain Kubernetes manifest files. The intent was to feed developer curiosity so they learn more about Kubernetes over time.*


This is the crux of a well-run Product team that has DevOps as a goal: if you can get developers to try out straightforward configuration of their own clusters, it gets them more familiar with how their code will be run in the real production environment.


## Case 2: Prezi


Prezi describes how their development environment[evolved over time](https://medium.com/swlh/the-3-phases-of-prezis-development-environment-evolution-f3ce3c1e813b) . The brief was a complex one from the start:


A full map of the offering has around 100 microservices. These microservices have a unified pattern that covers code structure, management details (such as building services, running tests, generating reports, and handling static assets), and extension points for special use cases. At the root of each service’s repository, there is a YAML descriptor file that provides information about the service, the features it requires, and how it intends to use them.


And while clusters offer big benefits for scalability, it can also lead to unintended consequences that take a while to figure out. In their article ‘[A Kubernetes Crime Story](https://engineering.prezi.com/https-engineering-prezi-com-a-kubernetes-crime-story-2e8d75a77630) ’ describes how a team noticed increased tail latency for some services after migrating their Nginx-based reverse proxy to Kubernetes. After reproducing the issue and investigating with tracing, they discovered a race condition in the source network address translation (SNAT) operation that caused connection timeouts. They were able to resolve the issue by turning off SNAT altogether on the node. The emphasizes the importance of observability and tracing in debugging complex systems like Kubernetes.


Prezi’s top 2 goals for a developer environment are worth sharing here


---


1. I think the **development environment should reflect the production environment** as closely as possible given your use cases and constraints. It greatly lessens the mental burden and makes it easier to coordinate between teams.
2. It should be **easy and straightforward** to get set up to the point where you can start getting things done with **short feedback loops** to enable fast iteration. However, it’s explicitly ***not* our goal to hide the complexity and the tools running under the hood** . The development environment should *not* feel like “magic”. We expect developers to debug issues themselves, and have a basic understanding of components like Kubernetes so that they can directly check on the pods.


---


These goals are explored in how they evolved to a shared Kubernetes testing cluster.


### Phase 1: Local builds with custom hooks


Prezi originally had a tool to set up a developer’s machine. The tool had hooks for services that instructed the tool on how to install dependencies, test the service, and run the service. The tool also had a layer to manage communication between the services.


However, this setup was problematic because each developer’s machine was different. The hook scripts often failed due to issues such as Python libraries depending on different versions of global binaries or header files that failed to compile. The developer experience team sometimes took a long time to fix these issues. But then they would run into another problem with another service, and the cycle would repeat when developers wanted to work with the service again.


### Phase 2: local containers


Next, Prezi improved their deploy tool to use containerized versions of dependencies, and orchestrated them with Docker Compose. This innovation led to greater stability, allowing developers to incorporate more complex setups and larger sets of services into their workloads. However, as the services grew in complexity, developers began to encounter issues with their hardware. Laptops were spinning up to annoying levels, causing frustration and productivity loss. Additionally, the services that were being actively developed still had to be run from the host machine, rather than from within Docker Compose. This meant that developers were still experiencing slowdowns and other issues, which impeded their ability to work efficiently and effectively. Despite these challenges, Prezi remained committed to improving their infrastructure and providing the best possible experience for their users.


### Phase 3: a shared Kubernetes environment


The Prezi team decided to build a remote development environment on top of Kubernetes. The developer’s code and dependencies would all run in the cluster so that we could manage it. Only a light CLI, and some libraries, would run on the developer’s laptop.


This requires extensive testing and maintenance by the developer experience team, founded as part of this organizational development, but leads to a much more consistent developer experience


### Results: a much improved developer velocity


Prezi’s results for developers are worth quoting at length:


---


The developer’s daily workflow is better:


- It takes a lot **less time and effort** to get to a working environment. Before, it took days to set up an environment that would break from time to time. Now, it takes minutes.
- Each developer’s environment is **disposable and encapsulated** .
- A **unique url** can be shared with others to show progress and open discussions.


---


## The Testing-on-Kubernetes journey


As emphasized a few times above, the journey to testing on Kubernetes is not one that will be resolved with a single tool. Team size, stack complexity, architecture, and testing needs will determine which model works best. Some questions to ask yourself:


- Is sensitive data available to the testing environment?
- Are we concerned about resource overuse if too many test containers are spun up?
- Are we so big that we will require tests to communicate between multiple clusters?
- Is our team small enough that a set of tests blocking others from deploying to testing is acceptable?


Answers to these questions will help determine the best tools for your team. There isn’t a one-size-fits-all solution for testing on Kubernetes. Each team will have different requirements and will need to choose the tools that work best for them.


## Once we agree we want to test on a K8s cluster, how is it implemented?


In our next article in this series, we’ll talk about how teams like Doordash, Lyft and Uber adopted a different model of multi-tenancy for testing in Kubernetes. They adopted a Kubernetes cluster specifically for testing, with the multiple stages of environment evolution.


‍
