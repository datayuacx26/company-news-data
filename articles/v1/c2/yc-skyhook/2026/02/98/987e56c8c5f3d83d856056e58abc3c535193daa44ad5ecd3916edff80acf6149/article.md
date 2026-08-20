---
schema_version: "1.0.0"
document_id: "987e56c8c5f3d83d856056e58abc3c535193daa44ad5ecd3916edff80acf6149"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/reverse-ml-using-ai-to-write-rules-not-run-them"
published_at: "2026-02-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T22:22:47.419354+00:00"
content_hash: "sha256:d6ae1f2d41e0e93e32a48109d49019d0658fcd9e29fef4f434f4decc18317e3b"
---

# Reverse ML: Using AI to Write Rules, Not Run Them

Everyone's racing to add LLMs to their runtime. We went a different direction.


When we needed to build a system that detects programming languages, frameworks, and configurations from codebases, the obvious approach was to call an LLM at request time. Feed it the file tree, let it reason about what it sees, return a classification. GPT-5, Claude or Gemini would probably get it right 90% of the time.


For this feature, we do use AI at runtime - but not as the starting point. Instead, we used AI extensively during *development* to generate deterministic rules that execute in milliseconds. These rules handle 90%+ of cases instantly. When they're uncertain, we optionally invoke AI - but with a head start: structured evidence and a preliminary classification that focuses the AI on specific ambiguities rather than cold-start exploration.


We call this pattern **Reverse ML** : instead of training a model on data, you use AI agents to explore real-world examples and synthesize explicit rules. AI at development time to build the rules; deterministic execution by default; AI at runtime only when needed, with context that makes it faster and more accurate.


## Why Not Just Use AI at Runtime? Or Just Write Rules by Hand?


There are three options for a classification problem like this:


**Option 1: AI at runtime.** Let an agent explore the repo, reason about what it finds, return a result. Modern agents are good at this. But a thorough exploration takes 2-5 minutes - dozens of tool calls, reading files, reasoning about conflicts. For a CLI tool where detection is the first thing users experience, that latency kills the UX. You also lose determinism (same repo, different runs, different answers), auditability (good luck explaining a decision buried in 40 tool calls), and you pay $0.10-0.50 per repo at scale. (For Kubernetes-specific agent workloads, our[Radar MCP server](https://radarhq.io/product/mcp) is the runtime surface - agents query structured cluster state instead of scraping` kubectl` output.)


**Option 2: Hand-written rules.** Engineers have built rule-based systems for decades. But the work doesn't scale. Labeling a testbed - thoroughly analyzing each repo to establish ground truth - takes 15-30 minutes per repo. Investigating detection failures takes 30-60 minutes each: open the repo, read manifests, check the Dockerfile, cross-reference framework docs, iterate. For 100+ repos across dozens of frameworks, you're looking at months. It's the kind of project that never gets prioritized because the ROI doesn't justify the effort.


**Option 3: AI-assisted rules.** Use AI at *development time* to do the tedious work - labeling repos (2-5 minutes instead of 30), investigating failures (minutes instead of an hour), exploring edge cases in parallel. The output is the same readable, deterministic rules a human would write. AI just makes it economically feasible to build something this comprehensive.


We chose option 3. Not because any single factor is decisive - if latency doesn't matter, if you're already building production agent infrastructure, runtime AI might be simpler. But for our constraints (instant CLI response, CI/CD determinism, auditability, and a small team that can't spend months on manual investigation), the tradeoffs made sense.


The novel part isn't the rules themselves. **It's the AI-assisted development process that makes comprehensive rule-building practical at scale.**


## How It Works: A Concrete Example


We built a codebase detection system that identifies languages, frameworks, ports, and configurations from project files. The goal: reduce manual onboarding inputs from 15+ fields to zero.


Here's the development process, step by step.


### Phase 1: Build a Testbed (AI-Labeled)


First, we curated 100+ real repositories covering:


Category Examples Purpose


Popular frameworks Express, FastAPI, Spring Boot High-frequency patterns


Emerging tools Bun, Deno, Hono Future-proofing


Monorepos Nx, Turborepo, Lerna Complex structures


Edge cases No manifest, multiple Dockerfiles Robustness


Failure modes Conflicting configs Error handling


Here's where AI helps first: **establishing ground truth** . For each repo, an AI agent spends 2-5 minutes doing thorough analysis - reading files, reasoning about project structure - to determine correct classification. Language, framework, port, build commands.


Manually labeling 100+ repos would take days. With AI running in parallel, we have ground truth in hours. We trust SOTA agents to get this right when given time to explore - same analysis a careful human would do, just faster.


Is this ground truth perfect? No. But it's good enough to catch rule regressions, and we can always manually verify the cases that matter most. The alternative - manually labeling everything - doesn't scale.


This testbed is our training set. Every rule change gets validated against all 100+ repos.


### Phase 2: AI-Assisted Rule Development


When detection fails on a repo, we launch a coding agent (Claude Code) to explore:


```text
Human: "Detection says this is Go, but it's actually Node.js.
Explore ~/testbed/problem-repo and figure out why."


Agent:
1. Lists directory structure
2. Reads package.json, go.mod, Dockerfile
3. Analyzes file contents and patterns
4. Identifies root cause
5. Proposes a detection rule


```


A typical exploration session:


```text
> The repo has both go.mod and package.json. Let me check each...


> go.mod exists but only contains tooling (golangci-lint, mockgen)
> package.json has application deps (express, typescript)
> Dockerfile uses node:18 base image


> Root cause: We detected Go first because go.mod exists, but this
> is actually a Node.js project that uses Go for build tooling.


> Proposed rule: When both manifests exist:
>   1. Check Dockerfile base image (highest signal)
>   2. Check if go.mod deps are tooling-only
>   3. Compare source file counts (.go vs .ts/.js)
>   4. Adjust confidence based on evidence strength


```


### Phase 3: Translate to Deterministic Code


The AI proposes rules in plain language. We translate to code:


**AI proposal:**


> "If package.json has only devDependencies and no dependencies/main/bin fields, it's likely a script runner, not a Node.js application. Reduce confidence by 30%."


**Implementation:**


```text
func   isScriptRunnerPackageJson  (  pkg   *  PackageJSON  )   bool   {
// No application entry points
if   pkg.Main   !=   ""   ||   pkg.Bin   !=   nil   ||   len  (pkg.Dependencies)   >   0   {
return   false
}
// Only has devDependencies (build tools, linters)
return   len  (pkg.DevDependencies)   >   0
}
```


This rule now runs in microseconds, with zero AI involvement.


### Phase 4: Validate and Iterate


After adding a rule, we re-run the entire testbed:


```text
$   skyhook   test-detection   ~/testbed   --html   report.html


Testbed   Results
───────────────────────────────────────
Total   repos:       127
Correct:           121   (95.3%)
Wrong:             4   (3.1%)
Low   confidence:    2   (1.6%)


Regressions   from   last   run:   0
New   fixes:   3
```


If the new rule causes regressions, we refine it. The HTML report shows exactly which repos changed and why.


The result:` skyhook init` now auto-detects language, framework, port, Dockerfile config, environment variables, and monorepo structure - everything users used to fill in manually - in under 50ms with no AI calls.


## The ML Parallel (and Where It Breaks Down)


Why call this "Reverse ML"? The name is deliberately provocative. The parallel to traditional ML isn't perfect, but the similarities are illuminating:


Concept Traditional ML Reverse ML


**Training set** Labeled examples Testbed of real repos


**Labeling** Humans label data to train a model AI labels data to validate rules


**Training** Gradient descent on loss function AI exploration + human rule synthesis


**Model** Opaque weights Explicit, readable rules


**Feature engineering** Transform raw data → features Evidence collection → structured signals


**Overfitting** Model memorizes training data Rules too specific to testbed repos


**Inference** Forward pass through network Rule evaluation


The economic logic is identical: accept expensive training (development-time AI exploration) because inference (runtime rule evaluation) is cheap and fast. Evidence collection is literally feature engineering - transforming raw codebases into structured signals that make classification easier. And just like in ML, overfitting is a real concern - we mitigate it the same way: diverse test data, watching for suspiciously specific rules.


Where the analogy breaks down: there's no loss function guiding us toward better rules - human judgment is the optimizer. And rules are discrete - they match or they don't, with no graceful degradation on edge cases. An ML model might get a weird input 60% right; our rules either handle it or punt to the confidence system.


## Architecture: Evidence Collection + Classification


The detection system separates evidence collection from classification: first gather all signals (manifests, Dockerfile, source patterns), then apply heuristics with confidence scoring.


This separation enables debuggability (see all signals, not just the winner), testability (evidence is deterministic, classification can be tuned), and transparency:


```text
$ skyhook detect


Detection Results
─────────────────────────────────────────────────
Language:    Go (85%)           ← from go.mod
Framework:   Echo (80%)         ← from github.com/labstack/echo import
Port:        8080               ← from Dockerfile EXPOSE


Alternatives Considered
Language:   Node.js (45%)     ← package.json exists but tooling-only


Decision Reasoning
1. Found go.mod → Go candidate (90%)
2. Found package.json → Node.js candidate (85%)
3. package.json has no deps, only devDeps → reduced to 45%
4. Dockerfile FROM golang:1.21 → confirms Go
5. Selected Go with 85% confidence


```


When confidence drops below 70% or conflicts exist, the system flags it for human review rather than guessing.


## Hybrid Mode: Best of Both Worlds


Pure deterministic detection handles the common cases well. But some repos are genuinely ambiguous - conflicting signals, unusual structures, or patterns we haven't seen before. For these, we use a hybrid mode.


The idea: run deterministic detection first (50ms), then optionally hand off to AI when confidence is low or the user opts in.


```text
┌─────────────────────────────────────────────────────────────────┐
│                     HYBRID DETECTION                            │
│                                                                 │
│   Codebase ──▶ Evidence Collection ──▶ Deterministic Rules     │
│                      (50ms)                 (10ms)              │
│                                                │                │
│                                                ▼                │
│                                    ┌──────────────────────┐     │
│                                    │ Confidence >= 80%?   │     │
│                                    └──────────────────────┘     │
│                                      │ Yes            │ No      │
│                                      ▼                ▼         │
│                              Return result      AI Refinement   │
│                                                  (30-60 sec)    │
│                                                      │          │
│                                                      ▼          │
│                                              Return refined     │
│                                                 result          │
└─────────────────────────────────────────────────────────────────┘


```


The key insight: **AI doesn't start from scratch** . It receives:


- The structured evidence we already collected
- The preliminary classification and confidence score
- The specific conflicts or ambiguities detected
- Access to the repo for targeted exploration


This fast-tracks the AI's analysis. Instead of spending 3 minutes exploring the entire codebase, it spends 30-60 seconds investigating the specific ambiguities. The evidence collection acts like a pre-computed feature vector that focuses the AI's attention.


For example, if deterministic detection says "Go (55%) vs Node.js (45%), conflict: both go.mod and package.json exist, Dockerfile ambiguous," the AI knows exactly what to investigate. It doesn't need to discover that conflict - it can immediately dig into which manifest represents the actual application.


This hybrid approach gives users a choice:


- **Fast mode** (default): Pure deterministic, 50ms, handles 90%+ of cases correctly
- **Thorough mode** (opt-in): Deterministic + AI refinement for ambiguous cases, 30-60 seconds, higher accuracy on edge cases


Neither mode is strictly better. Fast mode is right for CI pipelines and quick checks. Thorough mode is right for initial onboarding when you want high confidence. The system adapts to context rather than forcing a single tradeoff.


## Tradeoffs and Limitations


This approach isn't free:


**Upfront investment** : Building the testbed and iteration tooling takes time. You're trading runtime cost for development cost. AI dramatically reduces this cost - what might take months of manual investigation becomes weeks of AI-assisted exploration - but it's still nonzero.


**Maintenance burden** : Rules are code. They need to be updated as the world changes. When Deno 2.0 changes its manifest format, someone has to update the rules. The flip side: updates are surgical. You change one rule, not retrain an entire model.


**Coverage gaps** : Rules only cover patterns you've seen. A novel framework won't be detected until you add it to the testbed and write rules for it. The hybrid mode mitigates this - AI can handle novel patterns at runtime when confidence is low - but it's still a gap for pure deterministic mode.


**Diminishing returns** : Going from 80% to 95% accuracy is 10x harder than 50% to 80%. Some edge cases aren't worth the rule complexity. Again, hybrid mode helps: let rules handle the 95% and let AI handle the long tail.


For our use case - onboarding codebases to a deployment platform - these tradeoffs work. Millisecond detection with full transparency handles the common cases. AI refinement handles the edge cases when users opt in.


Your mileage may vary. If your patterns change daily or your input space is truly unbounded, runtime AI might be the right choice despite its costs. But if you're solving a classification problem with knowable patterns and you care about speed, cost, or explainability, this approach is worth considering.


## Conclusion


We call this Reverse ML because it inverts the traditional pattern: expensive AI exploration at development time produces cheap deterministic execution at runtime - with the option to bring AI back when the stakes justify it.


The approach works for us because of a combination of factors:


- We can leverage best-in-class tooling (Claude Code) at development time, saving our agent investment for where we add unique value
- We want instant CLI response times, not multi-minute waits
- We value transparency and auditability over probabilistic outputs
- Upfront development effort pays off as free runtime execution


None of these factors alone is decisive. If latency doesn't matter, if you're already building production agent infrastructure, if determinism isn't important - runtime AI might be simpler. We're not claiming this approach is universally better.


But for bounded classification problems where you can enumerate the patterns, where examples exist, and where some combination of speed, cost, capability constraints, or transparency matters - this is a viable third option between "just use AI at runtime" and "write rules by hand."


The novel part isn't the deterministic rules. Engineers have built rule-based systems for decades. The novel part is using AI to make comprehensive rule development economically practical - and giving that same AI a head start at runtime when the rules aren't enough.


The next time you reach for an LLM at runtime, ask: could I use AI to generate rules instead? And if some cases still need AI, can I at least give it a head start?
