---
schema_version: "1.0.0"
document_id: "33ebb043cc8b466bd97e8eaf8795fae25c99486ded3f976bf737a41b913d03f3"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/why-ai-tools-fail-at-service-boundaries/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:fefc5f515952b509f1fd0a5984084ae31ae449c9cc29fd6cb62a3246ce04f0d5"
---

# The Multi-Codebase Problem: Why AI Coding Tools Fail at Service Boundaries

# The Multi-Codebase Problem: Why AI Coding Tools Fail at Service Boundaries


Your agent can only reason about code it can see. In a microservices architecture, it can't see enough.


Apr 14, 2026 — 7 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


AI coding tools have a locality assumption. They reason about code on a local file system. In a monolith, that assumption holds. In a microservices architecture with 200+ repos, it breaks completely.


We see this consistently across our customers with distributed architectures. The tools are failing because they can only see one repo at a time, and the information they need lives in another one.


As one engineering leader at a customer with over 200 microservices put it: “A lot of our engineers have a hard time figuring out where everything is. But they know the problem statement.”


That gap between knowing the problem and finding where to solve it is one of the hardest challenges in AI-assisted development across distributed systems. And it’s a structural problem, not a tooling problem. As services grow linearly, their interconnections grow nonlinearly. The dependencies between them are often implicit, encoded across shared data structures that no single team fully owns. The tradeoff for microservices’ autonomy has always been this complexity. AI coding tools just made it more visible.


## Why AI Coding Tools Fail Here


Highly distributed, multi-codebase systems have structural properties that current AI coding tools aren’t designed for. Three in particular.


**The co-locality problem.** AI coding tools reason about code they can see. In a microservices architecture, no developer has every relevant repo cloned, open, and indexed simultaneously. An engineer working in the order service asks their agent how payment processing works. The agent can see the order service code. It finds an HTTP client call to the payment gateway. But it can’t look inside that service. It can’t read the handler on the other end. It can’t verify the contract. The agent is blind to most of the architecture most of the time.


**Implicit dependencies.** In a monolith, dependencies are explicit: imports, function calls, class hierarchies. An agent can trace them. In a microservices architecture, Service A calls Service B over HTTP, but the connection is a URL string in a config file, not an import. Service C reads from a message queue that Service D writes to, but they share no code. Two services read from the same database table without either one knowing about the other. These connections are real and critical, but they’re invisible to any tool that reasons about code in isolation. Retrieval-based tools can’t find what they don’t know to look for.


**Context that doesn’t scale.** Teams recognize this and try to solve it. They maintain documentation per repo, build internal context tools, try various RAG pipelines. These approaches work at small scale. They collapse as the number of services grows. Documentation goes stale. Internal tools become their own maintenance burden. RAG-based approaches chunk code into text fragments, destroying the structural relationships that matter most in distributed systems. (We’ve written at length about[why current approaches to context fail](https://www.driver.ai/blog/why-current-approaches-fail) and[why we built a compiler instead of a search engine](https://www.driver.ai/blog/intelligence-efficacy-why-we-built-compiler-not-search-engine) .)


AI coding agents fail in microservices architectures because they operate under a locality assumption: they can only reason about code within their current session. In a distributed architecture, the codebase context an agent needs to make correct changes often lives in a different repo entirely. Service connections are URL strings in config files, not traceable imports. Shared database tables create coupling that neither service’s code reveals. These implicit, cross-service dependencies are invisible to any tool reasoning about code in isolation. The agent isn’t unintelligent. The codebase context it needs doesn’t exist within its field of view.


## What a Solution Requires


A real solution to the multi-codebase problem needs four properties:


1. Deep understanding of each individual codebase
2. All codebases accessible through a single interface, with no co-locality requirement
3. A runtime capability that can synthesize understanding across codebases and trace implicit connections
4. Automatic freshness. Context that stays current without manual maintenance. This is table stakes.


We built Driver to deliver all four. The details of how our transpiler works, how we combine static analysis with LLM generation to produce symbol-complete context, and why compilation beats search are covered in depth in[our earlier posts](https://www.driver.ai/blog/why-current-approaches-fail) . For this post, what matters is what these capabilities look like specifically for multi-codebase architectures.


**One connection, all codebases.** Every codebase your organization onboards is accessible through a single MCP integration. Your AI coding tools connect once, and they can query architecture documentation, navigate directory structures, read file-level documentation, or access source code from any codebase in your organization. The code doesn’t need to be on the same file system because the codebase context already is.


**Cross-codebase context synthesis.** This is the capability that matters most for microservices. Our` gather_task_context` tool is a runtime agent: you describe what you’re trying to accomplish and which codebases are relevant, or let it determine this itself. It reads the pre-computed context for each codebase, navigates into the specific details that matter, and synthesizes a unified answer. The implicit dependencies that are invisible to tools reasoning about code in isolation are captured in the pre-computed context. The runtime agent reads across all of them and traces the connections.


**Incremental updates on every push.** When code changes, we re-analyze only what changed and what it affects. The codebase context stays current without anyone maintaining it.


## What This Looks Like in Practice


These aren’t hypothetical use cases. They’re patterns we see across our customers with large multi-codebase architectures.


**Cross-service debugging.** A customer had a bug where bookings were completing despite having no payments recorded. The investigation spanned 4-5 applications across order orchestration, admin, and payment services. This bug had been investigated multiple times before and missed, because previous investigators didn’t have cross-service context.


One engineer used Driver to investigate iteratively: broad exploration first, then narrowing hypotheses based on domain knowledge, then precise constraints. Once the engineer clarified that the issue wasn’t failed payments but *no payments existing at all* , Driver pinpointed the exact file, method, and line numbers where the validation logic silently passed orders with zero payment records. Total hands-on time: roughly 30 minutes of reading, refining, and iterating. As the engineer put it: instead of needing three people with intensive knowledge of different services, one engineer got 80% of the way there, then brought in specialists to confirm.


**Automated cross-repo tasking.** One customer with over 200 microservices built a skill on top of Driver that automates the breakdown of Jira stories into per-repo implementation tasks. Given a single ticket, the skill identifies which repos are impacted, calls` gather_task_context` for each one in parallel, and creates subtasks with specific file paths, class names, and patterns from Driver’s context. The engineer tasking the story doesn’t need to know every service’s internals. The context is already there.


Before this, they were building their own tool for multi-repo edits. The constraint, as one of their engineers described it: “you’ve got to know which repos.” With codebase context for all their repos accessible through a single integration, that constraint disappeared.


**Onboarding to unfamiliar services.** The same customer reorganized teams, putting engineers on codebases they’d never worked in. In a microservices architecture, this is a common and particularly painful form of context switching. Not just a new repo, but a new service with its own conventions, its own integration points, and its own implicit dependencies on other services nobody documented. With Driver, the team’s agents had immediate access to architecture overviews, onboarding guides, and cross-codebase context for their new domain.


## Results


We’re seeing strong early signals across customers using Driver with multi-codebase architectures.


One customer is measuring nearly double the industry P90 for software development throughput, tracked through GetDX. Their deep context agent usage is an order of magnitude higher than we anticipated, which tells us something about how much latent demand exists for cross-codebase context when it’s actually available.


These results are early. We expect them to sharpen as more customers at this scale adopt and as we continue to optimize the cross-codebase synthesis capabilities. But the pattern is consistent: the more codebases, the more total complexity and fragmentation, the more immediately the value compounds.


## Beyond Microservices


Microservices are the most recognizable version of this problem, and the one where we have the strongest early results. But any organization with many codebases faces the same structural challenges: co-locality constraints, implicit cross-codebase dependencies, context that can’t scale with the number of repos.


Some of our most engaged customers run architectures with hundreds of codebases and tens of millions of lines of code. The multi-codebase problem is general. Microservices just make it obvious.
