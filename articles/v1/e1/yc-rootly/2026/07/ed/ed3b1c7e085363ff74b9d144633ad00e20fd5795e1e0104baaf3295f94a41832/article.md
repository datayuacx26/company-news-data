---
schema_version: "1.0.0"
document_id: "ed3b1c7e085363ff74b9d144633ad00e20fd5795e1e0104baaf3295f94a41832"
company_key: "yc-rootly"
company: "Rootly"
source_id: "yc-rootly-news-import-8d53140345fd"
canonical_url: "https://rootly.com/blog/what-doom-taught-us-about-ai-assisted-incident-response"
published_at: "2026-07-16T19:35:00+00:00"
first_seen_at: "2026-07-22T12:12:19.953280+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:61e878cf3b805de8fcc2ff0e2564b20a5513a28fddc463deb247c23e2ee4e444"
---

# What Doom taught us about AI-assisted incident response

Introducing[Doom Agent Arena](https://github.com/Rootly-AI-Labs/doom-agent-arena) , an open-source real-time game environment benchmark where AI agents control Doom players via MCP, and fight each other across multiple rounds.


By building Doom Agent Arena, our goal was to expand our research scope to better understand how LLMs can assist incident responders in resolving outages as quickly and effectively as possible. Although built in a game setting, the benchmark targets reliability-relevant LLM skills: interpreting dynamic environments, reasoning about consequences, recovering from failed plans, and adapting under time or cost constraints.


Let’s dive into how we built it, the methodology, and the findings.


## How Doom Agent Arena was built


Most other Doom benchmarks feed game frames to a vision model. But because our goal was to test LLMs' reasoning capabilities, our benchmark is instead letting agents observe the Doom game state via our MCP server as structured JSON, including the map and resources, and submit high-level plans for Doom to execute.


Latency was one of the main engineering challenges. Doom is built for humans, who make decisions in milliseconds, but a model round-trip takes far longer, so we had to balance the two. The tool calls themselves are fast, but the model's thinking takes up to 8 seconds per decision. That ruled out letting the model play in real time. So we went with a mixed approach: the model makes the high-level decisions, like what the player should do and where it should go, while Doom's engine handles the execution (moving, aiming, shooting).


## Methodology


The game is a one-on-one Doom deathmatch on a walled arena stocked with resources: health packs and a shotgun. Two AI agents each control a player, and the goal is simple: kill the opponent before they kill you. Agents have to read the map, find routes around the walls, grab the shotgun and health at the right moments, and decide when to push a fight and when to retreat to heal.


To keep it fair, we built a custom symmetrical map so neither side starts with an edge. For the findings we will present, we ran four of OpenAI's models against each other across 120 matches, 20 rounds per pairing, swapping spawns at the halfway mark.


## Findings


Model gpt-5.5 finished first at a 66% victory score, followed by gpt-5.4 at 52.5%, gpt-5.3-codex-spark at 41%, and gpt-5.4-mini at 39.2%. Now let’s dive into some of the findings and how they could inform the design of better AI-assisted incident resolution.


### Thinking longer isn’t a winning strategy


You might expect an agent that takes longer to decide to be deliberating more carefully and playing better. The opposite was true. The clearest case is gpt-5.3-codex-spark: in rounds where it deliberated longer than its own median, its win rate fell by 28 percentage points. In other words, the extra time wasn't buying better decisions. More often, a slow turn was a sign the agent was in trouble, not a mark of care.


In incident response, every second counts. You want to give an agent enough time to get it right, but there's a balance to strike, and a stalling agent may be a signal worth watching, since longer deliberation can track worse outcomes rather than better ones.


### When the agent wrote its own runbook


For a few duels, we experimented with 30 rounds instead of 10. In those runs, gpt-5.5 stopped planning each turn with the model and wrote its own Python controller. It used shortest-path routing and a simple priority policy: grab the shotgun, heal if low, chase the last-seen opponent.


A lot of incident response is deterministic. For an AI SRE, that means you don't always need fresh model reasoning at every step. Encoding known investigation patterns as runbooks is faster, cheaper in tokens, more reproducible, and keeps the resulting actions auditable after the fact. The model is best used as the author and supervisor of those runbooks, not the per-step executor.


Those runbooks should come from the reliability practitioners, but they should remain living. In the Doom benchmark, the agent improved its own controller by watching the results and rewriting the policy when it got stuck. That is the loop you want in production: automation that keeps sharpening from real outcomes instead of going stale.


### Fast decisions didn't win, but speed compounds


We found that the lighter model codex-spark had the lowest decision latency, about 6.6 seconds per decision and roughly 44% faster than the slowest model, and it submitted nearly twice as many play plans as the fewest (730 versus 378). It planned faster and more often than anyone.


Faster decisions didn't win the game; judgment did. But in production, speed still compounds. An incident is a long loop of pulling metrics, querying logs, forming a hypothesis, and pivoting, and much of that loop doesn't need heavy reasoning. The mechanical steps, fetching dashboards, filtering logs, running known diagnostics, can go to a lighter, faster model like codex-spark, leaving the strong model for the judgment calls like forming the hypothesis and choosing the fix. Each step is also heavier in production than in our benchmark, often a large context window and several tool calls taking tens of seconds, so across the hundreds of decisions a complex incident can take, a few seconds saved per step add up to tens of minutes off your MTTR.


## What’s next


We're still processing the findings and want to dig deeper into other signals that could inform reliability research, with the goal of writing a paper. We are also expecting to find more interesting results and behavior by drastically increasing the number of rounds. If you are interesting in an in-depth article about methodology and findinfs


While Rootly AI Labs already has a benchmark dedicated to evaluating LLMs on SRE-type skills,[SRE-skill-bench](https://sreskillsbench.com/) , adjacent domains like this the Doom Agent Arena can surface perspectives a purpose-built benchmark might miss.


If you want to test the project (which is very fun to watch), go to its[GitHub repository](https://github.com/Rootly-AI-Labs/rootly-doom-agent-arena) . If you're interested in joining the project, reach out at sylvain@rootly.com or just open a PR.


‍


‍


‍
