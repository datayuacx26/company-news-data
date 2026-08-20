---
schema_version: "1.0.0"
document_id: "b958244a5e00c220782fe9de78541ea753a20fee4159e69efedbf6d16ae1040d"
company_key: "yc-agentic-fabriq"
company: "Agentic Fabriq"
source_id: "yc-agentic-fabriq-news-import-c3e20007c6cf"
canonical_url: "https://www.agenticfabriq.com/blog/designing-tools-agents-can-use"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T04:02:28.152001+00:00"
fetched_at: "2026-08-18T04:02:30.829740+00:00"
content_hash: "sha256:109dbf7800c29240dae1388dff277071948d7e8c52cdcf43f9c34d77e892770e"
---

# Designing Tools Agents Can Actually Use

## Overview


When an agent behaves badly, the instinct is to fix the prompt. Add a constraint, add an example, add a "CRITICAL: you MUST" line, run it again. Sometimes it helps. Often the real defect is one layer down: the model made a reasonable decision given a tool whose name did not say what it did, whose description assumed knowledge of a codebase the model has never seen, and whose parameters required it to guess a value it had no way to know.


A capable model with badly specified tools looks incompetent. The same model with well-specified tools looks careful. There is now direct evidence for how large that gap is. Anthropic reports that supplying worked input examples in tool definitions — a schema-side change, no prompt edits — moved accuracy on complex parameter handling from 72% to 90%, and their documentation is unusually blunt about where the leverage sits: detailed descriptions are "by far the most important factor in tool performance." A 2026 paper on rewriting tool descriptions puts it more sharply, observing that agent-side effort "increasingly plateaus due to the quality of the tool interfaces these agents consume."


The running example is a revenue-operations agent at a mid-sized software company. It answers questions from sales leadership by reading the CRM and a data warehouse, and can change a few fields on an opportunity record. Its first version had four tools and a 2,000-word system prompt explaining how to use them. Its second had six tools and a 300-word prompt, and worked considerably better.


**The rule this post argues for:** if the model is using a tool wrongly, fix the tool before you fix the prompt. The schema is read on every call, is cheaper to change, and generalizes across every prompt and every model you will run behind it.


## Naming


A tool name is the shortest description you will ever write, and the model reads it first. Names that describe a *mechanism* force the model to infer the *purpose* ; names that describe the purpose do not.


The agent's first version had a tool called` query` , wrapping a search endpoint that could hit accounts, contacts, or opportunities depending on an` entity` parameter. The model used it constantly and wrongly — sometimes passing SQL, sometimes a natural-language question, sometimes searching accounts when it wanted opportunities. None of that was unreasonable.` query` is a word that means six things.


```text
{ "name": "query", "description": "Query the CRM." }


```


Compare:


```text
{ "name": "crm_search_opportunities" }
{ "name": "crm_search_accounts" }
{ "name": "crm_get_opportunity" }


```


Three properties do the work. The **namespace prefix** says which system is being touched, which matters enormously once a second system appears —` crm_search_accounts` beside` warehouse_search_accounts` is an easy decision, while` search_accounts` beside` lookup_customers` is a coin flip. The **verb** separates retrieval from single-record fetch, operations with different cost profiles that a shared name hides. The **plural object** says what comes back.


Namespacing is not merely tidy. Anthropic's write-up on tool authoring reports that even the *shape* of the namespace is measurable: "we have found selecting between prefix- and suffix-based namespacing to have non-trivial effects on our tool-use evaluations." Service-based (` asana_search` ) and resource-based (` asana_projects_search` ) are both defensible; picking one consistently is what matters.


Two failure modes to name against: near-synonyms (` get_account` ,` fetch_account` ,` lookup_account` in one tool set will be confused, because they are confusable, and no description rescues them), and names encoding internal vocabulary (` get_sfdc_oppty_v2` names your schema migration, not the model's task). The same discipline applies inside the schema — Anthropic's guidance is specific that "instead of a parameter named` user` , try a parameter named` user_id` ."


## Descriptions


The description is where most of the available reliability sits unclaimed. It is not documentation for a developer who can read the implementation; it is the entire briefing for a reader who can see the schema and nothing else. Anthropic's framing is the most useful test available: "think of how you would describe your tool to a new hire on your team. Consider the context that you might implicitly bring — specialized query formats, definitions of niche terminology, relationships between underlying resources — and make it explicit." The published guidance gives a floor of three or four sentences, covering what the tool does, when it should be used *and when it should not* , what each parameter means, and what the tool does *not* return.


Before:


```text
{ "name": "crm_search_opportunities", "description": "Searches opportunities." }


```


After:


```text
{
"name": "crm_search_opportunities",
"description": "Search open and closed sales opportunities by account name, owner, stage, or close-date range. Returns up to 50 summary records (id, name, account, stage, amount, close date) ordered by close date descending. Use this to find opportunities when you do not already have an opportunity ID; use crm_get_opportunity instead when you do, because it returns the full record including line items and activity history. Amounts are in USD. This tool does not return contact details or forecast categories."
}


```


Four things the second version tells the model that the first does not: what it can search *by* , what comes back and in what order, **when to use a different tool instead** , and a unit convention it would otherwise guess. The third is the one people omit and the one that most reduces wrong-tool selection. Note also that "amounts are in USD" belongs here rather than in the system prompt: it is read where it is relevant, costs nothing on turns that never touch this tool, and stays correct when someone adds a second tool with a different convention.


One consequence of descriptions carrying this much behavioral weight: they are an instruction channel, and an unsigned, mutable one. Microsoft's incident-response team documented an attack in June 2026 that hides instructions in MCP tool metadata, noting that "the MCP blends instructions (tool descriptions) with data, so a change to a tool's metadata can redirect the agent's behavior." Review descriptions from sources you do not control with the same rigor as system prompts, and pin versions.


## Parameters


Parameter design has one governing principle: the model should never have to guess. Every parameter it cannot derive from the conversation is a coin flip, and every coin flip is a bug with a probability attached.


**Flat and explicit beats nested and clever.** A schema mirroring your internal request object is optimized for your convenience, not the model's accuracy.


```text
{
"filter": {
"type": "object",
"properties": {
"criteria": {
"type": "array",
"items": {"type": "object", "properties": {
"field": {"type": "string"}, "op": {"type": "string"}, "value": {}
}}
}
}
}
}


```


Expressive and nearly unusable: it requires the model to know your field names, your operator vocabulary, and your coercion rules, none of which are written anywhere it can see. The flat version encodes the same common cases and removes all three unknowns.


```text
{
"type": "object",
"properties": {
"account_name": {"type": "string", "description": "Exact or partial account name."},
"owner_email":  {"type": "string", "description": "Opportunity owner's work email."},
"stage": {
"type": "string",
"enum": ["prospecting", "qualification", "proposal",
"negotiation", "closed_won", "closed_lost"],
"description": "Restrict to one pipeline stage. Omit for all stages."
},
"closes_after":  {"type": "string", "description": "ISO date, e.g. 2026-09-01."},
"closes_before": {"type": "string", "description": "ISO date, e.g. 2026-12-31."},
"limit": {"type": "integer", "description": "Max records, 1-50. Default 20."}
},
"required": [],
"additionalProperties": false
}


```


**Enums over free strings** , wherever the set is closed. The` stage` enum removes a whole failure class: the model can no longer pass` "Proposal Sent"` and get an empty result it cannot diagnose. OpenAI's guidance frames the goal well — use types and structure to "make invalid states unrepresentable."


**Required means required.** A long` required` list is a design smell: usually the tool is really several tools, or it needs values the caller cannot supply. Conversely, marking something optional that the tool cannot work without produces a confident call and a 422. State defaults in descriptions.


**Do not ask for values the model cannot know.** The worst parameters are internal identifiers with no discovery path — a` tenant_id` , a` pricebook_id` . If the value is a property of the calling context, inject it at the boundary. A parameter the model must invent is a parameter it will invent.


Turn on strict validation where the platform offers it. With` strict: true` and` additionalProperties: false` , arguments are guaranteed to validate before your handler sees them, implemented by constraining token sampling to schema-valid outputs — which is why it is a guarantee rather than a retry. A tool needing` passengers: 2` can no longer receive` passengers: "two"` . Anthropic's implementation, unlike OpenAI's, does not require every property to be listed in` required` , so optional parameters survive.


**Figure 1 — The same task against two tools. The poorly specified one fails in the way a model cannot recover from: silently, with no signal about which guess was wrong.**


## Responses


Response design gets less attention than input design and causes at least as much trouble, because its failures are quiet. A tool that returns too much does not error — it fills the context window with noise, and the agent gets worse three steps later for reasons nobody attributes to the tool.


**Return only high-signal information.** A CRM opportunity record may have 180 fields; the model needs six. Anthropic's guidance goes further than "trim it" and says to strip low-level technical identifiers —` uuid` ,` 256px_image_url` ,` mime_type` — for semantically meaningful ones, with a striking claim attached: "merely resolving arbitrary alphanumeric UUIDs to more semantically meaningful and interpretable language (or even a 0-indexed ID scheme) significantly improves Claude's precision in retrieval tasks by reducing hallucinations." Opaque identifiers are tokens the model cannot reason about and will sometimes reconstruct incorrectly.


**Paginate, and make the continuation obvious.** The recommended levers are pagination, range selection, filtering, and truncation, each with sensible defaults.


```text
def search_opportunities(args, ctx, crm) -> dict:
rows, cursor = crm.search(**args, page_size=min(args.get("limit", 20), 50))
return {
"results": [summarize(r) for r in rows],
"returned": len(rows),
"next_cursor": cursor,                     # null when exhausted
"note": None if cursor is None else
"More results exist. Pass next_cursor to continue, or narrow "
"the filters instead if you already have enough.",
}


```


That hint matters more than it looks. Given a cursor and no guidance, models paginate exhaustively — which is how one question becomes forty tool calls. Telling the caller that narrowing is an option changes the behavior.


**Cap the size, and say when you truncated.** Silent truncation is worse than an error, because the model reasons confidently over a partial answer without knowing it is partial. Two Anthropic products converge on the same ceiling: Claude Code restricts tool responses to 25,000 tokens by default, and on the managed-agent runtime any output over 100,000 characters — about 25,000 tokens — is written to a file in the sandbox, with the model receiving a preview plus the path.


**Offer a verbosity control where it earns its place.** A response-format enum with` concise` and` detailed` lets the model take the expensive version only when needed — the published worked example renders the same result at 206 tokens detailed and 72 concise. Default to concise, and resist adding the knob everywhere.


## Errors as Instructions


An error is a message to a reader that can change its behavior. That reframing is the whole section.


A stack trace tells the model something failed and nothing about what to do next, so it retries or gives up vaguely. An error written as an instruction routinely produces a clean recovery on the next step. Anthropic's documentation is direct — "instead of generic errors like` failed` , include what went wrong and what Claude should try next" — and the MCP specification builds the distinction into the protocol, separating *protocol errors* (malformed requests, unknown tools: "issues with the request structure itself that models are less likely to be able to fix") from *tool execution errors* , which should "contain actionable feedback that language models can use to self-correct and retry with adjusted parameters."


Before:


```text
crm.client.CRMAPIError: (400, '{"errorCode":"MALFORMED_QUERY","message":"unexpected token: EMEA"}')


```


After:


```text
No results: 'region' is not a searchable field on this tool. Regional scope is
applied automatically from your access. To narrow by geography, filter on
account_name, or use warehouse_revenue_by_region. Retry once with corrected
arguments.


```


The second version carries four things the first does not: what was wrong, why the model could not have known, what to do instead, and whether retrying is appropriate. That last is the most valuable and most often missing — a model cannot distinguish transient from permanent, so it guesses, and it guesses "retry" far more often than it should. Anthropic notes Claude will typically retry an invalid tool call two or three times with corrections before apologizing, which is exactly the budget a good error message converts into a fix.


Class The model should Example wording


Bad arguments Fix and retry once "` stage` must be one of: … Retry with a valid value."


Not found Try a different identifier, or stop "No opportunity with that ID. Search for it first."


Not permitted Never retry; proceed without it "Not permitted for this user. Do not retry this tool."


Transient upstream Do not retry in-loop; report "The CRM is unavailable. Stop and report the outage."


Already applied Treat as success "Already recorded (idempotent replay). Continue."


Needs a human Park and report "Awaiting approval. Do not retry; report that it is pending."


Two rules go with the table. Never put an internal exception message in a tool result — uninformative to the model, and a small disclosure to anyone who can influence the transcript. And use the API's error flag (` is_error: true` , or` isError` in MCP) rather than returning an error-shaped success, so the platform and your telemetry both know what happened.


## Consequential Actions


Tools that change the world need properties read-only tools do not, and the schema is the right place for them because it is the part of the contract the model reads on every call.


There is a real tension to resolve first. Anthropic's documentation recommends consolidating related operations — grouping` create_pr` ,` review_pr` , and` merge_pr` behind one tool with an` action` parameter — because fewer, more capable tools reduce selection ambiguity. That is sound for selection and wrong for governance, because **the tool is the unit of gating** . On Anthropic's own managed-agent runtime, permission policies are configured per toolset and overridden per tool *name* :` always_allow` executes automatically,` always_ask` pauses for human approval. If` merge_pr` is an` action` value inside a mega-tool, there is no name to attach a policy to. The workable rule: consolidate freely among operations sharing a consequence class, never across one. Reads consolidate. Writes do not merge into reads, and irreversible writes do not merge into reversible ones. That runtime's defaults encode the same instinct — the first-party toolset defaults to` always_allow` while MCP toolsets default to` always_ask` , explicitly so that "new tools added to an MCP server do not execute in your application without approval."


MCP offers a vocabulary for declaring these properties —` ToolAnnotations` carries` readOnlyHint` ,` destructiveHint` ,` idempotentHint` , and` openWorldHint` , with pessimistic defaults:` destructiveHint` and` openWorldHint` are both` true` by default, so an unannotated tool is assumed dangerous. Use them, but read the caveat in the same specification: clients "MUST consider tool annotations to be untrusted unless they come from trusted servers." A design treating a server's self-declared` readOnlyHint` as an authorization decision has misplaced its trust boundary.


Enforcement belongs in your own schema and handler. Three patterns, offered as design guidance rather than vendor doctrine. **Idempotency keys on every write** — required, with a description saying how to derive them deterministically, and with the boundary recomputing the canonical key rather than trusting what arrived, so a model that regenerates it on retry cannot cause a duplicate. **A dry-run mode** where the action is expensive to reverse, which doubles as the first step of an approval workflow: the dry-run output is what the human reviews. And **a stated reason** , which costs the model nothing and attaches a human-readable justification to every audit record.


```text
{
"name": "crm_update_opportunity_stage",
"description": "Change the pipeline stage of one opportunity. This is visible to the account team and triggers forecast recalculation. Call with dry_run=true first to see the effect, then again with dry_run=false to apply. Derive idempotency_key as \"<opportunity_id>:<new_stage>\".",
"input_schema": {
"type": "object",
"properties": {
"opportunity_id": {"type": "string"},
"new_stage": {
"type": "string",
"enum": ["prospecting", "qualification", "proposal",
"negotiation", "closed_won", "closed_lost"]
},
"reason":  {"type": "string", "description": "One sentence, stored in the audit record."},
"dry_run": {"type": "boolean", "description": "If true, report the effect without applying it."},
"idempotency_key": {"type": "string"}
},
"required": ["opportunity_id", "new_stage", "reason", "dry_run", "idempotency_key"],
"additionalProperties": false
},
"strict": true
}


```


Making` dry_run` required rather than defaulted is deliberate: a required boolean forces the model to state intent explicitly, and explicit intent is something you can audit and gate. A defaulted one gets omitted.


One thing not to do: implement confirmation by having the tool return "are you sure?" and waiting for the model to call it again with` confirm: true` . The model will simply call it again with` confirm: true` . Confirmation is meaningful only when the confirming party sits outside the loop — a human, or a policy engine the model cannot address. The MCP specification says the same normatively: there "SHOULD always be a human in the loop with the ability to deny tool invocations."


**Consolidate within a consequence class, never across one.** Selection ambiguity is a reliability problem you can fix with better descriptions. A write hidden inside a read's schema is a governance problem you cannot fix at all.


## The Tool-Count Problem


Everything above assumes a handful of tools. The failure mode that scales worst is the one nobody designs for: what happens at fifty tools, or three hundred.


Two costs grow together. The first is mechanical: Anthropic measured a five-server setup — GitHub, Slack, Sentry, Grafana, Splunk — at 58 tools consuming roughly 55,000 tokens of definitions *before the conversation starts* , with GitHub alone about 26,000. That is a fixed tax on every request, paid whether or not any of those tools is relevant. The second is behavioral, and the vendors have converged on a remarkably narrow band for where it bites.


Anthropic states plainly that "Claude's ability to pick the right tool degrades once you exceed 30–50 available tools," and recommends adopting tool search at ten tools or 10,000 tokens of definitions. OpenAI recommends keeping fewer than 20 functions available at the start of a turn, described as a soft suggestion. Google's Gemini guidance says to keep the active set to 10–20 tools maximum. Three vendors, three independently derived numbers, all inside one order of magnitude.


The academic picture is more interesting than "more tools, worse accuracy," and getting it right changes which fix you reach for. A 2026 study introduced a chance-corrected metric for the question across registries from 20 to 3,251 tools and found that adaptive shortlisting holds coverage while presenting far fewer candidates: on BFCL, 370 tools, it reached 90.3% against the 90.8% of always showing fifty, while presenting seven on average, and its Claude Sonnet 4.6 validation showed 93.1% selection accuracy against 87.1% for a fixed five-tool list, widening to 76.8% against 60.9% on medium-difficulty queries where the right tool is present but not ranked first. Note the direction: a *too-small* fixed list is also a failure mode — on ToolBench's 3,251 tools a fixed shortlist of five wins on aggregate coverage, 64.7% against 61.9%, but finds *nothing* on hard queries, the ones where the correct tool ranks sixth to twentieth; searching deeper recovers 16.7% of them.


A second 2026 paper complicates the intuitive explanation. Examining where selection actually fails, it found the model attended most to the correct tool 80% of the time against a 21% chance baseline, and the correct tool was the under-attended one in only about 10% of failures — which "refutes the 'lost-in-the-middle' hypothesis about crowded tool lists." Reordering tools recovered at most 23% of failures; interventions at the decision step recovered 59–91%. The model is usually *finding* your tool and then failing to *pick* it. That is a discrimination problem, pointing at names and descriptions rather than list position, which is why the two halves of this post are the same argument.


The mitigations, roughly by leverage:


**Namespacing** is the cheapest, and it compounds with tool search because one query then matches a whole related group.


**Split agents by domain.** A tool set spanning finance, HR, and engineering is probably three agents rather than one with ninety tools. Each gets a smaller candidate set and a far simpler authorization story; where a task genuinely spans domains, a coordinator delegating to domain agents keeps every candidate set small.


**Progressive disclosure.** Present a small set and let the agent discover the rest on demand. Anthropic ships this as a first-class feature: tools marked for deferred loading are known to the request but kept out of context until a search surfaces them, and because schemas are appended as references rather than swapped in, the prompt cache survives — which is what makes it practical rather than merely clever. The effect is large in both dimensions: roughly 85% reduction in definition tokens (one worked example goes from about 77,000 to 8,700), *and* accuracy improvements on their MCP evaluations from 49% to 74% for Opus 4 and 79.5% to 88.1% for Opus 4.5. A related approach exposing tools as files an agent reads on demand illustrates the same effect with a worked example going from 150,000 tokens to 2,000. The token saving is the obvious win; the accuracy gain proves the tool count was hurting.


**Let scope intersection do some of the work** — free, if you already have an authorization boundary. The presented list should be the intersection of what the agent may do and what the current user may do, not the union of everything the platform can reach. The MCP specification explicitly permits this: the advertised tool set "MAY vary by the authorization presented on the request — for example, returning only the tools the caller's granted scopes permit." It improves accuracy and security simultaneously, which is rare.


One caution about the fashionable fix. Retrieval over a tool catalogue moves the problem rather than dissolving it, and the retrieval layer is itself attackable: a 2026 paper demonstrated injected tools whose metadata is positioned to "semantically span many user queries, dominate the top-k results, and push all benign tools out of the agent's context," reporting up to 95% attack success at a 1% injection rate. If tool search stands between your agent and three hundred tools, catalogue integrity is a security control.


Description quality and tool count are not independent, either: the description-rewriting work cited earlier reports cutting accuracy degradation by about 29% as catalogues scale past 150 candidates. Thirty well-differentiated tools beat twelve ambiguous ones. Anthropic's context-engineering guidance gives the cleanest test for whether you have a differentiation problem at all: "if a human engineer can't definitively say which tool should be used in a given situation, an AI agent can't be expected to do better."


separate different required params? -- yes --> separate one clear description covers both? -- no --> separate could a human engineer always say which to use? -- no --> separate -- yes --> one tool, optional params -->


**Figure 2 — Splitting heuristic. Consequence class always splits, because it is the unit of gating. Everything else is a differentiation question.**


## Built for Evaluation


The last property separates a tool set you can improve from one you can only argue about: tools you can test.


Agent behavior is measurable only if the environment is reproducible, and tools *are* the environment. τ-bench is the clearest exemplar — a deterministic database plus a simulated user, evaluated by "compar\[ing\] the database state at the end of a conversation with the annotated goal state." That design is what let it produce the field's most sobering reliability result: frontier function-calling agents succeeded on under half its tasks and were inconsistent across repeated trials, with` pass^8` under 25% in retail. A tool layer you cannot hold constant cannot produce that number at all.


**Make tools deterministic given fixed backing state.** A tool returning "the last 20 opportunities" returns something different every day, so a test against it fails for reasons unrelated to the agent. Either pin the backing store for evaluation runs, or make the time window an explicit parameter a test can fix. Anthropic's evaluation guidance adds the operational half: each trial should start from a clean environment, because "unnecessary shared state between runs (leftover files, cached data, resource exhaustion) can cause correlated failures."


**Make tools mockable at the schema boundary, not inside the client.** If the handler receives a client rather than constructing one, you can substitute a recorded fixture without touching the tool definition — the schema stays constant while the data varies.


```text
HANDLERS: dict[str, tuple[dict, callable]] = {}


def tool(schema: dict):
def register(fn):
HANDLERS[schema["name"]] = (schema, fn)
return fn
return register


@tool(SEARCH_OPPORTUNITIES)
def search_opportunities(args: dict, ctx, crm) -> dict:   # crm is injected
...


```


**Assert on state, not on prose — and not on trajectory.** Anthropic recommends deterministic graders wherever possible and LLM graders only where necessary, with an illustration that lands the point: "a flight-booking agent might say 'Your flight has been booked' at the end of the transcript, but the outcome is whether a reservation exists in the environment's SQL database." The same guidance warns against the other tempting shortcut — checking that the agent made a specific sequence of tool calls — as "too rigid," producing "overly brittle tests." Grade what the agent produced, not the path it took. The dividend is a suite you can maintain: a sprawling tool set produces one that is flaky for reasons nobody can isolate, which in practice means no suite at all.


## Conclusion


Tool design is not documentation and it is not plumbing. It is the interface through which a probabilistic system acts on a deterministic one, and nearly every property you want from that interaction — correct selection, correct arguments, bounded context, graceful recovery, safe writes, testability — is decided in the schema rather than the prompt. Name tools for purpose, with a consistent system prefix. Write descriptions for someone who cannot see your code, and say when *not* to use the tool. Keep parameters flat, use enums, never require a value the model cannot know. Return high-signal fields, paginate explicitly, say when you truncated. Write errors as instructions that state whether to retry. Put idempotency keys and dry-run modes on anything consequential, keep the confirming party outside the loop, and never merge a write into a read — the tool is the unit of gating. Then watch the tool count, because past roughly thirty to fifty tools it is the axis on which everything else silently degrades.


**The schema is read on every call; the prompt is written once.** That asymmetry is the argument. The published evidence points the same way from three directions — worked examples moving accuracy on complex parameter handling from 72% to 90%, deferred loading moving Opus 4 from 49% to 74%, rewritten descriptions cutting scaling degradation by about 29%. All three are schema changes. None is a better prompt.


## Sources


- [Writing effective tools for agents — with agents (Anthropic Engineering)](https://www.anthropic.com/engineering/writing-tools-for-agents)
- [Introducing advanced tool use on the Claude Developer Platform (Anthropic Engineering)](https://www.anthropic.com/engineering/advanced-tool-use)
- [Code execution with MCP: building more efficient agents (Anthropic Engineering)](https://www.anthropic.com/engineering/code-execution-with-mcp)
- [Effective context engineering for AI agents (Anthropic Engineering)](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Demystifying evals for AI agents (Anthropic Engineering)](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)
- [Define tools — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/define-tools)
- [Strict tool use — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/strict-tool-use)
- [Handle tool calls — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/handle-tool-calls)
- [Tool search tool — Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool)
- [Permission policies — Claude Managed Agents Docs](https://platform.claude.com/docs/en/managed-agents/permission-policies)
- [Tools — Model Context Protocol Specification (2026-07-28)](https://modelcontextprotocol.io/specification/2026-07-28/server/tools)
- [Function calling — OpenAI API documentation](https://developers.openai.com/api/docs/guides/function-calling)
- [Function calling with the Gemini API — Google AI for Developers](https://ai.google.dev/gemini-api/docs/function-calling)
- [How Many Tools Should an LLM Agent See? A Chance-Corrected Answer (arXiv:2605.24660)](https://arxiv.org/abs/2605.24660)
- [Looking Is Not Picking: An Attention-Segment Account of Tool-Selection Failures in LLM Agents (arXiv:2606.16364)](https://arxiv.org/abs/2606.16364)
- [Learning to Rewrite Tool Descriptions for Reliable LLM-Agent Tool Use (arXiv:2602.20426)](https://arxiv.org/abs/2602.20426)
- [ToolFlood: Beyond Selection — Hiding Valid Tools from LLM Agents via Semantic Covering (arXiv:2603.13950)](https://arxiv.org/abs/2603.13950)
- [τ-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains (arXiv:2406.12045)](https://arxiv.org/abs/2406.12045)
- [MCP-Universe: Benchmarking LLMs with Real-World Model Context Protocol Servers (arXiv:2508.14704)](https://arxiv.org/abs/2508.14704)
- [Securing AI agents: when AI tools move from reading to acting (Microsoft Security Blog, 30 June 2026)](https://www.microsoft.com/en-us/security/blog/2026/06/30/securing-ai-agents-ai-tools-move-from-reading-acting/)
