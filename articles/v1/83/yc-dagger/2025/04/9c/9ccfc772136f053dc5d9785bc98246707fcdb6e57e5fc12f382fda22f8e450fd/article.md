---
schema_version: "1.0.0"
document_id: "9ccfc772136f053dc5d9785bc98246707fcdb6e57e5fc12f382fda22f8e450fd"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/llm/"
published_at: "2025-04-23T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:a4d30d2003e09972fd3415f80010ec5e8fa03fa6479ff13e89b765f5d288ac73"
---

# Agents in your Software Factory: Introducing the LLM Primitive in Dagger

AI-generated code is blasting through your software delivery workflows like water from a firehose. And those CI/CD systems you’ve carefully built, held together by complex YAML and brittle scripts, are starting to crack under the pressure. Right about now you might be wondering how *you* could use AI to not just keep pace but embrace the firehose and keep shipping great software. We’ve been wondering the same…


**Today, we’re extending the Dagger engine with native Large Language Models (LLM) and tool use.** This means you can build AI-powered automation — or agents, if you prefer — directly into your software delivery workflows. Think AI-driven tests, intelligent deployment gates, or agents fixing code while you sleep.


(Want to jump right in?[Go straight to the quickstart.](https://docs.dagger.io/quickstart/agent/) )


## LLM: The new software primitive


In Dagger, fundamental tools like containers or Git repositories aren’t just external things you call; they are programmable building blocks you define, combine, and use directly in your code or command line. We see LLMs as the next essential building block for modern software. So we made LLM a native component inside Dagger. That means you can now make LLM an active participant in your workflow, capable of interacting with and even *producing* other building blocks.


You configure your model (OpenAI, Anthropic, Google, Ollama etc), chain operations like` .WithPrompt(...)` to give it tasks (like analyzing code or generating documentation), and connect its inputs/outputs within your existing Dagger workflows.


```text
// Example: Basic chaining with LLM object in Go
// Start with the LLM primitive
analysisResult, err := dag.LLM().


// Provide its controlled environment & tools
WithEnv(agentEnv).


// Give it instructions
WithPrompt("Analyze the code...").


// Kick off the agent loop
Loop().


// Get the resulting environment
Env().


// Extract the output
Output("analysis_result").AsString(ctx)
```


To give your` LLM` interaction its specific world — the data, tools, and guardrails it needs — you use` .WithEnv(...)` . This attaches an environment definition (` Env` object) created in your code. You specify exactly what inputs it gets (e.g., a source code` Directory` , a base` Container` for building software, API secrets) and what outputs to expect (like a generated report` File` or a modified` Container` ). Any Dagger objects added to this environment automatically expose their functions as tools the LLM can use; give it a` Container` , and it gets scoped to running commands *only* inside that container. This code-defined sandbox gives you precise control over the AI’s capabilities.


Building these AI capabilities often means tuning prompts and seeing how the LLM actually uses the tools. The Dagger CLI &[Shell](https://dagger.io/blog/a-shell-for-the-container-age-introducing-dagger-shell) streamline this significantly. Prompt Mode (just type` >` ) lets you chat directly with the LLM *inside* the` Env` sandbox you’ve defined.


As you interact, you get real-time visibility. See the prompts, replies, the exact Dagger functions (tools) the agent calls, arguments used, and resulting state changes in the environment. This built-in observability makes debugging agent logic much easier than piecing together scattered logs.


When you use the` LLM` object into your Dagger workflow, its interactions and any tool-use it triggers execute on the Dagger Runtime. There, everything is containerized for isolation and predictability. Dagger calls the LLM, letting it use the provided tools (Dagger Functions) to work towards the goal set in your prompt. This managed execution enables the LLM to perform complex tasks that might require multiple tool interactions or steps — the agent loop. Imagine tasks like analyzing dependencies or generating test cases based on code changes.


These operations also inherit the benefits of the Dagger engine: You get fast, efficient execution thanks to parallelism and automatic caching; your AI automation runs consistently everywhere and is portable to any OCI-compatible environment; and critically, they become composable parts you can reuse and share.


Basically, Dagger lets you treat LLMs as programmable primitives that could be chained with other software primitives to build powerful, autonomous, and containerized workflows. You build AI capabilities *as code* , iterate quickly using interactive tools with clear observability, and run them anywhere.


## See it in action


Let’s see how this looks in practice by building an AI code review agent directly into a Dagger workflow:


Or the same with Go:


```text
package   main


import   (
"  context  "
"  dagger/code-analyzer/internal/dagger  "
)


type   CodeAnalyzer   struct  {}


// Analyzes code quality using an LLM within a defined environment.
// Example usage: dagger -c 'analyze https://github.com/dagger/hello-dagger'
func   (  m   *  CodeAnalyzer  )   Analyze  (  ctx   context  .  Context  ,   src   *  dagger  .  Directory  ) (  string  ,   error  ) {
// 1. Configure agent environment
agentEnv   :=   dag.  Env  ().
WithDirectoryInput  (
"code_dir"  ,
src,
"Source code to review"  ,
).
WithStringOutput  (
"analysis_result"  ,
"Code review analysis report"  ,
)


// 2. Configure agent LLM
agent   :=   dag.  LLM  ().
WithPrompt  (  "You're a code reviewer. Focus on clarity, potential bugs, and best practices."  ).
WithEnv  (agentEnv)


// 3. Execute agent loop and retrieve modified environment
resultEnv   :=   agent.
Loop  ().
Env  ()


// 4. Return the string result from the "analysis_result" output variable.
return   resultEnv.
Output  (  "analysis_result"  ).
AsString  (ctx)
}
```


Both examples run an AI code review agent. First, we define the agent’s environment (` Env` ), giving it the source code` Directory` as input and specifying a string` analysis_result` as output. This acts as a controlled sandbox.


Then, we configure the` LLM` object, providing the environment and the prompt instructing it to review the code.


Finally, we execute the agent. The` loop()` command (in both Shell and Go) runs the core agent loop, allowing the LLM to use available tools (like reading files from the input` Directory` ) before producing the final environment containing the review. We then extract the` analysis_result` string.


The Go version packages this same logic into a[reusable Dagger Function](https://docs.dagger.io/api/custom-functions/) . Thanks to Dagger’s cross-language API and type safety, this agent capability is now callable from any Dagger workflow, whether it’s written in Go, Python, TypeScript, or another supported language.


## Get Started


The LLM primitive is available starting in Dagger v0.18, and is currently marked as experimental. We welcome feedback and we’re excited to see what you build with it!


- Install with****` **brew install dagger/tap/dagger**` or[see docs for installation](https://docs.dagger.io/install)
- [Try the Agent Quickstart](https://docs.dagger.io/quickstart/agent/)
- [See more examples in the docs](https://docs.dagger.io/examples/)
- [Read the docs for the LLM functionality](https://docs.dagger.io/features/llm/)
- [Join the Discord](https://discord.com/invite/dagger-io) to ask questions and share your feedback
- [Star Dagger on GitHub](https://github.com/dagger/dagger)
