---
schema_version: "1.0.0"
document_id: "673bedb3a4940f26f70023f0918ab4c09143574144b2c413012cbf8c6fa670c3"
company_key: "yc-butter"
company: "Butter"
source_id: "yc-butter-rss-cfe99054af6a"
canonical_url: "https://blog.butter.dev/muscle-mem-as-a-proxy"
published_at: "2025-10-01T19:25:44+00:00"
first_seen_at: "2026-07-24T22:18:01.434032+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:9860966ef82e2f3c70d8ff3c1a5d281fde0bf3b49c7e7012e17d1af5c1d0c8cf"
---

# Rethinking Muscle Mem as an LLM Proxy

### I believe two things:


1.


**AI is an incredible technology,**


2.


**AI will be used *less* * over the long term.**


(*) *proportionally speaking…*


Over the last few months, I’ve been working on building “Muscle Memory for AI”: tooling that specifically removes AI from places where deterministic scripts would serve just fine.


In May, I launched a Python package called` Muscle Mem` .


Titled *A Behavior Cache for Agents* ,` Muscle Mem` **was an SDK for instrumenting and replaying sequences of tool calls** .


It’d bolt on top of your existing agents as decorators on their tools, running snapshots every time the tool was invoked to record not only the action taken, but the environment in which it was taken (for safe cache validation). For more context such as its use cases, API design, and a demo,[the original launch blog](https://erikdunteman.com/blog/muscle-mem/) is worth a read.


` Muscle Mem` was well-received, gaining healthy intrigue on[Hacker News](http://news.ycombinator.com/item?id=43988381) and[GitHub](https://github.com/pig-dot-dev/muscle-mem) .


I sure felt clever!


Fast-forward a couple days: we had a more than 700 Github stars, but strangely… seemingly no users. While a few real users did eventually trickle in over the following month(s), it was clear something had to change.


Thankfully (or, not), it’s hard to talk me out of ideas that I consider to be inevitabilities.


I genuinely believe in the thesis that **many classes of automation are best expressed with deterministic software, and we’re wasting intelligence and reducing trust by performing them with agents** .


The road to AI is understandably tempting; there’s an long tail of cases to handle in automation software, and agents can confidently trailblaze across those edge cases into the unknown to figure it out. But what I assert is that: **for those repeat executions, we ought to be using software.**


## The Flaws of Muscle Mem


( *You would not believe how many failed attempts I’ve put into writing on this topic* )


` Muscle Mem` did not last long in the wild before it started hitting limitations. Its flaws fell into one of four buckets:


1.


Shit UX


2.


Rigid definition of a workflow


3.


Recording the wrong signals


4.


Ignoring the dynamic nature of data


### Shit UX


Conceptually cool, but a head scratcher to try to use.


` Muscle Mem` focused heavily on tool-calling as the integration point, along with user-defined callbacks on generic types to guard the execution against edge cases.


You’d decorate tools with a pre-` Check` , a guard that would run a` capture()->T` callback to capture some data` T` that you’ve deemed matters for later determining cache validity (yet another` compare(T, T)` callback to pass/fail relative to the current` T` ). Simple!


Confusingly to many,` Muscle Mem` did not ask users to decorate their tools which *read* from the environment: it didn’t care about replaying those. Just decorate the *write* tools. And the capture function would greatly depend on` T` . Even for us, generating examples proved difficult in this setup, which is pretty apparent in an example where we make use of a literal timestamp in` T` , resorting to TTL based cache invalidation.


If you’re struggling to follow, rest assured. For, in my excitement to make the system “bolt onto” your existing agents, I created a whole new set of hoops to jump through. Tools had to be manually instrumented, and you had to know in advance which ones mattered for replay. Cache validation was forced to be a user concern, and users didn’t seem terribly keen on spending their time writing such abstract callback handlers.


### Rigid definition of workflow


Following the assertion that` Muscle Mem` was automating “discrete” workflows, it became the responsibility of the user to group workflows into discrete buckets, such as:


-


“Fill out the intake form,” or


-


“Process a refund.”


This would require all workflows to be known in advance. It afforded no natural drift in workflows. It left no room for subagents and subtrajectories, which are usually powerful tools for abstraction in agents.


Moreover, to enter a workflow was a black box. A supposed cache-hit that would break mid-run wouldn’t have any way of letting the agent resume from partway through; its only option was to start from the very beginning.


This clashed with the callback structure as well. If a tool were to return a cache-breaking piece of data, that signal would certainly be present in the messages, but it’s possible the user couldn’t have anticipated having to read that data when designing` T` . Or, what if the new branch in behavior subtly involved an entirely different workflow? Well, it would all get baked into the cache under “Fill out the intake form.”


### Recording the wrong signals


The role of Muscle Memory is to accurately reproduce the same series of tool calls that an LLM would when in the same situation.


The artifact of an agent run in` Muscle Mem` was a linear list of function invocations with arguments, as well as any user-retrieved environment data which` T` would tag onto it. This differs from the content visible to the LLM: the LLM isn’t looking at tool traces and user data` T` . Instead, it only sees the context window to make branching logic decisions. Thus,` Muscle Mem` shared no overlap in signal with the models it was meant to proxy.


In reproducing tool calls in a model-like way, it is critical to have a 100% overlap in signal, else succumb to the final point:


### Ignoring the dynamic nature of data


This was, and is, the biggest challenge in this entire domain of Muscle Memory. No two LLM calls are exactly alike: they’re the result of structural templating, nondeterministic tool results, and chaotic user input.


If you require exact matches, as did` Muscle Mem` , your hit rate will be 0%.


We must be able to separate out “variables” from the code.


The easiest example of this is a form-filling bot navigating a browser. The button coordinates are static, as the buttons will almost always remain at their respective locations. But, typing into the “First Name” field would be dynamic: entering a unique string from run to run. Still, you could (and should) assert that form-filling with dynamic data constitutes the same workflow; it just involves variable data which the model is regurgitating into a tool call argument.


I will talk in great depth on this topic, so we’ll just say that` Muscle Mem` had nearly zero mechanics accounting for this.


## The Realization: Everything is Code


All of these concepts map to software systems.


### LLMs are pure functions, tools are the runtime


The[Chinese Room Experiment](https://en.wikipedia.org/wiki/Chinese_room) questions our ability to judge the intelligence of an agent locked away in a black box offering communication only by basic inputs and outputs. For us, we aren’t so much concerned with determining the intelligence of the agent inside. Instead, Muscle Memory attempts to learn from and emulate the apparent “intelligence” inside.


An interesting aspect of the Chinese Room setup is that it is *stateless* , with a text-in/text-out messaging interface. Peaceful, quiet, one request at time, and a pure function even by a Haskell programmer’s standards. At least until LLM API providers pull tool callings behind their API wall, **we may think of LLMs as pure functions** , interpreting inputs and transforming them into outputs per instructions without side effects.


**Tools are the runtime**


A pure function on its own has no use without side-effects. Tools are the runtime, the “hands” for models to reach out and read/write into the stateful world. They’re also the means by which caches get broken.


**If it quacks like a duck…**


Poorly summarized in polymorphism terms, the lesson we take from Chinese Room Experiment is:


> If it looks like a duck and quacks like a duck, it calls into question if ducks are real.


Whether or not ducks are real, the Chinese Room scenario says that as long as a black box is producing a valid sequence of outputs, it’s a viable system.


This system, executing tools into the external world, cares not for what’s in the black box, so long as that black box produces tool calls in the right order with the right arguments. That is, users of LLMs aren’t so much interested in the mechanics of the computation used to generate a response to their prompts. They’re mainly looking for responses they can trust.


### Data-vs-code separation


Programming languages (turning a blind eye to the lisps) generally draw a strict line between data and code. The code lays out a map of all possible branches a system can take, and the data slots into known shapes (types) to ultimately dictate which branches get taken.


**Context windows are effectively interwoven data + code** . This is especially the case with the workflow agents we’re targeting, which are usually triggered by some software system without a human chat so it carries structurally consistent data.


The separation of data and code introduces a concept of variables, which we’ll call *symbols* . In programming languages, symbols map to data, functions, etc. In context windows, it’s more nebulous, but for simplicity: think of symbols as pointers to substrings within the context, like` first_name` .


### The exact line of “intelligence” is variable scope


In interpreted programming languages, executing a line depends on all the used symbols being in scope by the time of execution, else raising an` Undefined Symbol` error.


```text
a =  1    # brings a into scope
b = a  # a already in scope, brings b into scope
c = d  # Undefined Symbol: d!


```


Imagine a perfect system for separating data and code from an LLM’s context window, one where you accumulate a vast trove of symbols and their respective data.


If that context is sent to a model, and it produces new symbols that have no computable origin from the context, we might call this the result of “intelligence.”


That is, the model pulled that variable out of thin air.


This is the equivalent of the` c = d` undefined symbol error above. Variables simply do not materialize out of thin air in deterministic programs. This helps us identify the intractable land of “intelligence” which we couldn’t possibly serve. Sadly, this is where we must draw the line for how helpful Butter can be.


### Can it be Muscle Memory?


Given this, the cleanest heuristic for whether an automated workflow might benefit from Muscle Memory systems is this: whenever you could, in theory, with infinite time, sit down and write heuristic-based software handling every case of the workflow.


If you could not write the workflow as software, if you couldn’t massage dynamic data of known shapes through a conditional branching logic without hitting some data transformation that can only be described as “an LLM pulling something out of thin air,” then you would be working with a fundamentally intelligent workflow.


So long as every single token output of a model is either static (inherent to that run, including planning traces) or a regurgitated derivation of dynamic data from earlier in the context window, we consider the generation to be a candidate for Muscle Memory.


It’s that simple. **Can it be code?**


## “Can it be code” – So we’re doing codegen?


Not exactly. In addition to the proxy approach, which I’ll discuss in a moment, I believe a[Voyager-style](https://arxiv.org/abs/2305.16291) codegen approach models the data-vs-code separation well.


Under this model, LLMs are used to write actual software which hooks into a low-level primitive set of functions, giving itself an on-the-fly generated tool it can call.


I have plenty of qualms with client-side codegen (which I’ve omitted from this blog for clarity), but all around I believe it can perform well, especially as codegen models improve. We’ve decided to keep focused on our current approach, but it’s worth acknowledging codegen as a hopeful alternative.


## Actually, we’re building an LLM proxy


LLM proxies, or “gateways,” are an increasingly common class of product that take advantage of the widespread support for[OpenAI’s Chat Completions format](https://platform.openai.com/docs/api-reference/chat) , by even non-OpenAI model providers or self-hosted servers like vLLM.


These gateways are HTTP reverse proxies which sit between any LLM client and server supporting the chat completions format. They are simply integrated with one line of change by repointing your LLM client to call the proxy instead of the provider’s default API:` BASE_URL=`[https://proxy.butter.dev](https://proxy.butter.dev/) .


Most gateways are used for analytics, or for intelligently routing across multiple providers.


The Butter proxy does something special: **it spoofs LLM responses** back to clients.


The proxy models all trajectories taken using a tree keyed at each level by the message content. When a response can be served deterministically, it is. If not, the request forwards to the LLM provider for an actual generation, and novel results are stored as a fresh branch in the tree, to be used as muscle memory in the future.


It’s not obvious at first, but a proxy cache serving back spoofed responses to a tool-calling agent achieves, in a heavy-handed way, the same thing that codegen does: we hold the branching logic of a system in the form of a tree-like cache, and guide the agent into invoking functions in the runtime (tools) to read and write data, which goes on to inform even more branching logic in the subsequent requests.


Again, **as long as those tools get called, does it matter who called them** ?


Remember the Chinese Room.


As far as the agent client is concerned, it’s always in “AI” mode. The duck keeps quacking, the results come back in valid format, and the world keeps going round.


I’m obviously sold, but let’s walk through how this Proxy approach solves many of the issues with our first` Muscle Mem` approach:


### Not-Shit UX


It’s dead-simple to use: just repoint your` BASE_URL` to the proxy. It’s a drop-in endpoint for any client system that speaks the chat completions API, as most do.


Users need not concern themselves with special APIs to record this vague concept of a` T` .


Cache invalidation is built into the way the tree is traversed, on the messages itself. Cache-breaking data from tool call results simply branch the tree and gracefully continue from there with the guidance of an LLM. This provides a completely smooth on/off-ramp between software systems (spoofing the agent down a proven route) and AI systems (generating novel responses).


### Fluid definition of a workflow


Grouping of trajectories is automatic and structural: “Does it follow an existing branch in the tree? Boom, that’s an instance of a workflow.”


This way, workflows taken on a form we can visualize: of a path or set of adjacent paths down the tree. This also allows for a “natural drift” as the workflows evolve: new branches growing, and stale branches eventually pruning.


### Recording the right signals


It hooks into the messages level, containing all of the data needed for Muscle Memory:


-


Message content for cache returns,


-


Tool call invocations,


-


Tool call results, which carry cache-branching external data, and


-


Any other context “symbols” or data that would push the trajectory down a specific branch.


A 100% overlap with what the model sees.


### (TBH still struggling with) The dynamic nature of data


[We’re working on this one](https://blog.butter.dev/template-aware-caching) **.** At the very least, we have access to every piece of “scope” that the model does, plus more if we grow our API surface area. In addition, the centralized nature of the cache allows trajectory-vs-trajectory comparison. This gives us the most complete shot at deconstructing data and code from context windows and building a cache that’s more structural than literal.


## Conclusion: If only it were Easier


The pivot to the proxy form factor was no obvious path to take, and it’s substantially harder to do this correctly than a Python client library.


Despite the challenges, I’m confident that it is the cleanest and most theoretically correct route to building Muscle Memory into AI systems, so we’ll be giving it an enthusiastic swing!


You can sign up and try Butter today[using our quickstart guide](https://docs.butter.dev/) .


Or reach out –erik@butter.dev – we’d love to hear your use-cases, and will try our best to prioritize the patterns that most quickly get you to an agentic system you can trust.


Cheers,


**Erik**
