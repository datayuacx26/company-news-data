---
schema_version: "1.0.0"
document_id: "f382d4949b292b4ee4a9083188824b2eb62053b4b19adb44b2212a988ef7199d"
company_key: "yc-happyrobot"
company: "HappyRobot"
source_id: "yc-happyrobot-news-import-a91b87efcc63"
canonical_url: "https://www.happyrobot.ai/blog/inside-happyrobots-workflow-engine"
published_at: "2026-07-16T19:27:40.624+00:00"
first_seen_at: "2026-07-21T22:26:45.397554+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:9c1f7dacf698d1b933d4c21f5520e93c9e50035c63564024d776ec734b20b6cb"
---

# Inside HappyRobot’s workflow engine

[The HappyRobot platform](https://www.happyrobot.ai/product/platform-overview) helps puts agents to work in complex enterprise environments. At the core of the platform is our workflow engine evaluating[complex workflow logic](https://www.happyrobot.ai/product/agents/workflow-logic) . While agents conduct the conversation and reasoning, workflow logic defines the steps, conditions, and agent behavior that keep every interaction on track, regardless of how the conversation unfolds. In this blog post, we dive deep into our workflow orchestrator and the underlying engine which drives all the workflows on our platform.


### Workflow editor


The HappyRobot workflow editor is a low-code/no-code way for our customers and Forward Deployed Engineers (FDEs) who serve our customers, to be able to express solutions for complex business use cases. The workflow UI editor is the primary way our users create workflows. The UI also allows them to inspect runs, audit the agent output, run A/B tests, and more.


While the editor is the primary way our users build, it isn't the only way. Every operation in the UI is backed by our public API, with a typed TypeScript SDK (@happyrobot-ai/sdk) on top: you can create workflows, add and configure nodes, manage variables, publish versions, and trigger runs entirely from code. The low-code experience is a layer over the same primitives - teams that prefer to treat workflows as code can build, version, and deploy them programmatically, and the engine described below executes them identically either way.


### Workflow representation


Our users, using the extremely simple no-code UX we offer, can create tens of thousands of unique workflows within our platform. As is common, we use a nodes and edges relational tables to represent the workflow graph. Our engine constructs an internal representation using these tables, amongst others, before execution begins.


This data model gives us the ability to keep a stable “blueprint” copy of the graph, and an execution model counterpart of the same blueprint. In some cases, the execution counterpart is nearly identical to the blueprint, but because we support code-like execution decisions like conditionals, loops (and breaks), and parallel executions, an execution graph can look drastically different from its original blueprint.


### Workflow executor overview


The core of our workflow executor is our orchestration engine. Whenever our users execute a workflow, that is what is running in the background. A rough sketch of how our execution stack processes a workflow, end-to-end:


#### Engine design principle


A core principle behind our workflow engine is creating a strict separation of responsibility between the orchestrator and the executables (nodes in the graph). The executable objects are self contained and the orchestrator has no idea how they work internally. This is not novel by any means - it’s commonly known as the node - actor model. The orchestrator and node actors have a coordinator / worker relationship.


The orchestrator owns the global view of the workflow graph. It decides which nodes are allowed to run, based on dependency readiness. The node actor owns the local lifecycle of one node execution.


A node actor owns its core set of graph operations within itself and the orchestration engine waits for feedback from the actor when it is completed. A simplified model of the node actor:


A key design decision is that a node actor is responsible for modifying the execution graph as it executes. This is exactly the behavior which allows us to expand loop iterations as the workflow is running without the orchestrator being aware of the details. Once a loop is expanded, we simply continue executing the graph like any other node: one node at a time.


#### Workflow throttling and system shock


Imagine a workflow which loads a list of 10,000 items, iterates over each item and performs a set of actions. As code, this is trivial of course:


```text
1  for   item   in   items  :     2      output   =     do_foo  (  item  )     3      output2   =     do_bar  (  output  )     4      output_3   =     do_voice_call  (  output2  )
```


But for our engine and our infrastructure, large workflows can be *really* harmful if we aren’t careful. Our infrastructure can be horizontally scaled to a degree, but a key challenge that no amount of responsible scaling will solve for is system shock. 10,000 iterations in isolation isn’t much, but it *could* be too much if we try to schedule and orchestrate each iteration near simultaneously amongst all other running workflows.


Given our goal to limit shock, our rate limiting/throttling has to account for two ways a workflow can hammer our infrastructure:


1. Large loops with extensive work within each body: using the example above, we have built in rate limiters within the *engine* itself that throttle executions of each iteration and in our scaling experiments, it has shown to be simple yet extremely effective in reducing system shock. Horizontal scaling can still absorb higher throughput without our infrastructure imploding
2. Large number of independent workflow executions: Our users have API access to launch workflows. Our API limiter has two knobs: number of workflows a user can launch per second, and the number of concurrent workflows they can have running at any given time. The calls-per-second rate limiter spreads out system shock and the concurrency limiter ensures no one client monopolized resources within the platform


#### Voice calls


The high level diagram shows how each node within our graph is executed sequentially. This also means that once a node has executed, our engine immediately tries to execute the next node - but how do we support voice calls in our workflows then? Let’s say we want to complete a call before we proceed to the next node.


Our voice service pods are stateful services where each pod can handle up to 200 voice calls concurrently. Each call is managed as a goroutine within the pod. Our voice agents can be on 30 minutes to 1 hour long phone calls with people at times, support transfers between parties and allow the agents answering the call to run custom tools built by the workflow creator. We also support nodes in our workflows *after* voice agents (e.g., analyze the call, make API calls, store data, launch subsequent tasks). All of this to say that we need a way to “pause” the workflow until a voice call is done *and* ensure that the voice agent can always reach into the workflow’s state to know what has been executed and what tools are available for the agent to use.


So how do we do this? In short, we use signals - a communication layer between the orchestrator pod and a voice agent pod. As we start a voice call within a pod, the stateful nature of the call lets us store the workflow specific information within the voice session. The session handler within the voice pod uses this workflow information to relay signals back to the orchestration engine worker regarding tool calling, session completion, transfers, call status and more. A simplified interaction diagram:


### Conclusion


The HappyRobot workflow engine was created using a combination of event driven architecture (e.g., signals) and strict separation of system responsibilities. This has allowed us to represent extremely complex business logic within a no-code platform, but still execute it as code to perform tasks and actions at scale. The engine is still evolving and we are constantly adding new features and capabilities to elevate the platform from a workflow executor to a truly distributed workflow operations system.
