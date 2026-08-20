---
schema_version: "1.0.0"
document_id: "cadb32c40aab545add28ad2543423adc9940890a5953543010e7897e83df49d0"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/agent-state-machines-for-business-workflows"
published_at: "2026-08-19T09:00:00+00:00"
first_seen_at: "2026-08-20T00:31:35.654845+00:00"
fetched_at: "2026-08-20T00:31:37.682913+00:00"
content_hash: "sha256:db62d4674421cb66eabc7445f375386f6f5b3e4e10cb5fc8ecb94c1b6ae1283d"
---

# Designing Agent State Machines for Long-Running Business Workflows

An agent starts a procurement approval on Monday, gathers quotes from suppliers, drafts a purchase request, and routes it to the VP for sign-off. But on Wednesday, an unrelated deploy restarts the pod it was running in, and everything the agent knew about that approval is lost. Yet, no alarm fires. By Friday, when the requester follows up with the VP about the purchase order, nobody knows where it stands.


The cause of the failed workflow was mundane: a deploy. The deploy monitoring tool showed no errors during the process, and the pod shipped to production and came up healthy. What died was a business process that existed only in an agent's RAM, so the failure affected a single process that nobody could account for.


## **Two lifetimes that have nothing to do with each other**


Prompt chains fail at modeling long-running business processes because they conflate two lifetimes. One lifetime belongs to the business process, governed by business rules, while the other pertains to the deployment process, governed by automations such as autoscalers and OOM kills, which terminate pods once they exhaust their memory quota.


In the[2025 DORA State of AI-assisted Software Development](https://services.google.com/fh/files/misc/2025_state_of_ai_assisted_software_development.pdf) , 16.2% of respondents deployed on demand, meaning multiple times per day. Another 21.9% deployed between once a day and once a week. For roughly four in ten organizations, any workflow that took days to complete spanned at least one deploy. So there's only one question that matters: *What does the system do when the process dies mid-workflow?*


You might think prompt chaining is the answer. Anthropic describes[prompt chaining](https://www.anthropic.com/engineering/building-effective-agents) as *"\[decomposing\] a task into a sequence of steps, where each LLM call processes the output of the previous one."* That description only applies to a process running within a single invocation, but a procurement approval is nothing like that.


Prompt chaining isn't the only source of trouble. Any agent loop inherits the failures distributed systems have dealt with for years. The Restate engineering team put it well in their post[Durable AI Loops](https://restate.dev/blog/durable-ai-loops-fault-tolerance-across-frameworks-and-without-handcuffs) , published in June 2025.


> *In practice, agents behave a lot like distributed systems: every tool call is a remote hop, every user interaction is a pause, every retry risks doing the same work twice. Distributed systems fail in a million tiny ways, and agents inherit every single one of them*


Agents have another problem on top of the distributed systems' failures. A crashed service loses in-flight requests. A crashed agent loses the memory of the workflow it was executing, and a conversation transcript makes a poor record for reconstructing it.


The fix is to model the workflow as a state machine, with its state held in a durable record that outlives any single agent process. Here's a simple test for whether a workflow model is well designed. Start a brand-new process instance. If it can pick up the workflow from a durable record that persists independently of the agent, the state model is correctly designed. Otherwise, it is a prompt chain dressed up to look like a state machine. The context window caches only a snapshot of that state, while the durable record persists.


## **Procurement approval as a state chart**


I'd draw a state machine diagram for the procurement approval process before writing a single line of prompt. Three kinds of events can trigger a state machine to change states.


- The agent's output after it completes a task.
- A human approving or rejecting the purchase request.
- A timer elapsing, since the process takes several days on average.


Over a five-day approval period, the agent might spend only four minutes actually computing, since more than 99.9% of the wall-clock time is humans deciding. Any waiting in these processes should cost the agent nothing and survive restarts. Every waiting state in the workflow needs a timer that advances it to the next state in the diagram.


## **Every LLM call becomes a journaled side effect**


A durable execution engine re-executes an agent's function from the top. But functions that have already run are stored, so the engine doesn't rerun them. Jack Vanlightly, in his post on[determinism in durable execution](https://jack-vanlightly.com/blog/2025/11/24/demystifying-determinism-in-durable-execution) , explains why this matters.


> *Determinism is required in the control flow because durable execution re-executes code for recovery. While any stored results of side effects from prior executions are reused, the control flow is executed in full.*


An LLM call is the least deterministic step in the execution engine. Any LLM call in a workflow needs to be journaled, so re-executing the agent function from the top never runs the same side effect twice.


Vanlightly's running example is a customer who gets double-charged on replay, because a non-deterministic read sends the retried control flow down a different branch, and that branch issues a second charge through a different path. The agent equivalent issues the same purchase order twice, and journaling the non-deterministic steps prevents both occurrences. Idempotency keys on the payment request serve as a downstream safety net, but journaling is what prevents the workflow from reissuing the request in the first place.


Temporal requires non-deterministic steps to run as Activities. Restate wraps them in journaled durable steps. The principle is the same either way. Inngest memorizes every step of the agent's process too.


Temporal limits the number of events that can be stored in the Event History, and long LLM responses can eat into that limit fast for long-running agents. Agents can use Continue-As-New in that case, which starts a new execution that does not share the old execution history. Journaling ensures that every step of the agent is replayed correctly, but keeping the agent up to date poses a separate problem, which I discuss below.


## **Human gates belong in the state model**


One approach is a function that blocks the agent's execution until a human approves. A better approach is to give the agent a waiting state, with transitions to a separate state for each possible human response.


Three different platforms landed on the same solution for including a human in the state machine.


First, Temporal lets a workflow block on a signal from another process, and a workflow blocked on a signal consumes no worker compute while it waits.


Second, LangGraph lets a developer call the` interrupt()` function to save state to a checkpointer, then resume later with` Command(resume=...)` . LangGraph's in-memory state saver loses all workflow state on restart, so a developer needs a persistent checkpointer, such as PostgresSaver, backed by a real database.


Third, AWS Step Functions has a callback task that waits for an external system to return a task token before the workflow continues. Here's the ASL for a workflow like that.


Amazon States Language


JSON


```text
{
"StartAt"  :    "Push to SQS"  ,
"States"  :    {
"Push to SQS"  :    {
"Type"  :    "Task"  ,
"Resource"  :    "arn:aws:states:::sqs:sendMessage.waitForTaskToken"  ,
"HeartbeatSeconds"  :    600  ,
"Parameters"  :    {
"MessageBody"  :    {    "myTaskToken.$"  :    "$$.Task.Token"    }  ,
"QueueUrl"  :    "..."
}  ,
"ResultPath"  :    "$.SQS"  ,
"End"  :    true
}
}
}
```


Any state in the workflow can be queried, reported on, and audited, so you always know which tasks were approved and which were rejected.


## **Every waiting state gets a clock and a destination**


A waiting state without a timeout has delegated its reliability to the good discipline of a human's inbox.


Procurement platforms already treat waiting as a first-class problem. Coupa, for example, supports automated approval reminders that repeat at configured intervals. It also has auto-escalation, which moves a document to the next approver in the chain when the current one doesn't act within the required timeframe. The escalation chain terminates at a final approver rather than looping forever. That final approver is the timeout's destination by another name. State machines make such transitions explicit.


The infrastructure supports very long clocks. But all clocks have ceilings. AWS Step Functions Standard workflows can run, and wait, for up to a year. AWS best practices still recommend setting HeartbeatSeconds, though, so a lost task gets detected instead of waited on forever. The escalation policy plays the same role at the business layer, ending in a decision, an approval by a backup, or an abort that notifies the requester, rather than another period of waiting.


Resuming a workflow after four days paused means re-entering a changed world. However, while restoring the workflow's state is easy, the hard problem is that the world changed while it waited. Vanlightly writes, *"The durable function is not an atomic transaction."* The same decision made with fresh data can land on a different branch than the one taken when the workflow started.


This is a known problem. The usual answer is to run any non-deterministic read as an activity, but that doesn't answer a separate question: *Does the new value for that external state invalidate the decisions already made during the workflow's initial run?* That's a problem for the application layer, and I haven't seen any vendor offer guidance on it. At least not yet. This is something we are figuring out as a field.


One solution here is a state declaration that spells out what it trusts from the checkpoint and what it revalidates upon entry into the state. Any changes to the external world can be routed back to a` pending_approval` state for review by the original authorizer. This will matter more to agents than to plain workflows. The model will happily reason over four-day-old context. But the state machine cannot endorse stale information.


## **Build versus adopt**


Temporal, Step Functions, Restate, Inngest and LangGraph all offer primitives that can support workflow execution. All of them offer some form of step-level checkpointing. All offer durable timers that perform no computation, such as Inngest's` step.sleep()` , which can run for up to a year. And all offer some way of signaling, or being signaled by, external events.


Each has edges. Step Functions requires you to encode every branch in advance in an ASL file. So dynamic branching on a language model's outputs does not fit well with the orchestrator. LangGraph, meanwhile, files checkpoints of the workflow state, rather than the history of events. Run it inside Temporal's plugin, though, and Temporal takes over state durability entirely.


None of these platforms provide the state model for a workflow. The model describes the states in a workflow, the transitions between them, which data lives in the durable record versus the context window, and what data will be validated on the entry into each state. That's the developer's job on every one of these platforms, but the good news is that the solution looks the same on each.


Model it once, independent of any platform, and it will survive a migration.


## **So what / do this next**


Here's the order I'd tackle the problems above:


- **Sketch the state machine for a workflow** before writing any prompts to implement it.
- **For any workflow with a finite state machine, move its state out of the context window** and into a durable record keyed on the workflow ID. Re-check the state in that durable record before trusting the context window to drive the workflow.
- **For any step that calls a language model or reads external state, wrap it in a journaled step.** Otherwise, a workflow re-run will call the language model twice.
- **Give every waiting state a transition to a destination on timeout.** That destination might be a reminder to the supply chain team, a backup approver, or an abort that notifies the requester. Without a timeout in every waiting state, the workflow's reliability depends on a single person's inbox.
- **For any state that commits money to an external system, validate on entry into that state.** If the data has changed since it was written into the workflow, route it back to` pending_approval` for the authorizer to review. The durable record holds whatever was true when the workflow was written, but the entry check confirms it's still true.
