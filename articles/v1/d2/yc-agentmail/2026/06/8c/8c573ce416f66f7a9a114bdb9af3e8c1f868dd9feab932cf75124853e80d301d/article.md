---
schema_version: "1.0.0"
document_id: "8c573ce416f66f7a9a114bdb9af3e8c1f868dd9feab932cf75124853e80d301d"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/give-your-langchain-agent-a-real-inbox"
published_at: "2026-06-13T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T21:44:22.050751+00:00"
content_hash: "sha256:680e8a830db89adca61c76a49136fd14b0d95f7b8f7dcb1b04c7da67a4238baf"
---

# Give your LangChain agent a real inbox

AgentMail's official LangChain integration is live. One` pip install` gives any LangGraph agent its own inbox it can send from, reply in, search, and react to.


## The problem


Your agent can reason and call tools, but it can't receive anything.


That's fine until it hits the real world. The agent tries to sign up for a service and the confirmation email goes nowhere. It needs an OTP and there's no inbox to read it from. Someone replies to a message it sent and the reply vanishes. Most "email for agents" is send-only (SendGrid, Mailgun, raw SMTP), which covers the easy half. The half that matters is receiving: catching the verification code, reading the reply, reacting to what lands.


You can build that yourself. Stand up an IMAP client, parse MIME, dedupe threads, verify webhook signatures, keep an address alive across runs. That's a few hundred lines of plumbing before your agent sends its first email.


## Why LangChain


You already build the agent here. LangChain is the default framework for wiring up tools, memory, and control flow, and LangGraph is where you run the multi-step logic. You're not looking for a new place to write agent code. You have one.


What you want is for the inbox to show up as tools inside the graph you already have.


## Why AgentMail


AgentMail is an inbox as an API. One call gives an agent its own address that can send and receive. The address persists across runs, so an agent that gets logged out signs back in instead of starting over. A webhook fires the moment a message arrives, so you react to inbound mail instead of polling for it.


The receive side is the point. It's what lets an agent get through a signup wall, read its own OTP, and pick a thread back up next week.


## Why the union matters


` langchain-agentmail` wraps the AgentMail SDK as standard LangChain tools, plus a document loader and a retriever. The plumbing from the first section (IMAP, MIME parsing, threading, webhook verification) is gone. You install the package and the inbox becomes a toolkit your LangGraph agent already knows how to call.


```text
pip install langchain-agentmail
export AGENTMAIL_API_KEY="your-api-key"
```


## What you can do with it


Four use cases, straight from the[integration docs](https://docs.agentmail.to/integrations/langchain) .


**Give agents their own inboxes.** Provision a dedicated address per agent so it can send and receive on its own. The toolkit hands the model one tool per operation: create an inbox, list and read threads, send, reply, label, draft.


**Triage and reply.** Read recent threads, summarize what's new, and reply inside the same thread with the right` In-Reply-To` headers. Hand the whole toolkit to a ReAct agent and that's a few lines:


```text
from langchain.chat_models import init_chat_model
from langgraph.prebuilt import create_react_agent
from langchain_agentmail import AgentMailToolkit


model = init_chat_model(model="claude-sonnet-4-6", model_provider="anthropic")
agent = create_react_agent(model, AgentMailToolkit.from_api_key().get_tools())


response = agent.invoke({
"messages": [(
"user",
"Check my inbox for anything new. Summarize the most recent thread in 2 sentences.",
)]
})
```


**Stage and schedule sends.** Use the draft tools to compose iteratively, revise, and ship, or set a delivery time with` send_at` :


```text
from langchain_agentmail import AgentMailCreateDraftTool


AgentMailCreateDraftTool().invoke({
"inbox_id": "ib_...",
"to": "alice@example.com",
"subject": "Following up",
"text": "Circling back on this.",
"send_at": "2026-06-20T09:00:00Z",  # omit to send on demand
})
```


**RAG over email.** Load messages as LangChain` Document` s and index them into a vector store for semantic search across the inbox. A bundled retriever does keyword search with no embeddings:


```text
from langchain_agentmail import AgentMailLoader, AgentMailRetriever


docs = AgentMailLoader(inbox_id="ib_...", labels=["inbox"], limit=50).load()


retriever = AgentMailRetriever(inbox_id="ib_...", k=5)
hits = retriever.invoke("invoice")
```


And to react to inbound mail instead of polling, the` webhooks` extra ships a FastAPI router with signature verification built in:


```text
from fastapi import FastAPI
from langchain_agentmail.webhooks import AgentMailEvent, create_fastapi_router


async def on_event(event: AgentMailEvent) -> None:
if event.event_type == "message.received":
# drive your LangGraph agent here
...


app = FastAPI()
app.include_router(create_fastapi_router(on_event), prefix="/agentmail")
```


## Try it


Grab a key from the[AgentMail Console](https://console.agentmail.to/) , then:


```text
pip install langchain-agentmail
```


The reference is on the[LangChain docs](https://docs.langchain.com/oss/python/integrations/providers/agentmail) , the package is on[PyPI](https://pypi.org/project/langchain-agentmail/) , and the source is on[GitHub](https://github.com/agentmail-to/langchain-agentmail) . Give the agent you already built an inbox and see what it does with it.


**[See the full LangChain integration docs at docs.agentmail.to →](https://docs.agentmail.to/integrations/langchain)**


AgentMail gives your agents real inboxes. Create inboxes via API. Send and receive Emails with 0 complexity. Free to start.


[Get Started](https://console.agentmail.to/)[Read the Docs](https://docs.agentmail.to/)
