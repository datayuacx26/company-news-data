---
schema_version: "1.0.0"
document_id: "320c73138b4978e4bfd2e9d2c7977d8f22ba41052b24c399f7901806852acdd3"
company_key: "yc-synth-3"
company: "Synth"
source_id: "yc-synth-3-news-import-d5c503e27bee"
canonical_url: "https://www.usesynth.ai/blog/go-explore"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-24T04:58:53.081142+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:617f223ac5290317d5e3854b448b8f68c6e8cfb69c41930799f2d4590029b433"
---

# GELO: Archive-Based Optimization for Long-Horizon Agents

Headline Results


### What moved across three worlds


Held-out evaluation in every lane; baselines share the same observations and budgets.


policy:gemini-3.1-flash-lite


modality: prompt optimization


Environment Benchmark Baseline Go-Explore Change


Crafter Wood pickaxe unlock 70% 83% +13pp


Furnace placement 0% 17% +17pp


Craftax Heldout mean reward 2.00 4.04 +2.04


Best deep rollout 4.0 best 8.1 0/80 diamond


NetHack Scout coverage (200-step) 29.4 tiles 37.0 tiles +26%


Heldout seeds and eval contracts match the per-benchmark sections below.


## The Algorithm


Prompt optimization becomes qualitatively harder when the task is an episode rather than an answer. On a static benchmark, a candidate prompt fails at the final prediction. In an agent environment, the same candidate can be locally good and globally unusable: it may reach the furnace and then loop, collect iron and craft the wrong item, or spend the evaluation budget redoing an opening route after the world state has changed.


This creates a credit-assignment problem that ordinary prompt search is not designed to solve. A full-episode score entangles three distinct questions: whether the policy can return to the right region of state space, whether it can execute the next skill from that region, and whether the resulting behavior survives heldout seeds. GELO separates these questions by making the checkpoint, the local objective, and the promotion rule explicit.


The design is inherited from[Go-Explore](https://arxiv.org/abs/1901.10995) , which was introduced for hard-exploration reinforcement-learning problems. The central move is simple: maintain an archive of promising states, return to one before exploring, and use the resulting trajectories to expand the archive. The later Nature version,[First return, then explore](https://arxiv.org/abs/2004.12919) , framed two common exploration failures as **detachment** — forgetting how to reach useful states — and **derailment** — exploring before returning to the intended state.


GELO ports that idea into long-horizon language-agent optimization. Atari cells become hosted checkpoints. Game-score deltas become verifier evidence, achieved labels, reward vectors, and heldout replay. Random exploration becomes candidate generation by proposer models, local hill-climbs, and archive-level consolidation. The object of search is no longer only the prompt; it is the tuple:


text


```text
(checkpoint region, local objective, candidate family, evidence, promotion rule)
```


At the top level, GELO is an archive loop rather than a single mutation loop:


1. **Archive.** Store checkpoints, trajectory segments, policy candidate IDs, achieved labels, heldout evidence, and lineage.
2. **Select themes.** Identify frontier regions where the agent is close to a meaningful new achievement or repeatedly fails in a consistent way.
3. **Hill-climb locally.** Run scoped candidate searches from each theme checkpoint, with a local success metric and a bounded rollout budget.
4. **Consolidate.** Re-score winners on heldout seeds; promote only candidates that improve the archive or expand coverage.
5. **Replay and relabel.** Use heldout replay to update evidence, label new achievements, and expose the next frontier.


Go-Explore algorithm


### Archive, branch by theme, consolidate the winners


checkpoints → themes → hill climbs


Go-Explore alternates between archive-level breadth and theme-level local search, and the loop is asynchronous: the core proposer can run several rounds, each adding themes to the queue, while aux proposers hill-climb every open theme in parallel. Consolidation dequeues winners only when their themes mature, fans them back into one archive, and replay re-scores the result on heldout seeds.


The Algorithm


### Remember, return, explore — then consolidate


checkpoints · archive loop · hill climbs


```text
1  archive   =   seed  (baseline, fresh_rollouts)     # candidates,   checkpoints  , heldout evidence       3  while   budget_remains:    4      themes =   select_themes  (archive)          # checkpoint region -> next objective    5        for   theme   in   themes:                 # local hill climbs, run in parallel    6            checkpoint   = theme.checkpoint    # remember, return    7          slate =   propose  (theme, archive.evidence,   plugin  )    8              # plugin: prompt edits · BC beta · future RLVR / OPSD    9            for   cand   in   slate:              # ...then explore from it    10              rollouts   =   resume  (checkpoint, cand)     # branch from checkpoint    11              cand.score =   local_objective  (rollouts)    12          theme.champion =   argmax  (slate)    13      archive =   consolidate  (archive, champions)    # promote winners, retire themes    14        relabel_heldout  (archive)                   # fresh seeds, achievement labels       16  return   archive.best_on_heldout             # plus the frontier map
```


The whole loop. Everything Go-Explore adds over single-prompt optimizers lives in three calls:` resume` branches from a checkpoint instead of restarting,` local_objective` scores the theme's next rung instead of final reward, and` consolidate` folds local winners back into one archive. The` plugin` argument is the candidate-type seam: prompt edits are the current path, filtered behavioral cloning is the beta lane, and RLVR / on-policy distillation remain named lanes that fail closed until enabled.


A **theme** is a typed local hypothesis: from this checkpoint distribution, optimize this next objective under this success metric. Themes prevent the optimizer from collapsing into one global prompt search. They also make the evidence auditable: a candidate is not merely “better”; it is better for a named frontier such as preserving the opening route, converting stone into a stone pickaxe, recovering coal after iron, or descending after locating a staircase.


The candidate surface is intentionally broader than a single system prompt. A candidate can change policy instructions, strategy context, tool-use rules, verifier-facing guidance, termination criteria, and attached skills. The archive tracks which candidate families earned evidence and which themes they cover.


This is the main algorithmic distinction from prior prompt optimizers. MIPRO and GEPA decide how to update a prompt or language-program component after observing evaluation evidence. GEPA, in particular, uses natural-language reflection over trajectories and a Pareto frontier to combine complementary lessons. GELO keeps that style of reflective candidate generation, but adds the long-horizon decision that GEPA does not try to solve directly: **which checkpoint should generate the next data?** The difference is not “better prompt search”; it is prompt search conditioned on an archive of partial progress.


We are not using this post to claim a completed causal ablation of checkpoints versus fresh-start search. The paired checkpoint-on/off and warm-archive/fresh-start ablation bar was not met, so the mechanism stays framed as the optimizer design and as observed search behavior, not as an isolated causal effect size.


### One Run, End to End


The Crafter furnace subgoal run is a compact example: 22 minutes, three proposer rounds, three themes, eleven candidates, and heldout furnace placement moving from` 0%` to` 17%` .


Crafter run trace


### Fan out into themes, fan back into the archive


3 proposer rounds · 3 themes · 11 candidates · heldout 0% → 17%


subgoal: place_furnace · 22 min


Mature themes


**root pickaxe transition** · make_wood_pickaxe


**stone-to-furnace finish** · place_furnace


baseline


hill climb


theme champion


consolidation


refinement


Edges = parent → child · gray edges = fold into archive · click a node for prompt + rationale.


Exemplar from the Crafter furnace subgoal run, drawn as a search graph with time running left to right: the archive seed fans out into theme hill climbs, the mature-theme winner fans in to two consolidations, refinements fan back out, and every surviving branch folds into the archive readout. Click any node for the prompt sketch and proposer rationale.


The graph should be read as a search lineage. Time runs left to right. The baseline seed fans out into theme hill-climbs; a theme champion fans in at consolidation; refinement candidates fan back out from the promoted state; surviving branches fold into the archive. Gray edges mark candidates that were useful as evidence but did not remain active after consolidation. This is the unit of analysis GELO is meant to expose: not just the best final prompt, but the route by which a local frontier became an archive item.


## Craftax


The strongest evidence so far comes from[Craftax](https://craftaxenv.github.io/) , a JAX-based benchmark designed to combine the speed of vectorized RL environments with the long dependencies of open-ended worlds. Craftax-Classic reimplements Crafter in JAX; the full Craftax environment extends the mechanics with richer dependencies and NetHack-like elements.


The first GELO pass produced a real but narrow improvement: the archive learned a route from early resource collection into basic tool use. That result was useful, but it was still an opening-route result. We therefore replayed the prior checkpoint with a higher-throughput proposer contract: maintain at least eight active themes, and explicitly allocate theme families to route preservation, stone-pickaxe conversion, coal and iron harvesting, furnace placement, iron-pickaxe conversion, diamond navigation and mining, and deep-run survival.


Two successful proposer iterations expanded the archive to 14 themes and 34 candidates, 27 of them new. Given the available budget and the current archive state, the search diversified instead of only polishing the opening route.


Craftax archive expansion


### 6 active themes to 14


34


total candidates


27


new candidates


before replay:6


target: at least 8


after replay:14


27 new proposals across two successful iterations


Opening and route · 5


- route first wood to stone
- route plus bonus preservation
- bonus survival opening
- bonus pickups and recovery
- compact route to coal


Post-stone ladder · 3


- stone-pickaxe conversion
- post-stone progression
- post-stone tool ladder


Deep resources · 5


- coal and iron harvesting
- furnace placement and use
- iron-pickaxe conversion
- diamond-visible navigation
- diamond mining extraction


Recovery · 1


- deep-run survival loop recovery


Archive state after the higher-throughput Craftax checkpoint replay. Theme names are grouped for readability; the source archive tracks the individual theme ids.


End-to-end run trace


### Theme hill climbs to consolidated candidates


local theme → consolidated prompt → readout


End-to-end trace for one Craftax Go-Explore pass. Each left node is a scoped theme hill climb; connectors show which local winners were consolidated; the right lane shows heldout or targeted readouts.


### What Improved


The strongest completed heldout evidence is now midgame behavior. The four candidate families in the trace are all prompt-optimization artifacts — no supervised finetuning or policy-gradient update was used:


Candidate family Heldout read What it reached


coal deep (` 648cfe…` ) mean` 4.93` , best` 7.10` / 6 deep seeds repeated coal; stone pickaxe on the best seed


coal branch (` 874341…` ) mean` 4.27` , best` 6.10` / 12 seeds coal on four seeds; stone pickaxe on one


iron/furnace (` 02e7cb…` ) mean` 3.93` , best` 7.10` / 12 seeds stone pickaxe, iron, and furnace on the best seed


manual frontier (` ed0a86…` ) mean` 3.97` , best` 8.10` / 8 seeds iron and furnace, with the strongest reward spike


Craftax achievement tree


### Go-Explore moves heat down the crafting ladder


Same dependency tree, before baseline versus after the Go-Explore route/deep frontier reads


view:aggregate


Aggregate over 20 seeds. Largest shifts: wood pickaxe +60pp, stone +55pp, table +50pp, stone pickaxe +33pp.


Craftax achievement tree heatmap. Early nodes use the 20-seed route-deepen heldout comparison; deeper nodes use the best Go-Explore frontier read that reached that milestone. Diamond remained cold across all 80 focused deep rollouts.


Craftax heldout frontier. Bars show mean reward for each candidate set; dots show the best rollout observed in that evaluation set. The \`8.1\` spike is real but still did not collect diamond.


The qualitative shift matters. The earlier route-following candidates showed that the policy could collect wood, place a table, make a wood pickaxe, and collect stone. The higher-throughput run pushed heldout trajectories into coal, stone-pickaxe, iron, and furnace states. “The agent sometimes reaches stone” and “the agent reaches iron and furnace, then fails to make the correct next tool” are different research problems. The latter is narrower, easier to instrument, and better shaped for the next GELO round.


### What Did Not Work Yet


We did not collect diamond. Across the focused deep probes and targeted manual probes, 80 completed heldout rollouts produced zero diamond labels.


We also attempted the longer-horizon Craftax refresh that would have upgraded this ceiling readout. It did not produce a terminal run: every viable Cx1 launch path eventually hit a container final-record boundary where a completed rollout returned` 404` from` /rollouts/<id>` before the runner could read a usable final record. That failed packet is retained as evidence for the fallback, not counted as a diamond result.


Craftax deep-resource milestones


### Where the diamond run actually stalled


modality: prompt optimization


diamond labels: 0/80


Candidate set coal stone pickaxe iron furnace diamond Reward


route heldout


f27c4ff042 / n=20


2/20


0/20


0/20


0/20


0/20


mean 4.04
best 7


coal branch


874341624c / n=12


4/12


1/12


0/12


0/12


0/12


mean 4.27
best 6.1


iron/furnace


02e7cbc3cc / n=12


1/12


4/12


1/12


1/12


0/12


mean 3.93
best 7.1


manual frontier


ed0a8650c5 / n=8


0/8


2/8


1/8


1/8


0/8


mean 3.97
best 8.1


Deep-resource achievement frequencies across the route, sequential salvage, and manual iron-pickaxe probes. This is the frontier map: coal and stone-pickaxe appear, iron and furnace are sparse, diamond is still zero.


The most informative trace was also the clearest failure. One rollout reached` make_stone_pickaxe` ,` collect_iron` , and` place_furnace` , then spent dozens of turns without further progress. In one near miss, the policy attempted` make_iron_sword` instead of entering the iron-pickaxe branch. A manual follow-up prompt removed the sword mistake and reached an` 8.1` reward spike, but it still missed coal and never completed` make_iron_pickaxe` .


That trace defines the next frontier. The next themes should target three checkpoint-local failures: avoid looping after furnace placement, fetch coal when iron is already in inventory, and force the iron-pickaxe craft when the recipe is nearly satisfied rather than drifting back into exploration. This is the intended shape of the algorithm: each archive improvement should turn a vague long-horizon failure into a small set of named subproblems.


## NetHack


To test whether the loop transfers outside crafting environments, we ran GELO on NetHack through the NetHack Learning Environment (NLE). NLE is a scalable, procedurally generated, stochastic roguelike benchmark for RL research, with long-horizon demands on exploration, planning, skill acquisition, and language-conditioned behavior.


We optimized a deliberately simple metric: **scout score** , the number of distinct` (dungeon level, x, y)` tiles the hero occupies in an episode. Scout score is not a complete measure of NetHack competence, but it is hard to game by local looping: standing still or walking back and forth over known cells adds no new points.


The protocol held the environment interface fixed. We used a` gemini-3.1-flash-lite` policy and a` gpt-5.4-mini` proposer, evaluated on fixed heldout seeds, 200-step episodes, with structured observations fixed and only the policy prompt changing. Candidate proposals were grounded in NetHack-specific behavior — corridor-first exploration, quadrant sweeps, frontier navigation, glyph awareness, and staircase reasoning — rather than leaking Craftax recipes into a new domain.


Go-Explore's discovered language policy improved held-out tile coverage from 29.4 to 37.0 mean distinct tiles (+26%) when structured observations were held fixed, and reached 48.8 at a longer 350-step budget.


NetHack scout replicates


### Coverage lift is smaller once replicated


60


paired rollouts


+3.7


mean lift


band seeds baseline champion lift DL2 seeds status


N3.1 200 steps


151-197 27.3 33.7 +6.4 0/0 positive


N3.2 200 steps


199-257 32.2 38.7 +6.5 0/0 positive


N3.3 200 steps


263-313 37.1 35.2 -1.9 1/1 negative


Replicate 3 is negative on scout coverage despite one champion seed reaching dungeon level 2; report the band, not only the mean uplift.


Three fresh paired scout bands keep the NetHack coverage result positive on average, but not monotonic: two bands improved and one band regressed.


NetHack follow-up


### Progression moved; descent did not


367


rollouts


0


descent hits


run rollouts checkpoints reward lift frontier readout status


N1 progression search nle_rust_20260611002142


300 259 +0.015 119 monster encounters, 120 food pickups progression


N2 descent push descent_nle_rust_20260611004350


67 219 +0.026 74 descent checkpoints, 4/4 theme budget descent open


N2 is a targeted descent-push negative frontier: the first-descent theme activated and saturated, but no rollout reached first_descent or dungeon level 3.


Follow-up NetHack runs moved progression/search reward, but the explicit descent push stayed a frontier: the first-descent theme saturated without any rollout reaching \`first_descent\` or dungeon level 3.


On a single heldout seed, the occupancy maps below show every tile the hero stood on during the episode, before and after optimization, in the same coordinate frame:


seed


Occupancy footprint on dungeon level 1, in a shared per-seed coordinate frame. Each square is a map cell; colour is how many times the hero stood on it. Use the buttons to scroll every held-out seed. Each panel is one representative (median-scout) rollout out of 3 per arm; the optimization’s across-seed mean is 29.4 →37 tiles over 30 rollouts per arm. Read directly from container rollouts.


Distribution shift in distinct cells visited. The headline mean uses all 30 rollouts per arm; the box plots use the representative seed-median rollout stored for each held-out seed in the heatmap artifact. Every rollout remained on dungeon level 1, so deeper levels received zero visited-cell mass in this packet.


Coverage also increased with a longer episode budget. At a 350-step budget, the same policy reached a` 48.8` mean scout score, with the best seeds exceeding 70–80 distinct tiles.


The intervention is a behavioral reallocation, not a uniform improvement. Exploration and discovery achievements rise, while combat, inventory, and score milestones fall. That is the correct tradeoff for this objective: the optimizer pays for new ground covered, not for fighting or looting.


Mean achievements unlocked per held-out rollout, grouped into categories, before (baseline) and after (Go-Explore). The optimization reallocates behavior toward exploration and discovery and away from combat and scoring — it is a behavioral shift, not uniformly “more.” Max dungeon level was 1 for every rollout in both arms: descent never fired, so that distribution is unchanged.


The main limitation is still structural. Almost all coverage remains on dungeon level 1. Descending requires finding a` >` staircase and executing the transition reliably; prompts that simply rush for stairs scored below baseline in these budgets. The two interventions that did move the metric were structured context — position, visited cells, blocked directions — and survival. Early death remains the largest source of lost coverage. Multi-level descent is the next NetHack frontier, in the same way that diamond is the next Craftax frontier.


## Crafter


Our first Crafter evaluation used mean reward. We replaced it because the aggregate was too coarse to support a strong claim. A mean-reward curve can improve while hiding which rung of the crafting tree moved, and it can punish useful local progress that does not survive to the end of the episode.


The rebuilt evaluation uses five binary **subgoal sub-environments** along the crafting tech tree: wood pickaxe, stone pickaxe, furnace, iron pickaxe, and diamond. Each environment asks one question: did the agent unlock the target achievement? GELO runs separately on each subgoal, in parallel, with reward shaping aligned to that target.


The baseline reliably reaches the first rung — a wood pickaxe on` 70%` of heldout seeds — and then stops:` 0%` stone pickaxes, furnaces, iron pickaxes, and diamonds. That clean wall is what makes the subgoal framing useful.


Each of the five tech-tree milestones is optimized as its own Go-Explore sub-environment (3 proposer rounds + consolidation, 11–12 candidates and 2–3 themes per run), then its dedicated champion is scored on that subgoal across30 held-out seeds. Go-Explore lifts the two reachable rungs — wood pickaxe and furnace, which the baseline never places — and then hits a hard policy-capability ceiling: stone pickaxe barely moves and iron pickaxe / diamond stay at zero, no matter how much search we add. The wall is the weakgemini-3.1-flash-lite policy, not the optimizer.


Per-subgoal optimization produces specialists. The furnace champion is the only policy that places a furnace, moving the heldout rate from` 0%` to` 17%` ; the wood-pickaxe champion moves the first rung from` 70%` to` 83%` .


The deeper result is a negative result. Increasing the deeper sub-environments to three proposer rounds, 10–12 candidates, and three themes each did not solve the underlying policy limitation. Stone-pickaxe achievement moved only to` 3%` ; iron pickaxe and diamond remained at` 0%` . Prompt search can improve the behavior the policy is capable of expressing, but it cannot reliably induce a weak policy to execute a precise “stand at the table holding stone and craft” sequence when the base model does not have that competence. GELO exposes that ceiling rather than hiding it behind an average reward.


### A Compute-Scaling Probe


After the subgoal guardrail, we ran three independent Crafter GELO jobs in parallel using DeepSeek Flash v4 for the proposer, data-miner, verifier, and terminator lanes. This is not a scaling law. The narrower hypothesis was operational: can the same inexpensive-model configuration run long enough, across three independent jobs, to produce archive promotions and heldout movement?


All three jobs completed, none crashed, and every job promoted at least two candidates.


Crafter scaling probe


### Three DeepSeek Flash v4 runs all reached promotion


modality: prompt optimization


3/3


succeeded


7


promotions


0


crashes


Heldout lift, champion


Run A


+0.550


Run B


+1.350


Run C


+0.600


Theme lift


Run A


+3.667


Run B


+3.300


Run C


+3.500


Run rounds promotions themes TC / CC wall rollouts


Run A


goex_f1fd…


8 3 6 19 /13 28m26s 230


Run B


goex_e31a…


8 2 7 12 /21 30m55s 230


Run C


goex_e28b…


8 2 5 16 /17 32m42s 320


Scoreboard: scoreboard_20260611_001121


. TC / CC = theme / consolidation candidates. Compute-scaling probe only; three runs are not a scaling law.


Crafter DeepSeek Flash v4 compute-scaling probe: three independent parallel runs, all terminal succeeded, all produced promotions. Treat this as a probe, not a scaling law.


The run-to-run spread is informative. One run kept only two themes but produced four promotions; another kept six themes and produced the strongest theme lift. In this development probe, the archive histories fan out in different places while the heldout readout moves in the same direction.


The caveat is equally important: the logs include skipped data-miner and consolidation substeps caused by JSON/schema failures. This is a successful development probe, not evidence that the scaling stack is solved.


## Exploring Posttraining Data


GELO is useful not only because it finds better prompts, but because it changes the shape of the data we can train on.


A conventional rollout dataset is dominated by prefixes. In a long-horizon environment, many examples show the policy collecting wood, walking corridors, or surviving the opening because those states are easy to revisit from reset. The rare states we care about — holding iron next to a furnace, standing near a staircase, recovering after a route deviation — appear sparsely, if at all. Training on that distribution gives the model many copies of the opening and very little pressure on the frontier.


GELO produces a different dataset: **frontier-conditioned experience** . For each archived checkpoint, we can collect positive and negative attempts against the same local objective; label whether the policy returned to the checkpoint distribution; separate route failures from skill failures; and replay promoted candidates on heldout seeds. This turns a raw episode trace into supervised and reinforcement-learning objects with cleaner credit assignment.


Concretely, the archive yields several posttraining views:


- **Return data:** trajectories that recover the archived state from earlier checkpoints.
- **Local skill data:** attempts from the checkpoint to the next achievement, including near misses.
- **Contrastive failures:** candidate pairs that share the same prefix but differ at the frontier.
- **Verifier data:** success and failure labels tied to explicit achievements, not just scalar episode reward.
- **Curriculum data:** a lineage of promotions that orders frontier skills by when the current policy could make use of them.


This is why we view GELO as an optimizer and a data engine. Prompt optimization is the first consumer because it gives immediate feedback and fast iteration. The beta behavioral-cloning lane can use promoted traces and corrected near misses. Future RLVR and policy-gradient variants can train against scoped objectives rather than full-episode sparse rewards. On-policy self-distillation can use the archive to sample states where the current policy is plausibly one or two decisions away from a new capability.


The important constraint is heldout replay. A checkpoint-local improvement is only useful if it survives beyond the exact seed and trace that discovered it. GELO therefore treats consolidation as part of the algorithm, not as a posthoc charting step.


For the June 26 hosted relaunch, we also ran the new public GameBench Rust smoke presets end to end. These are integration receipts, not new benchmark rows: the Craftax smoke run completed with integrity passing,` 12` total rollouts,` 108` training checkpoints, heldout mean` 1.0` , and no promotion because the seed prompt was already the champion; the Rogue smoke run completed with integrity passing,` 12` total rollouts,` 108` training checkpoints, heldout mean` 0.0` , and no promotion. The useful result is operational: both presets now exercise the Rust gold service, tunneled ReAct-style task container, prompt-only candidate contract, isolated heldout replay, and terminal GELO artifact packet.


**Run hosted GELO on your task.** Install the public package, point it at a GELO-compatible task container, and submit a hosted job. Use the public[GELO skill](https://github.com/synth-laboratories/optimizers/tree/main/skills/gelo) as the container-authoring runbook and the[hosted optimizer docs](https://docs.usesynth.ai/sdk/hosted-optimizers#gelo-launch-promo) for promo terms.


[Start hosted GELO](https://www.usesynth.ai/signup?product=optimizers&utm_source=blog&utm_campaign=gelo_free_72h_20260626&utm_content=gelo_blog_bottom_signup)[Get GELO skill](https://github.com/synth-laboratories/optimizers/tree/main/skills/gelo)


```text
pip install --upgrade --pre synth-optimizers==0.2.6.dev20260626
export SYNTH_API_KEY=...
synth-optimizers gelo startup


# Public container URL
synth-optimizers gelo submit \
--preset crafter_smoke \
--container-url https://YOUR-CONTAINER.example.com \
--follow


# Local container through Synth Tunnel
synth-optimizers gelo submit \
--preset craftax_gamebench_rust_smoke \
--tunnel-url http://127.0.0.1:8943 \
--tunnel-provider synth_tunnel \
--follow


synth-optimizers gelo watch goex_... --slice board --goex-events
```


Usage registration is privacy-preserving by default: GELO/GEPA, package version, model/provider names, anonymous install id, and a server-side IP hash. It does not send prompts, completions, code, artifacts, container URLs, or raw IPs. Opt out with` SYNTH_OPTIMIZERS_DISABLE_USAGE_REGISTRATION=1` or` \[usage_registration\] enabled = false` .


### Weight-update lanes: RLVR→OPSD (updated 2026-07-06)


The archive is not restricted to prompt candidates. GELO's typed plugin graph also runs weight-update lanes, and the coupled RLVR→OPSD lane has now completed its first full proof cycle on Crafter. The shape mirrors the prompt loop exactly: theme hill-climb becomes RLVR/GRPO — one expert trained per non-unity theme objective, starting from checkpoint-resumed partial objectives — and consolidation becomes on-policy self-distillation (OPSD), routing student rollouts to the theme teachers and accepting a distilled checkpoint only if the theme gate and the retention gate both pass. No privileged teacher signal is required; the teachers are themselves learned by RLVR on the theme objectives the archive exposed.


The Crafter cycle (local runs, single proof cycle, n as stated per row):


Stage Metric Before After Delta


Plant RLVR teacher (n=128) theme goal success 16.4% 37.5% **+21.1pp**


Table RLVR teacher (n=64) theme goal success 26.6% 35.9% **+9.4pp**


OPSD consolidation, accepted checkpoint (n=128) theme goal success — — **+15.6pp**


OPSD consolidation, accepted checkpoint (n=128) retention mean reward 1.14 1.33 **+0.19**


The consolidation detail is the point. The selector rejected the final training step and accepted the intermediate checkpoint where both theme objectives improved and retention improved — plant 7.8%→16.4%, table 29.7%→36.7%, standard mean reward up, no retention drop. This is the promotion rule from the prompt loop applied to weights: a distilled student is only archived when replay evidence shows it kept what it had while gaining the frontier skill. A Craftax replication of this recipe is in progress and will land in a follow-up post on compute-scaled RL recipes.


Hosted access remains invite-only: SFT is a beta lane for allowlisted organizations, and the RLVR→OPSD lane expands from the waitlist. Request access through the[hosted optimizer docs](https://docs.usesynth.ai/sdk/hosted-optimizers#gelo-plugin-lanes) and mention "GELO plugin lanes" in the request.


**72-hour hosted GELO promo.** Eligible organizations can claim a June 26 hosted GELO grant for **72 hours of GPT proposer spend** . Use the hosted API or the` synth-optimizers` CLI; credits apply to hosted GELO runs configured with GPT-family proposer roles, while task-container policy LLM calls remain owned by the container.


[Read the quickstart](https://docs.usesynth.ai/sdk/hosted-optimizers#gelo-launch-promo)


·[Crafter + GELO cookbook](https://github.com/synth-laboratories/cookbooks/tree/main/code/training/prompt_learning/gelo) ·` npx skills add synth-laboratories/optimizers/skills/gelo`


## Resources


- [Scaling Train Time Compute for Gepa](https://www.usesynth.ai/blog/scaling-train-time-compute-for-gepa) — Synth's prior optimizer release and implementation context.
- [GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning](https://arxiv.org/abs/2507.19457) — reflective prompt evolution with Pareto-based candidate selection.
- [Optimizing Instructions and Demonstrations for Multi-Stage Language Model Programs](https://arxiv.org/abs/2406.11695) — MIPRO / MIPROv2 background for prompt optimization in LM programs.
- [Reinforced Self-Training (ReST) for Language Modeling](https://arxiv.org/abs/2308.08998) — offline self-training baseline for language-model policy improvement.
- [Go-Explore: a New Approach for Hard-Exploration Problems](https://arxiv.org/abs/1901.10995) — original Go-Explore preprint.
- [First return, then explore](https://arxiv.org/abs/2004.12919) — Nature version of Go-Explore and the detachment / derailment framing.
- [The Craftax Benchmark](https://craftaxenv.github.io/) and[Craftax: A Lightning-Fast Benchmark for Open-Ended Reinforcement Learning](https://arxiv.org/abs/2402.16801) — Craftax and Craftax-Classic.
- [The NetHack Learning Environment](https://arxiv.org/abs/2006.13760) — NLE benchmark.
- [Benchmarking the Spectrum of Agent Capabilities](https://arxiv.org/abs/2109.06780) — Crafter benchmark.
- [The Swarm](https://drubinstein.github.io/pokerl/docs/chapter-3/swarm/) — Go-Explore-inspired swarming in PokerL / Pokemon RL.
- [Agent Learning via Early Experience](https://arxiv.org/abs/2510.08558) — training agents from their own interaction data rather than only expert demonstrations or scalar reward.
- [Continual Harness: Online Adaptation for Self-Improving Foundation Agents](https://sethkarten.ai/continual-harness/) — reset-free harness refinement during continuous play.
- [GELO skill](https://github.com/synth-laboratories/optimizers/tree/main/skills/gelo) — public runbook for authoring GELO-compatible task containers and preparing hosted optimizer submissions.
- [Crafter + GELO cookbook](https://github.com/synth-laboratories/cookbooks/tree/main/code/training/prompt_learning/gelo) — public walkthrough for a SynthTunnel-compatible hosted GELO task target.
- Evidence provenance — per-chart source identifiers and SHA256 hashes are embedded in the figure footers; raw packet artifacts are retained in Synth's internal launch evidence bundle.


## Citation


If you use GELO, the archive-based optimization algorithm, or the concept in academic work, please cite:


bibtex


```text
@misc  {  purtell2026gelo  ,
title   =   {  GELO: Archive-Based Optimization for Long-Horizon Agents  }  ,
author   =   {  Purtell, Josh  }  ,
year   =   {  2026  }  ,
howpublished   =   {  \url{https://www.usesynth.ai/blog/go-explore}  }  ,
note   =   {  Synth Blog  }
}
```
