---
schema_version: "1.0.0"
document_id: "8fa32a9670d6863818302358d825842c6530973febfd9d7325435bc08792cf88"
company_key: "yc-uselemma"
company: "Lemma"
source_id: "yc-uselemma-news-import-dd56f4936f86"
canonical_url: "https://www.uselemma.ai/blog/a-taxonomy-of-agent-failures"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-22T02:10:55.235409+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:57349a0daa424557ea6395b973027dfddfc7a18272b7ed08e06e8ce4d938e457"
---

# A Taxonomy of Agent Failures

An action cannot tell you whether it was correct.


It cannot tell you whether it fulfilled the request, violated an instruction, repeated a failed attempt, relied on something false, or left the user convinced nothing happened.


Correctness depends on information the action itself doesn't contain.


Yet most production monitoring still evaluates agent actions as though the span contains everything needed to judge correctness.


Imagine you have an agent in production. Users are churning. You pull up the traces: thousands of them. Latency, error rates, cost analyses. All look fine. Something is wrong and there's nowhere to look.


That's the actual problem. Traditional software usually failed in ways you could observe, such as an exception, timeout, or a crash. AI agents don't. Often, they return successfully while the failure stays concealed.


That's what we mean by *silent failure* . The agent appears to succeed, but the intended work never happened, or happened incorrectly. It may ignore part of the user's request, continue after a critical tool fails, or invent a record and log it as complete.


An agent that skips half its work and reports back 'done' has no name for what went wrong, so it gets logged as a success and shows up three weeks later as churn.


We've spent the last several months watching agents fail in production across millions of traces. The failure modes look different on the surface, but underneath they're the same shape: none can be caught by looking at an action on its own. Each is defined by a comparison against something the action itself doesn't carry.


There are four things an action gets compared against. What the agent was told. What already happened in the run. What was established as true. What the user took away. Every silent failure below is a mismatch against one of these four references.


Rendering diagram…


Here's what they all look like.


## What the agent was told


Three of the seven modes are failures against instructions. These instructions come in two kinds: obligations say *do X* , constraints say *never do Y* . Together they produce three different failures: an obligation left unmet, an action with no obligation behind it, and a constraint broken.


### Skipped Work


The agent was prompted to do something. For some reason it didn't do it. No error or exception appeared, it just didn't.


This is the hardest failure mode to catch in production. The agent returns a response, and that response is plausible. It acknowledges the task, and then the task never gets done.


The agent often does adjacent work. It answers a related question, completes nearby subtasks, and fills the hole with something close enough to feel correct. Sometimes it goes further and reports the work complete: logs a confirmation, returns a receipt, or claims it updated a record that was never written. By the time the user notices, the context is gone.


What the execution layer sees is a completed turn. Tokens in, tokens out. There's not a signal that an obligation was left open.


Catching such events requires holding what the agent was asked against what it did, across the full conversation. You need to track the instruction as a stateful object, follow it through the interaction, check whether it was fulfilled, and whether the agent's claim about fulfilling it is true.


This is bigger than a trace problem. Traces tell you what the agent executed. They don't tell you what was supposed to execute and didn't, or that the agent said it did.


### Out of Scope Work


The agent did something but nobody asked it to.


Skipped work is an obligation opened and never closed. This is the inverse: an action runs that no obligation ever opened.


The agent did something. However, nothing in the task called for it. Sometimes that's a wasted step, an agent modifying a resource it was never scoped to touch, or taking an action with side effects the user never authorized.


The agent has a goal and a set of capabilities, and nothing structurally binds the second to the first. Nothing says "only take actions the task actually requires." So when a path to the objective runs through an action that was never in scope, the agent takes it, the same way it finds a way past an ambiguous rule. The action is available, so it's on the table.


Here, the execution layer sees a completed action with no error, because doing extra isn't an error to anything watching in the span.


Catching it requires holding the action against the full set of obligations in force and asking whether any of them called for it. If nothing did, that's the flag.


### Instruction Violation


Simply put, the agent broke an explicit rule it was given. For example, a rule was given in the system prompt. To the human reader, it's explicit and unambiguous. However, the agent reasoned its way past it, and found a path to its goal that the rule should have blocked, took it, and returned success.


The reason this happens is structural. A system prompt isn't a hard boundary, it's a soft constraint living inside the same reasoning loop as the goal. When the goal is concrete and the constraint is ambiguous, the goal wins. What matters is that compliance is relational: the same action is permitted or prohibited depending on the rule in force when it ran.


A violation isn't defined by the action alone. An agent that concludes a rule no longer applies has already violated the instruction.


This is the sharpest articulation of the whole pattern: a violation is not a property of the action. It's a relation between two artifacts, the instruction and the action, that never appear in the same log line. The instruction either lives in a system prompt or an earlier user message. The action lives in the span, many tokens later. No execution signal connects them, because execution monitoring doesn't know the instruction exists.


The execution layer sees a completed action.


Catching it requires the instruction and the action to exist in the same evaluation context.


## What already happened in the run


The next two modes use the run's history as their reference. They compare the current action against earlier errors or previous attempts.


### Integration Failure


This one partially surfaces in execution monitoring. Here you can see the error, but you can't see what the agent did about it. For example, a tool call errored, but nothing in the agent's behavior shows it dealt with that.


The detectable property is precise: an error occurred and there's no visible failure handling. The surface behavior varies (sometimes the agent masks the error and reports success; sometimes it just stops), but the underlying failure is the same thing: an error existed and the handling didn't.


The absence of recovery is the failure. That's a judgement about behavior, not an error code. Error monitoring is responsible for telling you that a tool call failed, not whether the failure is handled, and you need both.


### Retry Loop


The agent repeatedly attempts the same action despite receiving the same failure.


This looks like a performance problem. Latency spikes, cost anomalies, spinning. It is one. But the root failure is behavioral: the agent has no stopping condition for repeated failure. It keeps trying because trying is what the objective tells it to do.


Unlike the other failure modes, this one has an immediate price tag. Every failed attempt becomes part of the prompt for the next one, growing the context each time. As a result, cumulative cost grows quadratically, not linearly. Every unnecessary retry increases both the size of the next request and the total bill.


The execution layer sees elevated latency and higher token usage, and maybe an eventual timeout. It sees each individual error, but not the behavioral pattern producing them. The repetition pattern itself is only visible if you're watching across spans within the run, not solely at the run level.


## What was established as true


One mode is measured against what's actually grounded: the facts established by the context, the tools, and the conversation.


### Hallucination


The agent asserts something with no basis in fact. A feature that doesn't exist, a policy never set, a number never established. It's stated as plainly as if it were true.


Where the others check against the task or the run, hallucination needs a grounding check: the claim against the context, the tool responses, the state the agent actually had. Different reference, same shape. A claim measured against something the action itself doesn't carry.


The execution layer sees a clean LLM call. Valid tokens, coherent prose, no error code for "this is false."


Catching it requires a grounding check. The output is compared against what was actually established, and unsupported claims are flagged.


## What the user took away


The last mode isn't about the agent's work at all. The work was correct. The reference is the user's understanding of it.


### Communication Failure


This happens when the agent did the work but the user thinks it didn't.


Communication is part of the task. An agent that does the work and can't convey it hasn't finished. It doesn't break, but it breaks your evaluation metrics.


Consider the following: the task was completed and the output was correct. But the agent buried the confirmation, described the work in terms the user didn't recognize, or answered a different framing of the question than the one asked. The user comes back with "you didn't do X." The work is there. Nothing in the response told them so.


Communication failures generate support tickets, negative feedback, churn signals, and downstream business costs. They make a working agent look broken. If you're using user feedback as a proxy for agent quality, they pollute that signal in both directions. You're penalizing correct behavior without realizing it.


The execution layer sees a completed action, but user feedback sees a failure. Neither is inherently wrong. They measure completely different things.


Catching this failure requires detecting the space between what the agent produced and what the user understood, which means watching the conversation after the agent's turn. The strongest signal is a user follow-up that contradicts what the agent actually did, though it takes interpretation, since the user can also simply be mistaken.


## The Boundary


Four references, seven failure modes, one shape. None can be detected by inspecting an action in isolation. Every one is a comparison against a reference the action itself doesn't contain.


What execution monitoring actually catches


Failure mode error latency cost


What the agent was told


Skipped work


Out of scope work


Instruction violation


What happened in the run


Integration failure


Retry loop


What was established as true


Hallucination


What the user took away


Communication failure


present


sometimes


5 of 7 failure modes produce no execution signal at all.


Only one of these references is local to the action itself. Integration failure is the exception: the error exists right inside the span, even if confirming no recovery happened means looking further. Every other failure depends on information outside the span. A single action cannot tell you whether an obligation was left open, whether an action ran without authorization, whether identical failures have repeated, whether a claim was grounded in anything real, or whether the user misunderstood completed work.


That's the boundary. Execution monitoring evaluates what happened inside the span. These failures are defined by relationships to information outside it.


## Why This Discontinuity Exists


Traditional software has a fixed specification. A test compares the output against that specification and either passes or fails.


Agents don't. Their instructions are written in natural language, and they change as the conversation unfolds. A user can add a new requirement, amend an earlier one, or revoke it entirely.


That means the correctness of an action depends on when it happened. The exact same action may be correct at one point in the conversation and incorrect later because the instructions changed.


This is why a single pass over the transcript can't assign fixed grades to spans. The evaluator has to reconstruct which instructions were in force when each action happened, not simply inspect actions in isolation.


## How This Ties into Production Monitoring


The seven failure modes point to the same conclusion: some production failures cannot be detected from local execution signals alone. Detecting them requires reconstructing the appropriate reference and evaluating the action against it. Concretely, that means four things execution monitoring doesn't:


- tracking each instruction across the full conversation, not just reading it as context
- evaluating every action against the instructions that were live at the moment it ran
- grouping recurring failures across traces, because one instance of skipped work is noise, but the same pattern across many runs is a product incident
- doing it automatically, across every trace, without being told in advance what failure looks like


That last point is the one that matters most. The failure modes above aren't things you know to look for until after they've burned users. The monitoring layer has to find them without being told what to find.


That's what we're building at Lemma


.
