---
schema_version: "1.0.0"
document_id: "be7a7883198b7ccafb2fd55c0acddc9931dd50c8365f2180622312c7d0076d50"
company_key: "yc-humanlayer"
company: "HumanLayer"
source_id: "yc-humanlayer-news-import-a7d3053d1b06"
canonical_url: "https://www.humanlayer.dev/blog/context-forking-to-save-time-trouble-and-tokens"
published_at: null
first_seen_at: "2026-07-25T08:50:39.103220+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:ba32d19872ec3ab7338af442f1c436e24fc81a98757135d2fe600d0a222f1447"
---

# Context Forking to Save Time, Tokens and Trouble

Context forking is a powerful coding agent primitive that allows you to build up high-quality context and then reuse it multiple times.


Lots of coding agents (OpenCode, Pi, Claude Code, and many others) support context forking, though they often give it different names: rewind, time traveling, or branching - but they're all variants of the same concept.


This post is a tactical guide on how to use context forking to save you time, tokens, money, and/or a great deal of frustration


## ## Context Windows as Operating System Stacks


I like to conceptualize agent context windows as[downwards-growing stacks](https://wiki.osdev.org/Stack) , inspired by stacks from operating systems:


In an OS stack, frames for new routines are appended to the bottom of the stack as routines are called. You can conceptualize a context window the same way, with each user message-assistant message turn as a new routine with a new stack frame:


Like a stack, coding agent context windows usually **prevent random access** . You can[push](https://wiki.osdev.org/Stack) things to the end of it by sending a user message, and you can[pop](https://wiki.osdev.org/Stack) (remove) things from the end.


But just like how an OS stack grows downwards as new routines are called & stack frames are pushed onto it, the context window only lets you push or pop from the newest end of the history. **You are stuck interacting with the end of it.**


Random access isn't allowed for several reasons:


- It can cause expensive cache misses against your inference API
- It can mangle important accumulated context
- It can interfere with your coding agent's internal state


Most coding agents have internal state that tracks which files the agent has read and/or written (among other things). When the agent tries to edit a file, this allows the harness to prompt & force the agent to read the file before attempting to modify it.


Erasing or adding tool calls into the middle of a context window would require surgical manipulation of the coding agent's context AND any internal state, and agents generally don't support that.


## ## How does context forking work?


Context forking allows you to pop 1 or more messages off the bottom of the context window stack to restore the state to an earlier version.


Just like how an OS stacks pushes & pops entire frames at a time, you can usually only rewind your context window at user-message turn boundaries, not in the middle of a sequence of tool calls:


Usually, you can do this more than once - forking from the same context window in many different ways.


Interface and implementation details vary from agent to agent. Some agents allow you to rewind the state of the code & disk when you rewind the state of the conversation. Others may create a new branch or worktree when you do this.


## ## When should you fork your context window?


### ### Rewinding to course-correct an agent


A popular use-case is for forking is to rewind a conversation while the agent is implementing a feature to account for something that was missed:


### ### Forking to explore different design paths


I often find myself forking sessions during the design phase of a task.


Once I have accumulated high-quality context about the codebase and problem I'm trying to solve, I'll fork the conversation to explore different design & architectural paths.


Then I'll review the results and decide which session to proceed from - or that more research is necessary!


### ### Forking to preserve a context window from context-inefficient operations


Another great use of forking is to rewind a conversation to preserve high-quality built-up context after the agent does something context-inefficient.


An example of this is when the agent reads a large file or runs a command that generates a TON of output - tens of thousands of tokens worth. Most[coding agent harnesses have hooks or other safeguards](https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents) to prevent this by writing the output to a file and just showing the agent the filepath rather than the entire contents, and instructing the agent to search through it. But sometimes, the agent just reads all 40,000 tokens one chunk at a time and fills up the context window:


Fortunately, we can salvage the conversation with forking!


## ## Let's recap:


1. Coding agent context windows can be conceptualized as downwards-growing stacks. You can push to or pop from the bottom, but generally can't interactively mess with the middle or top of the stack
2. Context forking can be used to rewind a context window to re-steer it when the agent missed something
3. Context forking can be used to branch and explore multiple different design or implementation paths
4. Context forking can be used to restore a context window to a state before a large volume of low-quality context was added.
