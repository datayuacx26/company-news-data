---
schema_version: "1.0.0"
document_id: "f75660aab782e5380a1245a177ce2d5c313efcb748b58483e290eea3c7459fea"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/ai-agents"
published_at: "2025-11-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T22:25:18.858836+00:00"
content_hash: "sha256:fdfe594ccd5fd998a0114248aea22a5e9f54019d08a611193d23ab7f4d442c70"
---

# AI agent steps in Windmill flows

The rise of large language models has created a new paradigm for workflow automation: instead of predefining every branch and decision in your[workflows](https://www.windmill.dev/docs/flows/flow_editor) , you can let an AI agent reason about which tools to use and orchestrate them dynamically based on context.


[AI agent steps](https://www.windmill.dev/docs/core_concepts/ai_agents) in Windmill bring this capability to your workflows. Define the tools available, a Windmill[script](https://www.windmill.dev/docs/getting_started/scripts_quickstart) or any tools exposed by an MCP server. Then let the agent decide which to call, when to call them, and how to combine their results. The agent becomes a flexible orchestrator that adapts to each request rather than following a rigid script.


This post explores how AI agent steps make sense in the specific context of Windmill, then dives into two technical challenges we solved: making structured output work consistently across different AI providers, and maintaining MCP protocol compliance as the ecosystem matures.


## What AI agent steps bring to Windmill​


Your browser does not support the video tag.[Download the video](https://www.windmill.dev/assets/medias/demo-f8b8db13e3a1b9ce51904d2982f33c3c.mp4) .


### Tool integration​


To sum up roughly,[workflows](https://www.windmill.dev/docs/flows/flow_editor) in Windmill are state machines represented as DAGs (Directed Acyclic Graphs) to compose scripts together. With AI agent steps, any Windmill[script](https://www.windmill.dev/docs/getting_started/scripts_quickstart) becomes a tool the AI agent can invoke. Write your tools in any of the 20+[languages](https://www.windmill.dev/docs/getting_started/scripts_quickstart) Windmill supports - Python, TypeScript, Go, Rust, PHP, Bash, SQL, and more. You can also use tools from the[Windmill Hub](https://hub.windmill.dev/) .


Because every Windmill script already defines its inputs through a[JSON schema](https://www.windmill.dev/docs/core_concepts/json_schema_and_parsing) , they naturally become tool definitions that AI agents can understand. The agent examines each tool's schema, understands its capabilities and required parameters, then reasons about which tools to use based on the user's request. No separate tool registration or documentation needed - the schema that defines how a script works is the same schema that tells the agent what the tool does.


**MCP integration** : Through[Model Context Protocol](https://modelcontextprotocol.io/docs/getting-started/intro) support, agents can also connect to external MCP servers: file system browsers, database interfaces, API integrations, and custom business logic servers. This extends the agent beyond Windmill's internal capabilities to any MCP-compatible service.


### Triggering AI agent workflows​


AI agent workflows can be[triggered](https://www.windmill.dev/docs/triggers) through multiple mechanisms. Use[webhooks](https://www.windmill.dev/docs/core_concepts/webhooks) or[HTTP endpoints](https://www.windmill.dev/docs/triggers/http_routing) to invoke agents programmatically from external systems. These triggers support both streaming and non-streaming modes, allowing you to choose whether to receive the agent's response incrementally or wait for the complete result.


**Conversational workflows** : For interactive use cases, enable[Chat Mode](https://www.windmill.dev/docs/core_concepts/ai_agents#chat-mode) in your[flow](https://www.windmill.dev/docs/flows/flow_editor) , and Windmill transforms your workflow into a conversational experience directly in the UI. Instead of traditional form inputs, users interact through natural conversation.


This works through two key mechanisms. Conversation memory keeps context across the entire interaction - the agent remembers earlier messages and builds on previous exchanges. Configure how much history to maintain, and the agent will recall relevant information throughout the conversation, understanding the broader context of what you're trying to accomplish rather than treating each message in isolation.


Streaming makes the agent's work transparent. As the agent calls tools, processes results, and formulates responses, users can see real-time updates showing exactly what's happening. This visibility is particularly useful for complex workflows where the agent might call multiple tools sequentially - you can follow along rather than staring at a loading spinner.


The result is a workflow that feels truly conversational: the agent maintains context like a human would, and users can see its reasoning unfold in real-time.


Your browser does not support the video tag.[Download the video](https://www.windmill.dev/assets/medias/chat_mode-36790dc76e969d4ba268972c81f8838f.mp4) .


### Multi-provider support and configuration​


Configure your AI agent with any[AI provider](https://www.windmill.dev/docs/core_concepts/ai_generation#models) :[OpenAI](https://platform.openai.com/docs/models) ,[Anthropic](https://docs.anthropic.com/en/docs/about-claude/models/all-models) ,[Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models) ,[Mistral](https://mistral.ai/technology/#models) ,[Google AI](https://ai.google.dev/models/gemini) ,[Groq](https://console.groq.com/docs/models) ,[Together AI](https://docs.together.ai/docs/serverless-models) ,[OpenRouter](https://openrouter.ai/models) , or any custom or local endpoint you operate.


Fine-tune your agent's behavior with configuration options: set system prompts to guide how the agent approaches tasks, adjust temperature to control creativity versus consistency, and set maximum output tokens to manage costs.


You can find the full documentation[here](https://www.windmill.dev/docs/core_concepts/ai_agents) .


### Additional capabilities​


**Structured output** : Conversational text is useful, but sometimes you need data in a specific format that downstream systems can consume reliably. With[JSON schema](https://www.windmill.dev/docs/core_concepts/ai_agents#output_schema-json-schema) validation, you can ensure the AI's response conforms to a precise structure, returning a standardized object rather than free-form text.


**Image support** : AI agent steps support[images](https://www.windmill.dev/docs/core_concepts/ai_agents#user_attachments-optional) as both input and output. Provide images for the agent to analyze, or have the agent generate images in response to your request. Generated images are automatically stored in your workspace's S3-compatible[object storage](https://www.windmill.dev/docs/core_concepts/object_storage_in_windmill) , making them immediately available for subsequent workflow steps.


## Technical challenges​


Building AI agent steps meant solving real technical challenges. Two stand out: makingstructured output work consistently across providers with different capabilities, and maintainingMCP protocol compliance in a maturing ecosystem.


### Structured output across providers​


Supporting multiple AI providers reveals an ongoing challenge: many providers claim OpenAI compatibility, but real-world differences require effort to handle. Structured output illustrates this well.


Most providers support structured output through a response_format parameter. You specify a JSON schema, and the model ensures its response conforms to that structure. This works straightforwardly for OpenAI, Mistral, Google AI, and several other providers.


Anthropic's models don't support response_format. Rather than limiting functionality for Anthropic users, we implemented a[workaround](https://github.com/windmill-labs/windmill/pull/6515) : define a special tool where the tool's input schema matches the desired output structure. The agent calls this tool as its final action, and the tool's arguments become the structured response.


From the user's perspective, structured output works uniformly across all providers. The implementation differs behind the scenes, but the interface remains consistent. This approach lets us support providers with different capabilities while maintaining a unified experience.


### MCP protocol compliance​


Windmill uses the official[rmcp](https://github.com/modelcontextprotocol/rust-sdk) Rust crate for MCP support. This is a well-engineered implementation that strictly follows the MCP protocol specification, exactly as it should.


However, MCP is still a young protocol. As the ecosystem develops, we've encountered servers that don't implement the specification precisely. These aren't malicious implementations, they're often early versions or experimental servers where the authors interpreted certain edge cases differently than the spec intended.


The types of issues that arise typically involve:


- Incorrect HTTP status codes in error responses
- Deviations in how servers signal unsupported features
- Inconsistent handling of optional protocol elements
- Subtle differences in message format expectations


When Windmill connects to a non-compliant server, the strict protocol implementation in rmcp correctly rejects the connection rather than trying to work around the deviation. This is a bet on the ecosystem's long-term health. By maintaining strict compliance, we provide clear error messages about what's wrong and create incentives for servers to fix protocol issues. As the MCP ecosystem matures, these compatibility problems should hopefully diminish.


## A natural fit for workflow orchestration​


AI agent steps in Windmill aren't a separate system grafted onto the platform, they're a natural extension of what Windmill already does well. By building on Windmill's existing[workflow engine](https://www.windmill.dev/docs/flows/flow_editor) , multi-language support, and schema-first design, we created a feature that feels native because it truly is.


The result is a system where AI agents orchestrate workflows the same way humans do: by calling tools, processing results, and making decisions based on context. The tools happen to be Windmill[scripts](https://www.windmill.dev/docs/getting_started/scripts_quickstart) in any language. The execution happens through the same job queue that runs every other workflow. The storage uses the same[S3 integration](https://www.windmill.dev/docs/core_concepts/object_storage_in_windmill) that handles all large artifacts.


As the AI ecosystem evolves, Windmill's AI agent steps will evolve with it. Not because we're constantly rebuilding, but because we built on solid foundations from the start. If you want to help us build features like this,[we're hiring](https://www.windmill.dev/careers) .


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
