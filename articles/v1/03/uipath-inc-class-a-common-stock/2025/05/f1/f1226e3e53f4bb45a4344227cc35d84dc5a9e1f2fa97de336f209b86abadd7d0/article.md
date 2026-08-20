---
schema_version: "1.0.0"
document_id: "f1226e3e53f4bb45a4344227cc35d84dc5a9e1f2fa97de336f209b86abadd7d0"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/uipath-api-workflows-engineering-a-scalable-secure-system-to-system-automation-engine-6934a59760b3"
published_at: "2025-05-28T06:21:59+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:d087480ba602bdee63475bc6172e2d3a6d336c6474b0d96cb7f600c145c7af1b"
---

# UiPath API Workflows: Engineering a Scalable & Secure System-to-System Automation Engine

# UiPath API Workflows: Engineering a Scalable & Secure System-to-System Automation Engine


[Arghya Chakrabarty](https://medium.com/@charghya?source=post_page---byline--6934a59760b3---------------------------------------)


12 min read


·


May 28, 2025


--


## What are API Workflows


API workflows are lightweight, powerful workflows purpose-fit for system-to-system API integration. API workflows allow to build **composite service/API** by chaining multiple API calls, building **multi-step processes** , and implementing **data consistency** scenarios with support to transform request/response using` JavaScript` snippets.


For example, consider a simple use case to get weather and news information for a city from different APIs and merge the response into a single response.


` /getNewsAndWeatherByCity` API workflow:


- Receives city and country as inputs
- Fetches news via a news API
- Obtains coordinates of the city using a geo-location API, then retrieves weather via a weather API
- Merges both results using JavaScript and delivers news and weather data as a combined response


Watch a quick introduction[here](https://www.youtube.com/watch?v=_WRRsi9O-mQ) .


## Motivation and vision


- **API-first strategy.** 75% of our customers report having an API-first strategy, but most lack the tools to execute that strategy efficiently and at scale. As automation becomes more agentic and AI-driven, workflows must shift from slower, UI-based triggers to real-time orchestration of data and decisions across systems via APIs with deterministic API Automation as a core construct.
- **System-to-system integration** . Seamless and fast integration between systems. For example, two-way sync of contacts between two different CRM systems.
- **Zero-touch runtime** . Execute on a fully automated, light-weight, fast, on-demand, secure, and dynamically scalable runtime.
- **Security and Governance.** Robust control of sensitive data and actions, protecting enterprise systems, and controlled access to scoped data and actions to AI agents **** via API Workflows.


## Engineering challenges


In the world of automation, API automation plays a crucial role. While creating simple automations involving a few API calls is straightforward, making a secure, performant, and scalable solution for complex API — driven use cases is not so simple.


Highlighted here are a few of the engineering challenges we tackled while building API Workflows.


**Security**


- Avoid noisy-neighbor problems with tenant and process-level isolation to execute API workflows.
- Execute customer-supplied JavaScript in isolation via **V8-isolates** . More details in the deep-dive section below.


**Speed & Performance**


- To illustrate — As API workflows execute as a single execution unit, creating a **purchase order** might involve ~10 API calls (inventory, billing, finance, record books, notifications etc.), plus data management and control-flow logic. This will result in a lot of memory due to data being pulled from different data sources, and high usage of CPU due to heavy data manipulation.
- Strong execution isolation demands more resources. Optimization in terms of memory/CPU without compromising the isolated requires careful design choices.


**Enterprise-scale execution**


- Invoking hundreds or thousands of API calls and workflows in parallel with a scalable runtime that requires zero management.


## Diving deep into the API Workflows engineering


## Design principles


Before we talk about the details of design flows, here are the broader principles we followed for design:


- **Open standards** : Widely adaptable, platform-independent, and interoperable
- **Security at the core** : Isolation and controlled access
- **Zero-copy data** : No unnecessary duplication
- **Fault tolerance:** with user-controlled behavior
- **Usability** : Fast to build, easy to test, customizable, reusable
- **Zero-touch runtime** : Deploy once, run forever
- **Observability**


## Core constructs


- The **workflow** itself is a simple, lightweight, platform-agnostic workflow metadata, stored as plain` JSON` files, and follows open source` CNCF`[Serverless Workflows](https://github.com/serverlessworkflow/specification)
- The workflow **execution engine** is an independent component, built from the ground-up for performance, portability, security and scalability
- Written in` JavaScript` (actually` TypeScript` ) for wide adaptability-runs on all major OS servers, containers, and modern browsers (practically, can run anywhere)
- As the system is built around API integrations, it supports open` HTTP` calls, as well as structured vendor API calls through[UiPath Integration Service](https://docs.uipath.com/integration-service/automation-cloud/latest/user-guide/introduction) ™
- The **designer UI** for building the workflows is fluid and natively supports the above constructs like` JavaScript` and` JSON` . It also supports` in-browser` debugging, where the debugger runs natively in the browser without any backing service or web assemblies
- API Workflows also have in-built support for generating fully working workflows through natural language prompts, using theUiPath conversational AI for developers tool,[UiPath Autopilot](https://www.uipath.com/product/autopilot) ™
- API Workflows run on UiPath own **serverless infra** , an in-house distributed infra for running automations at scale with execution isolation. ******* API workflows by design are platform independent, and can run on any infrastructure
- API workflows integrate into UiPath robust **downstream systems** for workflow management, authentication, monitoring, etc.


## Overview


Press enter or click to view image in full size


API workflow high-level overview


Now that we know about the basic building blocks of the system, lets understand how the whole system works together, from design to deployment and monitoring. We’ll keep it short, and talk about the essential parts.


API Workflows follow a simple execution model around` design → deploy → run → monitor`


1. API Workflows are designed in a web-based designer with **native in-browser** debugging
\[a\] The basic design goal behind this is to make debugging faster, smoother, and cheaper by running the full workflow engine natively in the browser, as a` JavaScript` module
\[b\] In the future, we’ll also enable remote debugging on Serverless, for specific scenarios
2. Once the workflow is fully developed and tested, it is published as a package with versioning
\[a\] It creates a simple compressed package with the workflow definition JSON, and a few other small config files. The goal is to make it a light-weight, shareable and reusable unit
\[b\] The package is then stored in the central workflow orchestration service for management and reuse.
3. When this package version is run, the request is sent to the Serverless Control Plane
\[a\] The control plane is the entry point and it manages scale, load balancing, and fair distribution of resources within the serverless infra.


Let’s look at some of the core components and see how they solve some of the core challenges we talked about earlier.


## The workflow engine


The` Workflow Engine` is the core of workflow execution. The engine reads, parses, validates, and runs the workflows. This is an independent component specifically designed to solve API automation problems. It enables the` API Workflows` to be fast, fluid, flexible and fault-tolerant.


## The design principles and components


Press enter or click to view image in full size


The workflow engine


The system is very modular, extensible and all the different parts of it are designed to be reusable. Here we’ll take a quick look at the different components and how they work together.


### Modularity and composability


1. ` Commons` : Core functionalities like` script` execution,` API` calls, and generic utilities as a reusable unit, published internally as` npm` package.
2. ` Workflow Engine` : Main module responsible for the flow control, state management and error handling. It does all the parsing, validation, and execution of the workflow. This is a pure` JavaScript` module, published internally as` npm` package, and can be used in any` JavaScript` enabled environments like desktop applications, browsers, and servers. This provides the test and debugging capability on the designer.
3. ` Runtime Executor` :This[Deno](https://deno.com/) application, that internally uses the` Workflow Engine` :to run the workflows, adds a layer of security and specific customization to run it efficiently on the serverless infrastructure.


### Extensibility model


The workflow engine supports a very flexible extensibility model and enables plugging in different components and handlers to override the default behavior of the system. Common injectable components are Loggers, Task Handlers, and Expression Handlers. For instance, the designer and serverless runtime (the main two consumers of the engine) inject their own loggers to log data to their intended systems.


### Error handling


API Workflows support different strategies for error handling, with full control given to the workflow designer.


- **Try-Catch** : API Workflows support` Try-Catch` construct. Users can use it to handle errors and manage fallbacks and controls flows
- **Retries** : All the tasks, including` Try-Catch` tasks support extensive retry mechanisms like different backoff and fallback strategies (coming soon)


### Observability & Debuggability


**Observability:** This follows the standard patterns of UiPath systems for full visibility into the executing systems.


1. The executor creates trace logs of the complete execution, individual steps, timing, errors etc. This works together with other components like the orchestration service and serverless, creating complete end-to-end transactional observability data
2. Key business statistics are collected through curated telemetry data
3. [UiPath Insights](https://docs.uipath.com/Insights/automation-cloud/latest) provides the necessary tools to easily build analytics dashboards


**Debuggability:** The system supports two types of debugging


1. Basic debugging through trace logs collected during execution
2. Comprehensive step-by-step debugging in the web designer with an in-built debugging module that supports a full debug protocol, enabling fine-controlled step-by-step debugging for pro developers


## The workflows designer


This was our first-class intention to support workflow generation from text. The workflow schema that we support is plain-text-based, readable to people and machines alike. Our strategy is to enable *“text to workflow”* as the mechanism to iteratively build workflows, with workflow designer as the helpful visual interface to ensure the intent is captured correctly!


## Get Arghya Chakrabarty’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The new designer is built on the[Studio Web](https://docs.uipath.com/studio-web/automation-cloud/latest/user-guide/overview) , a web based IDE to build, test, debug, and publish various types of workflows. There’s a quick-picker tool for control tasks like` if` ,` for` , and` try/catch` and inline editor for` JavaScript` code. It also offers a wide variety of Connectors for third party API integrations, including Office 365, GitHub, SAP, Oracle, Salesforce, Workday etc. It even supports custom connectors to create your own connector when needed. The designer supports testing the workflows in place, within the browser, allowing you to view and modify data from different APIs in realtime!


Press enter or click to view image in full size


The workflow designer


If we take a step back from our principle of *“text to workflow”* , we can start from natural language conversation for generating the text. The conversational AI tool for developers, aka[UiPath Autopilot](https://www.uipath.com/product/autopilot) ™ can help build fully functional API workflows from scratch, just by talking to it. Watch a short introduction on how you can[build API Workflows with Autopilot](https://www.youtube.com/watch?v=iH8EP6yeEZY) .


## Managing security and scale with custom JavaScript code


When there is user code involved in a workflow, a major challenge is securely executing that user code, while maintaining performance and scale! Within a workflow, users can write` JavaScript` expressions and functions. A user created function can always pose a risk to the system. Risks of:


- Accessing system or environment details
- Accessing data from past or neighboring workflow runs
- Overusing system resources (compute, memory, time, file system, etc.)
- Injecting malicious code to abuse or break the system


If not secured properly, some bad or malicious code could overload the system and push up costs, expose private data, or bring down parts of the system. The` API Workflow` runtime infra is designed to protect the system against all the possible security risks, and scale freely around that. These security measures are applied at two levels:


## API workflow engine with V8-isolates based isolation


Since the security model heavily depends on the` V8 Isolates` , I’ll start with a quick introduction of that, and then talk about the security model.


> ` *V8 Isolates*` are independent, isolated execution environments within the V8 JavaScript engine. They allow for running multiple, concurrent JavaScript code segments within a single process, preventing them from interfering with each other.[Docs](https://v8docs.nodesource.com/node-0.8/d5/dda/classv8_1_1_isolate.html) *.*


- This` API Workflow Engine` ensures the user’s scripts have limited access to the system, are isolated from each other, and cannot abuse the resources.
- The runtime is a[Deno](https://deno.com/) based server-side` JavaScript` application, and uses[V8 Isolates](https://chromium.googlesource.com/chromium/src/+/master/third_party/blink/renderer/bindings/core/v8/V8BindingDesign.md#Isolate) based` Deno` worker threads to run the user’s scripts in a` sandbox` environment. Those` Worker` threads are run with restricted permissions, just enough to enable script execution with no access to the system or ambient data.
- When a script is executed, the script executor module invokes an isolated worker and passes only the` user code` , required` arguments` &` context` to run the code. The worker is not allowed to access the system, read, or write data, and is bound in time.


Press enter or click to view image in full size


Workflow execution in serverless infra


## Instance isolation in serverless infra


Above the engine layer, UiPath serverless infra that runs the engine, executes it in an isolated execution mode specifically designed to run the API Workflows. This provides isolation, while enabling reuse of ‌instances for speed and scale.


Each job runs as an isolated` Unix` process, within a` microVM` , with a limited set of permissions.


> A` *microVM*` (micro virtual machine) is a lightweight virtual machine that combines the strong isolation of traditional VMs with the resource efficiency of containers. It is an isolated unit of execution within a serverless node, which can internally host and manage multiple workflow processes in parallel. It has necessary services in place to manage resource sharing and work distribution.


- It allows only I/O reads from workspace directory — file system writes are blocked. It has no access to I/O to the other parts of the system
- The memory limit for this process is pre-defined and strongly enforced
- The maximum execution time, as well as CPU times are also bound
- Every` microVM` has` Watchdog` services installed to ensure safe and fair usage of resources


> A` *Watchdog*` service monitors the services and workflow processes within a` microVM` , and intervenes in case of resource (memory, CPU time, total time etc.) abuse.


## API workflow execution flow


It’d help to understand how execution flows within the serverless infra.


- When a new workflow request comes to the` Serverless Control Plane` (a component that manages serverless internal traffic flow, load balancing, etc.), it finds a` microVM` with available capacity to execute the workflow. Otherwise, it spins up a new` microVM`
- The` microVM` internally finds a suitable, available process or spins up a new one (given it has capacity), and passes it the details to start the execution
- The process loads the workflow engine to start the execution routine
- The engine internally parses the workflow, validates it, then starts executing the tasks by invoking the corresponding handlers
- If there are script tasks (invocation of user scripts) it invokes the script worker, which is a` V8-Isolates` based sandbox, with the user code and required arguments
- Multiple such workers can run at once, to support parallel execution and scale. The workers are designed not to share any context between executions
- The workers internally form a small auto-scalable worker pool, improving speed and resource utilization
- The engine reads files and data from the process workspace, executes the workflow, and writes results and logs to designated sockets, which are forwarded to respective observability data stores


## Driving performance, scalability and cost


Now that we fully understand the process, we can see the main benefits it provides.


### Performance


- The new workflow engine is light-weight and leaner (in terms of execution and side effects), making the load time and runtime faster. The whole workflow is executed synchronously as a single unit of work, reducing hops and latencies
- The new workflow files are much lighter compared to traditional workflow files, with no need for additional heavy assemblies for execution, which further improves the performance
- It follows the principle of zero-copy, thus reducing the need for network load, storage, encryption, additional compute etc.


Press enter or click to view image in full size


API Workflows serverless scaling model


### Scale


The` API Workflow Engine` is a compact portable module, with a small memory footprint, enabling high-density serverless execution. The scale is handled at multiple stages -


- All workflow requests comes to the orchestration service, and gets routed to the serverless control plane
- The control plane handles the first level of load, and distributes to a` microVM` within a cluster
- Each cluster has multiple virtual machine nodes, and each node has many` microVM` s. Each` microVM` is capable of routing the request to a suitable workflow process to run the workflow
- These` microVM` s can scale horizontally, creating an infra to handle high demands


### Cost


All of the above-enabling faster startup, faster execution, shared runtime instances-contribute to cost reduction. This leads to overall cost optimization for the system, saving customers money eventually.


## Source Control, Governance


Since the API Workflows are deployed through the central orchestration system, the governance policies and source control can all be managed through[UiPath Automation Ops](https://docs.uipath.com/automation-ops/automation-cloud/latest/user-guide/introduction) .


## Summary


This should give workflow developers and engineers a good understanding of how the new` API Workflows` engine and runtime are designed and developed. In this blog, we have briefly talked about our vision and some of the engineering challenges we faced. Then we discussed our design philosophy, and how it is implemented practically in the system, giving a high-level overview of the system we have built to support systems integrations at scale.


### What’s next


In the future, we will have more blogs focused on specific aspects of the system, diving much deeper, and talking about some of the deep technical challenges we solved, trade-offs made, and our learnings in the process. If you’re as excited as we are, comment and let us know if you’d want to know more about any of the following topics, or something else.


- ` API Workflow` integration with other existing and new` UiPath` products
- ` API Workflow` as synchronous API with external-facing endpoints
- Details of the` serverless` infra and how it is designed to tackle high loads
- Upcoming features like sub workflows, retries, custom functions, etc.
