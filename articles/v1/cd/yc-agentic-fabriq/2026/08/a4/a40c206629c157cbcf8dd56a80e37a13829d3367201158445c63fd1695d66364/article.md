---
schema_version: "1.0.0"
document_id: "a40c206629c157cbcf8dd56a80e37a13829d3367201158445c63fd1695d66364"
company_key: "yc-agentic-fabriq"
company: "Agentic Fabriq"
source_id: "yc-agentic-fabriq-news-import-c3e20007c6cf"
canonical_url: "https://www.agenticfabriq.com/blog/build-your-first-production-agent"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-18T04:02:28.152001+00:00"
fetched_at: "2026-08-18T04:02:30.829740+00:00"
content_hash: "sha256:c3f39209735c471304124474fce673a8de30e2a12d3e955bc6b332a840897edb"
---

# Building Your First Production Agent: A Guide That Doesn't Skip Authorization

## Overview


Most "build an agent" tutorials follow the same arc. Import the SDK, define a tool that wraps an API client, put the API key in an environment variable, run the loop, watch the model call your tool. It works, and it takes about forty lines. Anthropic's own walkthrough is honest about this: its introductory loop is a bare` while response.stop_reason == "tool_use"` with no iteration counter, no timeout, and no error recovery, because those are not what the tutorial is teaching.


The problem is that the forty lines encode three decisions that are very hard to undo. The agent holds a credential directly, so nothing between the model and the API can narrow what a call may do. The tool set is defined once for all callers, so the agent's reach is the union of every user's reach rather than the intersection. And nothing records who asked for what, so the first time someone asks "why did the agent close that ticket," nobody knows. Retrofitting identity and authorization onto an agent built without them is not a refactor. It is a rewrite of every tool boundary you have.


OWASP named this precisely in its 2026 agentic top ten. "Identity and Privilege Abuse" (ASI03) arises from "the architectural mismatch between user-centric identity systems and agentic design," and without a distinct, governed identity of its own an agent "operates in an attribution gap that makes enforcing true least privilege impossible." That gap is not created by a security oversight later. It is created in the first commit, by the shape of the tool layer.


This guide builds the same simple agent and wires those things in from the start: an identity for the agent, an identity for the person it acts for, an authorization decision at the tool boundary, credentials injected at call time, and an audit event per tool call. The substrate is the Anthropic Messages API tool-use loop in Python, because it is the smallest complete thing that exercises every one of these concerns. Current model IDs are` claude-opus-5` ,` claude-sonnet-5` , and` claude-fable-5` .


Adjacent posts here cover different ground — "A Developer's Guide to Thinking in Agents, Not Apps" is mental models, "Agent Design Patterns" and "A Field Guide to Planning Algorithms for Agents" are architecture — but none is a build guide and none wires in authorization. Credential hygiene is covered separately in "Managing Secrets for AI Agents."


**The habit to build:** an agent does not hold permissions. It presents two identities and asks a boundary for a decision on every call. If that boundary exists in your first commit, everything you add afterward inherits it for free.


## The Scenario


Abstract agents produce abstract designs, so here is a concrete one. A mid-sized company's IT service desk takes a few hundred tickets a week. Most are password resets, VPN certificate problems, and questions already answered in the internal knowledge base. A support-triage agent reads an incoming ticket, searches the knowledge base for a matching article, and — when it finds a confident match — posts a reply on the ticket and tags it for review.


Three tools, deliberately different in kind:


- **` get_ticket`** — a read of a single record the requesting user is already entitled to see. Cheap, safe, idempotent.
- **` search_knowledge_base`** — retrieval across a corpus. Read-only, but the result set depends on who is asking: an article in the security team's private space should not surface for a helpdesk contractor.
- **` post_ticket_reply`** — a write the requester sees. Not idempotent, not reversible in any way that matters, and the only tool in the set where a wrong argument produces a real consequence.


That split is the right axis to design around, and there is evidence for it: τ-bench, the tool-agent-user benchmark, reports that in its retail domain task success declines monotonically as the number of database writes a task requires goes up. Side effects, specifically, are where agents come apart. Almost every governance decision falls out of the difference between the first two tools and the third, and a design that treats all three alike will eventually post something embarrassing to a customer.


The agent runs as a service. A helpdesk engineer triggers it from a queue view, so every run has a human on whose behalf it is acting — the normal enterprise case, and the one tutorials skip.


## The Agent Loop


The loop itself is unglamorous, which is a good sign. Send messages and tool definitions to the model. While` stop_reason == "tool_use"` , execute the requested tools, append the results, and send again. The loop exits on any other stop reason.


```text
# agent.py
import anthropic


client = anthropic.Anthropic()      # ANTHROPIC_API_KEY, or an `ant auth login` profile
MODEL = "claude-opus-5"
MAX_STEPS = 12                      # hard ceiling on loop iterations


SYSTEM = """You triage IT service-desk tickets.
Read the ticket, search the knowledge base, and post a reply only when an
article clearly answers the question. If nothing matches, say so and stop."""


```


Two wire-format rules govern how results go back, and both are enforced. Tool results must immediately follow the assistant message that requested them, with nothing interleaved; and within the user message carrying them, every` tool_result` block must come *before* any text block — text first is a 400. Return one result per` tool_use` block in a single user message: splitting them teaches the model to stop issuing parallel calls, quietly halving throughput on multi-lookup tasks.


```text
def run(user_message: str, ctx: "CallContext") -> str:
messages = [{"role": "user", "content": user_message}]


for _step in range(MAX_STEPS):
response = client.messages.create(
model=MODEL,
max_tokens=8192,
system=SYSTEM,
tools=tool_schemas_for(ctx),      # scoped per caller — see below
messages=messages,
)


if response.stop_reason == "pause_turn":
# A server-side tool hit its own loop cap. Echo the turn back as-is.
messages.append({"role": "assistant", "content": response.content})
continue
if response.stop_reason != "tool_use":
return summarize(response)        # end_turn, max_tokens, refusal, ...


messages.append({"role": "assistant", "content": response.content})
results = [
dispatch(block, ctx)              # never raises; see Recoverable Errors
for block in response.content
if block.type == "tool_use"
]
messages.append({"role": "user", "content": results})   # one message


return "Step budget exhausted before the task completed."


```


Three things about termination.` MAX_STEPS` stops a stuck agent from billing you all night; Anthropic's guidance on building effective agents names a maximum-iterations cap as a standard stopping condition, and the SDK tool runners expose` max_iterations` with **no default** — an unbounded loop is what you get unless you ask otherwise.


Second, check` stop_reason` before reading` response.content` . Safety classifiers can decline a request and return` refusal` with an HTTP 200 and an empty or partial content array; code that indexes` content\[0\]` breaks on it.


Third,` pause_turn` is narrower than usually described. It is not a client-loop budget signal — it means the *server-side* sampling loop that executes hosted tools such as web search reached its iteration limit, ten by default. The response may carry a` server_tool_use` block with no matching result, and the fix is to send the turn back unchanged. Adding a "please continue" message is the common mistake.


If you want the model to pace itself rather than simply be cut off, the API exposes a task budget: a token ceiling the model is aware of, injected as a server-side countdown, distinct from` max_tokens` , which it is not. Two caveats. It is a soft hint — the documentation is explicit that Claude may exceed it rather than abandon an action mid-flight — and the remaining budget appears nowhere in the response, so it is a steering mechanism rather than an observability one. The floor is 20,000 tokens, and the documented sizing method is to measure first and start at the p99 of per-task spend.


**Figure 1 — The loop, with the authorization checkpoint on the only path that reaches a real system.**


## Tool Definitions


A tool definition is a name, a description written for a reader who cannot see your codebase, and a JSON Schema for the inputs. The model selects tools almost entirely from those three fields — they are prompt engineering with a different file extension. Anthropic's documentation calls detailed descriptions "by far the most important factor in tool performance."


```text
GET_TICKET = {
"name": "get_ticket",
"description": (
"Fetch one service-desk ticket by numeric ID. Returns subject, body, "
"requester, status, and priority. Use this before anything else so "
"you are reasoning about the real ticket text rather than a "
"paraphrase of it. It does not return attachments or audit history."
),
"input_schema": {
"type": "object",
"properties": {
"ticket_id": {"type": "integer", "description": "e.g. 48213."}
},
"required": ["ticket_id"],
"additionalProperties": False,
},
"strict": True,
}


```


` strict: True` is worth the two seconds it costs. It constrains token sampling to schema-valid outputs, turning "the input should match the schema" into a guarantee rather than a hope — the difference between receiving` ticket_id: 48213` and occasionally receiving` "48213"` . It requires` additionalProperties: False` , which is good schema hygiene anyway.


The write tool is where the schema starts carrying safety semantics rather than just types.


```text
POST_REPLY = {
"name": "post_ticket_reply",
"description": (
"Post a public reply the requester will see, and tag the ticket for "
"human review. Use only when a knowledge-base article directly "
"answers the question. Never use it to ask for more information."
),
"input_schema": {
"type": "object",
"properties": {
"ticket_id": {"type": "integer"},
"body": {"type": "string", "description": "Plain prose, no markdown."},
"kb_article_id": {
"type": "string",
"description": "ID of the article this reply is based on.",
},
},
"required": ["ticket_id", "body", "kb_article_id"],
"additionalProperties": False,
},
"strict": True,
}


```


Requiring` kb_article_id` is a governance control disguised as a parameter: the agent cannot post a reply it cannot attribute to a source, which makes an ungrounded answer structurally awkward rather than merely discouraged. Note what is *not* in the schema — no idempotency key. The boundary derives that, for reasons we will come to.


## Two Identities


Here is the part the tutorials omit. Every tool call in an enterprise agent carries **two** identities, and both matter.


The **agent identity** answers "which piece of software is this?" It is stable, registered, and carries a scope set bounding what this agent may ever do — the triage agent may read tickets and post replies, and may not delete anything, ever, for anyone. The **user identity** answers "on whose behalf?" It is per-request and bounds what *this particular call* may do.


The authorization decision is the **intersection** . A helpdesk engineer who can read every ticket in the estate does not get an agent that can delete them, because the agent's scope set excludes deletion. An administrator running the same agent gets no more from it than the agent is permitted to do. And the agent, however capable, cannot reach a queue the current user has no business seeing.


OAuth 2.0 Token Exchange (RFC 8693) models exactly this shape at the token layer, and its vocabulary is worth borrowing even if you never implement the protocol. Section 4.1 defines the` act` claim, which "provides a means within a JWT to express that delegation has occurred and identify the acting party to whom authority has been delegated" — the user remains the token's subject while the agent appears as a nested actor. Section 4.4 defines` may_act` , which "makes a statement that one party is authorized to become the actor and act on behalf of another party." Section 1.1 draws the distinction that matters most: under impersonation, the acting party "is given all the rights that \[the subject\] has within some defined rights context and is indistinguishable from \[the subject\] in that context." Delegation keeps both identities legible. An agent that simply holds the user's token has chosen impersonation, and has thereby chosen to be unauditable.


Two consequences follow, and both are nearly free once a context object exists.


```text
from dataclasses import dataclass


@dataclass(frozen=True)
class CallContext:
agent_id: str                     # stable identity of this agent
agent_scopes: frozenset[str]
user_id: str                      # the human this run acts for
user_scopes: frozenset[str]
request_id: str                   # correlates every event in this run


@property
def effective_scopes(self) -> frozenset[str]:
return self.agent_scopes & self.user_scopes


```


First, the presented tool list becomes per-caller. There is no reason to describe a tool the caller could not invoke; offering it produces confident calls that get denied. This also shrinks the tool set, which is independently good — selection accuracy degrades as candidate counts grow, and definitions are a fixed token cost on every request.


```text
TOOL_SCOPES = {
"get_ticket":            "tickets:read",
"search_knowledge_base": "kb:read",
"post_ticket_reply":     "tickets:reply",
}
ALL_TOOLS = {t["name"]: t for t in (GET_TICKET, SEARCH_KB, POST_REPLY)}


def tool_schemas_for(ctx: CallContext) -> list[dict]:
return [s for n, s in ALL_TOOLS.items() if TOOL_SCOPES[n] in ctx.effective_scopes]


```


Second, the presented list is a convenience, never a control. The model can name a tool it was never given, and a prompt injection arriving through a ticket body can try to talk the loop into one. So the boundary re-checks on execution.


```text
class Denied(Exception): ...


def authorize(tool_name: str, ctx: CallContext) -> None:
required = TOOL_SCOPES.get(tool_name)
if required is None:
raise Denied(f"Unknown tool '{tool_name}'.")
if required not in ctx.agent_scopes:
raise Denied(f"Agent {ctx.agent_id} is not granted '{required}'.")
if required not in ctx.user_scopes:
raise Denied(f"User {ctx.user_id} is not granted '{required}'.")


```


Separating the two denial reasons is deliberate. "The agent cannot do this at all" and "you cannot do this" are different problems with different fixes; collapsing them costs an afternoon every time someone investigates.


Why code rather than prose in the system prompt? Because the temptation to write the rules into` SYSTEM` is strong and there is a measurement against it. τ-bench ablated the domain policy out of the system prompt and found the effect wildly inconsistent: in its airline domain task success collapsed from 33.2 to 10.8, while in retail it barely moved, from 61.2 to 56.8. The authors read the retail result as evidence that successful runs "mostly stem from using tools in an intuitive and common sense way, and that they may not actually be leveraging the policy documents to the extent possible." A rule the model may or may not be consulting is not a control. OWASP's version of the point is that planner output should be treated as untrusted and validated at a pre-execution policy enforcement point, with per-action authorization rather than one check at the start of a workflow.


That last clause names a bug that is easy to write. OWASP lists time-of-check-to-time-of-use among ASI03's vulnerability classes, with a scenario that reads like a postmortem: a procurement agent validates approval at the start of a purchase sequence, the user's spending limit is reduced hours later, and the workflow proceeds on the stale authorization to complete a now-unauthorized transaction. Authorizing once per run is natural and wrong. Authorize per call.


## Credentials at Call Time


The agent needs a service-desk token to do anything useful. It must never see one.


Anything in the model's context can be extracted from the model's context. A tool description, a system prompt, a tool result, a retrieved knowledge-base article — all of it is text the model reads, and all of it is reachable by a well-crafted injection in a ticket body. Anthropic's documentation carries the warning: tool results "often carry content from sources outside your control," should be treated as untrusted, and should be kept inside` tool_result` blocks rather than promoted into system prompts. The fix is not a better prompt. It is for the credential to live somewhere the reasoning loop cannot address, attached to the outbound request only after that request has been authorized.


```text
import httpx
from vault import fetch      # returns a short-lived token for (user, provider)


def service_desk_client(ctx: CallContext) -> httpx.Client:
"""Per-user credential, resolved at call time, never returned to the model."""
token = fetch(user_id=ctx.user_id, provider="service_desk")
return httpx.Client(
base_url="https://servicedesk.internal/api/v2",
headers={
"Authorization": f"Bearer {token}",
"X-Request-Id": ctx.request_id,
},
timeout=httpx.Timeout(10.0, connect=3.0),
)


```


The credential is keyed by user, not by agent. With a single service account, the downstream system's own logs attribute every action to the agent, and you lose the ability to answer "on whose behalf" at the only layer that is genuinely authoritative — the system that was actually changed. Per-user credentials mean the service desk's audit trail agrees with yours, and short lifetimes mean a captured token is a much smaller thing to lose than a standing key.


**Figure 2 — Both identities travel with the request; the credential joins it only after authorization and never returns to the model.**


## The Write Tool


The read tools and the write tool should not share a code path, because they do not share a failure profile. A wrong` get_ticket` costs a step; a wrong` post_ticket_reply` is visible to a requester forever. Four things the write handler does that the read handlers do not.


It **derives the idempotency key rather than accepting one.** The` Idempotency-Key` header is a de facto standard rather than a live one — the IETF draft specifying it expired without becoming an RFC — so build against your provider's contract. Stripe's is representative: keys up to 255 characters, accepted on all POST requests, the first response replayed for subsequent identical requests, an error if the same key arrives with different parameters, and pruning after roughly 24 hours. Note that Stripe replays saved` 500` s too — exactly the behavior you want, and exactly what a naive client-side retry defeats.


It **validates the grounding claim.** The cited article must be readable by this user and must have appeared in a search result earlier in the same run. Otherwise the agent has invented its source.


It **consults an approval policy** , returning a structured "awaiting approval" result so the run parks rather than fails. And it **writes the audit event before returning.** If the process dies between the API call and the log line, you want the log line to be the thing that already happened.


```text
import hashlib


def handle_post_reply(args: dict, ctx: CallContext) -> dict:
authorize("post_ticket_reply", ctx)


if not article_seen_in_run(args["kb_article_id"], ctx):
raise Denied("That article did not appear in this run's search results.")
if not article_visible_to(args["kb_article_id"], ctx):
raise Denied("Article not readable by this user; cannot cite it.")


# Derived, not supplied: stable across retries by construction.
key = hashlib.sha256(
f"{ctx.agent_id}:{args['ticket_id']}:{args['kb_article_id']}".encode()
).hexdigest()


if requires_approval(args, ctx):
approval = request_approval(args, ctx)          # non-blocking
return {
"status": "awaiting_approval",
"approval_id": approval.id,
"message": (
"This reply needs human approval before it is sent. "
"Do not retry; stop and report that approval is pending."
),
}


with service_desk_client(ctx) as api:
r = api.post(
f"/tickets/{args['ticket_id']}/replies",
json={"body": args["body"], "public": True},
headers={"Idempotency-Key": key},
)
r.raise_for_status()


return {"status": "posted", "reply_id": r.json()["id"]}


```


The` "Do not retry"` sentence is not decoration. The documentation notes Claude will typically retry an invalid tool call two or three times with corrections before giving up. Spend that budget on a fix, not a duplicate write.


**Design the write tool as if the model will call it twice.** Derived idempotency keys, server-side deduplication, and a result string that states whether a retry is appropriate are cheaper than a reconciliation script.


## Recoverable Errors


Tool results are the model's only feedback channel. An error returned as a stack trace tells the model something is broken; an error returned as an instruction tells it what to do instead. Anthropic's guidance is unambiguous — instead of generic errors like` "failed"` , "include what went wrong and what Claude should try next" — and the mechanic is a` tool_result` block carrying` is_error: true` .


One rule that is easy to miss: if you decide *not* to execute one of several requested tools, you must still return a result for it — a` tool_use` block with no matching` tool_result` is a protocol error. A skipped call becomes an explicit` is_error` result saying why.


```text
import json


def dispatch(block, ctx: CallContext) -> dict:
"""Never raises. Every outcome becomes a tool_result the model can act on."""
def result(content: str, is_error: bool = False) -> dict:
return {
"type": "tool_result",
"tool_use_id": block.id,
"content": content,
"is_error": is_error,
}
try:
payload = HANDLERS[block.name](block.input, ctx)
audit(ctx, block, outcome="ok")
return result(json.dumps(payload))
except Denied as e:
audit(ctx, block, outcome="denied", reason=str(e))
return result(
f"Not permitted: {e} Do not retry this tool. Continue with the "
f"tools you have, or explain what you could not do.",
is_error=True,
)
except httpx.HTTPStatusError as e:
audit(ctx, block, outcome="upstream_error", status=e.response.status_code)
return result(classify_http(e), is_error=True)
except Exception:
audit(ctx, block, outcome="internal_error")
return result(
"The tool failed for an internal reason. Do not retry. Report "
"that this step could not be completed.",
is_error=True,
)


```


Two habits make the difference. Never leak an internal exception message into a tool result — uninformative to the model, and a small disclosure to anyone who can influence the transcript. And always state whether a retry is appropriate, because the model cannot infer it.


```text
def classify_http(e: httpx.HTTPStatusError) -> str:
s = e.response.status_code
if s == 404:
return "That record does not exist. Check the ID or try a different one."
if s == 409:
return "Already applied (idempotent replay). Treat this as success."
if s == 422:
return f"Arguments rejected: {e.response.text[:200]}. Fix and retry once."
if s == 429:
return "Rate limited. Do not retry; report that the system is busy."
if s >= 500:
return "The upstream system is failing. Do not retry; stop and report."
return "The call was refused. Do not retry."


```


The audit event is emitted on every branch, including denials — the branch people forget. A record of what an agent *tried* to do and was stopped from doing is the single most useful thing in the log when you are establishing whether a boundary works.


```text
def audit(ctx: CallContext, block, **fields) -> None:
emit({
"ts": now_iso(),
"request_id": ctx.request_id,
"agent_id": ctx.agent_id,
"on_behalf_of": ctx.user_id,
"tool": block.name,
"arguments": redact(block.input),
"effective_scopes": sorted(ctx.effective_scopes),
**fields,
})


```


Both identities appear in every record. An audit trail naming only the agent tells you a robot did something; one naming the agent and the person answers the question anyone will actually ask. OWASP makes observability a first-class principle for this reason, arguing alongside "least agency" that "without clear visibility into what agents are doing, why they are doing it, and which tools they are invoking, unnecessary autonomy can quietly expand the attack surface." Regulated deployments make it concrete: under the EU AI Act, Article 12 requires high-risk systems to "technically allow for the automatic recording of events (logs) over the lifetime of the system" and Article 19 requires providers to retain them for "at least six months." Those obligations apply to standalone Annex III systems from 2 December 2027, following the deferral adopted in the Digital Omnibus on AI in June 2026 — which is a reason to build the logging in now rather than a reason to defer it, since the systems facing that deadline are the ones being written today. A log that cannot attribute an action to a human principal satisfies nobody's reading of it.


## What Breaks in Production


The agent above works. Here is what goes wrong once real traffic hits it.


**Runaway loops.** The classic shape is a tool that returns something the model reads as almost-success, so it adjusts one argument and tries again indefinitely.` MAX_STEPS` catches it, but the better instrument is a repeated-call detector: hash tool name plus arguments per run, and if the same hash recurs three times, say so in the tool result. Models are good at changing strategy when told they are repeating themselves and bad at noticing unaided.


**Retries on non-idempotent writes.** Your HTTP client retries on timeout. The write already succeeded; the response was lost. Now there are two replies on the ticket. A timed-out request is genuinely ambiguous and no care at the call site resolves it, which is why the answer must be server-side deduplication keyed on something stable — and why the boundary derives the key rather than trusting what the model produced on this attempt.


**Partial failures mid-workflow.** The agent posts the reply, then fails to tag the ticket. No transaction spans two API calls, so the honest options are to make each step independently idempotent and re-runnable, or to record intent before acting so a reconciliation pass can finish the job. Pretending the workflow is atomic is not an option, and this is the failure mode that most often turns a demo into an incident.


**Context growth.** Every tool result stays in the transcript, and quality degrades before the window does — Anthropic's context-engineering work calls this context rot, the observation that recall accuracy falls as token count rises. The tool-side mitigations are yours: return the fields that matter, paginate, cap result sizes. Platform-side, context editing clears stale tool results and compaction summarizes old turns; the announcement reported a 29% improvement from context editing alone, 39% combined with a memory tool, and an 84% reduction in token consumption on a 100-turn web-search evaluation. Those figures come from the blog post rather than the reference documentation and the harness is unpublished, so treat them as directional.


**The right tool with subtly wrong arguments.** This is the failure authorization catches and testing usually does not. The model correctly decides to post a reply, correctly composes it, and supplies the ticket ID of the *previous* ticket it looked at. Schema satisfied, scopes satisfied, wrong record. τ-bench quantifies the shape: in a hand-examined sample of retail failures roughly a fifth were wrong-argument errors, and its gpt-4o agent averaged 0.46 tool calls per task referencing user, product, order, or item IDs that did not exist at all. Defenses are narrow and specific — require arguments to be internally consistent, require approval where blast radius justifies it, and log the arguments so the mistake surfaces in minutes rather than at quarter end.


None of these are exotic. They are the ordinary failure modes of distributed systems, arriving through a component that generates its arguments probabilistically. That is precisely why the boundary matters: it is the one place where a non-probabilistic system gets to decide.


## Conclusion


The working agent here is not much longer than the tutorial version. What it has extra is a context object carrying two identities, an` authorize` call at the top of every handler, a credential resolved at call time rather than held in the process, a write tool deliberately unlike the read tools, error strings written for a model rather than a log aggregator, and one audit event per tool call including the denied ones.


Every one of those is easy on day one and expensive on day two hundred, because by then the tool layer has assumed their absence. The tool list that is not scoped per caller is used by four agents. The credential the process holds is baked into a deployment manifest. The handler that raises exceptions is called from three loops. Authorization is not a feature you bolt on; it is a property of where your tool boundary sits, and that gets decided in the first commit whether you think about it or not.


**Build the boundary before you need it.** An agent whose every tool call already names an agent, names a user, resolves a decision, borrows a credential, and leaves a record is an agent you can safely put more capability behind. One that skipped those is an agent you will eventually have to turn off.


## Sources


- [Tool use with Claude — overview (Anthropic)](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview)
- [Define tools — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools)
- [Handle tool calls — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/handle-tool-calls)
- [Parallel tool use — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/parallel-tool-use)
- [Strict tool use — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/strict-tool-use)
- [Handling stop reasons — Claude Docs](https://platform.claude.com/docs/en/build-with-claude/handling-stop-reasons)
- [Task budgets — Claude Docs](https://platform.claude.com/docs/en/build-with-claude/task-budgets)
- [Building Effective AI Agents (Anthropic Engineering)](https://www.anthropic.com/engineering/building-effective-agents)
- [Effective Context Engineering for AI Agents (Anthropic Engineering)](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Managing context on the Claude Developer Platform (Anthropic, 29 Sept 2025)](https://claude.com/blog/context-management)
- [RFC 8693: OAuth 2.0 Token Exchange](https://www.rfc-editor.org/rfc/rfc8693.html)
- [draft-klrc-aiagent-auth: AI Agent Authentication and Authorization (IETF)](https://datatracker.ietf.org/doc/draft-klrc-aiagent-auth/)
- [OWASP Top 10 for Agentic Applications 2026 (OWASP GenAI Security Project, Dec 2025)](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)
- [τ-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains (arXiv:2406.12045)](https://arxiv.org/abs/2406.12045)
- [EU AI Act Article 12: Record-Keeping](https://artificialintelligenceact.eu/article/12/)
- [EU AI Act Article 19: Automatically Generated Logs](https://artificialintelligenceact.eu/article/19/)
- [EU AI Act implementation timeline](https://artificialintelligenceact.eu/implementation-timeline/)
- [The Idempotency-Key HTTP Header Field (draft-ietf-httpapi-idempotency-key-header-07, expired)](https://datatracker.ietf.org/doc/draft-ietf-httpapi-idempotency-key-header/)
- [Stripe API: Idempotent requests](https://docs.stripe.com/api/idempotent_requests)
