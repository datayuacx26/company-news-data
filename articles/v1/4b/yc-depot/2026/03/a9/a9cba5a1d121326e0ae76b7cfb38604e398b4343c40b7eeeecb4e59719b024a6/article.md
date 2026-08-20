---
schema_version: "1.0.0"
document_id: "a9cba5a1d121326e0ae76b7cfb38604e398b4343c40b7eeeecb4e59719b024a6"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/lambda-durable-implementation"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:53:35.081205+00:00"
content_hash: "sha256:f462d93d8786c4f55e6f86f4b8e4cfd72d1cdc1239bd3d8ba53069fee3d67498"
---

# Lambda durable functions: Real implementation notes

A regular AWS Lambda function runs your code, and when it's done, it's done. You get 15 minutes max and if it crashes, your progress is gone. If you need to wait for something external, you'll be running compute the entire time your process waits or polls.


We've started working with *Lambda durable functions* to make use of their durable execution. In this post, we'll share the lessons and patterns we've learned along the way.


## Why use Lambda durable functions?


Durable functions have a few notable features that address the state and runtime limits of regular Lambda functions:


- Checkpointed executions: The runtime snapshots the state of the function at defined points, referred to as "steps". If the Lambda crashes, or is evicted or redeployed, it replays using cached results from checkpointed steps.
- Callbacks: You can suspend the execution and then later resume whenever an external event sends a callback. Notably, your function isn't running during that time and running up compute costs.
- Extended execution time: A durable execution can be suspended and resume for up to a year.


## Adding durability to Lambda


Instead of using a regular Lambda as a function call, you can use a durable Lambda as a process that can sleep, wake, and pick up where it left off. When a durable Lambda execution resumes after being suspended or recovering from a crash, it replays. This involves re-executing your function from the top but returning cached results for steps that already completed.


You can opt in to durability by wrapping your normal Lambda handler with the` withDurableExecution` function from` @aws/durable-execution-sdk-js` package.


```text
import   {
withDurableExecution,
type   DurableContext,
type   DurablePromise,
}   from   '@aws/durable-execution-sdk-js'


const   handler   =   withDurableExecution  (  async   (  event  :   MyEvent  ,   ctx  :   DurableContext  )   =>   {
// use durable steps and more from the durable context
})
```


The` DurableContext` gives you the primitives:` step()` for checkpointing,` runInChildContext()` for child workflows,` waitForCallback` /` createCallback()` for suspending, and` promise.any()` /` promise.all()` for coordinating concurrent work.


## Getting durable functions running


### The invocation train


Our durable Lambda functions might last minutes or hours, so we want to invoke them asynchronously. Our first instinct was to use an event source mapping like we do for other tasks in our system. We'd enqueue a message with SQS and the event source mapping runs the correct task. That approach did not work for this case because event source mappings invoke durable Lambda functions synchronously. This means the mapping waits for the entire execution to finish before processing the next batch and if it exceeds 15 minutes, it times out and fails. Synchronous invocation means that we can't utilize the extended execution time that durable Lambda provides.


AWS documents this limitation and recommends an[intermediary function pattern](https://docs.aws.amazon.com/lambda/latest/dg/durable-invoking-esm.html#durable-esm-intermediary) to work around it. We did something similar:


The invocation train: a dedicated worker Lambda fires the durable function asynchronously so the event source mapping doesn't block


We have a dedicated task whose job is simply to invoke the durable Lambda asynchronously by specifying` InvocationType: 'Event'` . It completes quickly and returns, which satisfies the event source mapping, and the SQS message is acknowledged while allowing the durable function to run independently for as long as needed. The` DurableExecutionName` is critical for idempotency. If SQS delivers the message twice and the worker Lambda fires twice, AWS won't create a duplicate execution.


```text
await   lambda.  send  (
new   InvokeCommand  ({
FunctionName:   DURABLE_FUNCTION_NAME  ,
Qualifier:   DURABLE_FUNCTION_NAME  ,   // Invoke via alias (alias name = function name)
InvocationType:   'Event'  ,   // Async invocation
Payload: Buffer.  from  (  JSON  .  stringify  (myPayload)),
DurableExecutionName: executionID,
}),
)
```


One thing to note: because the intermediary worker task succeeds as soon as the durable Lambda is invoked and not when it finishes, the event source mapping won't retry if the durable execution fails later. You need error handling inside the durable function itself. More on that later.


### Safe deploys with aliasing


If you deploy a new version of your Lambda while a durable execution is in-flight, replayed steps will run against your new code instead of the original code used during the first execution. If your new code changes the shape of step outputs or removes steps, replay will break.


We handle this by always invoking through a Lambda alias. On deploy, we publish a new immutable version and update the alias to point to it. In-flight executions will continue on their original version and new invocations hit the alias and get the latest code.


## Durable Steps


Steps are the core primitive the Durable Lambda gives you. Wrapping a chunk of work in` ctx.step()` results in the work getting executed as well as checkpointed so that on replay, a cached version of the results is used without re-executing it.


```text
const   record   =   await   ctx.  step  (  'loadRecord'  , ()   =>   db.  findRecordByID  (recordID))
await   ctx.  step  (  'updateRecord'  , ()   =>   db.  updateRecord  (recordID, {status:   'found'  }))
```


Generally speaking, code inside a step is executed once and checkpointed. Code outside a step will be re-executed on every replay and must produce the same outcome.


### What goes in a step


We wrap anything non-deterministic or side effect-y in a step. This includes things like DB calls, API calls, generating timestamps, and so on. Steps also give you built-in retry, so transient failures in a step don't need their own retry loops as long as the error throws.


Code at the root level runs on every replay so it must be deterministic. Given the same checkpointed inputs, it must produce the same outcome. Good use cases for this are pure computations utilizing the already checkpointed results, control flows, and coordination of awaiting the durable Lambda callbacks.


### Checkpoint limits


One limitation we quickly ran into was the 256K checkpoint limit. Every step output gets serialized and stored as a checkpoint. AWS limits checkpoint payloads to 256KB. If your step returns a large enough object, you'll get hit with the` CheckpointUnrecoverableExecutionError` error which can cause the entire function to fail. Depending on the case, we had a few different solutions to this, but they all boiled down to keeping the large data inside the step and only returning the lightweight data that we actually needed to use. In some places we were able to strip down the returns to actually be just the data that we needed for future code. In other cases we ended up storing some relevant data in the database and just returning DB records as the "summary" so we could read the data and access it in another step.


### Idempotency matters


Because steps replay, any code inside a step might not run on a given invocation. Or it might run and fail which would then cause it to run again next time. Both cases matter.


- Reads are safe: a SELECT that runs twice returns the same results
- Writes need to be idempotent: You can use upserts, check and set, or use conditional updates to ensure you're not making unnecessary updates.
- External calls need deduplication: If you're dispatching work to an external process, you may need to pass a deduplication key where possible to ensure duplicates aren't processed. You can also implement some kind of check and call pattern to track whether something has been called or not.


### But how many steps?


We don't have a hard rule and are learning as we go. We've found more granular steps are better for reliability and debuggability, but they slow down replay speed and increase cost. In practice, we wrap most DB calls and external API calls in their own step and try to group related lightweight operations outside of steps. If you're in a loop, we use indexed step names so they're deterministic across replays.


## Handling callbacks


Callbacks in a Durable context allow you to **suspend compute entirely** . The Lambda stops running and releases its resources and will resume later when an external event arrives and tells it to start up again.


```text
const   result   =   await   ctx.  waitForCallback  <  string  >(  'callback'  ,   async   (  callbackID  )   =>   await   callApi  (callbackID), {
timeout: {hours:   CALLBACK_TIMEOUT_HOURS  },
})
```


` waitForCallback` is a wrapper function that lets you create a callback, submit to an external system in a step, and awaits the result all in one call. This is very convenient, but we've found ourselves wanting to await more than one callback at a time and sometimes waiting for all of them to complete or racing them to wake up and continue the process when the first one completes. For this we use the lower level primitives of` ctx.createCallback` which gives us both the id and callbackPromise.


```text
const   [  promise1  ,   id1  ]   =   await   ctx.  createCallback  (  'task1'  )
const   [  promise2  ,   id2  ]   =   await   ctx.  createCallback  (  'task2'  )


// Send both callback IDs in a single step
await   ctx.  step  (  'dispatch-both'  ,   async   ()   =>   {
await   dispatchBothTasks  (id1, id2)
})


await   ctx.  step  (  'other-work'  ,   async   ()   =>   doSomething  ())


// Now await the results
await   ctx.promise.  all  (  `wait-${  loopIteration  }`  , [promise1, promise2])
```


From the other side, the external process needs to be able to tell the durable execution that it has completed by using those callbackIDs. We've generally architected this so that we have the external system tell our API when it's done and our API will use the callback to tell the Lambda to continue.


```text
await   lambda.  send  (
new   SendDurableExecutionCallbackSuccessCommand  ({
CallbackId: CallbackId,
Result: Buffer.  from  (  JSON  .  stringify  ({taskID, success:   true  })),
}),
)
```


### Racing multiple callbacks


Where it gets interesting is when you need to fan out to multiple external processes and react as each one finishes. We experimented with two approaches: callback per task and callback per batch.


#### Callback per task


With this first approach, each individual task gets its own callback. You can use the` ctx.promise.any()` to race them and wake up whenever any single one completes. When the` ctx.promise.any` resolves, you process the completed task, potentially start more, and then loop back to wait again. The unconsumed promises from still-running tasks carry forward into the next iterations. The pros of this include:


- Fine-grained control. You react to each completion individually
- Natural fit for a dependency graph
- Partial failures are isolated


The cons:


- Each callback adds overhead. With several parallel processes, that can be a lot of open callbacks and checkpoint state
- ` ctx.promise.any` may replay slower since the sdk has to match up more state on every resume


#### Callback per batch


Instead of a callback per process, you create one for the entire batch. When any process finishes, it sends the shared callback to wake execution where you can utilize some other state like DB state to see what changed. The pros of this include:


- Simpler state management, a single callback to track instead of one per task
- Less checkpoint overhead


The cons:


- You lose information about which process triggered a wake and have to query the database to figure out what changed
- You have to handle race conditions when multiple tasks completing result in the same callback id being used, and then you need to handle the discrepancies


#### Where we are


We've ended up using both callback per task and callback per batch. We started with the batch approach for everything but have iterated towards the per task approach for our main flow. We have found a use case for an additional rotating callback that functions more like the batch approach. We use these to send different signals to the execution rather than necessarily tracking the various processes we are awaiting. This is an area we are still iterating on. The durable execution model is new enough that we're still trying to establish the patterns that work for us so we'll continue to experiment.


## Error handling


Durable Lambda has distinct failure modes that need different treatment.


### Step-level retries


Some types of errors we want to retry due to their transient nature. Others, no matter how many times you retry, will continue to fail. The pattern we're generally following in steps: **throw** to retry and **return** to complete.


```text
// Transient failures in steps are retried automatically
const   record   =   await   ctx.  step  (  'fetchRecord'  ,   async   ()   =>   {
return   await   db.  findRecordByID  (recordID)   // If this throws, the step retries
})


// You can also configure a custom retry strategy
const   result   =   await   ctx.  step  (
'callExternalAPI'  ,
async   ()   =>   {
return   await   externalApi.  call  (params)
},
{
retryStrategy  : (  error  ,   attemptCount  )   =>   ({
shouldRetry: attemptCount   <   3  ,
delay: {seconds: Math.  pow  (  2  , attemptCount)},
}),
},
)
```


### Permanent vs. transient handler level errors


When an error is thrown at the handler level in the root` withDurableExecution` handler, the actual execution is marked as failed. Since we want to control cleanup regardless of error type, we catch all errors and return a result rather than letting the execution fail.


```text
const   handler   =   withDurableExecution  (  async   (  event  ,   ctx  :   DurableContext  )   =>   {
try   {
return   await   doTheThing  (ctx, event)
}   catch   (error) {
// Permanent errors: clean up and return a result
if   (error   instanceof   ValidationError  ) {
await   ctx.  step  (  'markFailed'  , ()   =>   markAsFailed  (id))
return   {status:   'failed'  , reason: error.message}
}


// Transient errors: clean up, but the execution still completes as failed.
await   ctx.  step  (  'markFailed'  , ()   =>   markAsFailed  (id))
return   {status:   'failed'  , reason:   'transient error'  , retriable:   true  }
}
})
```


### Infrastructure level errors


Some failures happen outside your handler in the durable execution framework itself like the checkpoint serialization error mentioned before as well as other sdk failures. These can show up as either:


- Execution level failures: the handler returns` {Status: "FAILED"}` instead of your return value
- Invocation level failures: an error is thrown around your handler


These can be handled by creating an outer wrapper to catch both types and clean up.


```text
const   durableHandler   =   withDurableExecution  (  async   (  event  ,   ctx  :   DurableContext  )   =>   {  ...  })


export   const   handler   =   async   (  event  :   unknown  ,   lambdaCtx  :   LambdaContext  )   =>   {
const   id   =   extractID  (event)


try   {
const   result   =   await   durableHandler  (event, lambdaCtx)


// Execution returned but with a framework-level failure
if   (result?.Status   ===   'FAILED'  ) {
await   safeCleanup  (id)
}


return   result
}   catch   (err) {
// Framework-level crash — clean up and rethrow
await   safeCleanup  (id)
throw   err
}
}
```


Notably our` safeCleanup` swallows its own errors so as not to mask the original failure. We rethrow because these errors are ones the SDK intentionally surfaces to crash the invocation, allowing the durable execution service to start a fresh one.


## Our learnings so far


The biggest mindset shift has been building with the assumption that your function *will* replay. Once you design your code with that in mind, it becomes more clear what belongs in a step, what stays at the root, and how you handle errors. Being intentional about which failures get retried vs which are terminal has saved us from a lot of stuck executions.


Durable Lambda functions have been a useful primitive that have been easy to start working with. The feature is still new and I'm sure our patterns will evolve. We're figuring it out as we go and hope sharing what we've learned can help others start using it as well.


## FAQ


What happens when a durable Lambda step returns too much data?


You hit the` CheckpointUnrecoverableExecutionError` — AWS limits checkpoint payloads to 256KB, and the entire execution fails when a step exceeds it. The fix is to return only what downstream code actually needs. In practice that means stripping API responses down to the fields you use, or storing large results in your database and returning a lightweight summary record as the step output.


Why doesn't an SQS event source mapping work for invoking durable Lambda functions?


Event source mappings invoke durable Lambdas synchronously, so the mapping waits for the full execution before moving on. Since durable executions can run up to a year, you'd hit the 15-minute Lambda timeout and fail. The fix is an intermediary Lambda that invokes the durable function with` InvocationType: 'Event'` , returns immediately, and lets the durable execution run independently.


What happens if a callback never arrives and the execution just hangs?


The` waitForCallback` and` ctx.createCallback` APIs both accept a` timeout` option. Set it to a reasonable ceiling for your use case. When the timeout fires, you'll get an error you can catch in your handler to clean up state and mark the execution as failed. Without a timeout, a stuck callback means your execution sits open for up to a year.


Can you share state between a parent durable Lambda and a child spawned with runInChildContext?


Child contexts have their own isolated checkpoint log but run within the same execution — not as a separate Lambda invocation. The parent receives the child's return value directly via await. One thing to watch: if the child's result is larger than 256KB, it won't be cached in the checkpoint log, which means the child context's operations get re-executed during replay instead of reading from cache. For large results, storing them in your database and returning a lightweight reference avoids that re-execution cost.


## Related posts


- [Graceful Shutdown in Go](https://depot.dev/blog/graceful-shutdown-in-go)
- [GitHub Actions Runner architecture: The listener](https://depot.dev/blog/github-actions-runner-architecture-part-1-the-listener)
- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)


Iris Scholten


Staff Engineer at Depot
