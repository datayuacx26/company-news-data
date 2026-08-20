---
schema_version: "1.0.0"
document_id: "d2c653f1e6ef1c3f858bfc3380756682ce137d94131bddc2630a2e77ffb7970e"
company_key: "jfrog-ltd-ordinary-shares"
company: "JFrog Ltd."
source_id: "jfrog-ltd-ordinary-shares-rss-4486f0fc66d1"
canonical_url: "https://jfrog.com/blog/jfrog-boost-saves-ai-coding-tokens/"
published_at: "2026-07-09T13:01:39+00:00"
first_seen_at: "2026-07-20T23:22:16.766689+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:f9dd3775670ea4676f3cb8fc2c04aa2af089315a1b36f29730797cbc14513d06"
---

# Boost Is Now In Public Preview

Today, we’re excited to announce that **Boost is moving out of beta and into public preview** .


After months of building, breaking, and rebuilding inside JFrog’s own R&D organization, Boost is ready for the world. If you are currently running into token limits, unpredictable costs, or runaway usage from AI agents,[Boost](https://boost.jfrog.com/) was built for you.


It has already helped our teams reduce token spend while maintaining performance. Now, we are making it available to the community for **free** , so you can see the same impact in your own development workflows.


Here’s the full story of how we got here.


## The Bill That Broke Us


A few months ago, we hit our absolute breaking point.


We opened our billing dashboards and saw what no engineer ever wants to see – individual Cursor bills hitting over **$700 for less than 2,000 requests** . Claude Code sessions clocking in at over **$1,700** , and then, right on cue, the dreaded corporate email with the subject heading: “ *AI Spend Guardrails Starting Feb 1* ”


We were bleeding cash and constantly running into “API limit exceeded” messages because our agents were consuming tokens like water.


As engineers who’ve spent our careers at JFrog optimizing build systems and cutting waste out of development pipelines, we do not tolerate waste. Not in our software builds, and not in our AI spend. So we did what engineers do, we went looking for the source of the problem.


## 90% of What You’re Paying For Is Noise


Here’s what we found: Coding agents aren’t dumb, they’re just drowning in irrelevant contexts.


Input context is now roughly[70% of your total AI bill](https://cursor.com/insights#input-context-is-becoming-the-main-token-cost) . Every prompt turn, your agent is ingesting massive amounts of text, most of which it doesn’t need. We found agents reading **10,000 lines of code to use 10 of them** . Over 90% of what you’re paying for gets read once and thrown away.


We traced this waste to three specific habits:


**1. The Test Log Avalanche**
When a test fails, the instinct is to stream raw log output directly to the agent. We tracked a real scenario where an agent was fed 847 lines of a pytest log. The model burned premium tokens parsing hundreds of “PASSED” lines, dependency warnings, and environment metadata, when all it actually needed were 9 lines: the exact failure traceback. We built a log-distiller that strips irrelevant lines before they ever leave your machine, shrinking payloads by over 80%.


**2. Blind Grep & Find Routines**
A basic` grep` for a common function like` getUser` returns an unordered wall of matches: minified caches, legacy docs, changelogs, mocks, all flattened together. In our testing, a single query pulled 119 matches. The agent re-read all 119 on every prompt turn just to find the one it needed. We solved this by building an LSP-backed binary that gives agents language-aware search, dramatically reducing search token waste and speeding up coding tasks.


**3. The Truncation Hallucination Loop**
Many basic token-saving scripts try to save budget by aggressively slicing terminal output. But we watched this create an entirely new problem: Agent amnesia. In our testing, when a script aggressively hid context, the agent would lose its bearings, panic, and enter a loop — running the exact same` ls` or` cat` command up to six times in a row just to re-verify its environment. The agent was burning more tokens on repetitive, defensive checks than we were saving by cutting the logs. This realization is exactly why we didn’t just build a blind filter; we built an architecture that gives the agent an understanding of the optimization layer, stopping the retry spiral before it starts.


## What We Learned Building the Beta


Our first version was simple based on a lightweight binary called` boost` that wraps every agent command.


Previously we used to run the standard agent command:


```text


ls -l
kubectl apply -f ...
docker run ...


```


Now, we add` boost` functionality to the same basic command structure:


```text


boost ls -l
boost kubectl apply -f ...
boost docker run ...


```


This gave us full visibility into every token flowing through the system. But the beta taught us three things fast:


**The security problem:** When every command is prefixed with` boost` , agent environments treat them as one program. One “Always Allow” click on` boost ls -l` and the agent has blanket approval for everything, including` boost rm -rf /` . We had to rethink permission handling completely.


**The false savings problem:** A lot of tools on GitHub claim 90% token reduction. Most either break your agent or are measuring the wrong thing. If you intercept output before a` grep` filter and claim credit for what` grep` was going to cut anyway, that’s not called savings. We needed to cut the right tokens, not just the most.


**The truncation problem:** Aggressive cutting made our agents useless. They’d lose context mid-debug, hallucinate fixes, and spiral. The goal was never minimal tokens. It was accurate agents that use fewer tokens.


## What We Shipped


The solution we landed on is intentionally lean featuring two capabilities that put control back in the hands of the agent and the developer:


` boost skill` – A minimal system prompt that teaches the agent exactly what` boost` is, how it works, and how to interact with the environment intelligently. The agent understands the optimization layer, rather than being silently manipulated by it.


` boost retrieve` – If the agent ever needs data that was filtered out, it runs` boost retrieve` . This instantly fetches the full, unedited output of the last executed command from its session history. Nothing is ever permanently lost, just efficiently stored until needed.


This architecture means agents stay smart. They can always get back what they need, and the only tokens consumed are the ones that matter.


## The Incredible Result: 100 Billion Tokens Saved


We didn’t just build this in a vacuum. We rolled Boost out gradually across JFrog’s entire R&D organization of over **1,000 engineers** , and let the data guide every decision. We tracked how often agents needed` boost retrieve` , which technologies required the most context, and where we could safely trim.


The results exceeded our wildest expectations.


We scaled our total token savings from a few billion to over **100 billion tokens saved** , all while making our internal coding agents faster, leaner, and more accurate than before.


**One thing we want to make clear: Boost collects only high-level telemetry such as token counts and macro errors to improve productivity. We never see, store, or transmit any code or conversations. Privacy isn’t just a checkbox for us, it’s at the very core of our application.**


## Try Boost Today


We’re incredibly proud of what Boost has become. It’s truly amazing to see how a tool that started as a weekend project, grew into an enterprise-grade optimization engine, tested across one of the largest software development organizations in the world.


If you’ve ever winced opening a billing dashboard for AI tokens, hit an API rate limit mid-flow, or watched an agent burn your entire context budget on a test log, Boost was built for you.


**Boost is now in public preview, and it’s free. Get started at[boost.jfrog.com](https://boost.jfrog.com/)** .


Let’s keep the agents honest and the pipelines humming.
