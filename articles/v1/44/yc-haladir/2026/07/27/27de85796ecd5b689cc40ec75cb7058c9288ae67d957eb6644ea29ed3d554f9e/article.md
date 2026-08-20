---
schema_version: "1.0.0"
document_id: "27de85796ecd5b689cc40ec75cb7058c9288ae67d957eb6644ea29ed3d554f9e"
company_key: "yc-haladir"
company: "Haladir"
source_id: "yc-haladir-news-import-6dc51fdf4310"
canonical_url: "https://haladir.com/blog/production-simulation"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-21T22:21:34.627558+00:00"
fetched_at: "2026-07-28T21:22:09.082656+00:00"
content_hash: "sha256:47eb89c5e9aae324b5692970b6c766a7d56279190f545225f9b19d3f46ca4bf0"
---

# Production Simulation: A Benchmark for Planning Under Constraints

Most of what we do at Haladir comes down to a single problem: which is how to make an optimal decision inside a complex operation, where every choice is bound by constraints that overlap and shift over time. Some time ago we built a benchmark for language models that, almost by accident, turned out to be a miniature of that exact problem. While the benchmark is about factory operations, the OR principles behind it, we believe, deserve some explanation.


The benchmark consists of eight different factories, spanning a kitchen, a job shop, a brewery, and more. Each is a turn-based production simulation where a model has to plan, build, and run a small factory under deadlines, scarce resources, and disruptions it cannot see coming. Labor pressure, limited machines, a breakdown at the wrong moment, none of these can be reasoned about in isolation, which is what makes it a clean test of planning under interacting constraints.


## The Environment


For the sake of simplicity, we’ll focus on the agent managing a CNC machining job shop.


The floor of the shop is represented by a 10 by 12 grid where the agent has the ability to build stations, hire workers, order materials, configure product pipelines, set routing rules, and ship finished products before their deadlines. Each turn it reads a text observation of roughly 1,500 tokens describing the current state of the floor and responds with tool calls. There are thirteen actions, and each one costs action points drawn from a budget of five per turn, so the model is never choosing a single move in isolation but a slate of moves against a tight allowance.


A condensed version of what the agent sees mid-game looks like this:


```text
=== Turn 16 | Phase: production | AP: 5 | Budget: $11240 ===
-- Stations --
Saw-1 [Band Saw] at (1,3) | status=idle | health=80% | recipe=saw_steel_4140
Lathe-1 [CNC Lathe] at (2,4) | status=processing | health=65% | recipe=turn_steel
Mill-1 [CNC Mill] at (3,5) | status=idle | health=0% | recipe=mill_steel_body | queue=2/5 [Turned Steel Shaft(f=9), Turned Steel Shaft(f=10)]
Grinder-1 [Surface Grinder] at (4,6) | status=processing | health=75% | recipe=grind_steel
Stage-1 [Staging Rack] at (6,5) | status=idle | health=90% | recipe=stage_part
Pack-1 [Deburring/Packaging] at (7,5) | status=idle | health=85% | recipe=deburr_manifold | setup=Hydraulic Manifold
-- Cold Storage (4/15) at (0,5) --
Metal:steel_4140 id=22 freshness=18 | Metal:steel_4140 id=23 freshness=18
Consumable:cutting_tools id=24 freshness=no expiry | Consumable:grinding_wheels id=25 freshness=no expiry
-- Workers --
W1 at (2,4) | status=assigned | fatigue=0 | assigned=(2,4)
W2 at (3,5) | status=idle
-- Dispatch Bay (0/10) at (9,5) --
-- Orders --
ORD-1: Hydraulic Manifold qty=2 due=30 shipped=0
-- Events --
Breakdown at Mill-1: health=0, offline until repaired
```


From this the agent has to notice the broken mill, weigh it against everything else competing for its five points, and commit to a response, perhaps repairing Mill-1 before the stalled queue backs up the whole line, before ending the turn. How a turn resolves comes down to three things:


**Two phases:** Turn zero is the planning phase, where the agent lays out stations, sets recipes, hires workers, places initial orders, and defines routing, all with unlimited points and instant construction. Every turn after is production, where the constraints matter: the points budget is enforced, construction takes real time, and workers have to walk to their stations before anything happens.


**The tick:** When the turn ends, a fourteen-step deterministic tick advances the world: delivering ordered materials, expiring stale intermediates, running recipes, dispatching finished outputs, deducting salaries, and applying whatever disruption the scenario has scheduled. Because it is deterministic, the same actions always produce the same world, so a score reflects the quality of the agent’s decisions and nothing else.


**Scoring:** The score is simply the budget the agent ends with divided by the budget it started with, which rewards shipping quickly and punishes idle workers, construction it did not need, and orders left unfulfilled.


## What Makes It Hard


The difficulty is in how the mechanics compound. A typical product takes roughly five to eight sequential steps across different stations, so the initial layout an agent commits affects the transit times for the rest of the game. But layout is only one thing it has to track. Stations lose health, tooling wears, and intermediates spoil. Breakdowns, walkouts, scrapped batches, rush orders, land on turns it cannot predict and invalidate parts of the plan it already committed to. And every fix has a cost somewhere else: an extra station to clear a bottleneck means budget, points, and a new hire, and whether that trade pays off is a marginal call against deadlines several turns out, something the agent has to reason toward on its own.


Each of these is manageable alone. What’s hard is holding all of them at once, before a single move: every added part is another thing to weigh at the margin and another consequence to trace forward, and the combinations branch faster than the agent can reason through. That coupling is the whole difficulty, and it’s the same property that keeps cross-cutting operational decisions in human hands outside the benchmark too.


## Grading


A run earns one number: the budget the agent ends with divided by the budget it started with, clamped to the range zero to one. Because the tick is deterministic, that number depends only on the agent’s decisions. A run that ends with at least the money it began with scores one, whether it broke even or came out ahead. A run that spends itself into the ground scores zero. Most land in between, and where they land is set by the quality of the initial plan.


The budget is the only ledger, and every choice flows through it. Shipping an order on time pays its full revenue, shipping it late pays the revenue minus a penalty, and an order that is never shipped is charged that penalty on the final tick. Against that income, the agent pays for everything it does: building and moving stations, hiring, upgrades, repairs, materials, and a salary every turn for every worker, busy or idle. The final balance nets all of it into one figure, and that figure, measured against the starting budget and capped at one, is the score.


Feasibility is settled before a scenario ever reaches a model. Each scenario passes a feasibility analysis that checks its critical-path timeline against the deadlines and its projected revenue against the budget, and we confirm it can be won by tuning a parameterized strategy with CMA-ES until that strategy finishes solvent. This is part of building the benchmark, not part of grading a run. It guarantees every scenario can be won, so a run that scores zero has failed to plan rather than hit a problem with no solution.


## Calibration


The benchmark runs eight domains and twenty scenarios, spanning single-product lines, multi-product coordination, disruption recovery, and tighter constrained and doubled-quantity variants, and every scenario is verified solvable before it is admitted. Every scenario also ships with two to four natural-language hints describing its key planning decisions, which lets us separate a model that does not know what matters from one that cannot act on it. (The results below draw exactly that line.) The scenarios sit in a useful range: a small fraction goes unsolved by every model, a stable floor is solved on every run, and most of the rest land in between, where the initial plan decides the outcome.


## Results


We tested nine model configurations: Claude Fable 5, GPT-5.5 at medium, high, and xhigh reasoning effort, GPT-5.4 at medium and high, Claude Opus 4.6, Claude Opus 4.8, and Gemini 3.1 Pro, plus hinted variants of GPT-5.5 at medium effort and of Gemini. Each configuration ran every scenario five times, a hundred rollouts per configuration, capped at a thousand turns per rollout. The set skews hard by design: Brewery and Cooking, the two toughest domains, make up nearly half of it, and disruption and multi-product scenarios feature heavily.


Claude


OpenAI


Gemini


Hinted run


Claude Fable 5


0.281


GPT-5.5 (high)


0.277


GPT-5.5 (xhigh)


0.274


GPT-5.4 (high)


0.220


GPT-5.4 (medium)


0.150


Claude Opus 4.6


0.144


GPT-5.5 (medium)


0.135


GPT-5.5 (medium, hinted)


0.100


Claude Opus 4.8


0.067


Gemini 3.1 Pro


0.044


Gemini 3.1 Pro (hinted)


0.032


Mean score per configuration across the benchmark's twenty scenarios, with the share of runs scoring above zero and the perfect and zero counts out of 100 rollouts. Run Mean score Score > 0 Perfect Zero


Claude Fable 5 0.281 57% 8 41


GPT-5.5 (high) 0.277 61% 5 39


GPT-5.5 (xhigh) 0.274 61% 6 39


GPT-5.4 (high) 0.220 47% 5 52


GPT-5.4 (medium) 0.150 53% 1 47


Claude Opus 4.6 0.144 51% 0 49


GPT-5.5 (medium) 0.135 41% 4 59


GPT-5.5 (medium, hinted) 0.100 38% 0 62


Claude Opus 4.8 0.067 35% 0 65


Gemini 3.1 Pro 0.044 16% 0 82


Gemini 3.1 Pro (hinted) 0.032 17% 0 83


Figure 1. Mean score per configuration, five rollouts per scenario. Faded bars are hinted runs. Claude Fable 5 leads every aggregate, with GPT-5.5 at high effort within noise of it.


Claude Fable 5 leads every aggregate: the highest mean, the most scenario wins (six of twenty across the nine runs), and eight of the twenty-four perfect games recorded. It is also the only model to make multi-product coordination tractable. It solves two-product scenarios outright, scoring 0.831 on the hardest chocolate scenario with three perfect runs, and leads the multi-product category by a wide margin, 0.402 against 0.260 for the next-best run. The two leaders split the categories: Fable 5 leads multi-product and disruption scenarios, GPT-5.5 at high effort leads single-product, constrained, and doubled-quantity scenarios, and the overall gap between them (0.281 vs 0.277) is within noise.


Claude Fable 5


GPT-5.5 (high)


Multi-product


6 scenarios


0.402


0.260


Disruptions


6 scenarios


0.379


0.371


Single product


6 scenarios


0.162


0.221


Constrained


1 scenario


0.081


0.220


Doubled quantity


1 scenario


0.000


0.200


Mean score by scenario category (Claude Fable 5 vs. GPT-5.5 at high effort), with the number of scenarios per category. Scenario category Scenarios Claude Fable 5 GPT-5.5 (high)


Multi-product 6 0.402 0.260


Disruptions 6 0.379 0.371


Single product 6 0.162 0.221


Constrained 1 0.081 0.220


Doubled quantity 1 0.000 0.200


Figure 2. Mean score by scenario category, Claude Fable 5 vs GPT-5.5 (high). Fable 5 leads where constraints interact most, in multi-product and disruptions, while GPT-5.5 leads the single-line categories.


The largest single lever we measured, however, is not model choice but reasoning effort. GPT-5.5 doubles its mean from medium to high effort, 0.135 to 0.277, closing from far behind to within noise of Fable 5, and then saturates: xhigh scores the same (0.274) while costing forty-five percent more per rollout. GPT-5.4 moves the same direction, 0.150 at medium to 0.220 at high.


GPT-5.5 (medium)


1.8M tokens · $9.74


0.135


GPT-5.5 (high)


2.2M tokens · $14.12


0.277


GPT-5.5 (xhigh)


2.7M tokens · $20.41


0.274


GPT-5.4 (medium)


2.1M tokens


0.150


GPT-5.4 (high)


1.9M tokens · $6.77


0.220


Claude Fable 5 (adaptive)


3.7M tokens · $26.32


0.281


Mean score by reasoning-effort configuration, with mean tokens and mean dollar cost per rollout. Run Mean score Mean tokens Mean cost


GPT-5.5 (medium) 0.135 1.8M $9.74


GPT-5.5 (high) 0.277 2.2M $14.12


GPT-5.5 (xhigh) 0.274 2.7M $20.41


GPT-5.4 (medium) 0.150 2.1M n/a


GPT-5.4 (high) 0.220 1.9M $6.77


Claude Fable 5 (adaptive) 0.281 3.7M $26.32


Figure 3. Reasoning-effort scaling. Each row shows a configuration’s mean score with its mean token and dollar cost per rollout. The medium-to-high step doubles GPT-5.5’s score; the high-to-xhigh step buys nothing.


That deliberation cannot be injected from the outside. Appending each scenario’s planning hints to the prompt helps neither GPT-5.5 at medium effort (0.135 falls to 0.100) nor Gemini 3.1 Pro (0.044 to 0.032), even though traces confirm both models faithfully execute the hinted advice: hinted chocolate runs pre-build the hinted stations at turn zero, hinted pilsner runs delay the yeast order as instructed. Hints tell a model what matters. Only reasoning effort changes whether it can act on it.


Model generations are not monotonic improvements, either. Claude Opus 4.8 scores less than half of Opus 4.6 run in the same week (0.067 against 0.144, with zero perfect games in a hundred rollouts), and GPT-5.5 trails GPT-5.4 at matched medium effort. Each model also fails in its own way: GPT-5.5 at medium effort over-revises, flip-flopping recipes and rewriting routes; Gemini either quits fast and cheap or burns turns without shipping; Opus 4.8 spends the same on wins and losses without converting.


One caution before reading any single number: the score behaves differently on either side of a capability threshold. Fable 5 and GPT-5.5 at high effort earn 94–95% of their score from runs that actually ship product, and they recorded eight and five runs respectively that ended with more budget than they started. Gemini’s score is mostly a budget-preservation artifact: its production-only mean, which zeroes every run that shipped nothing, is 0.006, because a model that spends little and submits early banks score without playing the game. Cross-model comparisons that span that threshold compare different quantities, and a ship rate should accompany any mean.


Failure is also the expensive path. Fable 5’s zero-score runs average 4.1M tokens and $33.85 against 2.0M and $7.12 for its perfect runs, and every model shows the same shape. Spending more tokens on the same model at the same effort is a symptom of a run going badly, not a cause of it going well. Provisioning more reasoning up front, through the effort setting, is the one lever that reliably moved scores, at least until it saturates.


Solid bars = perfect-score runs · faded bars = zero-score runs


Mean tokens (millions)


2M


4.1M


1.6M


2.6M


Claude Fable 5


GPT-5.5 (high)


Mean turns


54


64


65


76


Claude Fable 5


GPT-5.5 (high)


Token, turn, and dollar cost of failed (zero-score) versus perfect-score runs. Metric Claude Fable 5 GPT-5.5 (high)


Mean tokens, perfect-score run 2.0M 1.6M


Mean tokens, zero-score run 4.1M 2.6M


Mean turns, perfect-score run 54 65


Mean turns, zero-score run 64 76


Mean cost, perfect-score run $7.12 $10.16


Mean cost, zero-score run $33.85 $16.46


Figure 4. Token and turn cost of failed versus perfect runs. Zero-score runs consume roughly twice the tokens of perfect runs for Fable 5, and cost nearly five times as much.


Only two of the twenty scenarios, textile-jacket and woodworking-cutting-board, went unsolved by every configuration, and GPT-5.5 at xhigh posted the only nonzero score on the hardest brewery scenario. Everything else was cracked by at least one model at some effort level, which is the calibration working as intended: the failures the benchmark records are failures of planning, not of possibility.


The results state the benchmark’s whole point. The environment is small and fully specified. Real operations are not. But the demand on the agent is the same: track many constraints at once, plan before acting, and stay coherent when conditions change mid-run. And what the results show is that capability arrives unevenly: multi-product coordination is tractable for exactly one model, a successor model can score half its predecessor, and the biggest measured gains came not from newer weights but from paying for more deliberation. Even the best configuration averages 0.281 on scenarios that are all verified winnable. The cross-cutting decisions that run a real operation still rest with experienced people, and the gap this environment exposes is the one our platform is built to close.


## Read About How Poetiq AI Used Our Benchmark


[poetiq.ai Benchmarks Are Dead (for us) Poetiq's Metasystem autonomously built harnesses that set new SOTA across six diverse benchmarks, with zero human intervention, often on previous-generation models.](https://poetiq.ai/posts/benchmarks_are_dead/)


All runs: Claude Fable 5, GPT-5.5 (medium / high / xhigh reasoning effort), GPT-5.4 (medium / high), Claude Opus 4.6, Claude Opus 4.8, and Gemini 3.1 Pro, via OpenRouter with automatic provider routing (GPT-5.4 at medium effort ran via the OpenAI API directly). Twenty scenarios, five rollouts per scenario per run, max 1,000 agent turns, max_tokens 16384, extended thinking off, no system prompt; each run receives one user message (rules reference plus turn-zero observation) and sixteen tool definitions, with hinted runs appending the scenario’s hint block to that message. Temperature follows each model’s API contract: omitted for Fable 5 and Opus 4.8, 0.7 for Opus 4.6 and Gemini, 1 for GPT-5.5 and GPT-5.4. Full per-scenario detail, including score-above-zero rate and perfect and zero counts, is available on request.
