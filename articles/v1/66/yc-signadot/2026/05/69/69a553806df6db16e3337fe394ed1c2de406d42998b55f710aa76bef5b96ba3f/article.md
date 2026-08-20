---
schema_version: "1.0.0"
document_id: "69a553806df6db16e3337fe394ed1c2de406d42998b55f710aa76bef5b96ba3f"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/signadot-plans/"
published_at: "2026-05-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T22:13:02.720629+00:00"
content_hash: "sha256:2398ee21b0c48a017f6ec87f106a90ad234913a65155c52777c3b1b747806ea7"
---

# Signadot Plans

Today we’re shipping Signadot Plans, the next major piece of our agent-native, skills-based platform expansion. A plan is a small, reusable validation workflow that an agent can author from natural language and run against a live Sandbox. Plans give coding agents a versioned vocabulary for what correct means in your system, built on typed primitives your platform team controls. Available now in beta for all Signadot users.


Plans build on Signadot’s ephemeral environment and local development solutions to provide a comprehensive, agent-native platform for building and validating microservices. They run inside your Kubernetes cluster, where the agent’s work session already lives, and are governed by the platform team.


Below, we walk through what plans are, why the workflow is split the way it is, what they look like in action, and what this changes for teams building with agents using Signadot.


## Plans: validation workflows built for agents


A plan is a small, reusable validation workflow that runs against a live Signadot Sandbox. Plans are made up of two layers:


**Actions** are typed, deterministic building blocks. The catalog ships with` request-http` for API checks,` playwright` for browser flows, and` k6` for load scenarios, with more on the way. Each action is documented inline in markdown, so an agent reading the catalog can compose unfamiliar ones correctly without you holding its hand.


The catalog lives at[signadot/actions](https://github.com/signadot/actions) and accepts community contributions.


**Plans** wrap actions for specific validation use cases, each one scoped to a single user-visible behavior. They’re authored from natural language: a developer or agent describes what should be validated, drafts the plan, and commits it alongside the code as a versioned artifact. Every plan carries a` selectionHint` (a natural language description of what it validates), so the next agent can scan the library and pick the right one for the diff in front of it.


The value is in the breadth of the library. A team accumulates many small, focused plans over time, and agents choose the right ones for each change.


Plans are parameterized. The same plan can run against a sandbox or against the baseline cluster by toggling a` routingKey` , which means the same artifact serves both the agent validating its own change and a sanity check against the main branch. Plans can also be pinned to specific clusters, tagged for replay (` signadot plan run --tag ride-request-flow` ), and browsed in the dashboard alongside their run history.


PLAN


ride-request-flow


` selectionHint` · validates the end-to-end booking flow through the frontend


wraps


ACTION


playwright


Walks the booking flow, asserts the itinerary renders both location names.


runs against


SIGNADOT SANDBOX


live cluster


Real frontend, real services, real database. The plan executes against the actual system, not mocks.


## Skills as the interface


Coding agents interact with Plans through skills. Skills are the right interface for this work because they let the agent discover capabilities at the moment it needs them, without the developer pre-wiring anything into a system prompt or the agent guessing at an API. When an agent is mid-session and looking at a diff, the skill tells it what’s available, when to reach for it, and how to use it correctly.


We built two skills for Plans because authoring and running are different jobs, and keeping them separate lets the agent pick the right tool without ambiguity.


[signadot-plan](https://www.signadot.com/docs/integrations/coding-agents/agent-skills#signadot-plan) is the authoring skill. The agent inspects the action catalog, drafts a plan from a natural-language description, runs it once against baseline to confirm it works, tags it, and commits it.


[signadot-validate](https://www.signadot.com/docs/integrations/coding-agents/agent-skills#signadot-validate) is the runner. It reads the diff, picks a plan via` selectionHint` , spins up a sandbox with the change wired in, runs the plan, propagates the fix to whichever consumer broke, and re-runs until the plan passes.


Underneath both skills is a deliberate division of responsibility. Platform teams own the action catalog, which is where governance lives: what kinds of validation are allowed, what infrastructure they touch, what each action is permitted to do inside the cluster. Developers and agents author plans on top of that catalog. The boundary stays where you want it, and the surface area agents can reach stays bounded to what your platform team has approved.


Platform team owns


Skills bridge the boundary


Developers & agents compose


Action catalog


governance, allowed infra


request-http


playwright


k6


signadot-plan


authors plans


signadot-validate


runs plans


Plans library


authored per change


ride-request-flow


location-api-contract


checkout-load


## What this looks like in practice


A worked example from the[HotROD demo](https://github.com/signadot/hotrod) , four Go services on Kubernetes with Redis, MySQL, and Kafka.


You ask the agent to rename a field on the location service.` Name` becomes` LocationName` . The Go build is clean. Unit tests pass. Linters are happy. From inside the location service, everything looks done.


Before opening the PR, the agent invokes` signadot-validate` . The skill reads the diff, finds the existing ride-request plan via its selection hint (a Playwright check that walks the booking flow), spins a sandbox with the modified service wired in, and runs the plan against the real frontend, real database, and real downstream services.


The plan fails. The frontend still reads the old` name` field. The itinerary renders empty. The assertion times out.


The skill reads the failure, traces it to the contract, finds the four call sites in the React app that read the old field, updates each one, rebuilds, and adds the frontend to the sandbox as a second locally mapped workload. It re-runs the plan. It passes.


The PR that opens is already validated against the real cluster. The reviewer gets a diff plus a passing plan run they can click through and audit. The whole loop happened inside the agent’s session, before any human looked at the code.


The full walkthrough is in the[plan-based validation quickstart](https://www.signadot.com/docs/tutorials/quickstart/plan-based-validation) .


## What this unlocks


For teams using Signadot, Plans turns per-change validation from a problem each agent has to solve from scratch into a reusable library that grows over time.


- **A versioned vocabulary for what correct means in your system.** Each plan is a small artifact, readable by humans, runnable by agents, describing one piece of behavior worth checking. The library expands as your system does, and agents pick the right plan for each change.
- **Validation that fits the inner loop.** Plans run in seconds inside the agent’s work session, fast enough to validate every change as the agent iterates, against the real cluster rather than mocks.
- **Governance where it belongs.** Action authorship sits with your platform team. Plan authorship sits with the developers and agents doing the work. You adopt Plans without giving up control over what runs against your cluster.


With Plans, Signadot becomes a comprehensive, agent-native platform for building and validating microservices on Kubernetes. Signadot Sandboxes give agents lightweight, ephemeral environments inside your Kubernetes cluster, ready in seconds for any change an agent wants to validate. Plans and skills give agents the validation vocabulary to run inside those environments, governed by your platform team. More pieces coming.


## Get started


- [Plan-based validation quickstart](https://www.signadot.com/docs/tutorials/quickstart/plan-based-validation)
- [Agent skills documentation](https://www.signadot.com/docs/integrations/coding-agents/agent-skills)
- [Actions catalog on GitHub](https://github.com/signadot/actions)


If you’re already on Signadot, Plans is available in your account today. If you’re not,[sign up for free](https://www.signadot.com/signup/) or[book a demo](https://www.signadot.com/schedule-a-call/) .
