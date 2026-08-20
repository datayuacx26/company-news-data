---
schema_version: "1.0.0"
document_id: "116a8ba0c1bdae87aa1b2c221a5de3046b6d1c652951b91208c02cbcf9dfd64f"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/evals-as-code/"
published_at: "2025-08-04T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:600ebfc8388e91d225811bb9b749b77f6fd983554ed1091c54119acd0f3a4cd4"
---

# Evals as Code: CI for LLMs with Dagger

*Co-authored by Alex Suraci and Sam Alba*


Starting in February this year we shipped[Dagger’s support for orchestrating LLMs](https://dagger.io/blog/llm) . The goal was to make Dagger a great platform for implementing AI agents in software development and delivery workflows.


What that meant for us was figuring out how to translate Dagger APIs into tools the agent can use to perform its task inside a sandboxed environment.


In this post, we’ll share how we implemented the support for LLM’s Tools calling, how we interfaced the whole Dagger API with LLMs, and how we implemented Evals to continuously test this code.


The core workflow for Dagger AI agents is this:


1. *Write Dagger functions for whatever actions you need your agent to do.*
2. *Create an Env for your agent and provide those functions via input objects.*
3. *Plug that Env into an LLM and write your prompts for the agent.*
4. **The agent gets to work by calling tools, which map to the Dagger API.**
5. *Retrieve the agent’s work from the Env via output objects.*


This first part of the post covers the second to last step: how does the agent see and interact with the Dagger API?


The second part teaches you how to reuse[the Dagger Evaluator module](https://daggerverse.dev/mod/github.com/dagger/dagger/modules/evaluator) and how to implement efficient evals yourself.


## Building support for LLMs and Tool-calling into the Dagger API


The hardest part, by far, was figuring out how to express Dagger’s API to models in a form that is unambiguous and model-agnostic.


The bit about “providing tools” was complex to design: how do you expose the full Dagger API in a way that LLMs can reliably discover, compose and call the right functions – without overfitting to any one provider’s tool-calling quirks?


Or to put it simply: teaching LLMs how to use the Dagger API via tool calling.


Working on this implementation was not a straight line. It took five iterations (just counting the attempts that led to mergeable code).


For readers curious about the machinery under the hood, feel free to take a look at those five iterations:[0](https://github.com/dagger/dagger/blob/06194e5f3b90f1f6493d1ea94bb0068c5f4fac1a/core/llm_env.go) ,[1](https://github.com/dagger/dagger/pull/9956) ,[2](https://github.com/dagger/dagger/pull/9993) ,[3](https://github.com/dagger/dagger/pull/10134) ,[4](https://github.com/dagger/dagger/pull/10366) .


One advantage of using Dagger when building Dagger is that we can use Dagger Cloud to debug the LLM behaviors along the way.


Here is a peek at some of those Dagger Cloud Traces captured, so you can better understand the challenges encountered…


Weird behavior #1 – Here the LLM builds a binary only to destroy it, symbolizing the transitory nature of material life. ([See the Trace on Dagger Cloud](https://dagger.cloud/dagger/traces/c601169de683e1d73bab4bada675247a?span=dec232fa197423bb) ):


Weird behavior #2 – Here the LLM learns how to do math. It had to think about it. ([See the Trace on Dagger Cloud](https://dagger.cloud/dagger/traces/f91012f1c190722a6ef32e28cf8ce15f?span=b5a455ced9e5bd2b) ):


Weird behavior #3 – Gemini gets SO FRUSTRATED when you name a tool` return` . ([See the Trace on Dagger Cloud](https://dagger.cloud/dagger/traces/abe3fb3875bc836714a541ef547be393) ):


Weird behavior #4 – At this point this is getting awkward… ([See the Trace on Dagger Cloud](https://dagger.cloud/dagger/traces/2bab990ea29cfdfc91e920ae80f6934e?span=b88f8bf69b32ff12) ):


Weird behavior #5 – That’s not *quite* what I meant… ([See the Trace on Dagger Cloud](https://dagger.cloud/dagger/traces/d0cb57c172f7af903bdb7b30c3518cc8?span=659e2f85aa1093c0&debug) ):


Obviously, struggling through those issues helped us to learn a lot about the LLMs behavior. But testing LLMs by hand makes for a brutally slow feedback loop, and with how mercurial LLMs can be, running anything just once doesn’t inspire a whole lot of confidence, especially when we want to ship the product to all users. Eventually we needed a pattern for writing evals and running multiple attempts in parallel across a bunch of different models.


## What are evals?


Originally we thought the name “evals” was a coping mechanism: if you called them tests they’d be considered flaky. Turns out they’re closer to benchmarks? Except they’re not for measuring speed, so I guess making everyone learn a new term is kind of justified.


Evals measure the performance of an LLM for a given prompt, where “performance” might mean any/all of the following:


- Whether the LLM reached the desired state or produced the desired response x% of the time
- How many input/output tokens it took (and how many were cached)
- How many turns it took
- A custom score for a task-specific metric
- A score produced by *another* LLM grading the result
- Wall clock time, though this seems lower priority than usual


## What do evals look like?


Evals always have a name, a prompt, and a way to judge the result of the prompt.


We started writing them with plain old Go` testing` patterns, but that quickly proved insufficient. Yes, we could run the evals a bunch of times, but we didn’t really want to fail the whole suite if only, say, 1 in 10 runs failed. It was also a slow feedback loop: for each run, we had to analyze what each attempt did, and look for patterns across them, and try to interpret what that meant for our design.


So you don’t need to go through the same challenges yourself, we built the[Dagger Evaluator module](https://daggerverse.dev/mod/github.com/dagger/dagger/modules/evaluator@eac70cb839dfe461747ddcab9f5e3abb52151cd1) . This part explains how it’s architectured but keep it mind you can reuse it in order to build Evals for your own applications.


Here’s the Minimum Viable Interface for Evals that we ended up with in the module’s implementation:


```text
type   Eval   interface   {
// The name of the eval, to display to the user.
Name  (  context  .  Context  ) (  string  ,   error  )
// Returns the prompt that is going to be evaluated.
Prompt  (  base   *  dagger  .  LLM  )   *  dagger  .  LLM
// Validates the output of the prompt.
Check  (  ctx   context  .  Context  ,   prompt   *  dagger  .  LLM  )   error
// Export this as a Dagger interface
DaggerObject
}
```


And here’s an` Eval` implementation, simplified from[Dagger’s evals](https://github.com/dagger/dagger/blob/f3c88ce642ecda2b7106601acf2d620498af8f26/modules/evals/build_multi.go) :


```text
type   BuildMulti   struct  {}


// Test the agent's ability to pass objects around to one another and execute a
// series of operations given at once.
func   (  m   *  Evals  )   BuildMulti  ()   *  BuildMulti   {
return   &  BuildMulti  {}
}


func   (  e   *  BuildMulti  )   Name  ()   string   {
return   "BuildMulti"
}


func   (  e   *  BuildMulti  )   Prompt  (  base   *  dagger  .  LLM  )   *  dagger  .  LLM   {
return   base.
WithEnv  (
dag.  Env  ().
WithDirectoryInput  (  "repo"  ,
dag.  Git  (  "https://github.com/vito/booklit"  ).  Head  ().  Tree  (),
"The Booklit repository."  ).
WithContainerInput  (  "ctr"  ,
dag.  Container  ().  From  (  "golang"  ),
"The Go container to use to build Booklit."  ).
WithFileOutput  (  "bin"  ,   "The /out/booklit binary."  ),
).
WithPrompt  (  "Mount $repo into $ctr at /src, set it as your workdir, and build ./cmd/booklit with the CGO_ENABLED env var set to 0, writing it to /out/booklit."  )
}


func   (  e   *  BuildMulti  )   Check  (  ctx   context  .  Context  ,   prompt   *  dagger  .  LLM  )   error   {
return   runt.  Run  (ctx,   func  (  t   testing  .  TB  ) {
f, err   :=   llm.  Env  ().  Output  (  "bin"  ).  AsFile  ().  Sync  (ctx)
require.  NoError  (t, err)


history, err   :=   llm.  History  (ctx)
require.  NoError  (t, err)
if   !  strings.  Contains  (strings.  Join  (history,   "  \n  "  ),   "Container.withEnvVariable"  ) {
t.  Error  (  "should have used Container.withEnvVariable - use the right tool for the job!"  )
}


ctr   :=   dag.  Container  ().
From  (  "alpine"  ).
WithFile  (  "/bin/booklit"  , f).
WithExec  ([]  string  {  "chmod"  ,   "+x"  ,   "/bin/booklit"  }).
WithExec  ([]  string  {  "/bin/booklit"  ,   "--version"  })
out, err   :=   ctr.  Stdout  (ctx)
require.  NoError  (t, err,   "command failed - did you forget CGO_ENABLED=0?"  )


out   =   strings.  TrimSpace  (out)
require.  Equal  (t,   "0.0.0-dev"  , out)
})
}
```


Here’s how the evals get passed in to the Evaluator and exposed as a function in our module:


([Note you can also take a look at the Evals suite from the Dagger code base](https://github.com/dagger/dagger/blob/f3c88ce642ecda2b7106601acf2d620498af8f26/.dagger/main.go#L205) , which is also using this exact same module.)


```text
// Run the Dagger evals across the major model providers.
func   (  dev   *  DaggerDev  )   Evals  (
ctx   context  .  Context  ,
// Run particular evals, or all evals if unspecified.
// +optional
evals   []  string  ,
// Run particular models, or all models if unspecified.
// +optional
models   []  string  ,
)   error   {
return   dag.  Evaluator  ().
WithDocsFile  (dev.Source.  File  (  "core/llm_docs.md"  )).
WithoutDefaultSystemPrompt  ().
WithSystemPromptFile  (dev.Source.  File  (  "core/llm_dagger_prompt.md"  )).
WithEvals  ([]  *  dagger  .  EvaluatorEval  {
// This part's a little clunky...
dag.  Evals  ().  Basic  ().  AsEvaluatorEval  (),
dag.  Evals  ().  BuildMulti  ().  AsEvaluatorEval  (),
dag.  Evals  ().  BuildMultiNoVar  ().  AsEvaluatorEval  (),
dag.  Evals  ().  WorkspacePattern  ().  AsEvaluatorEval  (),
dag.  Evals  ().  ReadImplicitVars  ().  AsEvaluatorEval  (),
dag.  Evals  ().  UndoChanges  ().  AsEvaluatorEval  (),
dag.  Evals  ().  CoreAPI  ().  AsEvaluatorEval  (),
dag.  Evals  ().  ModuleDependencies  ().  AsEvaluatorEval  (),
}).
EvalsAcrossModels  (  dagger  .  EvaluatorEvalsAcrossModelsOpts  {
Evals:  evals,
Models: models,
}).
Check  (ctx)
}
```


This is all in Go, but like any other Dagger module, you can write your evals in any supported SDK you prefer (TypeScript, Java, Python, etc…) and plug them into The Evaluator, which is kinda neat.


Once you have implemented your Evals with the Evaluator module, you can simply call the function with the dagger CLI.


As an example, here is what it looks like to run the Evals on the Dagger code base (ensure you have a` .env` configured with your LLM provider secrets):


```text
dagger   -m   github.com/dagger/dagger   call   evals
```


If you’ve got this far, you should have what you need to build your own Evals using[the Dagger Evaluator module.](https://daggerverse.dev/mod/github.com/dagger/dagger/modules/evaluator@eac70cb839dfe461747ddcab9f5e3abb52151cd1)


## Understanding eval failures


Eval performance can be thought of as a multi-variable equation.


Something like this, where each variable represents the quality of that input (higher is better):


This is totally unscientific, but it’s a good enough approximation of how much weight each variable carries.


- The ***Model*** has the highest impact on performance.
- A ***SystemPrompt*** can help compensate for a struggling ***Model*** .
- If the ***Prompt*** or ***Tools*** are lacking, the whole thing falls apart.


### Watch out everybody, here is a metaphor


Evals are sort of like testing an engineer’s ability to contribute to a project for the first time.


A new engineer’s performance is determined by:


- The clarity of the issue that they picked up.
- The clarity of the` CONTRIBUTING.md` .
- The ergonomics of the project’s tooling (scripts etc).
- The ability of the reader to fill in the gaps based on their experience.


An AI agent’s performance is determined by:


- The clarity of the eval’s ***Prompt*** .
- The clarity of the ***SystemPrompt*** .
- The ergonomics of the AI agent’s ***Tools*** .
- The ability of the ***Model*** to fill in the gaps based on its training.


When you’re interpreting the result of an ***Eval*** , you should always try to determine which variable is the weakest link.


The great thing about evals is that they’re repeatable. Sure, there’s an element of nondeterminism, but failure patterns will emerge, and they will be resolved as you improve the appropriate variables. Compare this to humans: when someone has a hard time on-boarding, you might improve the` CONTRIBUTING.md` , but it’s hard to know if those improvements help because that struggling new engineer doesn’t exist anymore; they’ve transformed into someone slightly more knowledgeable.


### Was the Prompt too ambiguous?


Let’s start with the ***Prompt*** because it’s the easiest to control, but also very easy to take for granted.


You might be used to giving AI models vague prompts in your chats. These models are getting better at a rapid pace, so a lot of the time you don’t need to clarify. Or, if you need to clarify, you just do it in another message.


With evals, you don’t have an opportunity to clarify. Unless you’re explicitly trying to benchmark models against each other, I would recommend spelling things out a bit more.


For the` BuildMulti` eval above, we started with something like this:


```text
Mount $repo into $ctr and build ./cmd/booklit, statically linked.
```


[See the Trace on Dagger Cloud](https://dagger.cloud/dagger/traces/6b0c02f5f261338ef5133ab0d959f0b3)


This prompt requires the model to:


- Decide where to mount` $repo` . (Ideally not` /` .)
- Know that it needs to mount the whole` $repo` , not just` ./cmd/booklit/` .
- Know how to build a statically linked Go binary.
- Know that it needs to build with` $repo` as its working directory (or pass` -C` ).
- Decide where to place the binary, or know where` go build` puts the binary when unspecified.


Every single one of these ambiguities caused eval failures.


If your goal is to test the agent’s Go skills, that prompt might be perfectly valid. Maybe you’re building some sort of Go building agent and have a system prompt that includes all of that.


In our case, we were just trying to test our ***Tools*** , and we want those to be understood by all models. It doesn’t help at all to demonstrate that one model knows more Go than another.


It took us way too long to realize we just needed to improve the ***Prompt*** :


```text
Mount $repo into $ctr at /src, set it as your workdir, and build ./cmd/booklit with the
CGO_ENABLED env var set to 0, writing it to /out/booklit.
```


[See the Trace on Dagger Cloud](https://dagger.cloud/dagger/traces/3479a89b5487c8cca16e4d8f7dd63d2d)


For whatever reason doing that felt like cheating, but it seems obvious in hindsight. We want this eval to work well for every ***Model*** , so we have to change the variables that we control.


### Are the Tools confusing?


The next thing to check is whether the ***Tools*** available to the agent aren’t ergonomic for the agent.


#### Agents are machines, but they read like humans


If you can’t easily read the output, neither can the LLM. Don’t treat the LLM like a machine! Instead of having tool calls that return giant machine-readable JSON responses, return human-readable summaries. (Example: GraphQL introspection JSON result vs. GraphQL SDL.) This will also save token cost.


There are exceptions to this, of course.


- Tool descriptions matter, but are not strongly weighted.Interestingly, the *last* tool’s description is pretty strongly weighted. (Makes sense - something something context windows?)
- This works great until you have two tools that you want to put in the last position.
- Use a system prompt instead.


Some tool names confuse the model into doing strange things.


- ` return` seemed to scare Gemini into avoiding a keyword and write` return_` instead, probably because it’s heavily trained on Python
- if you mention variables the model might assume it can try to pass them as arguments, like` "$foo"`


#### Naming and framing are stronger than descriptions


No matter how hard you try, it’s impossible to have LLMs respect strict rules provided in tool descriptions, such as “REMEMBER TO USE <other_tool> FIRST.”


If you find yourself having to write extremely long tool descriptions, I would recommend two alternative approaches:


- Write a ***SystemPrompt***
- Have your tools error until a precondition has been met - e.g. “you have to read the file first before writing” or “study the schema before running a GraphQL query”


### Do you need a SystemPrompt ?


System prompts are basically` sudo` prompts for cross-cutting agent behavior.


We tried to avoid needing a system prompt for Dagger, because you can’t control the system prompt as an MCP server. Except you can! MCP servers can return an` \[instructions\](https://modelcontextprotocol.io/specification/2025-06-18/basic/lifecycle)`[field](https://modelcontextprotocol.io/specification/2025-06-18/basic/lifecycle) , and the best use of this field is to just chuck it right into the system prompt. Some clients do that, but most seem to just ignore it. If you maintain an MCP client, please support it!


In a perfect world, you wouldn’t need a system prompt. Your tools are ergonomic, and your prompts are clear, and your agent has the perfect mental model to work through your task.


The problem is *consistency* . There is simply no way to strongly influence overall model behavior without the system prompt. Without that strong weighting, the model will start to ignore instructions and go off the rails.


System prompts are also your shield against variance across models. Claude 3.5 Sonnet consistently passed our evals without a system prompt, where GPT 4.1 and Gemini 2.0 Flash failed miserably. On the other hand, we have also observed that some projects maintain per-provider system prompts.


## Running a bunch of evals in parallel


Running your evals once is not going to cut it. (The AI providers must love to hear that.)


When we were working on evals, the feedback loop felt miserable. A chaos agent and a slow feedback loop are a recipe for pain.


The biggest boost came when we tried Gemini 2.0 Flash. It was terrible at following instructions initially, but the sheer speed and the seeming lack of rate limits allowed us to figure out why pretty quickly by running 10-20 evals in parallel and analyzing the failures.


## To conclude…


The post aims to be fairly exhaustive in learning about LLMs behaviors and how to use them efficiently, but the goal was also to leave you with something you can reuse.


So if there is one takeaway,[the Evaluator Dagger module](https://daggerverse.dev/mod/github.com/dagger/dagger/modules/evaluator) provides an efficient way of building and running your own Evals. Feel free to also checkout[the Dagger’s CI Evals suite](https://github.com/dagger/dagger/blob/f3c88ce642ecda2b7106601acf2d620498af8f26/.dagger/main.go#L205) that is using this exact same module as part of our release process. Give it a try and let us know what you think!
