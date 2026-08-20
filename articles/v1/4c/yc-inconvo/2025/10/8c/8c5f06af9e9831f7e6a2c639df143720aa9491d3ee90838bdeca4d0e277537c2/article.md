---
schema_version: "1.0.0"
document_id: "8c5f06af9e9831f7e6a2c639df143720aa9491d3ee90838bdeca4d0e277537c2"
company_key: "yc-inconvo"
company: "Inconvo"
source_id: "yc-inconvo-news-import-030a8db3acc0"
canonical_url: "https://inconvo.com/blog/mcp-for-saas/"
published_at: "2025-10-15T16:00:00+00:00"
first_seen_at: "2026-07-25T09:26:00.881998+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:354ad3c03056dd5ddcd3051fa192579241bd55495b0c2572310a0f78a959b08d"
---

# The What, Why and How of MCP for SaaS

## What is MCP?


MCP stands for Model Context Protocol and it’s simply a standard for connecting external systems to AI apps.


*MCP bridges the gap between your applications and AI systems*


## Is MCP just like an API?


API as in **Application Programming Interface** ? … No.


API MCP


**Audience** 👨‍💻 Developers 🤖 AI Apps


**Purpose** 🖥️ ↔️ 🖥️
Connects applications to applications 🖥️ ↔️ 🤖
Connects applications to AI


**Interaction** 🔄 Request - Response 💬 Multi-turn, conversational


## Why give MCP servers to your customers?


**MCP servers are a great way to make your existing product AI-ready fast.**


Being AI-ready will provide you with new growth opportunities as new user behaviour changes from click-driven workflows to conversation-driven assistance.


**Product Metrics** **Business Metrics** **How**


Activation ⬆️ ARR ⬆️ Just ask AI = faster time-to-value.


Engagement ⬆️ NRR ⬆️ AI keeps users exploring. Low barrier to entry.


Retention ⬆️ LTV ⬆️ Consistent utility — keeps users coming back.


## How to build MCP servers?


MCP servers are made up of 3 blocks. The 80/20 of MCP servers is just tools. You can layer on the Resources/Prompts later as needed.


**Block** **Mode** **Role**


🔧 Tools Active Trigger actions


📦 Resources Passive Provide knowledge


💡 Prompts Directive Guide usage


*The three building blocks of an MCP server: Tools, Resources, and Prompts*


If you’re interested in the technical details, Cloudflare have[a great guide on how to build and deploy your first remote MCP server](https://developers.cloudflare.com/agents/guides/remote-mcp-server/) .


You can always use the Inconvo API within tool calls if you want to build, deploy and manage your own MCP server.


## How to connect to an MCP server?


Well-known AI applications with MCP clients:


- [Claude AI](https://claude.ai/)
- [ChatGPT](https://chatgpt.com/)
- [Le Chat](https://chat.mistral.ai/)
- [Perplexity](https://perplexity.ai/)
- [Gemini](https://gemini.google.com/)
- [Microsoft Copilot](https://copilot.microsoft.com/)


## How do tools work?


**Tools are model-controlled; they:**


1. Enable AI models to perform actions
2. Are discovered and invoked by AI models
3. Have clearly defined inputs and outputs


AI applications tell the AI model about the tools available in connected MCP servers. Then, when the user sends a message to the AI model, the model has the ability to choose to use a tool if it sees fit to help answer the message—hence “model controlled.”


*The AI model autonomously decides when to invoke tools based on user requests*


## How do resources work?


**Resources are application-controlled; they:**


1. Enable apps to access resource-based read-only data
2. Are often (but not always) selected by users and incorporated into AI model context by the application
3. Have a unique URI which can be either static or dynamic


AI applications provide methods for users to select resources via text commands (@resource or #resource) or attachment dropdowns. When a resource is selected by a user the app ensures that it is passed to the AI model as context—hence “application controlled”.


*Users select resources through the application, which then provides them as context to the AI model*


## How do prompts work?


**Prompts are user controlled** they:


1. Enable users to ask the AI model to use specific tools
2. Must be selected by users and incorporated into AI model context by the application
3. Have a name and description that define their purpose


AI applications provide methods for users to invoke prompts often via text commands (/prompt). The easiest way to think about them is that they are simply canned natural language instructions for the AI model.


## What should an MCP server for SaaS do?


An MCP server for SaaS should **expose the actions and data of the application.**


It’s easy to think about this from a SaaS UI perspective.


**Actions** Forms & delete buttons


**Data** Dashboards, reports & exports


## How can I expose actions?


**Create tools that call CRUD (Create, Read, Update, Delete) API endpoints.** You can actually drop the Read for now but CRD isn’t a well-known acronym.


Let’s say we have a petstore SaaS API that can add a new pet to the store with \[POST /pet\]


We create a tool add-pet with description add a new pet to the store.


The AI model can call the tool which calls our API to create the pet based on the message/command sent by a user to an AI application.


## How can I expose data?


**This is the trickier of the two between exposing actions and data.**


There are two approaches:


### 1. Wrap each of your GET (Read) endpoints in an MCP tool call.


Load all of your API endpoints into the MCP server as tools and have the client decide which aggregation of tools it should use to fulfil user requests.


This works by effectively making your API directly callable by AI models through AI apps.


I think this is where a bunch of the confusion around “Isn’t an MCP just an API?” comes from.


Through the tool interface the model becomes aware of all endpoints and their input shapes/types.


In order for the model to extract data from your system it must figure out which (or which combination of) endpoint(s) it needs to call to retrieve the requested data.


This approach works well if you already have a highly composable API designed for data analysis — concretely, the API expresses any combination of filters, sorts, and transformations in a structured way. Think GraphQL or OData.


However, most SaaS APIs are designed for record management, not data analysis or flexible retrieval.


**An MCP server with tools wrapping an API which isn’t sufficiently composable will break down** when presented with the wide array of potential user queries which come with a natural language input.


### 2. Build an AI agent for your database and attach it as an MCP tool.


This way when the AI model calls the AI data agent tool it uses the most composable interface of all — natural language.


Then your AI data agent communicates with your database and dynamically generates queries based on the message passed to the tool.


Of course with great power comes great responsibility …


You will need to add deterministic guardrails to ensure no rogue SQL is generated via prompt injection or otherwise.


Also, because exposing data this way relies on natural language — you will need to support multi-turn conversations. You need to manage conversation state and intelligently handle follow-up questions within your AI data agent.


If you have a multi-tenant database you’ll also need to ensure that data scoping logic runs on each query. This must be built with deterministic code rather than an LLM which cannot be trusted to reliably scope data every time.


In the real-world there is often a difference between how data is stored and how that data is spoken about. In those cases, you will need to prompt the AI data agent with information that helps it to understand how to map from the semantics of the data to its storage formats. This is essentially a “Semantic Layer/Model” of the data which you will need to build in order to improve the AI data agent’s performance over time.


As always — you can’t manage what you can’t measure so you’ll need to log agent traces (all steps, decisions, and outputs of an agent) in order to debug and continuously improve.


**Shameless plug, if you want all the power of this approach with none of the lift—[Inconvo](https://inconvo.ai/) solves this.**


## Takeaways


1. MCP is a standard for connecting external systems (e.g. SaaS) to AI
2. It’s not just an API
3. You should build an MCP server—It’s a great way to make your SaaS AI-ready.
4. You can reach the 80/20 of an MCP server with tools alone.
5. An MCP server for SaaS should expose actions and data.


- Expose actions by wrapping your Create/Update/Delete API endpoints in tools.
- Expose data by either wrapping your entire API in tools (less powerful) or building an AI data agent tool (more powerful)
