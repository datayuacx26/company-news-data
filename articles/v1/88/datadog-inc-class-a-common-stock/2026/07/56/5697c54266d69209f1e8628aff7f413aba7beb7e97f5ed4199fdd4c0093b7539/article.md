---
schema_version: "1.0.0"
document_id: "5697c54266d69209f1e8628aff7f413aba7beb7e97f5ed4199fdd4c0093b7539"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-71d6805fc9e1"
canonical_url: "https://www.datadoghq.com/blog/ai-guard-aws-strands-agents/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-25T01:09:56.516023+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:60abb8ce4b5a8bbbd8a55f1e2f97c956bebddae88866ac37e1df929ec3e22a53"
---

# Protect AWS Strands Agents with Datadog AI Guard

Kola Akinnibi


Solutions Architect at AWS


Vijay George


Product Manager


Emmanuelle Lejeail


Engineering Manager


Alexa Levine


Senior Product Marketing Manager


AI agents can reason through tasks, call tools, and adapt their next steps based on intermediate results. That flexibility is useful for building agentic applications, but it also creates security risk at runtime: A prompt injection attempt can change the agent’s instructions, a malicious request can try to exfiltrate sensitive data, and an unsafe tool call can lead to an action that the application owner did not intend.


[Datadog AI Guard](https://docs.datadoghq.com/security/ai_guard/) now works with[AWS Strands Agents](https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-frameworks/strands-agents.html) through a Strands plugin that evaluates prompts, model responses, and tool interactions as the agent runs. By using the Strands native hook system, AI Guard can monitor or block unsafe behavior in the agent loop without requiring teams to scatter security checks throughout application code. In this post, we’ll show how to:


-


Monitor the Strands agent loop


-


Evaluate prompts, responses, and tool calls inline


-


Configure enforcement without changing agent code


-


Investigate AI Guard evaluations in Datadog


## Monitor the Strands agent loop


Strands Agents takes a model-driven approach to orchestration. The model reasons through the task, chooses tools, builds context from previous steps, and decides when it has enough information to respond. This design helps teams build agents that can handle open-ended workflows, but it also means the application’s behavior can change with each user request and model decision.


Traditional application security controls are not always designed for this type of runtime behavior. An agent’s risk can depend on the sequence of prompts, intermediate tool results, and tool calls that led to a particular action. The attack surface is a dynamic sequence of model decisions that changes with every interaction. If teams add custom checks directly into each part of that workflow, those checks can make the agent harder to audit and maintain. Teams also need to redeploy the application whenever they change those checks.


The AI Guard plugin for Strands Agents gives teams a central place to evaluate agent behavior as the agent runs. The plugin assesses every interaction in the context of the full agent session to catch multistep attacks that become harmful only after several tool calls. It registers callbacks on Strands life cycle events, sends relevant content to AI Guard for evaluation, and applies the configured response before the agent loop continues. This makes AI Guard part of the agent’s execution path rather than a separate review layer after the fact.


## Evaluate prompts, responses, and tool calls inline


AI Guard evaluates the parts of an agent session where risk most often appears: user input, assistant output, tool invocations, and tool results. This inline approach helps teams inspect agent behavior in context, including the relationship between a tool call and the preceding steps that produced it.


The` AIGuardStrandsPlugin` registers callbacks for four Strands hook events:


Hook event What AI Guard evaluates Response when blocked


` BeforeModelCallEvent` User prompts, excluding tool results Raises` AIGuardAbortError`


` AfterModelCallEvent` Assistant text content Raises` AIGuardAbortError`


` BeforeToolCallEvent` Pending tool call and conversation context Cancels the tool with a descriptive message


` AfterToolCallEvent` Tool result and conversation context Replaces the tool result content


These checks help protect against common risks in production agent workflows. Prompt protection evaluates user prompts and model responses for attacks such as prompt injection and jailbreaking. Tool protection analyzes tool calls, arguments, intent, and surrounding context to help determine whether an invocation should continue. Sensitive data protection detects personally identifiable information (PII), secrets, and other sensitive content in LLM inputs and outputs.


The AI Guard plugin also avoids duplicate evaluations. Tool results that AI Guard evaluates during` AfterToolCallEvent` are excluded from the next` BeforeModelCallEvent` scan, which prevents the same content from being evaluated twice. If the AI Guard API is unreachable because of a network error, the plugin logs the failure at the debug level and allows the agent to continue.


## Configure enforcement without changing agent code


AI Guard supports a monitor mode that lets teams observe evaluations before they start blocking traffic. This is useful when you are first adding AI Guard to an agent, tuning policy behavior, or evaluating how detections map to real production traffic. After your team has reviewed the results, you can switch a service to blocking mode from the AI Guard settings.


You can also adjust detection sensitivity to make policies stricter or more lenient for a given service. For tool-specific controls, AI Guard supports a tool denylist that proactively blocks selected tools from being used by the agent. This gives security and platform teams a way to reduce risk for sensitive actions, such as file operations, administrative APIs, or payment-related tools.


Because these controls are managed in Datadog, teams can update policies without editing the agent or redeploying the application. This is especially helpful when multiple teams own different agents across environments. Security teams can start in monitor mode, review how agents behave in practice, and then tighten controls as policies mature.


## Investigate AI Guard evaluations in Datadog


After you add AI Guard to a Strands agent, evaluations appear in Datadog as spans that show whether an interaction was safe or unsafe. For unsafe interactions, Datadog includes the attack category, such as prompt injection, data exfiltration, tool misuse, or jailbreaking. These spans link to[Datadog APM](https://docs.datadoghq.com/tracing/) and[Agent Observability](https://docs.datadoghq.com/llm_observability/) traces, so teams can move from a flagged evaluation to the broader agent session that produced it.


This trace context is important because agent attacks often unfold over several steps. A single prompt, tool call, or response might look harmless in isolation, but the full session can reveal how an instruction changed the agent’s behavior or how a tool result influenced the next model call. By reviewing the span side panel, teams can inspect previous prompts, tool calls, and related context as part of the same investigation. Every evaluation decision is a traceable event linked to the full LLM trace, giving teams the audit trail they need to demonstrate that their agent behaved within policy.


AI Guard also provides an aggregate view of evaluations in the Signals tab. Instead of requiring teams to review raw evaluation logs one by one, Datadog groups and ranks events that warrant investigation. This helps security and platform teams prioritize high-signal activity across agents and environments.


## Protect agentic workflows in production


AI Guard for AWS Strands Agents helps teams evaluate agent behavior at the points where runtime risk appears: prompts, responses, tool calls, and tool results. By using the Strands hook system, the plugin brings AI Guard into the agent loop while keeping policy configuration separate from application logic. The result is a centralized way to observe, govern, and block unsafe agent behavior while preserving the trace context that teams need for investigation.


To get started, read the[Datadog AI Guard documentation](https://docs.datadoghq.com/security/ai_guard/) and the[AWS AI Guard Strands plugin documentation](https://strandsagents.com/docs/community/plugins/datadog-ai-guard/) . If you’re interested in trying AI Guard,[sign up for the AI Guard Limited Availability Program](https://www.datadoghq.com/product-preview/ai-security/) .


If you’re new to Datadog,sign up for a 14-day free trial .


-
-
-
