---
schema_version: "1.0.0"
document_id: "db5524fdd2a0516557bba67bdccc401ea6611d082efb57fec49b24b247fd3948"
company_key: "yc-sigmamind-ai"
company: "SigmaMind AI"
source_id: "yc-sigmamind-ai-news-import-39ccea9bc83d"
canonical_url: "https://www.sigmamind.ai/blog/ai-agent-builder-code-langgraph-crewai-claude"
published_at: null
first_seen_at: "2026-07-22T13:27:04.229547+00:00"
fetched_at: "2026-07-28T21:20:12.930591+00:00"
content_hash: "sha256:a07c4d7416be4a898f7849eb767ca4d9c27770b968332b0396d99c282bdf61dd"
---

# AI Agent Builder Code 2026: LangGraph, CrewAI, Claude

## TL;DR


This guide walks through building AI agents with actual code using the most popular frameworks in 2025 and 2026. You’ll find runnable Python snippets for LangGraph, CrewAI, and the Claude Agent SDK, plus step-by-step setup instructions for low-code and no-code paths. Each section covers the what, when, and how, including tool calling, memory, orchestration patterns, and voice AI integration. Copy the examples, modify them, ship agents.


## What Is an AI Agent Builder?


An AI agent builder is a development environment (framework, SDK, or platform) that provides the scaffolding to create, deploy, and manage autonomous AI agents. Instead of wiring up every capability from raw API calls, builders supply the primitives: memory management, state handling, tool access, and API integrations.


The boundary that matters: an AI agent builder is not a chatbot builder. A chatbot answers “What’s my order status?” by looking up a record. An AI agent handles “My package didn’t arrive, and I need a replacement shipped to a different address” by verifying the order, checking inventory, initiating a return, updating the shipping address, and confirming the new delivery. The agent reasons through multiple steps. The chatbot responds to one.


Worth noting: the industry has a growing “agent washing” problem. One popular thread on r/ArtificialIntelligence put it bluntly: “Real agents reason, make decisions, use tools, access external data, and complete end-to-end tasks. Most things called agents right now are just automation with a new label.” Keep this in mind when evaluating any tool that calls itself an AI agent builder.


## The Three Tiers of AI Agent Builder Code


When people search for “ai agent builder code,” they could mean three different things. Here’s how the tiers break down, followed by working code for each.


Dimension Code-First Low-Code No-Code


Who builds Developers Developers + technical ops Ops, support, marketing teams


Time to first agent Days to weeks Hours to days Minutes to hours


Customization ceiling Unlimited High (with code escape hatches) Moderate (extensible via APIs/webhooks)


Maintenance burden High Medium Low


Best for Complex multi-agent systems, regulated industries Prototyping with production path Standard workflows, rapid iteration


For a broader comparison of platforms across all three tiers, the[agent builder platforms guide](https://www.sigmamind.ai/blog/ai-agent-builder-platforms-guide) covers the full spectrum.


## Code-First: Building Agents with Python


Code-first is where the majority of production AI agents live. According to the Linux Foundation, 68% of production AI agents run on open-source frameworks. Here are working examples with the frameworks that matter most.


### LangGraph: Your First Stateful Agent


LangGraph models agent workflows as directed graphs. Nodes are functions, edges are transitions. This gives you fine-grained control over state, branching, and long-running processes.


**Setup:**


```text
pip install langgraph langchain-openai langchain-core
export OPENAI_API_KEY="sk-your-key-here"


```


**A basic ReAct agent with tool calling:**


```text
from langchain_openai import ChatOpenAI
from langchain_core.tools import tool
from langgraph.prebuilt import create_react_agent
from langgraph.checkpoint.memory import MemorySaver


# AI Agent Builder Code 2026: LangGraph, CrewAI, Claude
@tool
def lookup_order(order_id: str) -> str:
"""Look up order status by order ID."""
# Replace with your actual DB/API call
orders = {
"ORD-1234": {"status": "shipped", "tracking": "1Z999AA10123456784"},
"ORD-5678": {"status": "processing", "eta": "2 business days"},
}
order = orders.get(order_id)
if order:
return str(order)
return f"No order found with ID {order_id}"


@tool
def initiate_refund(order_id: str, reason: str) -> str:
"""Initiate a refund for a given order."""
# Replace with your payment processor API call
return f"Refund initiated for {order_id}. Reason: {reason}. Confirmation: REF-{order_id[-4:]}"


# Create the agent with memory
memory = MemorySaver()
model = ChatOpenAI(model="gpt-4o", temperature=0)


agent = create_react_agent(
model,
tools=[lookup_order, initiate_refund],
checkpointer=memory,
prompt="You are a customer support agent. Look up orders and process "
"refunds when customers request them. Always confirm before "
"initiating a refund."
)


# Run the agent
config = {"configurable": {"thread_id": "customer-session-001"}}


response = agent.invoke(
{"messages": [{"role": "user", "content": "I need a refund for order ORD-1234. It arrived damaged."}]},
config=config,
)


for message in response["messages"]:
print(f"{message.type}: {message.content}")


```


**What’s happening here:** The` create_react_agent` function builds a graph where the LLM decides which tool to call at each step. The` MemorySaver` checkpointer persists conversation state across turns, so the agent remembers context within a session. The` thread_id` in the config lets you maintain separate conversation threads per customer.


**Building a custom graph for more control:**


```text
from langgraph.graph import StateGraph, START, END
from typing import TypedDict, Annotated
from langchain_core.messages import HumanMessage, AIMessage
import operator


class AgentState(TypedDict):
messages: Annotated[list, operator.add]
order_data: dict | None
needs_escalation: bool


def classify_intent(state: AgentState) -> AgentState:
"""Classify the customer's intent from their message."""
last_message = state["messages"][-1].content.lower()
if any(word in last_message for word in ["refund", "return", "money back"]):
return {**state, "needs_escalation": False}
if any(word in last_message for word in ["lawyer", "lawsuit", "complaint"]):
return {**state, "needs_escalation": True}
return {**state, "needs_escalation": False}


def handle_refund(state: AgentState) -> AgentState:
"""Process refund request."""
return {
**state,
"messages": [AIMessage(content="I've started your refund. You'll see it in 3-5 business days.")]
}


def escalate_to_human(state: AgentState) -> AgentState:
"""Transfer to human agent with full context."""
context = " | ".join([m.content for m in state["messages"]])
return {
**state,
"messages": [AIMessage(content=f"Transferring you to a specialist with your full conversation history.")]
}


def route_by_escalation(state: AgentState) -> str:
if state["needs_escalation"]:
return "escalate"
return "handle"


# Build the graph
graph = StateGraph(AgentState)
graph.add_node("classify", classify_intent)
graph.add_node("handle_refund", handle_refund)
graph.add_node("escalate", escalate_to_human)


graph.add_edge(START, "classify")
graph.add_conditional_edges("classify", route_by_escalation, {
"handle": "handle_refund",
"escalate": "escalate",
})
graph.add_edge("handle_refund", END)
graph.add_edge("escalate", END)


app = graph.compile()


# Run it
result = app.invoke({
"messages": [HumanMessage(content="I want a refund for my damaged order")],
"order_data": None,
"needs_escalation": False,
})


```


This custom graph pattern is what makes LangGraph powerful for production: you define exactly how the agent transitions between states, with conditional routing that you control.


**Best for:** Complex workflows with conditional branching, cyclic reasoning loops, and multi-agent coordination.


### CrewAI: Role-Based Agent Teams


CrewAI takes a different approach. Instead of graphs, it models agents as a team of specialists that collaborate. Think of it as assembling a crew rather than drawing a flowchart.


**Setup:**


```text
pip install crewai crewai-tools
export OPENAI_API_KEY="sk-your-key-here"


```


**A three-agent customer support crew:**


```text
from crewai import Agent, Task, Crew, Process
from crewai_tools import SerperDevTool


# Agent 1: Triage specialist
triage_agent = Agent(
role="Customer Triage Specialist",
goal="Classify incoming customer issues by type and urgency",
backstory="You've worked in customer support for 10 years. You can "
"instantly tell the difference between a billing issue, a "
"technical problem, and a shipping complaint.",
verbose=True,
allow_delegation=True,
)


# Agent 2: Order lookup agent
order_agent = Agent(
role="Order Research Specialist",
goal="Find all relevant order information and shipping status",
backstory="You have access to order management systems and can pull "
"up any order details, tracking info, and delivery status.",
verbose=True,
)


# Agent 3: Resolution agent
resolution_agent = Agent(
role="Resolution Specialist",
goal="Propose and execute the best resolution for the customer",
backstory="You know company policies inside and out. You're empowered "
"to issue refunds up to $200, send replacements, and offer "
"store credit. Above $200 requires manager approval.",
verbose=True,
)


# Define tasks
triage_task = Task(
description="Classify this customer issue: '{customer_message}'. "
"Determine the category (billing, shipping, technical, other) "
"and urgency level (low, medium, high, critical).",
expected_output="Issue classification with category and urgency level.",
agent=triage_agent,
)


research_task = Task(
description="Look up all relevant information for this customer issue. "
"Find order details, history, and any previous interactions.",
expected_output="Complete order and customer history summary.",
agent=order_agent,
)


resolution_task = Task(
description="Based on the triage classification and research, propose "
"the best resolution. If it's within your authority, execute it. "
"If not, prepare an escalation summary for a manager.",
expected_output="Resolution action taken or escalation summary with recommendation.",
agent=resolution_agent,
)


# Assemble and run the crew
crew = Crew(
agents=[triage_agent, order_agent, resolution_agent],
tasks=[triage_task, research_task, resolution_task],
process=Process.sequential,  # Tasks run in order
verbose=True,
)


result = crew.kickoff(
inputs={"customer_message": "I ordered a blender 2 weeks ago and it still hasn't arrived. Order ORD-9012."}
)
print(result)


```


A typical three-agent sequential crew using GPT-4o costs approximately $0.10 to $0.20 per run, or $0.06 to $0.12 with GPT-4o-mini for simpler tasks.


**Best for:** Role-based collaboration scenarios, rapid prototyping, teams that want an intuitive mental model.


**The practitioner decision rule:** A widely shared piece of advice from production teams: start with CrewAI for speed, then migrate the parts that need more control to LangGraph. CrewAI’s LangChain compatibility means this isn’t a full rewrite. It’s a gradual transition.


### Claude Agent SDK: Anthropic-Native Agents


Anthropic’s official agent SDK powers the same architecture behind Claude Code. It provides production-grade primitives for tool use, hooks, MCP (Model Context Protocol) integration, and subagents.


**Setup:**


```text
pip install claude-agent-sdk anthropic
export ANTHROPIC_API_KEY="sk-ant-your-key-here"


```


**A tool-using agent with MCP:**


```text
from claude_agent_sdk import Agent, tool, MCPServer


# Define tools
@tool
def search_knowledge_base(query: str) -> str:
"""Search the internal knowledge base for relevant articles."""
# Replace with your actual vector DB query
return f"Found 3 articles matching '{query}': [Article on returns policy], [Shipping FAQ], [Warranty terms]"


@tool
def create_support_ticket(
customer_email: str,
subject: str,
description: str,
priority: str = "medium"
) -> str:
"""Create a support ticket in the helpdesk system."""
# Replace with your Zendesk/Freshdesk API call
ticket_id = "TKT-" + str(hash(customer_email))[:6]
return f"Ticket {ticket_id} created. Priority: {priority}. Subject: {subject}"


# Optional: Connect to an MCP server for external tool access
# mcp = MCPServer("http://localhost:3000/mcp")


# Create the agent
agent = Agent(
model="claude-sonnet-4-20250514",
tools=[search_knowledge_base, create_support_ticket],
system_prompt=(
"You are a customer support agent for an e-commerce company. "
"Search the knowledge base before answering questions. "
"Create tickets for issues that need follow-up. "
"Be concise and helpful."
),
max_turns=10,
)


# Run a conversation
response = agent.run(
"Hi, I bought a coffee maker last month and it stopped working. "
"What are my options? My email is jane@example.com"
)
print(response.final_message)
print(f"Tools called: {[tc.tool_name for tc in response.tool_calls]}")
print(f"Total tokens: {response.usage.total_tokens}")


```


**Best for:** Teams building on Claude models who want tight integration with Anthropic’s ecosystem and native MCP support for tool connections.


### Other Notable Frameworks


**AutoGen / AG2 (Microsoft):** Multi-agent conversation framework with strong enterprise support. Custom memory stores and flexible agent-to-agent communication patterns.


**Google Agent Development Kit (ADK):** Python-based framework for building agents within Google Cloud. 17,800 GitHub stars and 3.3 million monthly downloads. Deterministic guardrails for enterprise deployments.


**Semantic Kernel (Microsoft):** Designed for .NET and Python developers integrating LLM capabilities into existing enterprise applications.


**Pydantic AI:** Type-safe agent framework for Python developers who want strong validation and structured outputs.


For teams specifically building voice agents with code, the[enterprise voice agent architecture guide](https://www.sigmamind.ai/blog/building-enterprise-realtime-voice-agents-from-scratch) walks through the full technical stack.


## Low-Code and Visual Agent Builders


Low-code platforms occupy the middle ground. They attract teams that want visual design speed but need code for production edge cases.


### OpenAI Agent Builder


OpenAI’s Agent Builder provides a visual canvas for composing logic with drag-and-drop nodes, connecting tools, and configuring guardrails. It supports preview runs, inline evaluation, and versioning.


The community pushback is worth noting. Practitioners on Reddit and developer forums consistently report that Agent Builder requires more technical skill than the “no-code” marketing suggests. It works best for rapid prototyping within the OpenAI ecosystem.


### n8n: Workflow Automation with Code Escape Hatches


n8n is where many teams land when they want visual workflow design with the ability to drop into JavaScript or Python when needed. Here’s a practical pattern for connecting an AI agent to external tools:


```text
// n8n Code Node: Custom tool logic inside a visual workflow
// This node sits between the AI Agent node and your CRM


const orderData = $input.first().json;


// Custom business logic that would be hard to express visually
const refundEligible = (
orderData.days_since_purchase <= 30 &&
orderData.item_condition !== "used" &&
orderData.total_amount <= 500
);


const escalationNeeded = (
orderData.total_amount > 500 ||
orderData.customer_tier === "enterprise" ||
orderData.previous_refunds_count > 3
);


return [{
json: {
...orderData,
refund_eligible: refundEligible,
escalation_needed: escalationNeeded,
suggested_action: escalationNeeded ? "transfer_to_human" :
refundEligible ? "auto_refund" : "deny_with_explanation"
}
}];


```


**Flowise and Langflow** offer similar visual canvas experiences specifically optimized for LLM chain building. Dify, another low-code agent platform, has attracted[129,000+ GitHub stars](https://www.firecrawl.dev/blog/best-ai-agent-frameworks-2025) , signaling massive developer interest in the visual builder category.


All of these tools share a common pattern: visual design for the 80% of the workflow that’s straightforward, code for the 20% that isn’t.


## No-Code Agent Builders (with API Extensibility)


No-code platforms let you create agents using visual interfaces and natural language instructions. Key platforms include Voiceflow, MindStudio, Zapier Central, Make, Relevance AI, and SigmaMind.


The biggest value of no-code isn’t just simplicity. It’s speed and organizational access. Operations teams can prototype agents without waiting in a development queue. If the prototype works, engineers can extend it through APIs and webhooks later.


### When No-Code Is Enough


No-code handles standard customer support triage, FAQ handling, appointment scheduling, lead qualification scripts, and internal knowledge retrieval well. These are workflows with predictable paths and clear escalation points.


### When No-Code Falls Short


No-code breaks down with complex multi-agent coordination, custom reasoning pipelines, tight performance tuning, and regulated industries demanding detailed audit trails. The[80 to 90% AI project failure rate](https://www.rand.org/pubs/research_reports/RRA2680-1.html) from a 2025 RAND study isn’t just about technology. It’s often about picking the wrong tier for the problem’s actual complexity.


### Extending No-Code with Webhooks and APIs


Even on no-code platforms, you’ll typically write code for custom integrations. Here’s a common pattern, a webhook endpoint that a no-code agent calls to execute business logic:


```text
# Flask webhook endpoint called by a no-code agent builder
from flask import Flask, request, jsonify
import requests


app = Flask(__name__)


@app.route("/agent-webhook/refund", methods=["POST"])
def handle_refund():
"""
The no-code agent triggers this webhook when a customer
requests a refund. The agent passes structured data from
the conversation.
"""
data = request.json


order_id = data.get("order_id")
customer_email = data.get("customer_email")
reason = data.get("reason")
amount = data.get("amount")


# Step 1: Verify order in your system
order = lookup_order_in_db(order_id)
if not order:
return jsonify({"success": False, "message": "Order not found"}), 404


# Step 2: Check refund eligibility
if order["days_since_purchase"] > 30:
return jsonify({
"success": False,
"message": "Order is outside the 30-day refund window",
"suggestion": "Offer store credit instead"
})


# Step 3: Process the refund via payment processor
refund_result = process_refund_via_stripe(order["payment_intent_id"], amount)


# Step 4: Update helpdesk ticket
update_zendesk_ticket(data.get("ticket_id"), status="refund_processed")


return jsonify({
"success": True,
"refund_id": refund_result["id"],
"message": f"Refund of ${amount} processed. Customer will see it in 3-5 business days."
})


def lookup_order_in_db(order_id):
# Your database query here
pass


def process_refund_via_stripe(payment_intent_id, amount):
# Stripe API call here
pass


def update_zendesk_ticket(ticket_id, status):
# Zendesk API call here
pass


if __name__ == "__main__":
app.run(port=5000)


```


This webhook pattern works with virtually any no-code agent builder. The agent collects information from the customer, structures it as JSON, and sends it to your endpoint. Your code handles the business logic and returns a result the agent can relay to the customer.


For a deeper comparison of no-code options, see the[no-code agent builder platforms](https://www.sigmamind.ai/blog/best-no-code-agent-builder-platforms) breakdown.


Explore[pre-built integrations](https://www.sigmamind.ai/app-library) in the App Library to see how tool calling works without custom webhooks.


## Key Components of AI Agent Builder Code


Regardless of the tier you choose, every AI agent builder orchestrates five core primitives. Understanding them matters because, as one widely cited benchmarking study found, the same LLM model[scores 17 problems apart](https://www.morph.so/blog/best-ai-coding-agents-2026/) in different agent scaffoldings. The builder layer matters as much as the model.


### 1. LLM / Reasoning Engine


The model that decides what to do next. Model-agnostic platforms let you swap providers based on cost, quality, and latency. Here’s how you set up model switching in code:


```text
# Model-agnostic pattern: swap LLMs without changing agent logic
from langchain_openai import ChatOpenAI
from langchain_anthropic import ChatAnthropic
from langchain_google_genai import ChatGoogleGenerativeAI


def get_model(provider: str, use_case: str = "general"):
"""Select model based on provider preference and use case."""
configs = {
"openai": {
"general": ChatOpenAI(model="gpt-4o", temperature=0),
"fast": ChatOpenAI(model="gpt-4o-mini", temperature=0),
},
"anthropic": {
"general": ChatAnthropic(model="claude-sonnet-4-20250514", temperature=0),
"fast": ChatAnthropic(model="claude-haiku-4-20250514", temperature=0),
},
"google": {
"general": ChatGoogleGenerativeAI(model="gemini-2.0-flash"),
"fast": ChatGoogleGenerativeAI(model="gemini-2.0-flash-lite"),
},
}
return configs[provider][use_case]


# Use the same agent code regardless of which model you pick
model = get_model("anthropic", "general")


```


### 2. Tool Calling and Function Calling


How the agent interacts with external systems. Without tool calling, an agent can only talk. With it, the agent can process refunds, update addresses, book appointments. Here’s a structured pattern:


```text
from langchain_core.tools import tool
from pydantic import BaseModel, Field


# Structured input validation with Pydantic
class RefundInput(BaseModel):
order_id: str = Field(description="The order ID, e.g. ORD-1234")
reason: str = Field(description="Reason for the refund")
amount: float | None = Field(default=None, description="Partial refund amount, or None for full refund")


@tool(args_schema=RefundInput)
def process_refund(order_id: str, reason: str, amount: float | None = None) -> str:
"""Process a refund for a customer order. Use this when a customer
requests their money back for a defective or missing item."""


# Validate the order exists
# Call your payment processor
# Update your helpdesk
# Return confirmation


return f"Refund {'$' + str(amount) if amount else 'full amount'} processed for {order_id}"


@tool
def check_inventory(product_sku: str) -> str:
"""Check if a product is currently in stock for replacement shipments."""
# Your inventory API call
return f"SKU {product_sku}: 47 units in stock, ships within 1 business day"


@tool
def schedule_callback(
customer_phone: str,
preferred_time: str,
topic: str
) -> str:
"""Schedule a callback from a human agent for complex issues."""
# Your scheduling API call
return f"Callback scheduled for {preferred_time}. Reference: CB-{hash(customer_phone) % 10000}"


```


### 3. Memory


Agents need short-term memory (conversation context) and long-term memory (learning from past interactions). Here’s how different frameworks handle it:


```text
# LangGraph: Graph state persists across turns
from langgraph.checkpoint.memory import MemorySaver
from langgraph.checkpoint.postgres import PostgresSaver


# In-memory (development)
memory = MemorySaver()


# PostgreSQL (production)
# memory = PostgresSaver(conn_string="postgresql://user:pass@localhost/agents")


# CrewAI: Built-in memory modules
from crewai import Crew


crew = Crew(
agents=[...],
tasks=[...],
memory=True,  # Enables short-term, long-term, and entity memory
verbose=True,
)


# Custom long-term memory with a vector store
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import Chroma


embeddings = OpenAIEmbeddings()
long_term_memory = Chroma(
collection_name="customer_interactions",
embedding_function=embeddings,
persist_directory="./memory_store"
)


# Store a completed interaction
long_term_memory.add_texts(
texts=["Customer Jane (jane@example.com) prefers email communication. "
"Has had 2 previous refunds. VIP tier. Sensitive about shipping delays."],
metadatas=[{"customer_id": "cust_123", "date": "2025-12-01"}]
)


# Retrieve relevant context for a new interaction
relevant_history = long_term_memory.similarity_search(
"jane@example.com shipping complaint", k=3
)


```


### 4. Orchestration


How multi-step workflows execute. The patterns: sequential, parallel, graph-based, and role-based.


```text
# Parallel execution: run multiple tools simultaneously
import asyncio
from langchain_core.tools import tool


@tool
async def check_order_status(order_id: str) -> str:
"""Check order shipping status."""
await asyncio.sleep(0.5)  # Simulating API call
return f"Order {order_id}: Shipped, arriving Dec 5"


@tool
async def check_payment_status(order_id: str) -> str:
"""Check payment and refund history."""
await asyncio.sleep(0.3)  # Simulating API call
return f"Order {order_id}: Paid $49.99, no refunds"


@tool
async def get_customer_history(customer_id: str) -> str:
"""Get customer interaction history."""
await asyncio.sleep(0.4)  # Simulating API call
return f"Customer {customer_id}: 12 orders, 1 return, VIP status"


# Run all three lookups in parallel instead of sequentially
async def gather_context(order_id: str, customer_id: str):
results = await asyncio.gather(
check_order_status.ainvoke({"order_id": order_id}),
check_payment_status.ainvoke({"order_id": order_id}),
get_customer_history.ainvoke({"customer_id": customer_id}),
)
return "\n".join(results)


# This takes ~500ms instead of ~1200ms sequentially
context = asyncio.run(gather_context("ORD-1234", "cust_456"))


```


### 5. Observability


Logging, tracing, and cost tracking. This is where most teams underinvest.


```text
# Basic observability wrapper for any agent
import time
import json
from dataclasses import dataclass, field, asdict


@dataclass
class AgentRunMetrics:
run_id: str
start_time: float = field(default_factory=time.time)
end_time: float = 0
total_tokens: int = 0
tool_calls: list = field(default_factory=list)
errors: list = field(default_factory=list)
estimated_cost_usd: float = 0


def log_tool_call(self, tool_name: str, duration_ms: float, success: bool):
self.tool_calls.append({
"tool": tool_name,
"duration_ms": duration_ms,
"success": success,
"timestamp": time.time()
})


def finalize(self):
self.end_time = time.time()
duration = self.end_time - self.start_time
print(json.dumps({
"run_id": self.run_id,
"duration_seconds": round(duration, 2),
"total_tokens": self.total_tokens,
"tool_calls_count": len(self.tool_calls),
"errors_count": len(self.errors),
"estimated_cost_usd": round(self.estimated_cost_usd, 4),
}, indent=2))


# Usage
metrics = AgentRunMetrics(run_id="run_abc123")
# ... run your agent, log tool calls along the way ...
metrics.log_tool_call("lookup_order", duration_ms=145, success=True)
metrics.log_tool_call("process_refund", duration_ms=320, success=True)
metrics.total_tokens = 2847
metrics.estimated_cost_usd = 0.034
metrics.finalize()


```


You can also[test and debug agents](https://www.sigmamind.ai/playground) in real time using node-level logs in the SigmaMind Playground, which provides visual observability without writing custom logging code.


## AI Agent Builder Code for Voice AI


Voice AI agents add layers of complexity that text-based agent builders don’t face. On top of the five core components, voice agents must handle speech-to-text (STT), text-to-speech (TTS), telephony integration, and latency management. Voice conversations are brutally unforgiving of delays; anything above roughly one second feels unnatural. Sub-800ms voice-to-voice latency is the target most production teams aim for.


### Code-First Voice Agent Setup


If you’re building voice agents from raw APIs and SDKs, here’s the architecture and wiring:


```text
# Voice agent pipeline: STT -> LLM -> TTS -> Telephony
# This shows the conceptual wiring; each provider has its own SDK


import asyncio
import websockets
from deepgram import DeepgramClient, LiveTranscriptionEvents  # STT
from openai import AsyncOpenAI  # LLM
from elevenlabs import ElevenLabs  # TTS


class VoiceAgentPipeline:
def __init__(self):
self.stt_client = DeepgramClient(api_key="your-deepgram-key")
self.llm_client = AsyncOpenAI(api_key="your-openai-key")
self.tts_client = ElevenLabs(api_key="your-elevenlabs-key")
self.conversation_history = []


async def process_audio_chunk(self, audio_bytes: bytes) -> bytes:
"""Full pipeline: audio in -> audio out"""


# Step 1: Speech-to-Text (target: <200ms)
transcript = await self.transcribe(audio_bytes)
if not transcript:
return b""


# Step 2: LLM reasoning + tool calling (target: <400ms)
self.conversation_history.append({"role": "user", "content": transcript})
response = await self.llm_client.chat.completions.create(
model="gpt-4o-mini",  # Use smaller model for speed
messages=[
{"role": "system", "content": "You are a phone support agent. "
"Keep responses under 2 sentences. Be direct."},
*self.conversation_history
],
tools=[
{
"type": "function",
"function": {
"name": "lookup_order",
"description": "Look up order status",
"parameters": {
"type": "object",
"properties": {
"order_id": {"type": "string"}
},
"required": ["order_id"]
}
}
},
{
"type": "function",
"function": {
"name": "transfer_to_human",
"description": "Transfer call to human agent with context",
"parameters": {
"type": "object",
"properties": {
"reason": {"type": "string"},
"summary": {"type": "string"}
},
"required": ["reason", "summary"]
}
}
}
],
)


reply_text = response.choices[0].message.content
self.conversation_history.append({"role": "assistant", "content": reply_text})


# Step 3: Text-to-Speech (target: <200ms to first byte)
audio_response = self.tts_client.text_to_speech.convert(
text=reply_text,
voice_id="your-voice-id",
model_id="eleven_turbo_v2_5",
output_format="pcm_16000",
)


return audio_response


async def transcribe(self, audio_bytes: bytes) -> str:
# Deepgram streaming transcription
# Implementation depends on your telephony setup
pass


```


This is simplified. A production voice agent also needs barge-in detection (the caller interrupts the agent mid-sentence), DTMF tone handling, silence detection, and proper SIP session management. That’s a lot of infrastructure code.


### The Code vs. No-Code Decision for Voice


A practical framework from voice AI practitioners: pick code-first if **all** of the following are true:


- You have a developer who can dedicate 40+ hours to the first build
- Your use case has unique edge cases that no-code templates don’t cover
- You need maximum flexibility on which LLM and TTS to use
- You expect to ship five or more different voice agents over the next 12 months
- You need fine-grained control over latency and call handling


If those conditions don’t all apply, a no-code or low-code voice AI platform will get you to production faster.


### SigmaMind: No-Code Voice Agents with API Extensibility


SigmaMind’s[Agent Builder](https://www.sigmamind.ai/agent-builder) sits at this intersection: a no-code builder with model-agnostic orchestration across STT, LLM, and TTS layers. You pick the best provider for each layer independently. To extend it with custom logic, you wire up webhooks and API tools:


```text
# Webhook endpoint that a SigmaMind voice agent calls mid-conversation
# when it needs to execute custom business logic


from flask import Flask, request, jsonify


app = Flask(__name__)


@app.route("/sigmamind-webhook/order-lookup", methods=["POST"])
def order_lookup():
"""
SigmaMind sends structured data extracted from the voice conversation.
You return data the agent speaks back to the caller.
"""
payload = request.json


# The agent extracted these from the conversation
order_id = payload.get("order_id")
customer_phone = payload.get("caller_phone")


# Your business logic
order = db.query("SELECT * FROM orders WHERE id = %s", order_id)


if not order:
return jsonify({
"status": "not_found",
"response": "I couldn't find that order number. Could you double-check it?"
})


return jsonify({
"status": "found",
"order_status": order["status"],
"tracking_number": order["tracking"],
"estimated_delivery": order["eta"],
"response": f"Your order is {order['status']}. "
f"Tracking number is {order['tracking']}. "
f"Expected delivery is {order['eta']}."
})


@app.route("/sigmamind-webhook/warm-transfer", methods=["POST"])
def prepare_warm_transfer():
"""
Before transferring to a human agent, prepare context headers
so the human doesn't ask the customer to repeat everything.
"""
payload = request.json


return jsonify({
"transfer_to": "+14155551234",
"custom_headers": {
"X-Customer-Name": payload.get("customer_name"),
"X-Order-ID": payload.get("order_id"),
"X-Issue-Summary": payload.get("conversation_summary"),
"X-AI-Recommendation": payload.get("suggested_resolution"),
}
})


if __name__ == "__main__":
app.run(port=5000)


```


The platform supports built-in US telephony or[BYOC via SIP](https://www.sigmamind.ai/blog/how-to-configure-sip-with-twilio-and-telnyx-for-byoc) with Twilio and Telnyx, warm transfer with structured context headers, and sub-800ms latency targets.


[See transparent per-layer pricing](https://www.sigmamind.ai/pricing) to understand exactly what each component costs.


## Putting It Together: End-to-End Example


Here’s a complete, runnable example that ties the concepts together. This LangGraph agent handles customer support with tool calling, memory, and basic observability:


```text
# Complete runnable example: Customer support agent with LangGraph
# pip install langgraph langchain-openai langchain-core


import os
import time
from langchain_openai import ChatOpenAI
from langchain_core.tools import tool
from langchain_core.messages import HumanMessage
from langgraph.prebuilt import create_react_agent
from langgraph.checkpoint.memory import MemorySaver


os.environ["OPENAI_API_KEY"] = "sk-your-key-here"


# ---- Define Tools ----


@tool
def lookup_order(order_id: str) -> str:
"""Look up order details by order ID. Returns status, items, and tracking info."""
orders = {
"ORD-1001": {
"status": "delivered",
"items": ["Wireless Headphones"],
"total": 79.99,
"tracking": "1Z999AA10123456784",
"delivered_date": "2025-11-28"
},
"ORD-1002": {
"status": "in_transit",
"items": ["USB-C Hub", "Phone Case"],
"total": 45.50,
"tracking": "1Z999AA10987654321",
"eta": "2025-12-05"
},
}
result = orders.get(order_id)
if result:
return str(result)
return f"No order found for {order_id}. Ask the customer to verify the order number."


@tool
def process_refund(order_id: str, reason: str) -> str:
"""Process a refund for an order. Only use after confirming with the customer."""
return (
f"Refund processed for {order_id}. "
f"Reason logged: {reason}. "
f"Confirmation code: REF-{abs(hash(order_id)) % 100000}. "
f"Customer will see the credit in 3-5 business days."
)


@tool
def transfer_to_human(reason: str, conversation_summary: str) -> str:
"""Transfer the customer to a human agent. Use for complex issues
you can't resolve, or when the customer explicitly asks for a human."""
return (
f"Transferring to human agent. "
f"Context passed: {conversation_summary}. "
f"Reason: {reason}"
)


# ---- Create Agent ----


model = ChatOpenAI(model="gpt-4o", temperature=0)
memory = MemorySaver()


agent = create_react_agent(
model,
tools=[lookup_order, process_refund, transfer_to_human],
checkpointer=memory,
prompt=(
"You are a customer support agent for an online electronics store.\n\n"
"Guidelines:\n"
"- Always look up the order before making any changes\n"
"- Confirm refund details with the customer before processing\n"
"- Keep responses concise and friendly\n"
"- Transfer to a human if the issue involves: legal matters, "
"disputes over $200, or if the customer asks 3+ times\n"
"- Never make up order information. If you can't find it, say so."
),
)


# ---- Run a Multi-Turn Conversation ----


config = {"configurable": {"thread_id": "session-demo-001"}}


conversations = [
"Hi, I need help with my recent order ORD-1001",
"The headphones arrived but they're defective. The left ear doesn't work.",
"Yes, please process the refund. That would be great.",
]


for user_msg in conversations:
print(f"\n{'='*60}")
print(f"Customer: {user_msg}")
print(f"{'='*60}")


start = time.time()
response = agent.invoke(
{"messages": [HumanMessage(content=user_msg)]},
config=config,
)
elapsed = time.time() - start


# Print only the agent's latest response
ai_messages = [m for m in response["messages"] if m.type == "ai" and m.content]
if ai_messages:
print(f"\nAgent: {ai_messages[-1].content}")


# Print any tool calls made
tool_messages = [m for m in response["messages"] if m.type == "tool"]
if tool_messages:
print(f"\n[Tools called: {', '.join(m.name for m in tool_messages)}]")


print(f"[Response time: {elapsed:.2f}s]")


```


Save this as` support_agent.py` , set your API key, and run it. You’ll see the agent look up the order, confirm details with the customer, and process the refund across a multi-turn conversation with memory.


## How to Choose the Right Approach


The AI agent market hit $7.84 billion in 2025 and is projected to reach $52.62 billion by 2030. But Gartner expects 40% of agent projects to be scrapped by 2027. The difference between success and failure often comes down to picking the right tier.


### Four Questions That Determine Your Tier


**1. What’s your team’s technical skill?**
If you have Python developers who understand API architectures, code-first gives you maximum control. If your team is operations-focused, no-code or low-code keeps you from being blocked by engineering backlogs.


**2. How complex is the workflow?**
Single-purpose agents (FAQ, appointment scheduling, lead qualification) work well in no-code. Multi-agent systems with conditional branching, external data lookups, and custom reasoning need code.


**3. What are your compliance requirements?**
Regulated industries often need detailed audit trails, custom data handling, and control over exactly what data flows where. Code-first frameworks provide this. Some no-code platforms are catching up, but verify before committing.


**4. How many agents will you run, and at what scale?**
A single agent handling 50 calls a day has different infrastructure needs than 20 agents handling thousands of concurrent sessions.


### Common Mistakes


**Starting too complex.** Teams choose LangGraph when CrewAI (or even a no-code builder) would handle 90% of their needs. One practitioner’s production failure story circulating on Reddit captures this: “Forty seconds into the demo, a user asked a follow-up question. The agent called the same API three times, hallucinated a refund policy we didn’t have, then got stuck in a loop asking for clarification it already had.” The framework you choose determines failure modes you won’t see until production.


**Ignoring integration complexity.** Practitioners on Reddit consistently report that plugging agents into existing enterprise and legacy systems is often more difficult than building the AI agent itself. Budget twice the time you think you need for integrations.


**Skipping observability.** If you can’t see what your agent is doing, why it made a decision, and what it cost, you can’t improve it.


For a ranked comparison of platforms across all tiers, check out the[best AI agent builder platforms](https://www.sigmamind.ai/blog/best-ai-agent-builder-platforms) list.


## Frequently Asked Questions


### Do I need to know how to code to build an AI agent?


No. No-code platforms let operations and support teams build functional agents using visual interfaces. However, complex multi-agent systems, custom integrations, and performance-critical applications still benefit from code. The right question isn’t “do I need code?” but “does my use case need code?”


### What programming language is best for AI agents?


Python dominates. Nearly every major framework (LangGraph, CrewAI, Claude Agent SDK, Google ADK, Pydantic AI) is Python-first. TypeScript is a growing second option, particularly for JavaScript-heavy stacks.


### What’s the difference between an agent builder and an agent framework?


An agent framework (LangGraph, CrewAI) is a code library. You write code against its APIs. An agent builder is a platform with a visual interface that may use frameworks under the hood but abstracts them away. Frameworks are ingredients; builders are the pre-equipped kitchen.


### Can I build a voice AI agent without code?


Yes. Platforms like SigmaMind provide no-code agent builders purpose-built for voice, with model-agnostic STT/LLM/TTS selection, built-in telephony, and warm transfer. The[step-by-step voice agent guide](https://www.sigmamind.ai/blog/how-to-build-a-voice-ai-agent-without-code) walks through the entire process.


### How much does it cost to build an AI agent with code vs. no-code?


A LangChain-based agent typically costs $0.02 to $0.08 per conversation in API fees. At 500 daily conversations, that’s $300 to $1,200 per month before developer time. No-code platforms charge platform fees plus per-use costs. SigmaMind charges $0.03 per minute for voice plus provider costs for STT, TTS, LLM, and telephony.


### What is “agent washing” and why does it matter?


Agent washing is labeling simple chatbots or automation scripts as “AI agents” for marketing purposes. A true AI agent reasons across multiple steps, makes decisions, uses tools, and completes end-to-end tasks. When evaluating any platform, ask whether it supports genuine multi-step reasoning or just pattern-matched responses.


### How do I know when to migrate from no-code to code?


Common signals: you’re hitting platform limitations on conditional logic, you need custom integrations the platform doesn’t support, latency requirements exceed what the platform can deliver, or you need audit-level control over agent decisions. Start no-code for validation, then move specific high-complexity workflows to code.


### What are MCP and A2A, and why do they matter for agent builder code?


MCP (Model Context Protocol) standardizes how agents connect to external tools and data sources. A2A (Agent-to-Agent) protocols standardize how agents communicate with each other. Both reduce integration friction. The Claude Agent SDK has native MCP support, and other frameworks are adopting it. These protocols will increasingly determine which agents can interoperate in multi-agent systems.


---


Ready to build your first voice AI agent?[Start building for free](https://www.sigmamind.ai/sign-up) with SigmaMind’s no-code Agent Builder and pay only for what you use.
