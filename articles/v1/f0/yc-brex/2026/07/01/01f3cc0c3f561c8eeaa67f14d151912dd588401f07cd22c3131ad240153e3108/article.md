---
schema_version: "1.0.0"
document_id: "01f3cc0c3f561c8eeaa67f14d151912dd588401f07cd22c3131ad240153e3108"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/agent-automations-for-big-migrations"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-23T23:31:30.406837+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:a5cba5b4c641732c4d6bdde524c3ae25463f6c8549fc2827b901c38e0677ee70"
---

# Agent automations for big migrations: How we accelerated framework upgrades by 32x with Cursor

Large-scale framework migrations are notoriously painful. While essential for security, performance, and keeping a tech stack current, they frequently drain developer morale, derail product roadmaps, and stall innovation due to the amount of time and effort required to troubleshoot every compilation and runtime errors that surfaces.


Our core Kotlin framework at Brex is Micronaut and we use it across over 500 microservices. Last year, we tried migrating just 50 of these services from Micronaut 3 to 4. It took us 3 engineers working full time for an entire quarter and was an expensive, manual grind that forced us to pause other critical product initiatives.


A year later, we took a completely different approach. We built a pipeline of custom self-hosted AI agents in Cursor that are capable of writing, building, and dynamically self-correcting code. This allowed a single engineer to migrate 180 microservices in just one month.


This represents a **32x per capita acceleration** in delivery speed. Below is how we arrived at this highly efficient system, along with the missteps we made along the way that are worth understanding.


### **The first attempt: A manual grind**


When we first tackled this migration, it was in the middle of 2025, before the frontier models we have today. We evaluated OpenRewrite OSS, a tool for deterministic, recipe-based refactoring in the JVM ecosystem, but its Bazel support was limited and wouldn't integrate well with the custom Bazel tooling our monorepo relies on. We then built a straightforward script to automate predictable, repetitive tasks, primarily updating version flags in build targets and replacing import statements.


Once the script ran and generated a Pull Request, the rest of the lifecycle fell entirely on the engineer:


- Inspecting CI failures and manually writing fixes.
- Deploying the services to our development environment to catch runtime exceptions.
- Executing integration tests against the new version and resolving compatibility issues.


Every time we encountered a new error pattern, we documented it in a shared migration guide to streamline subsequent fixes.


This migration approach was painful, slow, and manual for a few reasons. The Micronaut framework sits underneath nearly every line of code in a service, wiring components together and pulling in dozens of supporting libraries (for JSON parsing, GraphQL, caching, and more) that all need to upgrade in lockstep.


Many of the resulting failures don't even surface at build time, instead appearing only once the service is running, like a component failing to wire on startup, a test silently no longer testing what it claims to, or a date being serialized with slightly different precision. Each of the 500+ microservices had to be individually rebuilt, redeployed, and revalidated against the new runtime, which is what made the original effort so expensive.


Many of the breaking changes required real code rewrites whose correct form depended on context. For example, the GraphQL library upgrade made many return types nullable, so call sites that used to be straightforward:


Now had to handle a nullable value. If the field was a runtime invariant, the fix was an explicit assertion:


If it was genuinely optional, the call site had to branch instead:


Each call site needed to be evaluated against the schema and the surrounding code, which is the kind of judgment a deterministic recipe can't make.


Recognizing the mounting friction, we paused the migration after one quarter. With three full-time engineers dedicated to the effort, we had managed to migrate only around 50 microservices.


### **The second attempt: What failed before it worked**


Nearly a year later, the migration had become critical. Postponing it further was beginning to block vital downstream dependency upgrades. However, LLM capabilities had advanced significantly in that short window, giving us renewed confidence that we could successfully automate the workflow.


Initially, we attempted to build a single, massive AI agent to handle the entire lifecycle: making code changes, running validations, and fixing errors. The underlying prompt was an exhaustive wall of text, stuffed with every edge case we had documented.


This approach failed for two primary reasons:


- **Context Overload & Hallucinations:** Overburdened by the massive prompt, the agent frequently experienced context fatigue. It hallucinated library methods, missed straightforward breaking changes, and bypassed critical validation gates just to force a build to pass.
- **The Operational Overhead of Local Execution:** We executed these runs on local laptops, connecting to multiple remote development environments to process migrations in parallel. This completely defeated the purpose of automation. If a laptop went to sleep, dropped its Wi-Fi connection, or was closed for a meeting, the agentic loop broke entirely. Instead of eliminating manual oversight, we had simply shifted our focus from debugging the code to monitoring the AI.


To scale, we had to drastically separate concerns, optimize the AI's context window, and move execution compute off local machines.


### **The architecture: A multi-agent pipeline**


We solved our scaling challenges by implementing an end-to-end, multi-agent AI pipeline using custom, self-hosted Cursor agents to fully automate the Micronaut 4 upgrade across Brex’s microservices ecosystem. This required three critical architectural pivots.


**1. Splitting into a Dual-Agent System**


We decoupled the cognitive load into two distinct agent archetypes working sequentially:


- **The PR Generation Agent:** Triggered automatically by the creation of specific tickets in Linear, our project management tool, this custom agent in Cursor handles the initial heavy lifting. It combines deterministic scripting (for predictable syntax changes) with LLM reasoning (for complex, contextual refactoring). It runs local builds, catches compilation and syntax errors, self-corrects on the fly, and opens a draft Pull Request (PR).
- **The PR Validation Agent:** Once the Generation Agent opens a PR and applies a specific label, the Validation Agent takes over to monitor the continuous integration (CI) pipeline. It deploys microservices to development environments and executes integration tests. If it encounters runtime or CI errors, it automatically patches them, up to a strict three-attempt threshold. If it hits a systemic blocker, it halts and outputs a clean, human-in-the-loop report as a PR comment.


**2. The Hybrid Approach (Scripts + Modular Skills)**


We stripped down our prompts. Deterministic tasks — BUILD attribute flips, dependency swaps, and YAML config rewrites — were offloaded to buildozer and Python scripts, while Kotlin source transforms like import rewrites, annotation fixes, and structural changes (e.g., object to class conversions) were handled by a PSI-based tool that operates on the Kotlin AST directly, making them precise and repeatable without LLM involvement. For non-deterministic compilation errors, instead of stuffing every solution into the core prompt, we built modular skills. The agents dynamically fetch only the relevant skill when a specific error is triggered, keeping the context window clean.


For example, when the build fails with a nullable receiver error on a call like \`env.getSource<User>()\` (the kind of failure shown earlier), the agent doesn't reason about it from scratch. It classifies the failure, loads the \`micronaut-4-nullability\` skill, and uses its documented rules to choose between an explicit \`requireNotNull\` assertion when the field is a runtime invariant and a \`?.let { ... }\` branch when it's genuinely optional. The same dispatch pattern handles GraphQL dependency rewrites, MapStruct injection failures, proxy interception failures, timestamp precision drift, ktlint violations, and so on, each routed to a single small skill instead of one giant prompt.


In total, we wrote eleven skills, each scoped to one class of failure. Both agents draw from this shared library, what differs is when each fires. The Generation Agent reaches for skills while resolving local build failures during the initial PR creation. The Validation Agent reaches for the same skills while triaging PR CI failures, deploy errors, and integration test breakages on the already-open PR.


**3. Migrating to Self-Hosted Cursor Agents**


To eliminate the local machine bottleneck, we transitioned execution to custom, cloud-based, and self-hosted Cursor agents.


First, we configured the self-hosted system within Cursor, granting it secure access to our cluster so it could execute tests, deploy microservices to the development environment, and run integration tests without requiring manual human authentication. We provisioned the dedicated nodes on which the agents would run, configured headless authentication (with only the required permissions), and adapted our internal CLI tool to interact seamlessly with the agents without requiring any interactive prompts.


With this foundation, we wired up the custom agents, using ticket creation as the trigger for the first agent and a GitHub PR label for the second.


**The Result:** Today, the workflow is entirely asynchronous. An engineer can create 20 tickets at 5:00 PM and close their laptop for the day. Overnight, the cloud infrastructure spins up, executes the pipeline, self-corrects errors, and completes integration tests. The next morning, the engineer opens their laptop to find completed draft PRs waiting for a human review.


### **Architectural tradeoffs and constraints**


Building agentic workflows at scale requires strict boundaries. We designed our system around practical constraints:


- **The 3-Attempt Cap:** We placed a strict limit on agent self-correction. If an agent cannot fix a failing test after three attempts, it stops, documents its findings, and flags the PR. This prevents infinite loops and runaway API costs on deeply rooted architectural blockers.
- **Not Fully Autonomous:** We deliberately chose not to allow agents to automatically merge code into production. A human review is always required.
- **No Universal Prompting:** We didn't try to build a "magic" migration prompt that works across all frameworks. We focused strictly on a highly optimized, context-isolated Micronaut 4 setup.


### **The impact: Changing the math on tech debt**


This approach completely shifts how we manage technical debt at Brex. Framework upgrades no longer disrupt our product roadmaps. By automating the Micronaut 4 migration, we unblocked critical downstream engineering initiatives, such as our GraphQL federation upgrade, and patched widespread security vulnerabilities across 180+ services without straining our core engineering teams. This success has redefined our strategy for large-scale migrations moving forward.


Beyond the massive acceleration in delivery speed, shifting from manual engineering hours to a token-based AI pipeline fundamentally changes the economics of tackling technical debt.


When comparing the infrastructure and token costs against dedicated engineering time, **we decreased our cost per microservice migration by approximately 90%** .


Instead of sinking a large portion of the team's quarterly budget into manual refactoring, the combination of self-hosted compute and targeted LLM reasoning allowed us to migrate double the number of microservices at a fraction of the traditional cost. This transforms framework upgrades from an expensive roadmap blocker into a highly cost-effective background process.


### **Next steps: Automated post-deployment monitoring**


Even with a dual-agent system executing local builds and running integration tests, moving at this velocity exposed a classic engineering vulnerability: fragile test coverage and monitoring gaps.


During the migration, a few automated PRs passed all existing unit and integration tests cleanly, only to cause subtle runtime bugs once they hit production traffic. The agents had done their jobs perfectly based on the rules they were given, but because certain services had gaps in their test suites or lacked robust monitoring, anomalies slipped through.


An agent is only as good as the validation gates you build for it. If your tests are bad or absent, your agent is flying blind.


To de-risk the remainder of this migration, we are looking beyond repository-level automation and exploring how to extend our agents into the deployment pipeline itself. We are currently developing environment-aware agents to monitor staging and production environments, watching deployed services to detect and flag any subtle anomalies related to the migration in real time.


To achieve this, we are using our observability vendor MCP and testing out an AI SRE agent API, the agent tool our teams currently use to investigate incidents and alerts. The automation identifies all microservices migrated over the past week by querying tickets, extracting the relevant deployment names from the codebase, and then leveraging our observability vendor MCP and the AI SRE agent API, to autonomously detect and investigate potential anomalies.
