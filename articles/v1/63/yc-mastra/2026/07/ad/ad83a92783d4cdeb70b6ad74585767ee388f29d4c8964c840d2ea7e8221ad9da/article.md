---
schema_version: "1.0.0"
document_id: "ad83a92783d4cdeb70b6ad74585767ee388f29d4c8964c840d2ea7e8221ad9da"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/how-to-create-a-chatbot"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-28T17:31:22.495262+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:978d4144383f2bfa3acaa13f26e04954269523adfe35db2faa2fa7eb91301504"
---

# How to create a chatbot: a complete guide for developers

If you’re building an AI chatbot in 2026, you can get something working with an LLM API call and a few dozen lines of TypeScript. Getting it production-ready is the harder part, and the right approach depends on your use case, your team, and how much control you need over the model’s behavior. Demand keeps rising too, with the global chatbot market reaching an estimated[$9.56 billion in 2025](https://www.grandviewresearch.com/industry-analysis/chatbot-market) .


Production chatbots today are generative AI systems built on large language models, accessed through an API rather than trained in-house. The engineering work sits around the model rather than inside it: prompts, memory, retrieval, tools, and the observability to tell you when any of them break.


This guide walks you through every stage of how to create a chatbot: scoping the use case, choosing a model provider, writing the request loop, designing prompts, adding retrieval-augmented generation and tool calling, testing, and deploying to production.


## Planning your chatbot: goals, scope, and knowledge sources


Your chatbot project will succeed or fail based on decisions you make before writing any code. Start with the problem you’re solving, not the technology.


### Defining the use case and user intent


You need to answer three questions before anything else. What specific task does this chatbot perform? Who are the users? What does a successful conversation look like?


Map out the core user intents your chatbot must handle. A support chatbot might need to handle billing questions, password resets, and escalation to human agents. A data science team’s AI assistant might need to explain query results and suggest visualizations. Write out 20 to 30 example conversations covering both happy paths and edge cases.


### Choosing a domain and gathering knowledge sources


The data that shapes your chatbot is not a training set. It is your system prompt, your few-shot examples, and the knowledge base you feed into retrieval-augmented generation. Each one is a lever you can pull in minutes rather than weeks.


Scope that knowledge base to the domain you defined, then clean it aggressively. Remove duplicates, normalize formatting, and strip out stale documents. Retrieval quality degrades fast when your corpus holds three versions of the same policy page, and the model gives you no signal about which one it picked.


### Choosing a model provider


You should start with a hosted LLM provider unless you have a specific reason not to. For text-based AI chatbots, OpenAI, Anthropic, and Google Gemini offer state-of-the-art models with well-documented APIs. Use a model routing library so you can swap providers later without rewriting your integration layer.


Self-hosting an open-weight model like Llama makes sense in a narrow set of cases: regulatory constraints on data residency, specialized domains where commercial models underperform, or extreme latency requirements. Even then, prove the chatbot works against a hosted API first. Moving a model in-house is an infrastructure decision, and you want your evals in place before you make it.


## Building a chatbot with an LLM API


This is where most chatbot development effort lands in 2026. You call a hosted LLM, shape its behavior with a system prompt, give it the context it needs, and wrap the whole thing in code you control. You get broad domain coverage and multi-turn conversation support without building any of it yourself. The tradeoffs are cost, latency, and the risk of hallucination.


### Deciding what your chatbot needs: completion loop, RAG, or agent


Every LLM chatbot is a loop around a model call. What changes between projects is how much you put between the user’s message and the model, and how much freedom the model has to act on its own. A hosted chatbot builder will make this choice for you; building it yourself means making it deliberately. Three shapes cover almost everything you will build.


**Shape** **What it adds** **Reach for it when**


Completion loop A system prompt and conversation history The model’s own knowledge answers the question


Retrieval (RAG) Relevant documents pulled in at query time Answers have to come from your content


Agent Tools the model can call and multi-step execution Answering requires a lookup or an action


Start at the top and move down only when you hit a wall. A completion loop that handles 80% of your traffic correctly is worth more than an agent architecture you cannot debug. Every layer you add multiplies latency, cost, and the number of ways a conversation can go wrong. Most conversational AI projects land between the second and third row, and the sections that follow build these up in order so you can stop wherever your use case is satisfied.


### Using the OpenAI API, Anthropic, or Gemini API


You call the provider’s chat completions endpoint with a list of messages (system, user, assistant turns) and receive a generated response. The OpenAI API, Anthropic’s Claude API, and Google’s Gemini API all follow this pattern. The table below summarizes how the major providers compare when you are picking a default.


**Provider** **API entry point** **Notable strength**


OpenAI Chat completions endpoint Broad tooling and a mature developer ecosystem


Anthropic Messages API (Claude) Long-context reasoning and prompt caching


Google Gemini Gemini API Very large context windows for document-heavy tasks


Start with a larger model and optimize for cost later. The right sequence, as[Principles of Building AI Agents](https://mastra.ai/books/principles-of-building-ai-agents) frames it, is: make it work, make it right, make it fast and cheap. Larger models like Claude’s Opus or GPT-5 produce higher quality output but cost more per token. Once your chatbot works correctly, experiment with smaller models for responses that do not require deep reasoning.


### Writing the request loop


Your chatbot’s core is a function that takes a conversation and returns the next message. Everything else in this guide is a layer wrapped around it. Set your API key as an environment variable first, so the same code runs in your development environment and in production.


```text
import   Anthropic   from   "  @anthropic-ai/sdk  "  ;


const   client   =   new   Anthropic  ({   apiKey: process.env.ANTHROPIC_API_KEY });
const   MODEL   =   "  claude-sonnet-5  "  ;


const   SYSTEM_PROMPT   =   `  You are a support assistant for Acme Cloud.
Answer in two sentences or fewer. If you do not know, say so.  `  ;


// A turn is plain text, or the content blocks the API gave back.
type   ContentBlock   =   Anthropic  .  ContentBlockParam  ;
type   Turn   =   {   role  :   "  user  "   |   "  assistant  "  ;   content  :   string   |   ContentBlock  []   };


export   function   textOf  (message  :   Anthropic  .  Message  )   {
return   message.content
.  map  ((block)   =>   (block.type   ===   "  text  "   ?   block.text   :   ""  ))
.  join  (  ""  );
}


export   async   function   respond  (history  :   Turn  [],   userMessage  :   string  )   {
const   messages  :   Turn  []   =   [  ...  history, { role:   "  user  "  , content: userMessage }];


const   response   =   await   client.messages.  create  ({
model: MODEL,
max_tokens:   1024  ,
system: SYSTEM_PROMPT,
messages,
});


const   text   =   textOf  (response);


return   {   text,   messages: [  ...  messages, { role:   "  assistant  "  , content: text }] };
}
```


Three details in that function matter more than they look. The model is stateless, so you send the entire conversation on every call and pay for it every time. The system prompt travels separately from the message list, which is what lets providers cache it across turns. And the response comes back as a list of content blocks rather than a string, which is why a turn is typed as either text or blocks: the moment you add tools, an assistant turn stops being a string and has to be stored exactly as the API returned it.


Return the updated message list, not just the text. Your caller needs it for the next turn, and threading it through explicitly keeps conversation state out of module-level variables, which will bite you the moment two users talk to your chatbot at the same time.


The same shape works against OpenAI and Gemini. Parameter names change and the system prompt moves into the message list on some providers, but the loop does not.


### Prompt design and system instructions


Your system prompt defines your chatbot’s behavior. Give it a clear role, specific constraints, and explicit avoidance instructions. A medical chatbot’s system prompt might include: a role (“You are a patient intake assistant”), output constraints (“Always ask one question at a time”), and guardrails (“Do not provide diagnoses or medical advice”).


Three prompt engineering techniques give you increasing control over output format:


-


Zero-shot: describe the task and let the model figure out the format


-


Single-shot: provide one example of the desired input-output pair


-


Few-shot: provide multiple examples for precise formatting control


Put examples in the system prompt and enable prompt caching. Most major providers now support prompt caching, which reduces both cost and latency for repeated calls with the same system prompt.


### Adding retrieval-augmented generation for domain knowledge


If your chatbot needs to answer questions about specific documents, products, or knowledge that the LLM was not trained on, you need retrieval-augmented generation (RAG).


A RAG pipeline works in five steps:


1.


Chunking: split your documents into bite-sized pieces for search.


2.


Embedding: transform each chunk into a vector representation using an embedding API from OpenAI, Voyage, or Cohere.


3.


Indexing: store vectors in a vector database optimized for similarity search.


4.


Querying: convert the user’s message into an embedding and find the most similar chunks.


5.


Synthesis: pass the retrieved chunks as context into the LLM along with the user’s question.


Before building a full RAG pipeline, consider simpler approaches. If your corpus is small enough, feed it directly into the context window. If your data is structured, give the AI agent search tools instead of pre-parsing documents. Many chatbot builder platforms, including LangChain and framework-level solutions, offer pre-built RAG integrations that reduce your setup time.


## Giving your chatbot memory


Your chatbot is only as good as the context you hand it. That context has to survive page refreshes, stay under the model’s limit, and cost you as little as possible on every turn.


### Managing conversation history and context windows


Your chatbot needs memory of the current conversation. Pass the full conversation history as a list of messages with each API call. This gives the LLM complete context for generating contextually appropriate responses.


Context windows have grown significantly. As of early 2026, Gemini supports[one million tokens](https://blog.google/technology/google-deepmind/gemini-model-thinking-updates-march-2025/) and Anthropic offers the same for Claude Sonnet in the API. But larger context windows do not mean you should dump everything in.


Around 100k tokens, even models with large context windows start to lose the ability to discern important information from noise. Manage your context deliberately: compress older turns through summarization when the conversation grows long, remove verbose tool call results, and prioritize recent turns and key reference information.


### Persisting conversations across sessions


Holding history in memory works until your process restarts or your traffic spans two instances. Store messages in a database keyed by conversation ID and load them at the start of each turn. The db calls below are whatever ORM you already use.


```text
async   function   loadHistory  (conversationId  :   string  )  :   Promise  <  Turn  []>   {
const   rows   =   await   db.messages.  findMany  ({
where: { conversationId },
orderBy: { createdAt:   "  asc  "   },
});


return   rows.  map  (({   role,   content   })   =>   ({   role,   content }));
}


async   function   appendTurns  (conversationId  :   string  ,   turns  :   Turn  [])   {
await   db.messages.  createMany  ({
data: turns.  map  ((turn)   =>   ({
...  turn,
conversationId,
createdAt:   new   Date  (),
})),
});
}
```


Write the user message and the assistant reply together, after the model call returns. If you save the user message first and the call fails, you will reload a conversation that ends on an unanswered question and the model will answer it twice on the next turn.


Store raw content blocks rather than flattened text once you add tool calling. A tool call and its result have to be replayed to the model in their original form, and a conversation missing either half will be rejected on the next request.


### Trimming and summarizing long conversations


Conversations grow until they hit the context window or your cost ceiling, whichever comes first. Set a token budget well below the model’s limit, then enforce it on every turn.


```text
const   TOKEN_BUDGET   =   40_000  ;
const   KEEP_RECENT   =   10  ;


async   function   countTokens  (system  :   string  ,   messages  :   Turn  [])   {
const   {   input_tokens   }   =   await   client.messages.  countTokens  ({
model: MODEL,
system,
messages,
});


return   input_tokens;
}


async   function   fitToBudget  (system  :   string  ,   messages  :   Turn  [])  :   Promise  <  Turn  []>   {
if   ((  await   countTokens  (system,   messages))   <=   TOKEN_BUDGET)   return   messages;


const   recent   =   messages.  slice  (  -  KEEP_RECENT);
const   older   =   messages.  slice  (  0  ,   -  KEEP_RECENT);
if   (older.  length   ===   0  )   return   recent;


const   summary   =   await   client.messages.  create  ({
model: MODEL,
max_tokens:   512  ,
system:
"  Summarize this conversation. Keep decisions, names, numbers, and open   "   +
"  questions. Drop pleasantries.  "  ,
messages: [{ role:   "  user  "  , content: JSON.  stringify  (older) }],
});


return   [
{ role:   "  user  "  , content:   `  Earlier in this conversation:   ${  textOf  (summary)  }`   },
...  recent,
];
}
```


Summarize instead of truncating. Dropping the oldest turns loses exactly the information users assume you still have, like the account number they gave you in their first message. A summary keeps the facts and discards the phrasing.


Cache the summary against the conversation ID so you are not paying for a summarization call on every turn once a conversation runs long. Regenerate it only when the recent window fills up again.


## Giving your chatbot tools


An AI chatbot that can only talk is limited to what the model already knows and whatever you managed to fit in its prompt. Tools let it check an order status, search your docs, or file a ticket. This is the line where a chatbot becomes a virtual agent that acts on the user’s behalf, and where most of the interesting failure modes start.


### Defining a tool and its input schema


A tool definition is a name, a description, and a JSON schema for its inputs. The model never runs your code. It reads the definitions, decides one is relevant, and returns a structured request for you to execute. Declaring the schema once in Zod and converting it keeps the contract the model sees and the validator you run from drifting apart.


```text
import   {   z   }   from   "  zod  "  ;


// One schema, used for both the model-facing contract and runtime validation.
const   orderSchema   =   z.  object  ({
orderId: z.  string  ().  describe  (  "  Order ID, formatted like AC-12345  "  ),
});


const   tools   =   [
{
name:   "  get_order_status  "  ,
description:
"  Look up the current status of a customer order. Use when the user asks   "   +
"  where their order is, whether it shipped, or when it will arrive. Do not   "   +
"  use for refund or cancellation requests.  "  ,
input_schema: z.  toJSONSchema  (orderSchema),
},
];
```


The description is the prompt. It is the only thing the model uses to decide whether the tool applies, so write it for a reader with no other context: what the tool does, when to reach for it, and when not to. Vague descriptions are the most common reason a tool either never fires or fires constantly.


Keep the schema tight. Every optional parameter is one more decision the model can get wrong, and required fields with explicit formats give you something to validate against. Your executor is a plain dispatch function, and it is the right place to enforce that schema before you touch your own systems.


```text
async   function   runTool  (name  :   string  ,   input  :   unknown  )   {
switch   (name)   {
case   "  get_order_status  "  :   {
// Throws on malformed input, before anything reaches the database.
const   {   orderId   }   =   orderSchema.  parse  (input);
const   order   =   await   db.orders.  findUnique  ({   where: { id: orderId } });


return   order
?   `  Status:   ${  order.status  }  . Estimated delivery   ${  order.eta  }  .  `
:   `  No order found with ID   ${  orderId  }  .  `  ;
}
default  :
return   `  Unknown tool:   ${  name  }`  ;
}
}
```


### Running the tool-calling loop


Tool use turns a single request into a loop. The model returns a tool call, you execute it, you send the result back, and the model either answers or calls another tool. You keep going until it stops asking.


```text
async   function   respondWithTools  (history  :   Turn  [],   maxSteps   =   5  )   {
let   messages  :   Turn  []   =   [  ...  history];


for   (  let   step   =   0  ;   step   <   maxSteps;   step  ++  )   {
const   response   =   await   client.messages.  create  ({
model: MODEL,
max_tokens:   1024  ,
system: SYSTEM_PROMPT,
tools,
messages,
});


messages   =   [  ...  messages, { role:   "  assistant  "  , content: response.content }];


if   (response.stop_reason   !==   "  tool_use  "  )   {
return   {   messages,   text:   textOf  (response) };
}


// flatMap narrows the block type; filter alone does not.
const   calls   =   response.content.  flatMap  ((block)   =>
block.type   ===   "  tool_use  "   ?   [block]   :   []
);


const   results  :   ContentBlock  []   =   await   Promise  .  all  (
calls.  map  (  async   (call)   =>   ({
type:   "  tool_result  "   as   const,
tool_use_id: call.id,
content:   await   runTool  (call.name, call.input),
}))
);


messages   =   [  ...  messages, { role:   "  user  "  , content: results }];
}


throw   new   Error  (  "  Tool loop exceeded maximum steps  "  );
}
```


The step limit is not optional. A model that misreads a tool result can call the same tool indefinitely, and without a ceiling you will find out from your invoice. Five steps covers almost every legitimate chain.


Return errors to the model as tool results rather than throwing. If the order ID does not exist, send back a result that says so and the model will ask the user for a correct one. If you throw, the conversation dies on a stack trace.


Validate every tool input on the way in. The model produces well-formed JSON most of the time, and most of the time is not a security model when the tool writes to your database or spends your money. The schema parse in runTool is doing that job, which is why the executor takes its input as unknown rather than trusting a shape it never checked.


### Returning structured output


When your chatbot needs to hand data to your own code rather than to a user, do not parse prose. Define the shape you want as a tool and force the model to use it.


```text
const   intentSchema   =   z.  object  ({
intent: z.  enum  ([  "  billing  "  ,   "  technical  "  ,   "  sales  "  ,   "  other  "  ]),
urgency: z.  enum  ([  "  low  "  ,   "  normal  "  ,   "  high  "  ]),
summary: z.  string  (),
});


const   response   =   await   client.messages.  create  ({
model: MODEL,
max_tokens:   512  ,
tools: [
{
name:   "  record_intent  "  ,
description:   "  Record the classified intent of the user's message.  "  ,
input_schema: z.  toJSONSchema  (intentSchema),
},
],
tool_choice: { type:   "  tool  "  , name:   "  record_intent  "   },
messages: [{ role:   "  user  "  , content: userMessage }],
});


const   call   =   response.content.  find  ((block)   =>   block.type   ===   "  tool_use  "  );
if   (call?.type   !==   "  tool_use  "  )   throw   new   Error  (  "  Model skipped the tool call  "  );


const   {   intent,   urgency,   summary   }   =   intentSchema.  parse  (call.input);
```


Forcing the tool choice guarantees you get the schema back instead of a paragraph describing the schema. The enums are doing real work here: an open string field for intent will drift into categories your routing code has never seen.


Parsing the result rather than casting it is the difference between a bad classification and a silent one. Required fields come back reliably, but the moment you depend on that without checking, a model or provider update will prove you wrong in production.


## Streaming responses and handling failures


Two things separate a chatbot that feels finished from one that does not. It starts answering immediately, and it does not fall over when the provider has a bad minute.


### Streaming responses token by token


A non-streaming chatbot sits silent for several seconds and then produces a wall of text. Streaming starts the response in a few hundred milliseconds, which is the difference between a user who waits and one who reloads the page.


```text
export   async   function   POST(  request:   Request  )   {
const   {   conversationId,   message   }   =   await   request.json  ();
const   history   =   await   loadHistory  (  conversationId  );


const   stream   =   client.messages.stream  ({
model:   MODEL,
max_tokens:   1024,
system:   SYSTEM_PROMPT,
messages:   [...history,   {   role:   "  user  "  ,   content:   message   }],
});


const   encoder   =   new   TextEncoder  ();


return   new   Response  (
new   ReadableStream  ({
async   start  (  controller  )   {
stream.on(  "text"  ,   (delta) =  >   {
controller.enqueue(encoder.encode(  `  data:   ${JSON.stringify({ delta }  )}\n\n  `  )  )  ;
}  )  ;


const   final   =   await   stream.finalMessage  ();
await   appendTurns  (  conversationId,   [
{   role:   "  user  "  ,   content:   message   },
{   role:   "  assistant  "  ,   content:   textOf  (  final  )   },
]);


controller.close  ();
}  ,
}),
{   headers:   {   "  Content-Type  "  :   "  text/event-stream  "   }   }
);
}
```


Persist the assistant message after the stream finishes, not as it arrives. A user who closes the tab halfway through leaves you with a partial reply, and storing that fragment means the next turn starts from a sentence that stops mid-word.


Handle tool calls differently in the stream. Users do not want to watch JSON arrive, so surface a short status line while the tool runs and resume streaming text when the model picks back up.


### Retries, timeouts, and rate limits


Provider APIs fail in three ways worth planning for: rate limits when you send too much too fast, overload errors when the provider is saturated, and timeouts when a long generation stalls. All three are retryable, and none of them should reach the user as a stack trace.


```text
const   RETRYABLE   =   new   Set  ([  408  ,   429  ,   500  ,   502  ,   503  ,   529  ]);


async   function   withRetry  <  T  >(  call  :   ()   =>   Promise  <  T  >,   attempts   =   4  )  :   Promise  <  T  >   {
for   (  let   attempt   =   0  ;   attempt   <   attempts;   attempt  ++  )   {
try   {
return   await   call  ();
}   catch   (error  :   any  )   {
if   (  !  RETRYABLE.  has  (error?.status)   ||   attempt   ===   attempts   -   1  )   throw   error;


const   retryAfter   =   Number  (error?.headers?.[  "  retry-after  "  ])   *   1000  ;
const   backoff   =   2   **   attempt   *   500   +   Math.  random  ()   *   250  ;


await   new   Promise  ((resolve)   =>   setTimeout  (resolve,   retryAfter   ||   backoff));
}
}


throw   new   Error  (  "  unreachable  "  );
}
```


Honor the retry-after header when the provider sends one, and add jitter to your backoff. Without jitter, every request that failed in the same second retries in the same second, which recreates the burst that triggered the limit.


Set your own timeout shorter than your platform’s. Serverless functions terminate without running your error handling, so a request that dies at the platform edge leaves you with no log line, no saved message, and no idea why the conversation stopped.


Fall back rather than fail. Route to a smaller model, then to a cached answer, then to a plain apology with a handoff to human agents. Each step down is worse than the last, and all of them beat a spinner that never resolves.


*An agent framework connects agents to tools, memory, and model providers through a unified interface.*


## Building production chatbots with Mastra


If you’re building your chatbot in TypeScript,[Mastra](https://mastra.ai/) gives you a framework that handles the agent scaffolding, model routing, memory, and workflow orchestration so you can focus on your chatbot’s domain logic rather than plumbing.


You create an agent with instructions, attach tools, and configure memory in a few lines of code. Model routing supports over 90 providers through a single interface, so you can swap between OpenAI, Anthropic, and Gemini with a one-line change. Conversations get persistent memory through built-in memory processors like TokenLimiter (which prunes the oldest messages when context approaches the limit) and ToolCallFilter (which removes verbose tool call results to save tokens).


```text
import   {   Agent   }   from   "  @mastra/core/agent  "  ;
import   {   createTool   }   from   "  @mastra/core/tools  "  ;
import   {   Memory   }   from   "  @mastra/memory  "  ;
import   {   TokenLimiter,   ToolCallFilter   }   from   "  @mastra/memory/processors  "  ;
import   {   z   }   from   "  zod  "  ;


const   getOrderStatus   =   createTool  ({
id:   "  get_order_status  "  ,
description:   "  Look up the current status of a customer order.  "  ,
inputSchema: z.  object  ({
}),
execute  :   async   ({ orderId })   =>   {
const   order   =   await   db.orders.  findUnique  ({ where: { id: orderId } });
return   order   ??   { error:   `  No order found with ID   ${  orderId  }  .  `   };
},
});


export   const   supportAgent   =   new   Agent  ({
id:   "  support-agent  "  ,
name:   "  Support Agent  "  ,
instructions: SYSTEM_PROMPT,
model:   "  anthropic/claude-sonnet-5  "  ,
tools: { getOrderStatus },
memory:   new   Memory  ({
processors: [  new   ToolCallFilter  (),   new   TokenLimiter  (  127000  )],
}),
});


const   stream   =   await   supportAgent.  stream  (  "  Where is order AC-12345?  "  ,   {
memory: { resource: userId, thread: conversationId },
});
```


That is the request loop, the tool dispatch, the memory trimming, and the retry handling from the preceding sections, minus a few hundred lines you would otherwise own and keep current. Every piece is still configurable, and running mastra dev gives you a local development environment where you can watch each tool call and memory write as it happens.


For chatbots that need to answer domain-specific questions, the built-in RAG pipeline supports recursive, character-based, token-aware, and format-specific chunking strategies out of the box.


*Mastra Studio shows a chatbot agent responding in an interactive chat interface.*


When your chatbot grows beyond a single agent, the workflow engine lets you chain steps, branch conditionally, and suspend and resume runs that need human approval or external data.[Build your first chatbot agent with Mastra](https://mastra.ai/ai-agent-framework) .


## Testing, observability, and debugging your chatbot


Your chatbot will behave differently in production than in development. You need automated testing, tracing, and evaluation to catch regressions and monitor quality over time.


### Unit-testing intents and response paths


You should write unit tests for each core user intent your chatbot handles. Test that your tool-calling logic triggers correctly and that structured outputs match expected schemas.


Treat chatbot testing like API contract testing. Define expected behaviors for critical conversation flows and test them in CI.


### Tracing multi-turn conversations


A trace records every step in your chatbot’s response pipeline as a tree of spans, following the[OpenTelemetry](https://opentelemetry.io/docs/) standard. Each span captures the inputs, outputs, latency, and metadata for one operation, whether it is an LLM call, a tool invocation, or a RAG retrieval.


Tracing tells you exactly where your chatbot spent time and tokens. It surfaces silent failures that return 200 OK but produce wrong answers. Without tracing, debugging multi-turn conversations is guesswork.


*An agent trace hierarchy shows how each chatbot response breaks down into LLM calls, tool invocations, and retrieval steps.*


### Evaluating response quality with automated evals


You cannot rely on traditional pass-fail tests for chatbot outputs because they are non-deterministic. Evals give you quantifiable metrics for measuring quality instead.


The most practical eval approach is LLM-as-judge: pass the chatbot’s output, the original input, and any retrieved context to a separate judge model with a scoring rubric. This scales well and handles cases where there is no single correct answer. Use a judge from a different model family than your chatbot to reduce bias.


Build your eval dataset in layers, the way data science teams build any labeled set. Start with hand-curated examples that force you to define “good,” then generate synthetic cases for coverage, and finally mine production logs for real-world edge cases.


### Monitoring for drift, hallucinations, and prompt injection


Your chatbot can regress without any code change. Model provider updates, shifting user behavior, and prompt injection attempts all degrade quality over time.


Monitor for three failure categories:


-


Accuracy drift: response quality degrades as user queries shift outside your eval dataset’s distribution


-


Hallucination: the chatbot generates confident but incorrect information, especially in RAG scenarios where retrieval misses


-


Prompt injection: users or embedded content attempt to override your system prompt


Set up automated alerts for each category. Built-in observability tools can surface traces, token costs, and eval results in a single dashboard, letting you run offline evals against a fixed dataset before deploys and online evals against live production traffic.


## Deploying your chatbot


Your chatbot needs a serving layer, a user interface, and an operational strategy for updates and rollbacks. How you deploy determines whether your virtual agent stays reliable at scale.


### Packaging and serving your chatbot backend


Your chatbot backend is a lightweight server that manages conversation state and proxies requests to the provider. Keep your API keys server-side, behind an API gateway or reverse proxy. Full agent logic cannot live client-side in the browser because it would expose your credentials.


Containerize the server so your deploy target stays interchangeable, and load prompts, tool definitions, and model configuration from environment variables rather than baking them into the image. You will change all three more often than you change the application code.


### Embedding a chatbot in a web or mobile interface


Your frontend needs a chat interface that streams responses, auto-scrolls, and displays tool call results. For JavaScript or TypeScript frontends, libraries like Vercel’s AI SDK UI handle the streaming and rendering boilerplate.


Your backend already streams, so the frontend’s job is to consume the event stream and render it: append each delta as it arrives, keep the viewport pinned to the newest text unless the user has scrolled up, and show a status line while a tool runs. Handle the disconnect case explicitly, because a dropped stream leaves a half-written message on screen that a reload will not reproduce.


### Hosting options: serverless, edge, and self-hosted


Your hosting choice depends on your traffic pattern and latency requirements. The table below summarizes the main options.


**Option** **Best for** **Watch out for**


Serverless (Vercel, Netlify, Cloudflare Workers) Auto-scaling, zero infrastructure management Timeout limits on long-running agent loops


Container services (AWS ECS, DigitalOcean, Fly.io) Sustained workloads, more runtime control Requires capacity planning


Self-hosted (your own servers) Maximum control and data residency Highest operational burden


Most teams deploying LLM-powered chatbots use auto-scaling managed services. Container services work well for B2B use cases without sudden usage spikes. According to the[CNCF State of Cloud Native Development Q1 2025 report](https://www.cncf.io/reports/state-of-cloud-native-development/) , container usage among backend developers has held steady at around 61% for several years, while serverless usage has actually declined as AI and ML workloads shift compute economics.


### Versioning, rollback, and continuous improvement


Version your system prompts, tool definitions, and eval datasets alongside your application code. When you change a prompt or swap a model, run your eval suite before deploying.


Keep at least two versions deployed (current and previous) so you can roll back quickly if evals or user feedback surface a regression. Monitor user feedback signals like thumbs-up/down ratings, conversation abandonment rates, and escalation frequency. These signals feed back into your chatbot builder workflow, helping you prioritize which intents or conversation flows to improve next.


## Wrapping up


You now have the full picture of how to create a chatbot, from scoping the use case and choosing a provider to deploying an LLM-powered agent with tools, RAG, and observability. Start with the simplest approach that solves your problem, then iterate based on real user behavior and eval results.


If you’re working in TypeScript,[Mastra](https://mastra.ai/ai-agent-framework) gives you the agent framework, model routing, and workflow orchestration to get your chatbot to production faster.
