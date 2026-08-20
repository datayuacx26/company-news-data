---
schema_version: "1.0.0"
document_id: "8087fc086d0f3ebc6a07f4e2817809d0effb0fe36c5d07987043f7efc93bf637"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/dark-code/"
published_at: "2026-04-15T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:ca74e00aad99ac0ae7f9ddc1b35decd4985e956d2585d53b62377c270a31dd19"
---

# Dark Code: The AI-Generated Software Nobody Understands

The biggest risk to your product isn’t AI-generated code that doesn’t work. It’s generated code that seems fine.


AI doesn’t optimize for correctness. It creates something passable. Something that passes the smell test. And when everybody in the industry is pushed to move faster and do more with less, you end up shipping software that *looks* correct. It passed your quick visual check. It passed all the tests. But no one ever **fully** understood it. Not the engineer who prompted it, not the one who approved it (because they’ve got 10 other PRs to review).


This is dark code.


## Dark Code


[Jouke Waleson](https://blog.waleson.com/2026/03/mapping-out-dark-code.html) first spoke about dark code at the OpenAI Codex Meetup in Amsterdam in March 2026.


> “Lines of software that no human has written, read, or even reviewed.”


The “dark” metaphor comes from manufacturing. Japan’s FANUC has run lights-out factories since the 1980s where robots work in complete darkness because no humans are required. Dan Shapiro’s “[Dark Software Factory](https://www.danshapiro.com/blog/2026/01/the-five-levels-from-spicy-autocomplete-to-the-software-factory/) ” applied the same idea to code. Specs go in, software comes out, no human writes or reviews anything in between.


If you have a dark factory, you get dark code.


## Levels to the Dark Factory


Shapiro’s framework is modeled on NHTSA’s autonomous driving levels.


Level Name Description


0 Spicy Autocomplete Not a character hits disk without your approval


1 Coding Intern Offload discrete tasks: “write a unit test,” “add a docstring”


2 Junior Developer Autopilot on the highway. Where most “AI-native” devs live now


3 Code Review You’re a manager now. “Your life is diffs.”


4 Spec-Driven Write a spec, leave for 12 hours, check if tests pass


5 Dark Factory Specs in, software out. No human writes or reviews code. Ever.


Most developers using Claude Code or similar are somewhere between level 2 and 3. They’re generating significant chunks of code, reviewing diffs, and shipping faster than they ever have before. Many teams are actively trying to get to level 5. Some teams already have (or so they claim). The interesting question is what breaks along the way.


## The Math Problem


Let’s run some back-of-the-envelope math. AI coding agents generate code far faster than humans can fully understand it — rough estimates put the ratio somewhere between 5-7x.


- AI: ~140-200 meaningful lines generated per minute
- Humans: ~20-40 lines per minute (for genuine comprehension)


Margaret-Anne Storey calls this **[comprehension debt](https://addyosmani.com/blog/comprehension-debt/)** . It’s the growing gap between how much code exists and how much of it anyone actually understands, or at least has tried to understand.


Unlike technical debt, which announces itself through failing tests and degraded performance, comprehension debt breeds false confidence. The code works. The tests pass. Nobody notices the debt until something breaks in a way that requires understanding the code to fix it.


[An Anthropic study](https://www.anthropic.com/research/AI-assistance-coding-skills) found that developers using AI for code generation delegation scored 17% lower on comprehension quizzes (50% vs 67%) compared to those using AI primarily for conceptual inquiry. The tool that makes you faster is also making you less capable of understanding what you shipped. This is actually a pretty obvious tradeoff. Once again we see that there is often no “best” in engineering, just tradeoffs.


## The Security Problem


The[numbers are not good](https://cycode.com/blog/ai-security-vulnerabilities/) .


- **40-62%** of AI-generated code contains vulnerabilities ([Cloud Security Alliance](https://cloudsecurityalliance.org/blog/2025/07/09/understanding-security-risks-in-ai-generated-code) : 62%,[Veracode](https://www.veracode.com/resources/analyst-reports/2025-genai-code-security-report/) : 45%)
- AI code generators produce vulnerabilities at roughly **2x the rate** of human-written code
- [Vibe-coded applications](https://checkmarx.com/blog/security-in-vibe-coding/) routinely ship with **multiple critical vulnerabilities**


CVEs from AI-generated code are accelerating, and most AI-generated vulnerabilities never even get attributed to AI because no one knows which code was AI-generated.


## The Governance Gap


Stanford Law’s “[Built by Agents, Tested by Agents, Trusted by Whom?](https://law.stanford.edu/2026/02/08/built-by-agents-tested-by-agents-trusted-by-whom/) ” asks the uncomfortable questions.


When agents write code without human review, who’s on the hook?


- Are engineers liable?
- The AI provider?
- The vendor who integrated the AI?


Traditional product liability assumes human involvement at critical checkpoints. It’s never been acceptable to respond to concern about work you’ve created and say, “IDK, Claude wrote it”, but that all changes when we are actively trying to remove humans from the loop.


The alignment problem is more subtle than crude test manipulation. Agents optimize for test-passing, not something that works under real-world conditions. The agent that makes every test green isn’t necessarily the agent that ships working software. At least not software that works long term, with heavy load, and handles edge cases gracefully.


Waleson put it bluntly. “Without human review and new architectural tools, codebases will deteriorate. AI agents, like evolution, take the easiest path forward. Systems will become an unholy mess and get stuck in local maximums.”


## StrongDM’s Dark Factory Experiment


Not everyone is waiting to see how this plays out. StrongDM’s three-person AI engineering team has been[operating a genuine dark factory](https://www.strongdm.com/blog/the-strongdm-software-factory-building-software-with-ai) since July 2025, building infrastructure access management and security software. You may have seen one of their[early posts](https://factory.strongdm.ai/) blog up on Hacker News where they claimed “If you haven’t spent at least $1,000 on tokens today per human engineer, your software factory has room for improvement”.


Their charter rules:


1. “Code must not be written by humans”
2. “Code must not be reviewed by humans”


If you’re building security software without human code review, you need something else to provide confidence. They built **production replicas** .


The StrongDM team built working replicas of Okta, Jira, Slack, Google Docs, Drive, and Sheets. These replicas mimic interfaces, edge cases, and behaviors. They run thousands of test scenarios per hour against these replicas. The code ships if and only if it behaves correctly against realistic simulations of real systems.


They don’t slow down to review code. They verify behavior against reality.


## The False Choice


Slow down and review everything manually, or ship fast and hope? Neither.


StrongDM shows a third way. **Ship fast with verification that actually works** . You can’t review every line of dark code any more than you can manually inspect every widget coming off a lights-out assembly line. But you can test the output against realistic conditions before it reaches users.


Traffic replay is the mechanism. Instead of reviewing code, you verify behavior.


```text
flowchart TD
AI[AI Agent Generates Code] --> Static[Static Analysis Clean]
Static --> Tests[Unit Tests Pass]
Tests --> Replay[Traffic Replay]
Replay -->|Behavior Matches Production| Ship[Ship with Confidence]
Replay -->|Behavioral Diff Detected| Block[Block and Investigate]


```


Captured production traffic becomes your source of truth. The AI can modify your test files, your server code, your generated files. But it can’t modify the recording of what production actually did. When you replay real traffic against new code, you’re testing against actual user behavior, the one thing AI can never game.


## What This Looks Like in Practice


1. **Capture real traffic** from your production or staging environment
2. **Generate code** with whatever AI tools your team uses
3. **Replay captured traffic** against the new code before merge
4. **Diff the behavior** (status codes, response shapes, latency, error rates)
5. **Gate deployment** on behavioral consistency, not just test passage


Tools like[proxymock](https://speedscale.com/proxymock/) make this practical for local development.[Speedscale](https://speedscale.com/features/ai-code-verification/) scales it for CI pipelines across distributed systems.


You’re not reviewing code anymore. You’re validating that the system behaves the same way it did before. The code can be dark. The behavior can’t be.


## Where This Goes


Dark code is already here. Legacy enterprise systems written by departed teams have always been “pretty dark,” as Waleson notes. AI just accelerates the phenomenon and removes the fiction that someone, somewhere, understood what shipped.


More code review won’t save you. Code generation has outpaced human comprehension. The answer is verification systems that don’t require understanding every line.


Traffic replay against production replicas. Behavioral validation in CI. Automated regression detection against production patterns.


The lights are going out whether you like it or not. Might as well know what’s coming off the line.


---


**Ready to validate AI-generated code against production behavior?**[Try proxymock free](https://speedscale.com/proxymock/) to capture and replay traffic locally, or see how[Speedscale’s AI code verification](https://speedscale.com/features/ai-code-verification/) catches behavioral regressions before they reach production.


**Related reading:**


- [AI Coding Agents Break What Works](https://speedscale.com/blog/ai-coding-agents-break-what-works/)
- [Silent Failures: Why AI Code Breaks in Production](https://speedscale.com/blog/silent-failures-why-ai-code-compiles-but-breaks-in-production/)
- [What AI Has Never Seen: The Context Gap in Code Generation](https://speedscale.com/blog/what-ai-has-never-seen-the-context-gap-in-code-generation/)
- [Enterprise Vibe Coding](https://speedscale.com/blog/enterprise-vibe-coding/)
