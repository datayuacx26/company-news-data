---
schema_version: "1.0.0"
document_id: "94af5946e047fc3da3dd14d668b6aa08b8ca2c73af96f89c32ce22c52b83c50a"
company_key: "shopify-inc-class-a-subordinate-voting-shares"
company: "Shopify Inc."
source_id: "shopify-inc-class-a-subordinate-voting-shares-news-import-b162817d780b"
canonical_url: "https://www.shopify.com/blog/openai-agent-builder"
published_at: "2026-08-06T15:14:35+00:00"
first_seen_at: "2026-08-07T06:19:57.963444+00:00"
fetched_at: "2026-08-07T06:19:58.816256+00:00"
content_hash: "sha256:90d9121b056fff9de1f3a7cc9592c50822704dfa7866728c1056b2ed3d1f3b32"
---

# How To Use OpenAI Agent Builder for Ecommerce

OpenAI Agent Builder is a no-code platform that lets you design and manage custom AI agents that can automate ecommerce tasks like order tracking and shopping cart recovery.


This is part of a broader shift: Nearly[44% of ecommerce brands](https://www.salesmate.io/blog/ai-agents-in-ecommerce-report/) have used at least one AI agent, and while most still rely on third-party AI agents,[a growing number](https://www.forbes.com/sites/johnkoetsier/2025/10/07/marketers-are-building-millions-of-ai-agents-what-does-that-mean-for-the-rest-of-us/) are building their own through no-code or low-code platforms.


Store owners who want to automate complex workflows can use OpenAI Agent Builder to tailor an agent to their needs. Here’s how it works and how you can take advantage of it.


## What is OpenAI Agent Builder?


[Agent Builder](https://platform.openai.com/agent-builder) is OpenAI’s no-code/low-code platform for creating custom[AI agents](https://www.shopify.com/blog/ai-agents) —software systems that use AI to autonomously make decisions and act on your behalf. It has a visual interface for defining agent behavior, connecting external tools, and testing how agents interact. Once built, specialized agents can be deployed on your website, within ChatGPT, embedded inside applications, or used as standalone tools.


## Primary components of OpenAI Agent Builder


Every agent consists of three fundamental elements: the[large language model (LLM)](https://www.shopify.com/blog/large-language-models) that powers how the agent operates, the tools or external functions it can use to take action, and the instructions and guardrails that govern what it can and can’t do.


OpenAI Agent Builder provides a canvas for building multi-step agent workflows. Here are the key components:


-


**Workflow canvas.** Where you design agent flows and connect components.


-


**Nodes.** Define each step you want an agent to take. If a customer asks where their order is, for instance, an OpenAI Agent Builder node might read the message and classify it as “order_status,” then pass that label onto a Router node that sends the question to an order lookup tool, a returns handler, or a human agent.


-


**Typed edges.** Typed edges connect nodes and allow them to communicate, ensuring the output type of one node matches the input type of the next.


-


**Control flow.** The branching and conditional logic that determines which step runs next in the workflow, based on prior output. The router node in the example above interprets order_status from the classifier node and decides where to send the customer’s question.


-


**Inputs and outputs.** Inputs are the information each node expects to receive; outputs are the structured result it produces so the next node can act.


-


**Guardrails.** Agent instructions that interrupt or alter a workflow when an output falls outside the rules you’ve set. If a customer uses inappropriate language while requesting a refund, for example, guardrails can tell the agent to ignore the hostile language and respond in the usual way.


-


**ChatKit.** A widget for embedding chat-based agents inside your applications—for example, a conversational interface for customers to check order status.


-


**Tools and connectors.** The external systems you want the agent to use, such as search engines,[application programming interfaces (APIs)](https://www.shopify.com/enterprise/blog/ecommerce-api) , customer relationship management (CRM) platforms, or other approved business apps listed in the Connector Registry.


-


**Connector Registry.** Where you manage the external platforms and data sources your agent is authorized to access, letting you limit each agent’s access to sensitive information and systems.


Build an online store with AI


Create a website in minutes with the AI store builder. Describe your brand or products to generate a free custom theme that fits your idea.


[Try it now](https://www.shopify.com/tools/ai-store-builder)


## Step-by-step guide to using OpenAI Agent Builder


1. Open Agent Builder and create a new workflow
2. Configure the Start node
3. Connect a Guardrail node to the Start node
4. Add an Agent node to classify input
5. Connect a Router node to the classifier
6. Connect your external data source
7. Give instructions to the order status agent
8. Add an output Guardrail node
9. Preview and test
10. Deploy with ChatKit


Before you start building agents, you need an OpenAI account with billing set up at[platform.openai.com](http://platform.openai.com/) and your organization verified in account settings. You also need credentials for the external system you want to connect—in this case, your[Shopify store](https://www.shopify.com/blog/build-a-website) ’s API or an order management system.


Here’s an example of an OpenAI Agent Builder workflow used to build a simple order status agent.


### 1. Open Agent Builder and create a new workflow


Go to platform.openai.com and navigate to the Agent Builder section. You’ll see three tabs: Workflows (published), Drafts (unfinished), and Templates (predefined starting points).


Select Templates and choose the Customer Service template, which pre-populates the canvas with a starter workflow, or choose “Blank workflow” if you want full control from the start. Name your workflow: for example, “Order Status Agent.”


### 2. Configure the Start node


The Start node is the entry point to any workflow. It accepts input variables (what the customer types into the chatbot) and state variables that persist across the workflow.


For example, you could use “input_as_text” to represent the input variable and “order_number” as the state variable the agent carries through every subsequent step.


### 3. Connect a Guardrail node to the Start node


The Guardrail node has several options: Moderation classifies and blocks harmful content; Jailbreak detects prompt injection and keeps the model on task; PII detection redacts personally identifiable information; and Hallucination verifies the agent’s output against a knowledge source.


At minimum, you’ll want to enable Moderation and Jailbreak to ensure hostile or off-topic inputs are filtered before they reach your agent.


### 4. Add an Agent node to classify input


Connect an Agent node to the Guardrail node and configure it to classify input using an instruction like, “Read the customer’s message and classify their intent as one of: order_status, return_request, product_question, or other.”


Set the output[format to JSON](https://www.shopify.com/blog/what-is-a-json-file) so the result is a clean label that the next node can act on. Connecting this classifier node to the Guardrail’s “pass path” ensures that the classifier only runs on input that’s already been screened.


### 5. Connect a Router node to the classifier


The router interprets the output from the classifier and sends the conversation down the right path: to an order lookup agent for “order_status,” a returns handler for “return_request,” and so on.


For the purposes of this example, route the “order_status” path to an order-lookup agent, and route other customer questions to a fallback response such as, “I’m only able to help with order status, please contact support with any other questions.”


### 6. Connect your external data source


Now the router needs to know which order-lookup tool to use. Agent Builder supports File Search (for uploaded documents), Web Search, Code Interpreter,[model context protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro) servers, and custom API actions as tools.


For a Shopify store, connect via the[Shopify MCP server](https://shopify.dev/docs/apps/build/storefront-mcp) —available in the MCP marketplace inside Agent Builder—and authorize it using your store credentials in the Connector Registry. The agent can now query live order data: status, tracking number, and estimated delivery.


### 7. Give instructions to the order status agent


Open the configuration panel for the order lookup agent and add clear instructions defining exactly what this agent should do. For example: “You have access to the customer’s order data. Look up their order using the order number provided, then return the current status, carrier, and estimated delivery date in a friendly, concise message. Do not speculate—only report what the data shows.” Set verbosity to medium and enable conversation history so the agent can handle follow-up questions in the same session.


### 8. Add an output Guardrail node


Add a second Guardrail node between the order lookup agent and the customer-facing response. This one checks the output rather than the input, ensuring the response doesn’t include raw data fields, sensitive account details, or anything outside the scope of the question asked.


### 9. Preview and test


Use the live chat interface in preview mode to simulate real interactions. You can inspect how the agent reasons step by step and review the intermediate outputs it generates at each node. Test edge cases such as an invalid order number, a hostile message, or a question outside the agent’s scope. Confirm the guardrails operate correctly and the fallback paths behave as expected.


### 10. Deploy with ChatKit


When the workflow is ready, publish it to generate a version ID. Then open the ChatKit settings and configure the widget—set the name, welcome message, and brand colors. Copy the embed code and paste it into your storefront or order confirmation page. Customers can now interact with the chat-based agent directly on your site without being redirected anywhere else.


Your 24/7 Shopify expert is here


From daily tasks to strategic planning, Sidekick delivers instant help the moment you ask. Learn how your AI assistant makes it easier to start, run and grow your business on Shopify.


[Meet Sidekick](https://www.shopify.com/magic)


## Using OpenAI Agent Builder alongside Shopify tools


[Shopify Sidekick](https://www.shopify.com/magic) is an AI assistant in your Shopify admin that you can use to generate content, build apps, get guidance, and complete tasks. Take UK fashion house[Maggy London](https://www.shopify.com/case-studies/maggy-london) , which used Shopify Sidekick to analyze top customer search terms, identify the landing pages drawing the most visits, and understand why certain products were returned more than others—all without deep technical or analytical expertise.


“Sidekick turned our ecom team into a strategic intelligence hub for the whole company,” says company president Sara Bako. “The insights we pull don’t just improve our website—they inform design and product development.”


While tools like Sidekick can help with back-office operations, OpenAI Agent Builder can automate connections to platforms outside of Shopify. Sidekick might help you analyze back-office data or write a product description; an OpenAI agent can answer a customer’s question, check inventory, obtain their loyalty status from your[CRM](https://www.shopify.com/blog/crm) app, apply discounts, and create a checkout link.


Workflow automation tools like[Shopify Flow](https://help.shopify.com/en/manual/shopify-flow/getting-started/workflow-examples) respond to customer requests based on predetermined rules, while an OpenAI agent can be trained to understand the context of each request, ask clarifying questions, and deliver a personalized response. With Flow, you define what happens for each specific event; with an agent, you define the goals and let it figure out how to achieve them.


## OpenAI Agent Builder FAQ


### Can you create agents with OpenAI?


OpenAI provides two primary ways to build agents: Agent Builder is the no-code/low-code option for people with minimal programming experience; the[Agents software development kit (Agents SDK)](https://developers.openai.com/api/docs/guides/agents) is aimed at professional developers who want full control over orchestration, tool execution, and state management.


### Is OpenAI Agent Builder free?


OpenAI Agent Builder operates using a freemium model. Building an agent and testing it costs nothing, but once you or your customers start using it, you’ll pay a small fee per interaction. How much you pay depends on which LLM you choose and how complex the agent’s actions are.


### How can I gain access to OpenAI Agent Builder?


Visit platform.openai.com and navigate to the Agent Builder section. You need an OpenAI account; if you don’t have one, create it at the same URL. Add your billing details and verify your organization in account settings. This is required to run agents in preview mode—you can build without it, but you can’t test until it’s done.
