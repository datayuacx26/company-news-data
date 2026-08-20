---
schema_version: "1.0.0"
document_id: "a716bd264b30fcf652d8267a6a1e1888296fe6e266266449074a987eedfe2ff0"
company_key: "yc-momentic"
company: "Momentic"
source_id: "yc-momentic-news-import-348aec23cbaf"
canonical_url: "https://momentic.ai/blog/how-agentic-testing-works"
published_at: "2026-05-26T00:00:00+00:00"
first_seen_at: "2026-07-24T05:02:31.373763+00:00"
fetched_at: "2026-08-19T18:58:31.052183+00:00"
content_hash: "sha256:bcb86e90da6646243760b03f460da0a76fe742392467f51445cce24e6235dc7b"
---

# Agentic Testing for Engineering Teams: A Definitive Guide (Part 2 of 2)

*This is the second part of our guide on agentic testing, covering how agentic testing works in practice and best practices for implementing inside your organization. You can check out the first part to understand what agentic testing is:[https://momentic.ai/blog/agentic-testing-guide](https://momentic.ai/blog/agentic-testing-guide)*


## How does agentic testing work in practice?


Underneath the four-verb definition, the technical architecture is converging fast. A few patterns are now well-established across vendors and open-source projects.


### Multi-modal page reading


The dominant pattern is accessibility-tree-first with a vision fallback. The a11y tree is the semantic representation of the page (the same one screen readers use), which makes it more stable across CSS and DOM refactors than raw selectors.


The agent reads the page by capturing a snapshot, a serialized text version of the tree at that moment. It looks something like this:


The LLM reads that snapshot, decides "I want to type in the textbox," and acts against ref=e5. Those reference IDs are local to this single snapshot: the next time the agent observes the page, it gets a fresh tree with fresh refs. Selector drift, the source of most flakiness in scripted suites, can't happen because there's no persistent selector to drift.


For surfaces the a11y tree can't describe well (HTML canvas, custom-rendered components, charting libraries), the agent falls back to a multimodal vision model interpreting the screenshot directly.


### Intent as the durable contract


Rather than page.click('#submit-btn-v2'), the test author specifies the intent: click the submit button. The agent's resolver, an LLM with multi-modal access to page state, picks the actual element at runtime. Intent is what the test author writes. Selectors are implementation details the agent works out for itself.


This matters because intent survives refactors. “Click the submit button” stays valid when the button changes from a <button> to an <a>, when its class name is regenerated, when it's wrapped in a new component, or when it moves up a level in the component tree. None of those changes touch the intent, so none of them require the test author to do anything.


### Observation loops


After each action, the agent re-reads the page and decides whether to proceed, retry, or revise the plan. The pattern follows[ReAct](https://arxiv.org/abs/2210.03629) :


1. **Thought** . What should I do next?
2. **Action** . Run a browser command, like type or click.
3. **Observation** . Read the result through the multi-modal signals above.
4. **Repeat** . Until the success criterion is met or the step budget is exhausted.


When the loop stalls or fails,[Reflexion-style](https://arxiv.org/abs/2303.11366) self-critique kicks in: the agent writes a textual reflection on what went wrong and seeds the next attempt with it. Momentic exposes this as[failure recovery](https://momentic.ai/docs/reliability/failure-recovery) , where the agent proposes corrected steps after a failed run that an engineer accepts or rejects in the run viewer. The broader pattern most production tools use is a hybrid: a planner emits a step-indexed sequence up front, then a per-step reasoning loop runs inside each step.


### Locator caching and self-healing


Pure agentic execution is slow and expensive. Every step costs at least one LLM call, and a hundred-step flow with model latency adds up to minutes per test. The solution that every serious tool has converged on is to cache the resolved locator after the first successful run, replay it deterministically on subsequent runs, and fall back to AI resolution only when the cached path is missed.


This is the technically honest version of “self-healing tests.” It's a cache-and-revalidate loop. The cache stores enough to replay the action deterministically. When the cached path misses, the agent re-resolves using the same multimodal grounding, updates the cache, and continues.


The speed difference is significant:[cached steps run only 52ms slower](https://momentic.ai/docs/performance) on average than equivalent Playwright commands. Only first-run, uncached steps pay the full AI cost, and over 99% of steps execute in under 500ms once cached.


What this means operationally:


- First-run tests are slow and rely on LLM inference at every step.
- Repeat runs are fast and mostly deterministic, with LLM inference only on cache misses.
- UI changes don't break the test; they just trigger a cache update on the next run.


The cache is one layer of the agent's run-to-run state. Momentic’s[memory](https://momentic.ai/docs/reliability/memory) is a record of past runs that captures which assertions tended to need retries, where the agent took alternative paths, and which signals proved authoritative for which flows. Caching keeps repeat runs cheap. Memory helps the agent reason consistently when conditions shift between runs.


The combination of intent-based specification, multi-modal verification, and cache-and-revalidate execution is what makes agentic tests cheap enough at steady state to actually deploy. Without the cache, the cost model doesn't work.


### Integration with the coding agent's workflow


Three integration surfaces matter, depending on where the agent is running.


#### 1. MCP servers


Model Context Protocol lets any IDE-resident coding agent (Claude Code, Cursor, Codex CLI, Copilot Chat, Windsurf) call tools as native tool calls. For agentic testing, this is the integration surface where the coding agent verifies its own work inside its working loop, before any code reaches CI.


What the MCP exposes matters. Playwright MCP and Stagehand expose browser primitives (click, type, navigate, snapshot).[Momentic's MCP](https://momentic.ai/docs/integrations/mcp-server) exposes the test layer directly: the agent can create a test, run it against a live browser, inspect traces, and classify failures without leaving the editor.


#### 2. CLI and skills


For high-throughput coding agents, CLI invocations can beat MCP on context cost. CLI calls don't load large tool schemas or verbose accessibility-tree snapshots into the model's window, leaving more room for code. Microsoft recommends this pattern for Playwright; agents discover the CLI syntax through structured markdown skill files that ship with the tool.


Momentic ships the same pattern. The[Momentic CLI](https://github.com/momentic-ai/cli) runs and manages tests. Matching[skill files](https://github.com/momentic-ai/skills) (npx skills add momentic-ai/skills. momentic-test for authoring and execution, momentic-result-classification for failure analysis) tell the coding agent how to invoke it. More setup than the MCP route, less context overhead per action.


#### 3. CI pipeline


[Agentic testing in CI](https://momentic.ai/docs/running-tests/ci) works like any other test suite. Platforms integrate with GitHub Actions, GitLab CI, and CircleCI through standard CLI calls: install browsers, run tests, and upload results. Sharding splits large test sets across parallel jobs, and a merge step aggregates outputs into a single run group for review.


What's distinct is what attaches to the pull request. Beyond pass/fail, the agent's trace (the recorded sequence of what it saw and did at each step), along with screenshots, videos, and the agent's classification of any failures, accompanies the PR. The reviewer doesn't need to re-run the test locally to understand what happened.


The strategic point is that agentic testing is most valuable inside the coding agent's working loop, not as a post-merge gate. A coding agent that writes a feature, opens a browser through MCP, navigates its own change, and verifies behavior before opening a PR is doing the modern version of shift-left.


## Who owns agentic testing?


Testing ownership has moved several times over the last twenty years. Dedicated QA organizations gave way to SDETs. SDETs gave way to “every engineer owns their tests” in the shift-left era. Each shift was driven by changes in how software was built. Agentic testing is the latest such shift.


When the agent owns the execution and maintenance of tests, the human's leverage shifts to a different layer. The work isn't writing the script anymore. It's:


- **Specifying intent precisely** . What does this test actually verify?
- **Picking authoritative signals** . When does network state matter more than the DOM?
- **Auditing the agent's traces** . Did it take the right path?
- **Tuning the verification policy** . How should this kind of failure be handled next time?


That set of responsibilities doesn't map cleanly onto any existing job title. It pulls from QA, SDET, devex, and staff engineering. In practice, whoever takes it on is doing something closer to that of a “ **verification engineer** ” than to any of the historical roles. The dev who built the feature and the QA who tested it collapse into the same engineer: the one who specified what the agent should verify.


Failure triage is the clearest place where the role is already taking shape. Momentic's[ai classify](https://momentic.ai/docs/cli-reference/momentic/commands/ai) tool sorts failed runs into categories with the agent's confidence and reasoning attached:


- Related application bug
- Unrelated application change
- Test can be improved
- Infrastructure issue
- Performance regression


The agent does the first pass. The human validates, marks intentional product changes, and tunes the test where the agent's verdict was off. Automatable enough to be a product feature, not trustworthy enough to run unattended.


## When do you need agentic testing?


If AI coding agents are in active use on your team, you're past the point where scripted automation can keep pace. Other diagnostic frameworks (test maintenance burden, suite trust, headcount allocation) are real, but they're all downstream of this one.


A few reasons coding agents change the math:


- **The volume of new code is materially higher** . 46% of code is AI-generated for active Copilot users, up from 27% at launch. More code shipped per week means more UI changes per week, and the testing surface compounds.
- **PRs touch a broader surface** . Renaming props, restructuring components, and regenerating CSS classes is routine work for a coding agent. A single PR can break a dozen selectors at once.
- **Tests don't get maintained alongside the refactor** . A human refactoring a button often updates the related tests in the same PR. A coding agent rewriting application code rarely reasons about which tests it just broke, so breakage builds silently across the suite.


If your engineers are still hand-writing most of their code, scripted automation is fine. The moment Copilot, Cursor, Claude Code, or similar agents start landing meaningful volume in your codebase, that calculus changes.


## How can teams prepare for agentic testing?


Agentic testing fits well with E2E user journeys. Long flows that hit multiple screens, depend on stable application intent, and change shape every few weeks are precisely where the cache-and-revalidate model pays off. It's less suited to deep backend logic, tightly regulated assertions, and reproducibility-critical work, all of which still belong in your existing harness. Two practices make the rollout substantially cheaper.


### Budget the first-run compute


Every fresh agentic test pays the full AI cost on its first execution: locating elements, evaluating assertions, and reasoning about page state. A 100-step suite can take 10-15 minutes on the first pass. Convert incrementally rather than bulk-migrating, prioritize the highest-churn E2E flows first (signup, checkout, onboarding, primary feature paths), and let the cache warm before benchmarking steady-state speed. Teams that try to flip a 500-test suite in one sprint burn tokens and end up with a slow first run nobody planned for.


### Build an exclude list before you build the test plan


Some categories don't belong in the agentic pipeline:


- Financial calculation rules and precision-sensitive math, where exact behavior matters more than maintenance cost.
- Compliance-driven assertions (HIPAA, SOC 2, PCI) where regulators may ask for bit-for-bit reproducible traces.
- Recommendation engines, generative AI features, or other surfaces with legitimate output variance that the agent might silently accept as correct.
- Heavy backend logic that's better covered by unit and integration tests than by browser-driven E2E verification.


Codify this upfront so engineers know which surfaces to skip when authoring agentic tests. The exclude list also doubles as a teaching artifact: it's the clearest way to communicate where agentic verification is reliable and where it isn't.


Done well, the rollout looks less like rip-and-replace and more like a planned shift of test ownership. Agentic handles the high-churn E2E surface, where the maintenance dividend is largest. Scripted, unit, and integration tests hold the high-precision rest. Most production stacks land on this hybrid model as a permanent steady state.


## The shape of agentic testing


Three things make agentic testing a distinct category rather than a feature bolted onto existing tools.


1. **Intent, not the selector, is the durable contract** . Everything downstream of that, from how the test reads the page to how it heals, follows from this single architectural choice.
2. **Verification is multi-modal by default** . A scripted test asserts on one or two signals. An agentic test fuses DOM, accessibility tree, screenshots, network, and console, and decides what's authoritative based on the page in front of it.
3. **The test runs inside the coding agent's loop** . The point of agentic testing in 2026 isn't to replace your CI suite. It's to give the AI agent writing your code a way to check its own work before it ships, in the same loop, with the same tooling.


This is the bet behind what Momentic builds. The test author specifies intent in natural language. The agent handles planning, running, observing, and adapting across multiple signals. The locator cache speeds up repeat runs. And the whole thing is reachable from Claude Code, Cursor, Copilot, or your CI without a separate orchestration layer.


### FAQs


1. **Is agentic testing the same as self-healing locators or AI codegen?**
No. Self-healing locators run within a scripted test; the script defines every step, and the AI just patches the selector when it breaks. AI codegen produces Playwright or Cypress code from a prompt that humans then maintain. Agentic testing has no script underneath. The test runtime is the agent itself, so there's nothing to heal and nothing to regenerate.
2. **Does agentic testing replace Playwright or Cypress?**
For most teams, no. Agentic testing fits high-churn end-to-end user journeys where the cache-and-revalidate model pays off. Backend logic, precision-sensitive math, and compliance-grade reproducibility still belong in your existing harness. Production stacks usually run in an agentic, permanent hybrid alongside scripted, unit, and integration tests.
3. **What does writing an agentic test actually look like?**
You write intent rather than code. A test reads something like: “Open the analytics dashboard, switch the date range to last 30 days, and verify the revenue chart re-rendered with data for the new range, the metrics API returned 200, and the console stayed clean.”
That one intent fuses three signals a scripted suite would check separately: a visual assertion on a canvas-rendered chart (which selector-based tests can't read at all), a network check, and console health. Most platforms render the intent as YAML or a similar structured format, so tests stay version-controlled and reviewable in PRs. The author says what to verify; the agent works out which elements to touch at runtime.
4. **How long does adoption take?**
Less than building the original suite, more than the first conversion. Start with 5-10 of your highest-churn E2E flows (signup, checkout, primary feature paths), validate the agent's verdicts for two to four weeks, then expand. Most teams run their existing scripted suite in parallel for the first quarter, then start retiring scripts where the agent has proven reliable.
5. **Does it cover mobile and API testing, or just web?**
Web is the most mature surface, and where most platforms started. Mobile (Android and iOS) is now supported by leading platforms, including Momentic, using the same primitives applied to native UI trees. API testing is generally outside the scope: agentic tools are designed around UI verification, and most API testing is better served by contract tests, schema validation, or integration tests that don't need an LLM in the loop.


*Don't forget to read what agentic testing is, what it isn’t, to understand the theory behind agentic testing and why it’s so important now:[https://momentic.ai/blog/agentic-testing-guide](https://momentic.ai/blog/agentic-testing-guide) .*
