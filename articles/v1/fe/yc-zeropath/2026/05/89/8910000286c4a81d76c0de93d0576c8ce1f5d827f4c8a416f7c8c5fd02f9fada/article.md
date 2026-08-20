---
schema_version: "1.0.0"
document_id: "8910000286c4a81d76c0de93d0576c8ce1f5d827f4c8a416f7c8c5fd02f9fada"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-d50bd684d529"
canonical_url: "https://zeropath.com/blog/ai-coding-assistants-are-not-sast"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-24T06:50:52.708520+00:00"
fetched_at: "2026-07-28T22:13:03.870884+00:00"
content_hash: "sha256:7b5185f1e96f9a492a9c3294da252364df52cdd6d08f5c281c0983ba63054eec"
---

# AI Coding Assistants Are Not a SAST Program

A question that comes up constantly in security conversations right now: *can we just use Claude Code (or Codex, or Cursor's review mode) as our SAST?*


It's a fair question. The latest generation of agentic coding tools is genuinely good at reading code. They can spot SQL injection in a Flask route, flag a missing auth check, point at a sketchy` eval` , explain why a regex is exploitable. For a developer who has never had any security review on their code, that's a real upgrade. And for engineering leaders staring at a stack of security tooling invoices, the appeal of consolidating onto a tool the team already pays for is obvious.


So it's worth taking the question seriously rather than dismissing it. Where does an agentic coding assistant actually function as security tooling, and where does the model break down? This post is an attempt at an honest answer.


## What agentic coding assistants do well


Start with the genuine strengths, because they matter.


**Inner-loop review.** When a developer is actively writing code and asks the assistant to look it over, the model has the right context: the file is in the window, the developer can answer follow-up questions, and any finding can be fixed in the next keystroke. This is the highest-leverage moment for security feedback, and it's a moment traditional SAST tools have historically been bad at - they show up at PR time or in a nightly scan, not while the bug is being written.


**Exploratory analysis.** Pointing an assistant at an unfamiliar repo and asking "what would you be worried about here, security-wise" is a legitimately useful workflow. The output isn't a complete audit, but it's a faster way to build a threat model than reading the code cold.


**Lowering the floor.** A solo developer or a small team without a dedicated security function can now get meaningful review on their code at marginal cost. That's a real expansion of who has access to security feedback at all, and it shouldn't be waved off.


**Explaining findings.** When an assistant flags an issue, it can explain *why* , suggest a fix, and answer follow-up questions in plain English. Traditional SAST tools have struggled with developer experience for decades; agentic assistants are good at it by default.


None of this is small. If the question is "should developers use Claude Code while they're writing code," the answer is yes.


The question this post is about is different: *can a tool like Claude Code stand in for a SAST program at the scale of an enterprise codebase?* That's where the model shifts.


## The products in this space


Two of the most visible entrants here are security review capabilities bundled into broader AI coding products:


### Claude Code Security Review (Anthropic)


A security review feature inside Claude Code. Two surfaces:


- **` /security-review`** - an on-demand slash command an engineer runs in their terminal. It scans the working codebase and surfaces vulnerabilities, with the option to ask Claude to implement fixes inline.
- **GitHub Action** - automatically reviews every pull request opened on a repo, posts inline comments with identified issues and recommended fixes, and supports customizable filtering rules.


Detection focus: SQL injection, cross-site scripting (XSS), authentication and authorization flaws, insecure data handling, and dependency vulnerabilities.


Distribution: available to Claude Code users on paid plans (Pro, Max) and pay-as-you-go API Console accounts.


### Codex Security (OpenAI, formerly Aardvark)


OpenAI's application security agent, built on the Codex agent and frontier OpenAI models. It:


- Builds an **editable threat model** for the repository - what the system does, what it trusts, and where it is most exposed.
- Searches for vulnerabilities using that threat model as context and **categorizes findings by expected real-world impact** .
- **Validates** findings in sandboxed environments where possible, producing working proof-of-concepts and reducing false positives.
- Proposes **patches** that align with system intent and surrounding behavior.
- **Learns from feedback** - when a user adjusts the criticality of a finding, that signal refines the threat model on subsequent runs.


Distribution: rolling out in **research preview** to ChatGPT Pro, Enterprise, Business, and Edu customers via Codex web.


Both products are built on strong frontier-model reasoning, embed naturally into the AI coding workflow developers already use, and provide PR feedback at the moment of change. Codex Security's editable per-project threat model and sandboxed validation with working proof-of-concepts is a genuinely useful design - it reduces noise and gives reviewers harder evidence.


For a developer who wants security signal on the code they're actively writing, these are good tools.


## What changes at enterprise scale


Four things that dedicated SAST platforms do, that general-purpose coding agents are not currently built for.


### 1. Whole-codebase coverage


Agentic assistants operate inside a context window. Even with aggressive context management, retrieval, and sub-agents, there's a hard ceiling on how much code the model can reason about at once, and the assistant chooses what to look at based on heuristics and the user's prompt.


For a 50-file project, this is fine - the model can effectively cover the whole thing. For a monorepo with millions of lines across hundreds of services, it isn't. The model will look at *some* of the code. It won't tell you, with certainty, that it looked at the auth middleware in service X, the deserialization path in service Y, and the file upload handler in service Z. There's no inventory of what was analyzed and what wasn't.


Dedicated SAST tools take the opposite approach: traverse the entire repository deterministically, build a graph of sources and sinks, and produce an auditable record of what was analyzed. This is less impressive in a demo and more important in production. When someone asks "did we scan the payments service," you need an answer.


ZeroPath scans the codebase in a principled way: traversal is structured so that coverage of the repository is guaranteed, rather than depending on what an agent decided to look at in a given session or run. The same code, scanned again, produces comparable results. This is non-negotiable for enterprise AppSec, where "did we look at all of it?" and "is this the same finding we saw last week?" are questions auditors and security leaders ask routinely.


### 2. Detection quality at scale


This is the part that's hardest to evaluate without numbers, so it's worth looking at some.


ZeroPath recently benchmarked Claude Opus 4.6 - currently Anthropic's strongest model - against a corpus of 435 real, disclosed CVEs, with prompting tuned specifically for vulnerability detection. With optimal prompting, the model caught roughly 28% of known vulnerabilities. False positive rates ran above 40%. Findings shifted meaningfully between runs on the same code, meaning two scans of the same repo could produce materially different reports. The full methodology is at[zeropath.com/blog/benchmarking-opus-4-6-vuln-detection](https://zeropath.com/blog/benchmarking-opus-4-6-vuln-detection) .


A few things worth being careful about with these numbers:


- **What they measure.** Detection rate against known CVEs in code the model wasn't specifically pointed at. This is a reasonable proxy for "would the model find this if it weren't told to look," which is the relevant question for autonomous scanning.
- **What they don't measure.** Cases where a developer is actively working in a file and asks for review. In that mode, the model performs much better, because the context is narrower and the user is steering. This is exactly the inner-loop case where these tools genuinely shine.
- **The variance.** The run-to-run instability is the part that matters most for program-level use. A SAST tool you can't reproduce results from is hard to integrate into a release gate, an audit, or a triage queue.


The takeaway isn't "the model is bad." It's that LLMs operating autonomously over large codebases are currently a low-recall, high-noise, non-deterministic detector. Dedicated SAST platforms - including ones that use LLMs internally - combine model-based reasoning with deterministic program analysis (data flow, taint tracking, reachability) precisely to get out of that regime.


This is also why ZeroPath has a first-class concept of a **stable issue** : a finding has an identity that persists across scans, branches, and time. If you fix it, ZeroPath knows it was fixed. If it reappears, ZeroPath knows it's the same one. That's what makes it possible to track application security posture as a program - open issues, MTTR, SLA compliance, ownership, regression detection - rather than as a stream of independent agent-run reports. Findings from a` /security-review` invocation or a Codex Security scan are scoped to that run; ZeroPath findings live in the platform as durable objects.


### 3. Workflow surface area


A finding that doesn't reach a developer in a workflow they're already in is a finding that gets ignored. This is the lesson every SAST vendor learned painfully over the last decade.


What "workflow surface area" looks like in practice:


- PR-level findings posted as review comments, blocking or non-blocking depending on policy
- Tickets auto-created in Jira/Linear with severity, ownership, and SLA tracking
- IDE plugins that surface findings before code is committed
- A central administrative console for security teams to triage, suppress, and assign findings
- Audit trails of what was scanned, when, by whom, with what configuration
- Deployment gates and policy enforcement
- Integration with existing identity, SSO, and SIEM infrastructure


Coding assistants are designed around the developer's terminal session. The integration surface that SAST platforms have built up - and that security programs are organized around - isn't there, and isn't really what those tools are trying to be. This isn't a flaw; it's a different product category.


ZeroPath is wired into the systems where security work actually gets done: **ticketing integrations** with Jira, Linear, and others, so findings flow into the systems engineering teams already use; **self-service for developers** , including in-product chat, so developers can interrogate and triage their own findings without going through AppSec for every question; and **automated workflow creation** for how findings are routed, escalated, and resolved.


ZeroPath also gives security teams granular control over the scanner, including learning from triage. When a user marks a finding as a false positive, ZeroPath uses that signal to suppress similar findings going forward - so the scanner improves over time for that specific codebase and that specific team's standards. Codex Security has its own feedback loop (criticality adjustments refine the threat model). Claude Code Security Review supports customizable filtering rules. ZeroPath's model is broader: false-positive learning, custom sources and sinks for the customer's specific taint patterns, and persistent repository context that augments how the scanner reasons about each codebase - all stored against the org's account, not re-explained per run.


### 4. Coverage beyond code logic


SAST is one piece of application security. A real program also includes:


- **SCA (software composition analysis):** known vulnerabilities in third-party dependencies, license compliance, transitive risk
- **Secrets scanning:** API keys, tokens, private keys committed to repos or build artifacts
- **IaC scanning:** misconfigurations in Terraform, CloudFormation, Kubernetes manifests
- **Container scanning:** vulnerabilities in base images and runtime dependencies


A coding assistant looks at code logic. It can find a SQL injection; it isn't built to flag that you're three majors behind on a dependency with a known RCE, or that your S3 bucket policy is world-readable, or that an AWS key is sitting in a` .env` checked into git. Treating an agentic assistant as a SAST replacement leaves these vulnerability classes uncovered, and in most enterprise environments they're where the actual incidents come from.


ZeroPath ships, in one platform:


- **SAST** for application code.
- **SCA / dependency analysis** , with reachability and SBOM exports.
- **Secrets scanning** with verification status.
- **IaC scanning.**
- **CI/CD configuration scanning** - catching vulnerable pipelines, not just vulnerable application code.
- **Auto-patching, PR creation, and managed PR scanning.**
- **Runtime validation** (beta) - analogous in spirit to Codex Security's sandboxed validation, but as part of a tracked AppSec program.


An organization adopting Claude Code Security Review or Codex Security still needs separate tooling for SCA, secrets, IaC, and CI/CD configuration.


ZeroPath also ships with a robust REST API for org-wide automation, and integrates with **GitHub, GitLab, and Bitbucket** - not just GitHub. Both Claude Code's GitHub Action and Codex Security are GitHub-oriented today. For enterprises standardized on GitLab or Bitbucket - or running a mix - this is often the deciding factor.


## Capability comparison


Capability Claude Code Security Review Codex Security ZeroPath


On-demand security review in CLI Yes (` /security-review` ) Via Codex web Via CLI / MCP integrations


Automated PR review (GitHub) Yes (GitHub Action) Yes Yes


Automated PR review (GitLab / Bitbucket) No No Yes


SAST / application-code vulnerabilities Yes Yes Yes


Dependency / SCA scanning Partial (dependency vuln checks) Application focus Yes (dedicated SCA product, SBOMs, reachability)


Secrets scanning No No Yes


IaC scanning No No Yes


CI/CD configuration scanning No No Yes


Auto-fix / patch suggestions Yes Yes Yes


Sandboxed validation of findings No Yes Yes (runtime validation, beta)


Threat model as first-class concept No Yes (editable per repo) Repo context + custom sources/sinks


Learns from false-positive / feedback Configurable filtering Yes (feedback refines threat model) Yes (per-codebase suppression of similar findings)


Stable issue identity across scans No Per-scan findings Yes


Persistent triage state, SLA, MTTR tracking No Limited Yes


Compliance / framework mapping & reports No No Yes


Ticketing integrations (Jira, Linear, etc.) No No Yes


Developer self-service chat for findings Inline in Claude Code Inline in Codex Yes (in-product)


Robust REST API for automation Limited Limited Yes


General availability GA Research preview GA


Independent of LLM-provider subscription Requires Claude plan Requires ChatGPT plan Yes


## The honest framing


The clearest way to think about this: agentic coding assistants and SAST platforms are not the same product, and the question isn't which one to pick.


Coding assistants are an inner-loop tool. They're best when a developer is actively in the code, the scope is narrow, and the user is steering. In that mode, they meaningfully raise the floor on code-level security awareness across the industry.


SAST platforms - whether the established players (Semgrep, Checkmarx, Snyk Code, Veracode) or newer LLM-native entrants (ZeroPath, Aikido, others) - are program-level tools. They're built around full-codebase coverage, reproducible detection, integration with developer workflow, audit and compliance requirements, and the broader set of scan types a real AppSec program needs.


The interesting question for the next few years isn't *coding assistant vs. SAST* . It's how the two layers compose: the assistant catches issues at the moment of writing, the platform catches what slips through and provides the program-level controls. The teams that get the most value will be the ones running both, deliberately, and not the ones trying to make either tool do the other's job.


## When to use which


**Use Claude Code Security Review** when:


- Your developers are already using Claude Code as their daily driver and you want lightweight security feedback in that workflow.
- You want a GitHub-native PR review action that flags common vulnerability classes (SQLi, XSS, auth, data handling, dependency vulns) with customizable filtering.
- The unit of work is the diff or the working repo, and findings don't need to be tracked over time.


**Use Codex Security** when:


- You want an agent that builds a per-repository threat model and uses it to prioritize findings.
- You value sandboxed validation with working proof-of-concepts as part of the finding evidence.
- You're already on ChatGPT Pro / Enterprise / Business / Edu and want to evaluate it during research preview.


**Use ZeroPath** when:


- You need to know the **entire codebase** is being evaluated, with coverage and consistency guarantees, not just what an agent decided to look at in a run.
- You need findings that **persist** across scans, with stable identity, triage state, and a full program around MTTR, SLAs, and ownership.
- You need AppSec capabilities **beyond static review** - SCA, secrets, IaC, CI/CD configuration, PR scanning, auto-patching, runtime validation - in a single platform.
- You need integrations with **ticketing** (Jira, Linear), **non-GitHub VCS** (GitLab, Bitbucket), and **built-in compliance reporting** .
- You need a scanner that **learns from your team's false-positive feedback** and respects your codebase's specific conventions via persistent custom sources, sinks, and repository context.
- You want your AppSec platform to be **independent of any particular AI vendor's subscription** .
- You're standing up, or running, an application security program at organizational scale.


If you're a developer or a small team without any security review today, start with the assistant - the marginal upgrade is real. If you're running an AppSec program at any kind of scale, the question of which dedicated platform fits your stack is the one worth spending time on. They're different problems.
