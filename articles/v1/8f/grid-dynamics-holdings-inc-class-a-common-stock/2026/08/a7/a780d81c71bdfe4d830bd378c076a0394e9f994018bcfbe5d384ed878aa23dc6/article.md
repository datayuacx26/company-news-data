---
schema_version: "1.0.0"
document_id: "a780d81c71bdfe4d830bd378c076a0394e9f994018bcfbe5d384ed878aa23dc6"
company_key: "grid-dynamics-holdings-inc-class-a-common-stock"
company: "Grid Dynamics Holdings Inc."
source_id: "grid-dynamics-holdings-inc-class-a-common-stock-news-import-14c47dcfb441"
canonical_url: "https://www.griddynamics.com/blog/agentic-ai-test-automation"
published_at: null
first_seen_at: "2026-08-04T11:41:26.745552+00:00"
fetched_at: "2026-08-04T11:44:39.476301+00:00"
content_hash: "sha256:da463d9a1780a0dc0aa7f969149c628fc24bfa824721a33a09c8b67b659f3ba8"
---

# How AI agents took automated test coverage from 20% to 80% in just 6 weeks

[Home](https://www.griddynamics.com/)


[Insights](https://www.griddynamics.com/blog)


[Articles](https://www.griddynamics.com/blog/articles)


How AI agents took automated test coverage from 20% to 80% in just 6 weeks


# How AI agents took automated test coverage from 20% to 80% in just 6 weeks


[Alexandr Grisin](https://www.griddynamics.com/author/alexandr-grisin)


Aug 04, 2026 • **11 min read**


Share


Follow


Subscribe


Follow


Table of Contents


**


- The challenge: Automation could not replace the client’s three-day manual regression cycle
- How we used AI-assisted test generation
- The challenges we ran into
- How the approach adapts to where you start
- Path 1: Documented intent, but no automation
- Path 2: Legacy automation, but no release trust
- Path 3: Production code as the only spec
- Conclusion


In just six weeks, our[QA experts at Grid Dynamics](https://www.griddynamics.com/solutions/qa-automation) helped a client team turn limited test automation into a quality gate enforced on every merge. Coverage grew from roughly 20% to 80% across a modern release stack, without expanding the QA team.


Two QA engineers directed the AI agents, reviewed test intent, and approved every merge, while the agents generated the volume. After launch, the same small team maintained the suite, with AI agents continuing to do the heavy lifting.


This blog explains how we established the right foundation, separated agent responsibilities, placed human review at critical gates, and how our approach applies across three common starting points: documented manual cases, untrusted legacy automation, and production code without an established test layer.


## The challenge: Automation could not replace the client’s three-day manual regression cycle


The client team shipped every sprint, but every release still required a three-day manual regression pass.


Six QA engineers worked through roughly 450 test cases before each release. They had started automating 18 months earlier, with two engineers building 60 web scenarios. Coverage grew, but not fast enough.


Each scenario was written by hand. Every product change sent engineers back to fix existing tests rather than add missing coverage, consuming roughly half of their sprint capacity on rework alone.


The team tried the obvious fix first: adding another engineer to the team for one quarter. Then two sprints were devoted entirely to authoring, but the team still could not keep pace.


The suite grew by another 30 hand-written scenarios, adding to the same rework load they meant to fix. For every sprint, the choice was the same: fix broken tests or build new coverage. Fixing won. The release still depended mostly on the manual pass, with automation covering roughly 20% of that suite.


**This team is not unique.** We have seen the same release-confidence gap take different forms. Teams that invest seriously end up with 400, sometimes 800 automated scripts, a dedicated SDET team, and a CI pipeline built to run them. They hit coverage milestones, then watch the suite erode as the product moves faster than the tests can follow. Maintaining outdated automation that no longer protects the release becomes its own tax on every sprint. Elsewhere, the backlog never leaves TestRail or a Confluence checklist: a few engineers still run regression by hand, and the manual-only translation debt grows each cycle with no path to close it. And then there are product teams that ship for months with no tests at all, no documented scenarios, no automation, and no record of what was checked before production. The quality layer is never funded because shipping is always prioritized.


For this team, the baseline testing approach looked different in every case. They lacked a trusted, sufficient release gate, which meant that someone had to run manual regression testing for every release.


All three starting states in the diagram below chased the same goal: enough confidence to hit a two-week release cadence without a three-day manual pass first. Coverage gets them there. Automation delivers it fast enough to matter. Every step along the way (analyzing requirements, writing test cases, building automation, fixing broken tests by hand) eats engineering time the team should spend shipping.


*[AI assistance on every step of the chain](https://www.griddynamics.com/blog/agentic-qa-platform-demo) closed the gap that more hands and dedicated sprints couldn’t.*


*Figure 1: Three starting states* follow the same approach


## How we used AI-assisted test generation


We did not start by writing a test for this team. We started with the foundation.


### Build the foundation first


First, we mapped the product. Coding agents reverse-engineered the repository into a grounded picture of its architecture, test conventions, and behavior map, working through it module by module and tying every claim back to real code rather than guessing.


An engineer reviewed and confirmed this foundation before any generation session began. What came out was committed to the repository alongside the code. From that point forward, it became the shared context every agent session drew from: versioned with the product, updated as it changed, built once and reused throughout.


Teams that already maintain *[AI-ready architecture](https://www.griddynamics.com/blog/ai-quality-assurance)* can move through this step faster. For this team, we completed the foundation pass before any test generation began.


### Keep agent responsibilities separate


Using that foundation, we grounded three agents for each session:


- A planning agent structured the scenario plan and surfaced coverage gaps before generation.
- A generation agent produced test code against the approved plan.
- A repair agent analyzed CI output and proposed targeted corrections.


That’s how we divided the work, and we kept the roles separate on purpose: the agent that writes a test is not the agent that decides whether its failure is real.


*Figure 2: Foundation context layer*


### Stabilize on a modern test stack built for agents


Playwright is what we reach for when agents are the ones writing and checking the tests. It is built to be driven and inspected programmatically, so agents can work with the framework rather than fight it just to get a test running.


Fortunately, this team had already been through one automation cycle. A much larger earlier automation program collapsed under its own maintenance, and what came next was running on Playwright. So we weren’t picking a framework. We were scaling up what was already sitting there.


### Use quality gates as enablers


We placed human governance before execution, not at the end of the release cycle, with CI enforcing automated gates throughout the process.


- Before generation, an engineer had to approve the test intent. The plan needed to define the scope and measurable assertions. Agents could not begin building without that approval. A green CI run proves only that the suite executed successfully. Without an intent gate, a team can ship automation that never checks the behavior that matters.
- No proposed change landed in the production suite without review. Rejecting a bad proposal takes minutes; merging one that weakens the gate can burn a sprint of cleanup.
- CI served as the automated gate, while the team’s engineers retained ownership of release confidence. Agents proposed repairs, but the engineers approved every change before merge. An agent that grades its own output will keep patching until the suite is green, whether or not the product is right.


This follows the broader pattern McKinsey has observed across agentic delivery teams: humans intervene at defined review gates rather than acting as intermediaries throughout the entire workflow.


*Figure 3: Shared governance loop*


## The challenges we ran into


We encountered real challenges during the engagement, and they explain why the gates sit where they do.


### Logistical challenges


The fastest part of this approach is the AI. The slowest part was everything standing between us and the point where the AI could start.


- Access and onboarding: Before an agent could touch a single scenario, IT and security on the other side had to grant access to Jira, Confluence, TestRail, Git, the CI pipeline, and the environment itself. In this engagement, the change-approval chain spanned multiple teams and took over two weeks before every permission was granted.
- Client-mandated environments: Across other engagements, engineers and agents sometimes have to work from a client-controlled machine, whether a VDI or another locked-down environment. When that environment has enough capacity, the overhead is negligible. When resources are constrained, its latency, not the agents, sets the pace of agent-assisted work.


Neither issue limits what the agents can reason about. Both limit the environment around them, and that friction can offset the speed the AI-assisted workflow is meant to deliver.


### Structural challenges


Checking that a test verifies the *right* behavior is still the least mature automated control. The intent gate therefore relies on a human to perform a function the tooling cannot yet handle reliably.


That makes the human a potential bottleneck, so we bound the gate to two points, intent and merge, and let CI carry the mechanical checks in between: running the suite and surfacing failures.


Underneath that sits a subtler, structural risk: what the agent inherits rather than what it invents.


- Pattern propagation: Aligning with existing test architecture means aligning to whatever is already there. Anti-patterns included. The 90 hand-written scenarios this team had already built carried exactly that kind of debt: arbitrary waits, brittle selectors, copied just as faithfully as the patterns worth keeping. Catching the propagated anti-pattern falls to the merge approval gate, the same gate that reviews everything else the agent proposes.
- Symptom-patching: The shared verify-and-correct loop has a matching blind spot. A failed run shows the symptom first: a timeout, a flaky locator, a retry that masks a real defect. The loop forces a classification before any fix. The agent names the failure as a test error or a real product bug. A human approves that classification before any fix is applied.
- Test data contamination: In other engagements, shared or non-deterministic test data can produce false failures that look like real bugs. A team chasing that phantom regression can burn a full review cycle before realizing the suite, not the product, was broken.


These are the reasons each gate sits where it does: skipped review is what lets the failure modes above reach the production suite.


One other lesson surprised us enough to change how we work. The obvious shortcut was to skip the foundation pass and let agents generate immediately. It felt faster, but it failed quietly. What came out was shaped by the agent’s guess rather than by the codebase, and it didn’t integrate with what this team was already running. Skipping the foundation created the mismatch. That is why the mapping now happens first.


## How the approach adapts to where you start


That was one team, with documented cases and thin automation already in the repo. The same governance loop applies across engagements, regardless of the starting point.


What differs is only the project starting state: manual cases with no automation, legacy scripts that had lost trust, or production code with no test layer at all.


Each starting point gives the agents something different to work from, reshaping the discovery and setup that comes before agents start building the tests.


### The platform behind every path


None of the three starting state paths below runs on a flow built from scratch each time. The instructions and skills behind it, the agent roles, the gates, the workflows, are already packaged into[Rosetta](https://griddynamics.github.io/rosetta/) , the open-source context-engineering system for AI agents that Grid Dynamics built.


## Path 1: Documented intent, but no automation


This is the lowest-discovery path when QA and product documentation already exist. AI-assisted authoring fits best here. The flow looks simple:


*Figure 4a: Automation test generation flow at a glance*


We ran this path on Rosetta’s automated QA flow.


- What we establish first: Define success before automation on cases the organization already owns: what counts as pass, what data applies, what is out of scope.
- Ingest and structure intent: Agents consolidate cases and product context from QA systems such as TestRail, Confluence, and Jira into a structured plan that the team can review before generation runs.
- Clarify success before automation: Agents surface gaps in steps and expected results. Humans supply assertions, test data, and scope. Engineers approve intent before generation runs at scale. This is the gate that catches inherited weakness. A source case that’s stale, vague, or never rewritten after the UI changes gets faithfully automated as-is unless a human tightens the assertion here. Skip this review, and you automate ambiguity and produce a green suite that validates the wrong behavior.
- Align, generate, and close the loop: The Foundation layer sets the conventions: framework, naming standards, and reusable test structure. Agents generate tests against the approved plan; what previously lived in tickets now runs in CI.


*Figure 4b: From documented intent to executable coverage*


## Path 2: Legacy automation, but no release trust


When teams start with legacy automation that no longer protects the release, what comes before generation looks different. They already paid for automation but no longer trust it, and the blockers are framework debt and maintenance load, not the absence of tests.


Figure *5a: Automation test conversion flow at a glance*


When the starting state is legacy automation, conversion runs on Rosetta’s modernization flow.


- What we ground first: Spec before conversion with grounded documentation of legacy behavior and an approved source-to-target mapping.
- Document what the suite actually does: Agents produce grounded descriptions of what legacy tests cover, how they’re coupled to legacy infrastructure, and what behavior must survive. Unknowns remain explicit rather than being guessed. Peer review confirms accuracy before mapping starts. Teams with no trust in the suite may optionally add baseline tests on legacy behavior before mapping.
- Define and approve the target mapping: Agents draft the modern stack target and source-to-target intent. Humans resolve blocking unknowns and approve intent on the mapping before bulk conversion.
- Convert in waves with CI proof: Agents convert the suite in batches. The pipeline runs each wave; a repair agent flags failures and proposes targeted fixes. Teams sign off before the next wave begins.


Rush conversion without an approved mapping regenerates the same debt faster. The baseline on legacy behavior is what protects attribution. Without it, a failure after conversion could indicate either a genuine product defect or a conversion error, with no reliable way to distinguish between them. Approving the agent’s fix without review risks burying a real regression beneath a green suite.


*Figure* *5b: Modernization workflow*


## Path 3: Production code as the only spec


There are no documented scenarios and negligible automation. Discovery runs longest before generation begins. Whatever verification happened along the way was ad hoc and left no trail. Production code is what agents inherit.


*Figure 6a: Product discovery before automation at a glance*


For this path, static discovery runs through Rosetta’s code-analysis workflow, followed by dynamic discovery and intent approval.


- What we resolve first: Static and dynamic discovery, merged into one scenario list that passes intent approval before test generation starts.
- Reverse-engineer the product through static discovery: Agents perform a scoped analysis of production code, component contracts, and API schemas to map candidate flows and assertions grounded in what the product actually covers: not just happy-path screens but feature flags, routing, session flows, and conditional UI.
- Selector strategy starts at the component layer. Stable test hooks, design-system boundaries, and ARIA contracts anchor the suite in the same contracts the team already maintains. The generated suite survives component refactors, not just the first CI run.
- Peer review confirms the analysis, including any structured requirements recovered from implementation, before it counts as spec.
- Explore the running application through AI-assisted dynamic discovery: Static analysis resolves what the code defines; live execution confirms what renders, which paths are reachable, and whether conditional branches behave correctly in a running session. This pass runs after static discovery to validate and extend the scenario plan against a staged build. Playwright MCP ( *Model Context Protocol* ) gives agents structured DOM state, live component tree snapshots, and reachable path data: the context an agent needs to reason over screens it hasn’t seen before. The observations fold into the same scenario plan. Dynamic exploration has a ceiling, though: long sessions accumulate more state than an agent can reason over cleanly, so this pass stays deliberately scoped rather than exhaustive. Exploratory output is input only; it does not replace peer review or intent approval.
- Reconcile discovery, then build and verify: Static and dynamic findings are merged item by item. Dynamic observation confirms, or discards each statically inferred candidate, and any runtime-only behavior it surfaces becomes a new scenario.


Engineers approve intent on the merged scenario list before automation spend begins, the only gate this path adds beyond the shared verify step. Because no legacy suite or previous baseline exists, the first approved green run becomes the baseline.


*Figure 6b: Dual-input Discovery before Automation Workflow*


## Conclusion


A team can turn whatever it started with, including hand-run cases, an untrusted legacy suite, or no tests at all, into a reliable test suite. What used to take 18 months of manual buildup now can be closed in six weeks, achieving the results stated below:


- 60% less test-authoring time when AI-assisted generation works from a scenario plan with approved assertions and scope. Agents surface ambiguous cases before generation runs, so what ships is automation against validated behavior, not ambiguity. The ceiling on throughput isn’t agent capacity; it’s how many intent and merge approvals engineers can clear per sprint.
- 30% faster CI runs with 50% fewer false test failures when AI-assisted modernization replaces a legacy suite with a modern test stack guided by an approved source-to-target mapping. This eliminates the maintenance load that made the old suite unusable. Teams with the highest legacy flakiness before migration see the largest gains.
- 40% reduced discovery effort when code-grounded static analysis and live exploration combine into one approved scenario list, even starting with no QA documentation. In one engagement, that cut a single QA engineer’s per-sprint time on this task from 8 hours to 4. That approved scenario list becomes the foundation for a release gate the team can trust. Most of the discovery cost is paid upfront. Teams that maintain AI-ready architecture pay it faster and avoid repeating the same discovery work as the product grows.


Are you sizing a test-improvement initiative or a broader SDLC efficiency program? Talk to our QA experts about an approach tailored to your starting point, engineering environment, and release goals.


## Tags


[Agentic AI](https://www.griddynamics.com/blog/agentic-ai)


[AI SDLC](https://www.griddynamics.com/blog/ai-development-lifecycle)


[Artificial intelligence](https://www.griddynamics.com/blog/ai)


[Cloud platform and product engineering](https://www.griddynamics.com/blog/cloud-platform-and-product-engineering)


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


[Quality and performance engineering](https://www.griddynamics.com/blog/quality-and-performance-engineering)


Share


Follow


Subscribe


Follow


## You might **also like**


Article


Experience debt is the bill AI passes to your customers


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


[Experience debt is the bill AI passes to your customers](https://www.griddynamics.com/blog/experience-debt)


Technical debt slows your developers. Experience debt drives your customers away. Most companies understand the first half of that sentence far better than the second. Technical debt has a language executives accept, dashboards that track it, and ratios that price it. It has earned its seat on t...


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


Practical agent evaluation techniques for a real-world knowledge assistant: A Grid Dynamics case study


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


[Practical agent evaluation techniques for a real-world knowledge assistant: A Grid Dynamics case study](https://www.griddynamics.com/blog/how-to-evaluate-ai-agents)


With the rapid growth of AI agent usage in production, Grid Dynamics began facing challenges in determining whether agents were healthy, identifying abnormal behavior, and understanding when agent behavior deviated from expected outcomes. This article provides an actionable approach to building eval...


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


AI agents are assembling adaptive UI. Here’s how validation needs to evolve.


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


[AI agents are assembling adaptive UI. Here’s how validation needs to evolve.](https://www.griddynamics.com/blog/adaptive-ui-validation)


User interfaces are no longer static. The industry is shifting toward adaptive systems where the interface is assembled at runtime. For decades, software was designed around fixed surfaces: a nav here, a hero there, content slots predefined by a designer. Users learned the interface. However, th...


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


Enterprise AI modernization as a daily operating model


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


[Enterprise AI modernization as a daily operating model](https://www.griddynamics.com/blog/ai-powered-modernization)


What does AI-powered modernization as a daily operating model look like? On Monday morning, your teams do not start by opening an incident queue. They start by reviewing a set of pull requests produced overnight by software agents focused on modernization. Each pull request is small.


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


Are your UI application development processes compliant with the EU AI Act?


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


[Are your UI application development processes compliant with the EU AI Act?](https://www.griddynamics.com/blog/eu-ai-act-compliance)


As of February 2026, the European Union Artificial Intelligence Act (AI Act) has transitioned from a legislative draft to the primary regulatory framework for software engineering in the EU. This landmark legislation is no longer a distant prospect; with prohibitions on unacceptable risks already i...


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


AI agent for UI design: A safer way to generate interfaces


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


[AI agent for UI design: A safer way to generate interfaces](https://www.griddynamics.com/blog/ai-agent-for-ui-a2ui)


Enterprise AI agents are increasingly used to assist users across applications, from booking flights to managing approvals and generating dashboards. An AI agent for UI design takes this further by generating interactive layouts, forms, and controls that users can click and submit, instead of just...


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


How AI brings a new WAVE of transformation to SDLC automation


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


Article


[How AI brings a new WAVE of transformation to SDLC automation](https://www.griddynamics.com/blog/wave-framework-ai-sdlc-transformation)


Today, agentic AI can autonomously build, test, and deploy full-stack application components, unlocking new levels of speed and intelligence in SDLC automation. A recent study found that 60% of DevOps teams leveraging AI report productivity gains, 47% see cost savings, and 42% note improvements in...


[Cross-industry](https://www.griddynamics.com/blog/cross-industry)


### Subscribe to Grid Dynamics
insights now
