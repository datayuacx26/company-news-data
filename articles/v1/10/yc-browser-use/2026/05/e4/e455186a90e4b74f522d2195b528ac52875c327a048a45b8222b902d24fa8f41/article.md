---
schema_version: "1.0.0"
document_id: "e455186a90e4b74f522d2195b528ac52875c327a048a45b8222b902d24fa8f41"
company_key: "yc-browser-use"
company: "Browser Use"
source_id: "yc-browser-use-news-import-3219c37ba697"
canonical_url: "https://browser-use.com/posts/production-architecture-browser-use"
published_at: "2026-05-09T00:00:00+00:00"
first_seen_at: "2026-07-21T11:46:29.258885+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:a7a105da2d3fbc3d90b11ce31e022fa674bd83afb39e1781ad432adb09aaa3b9"
---

# A Production Architecture for the Browser Use Open-Source Library

We open-sourced[browser-use](https://github.com/browser-use/browser-use) so anyone can run a browser agent locally with a few lines of Python.


Running *millions* of those agents in production with retries, timeouts, screenshots, audit trails, and billing requires infrastructure that took us 4,000+ commits to get right. This post is the architecture we use in production. If you want to run browser-use at scale in your own infrastructure, this is a good starting point.


## What the open-source library gives you


The Agent class is the unit of work. You pass it a task, an LLM, and a BrowserSession. On each step it takes a screenshot and extracts the DOM in parallel, sends both to the LLM to decide the next action, executes that action, and repeats until the task is done.


```text
from   browser_use   import   Agent, BrowserSession


agent   =   Agent(
task  =  "Book a table for two at the closest sushi place"  ,
llm  =  ChatAnthropic(  model  =  "claude-sonnet-4-6"  ),
browser_session  =  BrowserSession(),
)
result   =   await   agent.run()
```


This is the one method call we make in production. That's the entire open-source library. Everything else in this post, the queues, the workers, the state management, the retry logic, is the infrastructure we built around it.


## The architecture


The API is a FastAPI service on ECS Fargate. It accepts task creation requests, validates them, writes a row to the database, drops a message on SQS, and returns HTTP 202.


Behind it is a standard SQS queue with one message per agent run carrying the task ID and execution config. No ordering, no deduplication, no separate queue per workload type since agent tasks are independent of each other.


The worker is an AWS Lambda function with` from browser_use import Agent` at the top. It pulls messages off SQS, instantiates the agent, runs it to completion, and writes results to S3.


The complexity is in how each piece handles failure.


### API entry


The handler validates the payload, creates session and task rows in the database, commits, sends an SQS message, and returns HTTP 202 with the task ID in under 50 milliseconds. The work hasn't started yet.


### The queue layer


We use a single standard SQS queue for all agent runs. We tried per-customer queues and priority-based routing early on but none of it improved throughput and all of it added operational overhead we didn't want to maintain. A single queue with independent messages turned out to be enough.


The message carries the task ID, model and runtime settings, and a continuation counter that starts at zero:


```text
{
"agent_task_id"  :   "a1b2c3d4-..."  ,
"llm_model"  :   "claude-sonnet-4-6"  ,
"max_agent_steps"  :   100  ,
"use_vision"  :   true  ,
"continuation_count"  :   0
}
```


### The worker Lambda


The Lambda handler is wired to the queue via SQS event source mapping. For each message it parses the payload, provisions a browser session, constructs the LLM client, calls` Agent(...).run()` , writes step-by-step state to S3, and persists the final result to the database.


### State in S3


We store four kinds of state in S3. Agent checkpoints are serialized after every step as JSON, which is what makes resumption possible. Screenshots are captured on each step for both the agent and for humans debugging failed runs. Execution logs are uploaded once on task completion. Output files like downloads or generated artifacts are served back through presigned URLs.


The uploads are fire-and-forget. If S3 is slow or returns a 503 on one upload, the agent run continues because we would rather lose a screenshot than fail a task.


## When Lambda runs out of time


AWS Lambda has a 15-minute hard limit, but browser agents don't. A task that takes 20 minutes shouldn't fail because of a runtime constraint.


Two minutes before the Lambda deadline, the agent stops gracefully:


```text
# Inside the agent loop — check before each step
if   time.monotonic()   >=   deadline:
_time_limit_stop   =   True
return   True    # signals the agent to stop after this step


# After agent stops, re-queue with incremented counter
next_count   =   event_data.continuation_count   +   1
continuation_data   =   event_data.model_copy(
update  =  {  'continuation_count'  : next_count}
)
sqs.send_message(
QueueUrl  =  AGENT_TASKS_QUEUE_URL  ,
MessageBody  =  continuation_data.model_dump_json(),
)
```


The handler checkpoints state to S3, sends a new SQS message with the continuation counter incremented, and returns success.


A new Lambda invocation picks up that message, fetches the state from S3, restores the Agent, and resumes from the last completed step. The user sees one task while the system stitches together N Lambda invocations through S3.


We cap the counter at 12 right now, which gives us about three hours of wall-clock time. This isn't a hard constraint. We could raise it, but in practice we found that agents aren't reliable enough at that many steps to justify running longer.


## Failure handling


Lambda already gives you retries and SQS already gives you a DLQ, so we didn't need a custom retry framework. We just needed to know when to use which.


If the handler throws, we report the message as failed via` ReportBatchItemFailures` . SQS puts it back on the queue when the visibility timeout expires and a different Lambda invocation picks it up. After three failed attempts the message goes to the DLQ.


There is no retry counter in our database. The retry state lives entirely in SQS metadata, so if we need to know whether a task is on its second attempt we check the message attributes, not our own tables.


The DLQ is a fire alarm. When messages land there it usually means we introduced a regression or there's a deeper issue to investigate. It's rare, and when it happens an engineer looks. We don't auto-redrive.


## Ephemeral storage cleanup


One thing we learned is that Lambda's ephemeral storage (` /tmp` ) does not auto-clean between invocations on a hot Lambda. If a previous agent run left files in` /tmp` , the next invocation on the same Lambda instance will see them. This can leak data between sessions or cause disk space issues on long-running instances.


We wipe the workspace at the start of every invocation and scope all writes to session-specific directories:


```text
shutil.rmtree(  '/tmp/agent-workspace'  ,   ignore_errors  =  True  )
os.makedirs(  f  '/tmp/agent-workspace/  {  session_id  }  '  ,   exist_ok  =  True  )
```


## Why this works


The Lambda holds nothing across invocations. Everything that needs to survive lives in the database or S3, so we can kill workers without losing work.


The API doesn't wait for the agent. It accepts the task, drops it on the queue, and gets out of the way. Customers never wait on agent execution at the API layer.


Lambda's 15-minute limit was the biggest open question early on. Solving it with "save state, re-queue, restore state" instead of switching to a different runtime let us keep using Lambda's auto-scaling and SQS's retry semantics without introducing new infrastructure.


## What survived


The library turns a natural-language task into browser actions. The infrastructure keeps thousands of those tasks running concurrently.


Getting both to work together reliably took[4,000+ commits and a lot of mistakes](https://browser-use.com/posts/everything-i-got-wrong) . We've hit every failure mode SQS and Lambda can throw at you, from event loop deadlocks to ephemeral storage leaking between invocations to Lambda continuations silently dropping state. The architecture in this post is what survived all of that.


[Browser Use Cloud](https://cloud.browser-use.com/) runs the open-source library on this architecture, plus everything we've built on top.
