---
schema_version: "1.0.0"
document_id: "e8f31a0a70d776c928b88dec566411ae145fa2a0d6c35e2a5d1cf194a5f132a5"
company_key: "yc-klavis-ai"
company: "Klavis AI"
source_id: "yc-klavis-ai-news-import-5a70a26b6d6e"
canonical_url: "https://www.klavis.ai/blog/building-ai-agents-with-model-context-protocol-on-google-cloud-a-complete-developer-guide"
published_at: "2025-10-29T00:00:00+00:00"
first_seen_at: "2026-07-22T01:35:05.424952+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:a232eefbf40da14adcdf8a211aeeba75158b73e586d4f814910f2d9206448e4e"
---

# Building AI Agents with Model Context Protocol on Google Cloud: A Complete Developer Guide

In this guide, you'll learn how to build agents on Google Cloud that can interact with Gmail, Slack, databases, and hundreds of other services through standardized MCP servers. We'll walk through two approaches: using Gemini directly with function calling, and using Google ADK for more sophisticated multi-agent systems. By the end, you'll have working code and a clear path to production deployment.


## Why MCP + Google Cloud Makes Sense for Agent Development


[ADK supports Model Context Protocol (MCP)](https://cloud.google.com/blog/products/ai-machine-learning/build-and-manage-multi-system-agents-with-vertex-ai) , enabling secure connections between your data and agents. This integration isn't just a checkbox feature—it represents a fundamental shift in how we build AI systems. Here's why developers are choosing Google Cloud for MCP-based agent development:


Factor Why It Matters Google Cloud Advantage


**Native MCP Support** Zero friction integration[Gemini SDKs have built-in support for MCP](https://ai.google.dev/gemini-api/docs/function-calling) , reducing boilerplate code. When the model generates an MCP tool call, the Python and JavaScript client SDK can automatically execute the MCP tool and send the response back to the model.


**Model Performance** Better reasoning = better agents[Today, there are over four million developers building with the power of Gemini](https://blog.google/products/google-cloud/next-2025/) . This rapid adoption resulted in a 20x increase in Vertex AI usage in the past year alone.


**Enterprise Security** Production-ready from day one[MCP Toolbox offers enhanced security through OAuth2 and OIDC](https://cloud.google.com/blog/products/ai-machine-learning/mcp-toolbox-for-databases-now-supports-model-context-protocol) , and end-to-end observability with OpenTelemetry integration.


**Deployment Options** From prototype to scale[We recommend deploying your ADK agent to Vertex AI Agent Engine Runtime](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/agent-development-kit/overview) , a fully managed Google Cloud service specifically designed for deploying, managing, and scaling AI agents.


The real power comes from the ecosystem effect.[Following MCP's announcement, the protocol was adopted by major AI providers, including OpenAI and Google DeepMind](https://en.wikipedia.org/wiki/Model_Context_Protocol) . This means the MCP servers you build today will work across multiple AI platforms tomorrow.


## Understanding the MCP Architecture on Google Cloud


Before we jump into code, let's understand how the pieces fit together. The Model Context Protocol (MCP) is an open standard designed to standardize how Large Language Models communicate with external applications, data sources, and tools. MCP follows a client-server architecture, defining how data (resources), interactive templates (prompts), and actionable functions (tools) are exposed by an MCP server and consumed by an MCP client. Here's how it works in the Google Cloud context:


```text
┌─────────────────────────────────────────────────────────┐
│                    Your Application                     │
│  ┌─────────────────────────────────────────────────┐    │
│  │         Gemini / ADK Agent                      │    │
│  │         (Acts as MCP Client)                    │    │
│  └──────────────────┬──────────────────────────────┘    │
│                     │ MCP Protocol (JSON-RPC)           │
│  ┌──────────────────▼──────────────────────────────┐    │
│  │         MCP Server (Cloud Run / Local)          │    │
│  │  • Gmail Tools        • Slack Tools             │    │
│  │  • Database Tools     • Custom Tools            │    │
│                     │                                   │
└─────────────────────┼───────────────────────────────────┘
│
┌────────────┴───────────┐
│                        │
┌────▼─────┐            ┌─────▼────┐
│  Gmail   │            │  Slack   │
│   API    │            │   API    │
└──────────┘            └──────────┘


```


The security architecture of MCP is designed to be both distributed and centrally governed. Each component in the MCP architecture—Host, Client, and Server—plays a distinct role in enforcing data protection, access control, and operational integrity. Together, they ensure that context and tool invocations remain scoped, authorized, and auditable across all interactions.


### Deployment Options on Google Cloud


Google Cloud gives you flexibility in how you deploy MCP servers:


**1. Cloud Run (Recommended for Production)**


[Serverless environments like Cloud Run are ideal for simple, stateless tools](https://cloud.google.com/discover/what-is-model-context-protocol) . A serverless platform automatically scales your servers based on demand—even to zero—so you only pay for what you use. It offers built-in OAuth support through Google Identity Platform, automatic HTTPS and TLS termination, and global deployment with low latency.


**2. Google Kubernetes Engine (GKE)**


For complex, stateful applications that require fine-grained control over networking and resources, a managed Kubernetes environment provides the power and flexibility needed to run sophisticated MCP infrastructure at enterprise scale. It's ideal for high-throughput scenarios and offers advanced networking and service mesh capabilities.


**3. Vertex AI Agent Engine**


[Vertex AI Agent Engine is now generally available (GA)](https://docs.cloud.google.com/agent-builder/release-notes) . It provides a managed runtime with built-in monitoring and integrates with Google Cloud's security and compliance tools.


## Approach 1: Building Agents with Gemini + MCP


Let's start with the most straightforward approach: using Google's Gemini models directly with MCP servers. This approach is perfect when you need a single agent that can reason and act, without the complexity of multi-agent orchestration.


### Setting Up Your Environment


First, you'll need API keys for both Gemini and Klavis AI.[Klavis AI](https://www.klavis.ai/) provides production-ready MCP servers with enterprise OAuth support, eliminating weeks of integration work.


```text
# Install required packages
pip   install   google-genai klavis


# Set environment variables
export     GEMINI_API_KEY  =  "your_gemini_key"
export     KLAVIS_API_KEY  =  "your_klavis_key"


```


### Creating Your First MCP-Powered Agent


Here's how to create an agent that can read your emails and post to Slack—two tools that previously would have required separate integrations:


```text
import   os
import   webbrowser
from   google   import   genai
from   google  .  genai   import   types
from   klavis   import   Klavis
from   klavis  .  types   import   McpServerName  ,   ToolFormat


# Initialize clients
klavis_client   =   Klavis  (  api_key  =  os  .  getenv  (  "KLAVIS_API_KEY"  )  )
gemini_client   =   genai  .  Client  (  api_key  =  os  .  getenv  (  "GEMINI_API_KEY"  )  )


# Create a Strata MCP server with Gmail and Slack access
response   =   klavis_client  .  mcp_server  .  create_strata_server  (
servers  =  [  McpServerName  .  GMAIL  ,   McpServerName  .  SLACK  ]  ,
user_id  =  "user_123"
)


# Handle OAuth (opens browser for authentication)
if   response  .  oauth_urls  :
for   server_name  ,   oauth_url   in   response  .  oauth_urls  .  items  (  )  :
print  (  f"Authenticate   {  server_name  }  :   {  oauth_url  }  "  )
webbrowser  .  open  (  oauth_url  )


```


What's happening here? Klavis AI's **Strata** MCP server is doing something clever—it implements a progressive discovery approach that prevents overwhelming the model with hundreds of tools at once. Instead of dumping 200+ function definitions into Gemini's context window, Strata guides the model through a hierarchy: intent → category → action → execution.


### Implementing the Agent Loop


Now comes the interesting part—the agent loop that handles multi-step reasoning and tool calls:


```text
def     run_agent  (  mcp_server_url  :     str  ,   user_query  :     str  )  :
"""Execute an agent task with automatic tool calling"""


contents   =     [  ]
contents  .  append  (  types  .  Content  (
role  =  "user"  ,
parts  =  [  types  .  Part  (  text  =  user_query  )  ]
)  )


# Get available tools from MCP server
mcp_tools   =   klavis_client  .  mcp_server  .  list_tools  (
server_url  =  mcp_server_url  ,
format  =  ToolFormat  .  GEMINI
)


max_iterations   =     10
iteration   =     0


while   iteration   <   max_iterations  :
iteration   +=     1


# Get response from Gemini
response   =   gemini_client  .  models  .  generate_content  (
model  =  'gemini-2.5-flash'  ,
contents  =  contents  ,
config  =  types  .  GenerateContentConfig  (  tools  =  mcp_tools  .  tools  )
)


if     not   response  .  candidates  :
return     "No response generated."


contents  .  append  (  response  .  candidates  [  0  ]  .  content  )


# Check for function calls
has_function_calls   =     False
for   part   in   response  .  candidates  [  0  ]  .  content  .  parts  :
if     hasattr  (  part  ,     'function_call'  )     and   part  .  function_call  :
has_function_calls   =     True
function_name   =   part  .  function_call  .  name
function_args   =     dict  (  part  .  function_call  .  args  )


print  (  f"🔧 Calling:   {  function_name  }  "  )
print  (  f"📝 Args:   {  function_args  }  "  )


# Execute the tool via MCP
result   =   klavis_client  .  mcp_server  .  call_tools  (
server_url  =  mcp_server_url  ,
tool_name  =  function_name  ,
tool_args  =  function_args
)


# Add result back to conversation
function_response   =   types  .  Part  .  from_function_response  (
name  =  function_name  ,
response  =  {  'result'  :   result  .  result  }
)
contents  .  append  (  types  .  Content  (
role  =  'tool'  ,
parts  =  [  function_response  ]
)  )


# If no more function calls, we're done
if     not   has_function_calls  :
return   response  .  text


return     "Max iterations reached"


# Run it
result   =   run_agent  (
mcp_server_url  =  response  .  strata_server_url  ,
user_query  =  "Check my latest 5 gmails and summarize them in a Slack message to #engineering"
)


print  (  f"\n✅ Result:   {  result  }  "  )


```


This pattern—request, function calls, results, repeat—is the foundation of agentic behavior. Compositional or sequential function calling allows Gemini to chain multiple function calls together to fulfill a complex request. For example, to answer "Get the temperature in my current location", the Gemini API might first invoke a get_current_location() function followed by a get_weather() function that takes the location as a parameter.


For more details, you can check out[Klavis AI Gemini doc](https://www.klavis.ai/docs/ai-platform-integration/gemini) .


### Why This Works Well


This approach has several advantages:


1.


**Automatic Tool Execution** : The automatic function calling feature of the google-genai Python SDK automatically converts Python functions to the required schema, executes the function calls when requested by the model, and sends the results back to the model to complete the task.


2.


**Built-in OAuth** : Klavis AI handles the OAuth flow for Gmail and Slack, so you don't need to implement token management, refresh logic, or credential storage.


3.


**Type Safety** : The Gemini SDK handles schema validation automatically, reducing runtime errors.


However, this approach has limitations when you need multiple specialized agents working together, which brings us to...


## Approach 2: Building Multi-Agent Systems with Google ADK


ADK was designed to make agent development feel more like software development, to make it easier for developers to create, deploy, and orchestrate agentic architectures that range from simple tasks to complex workflows.


### When to Use ADK vs. Direct Gemini Integration


Use Case Use Gemini Directly Use ADK


**Simple task automation** ✅ Perfect fit ❌ Overkill


**Single-agent workflows** ✅ Simple and fast ⚠️ Works but adds complexity


**Multi-agent coordination** ❌ Hard to manage ✅ Built for this


**Production deployment** ⚠️ Manual setup required ✅ One command deploy


**Observability & debugging** ⚠️ Build your own ✅ Built-in tools


**Agent reusability** ⚠️ Copy-paste code ✅ Composable architecture


### Setting Up an ADK Project


Let's build a more sophisticated system where multiple agents collaborate. Start by creating a new ADK project:


```text
# Install ADK
pip   install   google-adk klavis


# Create a new agent project
adk create my_agent_system


# Project structure:
# my_agent_system/
#   ├── agent.py      # Main agent code
#   ├── .env          # Environment variables
#   └── __init__.py


```


### Implementing an ADK Agent with MCP


Here's a production-ready agent configuration that uses Klavis AI's MCP servers:


```text
import   os
import   webbrowser
from   google  .  adk  .  agents  .  llm_agent   import   Agent
from   google  .  adk  .  tools  .  mcp_tool   import   StreamableHTTPConnectionParams
from   google  .  adk  .  tools  .  mcp_tool  .  mcp_toolset   import   McpToolset
from   klavis   import   Klavis
from   klavis  .  types   import   McpServerName
from   dotenv   import   load_dotenv


load_dotenv  (  )


# Initialize Klavis client
user_id   =     "prod_user_001"


# Create Strata server with multiple integrations
strata_response   =   klavis_client  .  mcp_server  .  create_strata_server  (
servers  =  [
McpServerName  .  GMAIL  ,
McpServerName  .  SLACK  ,
McpServerName  .  GITHUB  ,
McpServerName  .  LINEAR
]  ,
user_id  =  user_id
)


# Handle OAuth for each service
if   strata_response  .  oauth_urls  :
for   server_name  ,   oauth_url   in   strata_response  .  oauth_urls  .  items  (  )  :
user_auth   =   klavis_client  .  user  .  get_user_auth  (
user_id  =  user_id  ,
server_name  =  server_name
)
if     not   user_auth  .  is_authenticated  :
print  (  f"🔐 Opening OAuth for   {  server_name  }  ..."  )
webbrowser  .  open  (  oauth_url  )
input  (  f"Press Enter after completing   {  server_name  }   OAuth..."  )


# Create the root agent (required by ADK)
root_agent   =   Agent  (
name  =  "engineering_assistant"  ,
model  =  "gemini-2.5-flash"  ,
description  =  "An agent that helps engineering teams with email, Slack, GitHub, and Linear"  ,
instruction  =  """You are a helpful engineering assistant with access to:
- Gmail: for reading and managing emails
- Slack: for team communication
- GitHub: for code reviews and repository management
- Linear: for issue tracking and project management


Always explain what you're doing and ask for confirmation before making changes.
Be concise but thorough in your responses."""  ,
tools  =  [
McpToolset  (
connection_params  =  StreamableHTTPConnectionParams  (
url  =  strata_response  .  strata_server_url  ,
)  ,
)
]  ,
)


```


With ADK, you can build an AI agent in under 100 lines of intuitive code. The real power is in the orchestration capabilities that come built-in.


Check out[Klavis AI ADK documentation](https://www.klavis.ai/docs/ai-platform-integration/google-adk) for more information.


### Running and Deploying Your ADK Agent


To test your agent locally:


```text
# Start the interactive web interface
adk web


```


This launches a local server where you can interact with your agent through a web UI. Behind the scenes, ADK handles session management, tool confirmation flows (HITL), conversation history, and error handling and retries.


## Frequently Asked Questions


**Q: How is MCP different from function calling?**


A: Function calling is a capability of the LLM—it can invoke functions you define. MCP is a protocol that standardizes how those functions are exposed and invoked. While Function Calling provides the core capability, the Model Context Protocol (MCP) takes it a step further — making the entire process structured, flexible, and interoperable across a wide range of tools and systems. Think of function calling as HTTP, and MCP as REST.


**Q: Do I need to build my own MCP servers?**


A: Not necessarily. Klavis AI provides production-ready MCP servers with OAuth support for services like Gmail, Slack, GitHub, Linear, Salesforce, and more. You can use these out of the box, or build custom servers for proprietary systems.


**Q: What's the difference between running MCP servers locally vs. on Cloud Run?**


A: Local servers (stdio transport) are great for development but don't scale to multiple users. Cloud Run deployment (HTTP+SSE transport) gives you multi-user support with OAuth, automatic scaling, better security isolation, and production-grade monitoring. For prototyping, start local. For production, deploy to Cloud Run or use Klavis AI's hosted servers.


**Q: How do I handle rate limits when agents make API calls?**


A: Implement retry logic with exponential backoff:


```text
from   tenacity   import   retry  ,   stop_after_attempt  ,   wait_exponential


@retry  (
stop  =  stop_after_attempt  (  3  )  ,
wait  =  wait_exponential  (  multiplier  =  1  ,     min  =  4  ,     max  =  10  )
)
def     call_mcp_tool  (  server_url  ,   tool_name  ,   args  )  :
return   klavis_client  .  mcp_server  .  call_tools  (
server_url  =  server_url  ,
tool_name  =  tool_name  ,
tool_args  =  args
)


```


Also consider implementing caching at the MCP server level to reduce redundant API calls.


---


Ready to get started? Grab your API keys for[Google AI Studio](https://aistudio.google.com/) and[Klavis AI](https://klavis.ai/) , clone the example code from[Klavis AI's GitHub repository](https://github.com/klavis-ai/klavis) , and start building. The future of work isn't going to automate itself—but your agents might.
