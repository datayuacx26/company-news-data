---
schema_version: "1.0.0"
document_id: "8e807ccaab41d104fd2a3a2bfa1f6837dc7683eed976b41bb7d01b50addee4c3"
company_key: "yc-mezmo"
company: "Mezmo"
source_id: "yc-mezmo-news-import-3b2f958954ed"
canonical_url: "https://www.mezmo.com/blog/builder-in-the-loop-tony-rogers-on-stress-testing-aura-before-production"
published_at: null
first_seen_at: "2026-07-22T04:13:38.440386+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:38dfa2e40146f0cf1c9681a84e80f0aef600754859bd37bd0726c871fb3421ef"
---

# Builder in the loop: Tony Rogers on stress-testing AURA before production

Builder in the loop is a Mezmo interview series focused on the engineers, product leaders, and operators shaping AURA, an open-source, MCP-native agent harness for production operations.


This installment features Tony Rogers, whose work on AURA is less about building new features and more about trying to break them before users can.


Tony tests the orchestration workflow, studies traces, builds internal benchmarks, and looks for the places where an agent appears to succeed but takes the wrong path to get there. For production operations, that distinction matters. A correct answer is not enough if the workflow behind it is slow, fragile, expensive, or hard to trust.


## The testing principle behind AURA


Tony's perspective is simple: the output is not the test.


An agent that gets to the right answer by doing the wrong things is not a reliable agent. It may call unnecessary tools, burn through too many tokens, recover from errors that should not have happened, or take a path that would not hold up under production pressure.


That is what Tony looks for.


His work has focused on testing and validating nearly every part of AURA, including its orchestration workflow. Some of that means running benchmarks. Some of it means manually inspecting traces. Some of it means finding edge cases where large tool outputs, weak MCP server implementations, or inefficient workflow paths could cause the agent to slow down or lose context.


The goal is not just to prove that AURA can complete a task. The goal is to understand how it completes the task, where it struggles, and what needs to improve before the workflow is trusted in production.


## Icebreaker: the tool Tony cannot live without


Tony's pick: TMUX.


He runs almost everything from the terminal, and TMUX lets him keep multiple panes open in a single session without losing context. It is a fitting choice for someone who spends his time watching AURA move through workflows, checking traces, and spotting where things slow down.


## Outputs are easy to grade. Behaviors are not.


When teams evaluate AI agents, the natural instinct is to ask a simple question: did it get the answer right?


Pass or fail. Score the output. Move on.


That instinct misses a lot of what matters.


Tony's role is to validate how AURA behaves under pressure. He is not attached to a specific implementation or feature path. His job is to find the places where the workflow breaks, slows down, overuses context, or appears to succeed in a way that would not be reliable enough for real production operations.


"I'm not a developer, I don't build many features inside of AURA. I do however, test and validate almost every feature."


That distance is useful. Tony is looking at the workflow the way a user eventually would: not from the perspective of how it was built, but from the perspective of whether it can be trusted.


One of the first things the team found was that existing benchmarks were not enough.


OllyBench, SREBench, TerminalBench, and other public benchmarks covered different agent capabilities, but they were not designed around AURA's orchestration patterns. In some cases, the tests were not difficult enough to surface meaningful failures.


Running AURA through a benchmark that could not expose problems was not enough. It proved that AURA could clear a public bar, but it did not prove that the workflow was ready for the kinds of messy, high-context investigations SRE and platform engineering teams deal with.


"In some cases, they aren't difficult enough to stress our workflows. And they also are testing agent outputs, more than agent behaviors, and I care about what the agent did to get to the output."


So the team built their own.


### What the traces actually show


Tony and the team are building internal benchmarks to test AURA against harder scenarios than the public options provide.


The goal is simple: find failures before users do.


Those benchmarks do more than check whether AURA got the right answer. They inspect the trace behind the answer: where the workflow slowed down, where errors appeared, which tools the agent called, and whether it used tokens efficiently.


"Inspecting the outputs in the traces to see where it took a long time, or were there errors in different parts of the workflow, or were there places where we could optimize the workflow as far as token usage?"


A trace shows what the agent actually did, step by step. It captures the tool calls, prompts, decisions, and the execution path that led to the final response.


That matters because the right answer can still come from the wrong path. An agent might call tools it did not need, pull in redundant context, recover from an avoidable error, or spend more tokens than the task required.


A simple score might miss that. A trace shows it.


Some of this review still happens manually today because public benchmark suites do not inspect agent workflows at this level. As our internal benchmarking suite matures, more of that inspection will happen automatically.


The result is more useful feedback for developers. Not just, "the output was wrong." More like: this step took too long, this tool call was unnecessary, or this part of the workflow used more context than it needed.


This type of feedback is what helped us build AURA from an impressive demo into a production-grade agent harness.


## Scratchpad: when your MCP server dumps everything


Context windows have limits.


Anyone who has worked with LLMs long enough has seen what using too much of the context window can do to agent performance. The agent starts focusing on unimportant things, makes more mistakes, or eventually overflows the window completely.


Scratchpad is how AURA handles that problem.


Scratchpad is a Mezmo-built capability, not a vendor integration. When AURA runs a workflow and a tool returns a large output, Scratchpad intercepts that output before it reaches the model.


Instead of pushing the full response into the context window, AURA stores it and provides the model with extraction tools to inspect the text, pulling only the pieces relevant to the current investigation.


"We intercept and cache tool outputs, rather than sending them directly to the model so it can go through and inspect what's relevant to the current investigation, and pull only those bits into the context window."


The result is an agent that can work with data volumes it otherwise could not handle.


Instead of hitting a context ceiling and failing, AURA reads selectively.


That matters even more when working with third-party MCP servers. Well-maintained MCP servers often include filtering tools that limit their output size, but not all MCP servers are built that way. Some return too much and expect the caller to sort it out.


"If you don't have control over that codebase, or if it takes a long time to get changes into that codebase, the Scratchpad kind of allows the agent to work with these MCP servers that may not have been written as well as they could have been."


AURA is designed to connect to the systems teams already use, and those systems vary in quality, shape, and implementation detail. Scratchpad helps keep the orchestration layer useful even when external tools return more data than the model should see at once.


For SRE and platform engineering teams, that is not an edge case. That is normal production reality.


## Where AURA fits


The primary focus for AURA today is the SRE use case: an agent that works with telemetry data to investigate production incidents.


That is where the team has concentrated its effort because the problem is real, the requirements are clear, and the impact on engineering teams is measurable. Incident investigation requires context from logs, metrics, traces, runbooks, code, prior incidents, and the systems they operate on. It is exactly the kind of work where an agent needs orchestration, context management, and access to tools to be useful.


Tony's view of AURA is broader, though.


AURA is not only an SRE tool. It is an agent harness for any situation where the work requires processing a large volume of information.


"It can be used to solve any problem where you have an agent that has to digest a ton of information."


The orchestration layer is what makes that possible. AURA can take a large task, break it into smaller pieces, manage context at each step, and carry useful information forward without flooding the model.


That same context problem becomes even more interesting when agents need to collaborate across domains.


## Wait, you can do that?


Early on, when the team was still figuring out how to stress-test AURA and explore what kinds of tasks it could handle, Tony got curious about software-defined radios.


Software-defined radio enables a computer to receive and process radio frequencies that would otherwise require dedicated hardware. Tony wondered what would happen if an agent could process streams of transcribed radio data and decide which updates were significant enough to surface.


"I was reading an article about software-defined radios and how you can listen in to radio waves around you, and I thought, wouldn't it be cool to have an agent listen in to the radio waves and kind of figure out what's going on in my area?"


So he built a side project around it.


The project combined radio reception hardware, transcription software to convert audio into text, and AURA as the harness making sense of the incoming data. Every five minutes, AURA reviewed the accumulated transcripts and decided which items were important enough to surface.


"There was a bunch of software that was there doing the transcribing and storing the text messages, and then AURA would read those text messages and determine what's important enough to post."


AURA handled the interpretation layer. The radio hardware and transcription software handled the upstream work.


In production operations, the same basic division holds, only with telemetry rather than radio traffic. Systems generate large volumes of raw signal. AURA helps decide what matters, what context is relevant, and what should happen next.


That side project is not the main use case. It is a reminder of what becomes possible when an agent harness can connect to arbitrary inputs, manage context, and reason across a large stream of information.


## What's next


AURA keeps moving, and the testing work moves with it.


Every new feature and architectural change eventually lands in front of Tony to get its edges found. That means more internal benchmarks, more trace inspection, more workflow validation, and more pressure on the system before users depend on it.


What Tony wants people to take away is not a specific feature. It is an invitation.


"We want people to try AURA and tell us what's good and what's bad about it. If I get anything out of people reading this blog post, that's what I'd want."


That is the point of building AURA in the open. The more real workflows it sees, the better the team can understand where agent behavior holds up and where it still needs work.


AURA is open source under Apache 2.0. Explore the repository on GitHub:[github.com/mezmo/aura](https://github.com/mezmo/aura)


## About Tony Rogers


[Tony Rogers](https://linkedin.com/in/tony-rogers-62341511/?skipRedirect=true) is a member of the Mezmo team whose work on AURA has focused on testing and validating the orchestration workflow across nearly every feature. He builds internal benchmarks, reads traces to find where agents misbehave before they reach production, and feeds that insight back to the engineering team.
