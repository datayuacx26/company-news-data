---
schema_version: "1.0.0"
document_id: "83f1d6f6acf73812c2d2ac3475c4c0bb6319d85807c28993363add21204036f9"
company_key: "yc-specific"
company: "Specific"
source_id: "yc-specific-news-import-e2a51d9495af"
canonical_url: "https://specific.dev/blog/what-is-durable-execution"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T16:09:56.950104+00:00"
fetched_at: "2026-08-13T16:09:58.669298+00:00"
content_hash: "sha256:a5da98050c76429df600c558a0415f44efd44a66559cf668b0ba0776bce36d69"
---

# What Is Durable Execution?

Durable execution is a way of running code so that its progress survives crashes, restarts, and outages. The runtime records every step a function takes; when the process dies, another one picks up the recorded history and resumes exactly where the function left off, instead of starting over. The result is code that can retry failed calls automatically, sleep for days in the middle of a function, and never lose its place.


This post explains the problem durable execution solves, how the mechanism actually works, and where to start if you want to use it.


## The problem: partial failure


Any multi-step process can fail in the middle. Consider an order flow:


1. Charge the customer's card
2. Reserve inventory
3. Send a confirmation email


If the process crashes between steps 1 and 3, you've taken someone's money and gone silent. The classic fixes are all versions of the same workaround: persist your progress in a database, push steps through queues, build a state machine that knows how to resume, and write reconciliation jobs for when that state machine gets it wrong. Every team ends up building a bespoke, half-tested workflow engine around their business logic.


Durable execution moves that machinery into the runtime, so the code you write is just the business logic:


```text
import   { proxyActivities, sleep }   from   "@temporalio/workflow"  ;
import   type   *   as   activities   from   "./activities"  ;


const   {   chargeCard  ,   reserveInventory  ,   sendConfirmation   }   =
proxyActivities  <  typeof   activities>({
startToCloseTimeout:   "30 seconds"  ,
retry: { maximumAttempts:   5   },
});


export   async   function   processOrder  (  orderId  :   string  ) {
await   chargeCard  (orderId);          // retried automatically on failure
await   reserveInventory  (orderId);    // resumes here after a crash, card is not re-charged
await   sleep  (  "24 hours"  );            // a durable timer, survives deploys and restarts
await   sendConfirmation  (orderId);
}
```


This is a[Temporal](https://temporal.io/) workflow in TypeScript. It reads like naive sequential code, but it will survive the process being killed at any line.


## How it works


Durable execution engines are built on three ideas:


**Event sourcing for function state.** Every meaningful step (an external call started, a result came back, a timer fired) is appended to a persisted event history. The history, not the process memory, is the source of truth for how far the function has gotten.


**Replay.** When a worker crashes, another worker re-executes the workflow function from the top. Steps already in the history aren't re-run against the outside world; their recorded results are returned instantly. Execution fast-forwards to the first step without a recorded result and continues live from there. This is why` reserveInventory` doesn't charge the card again.


**Separating orchestration from side effects.** The workflow function orchestrates and must be deterministic, since it may be replayed at any time. Anything that touches the outside world (HTTP calls, database writes, sending email) lives in activities. Activities get timeouts and automatic retries with backoff, and their results are what the engine journals.


Durable timers fall out of the same model:` sleep("24 hours")` writes a timer event and releases the worker. Nothing holds a thread open; when the timer fires, any worker resumes the workflow.


## What people use it for


The pattern shows up wherever a process is multi-step, long-running, or too important to lose:


- **Money movement** : payments, refunds, billing cycles, exactly-once-style semantics around charge APIs.
- **AI agents and pipelines** : long agent loops and LLM call chains that are expensive to restart from scratch, with retries around flaky model APIs.
- **Provisioning and orchestration** : sign-up flows that create resources across several systems and must not half-complete.
- **Human-in-the-loop** : workflows that wait days for an approval and pick up where they paused.


## The ecosystem


The idea has been proven at scale for over a decade.[Temporal](https://temporal.io/) is the most established open-source engine, descended from Uber's Cadence and used for exactly the workloads above; it has SDKs for TypeScript, Python, Go, Java, .NET, and PHP.[Cadence](https://cadenceworkflow.io/) itself is still active. Newer entrants like[Restate](https://restate.dev/) ,[Inngest](https://www.inngest.com/) , and[DBOS](https://www.dbos.dev/) explore lighter-weight takes on the same core idea, and the cloud vendors have their versions in AWS Step Functions and Azure Durable Functions.


## The tradeoffs


Durable execution isn't free, and it's worth knowing what you're signing up for:


- **Determinism constraints.** Workflow code is replayed, so it can't use random numbers, current time, or direct I/O outside the engine's APIs. The SDKs give you deterministic equivalents, but it's a real constraint to learn.
- **Versioning.** Long-running workflows may still be mid-flight when you deploy new code, so changing workflow logic requires care (engines provide patching and versioning APIs for this).
- **Operating the engine.** The engine itself is stateful infrastructure that has to run somewhere and be highly available. This is the piece most teams underestimate.


That last one is the adoption blocker in practice, and it's the part that can be entirely outsourced.


## Trying durable execution in minutes


In[Specific](https://specific.dev/) , a Temporal engine is one block in your project's config file:


```text
temporal   "jobs"   {}
```


` specific dev` runs a local Temporal server automatically, Web UI included.` specific deploy` provisions a managed[Temporal Cloud](https://temporal.io/cloud) namespace and injects the credentials into your services. Your code uses the standard Temporal SDKs either way. We cover the details in the[Managed Temporal guide](https://docs.specific.dev/guides/temporal) in our docs.


The fastest way to see durable execution working is our task queue example:[walkthrough](https://docs.specific.dev/examples/durable-workflows) ,[live demo](https://workflows.spcf.app/) , and[source code](https://github.com/specific-dev/examples/tree/main/node-workflows) .


To start in your own project, give your coding agent this prompt:


```text
Help me get started with Specific by following: https://docs.specific.dev/for-ai/onboarding
```


Or install the CLI yourself:


```text
curl   -fsSL   https://specific.dev/install.sh   |   sh
```
