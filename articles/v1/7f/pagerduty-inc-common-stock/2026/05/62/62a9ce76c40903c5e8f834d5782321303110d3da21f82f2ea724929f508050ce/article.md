---
schema_version: "1.0.0"
document_id: "62a9ce76c40903c5e8f834d5782321303110d3da21f82f2ea724929f508050ce"
company_key: "pagerduty-inc-common-stock"
company: "PagerDuty Inc."
source_id: "pagerduty-inc-common-stock-rss-b3e2d0380f72"
canonical_url: "https://www.pagerduty.com/eng/navigating-the-shift-to-agentic-ai/"
published_at: "2026-05-07T12:24:32+00:00"
first_seen_at: "2026-07-20T23:19:31.841537+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:567179aa49b931015bb04d0796d26c975d49360295489daec81289b831b5858f"
---

# Navigating the Shift to Agentic AI by Dave Bresci

As patterns for effective Agentic AI adoption become clearer, success will rely on an engineer’s ability to shift from “writing code” to “writing specifications”. From a human aspect, even developers who are “AI enthusiasts” have struggled with making this transition. Additionally, roundtable conversations indicate that while agents can condense weeks of work into days, they also serve as a “stress test” that exposes architectural inconsistencies and requires a fundamental shift in code ownership. To successfully navigate this shift, Engineering Managers must move beyond providing tools and start coaching a new discipline: Verification-Driven Development.


## Making Effective Use of Agentic AI


- **The “Plan Mode” Pivot:** Moving from raw prompting to an “Intent-First” workflow reduces rework and ensures the AI understands the architecture before a single line of code is written.
- **Greenfield Velocity:** Leveraging agents for “Figma-to-Code” prototypes enables teams to bypass boilerplate and reach 80% completion in record time.
- **Institutional Memory:** Using agents to synthesize ADRs and documentation ensures that “tribal knowledge” is codified immediately, rather than lost in Slack threads.
- **Pattern Skills:** Codifying repeatable tasks (e.g., formatting code according to a specific pattern in a repo, always running tests after writing code, etc.) into shared AI “skills” creates a force multiplier for junior and senior engineers alike.


## Adoption Strategy: Moving from Coding to Orchestrating


- **Standardize “Plan Reviews”:** EMs should consider how to address “Plan Reviews” in addition to PR reviews. Plans may not necessarily need an independent reviewer, but they should be treated with the same scrutiny as PRs. If the agent’s plan is flawed, the code will be too.
- **Serial Integration:** Emphasize breaking work down into small chunks for agents. This prevents 4,000-line “Mega PRs” that overwhelm human reviewers.
- **The “Agent-Friendly” Repo:** Invest in repo hygiene (clean READMEs, consistent SQL patterns). Agents perform best in environments with “high-signal” examples to follow.


## The “Rough Spots” (Friction Points)


- **Technical: Good Architecture Matters.** Agents replicate whatever patterns they see; if a repo has inconsistent styles, the AI compounds that technical debt at 10x speed. Furthermore, high throughput is currently exposing “flimsy foundations” in legacy services that weren’t designed for this volume of change.
- **Cultural: The Reviewer’s Burden.** There is a growing tension as humans struggle to keep pace with AI output. Reviewing code you didn’t write requires more cognitive load and a more disciplined focus on automated testing.
- **Emotional: The “Identity Crisis.”** Many senior engineers find fulfillment in the “meditative, puzzle-like quality” of writing code. Moving to a spec-writing role can feel like losing the “joy of coding” and raises fears of skill atrophy. Most folks didn’t become developers with the intent of writing specs and requirements docs as their primary function. Additionally, junior and mid-range engineers are facing increasing uncertainty about their futures and roles within a software development organization. At all levels, software developers’ relationship to their work is fundamentally changing.


## The “No-Go” Zone


- **Autonomous Production Access:** Human-in-the-loop is non-negotiable for deployments and production routing.
- **Pattern-Less Repositories:** Agents should not be used for bulk refactors in legacy repos that lack a “Gold Standard” pattern or robust test suites.


## Tips for Engineering Managers


- **Get Your Engineers to Think “Spec-First”:** Talk to your teams about how to use Plan Mode to generate implementation “plans” (ex: MD files, PRDs,[openspec](https://openspec.dev/) , etc) before they touch the code.
- **Audit Architectural Signal:** Identify one repo where inconsistent or multiple patterns exist and pick a standard. Use this as a template for a “Gold Standard” example for the AI to follow in all your repos.
- **Validate the Review Process:** Shift the focus from how much code developers write to code reviews, good documentation, and testing. The code will come.
- **Encourage Developers to Explore Unfamiliar Codebases with AI:** It seems particularly helpful for that (from roundtable conversations and last year’s DevEx survey).
- **Use AI to Generate Docs and Keep Them Up-to-Date:** Use AI to generate quality versions of the docs we’ve always valued (ADRs, user guides, operator guides, etc.)
- **Use Hackathons and Experiment Days to Get Developers Excited:** Help turn fear or reluctance into optimism and excitement. AI doesn’t have to only be applied to the backlog or to generating docs. Give your developers time to explore their creativity with AI.
- **Burnout is a Real Danger:** Burnout doesn’t have to come solely from overwork; it can result from cognitive overload. So much is changing so quickly, it’s easy to become overwhelmed. Developers may also feel a lack of a creative outlet in the shift away from coding, which can contribute to burnout. A few developers in the roundtables discussed their challenges and how they had to shift their mindset and realize they could be just as creative, if not more so, when working with agents.


**Recommended Reading:** Take a look at Steve Yegge’s excellent post on the “AI Vampire” since it captures a lot about how our developers (and we) might be feeling, specifically about feeling overwhelmed and drained by the pace of change:[https://steve-yegge.medium.com/the-ai-vampire-eda6e4f07163](https://steve-yegge.medium.com/the-ai-vampire-eda6e4f07163) (warning… it’s long)
