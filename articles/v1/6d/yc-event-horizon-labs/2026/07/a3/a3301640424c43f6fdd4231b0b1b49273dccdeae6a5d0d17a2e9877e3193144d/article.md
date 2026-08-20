---
schema_version: "1.0.0"
document_id: "a3301640424c43f6fdd4231b0b1b49273dccdeae6a5d0d17a2e9877e3193144d"
company_key: "yc-event-horizon-labs"
company: "Event Horizon Labs"
source_id: "yc-event-horizon-labs-news-import-36ee4ccaf0e5"
canonical_url: "https://www.ehl.markets/posts/execution.html"
published_at: null
first_seen_at: "2026-07-25T03:47:22.258094+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:e90548be36ec1af2a2ee5cec5a88ba04f17491a730d0808f2ab208f4119f7e06"
---

# When agents design execution algorithms, what actually matters? — Event Horizon Labs

[← Back to research](https://www.ehl.markets/research.html) 2026 · UChicago Project Lab × EHL · optimal execution


## When agents design execution algorithms, what actually matters?


We already know an LLM-driven agentic loop *can* discover better trading strategies on its own — our[earlier work with UW–Madison](https://www.ehl.markets/posts/noisy.html) established that it does real research rather than just p-hacking. So feasibility is settled; the open question is the design one. Given that it works, *how should you organize the agents* — which choices actually move the results, which don't, and by how much? Working with the University of Chicago Project Lab, we ran **five different research paradigms** against the same backtester and the same three strategy templates and compared them head to head. The pattern that emerged was not the one we'd have guessed: across the dimensions we varied, *more* was usually worse. More rounds of debate, more context fed back into the loop, more forced agreement between agents — each one eroded the strategies. The setups that won did *less* : a single round of proposal rather than three, a short curated summary rather than the full transcript, and light, scheduled sharing rather than constant coordination. **Sometimes less is more** — the island model that finished first on out-of-sample PnL and Sharpe across all three templates won precisely by sharing sparingly and letting its searches run independent the rest of the time.


THE SETUP


Every experiment shares one loop. A researcher agent reads context from a working directory, proposes and codes a change to an execution strategy, and hands it to a backtest engine; a local evaluator scores the result, the agent decides whether it is finished or should iterate, and finished runs are committed to GitHub and archived to S3, with heavier backtests farmed out to a self-hosted runner. What changes between experiments is only the *research policy* wrapped around that loop — how much the agent remembers, whether it argues with itself, whether it edits its own instructions, and whether many copies run in parallel and share what they find.


The shared agentic research loop. The researcher iterates against a backtester until it judges a strategy finished; results are versioned and archived. Only the policy wrapped around this loop differs across the five experiments.


All five paradigms were tested against the same three execution-strategy templates, each a tunable rule for how to work an order into the market: **AFG** (aggressor-flow-gate, trade with order-flow pressure), **PTG** (position-tier-gate, scale participation by inventory), and **VRS** (vol-regime-sizer, size by the volatility regime). Every result is quoted against the un-evolved base template, and the numbers that decide the contest are *out-of-sample* : a held-out window the agent never saw while searching.


FIVE WAYS TO ORGANIZE THE SEARCH


**1 · Context-dependent researcher.** How much of its own history should the agent carry between iterations — bare metrics, a brief summary, or the full reasoning trace?


**2 · Proposer–criticizer.** One agent proposes a hypothesis, another attacks it, for up to three rounds of debate before implementation. Does arguing produce better strategies?


**3 · Self-evolving prompt.** The agent is allowed to rewrite its own driving prompt between loops — but only if the rewrite measurably improves results.


**4 · Island model.** Several independent populations evolve in parallel, then periodically migrate their best strategies to one another.


**5 · Quality-diversity (MAP-Elites).** Instead of chasing one global best, illuminate a whole archive of strategies that are each the best in their own behavioral niche.


EXP 1 · HOW MUCH SHOULD AN AGENT REMEMBER?


The first experiment varied only the agent's memory: a *metrics-only* mode (tens to a few hundred characters per loop), a curated *brief-summary* mode (one to three thousand), and a *full-trace* mode (up to a hundred-plus thousand). Within a run, more context did help — the improvement in PnL and Sharpe rises weakly but consistently with the number of characters fed back (OLS \\(R^2 \\approx 0.17\\) for PnL, \\(0.19\\) for Sharpe). But the best *out-of-sample* strategies did not come from dumping the full trace back in; they came from the **brief-summary** mode. Curated memory beat raw volume: a short, well-chosen recap of what worked generalizes better than the entire transcript, which tends to anchor the agent on its own past noise.


EXP 2 · DOES DEBATE HELP? (NO)


The proposer–criticizer setup let two agents argue over a hypothesis for up to three rounds before committing to code. The intuition — more scrutiny, better ideas — did not survive contact with the backtest. **Each additional debate round cost roughly 12–33% of PnL versus the base template.** A single round of proposal-and-consensus produced the strongest strategies; piling on rounds two and three reliably eroded them (regression slope \\(\\approx -32\\%\\) PnL per round). The criticizer talks the proposer out of good, slightly-unusual ideas and toward safe, over-fitted consensus — the multi-agent version of groupthink.


More debate, worse strategies. One round averages \\(+73\\%\\) PnL over base; rounds two and three fall below it. The fitted slope is \\(-32\\%\\) PnL per round (\\(R^2=0.24\\)) and \\(-18\\%\\) Sharpe per round. EXP 3 · A PROMPT THAT EDITS ITSELF


Here the agent could rewrite its own driving prompt. Each cycle has two phases: a research phase that generates, codes, and backtests a hypothesis while writing reasoning traces, and a critique phase that reads those traces and proposes an improved prompt. The safeguard is a hard gate — a rewrite is kept only if it improves at least four of five metrics, and otherwise *reverts* , so the prompt can ratchet up but never regress. On the VRS template this compounded sharply: Sharpe climbed from \\(4.19\\) to \\(13.72\\) and out-of-sample PnL from \\(+40.9\\%\\) to \\(+95.3\\%\\) over the base, the best of which (loop 7) nearly doubled it. Letting the agent improve *how it thinks* , not just *what it tries* , paid off — but only because the gate stopped it from talking itself into worse instructions.


EXP 4 · ISLANDS AND MIGRATION — THE WINNER


The island model ran three populations in parallel — four generations, two loops each — with a migration step between generations that copies the strongest strategies from each island into the others. It won the whole study, and the reason is specific.


Structure: three islands evolve independently within a generation, then a migration step shares the best performers across islands to seed the next generation.


**Parallelism alone was not the point — sharing was.** Run in isolation, the aggressor-flow island was actually *losing* , at \\(-43\\%\\) versus base; after it imported ideas discovered on the other islands it turned to \\(+233\\%\\). The vol-regime island reached \\(+525.8\\%\\) by its eighth loop, and position-tier \\(+30.7\\%\\). Across all three, **every island's largest jump landed in the loop immediately after a migration step.** The islands behaved like a real research team: a dead end on one island became another island's biggest win, so sharing *failures* mattered as much as sharing successes. Out of sample, on a held-out window the agents never searched, all three island bests beat their baselines by \\(169\\)–\\(272\\%\\).


PnL versus base by loop for each island. Gains cluster immediately after migration steps; the aggressor-flow island climbs from \\(-43\\%\\) in isolation to \\(+233\\%\\), and the vol-regime island reaches \\(+525.8\\%\\). EXP 5 · ILLUMINATING AN ARCHIVE


The last paradigm changes the goal. Instead of optimizing one number, quality-diversity search (MAP-Elites) lays down a grid of behavioral niches — here, how *selective* a strategy is against how *concentrated* its trading is in time — and keeps the best strategy found in each cell. The payoff is a map rather than a point: you can see *where* in strategy space performance lives. On the aggressor-flow arm the best illuminated cell reached \\(+214\\%\\) over base, and PnL peaked at moderate selectivity and moderate timing concentration rather than at any extreme — a structural read on the strategy family that a single-best optimizer would never surface.


MAP-Elites illumination on the aggressor-flow arm: best execution algorithm per behavioral cell. The peak cell reaches \\(+214\\%\\) over base, in the interior of the map rather than at an extreme. THE SCOREBOARD


Ranking the four optimizing paradigms by their best out-of-sample strategy in each template, the island model is first in every single cell — a clean mean rank of \\(1.00\\) on both PnL and Sharpe. (Quality-diversity is scored separately: it produces an archive, not a single contender for the global best.)


OOS — best per template Island Per-iteration Self-impr. prompt Proposer–critic


AFG · PnL **$4,141** #1


$2,312 #2


$1,714 #3


$1,672 #4


PTG · PnL **$5,517** #1


$4,432 #2


$4,254 #4


$4,378 #3


VRS · PnL **$5,713** #1


$2,730 #3


$3,515 #2


$2,562 #4


AFG · Sharpe **21.66** #1


18.56 #2


15.67 #3


14.90 #4


PTG · Sharpe **23.07** #1


22.20 #3


21.41 #4


22.26 #2


VRS · Sharpe **24.96** #1


20.68 #4


21.34 #2


20.76 #3


**Mean rank** **1.00** 2.33 / 3.00 3.00 / 3.00 3.67 / 3.00


Out-of-sample best strategy per paradigm and template, with rank in parentheses. The bottom row is mean rank (PnL / Sharpe). The island model ranks first in all six cells.


WHAT WE TOOK AWAY


**Sharing beats parallelism.** The island model did not win by running more agents; it won by letting them exchange discoveries on a schedule. Independent agents that never communicate leave most of the gains on the table.


**Consensus can hurt.** Forcing agents to debate to agreement made strategies worse, not better — scrutiny converged on safe, over-fit ideas. The structures that won kept exploration alive instead of collapsing it.


**Curated memory beats raw memory.** A short summary of what worked generalized better than the full transcript.


**Self-improvement needs a gate.** Letting an agent rewrite its own prompt compounded only because reverts blocked any change that did not measurably help.


The through-line is the same tension we keep meeting elsewhere: the best results come from keeping many semi-independent searches alive and sharing between them on a schedule, not from one fast-converging consensus. It is the structure behind the[noisy-rewards work](https://www.ehl.markets/posts/noisy.html) , and it is exactly why the economics in[how many agents to run](https://www.ehl.markets/posts/tokens.html) favor structured fleets over brute parallelism on the depth-limited problems that real research turns out to be.


SCOPE


Results are conditional on the backtester's market model and on three specific strategy templates over a single held-out window (test period 2026-03-26 to 2026-04-06). Out-of-sample here means held out from the agent's search, not live trading; live validation is future work. A paper is in progress. Next steps the team flagged: widen the pool of base algorithms, and kill-and-reseed weak islands to concentrate compute on what works.


Anthony Vo, Elliot Chung, Franklin Liu, Lindsey Lin, Milind Pathiyal
University of Chicago Project Lab & Event Horizon Labs · Spring 2026 (paper in progress)
