---
schema_version: "1.0.0"
document_id: "a711c62fa112de2e086a4fbf44ca3e245cdb570a18fa905558f9dd67463df9fe"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/langgraph-agent-monitoring/"
published_at: "2026-05-29T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:ae8117440a88b8069ff4300336f376883662ed91f05ce0a739ec5e9bd55745e0"
---

# Monitoring LangGraph agents with Datadog: a practical guide

Thomas Sobolik


Senior Technical Content Writer


Takaaki Tsunoda


Senior Sales Engineer


Masaki Tsukada


Senior AI Platform Engineer


Minoru Onda


Technology Evangelist


AI agents tend to function as black boxes, and it can be difficult to trace and understand agent workflows end-to-end in order to characterize performance. Particularly, you need visibility into the following:


-


Agent steps leading to LLM calls, including input prompts and responses


-


Which tools the agent invoked and how they executed


-


Context injections and data transformations across the full workflow


-


Request latency and token consumption to understand performance and cost


By tracing full agent runs with[Agent Observability](https://www.datadoghq.com/product/ai/llm-observability/) , Datadog[AI Agent Monitoring](https://docs.datadoghq.com/llm_observability/monitoring/agent_monitoring) enables you to visualize workflows with flame graphs and quickly spot sources of failures and latency. Agent Observability also performs automated[LLM-as-a-judge](https://www.datadoghq.com/blog/custom-llm-evaluations/) evaluations to help you characterize response quality and improves agent observability across your organization by connecting this telemetry data to dashboards, alerts, APM, and more.


In this post, we’ll explore how you can use AI Agent Monitoring to instrument an[agent application](https://github.com/taka2noda/Datadog_LLMObs_LangGraph_Sample) built with[LangGraph](https://docs.langchain.com/oss/python/langgraph/overview) and monitor this application’s performance and reliability. We’ve adapted this agent from a sample LangGraph agent found in ​​[Build & Run AI Agents](https://www.sbcr.jp/product/4815636609/) , an agent handbook originally published in Japanese by the open source engineers[Minorun](https://x.com/minorun365) and[Tsukada](https://x.com/ma_tsukada?s=21) . First, let’s take a look at this sample application.


## Our sample LangGraph agent


The sample LangGraph agent we’ll use for this post invokes a few tools to research user questions, summarize findings, and deliver the results. The agent integrates with[Tavily](https://www.tavily.com/) for web search and[Amazon SNS](https://aws.amazon.com/sns/) for routing the output. The source code for this agent is available[on GitHub](https://github.com/taka2noda/Datadog_LLMObs_LangGraph_Sample) .


The agent’s state consists of three nodes that interact using a[ReAct](https://arxiv.org/abs/2210.03629) -style agent loop:


-


` agent` : Claude Sonnet 4.6 (via Amazon Bedrock) takes the user’s message history and system prompt, and decides whether to call a tool or produce a final output.


-


` route_node` : This node checks the last message. If the message contains` tool_calls` , the node routes it to` tools` ; otherwise, it ends the loop.


-


` tools` : This node contains two tools—` TavilySearch` (executes a web search, returns the top 2 results) and` send_aws_sns` (publishes output text to an SNS topic). The node invokes the requested tool (Tavily search or SNS publish) and appends the tool output to the agent state.


In a typical run, the agent will receive a question from the user, compose it into the system prompt, call` TavilySearch` , evaluate the output, run another search if needed, and if not, call` send_aws_sns` and exit. The following diagram shows how the three nodes are wired together:


Now that we know how the agent works, let’s configure AI Agent Monitoring to instrument it.


## Configure AI Agent Monitoring for a LangGraph agent


To enable AI Agent Monitoring for our Agent Observability-instrumented agent, we’ll first need to configure a few settings in the application code and` .env` file. First, we’ll add the following definitions to the application code:


```text
1   from   ddtrace.llmobs   import   LLMObs    2
3   LLMObs.enable(    4
5         ml_app  =os.getenv(  "ML_APP_NAME"  ),    6
7         api_key  =os.getenv(  "DD_API_KEY"  ),    8
9         agentless_enabled  =  True  ,    10
11   )
```


Next, we’ll add the following variables to our` .env` file:


```text
1   # For Datadog LLMObs    2   ML_APP_NAME=<app-name>    3   DD_API_KEY=<Datadog-API-Key>
```


The configuration above sends AI agent data to Datadog for visualization in Agent Observability.


We’ve taken the approach of appending variables to the` .env` file in the interest of security, but it is also possible to write everything directly in the Python file. You can also enable AI Agent Monitoring simply through the startup command, without touching the application code at all. For more information on configuration options, see the[Agent Observability documentation](https://docs.datadoghq.com/llm_observability/instrumentation/sdk/?tab=python#command-line-setup) .


## Trace LangGraph tool calls and LLM latency


Now that we’ve configured Agent Observability to send traces from our agent to AI Agent Monitoring, we can use Datadog to monitor tool invocation status, processing time per step, token usage and cost, inputs and outputs, and more. No longer a black box, our agent can be more easily debugged and analyzed with this additional visibility into the runtime. For instance, the following screenshot shows a trace of a Tavily search tool call executed by our agent. We can see the input query and output results and use this context to understand what was passed to downstream LLM calls and how that input was formulated. We can also see the processing time of this request took 1.15 seconds out of the 29.7-second processing time of the full agent execution.


For instance, let’s say the agent is asked to answer a question about a recent news event, but the final response includes outdated information. By examining the trace, we can see that the Tavily tool returned older articles or only partially relevant search results, and that those results were then forwarded to a downstream LLM call without additional filtering. With this context, we can trace the bad answer back to the search step rather than treating it as an isolated model error. Tracing the failure to the search step makes it easier to decide on the right fix, whether that means improving the search query, adjusting ranking and filtering logic, or changing the prompt used to guide the downstream LLM.


## Analyze latency, cost, and errors across agent runs


Sometimes, our monitoring questions will be broader than what trace-level investigations can tell us. To understand agent performance for a given release or region, determine total cost impacts, and spot recurring errors, we’ll want to use aggregated metrics over our agent traces. Agent Observability’s overview page provides breakdowns of errors, agent evaluation scores, latency, security, token consumption, and more, to help answer these broader questions. Taking in these insights at a first glance before drilling into individual agent runs can enable more efficient analysis and troubleshooting.


For instance, let’s say our agent is falling short of its response time service level agreements (SLAs), and we need to find and address latency bottlenecks. The Latency tab in the Agent Observability overview surfaces the slowest spans, so we can discern from a quick glance if there are specific tool calls, LLM calls, or other agent steps that are creating a significant speed bottleneck.


In the following screenshot, we can see that agent spans are roughly twice as slow as tool spans. The data shows that the most significant latency gains come from reducing LLM call latency. We might accomplish this by switching to a faster model, shortening the input, or tweaking parameters like[Claude’s “reasoning effort”](https://code.claude.com/docs/en/model-config) .


Cost is another key metric that should be tracked across all requests in a given time frame. By monitoring total cost and how it changes over time, we can quickly confirm that our application is within budget or makes the cost efficiency gains that our organization’s FinOps team is asking for.


In addition to total costs and cost changes over a given time frame, the Agent Observability Overview page surfaces the most expensive LLM calls to help us spot outliers. This can elucidate situations where the average cost per agent run is relatively low, but certain requests are triggering anti-patterns like excessive retries or conditional context bloat that can lead to excessive costs. We’ll want to dive into traces for these requests in order to kick off troubleshooting.


## Evaluate agent output quality with LLM-as-a-judge


Datadog also offers automated online LLM-as-a-judge evaluations that can help characterize output quality, spot jailbreak attempts, and flag cases where your agent breached its guardrails. There are only two settings to configure: first, the API key for the evaluation model, and then the selection of evaluations you want to run. For instance, you can monitor an agent’s decision accuracy with the “Tool Selection” and “Tool Argument Correctness” evaluators, or monitor chat experiences with “Input Sentiment,” “Output Sentiment,” and “Topic Relevancy” evaluators.


The “Prompt Injection” and “Input Toxicity” evaluators detect adversarial prompts designed to jailbreak our agent’s underlying model and cause the agent to violate its guardrails. Let’s test this feature by feeding our application a simple instruction attack. Once we change the prompt to ask the agent to return information about events that falls outside the application’s content moderation guardrails, we can collect traces and see how the evaluators catch and flag these adversarial prompts.


It’s a good idea to set alerts on high-risk prompt injection detections, so you can catch new zero-day threats before they fully compromise your system. You can use AI Agent Monitoring to create a new Datadog Monitor with a single click and set it up to alert you whenever a new prompt injection attempt is detected.


## Correlate LangGraph agent traces with APM, logs, and infrastructure data


Agent failures are not always caused by the model or prompt logic itself. In many cases, the real issue sits somewhere adjacent to the LLM workflow: An upstream Python service is adding latency, an external API is returning incomplete data, or a downstream managed service like Amazon SNS is timing out intermittently. To form a complete picture of your agent’s performance, you need visibility into all of its infrastructure and service dependencies. By correlating AI agent traces with APM, logs, and infrastructure telemetry, you can investigate your agent’s behavior in the broader context of the services and dependencies that support it.


For example, in the trace map shown below, the LangGraph application is connected to calls to api.tavily.com, Amazon Bedrock, and Amazon SNS. We can use this map to inspect not only the agent’s logical workflow, but also the performance of the external systems it depends on.


For instance, if a user-facing request takes too long, you can quickly determine whether the delay came from model inference, tool execution, or a downstream network call. And if an agent response is missing a notification or enriched context, you can correlate that run with the service call that failed or degraded. To enable this correlation, we’ll need to run the Datadog Agent and send agent traces through APM.


First, we’ll add the following configurations to` .env` :


```text
1   DD_ENV=dev    2
3   DD_VERSION=1.0    4
5   DD_HOSTNAME=<hostname>
```


Note that in the GitHub Codespaces environment used for this sample application,` DD_HOSTNAME` must be set explicitly because the Datadog Agent cannot automatically determine the hostname there. In many other environments, this setting is not required.


Next, we’ll launch the Datadog Agent:


Terminal window


```text
1   docker     run     -d     --name     datadog-agent     \    2        --env-file   ./.env     \    3        -p   8126:8126     \    4        -p   8125:8125/udp     \    5        datadog/agent
```


Once the Agent is running, we’ll prepend` ddtrace-run` to the agent’s startup command so that traces are sent through APM:


Terminal window


```text
1   ddtrace-run     python     2_graph_agent.py
```


After this setup is in place, AI Agent Monitoring can link the LLM workflow view with the corresponding APM trace. In the trace details panel, you’ll see a “View in APM” button that takes you directly from the agent run to the associated APM trace. You can also navigate from APM in the other direction and inspect the same request alongside infrastructure and service telemetry. For instance, the following screenshot shows an agent trace along with correlated CPU and memory metrics for the hosts our LangGraph app is running on.


By automatically detecting invocations of external APIs (in this case,[api.tavily.com](http://api.tavily.com/) ) and AWS managed services such as SNS, APM enables you to check the performance of external services invoked by the application, which can otherwise be difficult to keep track of.


APM’s telemetry correlations can be particularly useful during incident response. For instance, let’s say users report that an agent-backed workflow feels slow or unreliable. In AI Agent Monitoring, you might see that the overall run duration has increased, but the root cause may still be unclear.


Jumping into APM can reveal whether a specific dependency is responsible, such as a slow Tavily request, a delayed Bedrock response, or an SNS call that retried unexpectedly. From there, logs can provide the next layer of detail, such as an exception stack trace, a malformed request payload, or a provider-side error message. Instead of treating the agent as a black box, you can follow a single execution across the full application stack.


## Get started with Datadog AI Agent Monitoring


As AI agents move into production, teams need to understand how prompts, tool invocations, context, latency, cost, quality, and downstream dependencies all interact across a full agent workflow. Datadog AI Agent Monitoring provides that end-to-end visibility by helping you trace agent execution, analyze aggregate trends in performance and cost, evaluate output quality with LLM-as-a-judge, and correlate agent telemetry with APM, logs, metrics, and infrastructure data. We’ve shown in this post that by using Datadog AI Agent Monitoring, you can achieve all of this with a relatively simple configuration. For more information about AI Agent Monitoring, see our[documentation](https://docs.datadoghq.com/llm_observability/monitoring/agent_monitoring) . Datadog Agent Observability is free for Datadog customers submitting up to 40,000 LLM spans per month. To learn more about getting started with Agent Observability, see the[product page](https://www.datadoghq.com/products/ai/agent-observability/) and[documentation](https://docs.datadoghq.com/llm_observability/) . If you’re brand new to Datadog,sign up for a 14-day free trial .


-
-
-
