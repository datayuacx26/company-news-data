---
schema_version: "1.0.0"
document_id: "8c61812114a368cf322f231ed1fd0b1a7ce2d4be01b9ae907b0418248b19847d"
company_key: "yc-mem0"
company: "Mem0"
source_id: "yc-mem0-news-import-acb706cfa21b"
canonical_url: "https://mem0.ai/blog/procedural-memory-explained-teaching-ai-agents-how-to-perform-tasks"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-05T20:58:30.934569+00:00"
fetched_at: "2026-08-05T20:58:32.920649+00:00"
content_hash: "sha256:d70082f8f30800cc3247c8d47094995183d4573384a4558b9a6cebba39f61884"
---

# Procedural Memory Explained: Teaching AI Agents How to Perform Tasks

Have you ever set up a convention for your agent, say, how your team adds a new API endpoint, and wished it would just remember that the next time a teammate asks for something similar?


If yes, procedural memory is your answer. It's one of the memory types we depend on most and talk about least. It's the third pillar of agent memory, and right now, it's the one holding up the least weight.


So, I ran a simple test demo using Mem0 to test if a real-world scenario would benefit from it or not. Here is a quick look at what I built:


Please enter a valid YouTube, Vimeo, or direct video URL


**If this got you Interested?** **Get yourself a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and let's dive deeper.**


## **Memory and its types**


The most common mistake in agent memory is treating it as a single store, i.e, one vector index and one "memories" table, all blended in together. That store then has to serve a user's stated preference, a failed debugging attempt, a reusable deploy routine, and a reminder to follow up next week, all with the same shape and the same rules. It serves all of them badly, because the one with the strictest requirement gets handled at the level of the easiest.


Memory is of 7 types, but the ones that matter for long-term memory, the ones cognitive science has argued over for fifty years, come down to three pillars:


-


**Semantic memory** is what the agent *knows* : facts and preferences, detached from when they were learned. For example: "This service talks to Postgres." "The user likes four-space indentation."


-


**Episodic memory** is what *happened* : a specific, dated event like, "On Tuesday, mocking the network didn't fix that flaky test."


-


**Procedural memory** is what the agent *can do* : a reusable skill or routine. For example: "Here's how we add an endpoint, step by step."


The first two memory types solve recall problems. You store something, you search for it, you get it back, and the whole industry has gotten good at exactly this. Procedural memory is different, because it isn't there to be recalled and read. It's there to be executed, and that's the one almost no memory layer actually ships.


## **What is procedural memory?**


Procedural memory is a callable routine that contains step-by-step information the agent carries and reuses as-is. Think of a five-step deploy, or the skill library that let[Voyager](https://arxiv.org/abs/2305.16291) compound its abilities in Minecraft by writing successful behaviors into reusable skills and pulling them back when a new task needed them. The key here is that it's meant to be reused and improved over time, not merely remembered.


Out of the 2 memories, semantic memory changes what the agent says, and procedural memory changes what the agent does. That's the reason why procedural memory matters more and often gets built less. A fact retrieved wrong may give you a slightly-off answer, but a procedure retrieved right means the agent follows the workflow that works instead of improvising a new one. That reuse is a learned behavior an AI agent carries from one task to the next.


This is also the standard framing in the agent-memory literature. The[CoALA framework](https://arxiv.org/abs/2309.02427) splits an agent into a short-term working memory as well as long-term semantic, episodic, and procedural stores. The three pillars are what survives the session, where two of them get all the tooling done.


## **Why is it the missing pillar?**


Now you may ask: If procedural memory is so useful, why does almost every memory layer optimize for the other two? Simply because it's harder and in two specific ways:


1.


**Hard to capture:** A procedure is a trajectory that follows an ordered sequence of steps, the dependencies between them, and the conventions that only make sense in context. **Note:** A procedure stored without its reasoning is brittle. *For example:* "We add a test" is a rule, but why and when it applies is what makes it reusable. Capturing procedural memory well means keeping the shape of the workflow, not just a list of verbs.


2.


**Hard to keep correct** *:* A stale procedure can drive a wrong action, i.e, if the agent follows old release steps after the pipeline moved, or the old endpoint convention after the team changed it, then it can lead to a buggy implementation.


So I ran a test to teach an agent a procedure once and see whether a different request, from a different user, gets the benefit without anyone explaining it again or not.


## **The test: teach it once, watch it reuse**


Procedural memory doesn't only come from an agent fumbling its way to a routine. It also comes from being taught. One person explains how the team does a job, and someone else, later, benefits without ever being told. That's AI agent workflow memory doing its job.


### **Before you run it**


You'll need four things:


-


**Clone the repo:** Clone the code repo from[GitHub](https://github.com/aashidutt-mem0/Procedural-Memory-in-AI-Agents-The-Missing-Third-Pillar)


-


**A Mem0 account:** Grab a free API key at[app.mem0.ai](https://app.mem0.ai/) . This demo runs on the hosted platform, so you'll need this key specifically.


-


**A chat model:** Either Azure OpenAI or OpenRouter works. The demo config accepts both.


-


**Install libraries(requirments.txt):** Python, with` mem0ai` ,` openai` , and` streamlit` installed, if you want the interactive version.


Then set the environment variables and run the streamlit application using


```text
streamlit   run   app  . py
```


```text
streamlit   run   app  . py
```


```text
streamlit   run   app  . py
```


None of this needs deep memory-architecture knowledge going in. If you've used any LLM API before, you're set.


### **Act 1. User A inputs a checklist**


A senior dev spells out the checklist once. The agent stores it in Mem0, scoped to its` agent_id` , because a procedure belongs to the agent's job. This is the shared via team-level nature of procedural memory i.e, taught by one and available to all.


> **Wanna give it a try? Get a**[Mem0 API Key](https://app.mem0.ai/dashboard) **and try it yourself.**


```text
from    mem0    import    MemoryClient
mem   =  MemoryClient  (  api_key  = MEM0_API_KEY  )


mem  . add  (  [  {  "role"  :  "user"  ,    "content"  :  team_procedure  }  ]  ,
agent_id  = "coding-agent"  ,    infer  = False  )
```


```text
from    mem0    import    MemoryClient
mem   =  MemoryClient  (  api_key  = MEM0_API_KEY  )


agent_id  = "coding-agent"  ,    infer  = False  )
```


```text
from    mem0    import    MemoryClient
mem   =  MemoryClient  (  api_key  = MEM0_API_KEY  )


agent_id  = "coding-agent"  ,    infer  = False  )
```


### **Act 2. User B asks for something different**


A different developer, given no instructions, asks the agent to add a` POST /refunds` endpoint. Before writing anything, the agent pulls the procedure back out of Mem0 and follows it:


```text
hits   =  mem  . search  (  "checklist for adding a REST API endpoint"  ,
filters  = {  "agent_id"  :  "coding-agent"  }  ,    limit  = 10  )
procedure   =  "\n"  . join  (  m  [  "memory"  ]    for    m    in    hits  )      # injected into the agent's prompt
```


```text
hits   =  mem  . search  (  "checklist for adding a REST API endpoint"  ,
filters  = {  "agent_id"  :  "coding-agent"  }  ,    limit  = 10  )
```


```text
hits   =  mem  . search  (  "checklist for adding a REST API endpoint"  ,
filters  = {  "agent_id"  :  "coding-agent"  }  ,    limit  = 10  )
```


The key point: the two inputs were completely different requests, but the agent still answered in the team's specific way, because that convention only had to be taught once, not repeated by every developer who comes after.


> ⭐️ Checkout the complete demo code on[GitHub](https://github.com/aashidutt-mem0/Procedural-Memory-in-AI-Agents-The-Missing-Third-Pillar)


## **Results**


I ran the test for User B with a similar input as User A` Add a POST /refunds endpoint that issues a refund for an order` , and here’s what Mem0 stored from User A’s input:


> When we add a new REST endpoint to our service, we always follow this checklist, in order: 1. Add the route in[routes.py](http://routes.py/) . 2. Write the handler function in handlers/. 3. Register the handler in[app.py](http://app.py/) . 4. Add a unit test in tests/. 5. Update the OpenAPI spec in openapi.yaml. We never skip the test or the OpenAPI update.


With the learned procedure retrieved and injected, the plan came back as:


Five for five, in order, using the taught file names. User B never saw the checklist, but our agent did!


With no memory, the same question got a completely different plan: seven steps, and none of them followed the checklist User A set. It skipped the unit test and the OpenAPI update entirely, exactly the kind of gap that turns into technical debt down the line.


The no-memory agent isn't worse at coding, but it’s just guessing conventions it has no way to know, and it guesses wrong in exactly the way you'd expect. The gap is the whole story. Both agents can guess the obvious steps, add a route, write a handler. The no-memory one tends to miss the two it was never told. But the agent that learned the procedure once covers them, on a task nobody walked it through. That's procedural memory changing what the agent does, not just what it recalls.


## **Conclusion**


The loop is three moves, and Mem0 is the middle one:


1.


**Teach it:** Store the procedure under the agent's id (` mem.add(..., agent_id=...)` ).


2.


**Keep it:** It persists across sessions and across users, scoped to the agent, not to whoever taught it.


3.


**Reuse it:** On the next instance of the job,` mem.search(..., filters={"agent_id": ...})` pulls it back by meaning, and you hand it to the agent before it acts.


But procedural memory has sharper edges than fact memory, so two rules for you:


-


**Store the shape, not just the verbs:** A procedure recorded as a bare list of steps, with no sense of when it applies, gets misapplied.


-


**Version it, and revalidate:**A stored procedure is trustworthy only until the world it encoded changes. When the team's conventions move, the old procedure has to be superseded, not blindly reused. Treat a procedural memory as derived and human-verifiable, with an expiry tied to the thing it describes, not left to run forever.


Semantic memory enables an agent to know things. Procedural memory enables an agent to perform them consistently. If your agents recall everything and improve at nothing, this is the pillar you're missing.


——


Mem0 is an intelligent, open-source memory layer designed for LLMs and AI agents to provide long-term, personalized, and context-aware interactions across sessions.


Get your free API Key here:[app.mem0.ai](https://app.mem0.ai/)


If you are an agent, sign up using[app.mem0.ai](https://app.mem0.ai/?utm_source=mem0_blog&utm_medium=kw_blog&utm_campaign=understanding_memory_benchmark&utm_content=understanding_memory_benchmark) , or self-host Mem0 from our open-source[GitHub repository](https://github.com/mem0ai/mem0) .


——


## **Frequently asked questions**


### **Q. How is procedural memory different from just storing facts?**


A fact is what the agent knows; a procedure is what it does. Storing "we add a test for every endpoint" as a semantic fact still leaves the agent to assemble the workflow at runtime, and it will often skip a step. Procedural memory stores the assembled routine itself, so the agent follows it instead of rebuilding it from scratch.


### **Q. Why scope the procedure to**` **agent_id**` **instead of**` **user_id**` **?**


Because a procedure is shared, team-level knowledge, not a personal preference. In the demo, User A teaches it and User B benefits without ever being told. That only works if the memory belongs to the agent doing the job, not to the user who happened to explain it. User-scoped memory is for personal facts and preferences.


### **Q. Does the second request have to be identical to the first?**


No, and that's the point. User A explained the general procedure; User B asked for a specific, different endpoint (` /refunds` ) and never saw the steps. Procedural memory generalizes across instances of the same kind of job. When the procedure itself changes, you supersede it, which is the maintenance cost that makes this pillar harder than fact recall.


### **Q. Isn't this the same as reflection or self-critique?**


Reflection is a great *source* of procedural memory, but it's not the same thing. A reflection is a lesson about one past attempt. A procedural memory is a generalized routine you can carry to a new task. Here the routine came from a user teaching it; it could equally come from the agent reflecting on a run that worked. Either way, what gets stored and reused is the how-to.


### **Q. Will this help a non-reasoning model?**


Yes. Any agent that reinvents a multi-step workflow risks doing it inconsistently and skipping the steps it can't infer. Handing it the learned procedure fixes that regardless of model. The mechanism, teach once and reuse, is model-agnostic.
