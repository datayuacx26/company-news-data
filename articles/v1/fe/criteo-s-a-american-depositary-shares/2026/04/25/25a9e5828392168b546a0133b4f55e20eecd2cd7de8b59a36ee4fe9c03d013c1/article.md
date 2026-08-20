---
schema_version: "1.0.0"
document_id: "25a9e5828392168b546a0133b4f55e20eecd2cd7de8b59a36ee4fe9c03d013c1"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/beyond-the-demo-why-agentic-evaluation-matters-ccbeb9841c8b"
published_at: "2026-04-16T09:02:25+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:561ae98cafc9c594eba5480bb69e3acd9c9b5608fbec48b6c97ed150fabd2055"
---

# Beyond the demo: Why agentic evaluation matters

# Beyond the demo: Why agentic evaluation matters


[Criteo Tech](https://medium.com/@criteotech?source=post_page---byline--ccbeb9841c8b---------------------------------------)


9 min read


·


Apr 16, 2026


--


Author:[Fabian Höring](https://www.linkedin.com/in/fhoering/)


Press enter or click to view image in full size


Agentic systems powered by LLMs can be incredibly impressive in demos. With a few well-crafted prompts, they can demonstrate reasoning, calling tools, and solving complex tasks \[1\]. Demos are effective at showcasing what’s possible. Production environments, however, are where those capabilities are tested at scale and under real-world conditions.


The same agent that performs perfectly on curated examples can behave unpredictably when exposed to real users. Inputs vary widely, conversations evolve over multiple turns, and small prompt changes can lead to unexpected regressions. Even with temperature set to zero \[2\], the results are not fully deterministic \[3\].


At Criteo, as we integrate agentic capabilities into production systems, such as Agentic Audiences \[4\] and our Agentic Commerce Recommendation Service \[5\], where LLMs are, for example, involved in transforming user queries into structured queries to the recommendation service. We quickly realized that evaluation is the central challenge, not model capability.


If you are building, deploying, or maintaining LLM-powered agents, this post will help you understand how to evaluate them reliably in real-world conditions. In particular, we present:


- the challenges we faced,
- the infrastructure we built,
- and the key lessons we learned along the way.


## Agentic evaluation is hard


Evaluating agentic systems is fundamentally different from traditional software testing.


### A vast and unpredictable input space


One issue with agentic systems is that the possible user input space is large, and it gets even larger with multi-turn systems or agent-to-agent interactions. When developing a new agent, one generally starts with a few hand-selected prompts and can quickly obtain correct outputs for basic examples.


These prompts are often proposed by experts but do not necessarily capture the diversity of inputs that real users would ask. One can use LLMs themselves to enrich the handcrafted examples and create synthetic datasets \[6\], but real inputs come from real users.


### Iterate without regressions


Implementing agentic systems is a continuous improvement process by adding or experimenting with new features (see \[7\] for an example of a concrete iteration cycle). We need to make sure we can add them without breaking or degrading existing behavior. This is even more important as we know that prompt changes (also called Prompt Drift \[8\]) can affect an agent’s behavior toward unrelated sections or demonstrate cascading behaviors.


### Keep the agent safe and secure


Agentic systems should not expose prohibited or restricted content. We need to add safety guardrails to ensure the model replies with appropriate wording (i.e., not breaching our internal compliance and ethics rules) and within its scope of competence.


## The evaluation pipeline


Evaluation is the key to addressing the challenges above. It should be easy, fast, and integrate with the existing tech stack. Unlike classical software engineering testing, agentic evaluation is more of an iterative process that should be triggered as soon as possible in development, and focused on trying out as many ideas as possible. While many will fail, a few might work.


We started building our own small isolated evaluation pipelines, but as Criteo has many engineers working on agentic systems, sharing a common evaluation infrastructure was needed to share best practices, methodologies, and common metrics.


Instead of building our own home-built solution, we chose to integrate with an existing LLM engineering platform. There are many solutions out there, and you can choose whatever works best for you. At Criteo, we are using Langfuse \[9\] because the project is fully open-source and currently fits our needs.


Instead of static testing, we adopted an iterative evaluation loop (Figure 1):


- Build test cases
- Run offline evaluations
- Analyze results
- Improve the agent
- Validate with real user traces


This cycle continues when new tests run offline and as agents interact with real users.


Press enter or click to view image in full size


## Tooling and Infrastructure


The ability to iterate fast is only possible with solid tooling and reliable infrastructure.


### LLM engineering platform


Online traces are emitted from live applications and collected in Langfuse. Evaluations are triggered via the SDK and run either locally or on remote Jenkins instances. We heavily leverage datasets to share common scenarios across different users and visualize the results directly in the UI. Prompts for LLM-as-judge are directly saved and versioned in Langfuse’s prompt section.


Criteo mainly uses two development stacks: C# for online services like our Real-Time Bidding stack, and Python for offline jobs like our deep learning model training pipelines. We want to embed agents everywhere if needed, and therefore also need support for both languages. Whereas Langfuse has good support for all kinds of Python integrations, C# support is limited for now.


For Python, we use the default SDK integration with mainly Pydantic AI and Langchain \[10\]. We are developing our own OpenTelemetry-based C# SDK for tracing \[11\] with Microsoft’s Agent Framework \[12\] and a C# client based on the public API swagger specification \[13\]. We will eventually open-source those C# SDKs, as it could be useful for other users.


### An internal evaluation SDK


For evaluation, we noticed that we were rebuilding the same metrics for common use cases across different projects. For instance, for exact metrics, we often wanted to compare two JSON fields with different declinations (ignore cases, exclude fields, compare to a subpart on one side, compare by regular expression). We started building an internal evaluation SDK that allows us to trigger the agent and execute custom metrics. The API is very close to` run_experiment` \[14\], which accepts tasks and evaluators. In our case, each client defines a task to process the input, runs the agent, and computes the output. One can use prebuilt evaluators and provide the configuration with metadata in addition to the dataset, or provide an implementation for custom evaluators.


Prebuilt evaluators are useful to share common best practices across teams and configurability via the Langfuse UI. Figure 2 shows an example of tool correctness, comparing the expected output against` $.tool_calls\[-1\].input_parameters` output field. Figure 3 shows an example run with the computed score.


Press enter or click to view image in full size


Figure 2 — An example metric setup Press enter or click to view image in full size


Figure 3 — Example run for analytics agent


## Some real-world lessons from our experiments


### Simple metrics first


We started with simple metrics first to evaluate functional correctness:


- Agentic tool correctness (function or MCP \[15\]) to check if the actual called tools fit a list of expected tools
- Output status to capture basic application behaviors:
- No data vs. service success, as it often happened that no data was returned
- Checking safety and compliance, and rejecting otherwise
- Efficiency metrics like latency, token usage, and cost per task


We then noticed that multi-turn conversations are important to represent tool interactions as realistically as possible, making computation of deterministic metrics harder. LLM as a judge \[16\] has been used only where strictly necessary and if the previous method failed. It is a good use case to assess open questions like judging prompts for ethics/compliance, or checking the answer quality based on some criteria.


### Stable evaluation scope


Evaluation metrics can fluctuate significantly due to underlying LLM non-determinism. For example, consider an analytics agent that converts user input into structured queries. A user would need to specify a time range like ‘give me clicks and cost for my campaigns for last month’, with the user query being done on 15 of September, the agent was choosing:


- sometimes the full calendar month, from 1 August to 31 August
- sometimes the floating month, from 15 August to 14 September
- sometimes, plus or minus 1 day, from 15 August to 15 September


We spent a lot of time trying to stabilize the computed scores so they are consistent across runs and therefore comparable. This work included refining the agent by making instructions more explicit, simplifying the metrics by removing unnecessary fields, and shifting our success criteria. Instead of targeting 100% metric success, we prioritized stability, aiming for a consistent ~88% success rate over volatile results that swing from 100% one day to 50% the next. It is particularly hard to guarantee this over time because of external changes to involved components like the model, the APIs or the testing environment.


### Datasets


Building high-quality datasets is challenging. They are hard to obtain due to limited existing data, hard to trust because they are often created manually or generated using inherently biased LLM methods, and complex to analyze. At the same time, datasets evolve as frequently as the agentic code and evaluation metrics themselves, making consistency difficult to maintain. The same example as above, where metrics are assessed for dates in September, might no longer be relevant when the dataset is executed six months later against the latest agent and underlying APIs.


Rather than relying on LLM-as-a-judge metrics to mask inconsistencies, teams should prioritize rigor in maintaining dataset quality and diversity. Datasets should also be easy to update and manage, with changes handled programmatically, for example, through scripts that remove attributes or revise expected outputs.


### Handling API and agent state


Interacting with a multi-turn agent executing different tool calls is similar to testing an external REST API. For mutable calls, you need to make sure that your application is in the right state before being able to execute an endpoint. For example, to delete a user, one must ensure it has been created before. Classical unit testing resolves this problem with setup/tear down methods.


In our case, ‘setup’ means to put the agent in the right state before executing a scenario. We mostly store the serialized agent state in the dataset item input field. For example, we retrieve the full LLM conversation history for a recorded multi-turn testing scenario \[17\].


After executing the evaluation scenario, we leverage a tear down method to clean up mutable state. For example, if the agent had created a user before, we manually set up a script to delete the same user again. Even this simple action can be tricky because to delete a user, you need to know the recently created user ID and potential credential tokens to execute the underlying API endpoint. For simplicity, and to not complexify metadata fields, we are just saving raw agent instructions to clean up and then executing another turn, instructing the agent to explicitly clean up the created user.


### Include MCP testing


One additional use case that came up during our testing was evaluating MCP services \[18\]. We often wondered whether we really needed a specialized agent with a custom agent harness \[19\], or if it would be enough to simply expose our tools through an MCP layer and leverage an LLM to test it. It turns out that this problem is similar to testing websites against different browsers. One can set up the real MCP services and then simulate user interactions with different LLMs against those services. This involves creating a dataset of prompts and user scenarios, calling an LLM with the configured MCP, collecting the responses, and then computing metrics. It also allows us to see in what form popular LLMs consume MCP services differently.


Evaluation is key to the success of agentic projects. A focus on measurement and iteration means gaining insights into how the agent behaves for real users and how it provides value to our clients and us.


Evaluation best practices should include:


- Defining proper metrics, but not overengineering
- Ensuring that the evaluation scores are stable and don’t vary too much across different runs
- Making it easy to evolve the datasets, as they might change as often as the agentic code and metrics
- Having a stable test environment to be able to set up and tear down the agent state and get reproducible results
- Having tool calls exposed as MCP as part of the evaluation strategy


Having robust evaluation processes, tooling, and infrastructure in place is essential for the future, where production traces will become the primary datasets used to improve performance, enable model fine-tuning, and support the deployment of more customized, cost-efficient models.


## References


\[1\][What is a ReAct Agent? | IBM](https://www.ibm.com/think/topics/react-agent)


\[2\][What is LLM Temperature? | IBM](https://www.ibm.com/think/topics/llm-temperature)


\[3\][Defeating Nondeterminism in LLM Inference](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/) ​


\[4\][Introducing Agentic Audiences | Criteo](https://www.criteo.com/blog/introducing-agentic-audiences/)


\[5\][Criteo Introduces Agentic Commerce Recommendation Service to Power AI Shopping Assistants | Criteo](https://www.criteo.com/news/press-releases/2026/02/criteo-introduces-agentic-commerce-recommendation-service-to-power-ai-shopping-assistants/)


\[6\][Synthetic data generation (Part 1)](https://cookbook.openai.com/examples/sdg1)


\[7\][Thariq on Twitter / X](https://x.com/trq212/status/2027463795355095314)


\[8\][Prompt Drift: The Hidden Failure Mode Undermining Agentic Systems](https://www.comet.com/site/blog/prompt-drift/)


\[9\][Langfuse](https://langfuse.com/)


\[10\][Overview — Langfuse](https://langfuse.com/integrations)


\[11\][OpenTelemetry (OTEL) for LLM Observability — Langfuse](https://langfuse.com/integrations/native/opentelemetry)


\[12\][Microsoft Agent Framework Overview](https://learn.microsoft.com/en-us/agent-framework/overview/?pivots=programming-language-csharp)


\[13\][Public API — Langfuse](https://langfuse.com/docs/api-and-data-platform/features/public-api)


\[14\][Experiments via SDK — Langfuse](https://langfuse.com/docs/evaluation/experiments/experiments-via-sdk#usage-with-langfuse-datasets)


\[15\][Function calling | OpenAI API](https://developers.openai.com/api/docs/guides/function-calling)


\[16\][Using LLM-as-a-Judge For Evaluation: A Complete Guide –](https://hamel.dev/blog/posts/llm-judge/index.html)


\[17\][Multi-Turn Evaluation | DeepEval by Confident AI — The LLM Evaluation Framework](https://deepeval.com/guides/guides-multi-turn-evaluation)


\[18\][What is the Model Context Protocol (MCP)? — Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro) ​


\[19\][The Anatomy of an Agent Harness](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)
