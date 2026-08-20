---
schema_version: "1.0.0"
document_id: "0bb2aedf3f6204258e8a3952cbb78e64d24831595b517d18fefafe77e007f0c9"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-1135de35cf81"
canonical_url: "https://mastra.ai/articles/guardrails"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-10T16:04:57.697539+00:00"
fetched_at: "2026-08-10T16:04:59.642666+00:00"
content_hash: "sha256:f80cf114997d4dd6bcd244ae4fbd2a8bdad0b82321daf6aef21a083f00f097c5"
---

# Guardrails for AI agents: a practical guide for TypeScript developers

You shipped an AI agent that handles customer requests, and it works perfectly in your demo. Then a user feeds it a prompt injection, your agent calls a tool it should not have called, and sensitive data leaks into a response. Guardrails are the validation layers that prevent this, giving you programmatic control over what your agent accepts, processes, and returns.


According to[OWASP’s 2025 Top 10 for LLM Applications](https://www.evidentlyai.com/blog/owasp-top-10-llm) , prompt injection remains the number-one vulnerability in deployed LLM systems. As AI agents move from prototypes to production, your validation layers need to sit between user input and your LLM, between your LLM and your tools, and between your agent’s output and your end user.


This guide covers what AI guardrails are, the types you need, how to implement them in TypeScript, and how to monitor them once they are running in production.


## What are AI guardrails?


AI guardrails are programmatic checks that validate, filter, or block data at defined points in your agent’s execution. They intercept requests and responses to enforce rules that your LLM cannot reliably enforce on its own, covering everything from prompt injection detection to output schema validation.


Once you understand what these checks do and where they fit in your stack, you can start building more reliable agents.


Guardrails are engineering controls. They are code you write and deploy alongside your agent to enforce specific, testable rules at runtime. You can think of content moderation as one type of check, but these controls also cover structural concerns like schema validation, token limits, and tool-call authorization.


### Guardrails for agents vs. guardrails for models


Your model-level checks validate what goes into and comes out of a single LLM call. Agent-level controls operate higher up, wrapping entire workflows that may include multiple LLM calls, tool invocations, and handoffs between sub-agents.


The distinction matters because AI agents introduce control flow. An LLM returns text, but an agent decides what to do next. Agent-level validation needs to cover not just the content of each step but the decisions between steps, including which tools get called and what data gets passed to them.


## Why production AI needs guardrails


Your development environment and your production environment face fundamentally different threat surfaces. In development, you control the inputs. In production, you do not. Fine-tuning your LLM on clean data helps with quality, but fine-tuning alone cannot prevent adversarial attacks or enforce runtime compliance rules that change faster than you can retrain.


### Threats these controls protect against


You need validation checks to catch failures that LLM training alone cannot prevent. The most common threats fall into clear categories:


-


**Prompt injection:** user input that manipulates your agent into ignoring its instructions or executing unintended behavior


-


**Data leakage:** sensitive information like API keys, customer PII, or internal system details appearing in agent responses


-


**Harmful content:** outputs containing hate speech, profanity, or content that violates your usage policies


-


**Incorrect outputs:** confidently stated but fabricated information that your agent presents as fact


-


**Jailbreaking:** adversarial inputs designed to bypass your LLM’s built-in safety filters


### Risks unique to agentic workflows


Your AI agents introduce risks that single-model deployments do not have. When your agent can call tools, it can take real-world actions: sending emails, writing to databases, or invoking external APIs. A bad output from a standalone LLM is embarrassing. A bad tool call from an agentic AI agent can be destructive.


When your workflow spans multiple steps, errors compound. A mistake in step two becomes the input for step three, and a check at the final output may not catch a problem that originated several steps earlier. You need validation at each boundary, not just the edges.


## Types of AI guardrails


Your strategy needs to cover every point where data enters, moves through, or leaves your agent. Each layer addresses a different class of risk.


**Type** **When it runs** **What it catches** **Relative cost**


Input checks Before LLM inference Prompt injection, banned content, malformed requests Low


Output checks After LLM response Data leakage, schema violations, harmful content Low to medium


Tool checks Before and after tool execution Unauthorized tool calls, dangerous arguments, tainted return values Medium


Infrastructure checks Throughout the pipeline Rate limit breaches, token budget overruns, RAG source violations Low


*The checkpoints a trace passes through before storage — sampling, redaction, and truncation — mirror how guardrails intercept and filter data at each stage of an agent pipeline.*


### Input guardrails


Your input checks run before your agent processes a request. They validate user input against rules you define, catching prompt injections, banned content, or malformed requests before they reach the LLM. These are your first line of defense and typically the cheapest to run because they operate on raw text before any model inference.


### Output guardrails


Your output checks run after your agent produces a response but before that response reaches the user. They scan for data leakage, validate response format against a schema, and filter harmful content the LLM may have generated. This layer is your last chance to catch problems before they become visible.


### Tool guardrails


Your tool-level checks wrap individual tool invocations. They validate the arguments your agent passes to a tool before execution and inspect the tool’s return value afterward. This is critical because a compromised tool call can have side effects that no output filter can reverse, making tool-level AI agent guardrails essential for any production deployment.


### Data and infrastructure guardrails


Your infrastructure-level controls enforce constraints outside the agent’s logic: rate limiting, token budgets, maximum context window sizes, and retrieval-augmented generation (RAG) source restrictions.


Think of these as fall protection for your infrastructure and your budget. Just as fall protection keeps a worker from going over the edge on a job site, these controls keep your system from breaching cost and capacity limits.


## Deterministic guardrails


Your deterministic checks use rule-based logic that produces the same result every time. They are fast, predictable, and free of LLM inference costs.


### Rule-based filters and schema validation


You can catch a surprising number of problems with simple rules. Regex patterns detect known injection patterns. Keyword blocklists filter profanity and banned terms. Schema validation with tools like Zod ensures your agent’s structured outputs conform to expected types before downstream code consumes them.


```text
import   {   z   }   from   '  zod  '  ;
const   outputSchema   =   z.  object  ({
answer: z.  string  ().  max  (  500  ),
confidence: z.  number  ().  min  (  0  ).  max  (  1  ),
sources: z.  array  (z.  string  ().  url  ()).  max  (  5  ),
});
function   validateOutput  (raw  :   unknown  )   {
const   result   =   outputSchema.  safeParse  (raw);
if   (  !  result.success)   {
throw   new   Error  (  `  Output validation failed:   ${  result.error.message  }`  );
}
return   result.data;
}
```


### PII detection


Your PII detection logic scans for personally identifiable information like email addresses, credit card numbers, phone numbers, and API keys. You can handle detected PII in several ways: redact it, mask it, hash it for logging, or block the request entirely. The right strategy depends on your compliance requirements and whether the PII appears in input or output. Mastra provides PII detection out of the box, so you do not need to build this check from scratch.


### Tripwires and hard stops


Your tripwires are checks that halt execution immediately when triggered, rather than modifying content and continuing. When a tripwire fires, it throws an exception that stops the agent run. Use tripwires for violations that cannot be recovered from gracefully, like detected jailbreaking attempts or blocked content categories.


Tripwires differ from content filters in their intent. A content filter modifies and continues. A tripwire stops everything and forces your application to handle the failure explicitly.


## Model-based guardrails


Your model-based checks use an LLM or classifier to evaluate content with semantic understanding. They catch nuanced violations that regex and keyword lists miss.


### Using a secondary LLM as a classifier


You can run a fast, inexpensive LLM as a classifier alongside your primary agent model. The classifier receives the input or output, evaluates it against criteria you define in a prompt, and returns a structured judgment. This pattern lets you check for things like topic relevance, tone violations, or domain-specific policy compliance.


The tradeoff is cost and latency. Every model-based check adds an inference call. You can mitigate this by running the check in parallel with your agent when blocking is not required, or by using a smaller model like GPT-4.1 nano for classification.


### Built-in vs. custom model guardrails


You can choose between built-in provider filters and custom checks, and the right option depends on how much control you need. Amazon Bedrock provides configurable content filters that scan for hate speech, profanity, and violence at adjustable severity levels. Azure AI Foundry offers similar default filters with four intervention points for content risks, prompt injection detection, and PII filtering.


Your custom checks give you more control. You define the evaluation criteria, choose the classifier model, and decide the response action. Built-in filters from Amazon Bedrock or Azure are a useful baseline, but custom logic lets you enforce business-specific rules that no provider can anticipate.


**Approach** **Speed** **Semantic accuracy** **Cost per call** **Best for**


Deterministic (regex, schema, keyword lists) Sub-millisecond Low (pattern-match only) Free Known patterns, structural validation, profanity filters


Model-based (LLM classifier) 200-800ms High Per-inference pricing Nuanced policy, tone, topic relevance


Hybrid (deterministic first, then LLM) Variable High Reduced (LLM runs only on passed inputs) Production validation pipelines


## Mastra’s agent framework and observability layer


Your validation checks are only as useful as your ability to see them working.[Mastra](https://mastra.ai/ai-agent-framework) is an open-source TypeScript framework for AI agents that gives you agents, workflows, and observability in one integrated package, so you do not need to stitch together separate tools for monitoring and debugging.


You define validation logic alongside your agent configuration as TypeScript functions that scan for injection patterns, PII exposure, or content policy violations. When a check fails, your agent can halt execution or return a sanitized fallback response, with every decision logged as a traceable span.


You get visibility into which checks fired, what they detected, how long they took, and whether they blocked or passed the request.


*Observability and eval dashboard showing trace data, pass/fail results, and validation metrics across agent runs.*


Mastra supports **90+** model providers through one interface and is open source under **Apache 2.0** with no seats or usage tiers.[Build your first guarded TypeScript agent on Mastra.](https://mastra.ai/ai-agent-framework)


## Workflow boundaries and execution timing


Your checks need to run at the right point in your agent’s lifecycle. Running them too early means you lack context. Running them too late means damage is already done.


*Monitoring feedback loop showing the iterative cycle where validation rules are tested, tuned, and updated based on production data.*


### Before-agent checks


Your before-agent checks run once when a request enters the system, before any LLM inference or tool execution begins. They are ideal for authentication, rate limiting, input content filtering, and injection detection. If a before-agent check fails, your agent never runs, saving tokens and preventing side effects.


### After-agent checks


Your after-agent checks run on the final output before it reaches the user. They validate that the response meets your quality, safety, and compliance requirements. These always run sequentially because they need the complete output to evaluate.


### Combining multiple checks in a pipeline


You can stack checks in a pipeline where each one runs in sequence. A typical pipeline layers deterministic rules first (cheap and fast) and model-based rules second (expensive but thorough). If an early check fails, later ones never run.


This layered approach gives you both performance and coverage. Your regex filter catches obvious injections in microseconds. Your LLM classifier catches the subtle ones that slip through, but only for the inputs that passed the first layer.


## How to implement guardrails in a TypeScript agent


Your TypeScript stack gives you strong typing and schema validation tools that make implementing these checks more natural than in dynamically typed languages.


### Defining input validation logic


You start by defining validation functions that run before your agent processes a request. Each function receives the user input, performs its check, and either passes the input through or throws an error.


```text
type   GuardrailResult   =   {   passed  :   true   }   |   {   passed  :   false  ;   reason  :   string   };
function   checkPromptInjection  (input  :   string  )  :   GuardrailResult   {
const   patterns   =   [
/  ignore\s  +  (  all\s  +  )  ?  previous\s  +  instructions  /  i  ,
/  you\s  +  are\s  +  now\s  +  a  /  i  ,
/  system\s  *  :\s  *  /  i  ,
];
for   (  const   pattern   of   patterns)   {
if   (pattern.  test  (input))   {
return   {   passed:   false  ,   reason:   '  Prompt injection detected  '   };
}
}
return   {   passed:   true   };
}
```


### Wrapping tool calls with security validation


You can wrap your tool definitions with validation logic that inspects arguments before execution and return values after. This pattern ensures that every tool invocation passes through your AI security guardrails, regardless of which agent or workflow triggers it.


```text
function   withToolGuardrails  <  TArgs  ,   TResult  >(
tool  :   (args  :   TArgs  )   =>   Promise  <  TResult  >,
inputCheck  :   (args  :   TArgs  )   =>   GuardrailResult  ,
outputCheck  :   (result  :   TResult  )   =>   GuardrailResult  ,
)   {
return   async   (args  :   TArgs  )  :   Promise  <  TResult  >   =>   {
const   inputResult   =   inputCheck  (args);
if   (  !  inputResult.passed)   {
throw   new   Error  (  `  Tool input blocked:   ${  inputResult.reason  }`  );
}
const   result   =   await   tool  (args);
const   outputResult   =   outputCheck  (result);
if   (  !  outputResult.passed)   {
throw   new   Error  (  `  Tool output blocked:   ${  outputResult.reason  }`  );
}
return   result;
};
}
```


### Human-in-the-loop as a validation mechanism


You can use human approval as a control for high-stakes tool calls. Instead of automatically executing a tool, your agent pauses, presents the proposed action for review, and resumes only after explicit approval. This is one of the most reliable safeguards for operations like sending emails, modifying production data, or initiating financial transactions.


### Inheritance and override patterns


You can define validation rules at the system level and let individual AI agents inherit them automatically. When an agent needs different rules, it overrides specific checks without losing the baseline protections. This pattern keeps your configuration DRY and ensures that new agents ship with sensible defaults.


## Monitoring and debugging guardrails in production


Your checks will behave differently in production than in your test suite. Real users produce inputs you did not anticipate, and edge cases surface at scale. Responsible AI practices require ongoing monitoring, not just initial setup.


### Tracing decisions in agent runs


You need every validation decision recorded as a traceable event with inputs, outputs, duration, and pass/fail status. Without this data, you cannot distinguish between a check that is working correctly and one that is silently passing harmful content. Structured traces let you query behavior across thousands of agent runs and catch regressions before users report them.


*Agent trace hierarchy showing how validation spans nest within LLM calls and tool invocations, giving you full visibility into each decision point.*


### Common failure modes and troubleshooting


Your checks will encounter predictable failure modes that you should monitor for from day one:


-


**False positives:** overly aggressive content filters blocking legitimate user requests


-


**Slowdowns:** model-based checks running sequentially instead of in parallel


-


**Silent pass-throughs:** a check evaluating the wrong field or receiving malformed input


-


**Stale rules:** patterns that no longer match evolving jailbreaking techniques or updated compliance requirements


### Content flagged unexpectedly and tool calls not being scanned


Your users will report false positives where legitimate content gets blocked. When this happens, review the severity threshold settings on your content filters. A threshold set too aggressively catches edge-case patterns that your classification model interprets as violations.


If harmful content passes through tool calls undetected, verify that your checks explicitly cover tool call and tool response intervention points. Agent-level input and output validation does not automatically scan what happens inside tool invocations. You need dedicated tool-level checks for that coverage.


## Guardrail frameworks and tools


Your choice of framework depends on your language, your deployment model, and how much customization you need. Responsible AI requirements also influence whether you need a managed platform with built-in compliance reporting or a self-hosted framework you control entirely.


### Open-source options


You have several mature open-source options, though most are Python-first:


-


**NeMo Guardrails:** NVIDIA’s toolkit for adding programmable safety controls to LLM applications. It uses a Colang scripting language to define conversational rails and supports topical, safety, and security controls. LangChain integrations let you connect NeMo to existing LangChain pipelines.


-


**Guardrails AI:** a Python framework with a community-driven Guardrails Hub of pre-built validators covering structured output, PII detection, toxicity, and more. The Hub makes it easy to compose multiple validators into a single pipeline.


-


**LangChain:** offers validation through its middleware system, supporting both deterministic checks (keyword blocking, PII detection) and model-based safety classifiers. LangChain’s modular architecture means you can add checks at any point in your chain, and its large community provides pre-built components.


### Managed and cloud-native options


If you prefer managed infrastructure, you have strong options from major cloud providers.


-


**Amazon Bedrock:** a fully managed service with configurable content filters, denied topic detection, PII redaction, and contextual grounding checks that validate responses against your source documents. Its ApplyGuardrail API runs validation independently of model inference, so you can use Bedrock as a standalone validation layer in front of any LLM, not just models hosted on its platform. Configurations are versioned and promoted through staging environments.


-


**Azure AI Foundry:** provides similar managed controls, with configurable severity levels for content risks and injection detection.


-


**OpenAI's Agents SDK:** offers input, output, and tool validation with a tripwire mechanism that halts execution when violations are detected.


### Choosing between frameworks for a TypeScript stack


Your framework choice narrows quickly if you are building in TypeScript. Most open-source libraries, including NeMo and Guardrails AI, are Python-only. LangChain’s middleware is also Python-first.


Amazon Bedrock can be called from TypeScript through the AWS SDK, but the logic itself runs on AWS infrastructure rather than in your codebase.


If you want checks that integrate natively with your TypeScript agent code, look for frameworks built in the same language.[Mastra](https://mastra.ai/ai-agent-framework) provides agents, workflows, and observability in TypeScript, eliminating the need to maintain a separate Python service for validation work.


## Common challenges when implementing guardrails


Your implementation will face practical tradeoffs that require ongoing tuning. Knowing them upfront helps you make better architectural decisions.


### Performance trade-offs


Your checks add time to every request. Deterministic rules add microseconds, but model-based classifiers add hundreds of milliseconds of latency per LLM inference call. You can reduce the impact by running checks in parallel with agent execution when blocking is not required, and by using smaller classifier models.


The key question is whether a check must be completed before your agent starts. If it must, accept the added latency. If it can run alongside the agent, run it in parallel and cancel the agent if the check trips.


### Avoiding over-restriction and false positives


Your rules will block legitimate requests if you set thresholds too aggressively. This is especially common with model-based classifiers that flag edge-case language patterns. Monitor your false-positive rate as a core metric and adjust severity thresholds based on real production data, not assumptions.


Responsible AI teams should review flagged content regularly to calibrate between safety and usability.


### Ownership across engineering, security, and compliance teams


Your validation controls span multiple organizational concerns. Engineering owns the implementation. Security owns the threat model. Compliance owns the policy requirements and responsible AI reporting. Without clear ownership, rules become stale or inconsistent. Define who updates them, who reviews false positives, and who approves changes to severity thresholds.


## Wrapping up


Your guardrails are not a one-time setup. They are a living system that needs monitoring, tuning, and iteration as your AI agents evolve and your threat surface changes. Start with deterministic checks for known patterns, layer in model-based classifiers for nuanced detection, and trace every decision so you can debug failures and catch regressions.


If you are building in TypeScript, Mastra gives you agents, workflows, and observability in one framework so you can ship guarded agents from day one.
