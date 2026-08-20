---
schema_version: "1.0.0"
document_id: "57bee69d8b9b3c395b5a35c3d00c7a31e52a850541cc04495b61ae44b8647d6a"
company_key: "yc-airbyte"
company: "Airbyte"
source_id: "yc-airbyte-news-import-0f166651abb1"
canonical_url: "https://airbyte.com/blog/build-transactional-workflows"
published_at: "2026-06-10T09:25:00+00:00"
first_seen_at: "2026-07-21T23:17:10.236435+00:00"
fetched_at: "2026-07-28T21:23:21.398471+00:00"
content_hash: "sha256:4a1fd06197b5cb77c98a60d2e029b2894eddd84705a36264bb8f11c72761a383"
---

# How to Build Transactional Workflows Across Non-Transactional APIs

The post-mortems on how agents failed all pointed to the same failure. The agent would make three writes and then fail on the fourth; still, it would be six hours before anyone even noticed the failure.


A typical workflow would write to the CRM software, create an invoice, and notify the customer on Slack. The write to the CRM would succeed. The invoice would fail with a 500 error. And the notification to Slack would have fired during the write since the writes were all made in parallel. This is the reliability problem an agent that can[create invoices from your app](https://airbyte.com/use-case/create-invoices-from-your-app) has to solve before it writes anything downstream.


The customer would have received a confirmation for an invoice that would never come into existence. There would be no transaction to roll back because there had been no transaction at all.


This failure is not a bug in the agents but the consequence of trying to make a transaction across a variety of APIs that do not have any means of rolling back should the transaction fail.


There is a well-known[pattern for this failure mode](https://dl.acm.org/doi/10.1145/38713.38742) in the database community known as the saga, first published in 1987. The idea is to break down a long transaction into a sequence of smaller transactions. Each smaller transaction would have a compensating action associated with it to roll back that transaction if it fails.


Instead of distributed transactions or merely retrying the failed action, a saga allows a sequence of transactions to either all succeed or roll back the failed steps.


Teams that do not implement this pattern with their agents will find themselves deploying[agent workflows](https://airbyte.com/ai-developers) that look good in production and cause downstream failures when a third-party service fails during that workflow.


## **Why Distributed Transactions Don’t Apply**


There are a few reasons why distributed transactions will not apply to agents trying to write to third-party APIs.


First, the distributed transaction model requires that all participating services have access to a transaction manager. This will not be true for third-party APIs.


Stripe does not offer an XA endpoint. Salesforce does not participate in two-phase commits. HubSpot does not expose a transaction coordinator endpoint for agents.


In 2007, Pat Helland, author of a well-known[paper on distributed transactions](https://www.cidrdb.org/cidr2007/papers/cidr07p15.pdf) , called out that “In a system which cannot count on distributed transactions, the management of uncertainty must be implemented in the business logic”, and within the business semantics of the workflow.


HTTP is a stateless protocol. There is no such thing as an HTTP header to hold an XID to lock a database record on the response to that HTTP request. Any database write made on Stripe’s API is immediately visible to other systems. There is no such thing as an unprepared transaction for a database record that has been charged.


Acknowledging this fact is the first step toward fixing the failure of agents trying to commit transactions across third-party APIs.


## **The Saga, Named**


[Sagas](https://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf) were first published in 1987 by Garcia-Molina and Salem in the database community. The problem that existed within the database world at the time was that long-running transactions were locking database resources for extended periods of time.


To combat this, the authors proposed that a long-running transaction could be broken into a series of smaller transactions with compensating actions for each smaller transaction.


The original paper stated that a LLT (Long-Running Transaction) is a saga if it can be written as a sequence of transactions that can be interleaved with other transactions within the database system. The database system will either commit all transactions in the sequence (T1, T2, … Tn) or run compensating transactions (Cj, … C2, C1) to amend the system's record of a partial execution of the transaction list.


The sequence of transactions will either all succeed, or the partial sequence T1, T2, … Tj that did succeed will be undone by running the compensating transactions Cj, … C1.


Other transactions can see the partial execution of the transaction list. Sagas do not provide isolation between transactions. This is a deliberate design constraint of the saga pattern.


By giving a name to the problem, engineers will automatically think of solutions that have worked for others who faced the same problem. This will allow engineers to properly structure their agent with idempotency keys, compensation transactions, pivot transactions, and irreversibility of transactions.


Engineers who do not have an understanding of the saga pattern will produce a much more problematic solution.


## **Why Orchestration Wins for Agents**


Within the space of microservices, there has been an extensive discussion on whether to use orchestration sagas or choreography sagas.


Orchestration sagas use a coordinator to perform each transaction in the workflow. Choreography sagas use events to trigger the next transaction in a workflow.


For agent workflows, orchestration is the better pattern. The coordinator for the workflow can store the state of each transaction and provide end-to-end monitoring for each transaction within the workflow.


[AWS Prescriptive Guidance](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/saga-choreography.html) specifically stated that “end-to-end monitoring and reporting are more difficult to achieve in saga choreography compared with saga orchestration.”


For agents, orchestration offers end-to-end visibility into each transaction. For choreography, this is considerably more difficult to achieve.


Choreography offers fewer benefits to agents. AI agents may require different steps for different model outputs. Each API may have different rate limits and timeouts. The dual write problem, where an event and database write must occur within the same transaction, requires the transactional outbox pattern in addition to the orchestration of the workflow.


Orchestration adds a coordinator to the agent workflow. However, tools like Temporal allow for the orchestration of steps within a workflow while also maintaining the durability of the workflow process.


Choreography with only a few participants in a workflow is acceptable and will eliminate the need to interact with a coordinator. However, most agent workflows for third-party APIs will require orchestration.


## **Compensations Are Semantic, Not Transactional**


When most engineering teams first envision sagas, they envision a compensation transaction that rolls back the original transaction to its initial state.


This will inevitably lead to problems caused by other transactions that have occurred between the original transaction and the compensation of that transaction.


Microsoft’s[Architecture Center](https://learn.microsoft.com/azure/architecture/patterns/compensating-transaction) explicitly states that a compensating transaction does not necessarily roll the system’s data back to the same state as when the original transaction began.


Instead, the compensating transaction rolls back the work that the original transaction performed before it failed.


A payment that has been processed is compensated by another transaction that refunds the customer, not by a transaction that un-processes the payment.


A sent email could be compensated by a transaction that sends a correction to the customer, not one that un-sends the original email to the customer.


The customer’s bank statement will feature both the original transaction and the compensation transaction.


Compensations are based on business rules. The rule that a flight cancellation does not automatically entitle the customer to a refund is a business rule encoded in the compensating transaction for the flight booking.


Compensable transactions can be undone and can occur early within the workflow.


Pivot transactions are the point of no return in the workflow such that any transactions after the pivot transaction must succeed for the workflow to succeed.


Retryable transactions occur after the pivot transaction, are idempotent to allow for retries, and will eventually succeed.


Noncompensable transactions cannot be undone and will occur last in the workflow.


For many teams, the compensation for the Slack notification will be step two in a five-step workflow. Should step four within the workflow fail, the compensation for step two will be attempted in an attempt to undo step two. However, because the customer read the notification, there is no way to undo the transaction. This will lead to a failure in the workflow.


## **The LLM Is the Unreliable Narrator**


For most microservices implementations of sagas, the orchestrator can trust that each transaction within the workflow has completed.


For agents, this is not true.


When an agent[calls a tool](https://airbyte.com/agentic-data/tool-calls-explained) and the request times out, the result the agent sees doesn't necessarily reflect the actual state of the third-party service upon which that transaction was made.


A single agent will make the call to a tool and then proceed with its workflow without seeing the result of that call to the external tool. If the external tool reports an error, the agent’s LLM will find a way to complete the transaction with the external tool.


The LLM will report to the user that the workflow succeeded when it actually did not.


When a write times out at step N, the agent reports that step N succeeded.. The agent will proceed to step N+1. When step N+1 fails, the compensation will be attempted for step N, whose actual execution will be unknown. The saga is operating on a lie.


The fix is to include a follow-up read to the third-party API after every write.


Without this read, the saga will proceed as if the LLM’s narrative is true.


## **Idempotency on Both Directions**


Idempotency has been[discussed in relation to forward writes](https://agentblueprint.substack.com/p/designing-idempotent-write-operations) in the workflow. The parameters for the idempotency key cannot be derived from the output of the LLM.


The LLM is not a deterministic system. Should a retry of the transaction occur, the parameters for the write may be slightly altered, which will produce a different value for the idempotency key. The third-party system will treat this as a new write.


The compensation write also requires this same level of consideration.


Should an LLM specify a refund of $100.00 for a transaction that fails, a retry of that transaction could result in a refund of $99.99.


To handle this scenario, the idempotency key must include a distinguishing term that will ensure that the compensation idempotency key is different from the forward write’s idempotency key.


The Azure documentation specifically states that compensating transactions are eventually consistent. They can fail. The system must be able to resume the compensating transaction from the point of failure. This requires the compensation transaction to be idempotent.


It is important to note that idempotency does not guarantee exactly-once delivery of messages. No system will provide this. However, idempotent transactions will cause the system to achieve exactly-once delivery of effects when messages are delivered at-least-once.


## **Provider-Native Atomicity Where It Exists**


A few third-party APIs do offer atomicity for writes within their platform.


[Salesforce’s Composite REST API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_composite_composite.htm) , for example, supports up to 25 subrequests that are performed within a single API request. The allOrNone parameter controls whether the transaction rolls back if any subrequests fail. The default is false to allow for partial success within a request.


An agent that writes to the CRM software without setting the allOrNone parameter to true will create the same problems with the atomicity of the writes that the saga is supposed to fix.


[Salesforce’s Composite Graph API](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/using_resources_composite_graph.htm) extends this to 500 subrequests.


Stripe offers atomicity for a PaymentIntent, which will result in the successful creation of at most one charge to the customer’s bank account.


For larger batch operations, however, each object would be individually created, and the saga will manage each of these objects.


A look at the APIs for a few other platforms reveals a lack of atomicity for third-party APIs:Salesforce Composite REST. Atomic multi-write with allOrNone=true.Stripe PaymentIntent. Single-object atomicity. No multi-resource rollback.Shopify GraphQL Bulk. Async bulk execution with no documented atomicity across lines.Slack Web API. No batch primitive. Per-call semantics.HubSpot Batch CRM. No atomic primitive confirmed.


Most providers do not offer atomic multi-write. The ones that do are worth using because they reduce the number of compensating actions to maintain.


## **Runtime to Handle Long-Running Sagas**


The sagas that are created within a distributed system will typically take place in seconds.


For agents, however, the transactions could take place over days. They could be waiting for a human to approve a transaction that might take three days to be approved. They could be scheduled to occur at a later date. Or the LLM could be taking a while to complete the steps required by the agent to perform the actions for the customer.


The runtimes that are typically offered by the cloud providers for functions are limited to a few minutes. AWS’s Lambda function will time out after fifteen minutes of runtime. A workflow that is waiting for a customer’s approval will time out and be lost.


A durable runtime will allow for workflows to be parked and scheduled to occur at a later date without losing their state.


The four that exist in the market are Temporal, Restate, Inngest, and AWS Step Functions.


Restate is the cleanest fit for this workflow for which we are writing the agent. AWS has an implementation of Temporal that will work for most teams. Temporal is the de facto standard in most organizations. Inngest is a lighter alternative to the others. AWS Step Functions can be used but will require more effort to implement; for this workflow, use the Standard workflow type instead of the Express workflow. Express will time out after five minutes and will fail to implement workflows that require waiting for human input.


Here’s a Restate example that shows what I’m describing:


JAVASCRIPT


```text
const   bookingWorkflow = restate. service  ({
name  :  "BookingWorkflow"  ,
handlers  : {
run  :  async   ( ctx  : restate. Context  ,  req  :  BookingRequest  ) => {
const   compensations = [];
try   {
compensations. push  ( () =>
ctx. run  ( "Cancel flight"  ,  () =>   flightClient. cancel  (customerId))
);
await   ctx. run  ( "Book flight"  ,  () =>
flightClient. book  (customerId, flight)
);


compensations. push  ( () =>
ctx. run  ( "Cancel car"  ,  () =>   carRentalClient. cancel  (customerId))
);
await   ctx. run  ( "Book car"  ,  () =>
carRentalClient. book  (customerId, car)
);
}  catch   (e) {
if   (e  instanceof   restate. TerminalError  ) {
for   ( let   i = compensations. length   -  1  ; i >=  0  ; i--) {
await   compensations[i]();
}
}
throw   e;
}
},
},
});
```


The ordering of the actions within the workflow is intentional. A “cancel flight” action will be registered as the compensation transaction for a “book flight” transaction before the “book flight” transaction occurs. This has to do with the pivot transaction in the workflow.


If you register the compensation transaction for an action after you register that action, you have a window during which the transaction can fail and leave the system in a state with no transaction to roll back.


One thing that will trip people up is that only the restate.TerminalError error will trigger a compensation transaction. Any errors that are transient will cause the function to automatically retry the same ctx.run block for that transaction.


This is why the forward transactions have to be idempotent. A transient error should automatically be retried. A business rule error will roll back the transaction.


This is a crucial point. The durable runtime will provide teams with a state that survives crashes. It will allow for retries without having to babysit the process. It will allow for workflows to be parked and scheduled for a later time.


It will not write the compensating transactions for a workflow. It will not make non-idempotent actions safe to retry. If either one of these aspects is skipped, the durable runtime will dutifully execute the broken logic that was written by the engineer.


## **A Decision Framework**


Before designing a workflow for agents to perform a series of actions on third-party APIs, answer four questions.


**Does the workflow span multiple independent data stores?**


If your services use a shared data store for transactions, you do not need a saga. A saga exists between services with independent data stores. A saga does not provide isolation between data stores. If your workflow requires data isolation, a saga is not an appropriate architectural choice for your system.


**What is the latency tolerance?**


Any workflow that takes place in under a second will not work with the compensation transactions of a saga. Any workflow that takes place between a few seconds to a few minutes in duration could potentially work with messaging systems that are asynchronous for compensation. For agents, workflows that take several minutes to several days to complete work best with a durable runtime and sagas that are orchestrated by an agent.


**What is the reversibility of each action?**


Action Reversible? Idempotent? Classification


**Reserve inventory** Yes Yes Compensable


**Charge payment** Yes With idempotency key Compensable or Pivot


**Send confirmation email** No No Noncompensable, place LAST


**Update internal DB record** Yes Yes Compensable


**Fulfill shipping order** Partial No Pivot candidate


For each action in the workflow that the agents will perform, determine whether it is a compensable transaction, a pivot transaction, a retryable transaction, or a noncompensable transaction.


The most common mistake will be identifying the wrong action as the pivot transaction. Any action that is irreversible must come after the pivot transaction to avoid leaving behind side effects from the failed action.


**What runtime will handle the workflow?**


If the workflow may take longer than a function timeout, the ephemeral function runtime will fail the workflow. Any workflow that includes waiting for a scheduled time or for a human to perform an action requires a durable runtime.


A framework for making these decisions will allow agents to implement sagas between third-party APIs that survive crashes, automatically retry, and do not require babysitting but require a design with a solid understanding of the problems that exist between agents and third-party APIs.


## **Do This Next**


Here is what I'd prioritize, in order.


- **Audit every multi-system agent workflow for split-state risk.** List every tool the agent calls that produces a write. When there are writes to multiple independent systems, treat distributed consistency and failure handling as part of the design, not as cleanup after launch.
- **Classify reversibility first, then push read-back verification into every write path.** Use the compensable, pivot, retryable, noncompensable taxonomy before writing code. If the saga starts with an irreversible step, the first partial failure produces a state that cannot be cleaned up. Add a follow-up read after every third-party write, or the saga proceeds on the LLM's narrative.
- **Derive structural idempotency keys for both forward actions and compensations.** Use hash(agent_run_id, step_id, tool_name) for forward writes and hash(agent_run_id, step_id, tool_name, "compensate") for compensations. Never derive keys from LLM output.
- **Register compensations before executing the corresponding action.** If the action succeeds but confirmation is lost, the compensation is already registered. And when a provider like Salesforce offers allOrNone=true, use that first because it collapses multiple saga steps into one.
- **Move to a durable execution runtime as soon as the workflow includes long waits or human approval.** Temporal, Restate, Inngest, and AWS Step Functions all support this. Ephemeral runtimes lose state mid-saga.


## **The Shape of the Problem**


Distributed transactions do not cross SaaS API boundaries. Teams that want to avoid split-state failures need to treat multi-system writes as one logical operation with explicit forward actions, explicit compensations, structural idempotency, read-back verification, and a durable runtime that holds the state together across the gaps.


Look at what that actually means in practice. A saga orchestrator. Forward and compensating idempotency keys derived from structural context. Read-back verification on every third-party write. Per-provider audits of which APIs offer atomic multi-write and which don't. A durable runtime that holds state across human approvals. And a per-integration map of which actions are compensable, which is the pivot, and which are noncompensable.


That is the plumbing every team building production agents ends up rebuilding. Most of it has nothing to do with the model and everything to do with making third-party APIs behave like a coherent system. The model race is visible. The plumbing is where the real failures hide, and where the real engineering cost lives.


This is the layer we're building.[Airbyte Agents](https://airbyte.com/) is the data and action layer between agent frameworks and the systems agents have to write to, so teams don't have to invent their own saga orchestrator, idempotency layer, and read-back protocol per integration. If the split-state pain in this article looks familiar, that is exactly the problem we're trying to take off your plate.


This is part of the[Agent Blueprint](https://agentblueprint.substack.com/) series, where I write about the infrastructure patterns that make agents work in production, not just in demos. Subscribe if you want more of these patterns before you hit them in production.
