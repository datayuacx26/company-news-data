---
schema_version: "1.0.0"
document_id: "29edf92b94f9c1551969d1ad3ddee040804d4adec231099ef5cb079b1b927339"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/cekura-for-agents"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-24T05:11:51.123297+00:00"
fetched_at: "2026-07-28T22:12:51.498055+00:00"
content_hash: "sha256:4422bac4911f7ab1b074a416bba5937fe53677486407e7f0d9a823282c1bf638"
---

# Cekura for Agents: MCP Server and Tools for Voice AI Testing

Cekura has an MCP server. Coding agents (Claude Code, OpenAI Codex, Cursor, and Windsurf) can trigger voice agent test runs, schedule recurring evals, and review pass/fail results without leaving their editor.


Agents fail less from missing capability than from missing product expertise at the interface layer. A growing share of the traffic hitting our API now comes from these coding agents acting on a person's behalf, and the interface they read is our MCP server, our skill packages, and the agent-readable surfaces of our docs.


## Agents Are a New Kind of User


Agents see your tool descriptions in their model context at the moment they're deciding which tool to call. The decision is made against the registered schema alone: no docs browsing mid-selection, no follow-up question to the user, no pattern-matching from a project they shipped last week. Whatever the description says is what determines whether a tool gets used at all.


That makes the interface to a product (not the product itself) the bottleneck. The rest of this post is what we changed at that interface, grouped by surface:


- **OAuth 2.1** so agents can connect without a pasted token.
- **Tool descriptions** that carry intent, not just call shape.
- **` llms.txt`** for context that doesn't fit in a tool schema.
- **Skills** , organized in layers, for workflows tools can't teach on their own.
- **MCP tools** that let you trigger voice agent test runs, schedule recurring evals, and review results without leaving your editor.


From any MCP-enabled editor, you can trigger scenario runs against your Retell, Vapi, or LiveKit agent, schedule recurring regression tests, and review pass/fail results. No browser or terminal required.


## OAuth 2.1 by Default


Authentication follows OAuth 2.1 with PKCE (RFC 7636) required on every flow. Discovery is mediated by the MCP endpoint itself:


- An unauthenticated call returns a Protected Resource Metadata pointer (RFC 9728).
- The client resolves the authorization server's metadata (RFC 8414).
- The consent flow opens in the user's browser; the agent receives a token without anyone editing a config file.


API keys remain available as a fallback for headless and CI workflows that can't run the handshake.


For Claude Code, Codex, and other MCP clients, this means connecting to Cekura requires no manual token setup. The coding agent handles the handshake.


- No token pasting required for Claude Code, Codex, or Cursor.
- API keys remain available for headless and CI workflows.
- Discovery is mediated by the MCP endpoint itself.


## Tool Descriptions That Carry Intent


Spec-derived descriptions tell the agent what a call does. They rarely tell it *when* to use the call, what to pair it with, or which of two similar tools to pick. We layer that intent guidance on top of every spec description.


What the guidance covers:


- When to reach for a tool (and when to prefer a close cousin).
- Which tools commonly pair with it.
- Which spec examples to surface, and which to suppress. One misleading example outweighs three correct sentences.
- Which operations are destructive, so the client can gate confirmation.


A before/after for a list endpoint that originally read just "List scenarios":


```text
List scenarios in the active project.


Use this when the user wants to find or run an existing evaluator. To
browse organizationally (by folder), use `scenarios_folders_list` instead.
Returns a slim projection by default; pass `detail=true` for the full
conditional-action and instruction text.


```


The "use this when / instead use" pattern is the lever. It turns a list of tools into a decision tree the agent can follow.


Because Cekura exposes these tools through MCP, a developer can trigger a voice agent test run from inside Claude Code or Cursor without opening a browser or terminal.


- Tool descriptions carry intent, not just call shape.
- "Use this when / instead use" pattern turns a tool list into a decision tree.
- Destructive operations are flagged so the client can gate confirmation.
- All tools are callable from Claude Code, Codex, and Cursor without leaving the editor.


## ` llms.txt` for the Long Tail


A tool description can only carry so much. For concepts, walk-throughs, and the full reference, we publish an[llms.txt](https://docs.cekura.ai/llms.txt) at the docs root — a flat, link-keyed index of every docs page, auto-generated from the sitemap. An agent that hits an unfamiliar concept can fetch context on demand instead of guessing.


This matters specifically for developer tools. AI coding assistants (Claude, Copilot, Cursor) actively consume` llms.txt` files when evaluating APIs and SDKs. If your documentation is not surfaced there, coding agents will skip you for a competitor whose docs they can read.


## Skills in Three Layers


The commands and skills below are the agent-facing surface Cekura exposes. A coding agent can call` /run-evals` , select scenarios against a LiveKit or Retell agent, and get structured pass/fail output back in the same context window.


Tool descriptions are bounded by the context they pay for at registration time. A multi-step workflow — *"create a metric, run it against last week's calls, review misalignments, update the prompt"* — can't live inside any single tool description without bloating every other one. We ship workflows as a separate primitive, organized in layers.


**Commands** are the shallow layer. One markdown file per command, invoked by name:` /create-metric` ,` /run-evals` ,` /eval-results` . They're for actions a user already knows they want to take — a short, opinionated recipe that drives MCP tools.


**Skills** are the deep layer. Each skill is a directory:


```text
cekura-skills/cekura/skills/cekura-eval-design/
├── SKILL.md                  # frontmatter activation + main workflow
├── references/               # lazy-loaded sub-files
│   ├── conditional-actions.md
│   ├── coverage-patterns.md
│   ├── mock-tool-design.md
│   └── ...
├── examples/                 # worked examples
│   ├── red-team-eval.md
│   └── workflow-eval.md
└── agents/                   # subagent definitions
└── eval-suite-planner.md


```


- ` SKILL.md` carries the frontmatter activation description (matched against user intent at runtime) and the main workflow body that loads when the skill activates.
- ` references/` files load on demand from inside the workflow — a skill can carry hundreds of KB of reference material without paying for it upfront.
- ` examples/` shows what a good output looks like, not just how to produce it.
- ` agents/` defines subagents the skill can spawn for discrete sub-tasks. The eval-design skill ships an` eval-suite-planner` that takes an agent description and returns a test-coverage plan.


The split matters. Lazy references mean a skill can ship deep expertise without making every activation expensive. Shallow commands keep high-traffic actions fast and predictable.


Two design choices that paid off:


- **Skills reference tools by name, not by signature.** When we tighten a tool's description, no skill body needs to change.
- **Activation descriptions are a tuning surface.** What activates and what doesn't is observable; the same feedback loop we apply to tool descriptions applies here.


## What Coding Agents Can Do with Cekura


Core MCP tools available to coding agents:` scenarios_run` ,` scenarios_run_retell` ,` scenarios_run_livekit` ,` scenarios_list` ,` cron_jobs_create` , and` results_list` . All are callable from Claude Code, Codex, and Cursor without leaving the editor.


Here is what that looks like in practice — a coding agent running a Cekura scenario suite in Python:


```text
import   os
import   requests


response = requests.post(
"https://api.cekura.ai/test_framework/v1/scenarios/run"  ,
headers={ "X-CEKURA-API-KEY"  : os.getenv( "CEKURA_API_KEY"  )},
json={
"agent_id"  :  int  (os.getenv( "AGENT_ID"  )),
"scenario_ids"  : [ int  (s)  for   s  in   os.getenv( "SCENARIO_IDS"  ,  ""  ).split( ","  )  if   s]
}
)
result = response.json()
assert   result.get( "passed"  ),  f"Voice agent regression failed:  {result.get( 'failure_reason'  ,  'unknown'  )}  "
print  ( f"All scenarios passed. Run ID:  {result.get( 'run_id'  )}  "  )


```


This snippet works inside any CI/CD pipeline, GitHub Actions workflow, or coding agent task — the same` CEKURA_API_KEY` and` AGENT_ID` used to configure the MCP server are all that is needed.


## Watch How Agents Use the Interface


Improving the interface only works if you can see how agents are using it. The signals worth watching are simple: retry rates per tool, activation rates in clients that lazy-load, skill-activation rates for the workflows we ship. A tool with high retries usually has an ambiguous description; a skill that rarely activates usually has a weak entry-point description, not a missing use case.


Teams running LiveKit voice agents use Cekura's MCP tools to trigger scenario runs after each agent config change, with no manual invocation. Recurring test suites run on a daily schedule, catching regressions before they reach production callers.


Agent confusion is a release-quality signal. If an agent gives up halfway through a workflow, that's a failure of the interface.


- Retry rates per tool signal ambiguous descriptions.
- Skill-activation rates tell you if entry-point descriptions are weak.
- LiveKit agent teams run scenario checks automatically on each config change.
- Recurring test suites catch regressions on a daily schedule without manual invocation.


## Common Questions


**Can I run voice agent evals through an MCP server?** Yes. Cekura has an MCP server. Coding agents can call tools to list scenarios, trigger voice agent test runs against LiveKit, Retell, or Vapi agents, and retrieve structured pass/fail results. All of this happens within the agent context window.


**Can I test voice AI agents without leaving my editor?** Yes. Cekura's MCP tools let you test voice AI agents without leaving your editor. Claude Code, Codex, and Cursor can invoke Cekura scenarios, run evals, and return structured pass/fail results directly in the agent context window.


**What voice agent testing tools work with Claude and Codex?** Cekura works with Claude Code, OpenAI Codex, Cursor, and any MCP-enabled coding agent. Connect once via OAuth and your coding agent can trigger voice agent test runs, browse scenario results, and schedule regression tests without leaving the IDE.


**How do I set up automated recurring voice agent tests?** Cekura supports automated recurring voice agent tests via` cron_jobs_create` . Test suites run on a daily schedule, or on every deploy via CI integration, catching regressions before they reach production callers.


**How do I test a LiveKit voice agent?** Cekura supports LiveKit voice agent testing via the` scenarios_run_livekit` MCP tool. Teams using LiveKit agents trigger scenario runs after each config change, with no manual invocation or browser required.


## Closing Thought


Cekura's MCP server works with Claude Code, OpenAI Codex, Cursor, and any MCP-enabled client. Voice agent testing across LiveKit, Retell, and Vapi runs from inside your editor, on a schedule, or wired into CI.


Agents don't need a new platform. They need product expertise made readable to a model. That's what an MCP server, a set of skills, and a thoughtful docs surface, done well, actually are.


---


Cekura's MCP server works with Claude Code, OpenAI Codex, and Cursor. Connect your voice agent test suite to your editor in under 5 minutes:[MCP setup guide](https://docs.cekura.ai/mcp/overview) .
