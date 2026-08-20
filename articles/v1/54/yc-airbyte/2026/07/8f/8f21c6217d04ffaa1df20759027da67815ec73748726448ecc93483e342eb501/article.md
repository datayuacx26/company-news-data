---
schema_version: "1.0.0"
document_id: "8f21c6217d04ffaa1df20759027da67815ec73748726448ecc93483e342eb501"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/agent-pipeline-approval-gates"
published_at: "2026-07-22T14:41:00+00:00"
first_seen_at: "2026-07-22T23:17:45.253356+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:c8ca2c8b6277a5f193348bca09156978a2fa5cb43180c893d39bacf9e165f5b9"
---

# Implementing Confidence Thresholds and Approval Gates in Agent Pipelines

Most teams treat human-in-the-loop interactions as a UI feature. But the reality is that it's a distributed systems problem. The difference between the two approaches becomes apparent when a routine redeploy restarts the process an agent is paused on and the pending approval disappears with it or when an approver heads to lunch but an agent waits three hours for the approver’s response.


The approval button is the easy part. Everything else, including what has to be true for the agent to pause and resume, is the actual engineering component. This means the async queue, the timeout, and, most importantly, the confidence signal that dictates if the agent even pauses in the first place.


An agent waiting on a human to approve an action is in the same position as a workflow waiting on an external callback function. Both systems hand over their state and computations to a durable storage system and later get instantiated from scratch on a different machine days after the original process.


There are four components that usually get implemented into agents requesting human approval in the wrong order. The approval button gets implemented first. Then, when developers encounter the problem of too many requests for an agent to process, a queue gets implemented. Then, a timeout gets implemented for the approval request after the first run of the agent gets stuck. Finally, after a deploy restarts the worker mid-approval and the agent’s in-progress state vanishes with the old process, they add durable state.


I would implement the durable state first. Then implement the async queue, the timeout, and the confidence threshold. It’s the same idea with context infrastructure.


When I work on context infrastructure, I'm very careful to keep the context of an agent out of its orchestration system. Orchestration does not run the agent or make the decision of when it should pause.


The context of the agent gets implemented such that the business context is stable enough for orchestration to read from. This distinction allows agents to have the trusted state and context they need to function in the real world while also having infrastructure that can handle consequential actions for the agent.


When I work with customers, this becomes apparent in their pain points with agents. Companies that do not have a managed layer for their agents end up burning cycles implementing bespoke connectors for every new integration. On the other hand, companies that do have the managed layer are able to test their integrations in the first week of implementation instead of six-plus months as they would have otherwise spent implementing those connectors.


The same can be said for the four separable components that make up an agent’s approval gate. Each component has a specific role to play in the approval system. The confidence threshold determines whether the agent should even pause. The async queue holds the decision. A timeout determines how long the agent waits before it proceeds. Finally, the persisted state component ensures that the other three components do not get lost during a deploy.


An approval gate is not one feature living in the UI. It cuts across most of the stack at once. Durable state sits in the memory and persistence layer, the approval queue sits in orchestration, and the confidence threshold sits in policy and evaluation. Underneath those, context freshness depends on how data gets acquired, prepared, indexed, and retrieved, and the write itself runs through the agent's action path. Governance sits on top of all of it. Each layer owns a different part of the guarantee, so the gate is only as reliable as its weakest layer.


If any one component fails, the approval gate either turns into a memory leak for the company or a rubber stamp for the human approver.


Both appear to function fine when presented in a demo. But both will be the first to be called “indefensible” during a postmortem meeting after a failed integration run.


## **Causes of Blocking Approval Call Failures in Production**


When I see a scenario where a blocking approval call fails in production, the first thing that I consider is whether or not the approval system can be reconstructed on a new, cold, worker process. If it cannot, then what I see is not an approval gate.


CSS


```text
if needs_approval:
decision =  input  ( "Approve this action? "  )
if decision ==  "yes"  :
execute_tool  (args)
```


### **State Loss During Process Termination**


For agents, the entire workflow state lives in the process’s heap memory. This memory contains the state of the conversation, the tool calls, the arguments passed to those tools, and the AI model’s returned outputs. The process must maintain this state and consume compute cycles while awaiting the approval.


The approval system couples the agent’s workflow to the process that executes it. In serverless environments, when a platform process is recycled, the agent is essentially lost and dies with the process.


Let’s consider a scenario where an agent has paused for a developer to approve a customer refund. A pull request gets merged into the main branch at 2pm. The CI system starts a rolling redeploy of the application. The running process gets terminated by the platform while the agent is still waiting for the developer to approve that refund through the \`input()\` function. After forty minutes, the platform reaps the process. The agent has no state, no tool arguments, and no position in its workflow. When the developer clicks approve on the refund at 2:05pm, they are hitting an endpoint that returns no response.


The agent’s run cannot be resumed. That refund never runs. The logs reveal no trace of what caused the agent to fail.


The quieter version of the same problem happens when the developer heads to lunch for three hours while the agent sits on a call. While the developer is away, the blocked process keeps consuming compute the entire time it sits idle. It also ties up a worker that could have been running other jobs.


The[LangChain team](https://www.langchain.com/blog/runtime-behind-production-deep-agents) has written about how their system survives this kind of situation through the use of a durable execution system. With durable execution, agents can “wait indefinitely for human input, run in the background, survive deploys mid-run, and handle concurrent inputs without corrupting state.”


## **Production Events That Kill Approval Gates**


There are three production events that can kill an agent’s approval gate. Each one starts with a wait loop that the process remembers.


1.


### **Autoscaler scale down**


A process waiting on user input can look like an idle process to the autoscaler. In the case of functions like AWS Lambda and Cloud Run, an autoscaler can scale the function to zero instances. Fargate services can also be configured to scale the number of tasks to zero when the minimum number of tasks is set to 0. Inngest, the creators of the waitForEvent function, state that their platform[will suspend a workflow](https://www.inngest.com/blog/durable-execution-key-to-harnessing-ai-agents) through deployments, scaling events, and infrastructure changes. A process waiting on an \`input()\` function will remain alive and billable while doing nothing until the platform kills it due to inactivity.


1.


### **Process crash**


Any crash of a running process will destroy the state that lives in memory. In the[Temporal HITL tutorial](https://learn.temporal.io/tutorials/ai/building-durable-ai-applications/human-in-the-loop) , durable processes allow workflows to persist through human interactions and failures. Temporal’s durability ensures you maintain control over AI-generated content while reliably handling the human approval process.


1.


### **OOM kill**


Long-lived agents that execute with large context windows can exhaust the memory limit of the process. The operating system may terminate the process with a SIGKILL signal, reported as an exit code of 137. The Temporal Durable Asyncio Event Loop[blog post explains](https://temporal.io/blog/durable-distributed-asyncio-event-loop) that when a Python asyncio process sleeps or starts a task and crashes, its state and ability to resume where it left off are lost.


The same root cause kills the approval in each of these instances. The wait state must move out of the process into a durable orchestration system. This means it cannot live in the memory of the process that executes the agent.


## **Precedent for Workflow Orchestration Patterns**


An agent that is waiting on a human to perform an action is the same as a workflow that is waiting on an external callback function. This pattern has been solved by orchestration systems.


### **Borrowing Proven Orchestration Patterns**


A pattern for durable workflows has been implemented for years. When a Temporal workflow cannot proceed without an external result, the Temporal worker “[batches the Commands](https://docs.temporal.io/workflow-execution) and then suspends progress.” It suspends its virtual thread while it waits for an external result.


Temporal workflows are cooperative. Temporal blocks and resumes the workflow’s virtual thread when work arrives from the worker. AWS Step Functions also uses a waitForTaskToken callback function to temporarily[pause the workflow](https://docs.aws.amazon.com/cdk/api/v2/python/aws_cdk.aws_stepfunctions_tasks/README.html) until an external process returns a task token. Inngest’s waitForEvent exposes a similar functionality. LangGraph also exposes an interrupt function that raises an exception and resumes when a Command(resume=...) object is passed into the agent.


This pattern repeats itself in infrastructure solutions. For decades, databases have existed as a separate system before they were incorporated into the modern data stack. The durable execution systems would have discovered how to suspend a workflow’s virtual thread while awaiting an external result. So, instead of writing our own solution to handle human approvals in agents, we can borrow this proven solution from the durable-execution systems.


### **The Priority of Durable Systems**


The components of an agent’s approval gate depend on each other. An approval system with confidence thresholds but without an async queue will result in blocked threads. An async queue without durable state will result in lost approvals during deploys. Timeout for async queues without durable timer functions will result in timers that die with the agent process.


Once the durable state component exists and survives the process, the confidence threshold, async queues, and timeouts can be implemented as knobs to control the system. Without durable state, the system will forget the human input and the approval gate will not function properly.


## **Confidence Scoring and Pause Decisions**


Durable state, a queued pause, and a timer decide how an agent waits for a human. None of them decide whether it should wait at all. That call belongs to the confidence threshold, and it is the piece teams tend to bolt on last, even though it controls how much work ever reaches a person in the first place.


Both ends of that dial are wrong. Route every action to a human and the approval gate turns into theater, because a reviewer buried in requests stops reading them. Route nothing and the agent takes irreversible actions on its own, like issuing a refund or deleting records, with no one in the loop. The threshold has to land somewhere in between, and where it lands should be a deliberate choice rather than a default the production environment picks for you.


Selective prediction allows a model to decide whether to take a prediction step or to abstain from making a prediction. Such a decision can be modeled as a trade-off between the coverage and accuracy that results from making a threshold in the rejection of predictions. Therefore, the confidence of the model itself may be a signal of where to place such a threshold.


Yet the literature on model confidence signals reveals that the signal is inherently unreliable unless it is engineered into the model. For example, Yang, Tsai, and Yamada in On Verbalized Confidence Scores for LLMs,[arXiv 2412.14737](https://arxiv.org/abs/2412.14737) find that:


“Our results reveal that the reliability of these scores strongly depends on how the model is asked, but also that it is possible to extract well-calibrated confidence scores with certain prompt methods.”


The numbers behind their finding help to reveal that for models with 70B parameters or more, the expected calibration error (ECE) for their confidence scores is around 0.1, meaning that the confidence of the model’s predictions deviates from the true accuracy of those predictions by around 10%. For models with fewer parameters, the verbalized confidence scores became almost independent of the accuracy of the model’s predictions.


The best prompting techniques for reliability inverted with the size of the models. For instance, while techniques like few-shot prompting or ranking were found to significantly degrade the reliability of predictions from models with fewer than 50M parameters, using the raw probability score of the model’s predictions was the most reliable.


**Prompt an agent to act whenever the model’s confidence clears a threshold tied to its raw confidence, and the agent will take actions it feels sure about but is likely wrong about. Flag any threshold tied to raw confidence in a design review.**


In a design review, ask for evidence that the team actually calibrated the model’s confidence signals. “The model said it was confident” is not the same as building a mechanism that calibrates that confidence.


## **The Disconnect Between Confidence and Accuracy**


The confidence of the model’s predictions is likely to lie independently of accuracy in the way that an engineer may believe a human’s confidence in an outcome lies independently of the accuracy of the outcome. Yet the human’s tendency to over-rely upon the language of the models is likely to work against any safety mechanism established by the confidence signal.


Models are found to be systematically overconfident in their predictions and the extent of such overconfidence is worst for the smaller models that are typically used by small teams. For example, Rathi, Jurafsky, and Zhou write in Humans overrely on overconfident language models, across languages,[arXiv 2507.06306](https://arxiv.org/abs/2507.06306) :


“We first analyze the distribution of LLM-generated epistemic markers and observe that LLMs are overconfident across languages, frequently generating strengtheners even as part of incorrect responses.”


This aspect of language models is worrisome because it presents the aspect of oversight from the outside while simultaneously working against the proposed safety mechanism from the inside.


The overconfidence rate for model predictions is the fraction of predictions made by a model that are confidently-stated but which turn out to be wrong. The following table summarizes such an overconfidence rate by model size.


Model Overconfidence Rate


**GPT-4o**[15.22%](https://arxiv.org/html/2507.06306v2)


**Llama 70B**[39.15%](https://arxiv.org/html/2507.06306v2)


**Llama 8B**[49.04%](https://arxiv.org/html/2507.06306v2)


In addition to being overconfident in its predictions, language models are often the target of humans who over-rely upon the model’s predictions as a basis for their own actions. For example, a[related study](https://arxiv.org/html/2401.06730v1) in Human overreliance on language models found that in the early stages of interacting with language models, users tended to rely 7% of the time upon model outputs that included statements that weakened the model’s assertions, while relying 94% of the time upon model outputs that included strong assertions by the model.


This failure mode can manifest itself in an agent whose small model proposes for the deletion of a set of production records. The agent may confidently state its score for the proposed action to be 0.91. This proposed action with a confidence of 0.91 may be routed to a human. The human’s response may be to click the approve button. The action is performed. Yet the model had a 49% chance of proposing an action that was confidently stated but wrong. This way, the human has become a co-signer of an action that the model proposed.


### **Interface Design for Model Uncertainty**


The interface between the agent and the human may be improved by surfacing for the human not the model’s stated confidence in the proposed action by the agent, but also some signal of the model’s disagreement with the action, its uncertainty in the model’s ability to perform the action, or cases in which it has proposed actions that turned out to be wrong.


## **Queue-Based vs. Thread-Based Pauses**


A pause in the execution of an agent should be independent of the thread on which the agent is executing. The request for the human’s approval can be separated from the agent execution that results in that request for approval. The rest of the system will become easier once this decision is made.


### **The Architecture Durable Engines Share**


A durable engine for an agent shares an architecture with the others that have proven to be durable. Such an architecture includes a primitive that allows the agent’s execution to be paused, an identifier for the request that will allow it to be addressed when the human has approved the agent’s action, and a means of resuming that agent’s execution.


When I review implementations of agents, I look for these three elements of the shared architecture before I review the approval screen.


System Pause Primitive Resume Mechanism Addressing Token


**Temporal**` workflow.wait_condition()` Signal via` handle.signal()` Workflow ID + signal name


**Step Functions**` .waitForTaskToken`` SendTaskSuccess` /` SendTaskFailure` Opaque task token


**Inngest**` step.waitForEvent()` Matching named event Event name + match field


**LangGraph**` interrupt()`` Command(resume=...)`` thread_id`


In Temporal, the pause in the execution of an agent is implemented as the condition within which a workflow will wait for a signal and within which a timeout is set.


PYTHON


```text
@workflow.defn
class    HumanInTheLoopWorkflow  :
def    __init__  ( self  ):
self  .current_decision:  Optional  [ApprovalDecision] =  None
self  .pending_request_id:  Optional  [ str  ] =  None


@workflow.signal
async    def    approval_decision  ( self, decision: ApprovalDecision  ):
if   decision.request_id ==  self  .pending_request_id:
self  .approval_decision = decision


await   workflow.wait_condition(
lambda  :  self  .approval_decision  is    not    None  ,
timeout=timedelta(seconds=timeout_seconds),
)
```


For this example workflow, when the workflow is in execution, the current workflow task will be completed. The workflow will enter into an idle state while the workflow waits for the signal and timeout. Regardless of how long the workflow waits - for example, five seconds or five months - the execution of the workflow will be resumed once the signal is sent or the timeout period has elapsed. Additionally, if the downstream system fails in the completion of the task, the workflow will be automatically retried by Temporal without the need to ask the human user for approval.


Inngest takes a more compact form of expressing the same idea.


PHP


```text
const    approval   = await step. waitForEvent  ( "wait-for-approval"  , {
event  :  "approval/received"  ,
match  :  "data.requestId"  ,
timeout  :  "24h"  ,
});
```


Like Temporal, inngest will suspend the function in which the code is written and resolve the await call when the event is received or after the timeout period.


The same payoff is experienced by all of these workflow systems. When an agent’s execution can be paused in a queued manner rather than being blocked on that thread, humans may take an hour or a week to act. Yet the cost of that execution and the reliability of that agent will not be dependent upon the speed with which the human can respond.


The token or thread ID associated with the agent’s execution when the agent was paused is the artifact that connects the agent’s paused execution to the action that will be performed when the agent is resumed. If this artifact is lost, leaked, or guessed by a third party, then the system has lost or potentially compromised the ability to resume the agent.


## **The Necessity of Wait Timers**


Every wait for a human in the execution of an agent must include a timer. Otherwise, the agent will eventually be lost to the execution of the agent’s tasks. The defaults established by an agent system for a timer will not save the system.


In systems like Step Functions, the TimeoutSeconds and HeartbeatSeconds parameters can be configured for a task. For example, TimeoutSeconds can be used to set how long a task will wait before it times out. Tasks with a timeout will wait until the execution reaches the one year service quota for AWS Step Functions.


The[AWS documentation](https://docs.aws.amazon.com/step-functions/latest/dg/sfn-best-practices.html) suggests that this timeout should not be relied upon. For tasks that are to be performed as callbacks to other systems, HeartbeatSeconds can be used to configure how often the task will “heart beat” back to the system that created the task.


### **Defining Timeout Semantics**


The timer carries cleanup semantics too. I don't want timeout hidden in plumbing. The agent logic has to treat expiry as a real decision.


Timeout Action Semantic When I'd Use It


Catch → escalation state **Escalate** Unreviewed request needs re-routing


Catch → terminal fail state **Fail-closed** Irreversible action must not proceed unapproved


No catch, propagate` States.Timeout` **Fail-closed (default)** Hard stop, surface to orchestrator


Default` 99,999,999s` **Wait forever** Anti-pattern: stuck execution, no recourse


The timeout action may be essential to determining how the agent system will respond when the human fails to respond to the request for approval. For instance, for a request for the agent to initiate a refund for a customer whose order was valued above a certain threshold, the timeout action would be to fail-closed. Yet for a system that is to draft an email for a customer that may require human review before it is sent, the timeout action may be to proceed with the default action of allowing the request to proceed.


## **Durable State as the Foundational Primitive**


A snapshot of the agent’s state that exists outside of any process is the piece of the puzzle that every other primitive depends upon. It is the primitive that every other primitive team reaches for in their design.


### **Requirements for Persistent State**


Durable agent state usually takes two forms: LangGraph saves a snapshot to durable storage under a thread_id after every super-step of the agent’s execution. Temporal takes an event history of all the events that occurred during the agent’s execution and stores that in the backend of the system.


In either case, the agent must be able to be reconstructed from this stored state by a process that has seen the agent but never this state, and on a different machine from the one that executed the agent.


The problem of getting an agent to approve an action requires the state to have the same reliability, but also requires the system to be able to retrieve the business context upon which it made its decision: the data, the state of that data, and the system upon which it is dependent.


### **Integrating Business Context**


Indexed context gets used when you need to retrieve the stored, searchable replica of the business context to make a decision. For live data and writes to systems, API access is used instead. Indexed context is for retrieving data quickly, whereas live access to data is used when the action that is being taken requires the most up to date and fresh data from the source.


The production checkpointer concept exists for both LangGraph and Temporal. For LangGraph, using a database such as PostgreSQL is recommended for production use. The in memory storage of the state does not survive restarts of the process that is using the LangGraph system.


LangGraph’s “exit”, “async”, and “sync” modes are used to indicate whether and when the state of the agent is to be persisted between steps in the agent’s execution. Only “sync” mode persists the state of the agent before it begins the next step of its execution. The “exit” mode will not be able to recover from the crash of a process during the execution of a step.


Temporal limits the size of the event history to 10,240 events, but capping at 51,200 events or 50MB of data. If an agent workflow reaches either of these limits, it will automatically initiate the “Continue-As-New” workflow option. This allows the agent workflow to pause and then be resumed by a cold process.


Timeouts, confidence thresholds, and queues will all fail in production if the system is not already capable of pausing and resuming the agent workflow.


## **Determinism and Approval Integrity**


One of the constraints upon agent workflows that is the sharpest of all the discussed constraints is the determinism of the agent.


### **Contract Changes During Workflow Replay**


When an agent workflow is resumed from a saved snapshot of its state, it is required that replaying the agent workflow produce the same execution path as prior execution. This is why durable agent workflow engines separate the deterministic orchestration of an agent from the non-deterministic side effects of the agent’s steps.


Temporal’s documented approach is to ensure that when a workflow calls an activity, that the activity will only be called once during the execution of the workflow, and that the result of that activity is stored in the event history of that workflow. During the replay of that workflow, that result is accessed by the workflow rather than being called again by the workflow.


Temporal’s approach to LLM calls is to ensure they are called as activities so that their output is saved and used by the workflow rather than re-creating the same call and its result.


Because an LLM call can be non-deterministic, any output of a step of the agent that is created by an LLM call cannot be re-created when the agent workflow is resumed. If the human reviewed and approved of a particular action that was taken by the agent, if the agent workflow resumes without pinning the output of that LLM call, then the agent will take a different action than the one that was approved by the human.


When re-running a non-deterministic agent workflow from the start, then, it is not possible to recreate the same outcome as during its initial run. The outcome will be a completely new run of the agent’s workflow.


### **The Nuances of Checkpointing**


Checkpointing in LangGraph is different from the event history based replay of actions that is used by Temporal. LangGraph saves checkpoints of the agent workflow, but during the replay of its workflow, each node within the workflow is re-executed.


The actions within those nodes, such as LLM calls and API requests, are re-fired and may return different results from those that were returned during its initial run. Unlike Temporal, LangGraph does not automatically pin the LLM call within each resumed node.


Therefore, each step of the workflow gets re-run and may result in a different outcome than during the initial execution. For this reason, idempotency is considered a non-negotiable requirement of any node that has the potential to create a side-effect within the agent workflow.


Determinism is the quality that ensures the approval of a plan by a human means that that is the plan that will be executed by the agent. Without determinism, the approval is meaningless.


## **Rubber-Stamping as a Quiet Failure Mode**


This last failure mode requires no engineer to be paged in order to occur.


### **How Review Volume Degrades Judgment**


As the volume of reviews that are performed by humans increases, the level of scrutiny that those humans apply to the system decreases. This is[well documented](https://www.edps.europa.eu/system/files/2025-09/25-09-15_techdispatch-human-oversight_en.pdf) in the literature on human oversight of automated systems.


The quiet failure mode is not signaled by an alert to the engineers, but rather by the increasing volume of decisions that the engineers are forced to review.


Alert fatigue across other systems suggests that this failure mode is occurring at scale. Physicians will override an Electronic Health Record (EHR) alert[49% to 96% of the time](https://www.mlmic.com/blog/khn-clinicians-ignore-ehr-alerts-49-96-of-the-time) . One study into physicians behavior published in the journal of the American Medical Association (JAMA) indicated that[55% of physicians](https://pmc.ncbi.nlm.nih.gov/articles/PMC12307096) will ignore an EHR alert and not read its message.


[Another study](https://strangebee.com/blog/what-is-cybersecurity-alert-fatigue-and-how-to-fight-back) of Security Operations Center (SOC) analysts revealed that 32% of those analysts will simply ignore an alert from the SOC system if they believe the system is not any longer trustworthy.


In the field of HITL (human-in-the-loop) systems, this failure mode is well documented by the literature. For instance, there is a trade-off between setting up an approval gate that is too paranoid such that the human always has to approve of any actions taken by an automated system (rubber stamping every alert until the approval gate becomes theater), versus setting up a system that allows for too many automated systems to take an action without human oversight.


### **The Limitations of Adding Reviewers**


Adding more reviewers will not fix this problem alone. The issue is related to cognitive overload when performing multiple tasks.


Adding more reviewers to a system that receives the same high volume of alerts will lead to the same rubber stamping of automated system outputs.


Reducing the number of automated system actions that reach the human reviewer addresses the root of the problem. This is exactly the reason for the confidence threshold gate at the top of this article.


Approving everything will result in rubber stamping. Allowing all actions to occur without review will result in unauthorized actions by the automated system. Finding a calibrated threshold for the confidence of the automated system will allow the human to review fewer actions, but still those that require review.


## **So What / Do This Next**


Here are the actions that I would prioritize for fixing these failure modes.


- **Put durable state underneath the approval UI first.** Any system that uses durable state such as a database-backed checkpointer or workflow engine will come before the approval button is even clicked. Otherwise, the state that exists only in the worker process can still result in the loss of a pending decision during the next deployment.
- **Make the pause a queued, addressable thing rather than a blocked thread.** Making the pause into a wait condition or interrupt will allow the system to not consume a thread while waiting for the human to take action.
- **Set an explicit timeout and design the timeout branch.** Any action that is taken by the agent must state what is to happen in the event that the action times out. Otherwise, the action will continue to wait for a human without a time-out, a limit, or an intervention from that human.
- **Pin non-deterministic model output across resume.** Making LLM calls an activity will ensure that the output of the model is recorded and used rather than rolled again when the agent workflow is resumed. Additionally, making idempotency an expectation for every side-effectful node ensures that the workflow will not take a different action than it took during the initial execution.
- **Calibrate the confidence signal before wiring it to a threshold.** Any threshold that is used to determine whether an action should be taken by the human should be based upon model prompting with extra skepticism for actions from a small model.
- **Surface uncertainty in the approval interface instead of the model's stated certainty.** Showing the reviewer the uncertainty of the model’s suggestions rather than asking the reviewer to act upon the certainty of the model’s stated opinion will ensure that the reviewer understands the risks of the action.


Human-in-the-loop only works in production with a durable workflow. Every engineer that builds these systems should build the approval button first. The durability of the workflow surrounding the button matters more than anything else.


The durable workflow that will survive a deployment is the one that will allow the automated system to wait for a week before resuming and taking the same actions as it took during its initial run.


This is part of the[Agent Blueprint](https://agentblueprint.substack.com/) series, where I write about the infrastructure patterns that make agents work in production, not just in demos. Subscribe to learn more about these patterns before hitting them in production.
