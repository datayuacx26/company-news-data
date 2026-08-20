---
schema_version: "1.0.0"
document_id: "63a6980ac136a7baef0697fd26294d0a48557942ed019ea49a2a8947add02b02"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/jira/high-throughput-engineers-ai"
published_at: "2026-08-10T04:06:01+00:00"
first_seen_at: "2026-08-13T01:30:56.386294+00:00"
fetched_at: "2026-08-13T01:30:58.023305+00:00"
content_hash: "sha256:4efcad9174e4ae57e5f0eaaaa8a182b2407f1915bfca83a837eed90508dc9150"
---

# What High-Throughput Engineers do Differently and Why AI Widens the Gap

We interviewed 15 high-throughput Atlassian engineers, identified through PR throughput data and peer nomination. All work on brownfield codebases: systems with years of history, broad surfaces, and layers of complexity. The most interesting finding: AI didn’t make their fundamentals obsolete – it accelerated them significantly **.** Those fundamentals are long-standing habits, but in an AI-accelerated world, the gap between engineers who apply them and those who don’t has become more noticeable.


Here’s what consistently came up:


## 1. Small, single-responsibility PRs: an AI amplifier, not just hygiene


The strongest engineers have always scoped work atomically — one responsibility per change — for the benefit of both human reviewers and agents.


AI changed the math on scope. Smaller changes are easier for a human or an agent to reason about, more reliable to self-test, and less prone to merge conflicts down the line. Faster builds lowered the cost of managing many small PRs, so work that used to be batched now splits naturally.


**Adopt:** make[small, single-responsibility PRs](https://atlassianblog.wpengine.com/development/scale-agent-impact-with-jira-automation) your default practice. Harness agents around it and teach it as one of the highest-leverage habits you can build to make AI agents more reliable.


## 2. Spec-driven development – an old discipline in a new form


[Spec-driven development](https://atlassianblog.wpengine.com/company-news/ai-sdlc) is not new. Writing the intent down before writing the code (requirements, use cases, domain logic, and so on) has always separated strong engineers from the rest.


The form has changed, though, the spec is no longer just a document a human reads, it’s the context an agent executes against. Newer AI-native frameworks like[BMAD](https://github.com/bmad-code-org/bmad-method) are essentially this old discipline repackaged for agents. The same structured-intent mindset now echoes what the community calls graph engineering: designing explicit workflows as nodes, edges, and shared state to give agents a structured map of intent rather than a loose prompt.


One high-leverage pattern we’ve observed is breaking down a project into tightly scoped Jira work items (we use a mix of tools, including our own[Jira planner](https://www.atlassian.com/software/jira/dev) , to do this). Those items are assigned to coding agents that run in parallel, and the engineer reviews the resulting draft PRs. The work item, not a chat or CLI, is what coordinates the atomic unit of work across the team and agents.


Several engineers prompt or harness the planner to minimise overlap between PRs and to sequence dependent work, so merge conflicts are reduced.


The richer and more complete the spec (repo context, code paths, acceptance criteria, test levels, gating), the more reliable the agent’s output tends to be. This is exactly why engineers who already had the “write the spec first” habit are getting the most out of AI today. Structured intent consistently outperforms ad-hoc prompting.


**Adopt:** treat planning as part of implementation. Decompose work into rich, self-contained work items, assign them to agents directly from your work item tracker, and keep the human on review. Let your work-tracking system, not a chat log, be the source of coordination and context.


## 3. A fast, trustworthy dev loop is the real unlock


One of the engineers rebuilt test mocks to behave like the real backend and added end-to-end coverage, this meant they could test 90% of changes from Storybook directly (up from 10%). Fewer bugs was only half the payoff. Strong coverage let AI test and fix most errors on its own, because *“the test coverage provided the spec, and it knew how to run these tests autonomously”* . Across the board, fast builds and trustworthy tests were cited as bigger enablers than any specific AI tool.


**Adopt:** invest in coverage, realistic fixtures, and fast builds. High coverage does double duty – it also becomes the specification your agents work against (see point 2).


## 4. Bounded parallelism: agents scale, attention doesn’t


Almost all 15 engineers converged on the same limit: **no more than 2–4 active, high-cognition tasks at once.** Pre-planned atomic work can be fanned out to agents freely, while anything requiring real thought stays capped. This is where the cheap end pays off: routine RtB / KTLO tasks (stale feature flags, flaky tests, dependency bumps) can be automated end-to-end — in our case, assigned to[agents straight from Jira workflows](https://www.atlassian.com/blog/development/ai-agents-jira-engineering-maintenance) or automation — while your deep-focus attention stays protected.


Several engineers were explicit that over-parallelising led to mental load, anxiety, and near-burnout. The lesson is about sustainability, not just productivity.


**Adopt:** use agents to parallelise bounded work, limit active human contexts, make dependencies explicit, and keep one person accountable for integration.


## 5. Warm contexts: offer options, not a mandate


High-throughput engineers manage parallel work by keeping multiple contexts warm and ready. Approaches vary: some use multiple git worktrees, others run several projects in one IntelliJ window, and some prefer separate checkouts across a laptop plus remote dev environments (to avoid re-indexing costs).


The specific tool or setup mattered less than the outcome: how cheaply and quickly they could switch between contexts without losing mental or technical state. As one engineer mentioned, the priority is tooling that supports parallel background workflows and preserves context. The failure mode is the opposite: so many half-loaded terminals, tools, and projects that switching between them costs more than parallelism saves.


Several engineers go further, deliberately keeping concurrent workstreams independent so they never collide in rebases or merge conflicts.


There is a hardware limit, however. Engineers on high-end machines can afford many warm local contexts; those on slower laptops benefit more from remote or cloud dev environments. This makes fast, reliable remote access a meaningful leadership investment rather than a personal productivity hack.


**Adopt** : Give engineers real options for parallel-context setups instead of mandating one approach. Ensure high-quality remote/cloud dev environments are readily available, especially for those with slower machines. As agents generate more work in parallel, the engineer’s ability to switch contexts quickly and cheaply becomes a key multiplier.


## 6. Fast review – reciprocal reviewer networks


High-throughput engineers rely on an informal “review economy,” trusted, fast, domain-routed reviewers who reciprocate. They often just bounce PRs off each other and turn them around quickly. Fast review is also a quality gate. The strongest engineers jump on reviews early precisely because they maintain a high bar: they want to catch weaker or riskier changes as soon as possible. Anecdotally, slower reviews, by contrast, increase the chance that “good enough” PRs slip through.


AI-assisted review can accelerate this further (draft comments, automated quality and compliance checks), but human judgment in the loop remains essential – especially for nuanced domain logic, architecture, and regulatory requirements.


**Adopt:** build reciprocal, domain-routed review networks and set team review SLOs. As code generation speeds up with agents, review velocity becomes the new bottleneck. Treat fast, high-quality review as a shared team capability, not an individual hero activity.


# What leaders should invest in


1. **Review capacity and ergonomics** . Reviewer load-balancing, domain routing, and visibility into open reviews, plus AI-assisted review where compliance allows. As generation speeds up, review becomes the binding constraint.
2. **Fast, trustworthy feedback loops** . Build and test speed, realistic fixtures, and coverage you’d stake a release on. This is the highest-leverage infrastructure spend that lets agents self-correct instead of handing errors back to a human.
3. **Structured work as the path of least resistance** . Well-scoped work items should be the easy option, so planning and delegating to agents is frictionless. If structuring the work is harder than doing it, engineers keep work and context in their heads and the agents stay idle.
4. **Parallel environment options** . Offer remote and cloud dev environments so parallel workflows aren’t limited to engineers with the best laptops. Practices only scale if the environment does.
5. **Sustainability as a first-class metric** . Multiple engineers linked peak output to anxiety and near-burnout. Throughput that cannot be sustained is not real throughput.


# The takeaway


[AI is a force multiplier](https://www.atlassian.com/blog/company-news/ai-sdlc) that multiplies what’s already there. The teams pulling ahead pair strong fundamentals (small PRs, fast trustworthy loops, structured planning) with agents that amplify them, coordinated through the work items that keep humans and agents pulling together. Fundamentals decide whether AI is a 2x or a 10x.
