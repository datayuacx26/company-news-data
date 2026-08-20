---
schema_version: "1.0.0"
document_id: "6bc0ca307ff1febfd18d2b3a3ecd2c11a647846ad0d52924b17bcb18c7b66ab4"
company_key: "riskified-ltd-class-a-ordinary-shares"
company: "Riskified Ltd."
source_id: "riskified-ltd-class-a-ordinary-shares-rss-dd7d0cc56e2d"
canonical_url: "https://medium.com/riskified-technology/lgtm-2-0-zero-noise-ai-code-review-agents-857441ec4f1a"
published_at: "2026-02-11T18:48:11+00:00"
first_seen_at: "2026-07-20T23:18:31.853064+00:00"
fetched_at: "2026-08-20T02:06:38.087890+00:00"
content_hash: "sha256:52d37cc14d70240469b8ab4c5231ba42423406d3cd6174fd7eb2c291df4b77b1"
---

# LGTM 2.0: Zero-Noise AI Code Review Agents

### **LGTM 2.0: Architecting Zero-Noise AI Code Review Agents**


#### *How I built an architectural enforcer with dynamic skills, strict governance, and active noise cancellation.*


### The Review Gap Nobody Talks About


There’s this frustrating middle ground in code review that every team hits eventually.


On one end, you have linters. Fast, cheap, deterministic. They catch the easy stuff instantly. On the other end, you have your experienced engineers, the ones who can spot architectural issues, subtle security issues, or that race condition hiding in plain sight. But they’re expensive, stretched thin, and by their tenth PR of the day, they’re just trying to get through the queue.


The gap between these two? That’s where the real bugs live.


**Here’s what that gap looks like in practice:** a developer opened a PR to add two new fields to our JWT tokens. Solid work, updated the types, wired it through the service layer, and touched up every test file. Eight files changed, everything consistent, tests passing. Looked ready to ship. It wasn’t. We’ll come back to this one.


Code review was supposed to be our quality gate, but when you’re looking at a 50-file PR, it’s hard to catch that one API misuse buried on line 847. It’s not that the reviewers aren’t good; it’s that humans aren’t built to maintain that level of attention across hundreds of files a week.


That gap is exactly where an AI agent fits.


I’ll walk through how I built an AI code review agent by orchestrating Claude instead of rolling my own agent loop.


We’ll cover:


1. **Orchestration:** Leveraging Claude Code and custom tools instead of building an agent loop from scratch.
2. **Precision:** Dynamic skills and “active noise cancellation.”
3. **Governance:** Cost control with LiteLLM.


### 1. Don’t Build an Agent Loop, Orchestrate One


The temptation when building AI tools is to write your own custom agent runtime. I thought about it too, but what’s the value here? Can I really beat the existing tools without going out of my way? Would you build a backend framework from scratch instead of using an existing one? Probably not.


I went a different route. **Claude Code** is already capable as a general-purpose agent, and Anthropic already supports GitHub Action integration for it, so why not just use that?


I gave it some tools and a prompt and got to work.


Before the agent even opens a file, it pulls the PR diff and metadata via the GitHub CLI (gh pr diff, gh pr review --json ).It checks existing comments and reviews to avoid repeating resolved feedback.


**The cleanup loop.** This one took a while to get right. The agent didn’t auto-resolve comments, so old, irrelevant comments started piling up and annoying everyone.


My solution: before each review, the agent scrapes its own previous inline comments via the GitHub CLI (gh api), checks the current diff to see if the issue has been addressed, and if so, resolves the comment.


Sounds simple, but it changed how the team perceived the tool. It went from “that annoying bot” to something that actually maintains itself.


**Inline comments with actionable suggestions.** Rather than dumping a wall of text at the bottom of the PR, the agent posts comments directly at the relevant line using GitHub’s suggestion blocks.


Reviewers can apply fixes with one click. Each comment is 1–2 sentences max, no headers, just the problem and the fix.


I started using claude-code-action before v1, back when things were still rough around the edges. One bug I ran into: suggestions would duplicate code when applied. The model didn’t understand that GitHub suggestions *replace* the entire line range, not just modify parts of it.


**Claude Code** isn’t open source, but Claude Code Action is, and it’s well-maintained.


I contributed a fix upstream that added explicit guidance about this behavior.


### 2. Dynamic Skills & Active Noise Cancellation


Want to kill adoption instantly? Have your AI leave 20 comments about variable naming. Engineers will ignore it within a day.


I learned this the hard way. The first version was way too noisy. So I rebuilt the whole approach around two ideas: **dynamic context** and **negative constraints** .


**Shared rules, multiple contexts.** Consistency is critical, whether you’re running checks in a CI pipeline or locally on your laptop before pushing. To ensure this, I extracted all the review logic into a single sharedcode-reviewskill. Both the GitHub Action and the local CLI agent load this as their first step. The result? Same rules, same filter criteria, same output format — everywhere.


#### Skills injection (Just-in-Time)


Loading every engineering rule into one massive prompt doesn’t make any sense. The model’s attention gets diluted, and you just spend more tokens.


Instead, I implemented dynamic skill loading based on what files actually changed:


This hierarchy means team-specific decisions (like “always use X library for payments”) override the global defaults automatically.


#### What NOT to do


This part might sound obvious, but explicitly telling the AI what it’s **forbidden** to do made a huge difference.


```text
## FORBIDDEN  - Praise or compliments ("Great job on this function!")  - Questions ("Have you thought about…")  - Emojis in inline comments
```


And then the filter gate. The agent only reports an issue if **at least two of these criteria are met:**


1. Can quote the exact rule being violated.
2. Verified it’s from this PR (not pre-existing code).
3. Will cause incorrect runtime behavior.
4. Objectively wrong (not a style preference).


One more thing: if the diff only contains lockfiles, Markdown files, version bumps, or generated files, the agent skips the review entirely. No point wasting tokens on noise.


#### API detection


One pattern we hardcoded: whenever the agent detects new endpoints, it flags them with a standard authentication/authorization checklist. HMAC, JWT, Istio policies, cross-account access prevention — the basics that are easy to forget when you’re focused on shipping.


The result? **Silence by default.** When it does speak up, people actually pay attention.


### 3. Cost, Scale, and Monitoring


AI agents can be expensive. An unmonitored loop could easily drain your budget overnight.


#### Exit early, exit cheap


A common mistake is putting guard conditions in the prompt.


“Don’t review if this is a draft PR” technically works, but the agent still spins up, loads context, burns tokens, and then exits. You’re paying for nothing.


Instead, check those conditions in your CI pipeline *before* the agent starts:


```text
if: ${{ !github.event.pull_request.draft && !github.event.pull_request.merged }}
```


Now the workflow short-circuits before any tokens are spent.


#### LiteLLM as a gateway


We use LiteLLM centrally to manage every LLM API in the same place. By routing everything through this proxy instead of connecting directly, I get:


- Actual visibility into spend per repo and per team.
- Hard budget caps that cut the connection if something goes wrong.


#### On multi-agent architectures


I did experiment with spawning four parallel agents:


1. **CLAUDE.md Auditor** — checks for violations of repo-level rules.
2. **Skill Auditor** — checks for loaded skill violations.
3. **Bug Detector** — finds bugs that cause incorrect runtime behavior.
4. **Security Analyzer** — finds vulnerabilities with concrete exploit paths.


They run simultaneously, then their findings get merged and deduplicated before applying the same 2-of-4 filter criteria.


Here’s the honest truth: it consumed **up to 7x more tokens** in my tests. Slower, way more expensive. Great for deep architectural migrations, but overkill for everyday PRs.


So I made it optional.


### The Payoff


By orchestrating a proven agent runtime instead of building from scratch, I avoided a lot of complexity while getting something actually reliable.


Remember that JWT PR from the intro? The review came back: BLOCKING. Turns out there’s a Zod schema that validates every JWT payload at runtime, and the developer had no idea it existed.


Without updating it, every single login would have failed. You can see it in the commit history, their follow-up commit is just called “feat: add to schema.” The one file they never knew they had to touch.


No one would have caught this eyeballing the diff. The tests passed, the types were correct, everything compiled fine. That’s the gap this was built to close.


The result isn’t just “automated code review.” It’s a quality gate that enforces your engineering culture, without the noise, and without blowing the budget.


If you’re thinking about building something similar: don’t start from zero. Find a good agent runtime, constrain it heavily, and make it silent by default. The signal-to-noise ratio is everything.


---


[LGTM 2.0: Zero-Noise AI Code Review Agents](https://medium.com/riskified-technology/lgtm-2-0-zero-noise-ai-code-review-agents-857441ec4f1a) was originally published in[Riskified Tech](https://medium.com/riskified-technology) on Medium, where people are continuing the conversation by highlighting and responding to this story.
