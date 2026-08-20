---
schema_version: "1.0.0"
document_id: "1d297581b25aea1927a27e425ca9394d8e043db53544e4ca684c21f00151bb69"
company_key: "yc-moss"
company: "Moss"
source_id: "yc-moss-rss-e49397d0a304"
canonical_url: "https://www.moss.dev/blog/the-production-ai-stack"
published_at: "2026-07-18T09:00:00+00:00"
first_seen_at: "2026-07-27T03:53:19.807128+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:2b9d004b2f57fddbe3fd80c209e1d4a832423521215cad4ce71b7656dc048a92"
---

# The Production AI Stack: A Reference Architecture for Real-Time AI Systems

A founder built an AI support agent for his ecommerce store using what has become the standard modern AI stack: a large language model, a cloud vector database containing the company's help center, and a small set of tools for common actions like looking up orders and initiating returns. The system worked well. Customers could ask questions, check the status of an order, or begin a return without ever needing to speak with a human.


The problem wasn't correctness. It was latency.


Every interaction followed the same execution path. A customer asked a question, the agent queried the vector database for relevant context, waited several hundred milliseconds for the results to return, and only then could the model begin generating a response. Nothing in the architecture was technically broken, but the cumulative delay was obvious enough that conversations felt slower than they should.


Like many teams building production AI systems, he optimized for responsiveness by removing retrieval altogether.


Instead of retrieving context on every turn, he embedded everything directly into the system prompt. The help center, shipping policies, return rules, pricing information, FAQs, and any other information a customer might reasonably ask about all became part of the prompt that accompanied every request. It eliminated an external dependency, reduced latency, and simplified the architecture.


For a while, it looked like the right tradeoff.


As conversations became longer, however, the architecture started failing in more subtle ways. Every message added another layer of conversational history to a context window that was already carrying the company's knowledge base, forcing the model to reason over an increasingly large body of information. As the available context filled up, retrieval accuracy was effectively replaced by probabilistic recall. Shipping questions began pulling in return policies, discounts from unrelated products appeared in responses, and facts that had been clear at the beginning of the conversation became less reliable over time.


The most frustrating part was that these failures rarely appeared during testing. Short benchmark prompts continued to perform well, while the conversations that actually mattered in production were the ones that exposed the weaknesses of the architecture. Customers asked follow up questions, referred back to earlier messages, changed their minds halfway through a workflow, and expected the system to maintain consistency across dozens of conversational turns. Those were precisely the interactions where the model became least reliable.


This pattern appears across every category of production AI application, from customer support agents and enterprise copilots to voice AI platforms and conversational search systems. Teams often assume they're dealing with a prompting problem or a model quality problem, when in reality they're running into architectural limits. Large prompts eventually become expensive to process, context windows inevitably become saturated, and relying on a language model to remember everything produces systems that become less predictable as conversations grow longer.


## The Production AI Stack


Every production AI system, whether it's powering a voice AI platform, an enterprise copilot, or a conversational search experience, is ultimately composed of the same seven architectural layers:


1. Models determine which foundation model is responsible for each task.
2. Inference controls where and how token generation happens.
3. Search retrieves the knowledge required to answer each request.
4. Memory persists information about users and previous interactions.
5. Sessions maintain conversational state across multiple requests.
6. Orchestration coordinates the execution of every component throughout the conversation.
7. Deployment determines where each layer runs, whether in the browser, at the edge, on device, or in the cloud.


Each of these layers can be optimized independently, but production AI systems are rarely limited by any single component. Performance, latency, reliability, and cost emerge from the interactions between them. Understanding those tradeoffs requires looking at the entire architecture rather than any individual layer, which is where we'll begin.


## Start With the Latency Budget


Every production AI system begins with the same constraint: latency.


Before deciding which model to use, how to structure retrieval, or where to run inference, you need to understand how much latency your users will actually tolerate. Every interactive product has a latency budget, and once that budget is exceeded, no amount of model quality can recover the user experience.


Human computer interaction research established these thresholds decades before large language models existed. Responses under roughly 100 milliseconds feel instantaneous. Around one second, users remain in their flow of thought but become aware that they're waiting. Beyond ten seconds, attention shifts elsewhere. IBM's work on the Doherty Threshold arrived at a similar conclusion in the early 1980s, arguing that interactive systems should respond within roughly 400 milliseconds to maintain a continuous feedback loop between the user and the computer.


Conversation is even less forgiving. Human turn taking typically happens within about 300 milliseconds, which means every millisecond an AI system spends retrieving data, waiting on network calls, or generating tokens directly competes with a rhythm that people have spent their entire lives expecting.


The acceptable latency budget therefore depends on the product you're building:


These aren't arbitrary targets. They directly influence user behavior.


Google demonstrated this in a controlled experiment in 2009 by intentionally adding between 100 and 400 milliseconds of server side latency to search results for a randomized group of users. Even delays at the lower end of that range caused people to perform fewer searches, with engagement steadily decreasing as latency increased. Small delays that seem insignificant in isolation become measurable product problems when they occur millions of times every day.


Voice AI makes these constraints impossible to ignore.


Among all AI applications, voice has the smallest latency budget while requiring the largest number of sequential operations before a response can begin. A typical conversational turn requires speech recognition, retrieval, language model inference, and speech synthesis to happen one after another. Because each stage depends on the output of the previous one, their latencies accumulate rather than overlap.


A representative production pipeline looks something like this:


One number stands out immediately.


The language model isn't usually the largest source of latency. Retrieval is.


Despite often being treated as solved infrastructure, retrieval is frequently both the slowest and most variable stage in the request pipeline. Under ideal conditions it consumes a substantial portion of the latency budget, and under realistic production conditions it often exceeds the entire budget before the model has generated a single token.


The obvious question is why.


## Why Web Infrastructure Breaks Down for Real Time AI


The default AI stack inherited its architecture from the web.


For more than a decade, web applications have followed the same pattern: keep application servers stateless, centralize data in managed services, and connect everything over the network. That architecture is highly effective for traditional web workloads because most user interactions involve only a handful of network requests, and adding another database query rarely changes the user experience in any meaningful way.


Production AI systems have a fundamentally different execution model.


Instead of serving a page with one or two database lookups, an AI application performs retrieval, tool execution, model inference, memory access, reranking, and additional retrieval repeatedly throughout a conversation. Every conversational turn becomes a pipeline of dependent operations, each introducing another network boundary and another opportunity for latency to accumulate.


The industry is already experiencing the consequences. LangChain's State of Agent Engineering survey found that among teams already deploying agents in production, latency ranks as the second largest engineering challenge, surpassed only by output quality.


The reason becomes obvious once you look beyond raw network latency.


A network hop isn't simply the time required to transmit packets between machines. Every request also incurs serialization, TLS negotiation, authentication, connection pooling, load balancing, scheduling, and queueing before the remote service begins executing. Even within a single cloud region, that overhead commonly adds 40 to 150 milliseconds to every request. Cross region communication can easily increase that to 150 to 400 milliseconds, all before the model has processed a single token.


Those costs become even more significant because AI systems rarely perform one isolated request.


Google's paper The Tail at Scale showed that once requests fan out across multiple services, infrequent slow responses begin to dominate overall system latency. Production AI systems exhibit the same behavior, except instead of making many requests in parallel, they often perform them sequentially. The tail latency of one service becomes the starting point for the next, causing delays to compound across an entire conversational turn.


This is why median latency is often a misleading metric.


A managed vector database might appear perfectly acceptable at P50 while exhibiting dramatically higher latency at P99 because of cold connections, garbage collection pauses, noisy neighbors, or temporary resource contention. In our own production benchmarks, cloud vector database round trips commonly cluster between 500 and 900 milliseconds at P99.


Users never experience your median latency.


They experience your worst moments.


In a conversation lasting twenty turns, those worst case requests stop being statistical outliers. They become inevitable. And once retrieval consumes half a second or more before inference even begins, no language model can make the system feel responsive.


## Layer 1: Models


Models are the layer most teams spend the most time debating, yet they're rarely the primary bottleneck in a production AI system. Foundation models have become remarkably capable, latency continues to improve, and switching between providers is easier than ever. The challenge is no longer finding the best model. It's using the right model for the right job.


Production AI systems shouldn't be built around a single model. They should be built around a portfolio of models optimized for different workloads. Frontier models are reserved for tasks where deeper reasoning justifies the additional latency and cost. Faster models handle routing, classification, tool selection, and other decisions that occur on nearly every turn. Smaller or local models power embeddings, reranking, moderation, and guardrail checks. Most requests in a production system are relatively simple, and routing those decisions to a model that responds in a few hundred milliseconds instead of more than a second is often the easiest latency improvement you'll make.


For interactive applications, time to first token (TTFT) is a far more meaningful metric than benchmark scores or tokens per second. Users notice how quickly a response begins, not how quickly it finishes. TTFT varies significantly across hosted models and grows with prompt size, which means published benchmarks are rarely representative of production workloads. Measure it yourself using realistic prompts and conversation histories rather than synthetic benchmarks.


Model configuration also has a direct impact on latency. Reasoning modes, thinking budgets, and other advanced inference settings can add seconds to every request. Those tradeoffs may be worthwhile for complex planning or analysis, but for most grounded conversational interactions they introduce noticeable delays while providing little measurable improvement in response quality.


Finally, assume every model you choose today will eventually be replaced. The pace of model releases makes portability a practical engineering requirement rather than an architectural ideal. Keeping prompts in version control, maintaining a reliable evaluation suite, and abstracting model providers behind a consistent interface makes it possible to adopt better models as they become available without rebuilding the rest of your system.


## Layer 2: Inference


If models determine what generates the response, inference determines where and how that response is generated. Those decisions establish both the latency floor and the long term cost profile of your system.


The first rule is simple: stream everything. Production AI systems should never wait for an entire completion before responding. Measure time to first token and tokens per second independently because they optimize for different outcomes. TTFT determines perceived responsiveness, while generation speed influences throughput, infrastructure utilization, and overall cost.


Not every response needs to come from a language model. Greetings, acknowledgments, confirmations, and other predictable conversational patterns can often be served from templates, and in voice applications they can be sent directly to text to speech while the model continues reasoning in the background. Delivering the first audio or visual feedback a few hundred milliseconds earlier often makes the entire interaction feel dramatically faster, even when the underlying inference time remains unchanged.


Inference also benefits from aggressive caching. System prompts, tool definitions, and other static prefixes rarely change between requests, making them ideal candidates for prefix caching. Most hosted inference providers and self hosted serving frameworks support this optimization, allowing repeated prompt tokens to be reused instead of reprocessed. For applications with large system prompts, it's one of the simplest and highest impact ways to reduce TTFT.


Finally, choose your inference environment intentionally rather than by default. Hosted APIs provide access to the latest frontier models but introduce network latency and queue variability. Self hosted GPUs offer predictable performance and greater operational control, but require significant infrastructure investment. On device inference eliminates network latency entirely while improving privacy, although current models remain more constrained. Increasingly, production AI systems combine all three, selecting the execution environment that best matches the latency, capability, and cost requirements of each request.


## Layer 3: Search


Every production AI system retrieves information. Whether it's querying a knowledge base, product catalog, documentation, user history, or internal company data, retrieval sits on the critical path of nearly every grounded response. The default architecture has been to attach a hosted vector database to the stack and call it Retrieval Augmented Generation (RAG). That decision has become so common that it's rarely questioned, even though it's often the single largest source of user facing latency.


We refer to this as the retrieval latency tax: the 100 to 500 milliseconds of infrastructure overhead incurred before the model has even seen the context it needs to answer the question. Most of that time isn't spent searching. It's spent crossing the network.


Consider what actually happens during a retrieval request. A customer asks where an order is, the application generates an embedding, sends it to a hosted vector database, waits for the top results to come back, and only then constructs the prompt for the language model. The database itself typically spends only a few milliseconds performing the nearest neighbor search. Everything else is serialization, authentication, network transit, load balancing, and queueing.


The solution isn't simply choosing a faster vector database. It's removing the network from the hot path entirely.


When retrieval executes inside the same runtime as the agent, search starts behaving like a local function call instead of a distributed systems problem. In our own profiling, moving retrieval from a hosted service into the application process reduced median latency from 67 milliseconds to 5 milliseconds, while P99 latency dropped from 222 milliseconds to 13.5 milliseconds. The improvement wasn't the result of a better indexing algorithm. It came from eliminating unnecessary infrastructure between the application and its data.


Our published benchmark illustrates the same pattern across a 100,000 document index with a top k of five, including embedding inference:


The difference isn't search quality. It's architecture. Hosted databases spend most of their latency budget moving requests across infrastructure. In process retrieval spends nearly all of it searching.


Several concerns naturally follow. The first is memory usage. Fortunately, most production knowledge bases are far smaller than many teams assume. A corpus of 10,000 to 100,000 chunks typically requires tens to hundreds of megabytes of memory depending on the embedding dimensions and quantization strategy, making it practical to run inside a server process, browser, or even a modern mobile device.


The second concern is maintaining a source of truth. Local retrieval doesn't replace centralized infrastructure. Instead, it separates the control plane from the data plane. The cloud remains responsible for building, versioning, and distributing indexes, while the runtime executes queries against a local copy. Updates arrive through background synchronization and hot swapped indexes rather than synchronous requests on every conversational turn, following the same architectural pattern that CDNs introduced for web applications years ago.


Finally, semantic search alone is rarely sufficient. Product names, SKUs, error codes, and other exact identifiers don't embed particularly well. Production retrieval systems therefore combine semantic similarity with keyword search, blending both signals into a single query so they can retrieve conceptual matches and exact matches without sacrificing latency.


## Layer 4: Memory


Agent memory often feels like a fundamentally new capability, but in practice it's another retrieval problem.


Every memory operation ultimately answers the same kinds of questions. What has this user told us before? What happened earlier in this conversation? Which preferences should carry over into future sessions? Each of those is simply a search over accumulated state.


That means memory inherits the same architectural constraints as retrieval. If conversation history lives behind another hosted service, every conversational turn introduces an additional network request before the model can respond. The system pays the retrieval latency tax once to retrieve external knowledge and again to remember what it already knows about the user.


It helps to distinguish the three different kinds of memory because each has a different lifetime and belongs in a different part of the architecture.


Working context consists of the messages, retrieved documents, and tool outputs included in the current prompt. It exists only for the duration of a single model invocation.


Session memory captures information established throughout an ongoing conversation, such as user preferences, extracted facts, unresolved tasks, and conversation summaries. Because it's queried continuously, it needs to be available at conversational speed.


Long term memory persists across sessions. It represents durable information about the user, changes relatively infrequently, and is read much more often than it is written.


Production systems generally reflect those different lifetimes in their implementation. Session memory remains inside the runtime as a small, mutable local index that is continuously updated as the conversation evolves. Rather than storing raw transcripts, the agent writes distilled facts, summaries, and structured state, improving retrieval quality while keeping the index compact. Long term memory, meanwhile, is synchronized asynchronously with a persistent system of record after responses are sent or sessions end. When the next conversation begins, that memory is loaded once into the runtime and queried locally alongside the knowledge base, avoiding another network dependency on every turn.


## Layer 5: Sessions


A session is the unit of state in a conversational AI system. One user, one conversation, one evolving context. Unlike traditional web applications, where requests are largely independent and can be routed to any server, AI systems maintain state over minutes or even hours while multiple components update that state simultaneously. The language model, tool calls, user input, and memory layer are all contributing to the same conversation, making session management a core architectural concern rather than an implementation detail.


Most production issues fall into one of two categories.


The first is state leaking across sessions. If two conversations can ever reference the same mutable state, eventually one user will inherit another user's context. These bugs are often rare, difficult to reproduce, and severe when they occur. Every session architecture therefore needs a clear isolation boundary, whether that's a dedicated process, runtime, or another execution model. The important property isn't the implementation itself but the guarantee that state cannot cross that boundary, even under failure conditions.


The second failure mode is state fragmentation within a session. The model maintains its own conversational context, each tool often tracks its own internal state, and the client may keep yet another representation. Over time those views inevitably diverge. An agent might confirm a reservation that has already been canceled because one tool consulted stale state while another had already processed the update. The solution is to establish a single authoritative representation of the session and treat every other view, including the model's context window, as a derived cache rather than the source of truth.


Latency introduces a third challenge. Many of the costs associated with starting a conversation are paid on the very first request, exactly when users are forming their initial impression of the system. A useful way to reason about initialization is to classify every operation by how frequently it should occur: once per deployment, once per worker, once per session, or once per conversational turn. Model loading belongs at worker startup, not session creation. User state should be fetched when the session begins, not when the first question arrives. Ideally, the first turn performs only the work required to answer the first turn.


Finally, sessions must survive failure. Network connections drop, browser tabs close, workers restart, and users frequently return after long periods of inactivity. Persisting session snapshots allows conversations to resume instead of restarting from scratch, while recognizing that capacity planning shifts from requests per second to concurrent active conversations. In production, you're no longer sizing infrastructure around HTTP requests. You're sizing it around live sessions.


## Layer 6: Orchestration


Orchestration coordinates the execution of every layer in the stack. It receives user input, decides what happens next, invokes tools, retrieves context, calls models, updates memory, and repeats that process for every conversational turn. Frameworks such as LiveKit Agents, Pipecat, LangChain, and the Vercel AI SDK provide abstractions for building these execution loops, but the orchestration layer itself is rarely the limiting factor. As Anthropic has observed, the most successful production systems typically rely on simple, composable patterns rather than increasingly complex orchestration frameworks.


What orchestration does amplify is infrastructure latency.


Every tool invocation inherits the cost of the service behind it. An agent that performs three hosted lookups during a single conversational turn doesn't pay the latency once. It pays it three times. A 300 millisecond network request quickly becomes nearly a full second of waiting before the model can produce a response. Replace those same operations with in process function calls and the orchestration logic remains identical, but the user experiences an entirely different system.


That leads to a useful design principle: everything that executes on the hot path should behave like a function call. Network requests should either happen infrequently or be moved off the critical path altogether. Invoking a booking API once during a conversation is rarely a problem. Querying a remote knowledge service on every conversational turn is.


Grounding illustrates why this distinction matters. The most reliable production agents retrieve fresh context before answering substantive questions rather than relying on the model's parametric memory. That strategy significantly improves factual accuracy, but only if retrieval is inexpensive enough to perform on every turn. When retrieval consistently costs several hundred milliseconds, teams begin skipping it to reduce latency. When retrieval behaves like a local function call, grounding becomes the default rather than an optimization that must be rationed.


## Layer 7: Deployment


The final layer is deployment, although it's often the first question teams ask: Where should the application run?


That's the wrong question.


The better question is where should each layer run? A production AI system is no longer deployed to a single environment. Different parts of the stack belong in different places depending on their latency, privacy, and compute requirements.


The same architectural principle that applies to retrieval applies to the entire stack: the cloud should operate as the control plane, while the browser, edge, and device form the data plane. The control plane is responsible for building indexes, distributing updates, synchronizing state, and coordinating infrastructure. The data plane executes the hot path as close to the user as possible.


This represents a meaningful shift from the architecture that defined the last decade of web infrastructure. Traditional applications centralized both state and compute behind managed services because capable computation could only happen inside a data center. AI systems inherited that architecture by default. That assumption is becoming less true every month.


Foundation models are rapidly commoditizing. Performance differences between leading providers continue to narrow, switching costs continue to fall, and the model itself is becoming one of the most replaceable components in the stack. At the same time, local capabilities continue to improve. Small language models are increasingly capable, retrieval already runs comfortably on consumer hardware, and modern browsers, laptops, and mobile devices now include hardware accelerators designed specifically for AI workloads.


As those trends converge, the default architecture shifts toward local first execution. The cloud remains essential for coordination, synchronization, and heavyweight inference, but an increasing portion of the user experience no longer needs to depend on a network request.


## If I Were Building a Production AI System Today


Every architectural decision would start with a single constraint: the latency budget. Rather than selecting technologies first and measuring performance later, I'd define the target latency for the product and work backward from there. Every component in the stack would need to justify the latency it adds.


The resulting architecture would look like this:


Layer Recommendation Why


Models Route across multiple models. Use frontier models for reasoning, fast models for routing and tool selection, and small local models for embeddings, reranking, and guardrails. Most requests don't require the largest model.


Inference Stream every response, enable prefix caching, and avoid unnecessary model calls with templates where appropriate. Minimize time to first token and improve perceived responsiveness.


Search Run hybrid semantic and keyword retrieval in process. Build and distribute indexes from the cloud, but execute queries locally. Eliminates the retrieval latency tax by removing network round trips.


Memory Keep session memory local. Load long term memory once at session start and synchronize asynchronously. Avoids paying another network hop on every conversational turn.


Sessions Isolate each conversation with a single authoritative source of state. Snapshot sessions for recovery and reconnects. Prevents state leakage while making conversations resilient.


Orchestration Keep the execution loop simple. Everything on the hot path should be a local function call whenever possible. Latency compounds across tool calls.


Deployment Treat the cloud as the control plane and the browser, edge, or device as the data plane. Execute latency sensitive work as close to the user as possible.


The architecture follows a few simple principles:


- Route models instead of standardizing on one.
- Treat retrieval as a local operation, not a network service.
- Keep conversational state inside the runtime.
- Move network calls off the critical path whenever possible.
- Separate the control plane from the data plane.
- Optimize for time to first token, not benchmark scores.


The end result isn't a fundamentally different AI stack. It's the same seven layers, reorganized around the constraint that matters most in production: latency. Once retrieval, memory, and session state become local operations instead of distributed systems problems, the entire architecture becomes simpler, faster, and more predictable.


## The Future of the Production AI Stack


The last two years have been dominated by the race for better models. Every release has improved reasoning, expanded context windows, lowered inference costs, or pushed benchmark scores higher, leading many teams to assume that choosing the right model is the primary architectural decision when building an AI product. In practice, however, production systems are increasingly limited by everything surrounding the model rather than the model itself.


Once an application reaches production, latency, reliability, and consistency are determined less by which LLM generates the response and more by how quickly the system can retrieve knowledge, access memory, coordinate tools, maintain session state, and move information between components. Every network hop added to that execution path compounds across a conversation, eventually becoming the dominant factor in the user experience. Improving the model cannot recover time that has already been spent waiting on infrastructure.


That is why we believe the next generation of AI infrastructure will look fundamentally different from the web architectures it inherited. Instead of treating retrieval, memory, and state as remote services accessed over the network on every request, production AI systems will increasingly execute those operations where the conversation is actually happening, whether that's in the browser, on the edge, on device, or alongside the application itself. The cloud doesn't disappear, but its role shifts toward building indexes, synchronizing state, coordinating deployments, and acting as the system of record, while the latency sensitive execution path moves as close to the user as possible.


This architectural shift is what led us to build Moss.


Rather than treating search as another hosted service, Moss executes hybrid semantic and keyword retrieval directly inside the runtime, allowing agents to retrieve knowledge in single digit milliseconds without paying the retrieval latency tax that has become standard across much of the industry. The goal isn't simply to build a faster search engine, but to make retrieval behave like any other local function call so that developers can ground every meaningful interaction without compromising responsiveness.


If the central argument of this article is that production AI systems should be designed around latency budgets rather than infrastructure defaults, then Moss is our implementation of that philosophy. We believe the fastest AI systems won't be the ones with the best models alone, but the ones whose surrounding infrastructure is architected so efficiently that the model can begin reasoning almost immediately. That's the stack we're building toward, and we think it's where production AI is headed.
