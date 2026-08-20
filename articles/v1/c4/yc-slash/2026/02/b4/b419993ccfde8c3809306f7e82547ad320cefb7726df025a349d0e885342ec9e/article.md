---
schema_version: "1.0.0"
document_id: "b419993ccfde8c3809306f7e82547ad320cefb7726df025a349d0e885342ec9e"
company_key: "yc-slash"
company: "Slash"
source_id: "yc-slash-news-import-9ec40849fc98"
canonical_url: "https://www.slash.com/engineering/blog/card-transaction-processing"
published_at: "2026-02-26T21:12:37.273+00:00"
first_seen_at: "2026-07-24T01:09:43.930358+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:1a97e118834e1636309b137788462bfeac732d507e6c2c53c41b69b533f43988"
---

# Building Card Transaction Processing at Slash

Slash processed over $4 billion in transactions in 2025.¹ We are one of the largest US-based corporate card fintechs on the Visa network by processing volume. Since we fully launched our charge card program in January 2024, we have grown our monthly transaction volume by 30x.


This incredible growth in processing volume created challenges for our most critical card transaction processing system. This is the story of scaling our transaction processing system from 0 to hundreds of millions in monthly volume and the learnings we had along the way.


## Context


Before 2024, Slash was a banking platform that offered a debit card product. Transactions were funded by a checking account, and our transaction processing was relatively straightforward.


We wanted to give our users real credit card benefits (such as higher rewards) without the credit risk, so we partnered with VisaDPS and Column NA and built a secured card program. In Jan 2024, we launched our new secured card program with a much more complicated flow of funds. To understand some of the challenges we faced when scaling our transaction processing system, you first need to understand how card payments actually work under the hood.


## Part 1: The Network


Have you ever wondered how money leaves your credit card when you tap it at a grocery store? There is a whole card payment ecosystem around this seemingly simple flow, with many parties involved in facilitating money movements between merchants and Cardholders, Slash being one of them.


We play an important role in enabling our users to spend with their cards. When a card is swiped, a message containing payment information, such as the merchant name, amount, and POS location, is sent from the merchant to the issuer through the card network (Visa/Mastercard). The issuer makes a decision, and the response flows back up the same path.


VisaDPS is the issuer processor we use, which allows us to communicate with the card network. The important thing for us is that once Visa DPS forwards the ISO message to us, we must respond with approval or a decline within 3 seconds. Within this time, we check the user's balance, evaluate spending controls, run fraud checks, and send back a decision. If we don't respond in time, Visa marks the authorization as a timeout, resulting in a transaction decline.


A decline may not matter much to many; if my card is declined at a grocery store, I can just retry or pull out a different card. But many of Slash's customers are high-spend businesses, and for them, it's mission-critical. Many merchants don’t allow retries for declined cards, and some will even flag the cardholder as potentially fraudulent after a decline, which can block future transactions or even freeze the merchant relationship entirely. A single timeout can cause real damage to our client's business.


Our Real-Time Authorization service (RTA) is the most latency-sensitive part of our infrastructure, handling authorization decisioning within a 3-second window. We deployed some very cool batching tricks to reduce the number of balance checks. Though not a focus in today's blog, we will write about it at some point in the future.


## Part 2: Merchants Can Do (Almost) “Anything”


After the initial authorization is approved, any of the following can happen:


1. Merchants can modify the original authorized amount:


- **Incremental Auth** : The merchant needs more money. Tips at restaurants usually come through as an incremental auth.
- **Pre-authorization** : The merchant authorizes a small amount just to verify the card is valid. Gas stations do this. They pre-authorize a small amount, then send the actual charge once you're done pumping.


2. Merchants can choose to capture or reverse any amount, multiple times:


- **Partial capture** : The merchant captures less than the authorized amount. You authorized $200 at a restaurant, but your final bill was $180. The merchant only captures $180, and we need to release the remaining $20 hold.
- **Full capture/settlement** : The merchant captures the full authorized amount. This is the happy path.
- **Reversal** : The entire authorization is voided. Maybe the transaction was a mistake, or the customer cancelled their order before it shipped.
- **Partial reversal** : Part of the authorization is reversed. You authorized $200 for 4 items, but returned 1 item worth $50 before the order shipped, so the merchant reversed $50 of the original authorization.


3. Other events that can happen at any time:


- **Refund** : Money flows back to the cardholder after a previous capture.
- **Aging/expiration** : Authorization was never captured and has aged out. Visa sends us an aging advice with a zero amount, telling us to release the hold.
- **Force capture** : The merchant captures a transaction that was never authorized through us. Yes, merchants can do this.
- **Declines** : We also receive decline messages for transactions that were declined by Visa before they even reached us.


In the simplest flow, we see one authorization event followed by a capture event, but this is rarely the case. We parse the raw ISO20022 messages we receive from VisaDPS and store them as` AuthorizationObject` entities.


```text
AuthorizationSet   (lifecycle   of   one   card   swipe  )
├──   AuthorizationObject:   type=auth,   amount=  $200  ,   holdDelta=+  $200
├──   AuthorizationObject:   type=incremental-auth,   amount=  $50  ,   holdDelta=+  $50
├──   AuthorizationObject:   type=reversal,   amount=  $50  ,   holdDelta=-  $50
└──   AuthorizationObject:   type=capture,   amount=  $200  ,   holdDelta=-  $200
holdAmount:   $0   ✓
```


This immutable log provides a complete audit trail and allows us to reconstruct the exact state of any authorization at any point in time.


## Part 3: Money Movement


So far, we've discussed authorization and holds, but as an issuer, we also need to facilitate real-money movements when card transactions settle.


Because Slash operates a **secured card** program, every card transaction is a lending transaction. There are four accounts involved:


- **Card Collateral** -- where the user deposits funds. Acts as collateral against their credit line, and is the balance we check during RTA.
- **Loan Account** -- tracks the user's outstanding loan principal.
- **Visa Settlement** -- where disbursed funds accumulate until Visa draws down at eod to pay merchants.
- **Repayment** -- holds funds from collateral and uses them to repay the loan at eod.


```text
User   deposits   funds
│
▼
┌─────────────────────┐
│    Card   Collateral      │   ◄──   Collateralizes   the   loan
└─────────────────────┘


──   On   auth   and   capture   ──────────────────────


┌─────────────────────┐                      ┌─────────────────────┐
│    Card   Collateral      │   ──   Book   transfer   ─►│    Repayment            │
└─────────────────────┘                      └─────────────────────┘


│    Loan   Account         │   ──   Disbursement   ──►│    Visa   Settlement      │


──   End   of   day    ───────────────────────────


│    Repayment            │   ──   Loan   payment   ──►│    Loan   Account         │


┌─────────────────────┐
│    Visa   Settlement      │   ──►   Visa   draws   down
└─────────────────────┘
```


When an authorization comes in, two transfers are created in a hold state: a **loan disbursement** (Loan -> Visa Settlement) and a **book transfer** (Collateral -> Repayment). When a capture event arrives, both transfers settle and real money moves. At eod, the cycle closes: Visa draws down from the settlement account, and a **loan payment** (Repayment -> Loan) brings the principal back to zero.


As the issuer, it is our responsibility to ensure that by the end of the day, the exact amount of all settled spend is


- disbursed into the Visa settlement account for daily drawdown
- moved from card collateral and used to complete the daily loan payment


This means our system must guarantee 100% correctness in the transfers, since any failure at any step will cause a balance mismatch. Since we process a high volume of transactions every day, we also need to ensure that the system has a high throughput.


## Initial Design


With these objectives in mind, we designed our initial solution using our **flow-of-funds** service to orchestrate all these money movements. Flow of funds is a declarative, rule-based orchestration system. Instead of handlers calling each other with state scattered across tables, we define an explicit flow of events triggering side effects.


We have a separate blog post that goes into the design of the flow of funds in much more detail.


In our case, the entire flow of funds is triggered by VisaDps card events, which get mapped to FlowOfFund events listed below:


Flow of Fund Event DPS Card Event Trigger Action


card_authorization.begin


Authorization


Transfer is placed in a hold state


card_authorization.updated


Incremental auth


Updates the original authorized amount, hold amount gets updated


card_authorization.captured


Capture


Funds settle and real money movement happens


card_authorization.reversed


Reversal


Holds get dropped


The flow of funds is defined as follows:


```text
Root   Node   (card_authorization.begin)


└──   SideEffect:   InitiateTransfer   (Loan   Disbursement  )
│     →   Creates   hold   transfer   from   Visa   Settlement   Account
│     →   Type:   loan_transaction   (disbursement)
│
├──   On:   intent.holds_placed
│     │
│     ├──   SideEffect:   CloseCardTransactionAccountHold
│     │     →   Closes   original   auth   hold
│     │
│     ├──   AsyncSideEffect:   InitiateTransfer   (Book   Transfer  )
│         │     →   Moves   funds:   Collateral   →   Repayment   Account
│         │     →   Creates   account   hold
│         │
│         ├──   On:   intent.holds_placed
│         │     └──   SideEffect:   CreateEntity   (TransferHold)
│         │         →   Reserves   funds   for   loan   payment
│         │
│         ├──   On:   intent.completed
│         │     └──   SideEffect:   UnblockTransferHoldForLoanPayment
│         │
│         └──   On:   intent.canceled
│             └──   SideEffect:   ResolveTransferHoldForLoanPayment
│
├──   On:   card_authorization.updated
│     └──   AsyncSideEffect:   VisaDPSHandleUpdatedAuthorizationAmount
│         →   Updates   book   transfer   and   disbursement   amounts
│
├──   On:   card_authorization.captured
│     └──   AsyncSideEffect:   VisaDPSHandleCardAuthorizationCapture
│         →   Captures   book   transfer   and   disbursement
│
└──   On:   card_authorization.reversed
└──   AsyncSideEffect:   VisaDPSHandleCardAuthorizationReversed
→   Cancels   book   transfer   and   disbursement
```


Each AsyncSideEffect is executed asynchronously via an abstraction we call **an action intent** , which is essentially a database record for an action that needs to be executed immediately but doesn't require a database transaction. The objective of using these action intents is to avoid long-running transactions. Each action intent gets executed in a Temporal workflow.


In this initial design, processing was split across many asynchronous queues:


Queue Entrypoint Purpose Ordering


IssuerEvent


VisaDPS webhook


Initiate money movement flow by executing transfers. Fires a card_authorization event, which triggers its corresponding side effect.


Unordered


ActionIntent


Spawned by execution of other queue items


Handles all async side effects


Unordered


BankingEvent


Column NA webhook


Write ledger entries


Ordered per account


TransferRetry


Cron


Retry transfers


Unordered


This design has a few flaws:


- **Database contention when ensuring correctness** : **Ledger-level contention:** If a user has 10 transactions coming in at the same time, balance checks will contend with each other because they all need to read and write to the same balance. (We might write another blog talking about how we optimized our ledger for this issue!)


**TransferEntity-level contention:** Multiple queues can be reading and writing to the same non-ledger entities (` transfer_intent` ,` authorization_object` ,` authorization_set` ) at the same time, so database-level locks are required to guarantee correctness.


During a partial capture, say capture 1 comes in, immediately followed by capture 2. Since these are triggered by separate VisaDPS webhooks, two workflows in the IssuerEvent queue will both try to capture transfers. The way to guarantee ordering is to use database locks, which create database contention


- **Low throughput, hard to scale** : As soon as one queue falls behind, it blocks items in other queues due to the implicit dependencies between queues. This makes horizontal scaling very difficult, since scaling up one queue might flood another. There's no single knob to control the throughput.
- **Bad observability** : To understand the state of a single card authorization lifecycle, engineers had to look across multiple database entities, Datadog metrics, and Temporal workflows. We even had to build a dedicated health-check service to monitor card authorization states).


All of these issues hit us at once during our first P0 incident after launching the charge card program. We came in one morning and found all four queues backed up with hundreds of thousands of items -- and the numbers kept climbing.


A few of our largest users had significantly increased their spend over the weekends, causing a burst of volume that our system wasn't built to handle. Workflows in different queues were fighting for locks on the same entities, causing transactions to roll back. Failed transfers piled into the retry queue, adding even more load. We provisioned more Temporal workers, thinking it would help, but it only created more concurrent database access. This quickly became a vicious cycle that degraded the entire database.


We went back to the drawing board, first-principled how transaction processing should really work, and put out a new solution we now call TPS.


## Solution: Transaction Processing Service (TPS)


The core concept of TPS is that all card events within a given lifecycle are processed by a single Temporal workflow. Each subsequent card event is a signal to that workflow, driving state transitions until it reaches a terminal state.


A card authorization lifecycle is essentially a finite state machine.


State Total Hold Amount Loan Disbursement Book Collateral -> Repayment Description


Initial


N/A


N/A


N/A


No transfer has been created


Intermediate


> 0


Hold, banking event fully processed


Hold, banking event fully processed


Both transfers created and in hold


Terminal (Settled)


0


Settled, banking event fully processed


Settled, banking event fully processed


Both transfers settled


Terminal (Reversed)


0


Canceled, banking event fully processed


Canceled, banking event fully processed


Both transfers reversed


Pending State


Any


Any


Any


Banking events not fully processed, or transfers not yet in the same state


```text
┌─────────────────────────────────────────┐
│   Partial   Capture   /   Partial   Reversal   /      │
│   Increment   Auth                            │
└──────────────────┐                        │
▼                        │
┌──────────────┐                │
│    Authorized    │   ─────────────┘
└──────────────┘
/                \\
Full   Capture   /                  \\   Full   Reversal
/                    \\
▼                      ▼
┌────────────────┐     ┌────────────────┐
│   Fully   Captured   │     │   Fully   Reversed   │
└────────────────┘     └────────────────┘
```


For each state transition to happen, two things need to have taken place:


- A new card event webhook from DPS
- All tasks from the previous transition must be completed (e.g. banking event processing, action intent execution)


If there are still pending tasks, the state transition doesn't happen, and the lifecycle stays in a pending state. We only start processing a new DPS card event once the previous transition has fully completed.


Each task is abstracted into a temporal activity, and they are defined as follows:


```text
1.   executeActionIntentActivity         -   Execute   pending   action   intents
2.   retryTransferActivity               -   Retry   any   failed   transfers
3.   processBankingEventActivity         -   Process   banking   events   from   Column
4.   saveAuthorizationEntities           -   Process   and   save   authorization   related   entities
5.   processIssuerEventActivity          -   Initiate   flow   of   funds   (transfers)
```


A task has two parts:` fetch` checks whether there's any work to do in this task for this lifecycle (e.g., are there pending action intents? any unprocessed account events?). If there's nothing, the task is skipped. If there is,` process` executes the actual work.


Each task returns a state that tells the workflow what to do next:


- ` continueAsNew` : This task may have spawned new work (e.g., executing an action intent triggered a new side effect). Go back to the top of the task list and re-process from the beginning.
- ` nothingToProcess` :` fetch` found nothing to do. Skip this task and move on to the next one.
- ` processed` The task completed its work. Continue to the next task.


This is how the ordering guarantee works. If` executeActionIntentActivity` returns` continueAsNew` , the workflow restarts from the top, ensuring all downstream work is finished before moving on to the next card event.


A code snippet for the executeActionIntentTask


```text
export   const   executeActionIntentTask   =   createTpsTask  <
TPSWorkflowActivityData,
ExecuteActionIntentTaskOutput,
ExecuteActionIntentTaskIdempotencyCheck
>  ({
fetch  :   async   (  input  )   =>   {
const   {   runId  ,   workflowId   }   =   input;


// Get all action intents with matching trace IDs
const   actionIntents   =   await   from  (ActionIntent)
.  where  ((  _  )   =>   _.ai.runnerId.  equals  (workflowId))
.  orderBy  ((  _  )   =>   [_.ai.timestamp.  asc  ()]);


if   (actionIntents.  length   ===   0  ) {
return   {
shouldProcess:   false  ,
};
}


// Only return the first action intent ordered by timestamp ascending
return   {
shouldProcess:   true  ,
toProcess: actionIntents,
};
},
name:   'executeActionIntentTask'  ,
process  :   async   (  actionIntents  ,   deps  )   =>   {
const   actionIntent   =   actionIntents[  0  ];


await   deps  ().applicationServices.actionIntentService.  runActionIntentAndUpdateStatus  (
{
id: actionIntent.id,
}
);


return   {
workflowState:   'continueAsNew'  ,
idsProcessed: [actionIntent.id],
};
},
});
```


Conceptually, a running workflow means the lifecycle it represents is still in a pending state, and one of the following is happening


- Transfer entities are being created
- Action intents are being executed
- Transfers are being retried
- Banking events are being processed


When the lifecycle reaches a new state, one of two things can happen


- If no new signal arrives, the workflow completes, and a new workflow instance with the same workflowId will spawn on subsequent card events.
- If we receive a new card event webhook of the same lifecycle while the TPSWorkflow is processing a previous event in the same lifecycle, our signal handler will set` hasNewSignal` to true, and a re-run will automatically happen.


```text
export   const   {   TPSWorkflow   }   =   createWorkflow  ({
name:   'TPSWorkflow'  ,
workflow  :   async   (  data  :   TPSWorkflowData  )   =>   {
const   workflowId   =   wf.  workflowInfo  ().workflowId;


const   {   card  ,   account  ,   loan   }   =   await   fetchTpsEntitiesActivity  (data);


const   activityArray   =   [];   // containing activities listed above


const   activityData  :   TPSWorkflowActivityData   =   {
lifeCycleId: data.lifeCycleId,
account,
card,
loan,
runId,
workflowId,
};


let   shouldContinueAsNew   =   true  ;


while   (shouldContinueAsNew) {
shouldContinueAsNew   =   false
for   (  const   activityFn   of   activityArray) {
const   res   =   await   activityFn  (activityData);


if   (res.workflowState   ===   'continueAsNew'  ) {
continueAsNew   =   true  ;
break  ;
}
}
}


if   (hasNewSignal) {
// Finally, restart the workflow to process any new events that may have been received while the workflow was running
await   continueAsNew  <  typeof   TPSWorkflow>(newData);
}
},
queue: tpsTaskQueue,
});
```


Now, how exactly did this solve the flaws in the original design?


- Database contention is eliminated. In the original design, contention arises from checks that ensure transfers aren't in an intermediate state before starting new ones. For example, during a partial capture, we had to verify that the` transfer_intent` was in a hold state, so two different card events couldn't capture it at the same time. Since only one workflow now processes a single card authorization lifecycle at a time, there's no contention on non-ledger tables like` transfer_intent` ,` authorization_object` , or` authorization_set` . Sequential task execution guarantees ordering, eliminating the need for guardrail code to check entity states.
- Abstracting the state machine using temporal also gives us all the benefits of Temporal workflow and activities. Every step of transaction processing is now durable. In the event of a failed workflow, Temporal resumes exactly where it left off on the next retry and self-heals.
- Observability became much better. Every step of the execution is in a single unified view. If a workflow is stuck in a failed/retry loop, it means one of the async tasks is failing, preventing it from transitioning to a proper state in the state machine. We easily set up alerts and metrics to monitor the workflow health.
- Horizontal scaling is now easy because everything related to transaction processing lives on a single Temporal task queue; it's as simple as a Kubernetes command. This also allowed us to build auto-scalers based on the temporal task queue metrics.


## Takeaways and next step


Even with all the improvements we got from the TPS workflow, TPS will by no means handle 100x our current volume. There isn't really an "end game" when it comes to scaling our system, and as our growth team continues to cook, the engineering team just needs to always assume that our system will need constant improvements to handle future volume.


There will always be new engineering challenges when it comes to issuing / banking. If you made it here, thank you for reading. If you are interested in the real-world engineering problems we are solving as a fast-growing startup, apply here. We’d love to hear from you.


## Read more from us


### [One Engineer, Three Months, Zero Compromises](https://www.slash.com/engineering/blog/new-mobile-app)


≈ 17 minutes read


### [Building Banking-Grade Stablecoin Rails at Slash](https://www.slash.com/engineering/blog/banking-grade-stablecoin-rails)


≈ 19 minutes read


### [My Internship with Slash](https://www.slash.com/engineering/blog/intern-blog)


≈ 13 minutes read


### [The Facelift](https://www.slash.com/engineering/blog/facelift)


≈ 15 minutes read


### [Winning & Culture](https://www.slash.com/engineering/blog/winning-and-culture)


≈ 4 minutes read


### [Scaling 1M lines of TypeScript: Registries](https://www.slash.com/engineering/blog/scaling-1m-lines-of-typescript-registries)


≈ 5 minutes read


### [Health Checks](https://www.slash.com/engineering/blog/health-checks)


≈ 7 minutes read


### [Type challenge: Datadog Logs Preview](https://www.slash.com/engineering/puzzles/datadog-preview-type-challenge)


≈ 2 minutes read
