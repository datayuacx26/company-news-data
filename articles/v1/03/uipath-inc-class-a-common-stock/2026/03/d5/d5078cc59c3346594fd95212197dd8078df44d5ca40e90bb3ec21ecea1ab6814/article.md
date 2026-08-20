---
schema_version: "1.0.0"
document_id: "d5078cc59c3346594fd95212197dd8078df44d5ca40e90bb3ec21ecea1ab6814"
company_key: "uipath-inc-class-a-common-stock"
company: "UiPath Inc."
source_id: "uipath-inc-class-a-common-stock-rss-2f83a748bf9d"
canonical_url: "https://engineering.uipath.com/state-restoration-in-long-running-agent-workflows-c417ec85c7a8"
published_at: "2026-03-10T12:23:23+00:00"
first_seen_at: "2026-07-20T23:16:59.255384+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:f913fbb54ff81fcf8564d26d9f54607b4050aa415854ec9d19a58d99b1996f7f"
---

# State Restoration in Long-Running Agent Workflows

Machine Learning


Artificial Intelligence


Agentic Ai


# State Restoration in Long-Running Agent Workflows


[Akshaya Vishnu Shanbhogue](https://medium.com/@akshaylive?source=post_page---byline--c417ec85c7a8---------------------------------------)


10 min read


·


Mar 10, 2026


--


Long-running agent workflows that require a person to intervene face a fundamental challenge: how to pause execution during extended waits without wasting compute resources. This post explores three approaches to state restoration—snapshotting, deterministic replay, and checkpointing— comparing how each recovers system state after failure and the trade-offs they introduce in determinism, idempotence, and code complexity. We’ll use a refund processing workflow as a running example to illustrate how each approach handles interrupts and human feedback loops, with particular attention to the determinism and idempotence constraints that enable reliable state restoration.


A customer service agent reviews a refund request, determines it needs human approval, and waits. Three days later, a manager finally responds. The compute instance has been running idle the entire time—spending money and preventing any code updates.


This is the challenge of long-running agent workflows: how do you pause execution during extended waits without wasting resources? The naive approach of blocking doesn’t work:


Press enter or click to view image in full size


***Figure: A naive, thread-blocking approach to maintaining execution state.***


We need to save the execution state, shut down the process, and restore it later when the human responds. But state restoration introduces complexity around two critical properties:


## [Determinism](https://en.wikipedia.org/wiki/Deterministic_algorithm)


A deterministic operation always produces the same output for the same input.


**Deterministic:**


```text
def abs_value(x):    return abs(x)   y = abs_value(-5)  # Always returns 5
```


**Non-deterministic:**


```text
def get_timestamp():    return time.now()  # Returns different value each call   def call_llm(prompt):    return llm.generate(prompt)  # May return different responses
```


## [Idempotence](https://en.wikipedia.org/wiki/Idempotence)


An idempotent operation leaves the system in the same state whether you run it once or multiple times.


**Idempotent:**


```text
def set_status(user_id, status):    database.update(users, id=user_id, status=status)   set_status(123, "active")  # Database: user 123 is "active"  set_status(123, "active")  # Database: user 123 is still "active" (same state)
```


**Non-idempotent:**


```text
def purchase(item):    database.insert(item)    return item   purchase(item)  # Database: 1 purchase record  purchase(item)  # Database: 2 purchase records (different state!)
```


## The Example


Throughout this post, we’ll use a refund processing workflow in which humans can request additional context before making decisions:


```text
def refund(request_text):    # non-deterministic invocation    should_refund = llm.is_refund_request_reasonable(request_text)     if should_refund:      return "automated: refund approved"    else:      # long-running process with potential loop      context_history = []      iteration_count = 0      max_iterations = 10       while True:        human_action = human.review(request_text, context_history)         if human_action == "request_info":          iteration_count += 1          if iteration_count > max_iterations:            return "rejected: too many information requests"           # Human wants more information - fetch and loop back          additional_info = fetch_order_history(request_text)          context_history.append(additional_info)        elif human_action == "approve":          return "human: refund approved"        else:          return "human: refund denied"
```


## Solutions


Three approaches address this challenge: **snapshotting** captures complete runtime state, **deterministic replay** reconstructs state by re-executing code with cached results, and **checkpointing** explicitly serializes state at node boundaries. Each makes different trade-offs between simplicity, determinism requirements, and developer control.


## Snapshotting


Snapshotting captures the complete runtime state of an executing program by serializing its memory, file descriptors, network connections, and CPU registers. This can be done at various levels — individual processes using tools like[CRIU (Checkpoint/Restore In Userspace)](https://criu.org/) , entire containers, or full virtual machines. The granularity chosen affects resource usage and deployment density.


When a human-in-the-loop invocation occurs, the execution state is frozen and persisted to storage. Upon human response, the snapshot is restored, and execution continues from the exact point it was paused — including all open files, network connections, and in-memory state.


For our refund processing example, this would mean freezing the Python interpreter mid-execution during the` human.review()` call, including all stack frames, heap memory, and global state. Upon restoration, the process resumes as if no time had passed.


**Pros:**


1. Language- and framework-agnostic: works with any programming language or runtime
2. Does not rerun code: non-deterministic functions and side effects are preserved exactly as they occurred
3. Perfect state fidelity: captures everything including file handles, network connections, and OS-level resources
4. Flexible granularity: can snapshot at process, container, or VM level depending on isolation and density requirements


**Cons:**


1. Storage overhead for snapshots (tens to hundreds of MBs for process-level, up to several GBs for container/VM-level snapshots)
2. Time-sensitive resources such as network connections timeout, authentication tokens expire, file locks may become stale
3. Cannot update code/patch security issues mid-execution — the frozen state contains the old code


## Deterministic Replay


Deterministic replay is a state restoration technique that works by combining event sourcing with re-execution of code. Instead of serializing the entire program state, the system stores an event history (activity results, signals, timers, etc.) and replays the workflow code using these cached results to reconstruct state.


Temporal is a popular framework that implements this approach. It requires[workflows to be deterministic](https://community.temporal.io/t/workflow-determinism/4027) and strongly recommends[activities be idempotent](https://temporal.io/blog/idempotency-and-durable-execution) .


For our refund processing example, the implementation looks like natural async Python code with a straightforward loop:


```text
@workflow.defn  class RefundWorkflow:      def __init__(self):          self.human_action = None          self.context_history = []       @workflow.signal      async def submit_human_action(self, action: str):          """External system calls this signal to provide human input"""          self.human_action = action       @workflow.run      async def run(self, request_text: str) -> str:          # Non-deterministic LLM call wrapped as activity          should_refund = await workflow.execute_activity(              check_refund_reasonable,              request_text,              start_to_close_timeout=timedelta(seconds=30)          )           if should_refund:              return "automated: refund approved"           # Loop until human makes final decision          iteration_count = 0          max_iterations = 10           while True:              # Suspend until human responds via signal              self.human_action = None              await workflow.wait_condition(lambda: self.human_action is not None)               if self.human_action == "request_info":                  iteration_count += 1                  if iteration_count > max_iterations:                      return "rejected: too many information requests"                   # Fetch additional context as activity                  additional_info = await workflow.execute_activity(                      fetch_order_history,                      request_text,                      start_to_close_timeout=timedelta(seconds=30)                  )                  self.context_history.append(additional_info)                  # Loop continues - wait for next human action              elif self.human_action == "approve":                  return "human: refund approved"              else:                  return "human: refund denied"
```


**Pros:**


1. No explicit state serialization required.
2. Code can be updated via[safe deployments](https://docs.temporal.io/develop/safe-deployments) .
3. Can handle short-lived tokens gracefully by refreshing them during replay.


**Cons:**


1. The workflow code needs to be deterministic. All sources of non-determinism should be offloaded to activities.
2. All I/O should be serializable.
3. Event history size limits make it unsuitable for workflows handling large datasets that need to pass through the workflow.
4. Not suitable for low-latency workflows requiring sub-second completion times, as every decision must be persisted to storage before proceeding.


## Checkpointing


Checkpointing is a state restoration technique that works by explicitly serializing and deserializing the program state at node boundaries in execution. The system saves snapshots of the state after each node completes and restores them when resuming.


## Get Akshaya Vishnu Shanbhogue’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


[LangGraph](https://www.uipath.com/blog/product-and-updates/langgraph-uipath-advancing-agentic-automation-together) is a framework that implements this approach through its[persistence](https://docs.langchain.com/oss/python/langgraph/persistence) feature. It requires developers to represent execution as a graph where the state structure is serializable. An important characteristic: when a node containing an` interrupt()` call resumes,[the entire node re-executes from the beginning](https://docs.langchain.com/oss/python/langgraph/interrupts) .


For our refund processing example, the LangGraph implementation uses a graph structure with explicit state management:


```text
from langgraph.graph import StateGraph, END  from langgraph.types import interrupt  from typing import TypedDict, Optional, List   class RefundState(TypedDict):      request_text: str      human_action: Optional[str]  # "approve", "deny", or "request_info"      context_history: List[str]      iteration_count: int   def llm_check(state: RefundState) -> RefundState:      should_refund = check_refund_reasonable(state["request_text"])      if should_refund:          return {**state, "result": "automated: refund approved"}      return state   def gather_context(state: RefundState) -> RefundState:      info = fetch_order_history(state["request_text"])      state["context_history"].append(info)      state["iteration_count"] += 1      return state   def human_review(state: RefundState) -> RefundState:      # Suspend and wait for external input      state["human_action"] = interrupt("waiting_for_human_decision")      return state   def route_after_llm(state: RefundState) -> str:      return END if "result" in state else "human_review"   def route_after_human(state: RefundState) -> str:      if state["human_action"] == "request_info":          if state["iteration_count"] >= 10:              return "reject"          return "gather_context"      elif state["human_action"] == "approve":          return "approve"      else:          return "deny"   def approve_handler(state: RefundState) -> RefundState:      return {**state, "result": "human: refund approved"}   def deny_handler(state: RefundState) -> RefundState:      return {**state, "result": "human: refund denied"}   def reject_handler(state: RefundState) -> RefundState:      return {**state, "result": "rejected: too many information requests"}   # Build the graph  graph = StateGraph(RefundState)  graph.add_node("llm_check", llm_check)  graph.add_node("human_review", human_review)  graph.add_node("gather_context", gather_context)  graph.add_node("approve", approve_handler)  graph.add_node("deny", deny_handler)  graph.add_node("reject", reject_handler)   graph.set_entry_point("llm_check")  graph.add_conditional_edges("llm_check", route_after_llm)  graph.add_conditional_edges("human_review", route_after_human)  graph.add_edge("gather_context", "human_review")  graph.add_edge("approve", END)  graph.add_edge("deny", END)  graph.add_edge("reject", END)   app = graph.compile(checkpointer=memory_checkpointer)
```


**Pros:**


1. The entire codebase need not be re-executed. Because of this, compute-bound operations can be offloaded to different nodes.
2. Code can be updated by deploying new graph versions, though changes to state schema or graph structure require careful handling of backward/forward compatibility with existing checkpoints.
3. It can handle short-lived tokens gracefully by refreshing them when resuming from checkpoint.


**Cons:**


1. Serialization of state is required.
2. Code is harder to read and write because state must be managed explicitly. As a result, loops and nested structures are harder to reason about and development complexity is high for complex agents.
3. Error-prone due to manual state management — forgetting to persist a variable in the checkpoint can lead to subtle bugs upon restoration.


## Comparison


## Human-In-The-Loop with UiPath


UiPath coded agents implement human-in-the-loop workflows using the` interrupt(CreateAction(...))` API, which creates escalation tasks in UiPath Action Center. When called, it suspends execution, persists state via LangGraph's checkpointing, and automatically resumes when the human responds.


## Refund Processing Example


```text
from langgraph.graph import StateGraph, END  from uipath.models import CreateAction  from langgraph.types import interrupt  from typing import TypedDict, Optional, List   class RefundState(TypedDict):      request_text: str      human_action: Optional[str]      context_history: List[str]      iteration_count: int   def human_review(state: RefundState) -> RefundState:      # Create Action Center task and suspend execution      # When human responds, this node re-executes from the beginning,      # but interrupt() returns the human response instead of pausing again      action_response = interrupt(CreateAction(          app_name="RefundReview",          app_folder_path="CustomerService",          title=f"Review refund request: {state['request_text'][:50]}",          data={              "request": state["request_text"],              "context": state["context_history"]          },          assignee="customer-service-team@company.com"      ))       state["human_action"] = action_response["decision"]      return state   # Other nodes (llm_check, gather_context, routing) follow the  # same graph structure as the LangGraph checkpointing example
```


The human receives a structured task in Action Center with the refund details and options to approve, deny, or request more information. When they complete the action, the workflow automatically resumes with their decision.


Like the checkpointing approach it’s built on, this requires explicit state management and graph-based modeling, but provides enterprise features like task routing, SLA tracking, and audit trails through Action Center integration.


## Handling Non-determinism Within Nodes


A key constraint of LangGraph’s interrupt mechanism is that **non-deterministic operations and interrupts cannot coexist in the same node** . When execution resumes after an interrupt,[the entire node re-executes from the beginning](https://docs.langchain.com/oss/python/langgraph/interrupts) , and non-deterministic operations may produce different results.


Consider this problematic example:


```text
def review_with_llm_triage(state: RefundState) -> RefundState:      # Non-deterministic LLM call      complexity = llm_assess_complexity(state["request_text"])       if complexity == "simple":          # Direct approval for simple cases          state["result"] = "automated: refund approved"          return state      else:          # Complex case - need human review          action_response = interrupt(CreateAction(              app_name="RefundReview",              title="Complex refund request",              data={"request": state["request_text"]},              assignee="senior-team@company.com"          ))          state["human_action"] = action_response["decision"]          return state
```


**The problem:** when the human responds and execution resumes, the entire` review_with_llm_triage` node re-executes. The` llm_assess_complexity()` call runs again and might return "simple" instead of "complex", causing the workflow to skip the human response and take the wrong branch.


**The solution:** separate non-deterministic operations into distinct nodes so their results are checkpointed before the interrupt (see below).


```text
def assess_complexity(state: RefundState) -> RefundState:      # Non-deterministic LLM call in its own node      state["complexity"] = llm_assess_complexity(state["request_text"])      return state   def handle_simple_case(state: RefundState) -> RefundState:      state["result"] = "automated: refund approved"      return state   def handle_complex_case(state: RefundState) -> RefundState:      # Interrupt in a separate node - complexity already checkpointed      action_response = interrupt(CreateAction(          app_name="RefundReview",          title="Complex refund request",          data={"request": state["request_text"]},          assignee="senior-team@company.com"      ))      state["human_action"] = action_response["decision"]      return state   def route_by_complexity(state: RefundState) -> str:      return "simple" if state["complexity"] == "simple" else "complex"   # Graph setup  graph.add_node("assess", assess_complexity)  graph.add_node("simple", handle_simple_case)  graph.add_node("complex", handle_complex_case)  graph.add_conditional_edges("assess", route_by_complexity)
```


Now, when the workflow resumes after the interrupt, only the` handle_complex_case` node re-executes. The` complexity` value was already checkpointed after the` assess_complexity` node completed, ensuring consistent routing.


This limitation highlights a fundamental difference from deterministic replay systems like Temporal, where activity results are cached and replayed deterministically, allowing non-deterministic operations and control flow to safely coexist within workflow code.


## Idempotency Requirements for Node Re-execution


Since nodes containing interrupts re-execute from the beginning when resuming, any operations performed before the` interrupt()` call will run multiple times. This requires those operations to be idempotent to avoid unintended side effects.


Consider this problematic example:


```text
def process_refund(state: RefundState) -> RefundState:      # Non-idempotent operation: sends email every time      send_email(          to=state["customer_email"],          subject="Refund request under review",          body=f"Your refund request is being reviewed by our team."      )       # Wait for human decision      action_response = interrupt(CreateAction(          app_name="RefundReview",          title="Review refund request",          data={"request": state["request_text"]},          assignee="support-team@company.com"      ))       state["decision"] = action_response["decision"]      return state
```


**The problem:** when the human responds and execution resumes, the` process_refund` node re-executes from the beginning. The` send_email()` call runs again, sending a duplicate notification to the customer. If there are system failures and retries, the customer could receive many duplicate emails.


**The solution: move side effects to separate nodes.**


```text
def request_human_review(state: RefundState) -> RefundState:      # Interrupt first, before any side effects      action_response = interrupt(CreateAction(          app_name="RefundReview",          title="Review refund request",          data={"request": state["request_text"]},          assignee="support-team@company.com"      ))       state["decision"] = action_response["decision"]      return state   def notify_customer(state: RefundState) -> RefundState:      # Side effect happens in a separate node, after human review      send_email(          to=state["customer_email"],          subject="Refund decision",          body=f"Your refund request has been {state['decision']}."      )      return state
```


By placing side effects in separate nodes that execute after the interrupt completes, you ensure they only run once. Since checkpoints happen at node boundaries, any state modifications made within a node before an interrupt are lost when the node re-executes. This is particularly important for operations like payment processing, external API calls, or database modifications — they must either be idempotent (safe to re-execute) or moved to separate nodes that execute after the interrupt completes (executed only once).


## Conclusion


State restoration in long-running agent workflows requires careful consideration of determinism and idempotence. While snapshotting offers simplicity and framework independence, deterministic replay provides maintainability through natural imperative code, and checkpointing gives fine-grained control at the cost of increased complexity.


Interestingly, the UiPath human-in-the-loop recommendation takes a hybrid approach: checkpointing state at node boundaries combined with deterministic replay-like behavior for interrupts — when a node re-executes after resuming, the` interrupt()` call returns the cached resume value instead of pausing again. This allows developers to make trade-offs between code complexity and determinism requirements at the granularity that works best for their workflows.


The right choice depends on your specific requirements: existing infrastructure, team expertise, workflow characteristics, and long-term maintenance needs.


**All code examples in this post are simplified for clarity and omit error handling, retries, and other production-level concerns.**


We’re hiring! Join the UiPath Engineering team:[check out our open positions](https://www.uipath.com/careers/jobs) .
