---
schema_version: "1.0.0"
document_id: "728961408fcee9da93df060c9a416e1148ad1f1f520116e75c53381ecc1f5bcc"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/how-we-built-the-brain-behind-our-self-healing-system-context-retrieval-at-org-scale"
published_at: "2026-07-02T06:58:25+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:6c29a401e903aec1e18b31b7906b028522e6be51fe19577a0bf8b65b582fd676"
---

# How We Built the Brain Behind Our Self-Healing System: Context Retrieval at Org Scale

Ask a naive agent to find a bug in a codebase with thousands of repositories, and it will find the first textual match and stop. It won't ask why. It won't check whether what it found is actually relevant. It won't explore related services. It'll just return something and call it done.


We, at


**Wix** , discovered this the hard way. And it's what pushed us to build something more deliberate: a research engine that doesn't just search, but reasons about what it's looking for and why.


##


**The Problem: Search Doesn't Scale to Org Complexity**


At Wix, finding anything specific inside the codebase is genuinely challenging.


Thousands of services, thousands of developers, every team with their own agents and tooling, all of them pushing code continuously. When a bug surfaces in production, the question isn't just "where is the relevant code" it's "which of the thirty services that touch this feature is actually responsible, what was the database state at the time, what do the logs say happened, and is this behavior even a bug or is it by design?"


A year ago, someone asked me to find a specific link somewhere in our system. I spent two days on GitHub. That experience stuck with me.


Not because finding things is inherently hard, but because the tooling we had wasn't built for the kind of cross-service, cross-context search that makes up a huge chunk of daily engineering work.


When we started building Wix Octocode Orchestrator, our autonomous bug-fixing system and needed an enrichment layer to gather context before attempting any fix, I had a clear picture of what not to build: a wrapper around GitHub search that just returns the first match.


##


**What We Built: OctoCode, and Then Wix Octocode Research**


I started by building


[OctoCode](https://github.com/bgauryy/octocode) initially for myself, as an MCP server on top of GitHub's tooling. The goal was simple: help agents navigate repositories intelligently, understand relationships between packages and services, and get real definitions of things instead of textual guesses.


Eleven months later, OctoCode sits at around


**90,000 downloads** ,


**5,000 weekly active users** , and


**4,500 weekly downloads** . That adoption wasn't the plan it came from solving a real problem that turns out to be universal in any large engineering organization.


What made it different from raw GitHub search was a set of deliberate design decisions around how agents use tools:


1.


**Every tool call requires a stated reason.** Before calling any tool, the agent must articulate its research goal and why it's making this specific call. This isn't bureaucratic overhead, it forces the agent to actually think about what it's looking for rather than pattern-matching on surface-level keywords. The reasoning becomes part of the context window, which means subsequent calls benefit from what was already figured out.


2.


**Parallel branch exploration.** Instead of a linear search, find one thing, decide, move on, agents can explore multiple branches simultaneously. Check the service that's most likely responsible


*and* the service that's adjacent


*and* the documentation that describes expected behavior, all at once. This dramatically reduces the time to a complete picture.


3.


**Semantic fallback hints.** When a search returns nothing useful, instead of giving up, the agent receives a hint: try searching for something semantically similar to what you were looking for. No vector database required just a prompt-level instruction that tells the agent to broaden its search strategy.


4.


**Hints on tool responses.** When a tool returns results, it also returns guidance on what to do next. This keeps the agent from getting stuck or looping, and means the search process is guided by accumulated context rather than starting fresh with each call.


All of this came from failure, not design. Every one of these mechanisms exists because I watched something break and figured out why.


##


**Wix Octocode Research: Plugging OctoCode Into the Wix Octocode Orchestrator Pipeline**


[OctoCode](https://github.com/bgauryy/octocode) solves the GitHub search problem. Wix Octocode Research, the enrichment service that powers Wix Octocode Orchestrator solves the wider problem: assembling complete context across everything relevant to a bug, not just code.


Wix Octocode Research uses OctoCode for static code analysis, Trino for normalized query access across all Wix databases, Grafana for live logs and runtime errors, and our internal and public documentation to determine whether a behavior is a bug or by design. Its output isn't just data, it's a structured research plan that the coding agent downstream can act on directly.


**Under the hood, Wix Octocode Research's architecture has three layers:**


**1. The Planner** takes the incoming research request, a support ticket, a Jira issue, a bug description and unpacks it. It identifies the intent, determines what the output schema should look like (because different downstream services want answers in different formats), does an initial documentation pass, and queries memory for anything relevant we've already learned. Only then does it hand off to the Researcher.


**2. The Researcher** is a full orchestrator with access to all of our tooling OctoCode, Trino, Grafana, documentation, and a set of sub-agents. Those sub-agents are the key architectural decision: rather than one agent with fifty tools crammed into its context window, we have specialized agents: a database agent, a logs agent, a code agent each with a focused toolset and their own MCP server.


Why? Because a database agent whose entire context is about querying and interpreting database results is far more effective than a general agent juggling everything at once. Context windows are finite, and attention matters. The cost is complexity at the boundary getting the schema right for how sub-agents return context back to the orchestrator took significant iteration, and we're still refining it.


We also experimented with


**dynamic helpers** : instead of calling a fixed set of sub-agents, the orchestrator can spin up an ad-hoc agent with exactly the tools needed for a specific investigation. A query that needs Grafana plus code analysis plus a database lookup gets one agent with those three tools, rather than three round-trips through separate sub-agents. The tradeoff between flexibility and predictability is still something we're working through.


**3. The Memory Layer** is what makes the system learn. Every completed research cycle the query, the planning decisions, the tools called, and the final output gets embedded and stored in a vector database. On the next similar request, the Planner retrieves those embeddings as hints, giving the Researcher a head start. Beyond storing results, the system also reviews itself: after each research cycle, the orchestrator reflects on which tools worked well, which instructions were effective, and feeds that back into the process.


##


**A Design Principle That Took Us Too Long to Learn**


When we first built Wix Octocode Research, the architecture was: Researcher does the work, hands off to a separate formatting agent to produce the final output. Seemed clean. It failed.


The formatting agent had no understanding of what the Researcher had actually discovered. It produced generic, shallow responses because it had no context for why anything mattered. The lesson:


**the agent that does the hard investigative work should be the one that produces the output.**


**Fighting the Frameworks: Why We Forked Google's ADK**


When designing Wix Octocode Research’s orchestration layer, we evaluated existing frameworks. LangChain felt too unstable for production breaking changes across frequent version updates made it hard to rely on at our scale. We moved to Google's ADK, which had a strong vendor behind it and a cleaner model.


But building at scale always comes with a tax. The ADK's documentation and examples were written for Python, and we were deep into a TypeScript effort. That meant forking the framework and doing significant heavy lifting ourselves. The bigger issue: out of the box, the TypeScript ADK executed MCP tool calls sequentially one after another.


For a research engine built entirely around parallel execution across GitHub, Trino, and Grafana, sequential calls were a performance killer. We rewrote the core tool-calling mechanism to support true concurrency, and optimized the token compaction logic ourselves. More maintainability debt, but the customizability we needed.


A second principle that saved us significant debugging time:


**uniform tool protocols.** Every tool in Wix Octocode Research regardless of what it queries speaks the same language: before calling, state why; when returning, include hints about what to do next. When agents share a protocol, the math of attention works in your favor. Agents don't have to spend context figuring out how each tool works they already know.


##


**What's Still Hard**


Finding specific records in a database from a high-level bug description is genuinely difficult. Isolating the right error from Grafana across dozens of services producing logs simultaneously is genuinely difficult. We haven't solved these, we're actively working on improving both, and we're honest about the gap between what the system can do today and what we need it to do.


The organizational knowledge problem is also unsolved. Teams have context that lives in Slack threads and people's heads, which services are in the middle of a migration, which behaviors are intentional despite looking broken, which architectural constraints make an otherwise-obvious fix dangerous.


We're building toward a model where each team maintains their own knowledge agent, and Wix Octocode Research can query those agents as part of any research cycle, without forcing teams into a specific format or tooling choice.


To tackle this structural knowledge gap, we're developing


**CDD Context Driven Development** . The idea is to give every engineering unit at Wix a simple interface to explicitly declare their domain context: their active repositories, primary database tables, and external dependencies. By making context declaration a first-class part of the development lifecycle, we can feed structured, up-to-date data directly into Wix Octocode Research's Planner, moving away from guessing toward precise architectural alignment.


##


**Conclusion: The Centrality of Learning and Measurement at Scale**


The long-term success of any self-healing system at an organization's scale fundamentally relies on constant measurement and moving from static processes to a continuous feedback and learning loop. For systems like Wix Octocode Research, this iterative process is baked into the architecture:


**Closed-Loop Learning:** The system must operate as a closed control loop.


Wix Octocode Research’s Memory Layer stores embeddings of every completed research cycle, including planning decisions and tool outcomes in a vector database. This gives the Planner a head start on similar requests by retrieving past resolutions as hints. Crucially, the orchestrator reflects on which tools were effective and feeds that learning back, ensuring continuous improvement and policy synthesis to reduce recurrence of similar defect classes.


**Key and Constant Measurement:** Efficacy and accountability are enforced through a uniform tool protocol across all agents. This requires every tool call to state its reason and every return to include hints on the next steps. This protocol ensures the agent is reasoning and not pattern-matching, turning internal behavior into a measurable, traceable process.


**Structural Context Alignment (CDD):** Overcoming the organizational knowledge problem requires a structural solution.


**Context Driven Development (CDD)** mandates that every engineering unit explicitly declares their domain context (repositories, database tables, dependencies). By making this context declaration a first-class part of the development lifecycle, up-to-date data is fed directly into Wix Octocode Research’s Planner, moving the system toward precise architectural alignment rather than relying on guesswork.


This emphasis on structural context, traceable protocol, and embedded outcome-driven learning is the core principle for building a research engine that doesn't just search, but reasons at massive scale.


[https://octocode.ai/](https://octocode.ai/) |


*Disclaimer: We are currently in ongoing improvements and learning cycles, constantly striving to optimize cost and quality while learning from every outcome.*


Watch


**Guy Bary** 's latest seesion covering octocode's story -


**From Code Search to AI-Powered Research Engine** (Hebrew):


This post was written by


**Guy Bary**


**More of Wix Engineering's updates and insights:**


-


**Follow us on:**[Twitter](https://twitter.com/WixEng) **|**[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) **** **|** ****[Instagram](https://www.instagram.com/wix_eng/) **** **|** ****[Facebook](https://www.facebook.com/WixEngineering/)


-


**Join our**[Telegram channel](https://t.me/wixeng) ****


-


**Visit us on**[GitHub](https://github.com/wix) ****


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/) ****


-


**Subscribe to our**[YouTube channel](https://www.youtube.com/WixTechTalks) ****


-


[Follow our Medium publication](https://medium.com/wix-engineering) ****


-


**Listen to our podcast on**[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) **,**[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA)
