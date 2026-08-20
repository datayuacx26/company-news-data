---
schema_version: "1.0.0"
document_id: "5748dba7a9466eabca458aa561ee92b96017622286be5d5a0ca41f7ad346d071"
company_key: "yc-avo"
company: "Avo"
source_id: "yc-avo-news-import-5b3bdf315861"
canonical_url: "https://www.avo.app/blog/stop-ai-breaking-analytics"
published_at: null
first_seen_at: "2026-07-21T08:54:15.034482+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:d27457dbf8504d9ca0a3ef53c36b7bd4b01b68228c9f76cec12ac17c07fc8949"
---

# Stop your AI coding agents from breaking analytics tracking

## Key takeaways


- Point several AI coding agents at one product and you get **split-brain analytics** : each is consistent in its own repo, none can see the others, so one user action ends up with four names.
- The agent can't self-police. It has no memory of your taxonomy between sessions, and it hallucinates names at measurable rates.
- Analytics is uniquely unforgiving. No compiler error and no failing test catches a misnamed event, so the damage surfaces weeks later, by which point it's unrecoverable.
- The fix isn't "prompt the agent better." It's a governed, source-of-truth tracking plan the agent reads and writes through, with the rules enforced on the server and the result verified at build time and in production. That's what the[Avo MCP](https://www.avo.app/docs/reference/avo-mcp/overview) is built for.
- **Govern at the top of data creation, then trust and verify.**


## Split-brain analytics: how one action ends up with many names


Picture this: over a couple of sprints, three teams each add a piece of checkout tracking, each in its own codebase, each with its own AI coding agent. You already have a tracking plan, and it defines the event as` Checkout Started` . The iOS developer remembered to point her agent at it, so the agent checked the plan and used the right name. The other two forgot. Their agents had started a fresh session with no memory that the plan existed, so they took the event name from the words in the PRD and applied whatever casing they preferred: one wrote` begin_checkout` , the other` CheckoutBegin` . Three names for one action, and the only thing that decided right from wrong was whether a human happened to remember the plan.


This is **split-brain analytics** . In distributed systems, split-brain is when isolated nodes lose sight of each other, each keeps making locally valid decisions, and the cluster ends up in two conflicting states at once. Swap the nodes for AI agents and that's your tracking plan: each agent is perfectly consistent inside its own repo, none of them can see what the others are minting, and the taxonomy quietly forks. A shared plan could have kept them aligned, but it reached only the agent a developer thought to point at it. Leave consulting it to whoever remembers, across every developer and every repo, and divergence stops being a risk and becomes the default.


And nothing catches it. For ordinary code the feedback loop is tight: the type checker complains, the test fails, the build goes red. That loop is exactly why agents now write[up to 30% of the code at Microsoft](https://techcrunch.com/2025/04/29/microsoft-ceo-says-up-to-30-of-the-companys-code-was-written-by-ai/) , with Google[reporting a comparable share](https://fortune.com/2024/10/30/googles-code-ai-sundar-pichai/) and[still climbing](https://devops.com/google-ceo-says-75-of-new-code-is-ai-generated/) . Analytics has no such loop. All three names compile, pass tests, and go live, because to each codebase nothing went wrong.


So the failure stays invisible until the funnel report turns to nonsense three weeks later, in a board deck already shaping a roadmap call, with no stack trace pointing back. And the wrong chart is the mildest version of the damage. The same events drive real behavior: they decide which lifecycle email a customer gets, what the app personalizes, which cohort an experiment counts them in, and increasingly what your models train on. Feed those systems a corrupted signal and the cost isn't a bad dashboard, it's a customer dropped into the wrong automated flow and a model that learned the wrong thing. Analytics bugs don't throw. They just poison the numbers until someone downstream queries them and gets the wrong answer.


The root cause is that being careful was never going to be enough. A misnamed event (or a misspelled property, a number sent as a string, an event that fires on the wrong screen) is just a value that happens to be valid. Correctness here can't come from the agent. It has to come from a system every agent writes through.


## Why the agent can't fix this itself


Why not just reach for a smarter model or a better prompt? Because this isn't a capability gap the next model closes. It's structural:


- **No memory of your taxonomy.** Each session starts cold; the agent doesn't know you already have` Product Viewed` , so it invents` ViewedProduct` .
- **It drifts on naming.** AI code is[documented to shift naming style mid-codebase](https://blog.exceeds.ai/ai-generated-code-quality-metrics/) , and` sign_up` and` signup` are two different events to every downstream tool.
- **It hallucinates names, measurably.** A[USENIX Security 2025 study](https://devops.com/ai-generated-code-packages-can-lead-to-slopsquatting-threat-2/) of 576,000 samples found 19.7% of suggested packages didn't exist, and 43% of those hallucinations repeated. A made-up event name fails the same way: it looks plausible, compiles, and ships. Spot-checking won't save you.


Developers already sense it. In[Stack Overflow's 2025 survey](https://survey.stackoverflow.co/2025/ai/) , adoption hit 84% while trust in AI accuracy fell to 29%, and 45% said debugging AI code takes *more* time. The agent is a fast, confident junior with no institutional memory, which is exactly the profile that produces analytics drift. So the question isn't "how do I make the agent careful?" It's "how do I make correctness something the agent can't get wrong?"


## What a tracking plan actually is


At its barest, a tracking plan is just a list of the events you send. A good one is far more: the single source of truth defining every event, its properties, their types, the values they're allowed to take, and the naming rules they all follow. The richer it is, the more it can actually enforce, until it becomes the layer of meaning that says what` Checkout Started` is allowed to be and what it never can.


It can live anywhere, and where it lives is the whole story. Most teams keep it in a Google Sheet, a Confluence page, or Airtable. Everyone can read it, including the non-technical PMs who own the taxonomy, but it enforces nothing, validates nothing, and an agent can't semantically search it for the event it ought to reuse.


Some engineering teams keep it as JSON or YAML in a git repo instead. That's version-controlled, and an agent with access to the repo can read it, but it still enforces nothing, locks out non-technical owners, and an agent in a different codebase never sees it. Either way the plan is a passive document, and a document that nothing connects to design, enforcement, or implementation is just a suggestion.


That's the difference between an agent guessing and an agent knowing. Anthropic's data team measured the gap: governed, structured context lifted analytics accuracy from 21% to 95%, while raw data access barely moved it ([Anthropic](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) ). Their agents were querying a warehouse, not writing` track()` calls, but the lever transfers directly: give the agent a plan to read and it reuses` Checkout Started` instead of inventing a name, the same way governed context beat a bigger model there. Governance, not horsepower. And the gain decayed once the context wasn't maintained, so structure only works when it's live and kept current, which is the whole thesis behind[why you govern your data before your agents depend on it](https://www.avo.app/blog/govern-your-data-your-agents-depend-on-it) .


So what turns a tracking plan from a document into something your agent reads from and your pipeline enforces? Six things.


## What it takes to keep your agents in sync


The first two are about the agent, the next two about how changes are governed, the last two about the code that comes out.


### 1. The agent has to check what already exists


Before it implements a new event or property, the agent should look for what's already there, even when it doesn't know the exact name to search for. That means the plan has to be discoverable by meaning, not just by exact match, and the agent has to be told to check it first. Then it finds and reuses your existing` Checkout Started` instead of minting a fourth name for it.


### 2. The agent has to know how to use the plan


Access isn't usage. An agent that *can* reach the plan still has to know to reach for it every session (not only when a developer happens to remember to tell it), to search what exists before it suggests anything, to update the plan before it writes the implementation, and to do those steps in the right order. That's a procedural playbook, the same kind of structured guidance Anthropic credited for the jump to 95%, and it has to travel with the plan as a durable harness, not get re-improvised each session. Concretely, it's the kind of thing you'd otherwise hand-write as a` CLAUDE.md` , a` .cursorrules` file, or a[Claude Code skill](https://github.com/avohq/avo-mcp) — except re-authoring it per developer is exactly the fragile part. Better if it ships with the plan itself, so the agent shows up already knowing the routine instead of everyone re-teaching it from scratch.


### 3. The rules have to be known and enforced, not guessed


An agent might infer your casing from a few examples, or it might not, and "probably consistent" isn't governance. Your naming conventions, your no-duplicates rule, your insistence that a revenue property is a number and not "any", your requirement that every event ships with a description: each has to be explicit to the agent and enforced by the system, not left to inference. A duplicate gets merged into the existing definition rather than cloned, and an off-spec change is blocked or flagged according to your rules, not waved through. The agent doesn't have to be trustworthy, because it isn't the thing deciding what's allowed.


### 4. Changes are drafted in isolation, with human review when it matters


An agent drafting analytics shouldn't touch the live plan. Its changes are made in an isolated draft, separate from the source of truth, so nothing it does can corrupt what's already live. From there, a human reviews the changes that warrant it: when the agent isn't confident, when the change touches an area a specific team owns, or when it's governance-sensitive enough that a person should sign off before it goes live. Once approved, the draft is applied to the source of truth and the new tracking goes live. Propose, don't publish, so if the agent gets it wrong, the worst case is a draft you throw away, not corrupted production data you notice three weeks later and can't recover.


### 5. The implementation has to stay connected to the plan


A correct plan is worthless if the code sends something else, and worse if the two quietly drift apart over time. The implementation has to stay connected to the plan, not copied from it once: change the plan, and the implementation changes with it. Left to hand-typing, whether it's a developer or an agent writing the call, the implementation ships` track('Checkout Started', { cart_total: '49.99' })` , a string where the plan says number, and nothing keeps it in line. Generate the tracking code from the plan and the connection is live and type-safe: an undefined event or wrong-typed property won't compile, and a renamed event updates everywhere it's used. Even without generated code, the plan is the exact per-event spec to build against, so there's no "is` cart_total` a string or a number?" round-trip with the data team.


### 6. The implementation has to be easy to validate


When something's wrong, you should see it immediately, not three weeks later. While you build, validating an event against the plan should be quick and effortless, with errors that jump out at you instead of staying buried. And once events are live, you should be notified when something drifts from the plan, not left to dig through the data after the fact or stumble onto the breakage only when you go to use it. Validation you have to go hunting for is validation nobody runs.


## Where you store the plan decides how many of these you get


‍


The two document options differ only in degree, and the table tells the story: a spreadsheet clears almost nothing, a git repo isolates changes but little else. You can bolt a few of the gaps shut by hand, like a schema check in a pre-commit hook or a typed wrapper you maintain, but each is a separate system that drifts out of sync with the plan the agent writes to. Which is the exact drift this post is about.


This is the split[Avo](https://www.avo.app/docs/reference/avo-mcp/overview) is built to close: all six in one governed plan.


Avo is that governed plan, and your agent reaches it through the[Avo MCP](https://www.avo.app/docs/reference/avo-mcp/overview) : the interface that lets the agent read the plan and propose changes without ever touching the live source of truth. Here's how it covers the six:


- **Checks what exists first.**` search` /` get` over the[Avo MCP](https://www.avo.app/docs/reference/avo-mcp/overview) surface related events by meaning, not just exact match.
- **Knows how to use the plan.** The MCP server ships usage instructions to every client that connects (Cursor, Claude, Codex, or anything else speaking MCP), so the playbook travels with the plan instead of getting re-taught per agent. Claude Code users get an extra layer:[installable skills](https://github.com/avohq/avo-mcp) that bundle the full routine. Either way, nobody hand-writes it.
- **Rules known and enforced.** Your[audit rules](https://www.avo.app/docs/data-design/best-practices/naming-conventions) run server-side on every write, merging duplicates and blocking or flagging violations per your config.
- **Isolated changes, reviewed when needed.** Edits land on a branch (` Draft → ReadyForReview → Approved` ) with the owning teams as required reviewers; the agent proposes, never publishes.
- **Connected implementation.**[Type-safe codegen](https://www.avo.app/docs/implementation/cli) won't compile on drift (` avo pull` in CI); without it, each event's pseudocode snippet is the exact spec to build against by hand.
- **Easy to validate.** Avo's debuggers (a live overlay on device and a real-time feed in the app) flag events green or red as you build, and once events are live,[Inspector](https://www.avo.app/docs/inspector/start-using-inspector) (Avo's production monitor) watches for you — it alerts you in Slack when new issues or regressions appear, and an issues view shows exactly which events and properties are diverging from the plan, instead of you digging through the data after the fact. Trust is verified, not assumed.


It's the productized version of the idea that[your AI doesn't remember your tracking plan, but the MCP does](https://www.avo.app/blog/your-ai-doesnt-remember-your-tracking-plan-the-avo-mcp-does) . Design-time governance, build-time checks, production checks: the whole loop, closed.


Keeping that plan consistent across teams, products, and platforms, as more analytics gets designed by agents rather than just implemented, is its own discipline. That's the data leader's side of this story:[how to govern the analytics your AI agents design](https://www.avo.app/blog/govern-data-design-ai-agents) .


## One plan, one brain


Analytics is the one place in your codebase where being wrong doesn't fail loudly. Point a fleet of agents at it, each blind to the others, and split-brain is the default: fast, confident, no memory of what you've already named. You can't fix it by being careful, or by hoping the model improves.


You fix it with a system: a plan the agent checks before it builds, rules it knows and the server enforces, changes made in isolation with review where it matters, and an implementation that stays connected to the plan and is easy to validate. Govern at the top of data creation, then trust and verify.


Your agent is going to keep writing` analytics.track(...)` . Give it a plan worth tracking against.


**Connect your tracking plan to your AI agent →**[Avo MCP](https://www.avo.app/docs/reference/avo-mcp/overview) **.**


## Common questions


### How do I stop Cursor or Copilot from creating duplicate analytics events?


Connect it to a tracking plan over the Avo MCP so it looks up existing events, and let the` save_items` server step deduplicate on write: an exact match updates instead of cloning, near matches get flagged. The enforcement is server-side, so a forgetful agent can't bypass it.


### How do I make my AI agent use my existing event names?


Give it semantic search over your plan. Through the Avo MCP, the agent's` search` and` get` read your real taxonomy, so instead of inventing` ViewedProduct` it finds and reuses` Product Viewed` , the memory of your taxonomy it otherwise loses every session. Because it searches by meaning, the agent pulls only the handful of relevant events into context instead of loading your whole plan on every lookup, which keeps it faster and cheaper on tokens as the plan grows.


### Do I need to write Cursor rules or a CLAUDE.md to make my agent use the plan?


No. With Avo the guidance ships for you: the MCP server sends usage instructions every session, and the installable[skills](https://github.com/avohq/avo-mcp) bundle the playbook in the same step that registers the server. You connect the plan; the agent already knows how to use it.


### Can an AI agent push bad analytics straight to production?


No, if changes are branch-scoped. Agent edits land on a branch that moves` Draft → ReadyForReview → Approved` with a human reviewing before merge when your setup requires one. An edit-scoped agent can't modify the live plan, so the worst case is a branch you delete.


### How do I validate analytics events in CI?


Generate typed code from your plan so an undefined event or wrong-typed property fails compilation, then run that check in CI. With Avo that's` avo pull` plus your type-check, and the pipeline goes red on drift like any other type error.


### How do I connect the Avo MCP to my agent?


For Claude Code, two commands install the skills and register the server:


` claude plugin marketplace add avohq/avo-mcp
claude plugin install avo@avo-mcp
`


For any other MCP client (Cursor, Codex, and the rest), add the server to its MCP config:


` { "mcpServers": { "avo": { "url": "https://mcp.avo.app/mcp" } } }`


Either way the agent gets your plan plus the usage guidance, with nothing to hand-write.


‍
