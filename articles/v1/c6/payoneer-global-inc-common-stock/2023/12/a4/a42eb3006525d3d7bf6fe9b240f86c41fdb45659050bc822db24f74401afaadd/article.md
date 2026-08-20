---
schema_version: "1.0.0"
document_id: "a42eb3006525d3d7bf6fe9b240f86c41fdb45659050bc822db24f74401afaadd"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/our-journey-making-netflix-conductor-production-ready-for-our-platform-608d2a192035"
published_at: "2023-12-21T07:01:45+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:0d2bb0d14c49d0e31837fd6451f2b335d6084ac7b0b9e691153859571d2f520f"
---

# When Our Platform Met Conductor

# When Our Platform Met Conductor


[Amir Popovich](https://medium.com/@amirpopovich?source=post_page---byline--608d2a192035---------------------------------------)


7 min read


·


Dec 21, 2023


--


Press enter or click to view image in full size


As a fintech company that specializes in global payments, we have endless
challenges supporting multiple complex payment flows.
Aspects like reliability, scalability & efficiency are key factors for us.
This is where the strength of an advanced orchestration tool becomes critical.


After reviewing multiple orchestration tools, we found Conductor as the most suitable solution for us.
The main reasons for choosing Conductor were the DSL support and the ability to work with the workflow engine without an SDK.
This enabled developers and other stakeholders to create and maintain workflows.


Conductor is a platform created by Netflix to orchestrate workflows that span across microservices.
In this blog post, we will dive into the challenges we faced integrating Conductor as an orchestration tool.


Press enter or click to view image in full size


## Our Setup


**✓** Persistence: Redis
**✓** Queueing: Redis
**✓** Indexing & Archiving: Elasticsearch
**✓** Events: RabbitMQ (AMQP)


I’ll walk you through the primary actions and decisions we made as part of our integration with Conductor and share some insights on challenges we had and how we tackled them.


## **1.** Learned the Architecture of The Product


We deep dived into the main components & modules and got proficient with the codebase. This was critical as we encountered many challenges during our integration and it was easy for us to understand how to fix them efficiently & correctly.


## **2.** Changed the Project Structure


Nowadays, there are two main repos


- [Main conductor repo](https://github.com/conductor-oss/conductor) —The original repository
- [Conductor community repo](https://github.com/Netflix/conductor-community) — Community contributions & extensions


The current practice is to use the community repo as the baseline and reference:


1. The main repo’s modules as packages.
2. The community modules as local projects.


The structure we currently use is different. We use:


1. The main repo as our baseline
2. Specific community modules as imported as packages.


Currently, the main repo holds all the core interfaces as part of the core module, creating tight coupling between community modules and the conductor core module.


That introduced some challenges for us:


- *Aligned Versioning*
The versioning needs to be aligned, meaning that you cannot consume a community package which is not aligned version wise with the core modules version since interfaces may have discrepancies and create runtime errors. This practice also exists when you use the community repository as your baseline, but in our case, since we modified the files, we needed to keep track of versions every time we merged changes from the main Conductor repo.
- *Preventing Circular Dependency Overrides*
When importing community packages, we needed to exclude the core module dependencies in order to prevent them from “overriding” our project dependencies in the *runtimeClasspath* .
This is due to the logical circular reference ( *community-module* -> *conductor-core* package vs our local *conductor-core* project) that was created when we changed the project structure.
*A good solution would be to extract the shared interfaces to an external standalone module that both the core and the community modules would reference.*


This is a small example of how our server/build.gradle file looks like:


```text
// Core local module  implementation project(':conductor-core')    // More local modules   // Community module as a package  implementation("com.netflix.conductor:conductor-metrics:${revConductorCommunityDeps}"){      // Exclude the core dependecies to prevent them from being in the runtimeClasspath since we want to use our local modules instead      exclude group: 'com.netflix.conductor', module: 'conductor-core'      exclude group: 'com.netflix.conductor', module: 'conductor-common'  }   // More community module packages
```


We found that working with this project structure made it a lot easier for us to debug, add logs, metrics, make code changes and fix bugs immediately on core modules when we needed to, without waiting for PR acceptance and releases which are not controlled by us.
When we need to make immediate changes in a community package, then we handle this specifically per module, depending on the use case.


## **3.** Developed Custom Tailored In-House Components


- A custom UI that made it easier for developers to create workflow definitions and analyze the workflow executions.
- A mitigation “bridge” for events, making Conductor transparent for microservices in our backend. Backend microservices would raise business events and behind the scenes a component would mitigate between those business events to starting & resuming actions on conductor workflows.
- A worker SDK to support efficient task pulling mechanisms from Conductor.
The worker helps us with different use cases like priority based flows and bulk processing in other cases.
Also, our worker implements the site awareness pattern to help us with our active/warm passive disaster recovery strategy, meaning that only workers on active sites pull tasks from Conductor.


## **4.** Used Workers


We believe a Conductor server instance should be a lightweight process, focusing on it’s single responsibility, which is orchestration.


## Get Amir Popovich’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Async tasks should run on external small worker processes rather that as a part of the Conductor server process, making it easy to scale out when you need to.
Every async task is backed by it’s sole queue, making it easy to monitor and identify bottlenecks rapidly.
Using workers gives you the flexibility to code them in whatever language you think is relevant for a specific task.


The main problems start appearing when you run tens\\hundreds of thousand large workflows concurrently and then, instead of scaling specific small worker processes, you find yourself scaling more and more instances of the Conductor server.


We found that workers is the way to go after numerous efforts trying tweaking the “System Task Worker” configuration and finding the magic numbers.
You can find multiple posts over the web regarding the “optimal configuration values” for the following keys:


```text
conductor.app.systemTaskMaxPollCount=?  conductor.app.systemTaskWorkerCallbackDuration=?  conductor.app.systemTaskWorkerThreadCount=?
```


***Avoid Using the Default Http Task***
Since by default, Conductor uses a single queue for async system tasks like Http, this can cause starvation and delays in you workflow executions.
This happens since a single queue handles all your http calls.
Slow http response times will delay other items in the queue, causing a ripple effect.
Running lot’s of workflows concurrently with this behavior can cause large delays since workflows run task by task.
When you have hundreds of thousand workflows running concurrently, then it takes time for a single workflow to complete.


## 5. Idempotency, Idempotency, Idempotency!


All our workflows and tasks are idempotent. This really helps when things go wrong, as we could easily re-run tasks and workflows without getting redundant side effects across the system.
Also, we start our processes outside of Conductor. We use Conductor as the engine that orchestrates the main processes, but if anything goes wrong with Conductor, we still have 100% track of everything.


## **6. Fine Tuning**


- We started using[orkes-queues](https://github.com/orkes-io/orkes-queues) over[dyno-queues](https://github.com/Netflix/dyno-queues) since we had some issues with[dyno-queues](https://github.com/Netflix/dyno-queues) and preferred to use a maintained library.
- We configured Redis to persist (fsync) every second and not on each write due to the frequent writes Conductor performs.
- We stopped indexing tasks by overriding the indexDao layer, simply since it was irrelevant for us.
- We started using workers for all async tasks. That gave us the ability to scale things better and reduce burden from Conductor.
- We enabled the workflow repair service by configuration:
*conductor.workflow-repair-service.enabled=true* The workflow repair service kicks in as part of the sweeper process and fixes multiple use cases where workflows or tasks are in semi-state.
We encountered multiple issues that the workflow repair service fixed. One example is that the Conductor process crashed in the middle of the task scheduling process.
Since the scheduling had a couple of steps and was a non transactional operation, we ended up with a taskId enqueued into the queueDao before the task instance was persisted to the executionDao.


## **7. Monitoring**


Conductor throws lots of metrics and gives you the ability to export to Prometheus easily.
We created monitoring dashboards using Grafana and added alerts when needed. This helps us find the hot spots and bottlenecks.


Here are some key metrics to look into:


- workflow_running - Indicates the number of running workflows
- task_execution_seconds - Indicates the execution time of a task
- task_queue_wait_seconds - Indicates how long a task is stuck in a queue


Also, don’t forget to monitor the usual stuff — RAM, IO, File Descriptors, Garbage Collection etc.


## **Future Thoughts**


We identified a couple of areas that we would like to improve in terms of reliability and performance.


- *Reduce the Workflow’s Memory Footprint* Currently, every workflow instance holds a copy of the definition.
Some of our workflow definitions are really large, and when you have hundreds of thousand instance running concurrently, then it starts to become an issue in terms of memory since we are using Redis.
The solution we thought about is to create strict versioning on the workflow definitions, meaning every change will create a new immutable version of the workflow definition and instead of carrying the whole definition on each instance, we will carry the definitionId which is immutable and will be cached in-memory, mapping to it’s definition.
- *Decoupling and Improving the Indexing & Archiving* Nowdays, the indexDao handles both the indexing and the archiving.
We think about using different storages for indexing and archiving.
In our use case, indexing is nice to have, as we don’t really use the full text search capabilities.
Also, we would like to create a more reliable approach to archive workflows, supporting reliable retries since we encountered issues where the archiving failed due to various reasons and we ended up without an archived document, making diagnostics hard.
Decoupling indexing and archiving will also make it easy for us to create a retention strategy, since today a single Elasticsearch document is used for both, indexing and archiving and the indication of an archived workflow is by the {‘ *archived’:true}* property on a document.


## Conclusion


Our experience with Conductor as an orchestrating tool has been positive after making the relevant adjustments.
Its capability to manage complex workflows efficiently has significantly enhanced our R&D processes, delivering a seamless and satisfying experience for all involved stakeholders.
