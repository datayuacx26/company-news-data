---
schema_version: "1.0.0"
document_id: "cefc2441eb426d0bf526c40829979f61ddd63696a916ccef7798f96f1da9b1e9"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/beyond-tokens-best-ideas/"
published_at: "2026-07-07T16:28:33+00:00"
first_seen_at: "2026-07-20T23:22:16.766689+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:b155a3649e638cc3b88a739b71e611ac69a56dfded1044e17b3bcb467541f258"
---

# Beyond Tokens SF: Best Ideas of the Evening

AI agents are changing how software gets built, but the infrastructure around them hasn’t caught up. Agents burn through tokens on noise. They take actions they shouldn’t. Context evaporates between releases. And most delivery pipelines were never designed for the pace and volume of agentic development.


On June 11th we brought together developers in San Francisco for **Beyond Tokens** , a builders night focused on what’s next: faster agents, cleaner context, fewer tokens, secure infrastructure for agents to run on, and AI-native delivery that actually keeps up with how you build. Three teams took the stage, the creators of[NanoClaw](https://nanoclaw.dev/) ,[Boost](https://boost.jfrog.com/) , and[Fly](https://jfrog.com/fly/) , each tackling a different layer of the same problem: *how do you make agents that are fast, trustworthy, and actually fit into the way modern software gets shipped?*


Here are the recaps of each session.


## JFrog Fly: Managing Release Context at Agentic Scale


**Presenter: Asaf Ezra, R&D Team Lead, JFrog Fly**


When you’re shipping at agentic speed, context evaporates. Intent built up through Slack threads, design discussions, and agent conversations disappears the moment a release ships, and the next agent has to reconstruct everything from scratch.


**JFrog Fly** is a smart registry that stores not just artifacts (e.g., Docker images, npm and Python packages), but the full context of a release: commits, PRs, issues, and the reasoning behind decisions. Its[agent context tracing](https://docs.fly.jfrog.com/fly-platform/decision-records/#agent-context-for-better-development) feature automatically captures architectural, product, and security decision records and attaches them to every release.


**Standout moment from the session:** when Asaf asked the agent to change the default theme from light to dark, Fly flagged a conflict. A prior decision record had justified light mode based on user research, so the agent stopped before writing a single line of code. That’s the power of persisted context.


**The takeaway:** Agentic development creates a flood of releases. Treating context as a first-class artifact, not an afterthought, keeps your agents aligned.


[Try JFrog Fly](https://jfrog.com/fly/)


## JFrog Boost: Reducing Token Costs Without Changing How You Code


**Presenters: Yahav Cohen & Shay Dahan, Co-founders, JFrog Boost**


Token costs are climbing from two directions: frontier models keep getting more expensive, and smarter models consume more input tokens. Yahav and Shay spent the past year asking: where exactly is the waste?


They found four main culprits:


- **Log pollution:** agents read hundreds of lines of test output when they only need ten.
- **Inefficient search:**` grep` and` find` return walls of text, while IDE-style LSP tooling returns only the relevant result.
- **Missing pre-push discipline:** skipping linting and unit tests before pushing creates expensive CI failure loops.
- **Existing optimization tools that break things:** most hide information the agent actually needs, causing silent failures.


Their solution, **JFrog Boost** , filters noise before it enters the context window. It also does something counterintuitive: it lets the agent disable it. Every time that happens, the event is logged and an internal agent factory opens a PR to fix the gap. The product improves automatically from its own failure cases.


**Standout moment from the session:** Using Boost results in an average of **35% more effective monthly token usage** for every developer at JFrog, with no increase in the bill. The demo showed a Python bug-fix task running 36% cheaper with Boost than without. Same solution, same outcome, cleaner context window.


Boost is free and installs with a single terminal command at[boost.jfrog.com](http://boost.jfrog.com/) .


**The takeaway:** Token spend is a signal that your agent is doing unnecessary work. Eliminating context pollution makes agents faster and cheaper without changing what they accomplish.


[Try JFrog Boost](https://github.com/jfrog/boost)


## NanoClaw: Building Agents You Can Actually Trust


**Presenter: Gavriel Cohen, Co-founder, NanoClaw**


Gavriel’s premise: he has an AI assistant with access to his email, calendar, and call notes, and he lets anyone talk to it. His session built an argument for why that’s not reckless, using three security concepts in NanoClaw:


1. **Isolation:** Any agent handling unsanitized input is operating in enemy territory. NanoClaw separates the agent from the orchestration layer at the OS level so it can’t modify code, access outside files, or influence message routing.
2. **No credentials in the agent environment.** The only guarantee against credential leakage is the agent never having them. Requests proxy through a vault that injects credentials only when authorized, so the sensitive action happens entirely outside the agent runtime.
3. **Human-in-the-loop approvals.** Policies define what an agent can and can’t do. For the most sensitive actions, a human approval gate sits between the tool call and tool execution, and critically, the agent is never aware it happened.


**Standout moment from the session:** The JFrog integration demo brought these concepts to life. When an agent tries to install an npm package, the request routes through JFrog’s scanned registries. Vulnerable versions get blocked automatically and safe alternatives are suggested, enforced outside the agent’s environment so it can’t route around the block.


At the time of the event, NanoClaw was four months old, open source, and had ~30K GitHub stars.


**The takeaway:** Instructions don’t enforce security, structure does. And “human in the loop” doesn’t mean slowing things down; it means the agent surfaces decisions and humans make them.


[Try NanoClaw](https://github.com/nanocoai/nanoclaw)


## The Through-Line


Three different problems, one shared mission: giving agents the tools, guardrails, and signal they need to operate autonomously without breaking things.


Fly gives agents the release history and decision records to act on the right information. NanoClaw gives them the security boundaries to act without risk. Boost gives them the efficiency to act without waste.


The next wave of agentic development isn’t about making models smarter. It’s about building the foundation that lets them do more on their own.


If you’re interested in trying any of these tools, here’s where to start:


[JFrog Boost →](https://boost.jfrog.com/) ·[JFrog Fly →](https://jfrog.com/fly/) ·[NanoClaw →](https://nanoclaw.dev/)
