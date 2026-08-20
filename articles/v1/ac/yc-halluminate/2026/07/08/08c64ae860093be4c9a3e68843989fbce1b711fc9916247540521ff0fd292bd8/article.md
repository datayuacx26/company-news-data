---
schema_version: "1.0.0"
document_id: "08c64ae860093be4c9a3e68843989fbce1b711fc9916247540521ff0fd292bd8"
company_key: "yc-halluminate"
company: "Halluminate"
source_id: "yc-halluminate-news-import-9819d7d93bdb"
canonical_url: "https://www.halluminate.ai/blog/sim-to-real-webagents"
published_at: null
first_seen_at: "2026-07-27T22:48:47.303766+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:a00673acd95e4f79d1536c4a771698237db40100165c6d27169657a33a3c4599"
---

# Simulated-to-Real Transfer for Web Agents: A small scale study

In this post, we study transfer for web-navigation agents, in both directions: we take a model trained on one site, simulated or real, and measure how well it does on the other. We train Qwen2.5-VL-7B, a small open vision-language model, under three conditions: only simulated tasks (Goodbuy, a Best Buy clone), only real tasks (Best Buy), or a 50/50 mix. This is deliberately a small, controlled experiment: one website pair, one model, and the same training recipe in every condition. We report not only the final findings but also the journey to get there, including the reward designs we tried along the way (binary vs. shaped rewards, and an entropy bonus for exploration).


Web agents are AI agents that navigate the web to complete tasks for you: buying groceries, booking flights, ordering clothes. These tasks are long, involve many steps, and the agent must be safe. Nobody wants unwanted purchases from a sloppy agent. Training on real websites carries real risks: the live web is adversarial to agents, the infrastructure is opaque, and an action taken by an agent can have real world effects. Simulated environments remove risks and improve stability by mocking effects and letting researchers have full transparency and control of the environment infrastructure. That is what[Westworld](https://www.halluminate.ai/blog/westworld) is: Halluminate's benchmark of five simulated clones of popular real websites, including Goodbuy, a Best Buy-style electronics store and the star of this post. These simulators are not just for evaluation. Yutori trained its[Navigator agent](https://yutori.com/blog/introducing-navigator) partly on Westworld and never let it practice on the real Google Flights, yet what it learned on the simulated version worked on the real site.


Figure 1 shows the main result. Each training condition is evaluated on both task sets, simulated and real. Two things jump out. Transfer is asymmetric: what the model learns on the real site carries over to the sim far better than the other way around. Moreover, the best of the three is not a specialist at all: **training on both real and simulated websites at once wins at both.**


*Figure 1: eval success rate on Goodbuy (simulated) and Best Buy (real) tasks for each training condition. Real-to-sim transfers far better than sim-to-real, and mixed training wins on both sites.*


## Two Stores, One Task List


### What Westworld Ships


The original 20 Goodbuy tasks ship with[Westworld](https://github.com/Halluminate/westworld) . They come in three types: checkout, delivery instructions, and store pickup. Every task starts from the same place: a cart pre-populated with two items.


A sample checkout task looks like this:


> Purchase the items in the cart and deliver them to Violet Ritchie. Use Violet Ritchie's payment info. Set the delivery instruction dropoff location to front door and property type to apartment. Your job is done when the order is successfully placed. Use guest checkout. User info: {user_info}.


### Cloning the Tasks onto the Real Store


We wanted each Best Buy task to be as close as possible to its Goodbuy twin: same recipient, same address, same card, same goal. The starting state is mirrored too. Best Buy tasks begin from a pre-populated cart with the closest real-catalog equivalents of the sim's two items (the sim's exact LEGO set was no longer sold, so we used the LEGO Botanicals Mini Orchid 10343 alongside the Nacon Revolution 5 Pro controller). The two worlds, side by side:


*The two worlds, side by side. Left: Goodbuy (simulated), where every task starts from this cart. Right: Best Buy (real), the same cart mirrored on the live site.*


One change was non-negotiable: on the real site the agent must never place an order. So the Best Buy instructions stop at the point of purchase. The mirror of the task above reads:


> Purchase the items in the cart and deliver them to Violet Ritchie. Use guest checkout. Fill in the payment information using the provided credit card details, but do NOT place the order. User info: {user_info}.


To keep the comparison fair, both sites are scored with the same success criterion, which we call the matched criterion: the agent reached the payment page with the recipient's name and address on the page and the card details filled in. This means the Goodbuy reward does not test that the order was actually placed, even though the simulator could verify it server-side. We deliberately gave that check up: requiring the sim's extra place-order click would make every cross-site comparison unfair to the sim-trained side. The order-placed check is still logged as a diagnostic, but it is never rewarded. Still, because the verifier only inspects the final page state, an agent that does place the order lands on a confirmation page where the payment-field checks no longer match and fails anyway: not placing the order is enforced in effect, not by an explicit rule. One asymmetry remains in the wording: the original Goodbuy instructions still tell the agent to place the order, while the Best Buy instructions explicitly say not to. The reward is identical on both sites regardless of the wording.


One practical fix was needed for the pickup tasks. The original task addresses (Racine WI, New York NY, Green Bay WI) turned out to have no stores with both items available for pickup, which made those tasks impossible. We moved all pickup tasks to San Francisco addresses (zip codes 94103 to 94118), where one store stocks both items.


### From 20 Tasks to 320


Twenty tasks per site is far too small a pool for RL: the model can only memorize it. So we grew it to 320 tasks per site by generating variations of the originals, always in matched Goodbuy/Best Buy pairs.


**How the augmentation works**


- We generated 300 new tasks per site (100 per task type) on top of the 20 originals. Each Best Buy task is a 1:1 mirror of a Goodbuy task, built from the same values, so transfer is still measured on matched pairs.
- The generator varies every field the agent types into a form: name, shipping address, email, phone number, credit card number, delivery dropoff location, property type, delivery notes, and pickup method (in-store vs. curbside).
- It also rotates between five phrasing templates per task type, so the model cannot memorize a single sentence pattern.
- One thing we did not vary is the starting cart. The two items are hardcoded in the Westworld site code, with no way to seed them from outside, so changing them would mean modifying the simulator itself.
- The honest consequence: the 300 new tasks are parameter variants of 3 flows, not 300 new flows.


## One Harness for Both Worlds


The agent interacts with both websites through the same harness. This guarantees symmetry: the training data source is the only thing that changes between conditions. At each step, the observation the agent receives consists of four things:


1. The task instruction.
2. A screenshot of the page (1280x720).
3. A list of the page's interactive elements.
4. Its recent history: the previous actions taken, whether each one executed successfully, and the reasoning from its last two steps.


The list of interactive elements is extracted from the browser's accessibility tree. Each element is tagged with a BID (Browser ID), a sequential integer injected into the DOM (Document Object Model). Each line gives the BID, the element's role, and its label:


```text
[8] textbox 'First Name'
[13] combobox 'State' options=[AL, AK, AZ, ...]
[22] button 'Continue to Payment'


```


The agent responds with its reasoning inside a` <think>` block, followed by a JSON action inside an` <answer>` block:


```text
<think>The shipping form is open. I should fill in the first name.</think>
<answer>{"action": "fill", "bid": 8, "value": "Violet"}</answer>


```


There are six possible actions:


1. ` click` : click an element by BID.
2. ` fill` : type a value into an element by BID.
3. ` select` : choose an option from a dropdown by BID.
4. ` scroll` : scroll the page up or down.
5. ` press` : press a keyboard key, such as Enter.
6. ` complete` : declare the task finished.


**Technical Specs for the Harness**


- Browser: async Playwright driving headless Chromium, with one isolated browser context per episode, so no cookies or state carry over between tasks.
- Element extraction: Goodbuy returns an empty accessibility tree, so a JavaScript fallback extracts up to 500 interactive elements and formats them in the same BID format. The 500 cap exists because Best Buy's filter-heavy pages carry over 150 checkboxes alone.
- Action validation: Playwright only reports hard errors, so the harness also hashes the interactive elements before and after each action. If nothing changed, the action is marked as failed even though it executed, and the agent sees that in its history.
- A short settle wait follows every action, giving the page's async renders time to finish before the next observation is captured.
- Best Buy plumbing: the cart is pre-populated through Best Buy's add-to-cart API before each episode, and the browser runs with fingerprint patching and geolocation permissions, which the store picker widget requires.
- Verifier mechanism: success is decided by deterministic JavaScript inspection of the final page state, checking the matched criterion described in the training section. No LLM judge: evaluation is instant, free, and returns the same verdict for the same page every time.


## Imitate, Then Explore: SFT and GRPO


Training has two phases: SFT followed by RL, specifically Group Relative Policy Optimization (GRPO).


For the SFT phase we used Claude Fable as a teacher. We ran Fable on all 640 tasks (320 per site) and it completed 505 of them successfully. We kept only the successful trajectories from the training split and discarded the rest. Each trajectory is a sequence of steps, and each step contains what the agent saw (the screenshot, the task instruction, the list of interactive elements, and the action history) plus what the teacher did (its reasoning and its JSON action). We then fine-tuned the base model with Quantized Low-Rank Adaptation (QLoRA) to predict the teacher's response tokens given each observation. In other words, SFT teaches the base model to imitate the teacher, step by step, on successful runs only.


After SFT comes GRPO. In each iteration we sample a batch of tasks and generate 8 rollouts per task, called a group. Every rollout in a group is scored by the reward function, and each rollout's advantage is computed relative to its own group's average. Rollouts that score above the group average are reinforced and rollouts below it are penalized. Our reward is a binary pass or fail per rollout, using the matched criterion: the agent reached the payment page, the recipient's name and street address appear on the page, the card details are filled in. Pickup tasks additionally require the chosen pickup method to be visible.


To keep the comparison clean, all three conditions (Goodbuy only, Best Buy only, and a 50/50 mix) use the exact same recipe: the same SFT procedure, the same GRPO hyperparameters, and the same reward. The only thing that changes between conditions is which site's tasks the model trains on (and, as a result, how many trajectories Fable produced).


**Technical Specs for the Curious Reader**


**SFT**


- Base model: Qwen2.5-VL-7B-Instruct, fine-tuned with QLoRA (4-bit NF4 quantized base, LoRA rank 64, alpha 128, applied to all attention and MLP layers).
- Data: success-only teacher trajectories from the training split, formatted as one example per step (observation in, teacher reasoning + action out). For the Best Buy condition this is 208 trajectories, about 2,900 examples; for Goodbuy it is about 195 trajectories but roughly 4,800 examples, since Goodbuy's checkout runs longer per task.
- Optimization: learning rate 5e-5, cosine schedule with 5% warmup, effective batch size 2, max sequence length 4096, gradient checkpointing on.
- Checkpoint selection: by validation loss (10% held-out split, fixed seed). One epoch is a single full pass over the SFT training examples above (about 2,900 for Best Buy); all three conditions reached their validation minimum in under one epoch.
- Hardware: one A100 80GB per condition, on Azure ML.


**GRPO**


- Group-relative policy optimization: 8 rollouts per task per group, 8 tasks per iteration, 10 iterations, one training pass per batch. Each condition starts from its own SFT adapter and trains on its own 257-task training split (about 80% of the 320, with the other 63 held out for evaluation).
- Reward: binary pass or fail per rollout, from the deterministic verifier described above.
- KL penalty (coefficient 0.03) against the frozen SFT adapter; clipped surrogate objective (clip 0.2) with importance ratios computed against generation-time log-probs.
- Sampling at temperature 1.0 and top-p 1.0, so the sampling distribution matches the model distribution and the importance ratios stay valid.
- Episodes capped at 60 steps; rollouts run in concurrent browser sessions (6 on Goodbuy, 8 on Best Buy).
- A LoRA checkpoint is saved after every iteration. One A100 80GB per condition; an iteration takes roughly 4 to 7 hours, about 3 days per run.


We chose the best GRPO adapter per condition by success on held-out tasks, not by the training curve. Figure 2 shows why. The left panel plots the training success rate per iteration: the fraction of successful rollouts among the episodes generated during that iteration (8 tasks drawn from the training split, 8 rollouts each). This number is noisy and hard to interpret: each round draws a different handful of tasks, and success is measured on the stochastic rollouts the trainer happens to generate, not on a fixed eval. The right panel plots the same checkpoints evaluated on the fixed held-out test tasks, which the models never trained on.


The two panels disagree, and the right one is the one to trust. The training curve slides downward over the run and looks like a slow collapse. The held-out curve shows no such decline; for the Goodbuy-trained condition it actually rises. The policy is not degrading. For each condition, we select the checkpoint with the best held-out success rate.


*Figure 2: training success rate per GRPO round (left) and success on held-out tasks for the checkpoints at rounds 2, 5, and 9 (right). The training curve slides downward, but held-out performance does not degrade; checkpoint selection uses the held-out curve.*


**Wall-clock training time across the three runs**


Training in the simulator was not cheaper. As Figure 3 shows, the Goodbuy (simulated) run took about 35% more training time than the Best Buy (real) and mixed runs, which finished about even, even though all three ran on the same hardware. To understand the gap, we compared the runs along several dimensions. The simulated and real tasks ran for about the same number of steps, and the simulator's pages were not larger for the model to read; in fact, the real pages were larger. Moreover, the real site spent more time setting up each task than the simulator did. As a result, we did not identify a single cause for the 35% gap. Even so, tracking these training times matters, because they help us build better and more efficient environments.


*Figure 3: cumulative training time per condition. The simulated (Goodbuy) run ends about 35% higher; the real (Best Buy) and mixed runs finish about even. Task length, page size, and setup cost did not account for the gap.*


## Detours and Dead Ends


Before committing to the full training runs, we prototyped reward designs on a small testbed: the original 20 tasks per site. Small pools are cheap to iterate on, and they stress-test a reward quickly. We tried three reward designs.


1. A shaped reward that gave partial credit for progress, such as filling some of the checkout fields. It was hacked immediately: the model learned to collect the partial credit and quit early, never finishing a task.
2. A pure binary reward. It starved instead.
3. An entropy term meant to keep the policy from drifting into repetitive behavior. It only delayed the decline.


Along the way we also ruled out the boring explanations, like Best Buy's bot detection silently blocking our rollouts (it never flagged us, even with many concurrent sessions). In the end we decided to go first with a plain binary reward on the big set, without the entropy term, since the policy collapse was probably due to the smallness of the training set rather than the reward itself. That configuration, the augmented task pool and a binary reward, is what everything in this post is built on.


## Who Transfers, and What Mixing Buys


In Figure 4 we show the pass@1 success rate on the 320 tasks per site, simulated (Goodbuy) and real (Best Buy), for the three training conditions and the untrained baseline. Three things stand out:


1. The untrained Qwen baseline solves none of the 640 tasks, so every point of success is attributable to training.
2. Transfer is asymmetric. The Goodbuy-trained model is the stronger one on its own site, but almost nothing it learned carries over to the real Best Buy. The Best Buy-trained model keeps a good share of its capability when moved to the sim.
3. The main result: training on both sites at once beats specializing. The mixed model scores highest on Goodbuy and on Best Buy alike.


GRPO adds on top of SFT unevenly across conditions. The largest SFT to GRPO improvement appears in the mixed condition on the Best Buy tasks, which climbs from 7.2% to 23.4% (+16.2 points).


*Figure 4: pass@1 success rate on 320 tasks per site, by training condition. Light bars are Goodbuy tasks, dark bars are Best Buy tasks; the overlay marks the best GRPO checkpoint.*


We can see in Figure 4 that the clearest case where the SFT and GRPO error bars are separated is the mixed-training condition, where the improvement from SFT to GRPO is noticeable.


**Technical discussion for the curious reader**


To check where reinforcement learning genuinely helps, we computed a McNemar p-value for the SFT-vs-GRPO comparison within each condition. The mixed condition shows the clearest improvement (p = 0.0001 on simulated tasks and p < 0.0001 on real tasks), and Best Buy on real is significant but only barely (p ≈ 0.002). Every other condition's p-value is far larger than 0.0001 (for example, Goodbuy on simulated tasks gives p ≈ 0.20), so those SFT-to-GRPO changes are not significant.


We then ran the same test between the trained adapters themselves, on the same tasks (Table 1).


Eval site Comparison (A vs B) Success (A / B) Only A solved Only B solved *p* (exact)


Simulated Mixed vs Goodbuy 39.1% / 25.0% 98 53 0.0003


Simulated Mixed vs Best Buy 39.1% / 5.0% 118 9 < 0.0001


Simulated Goodbuy vs Best Buy 25.0% / 5.0% 74 10 < 0.0001


Real Mixed vs Best Buy 23.4% / 15.3% 61 35 0.010


Real Mixed vs Goodbuy 23.4% / 0.6% 74 1 < 0.0001


Real Best Buy vs Goodbuy 15.3% / 0.6% 49 2 < 0.0001


*Table 1: between-condition significance from an exact two-sided McNemar test on the shared 320 tasks per site (best GRPO adapter), where the better-performing condition is always listed first and "Only A / Only B solved" are the discordant task counts the test uses.*


In Table 1 we report the McNemar p-value for each pair of adapters evaluated on the same tasks (simulated or real). On the simulated tasks all three comparisons are significant: the mixed adapter is better than both Goodbuy and Best Buy, and Goodbuy is better than Best Buy, so the full ordering (mixed > Goodbuy > Best Buy) holds. On the real tasks, the mixed and Best Buy adapters are each clearly better than Goodbuy; the mixed adapter also edges out Best Buy, but this is the one borderline result (p = 0.010): significant at the 0.05 level, yet it does not survive a strict multiple-comparison correction, so we cannot claim it as confidently. We cross-checked these conclusions with Wilson 95% confidence intervals (the error bars in Figures 1 and 4) and a Holm–Bonferroni correction for the six comparisons; every conclusion holds except that borderline mixed-vs-Best Buy comparison on the real tasks.


## Does Training on One Website Transfer to Another?


We put the models through a harder test: we ran our best RL-trained adapters, and the untrained base model, on 80 tasks from the other four Westworld domains, 20 tasks each. Those domains are Noodle Flights (a clone of Google Flights), Travelpedia (a clone of Expedia), Azora (a clone of Amazon), and Megamart (a clone of Walmart). Almost nothing worked. Across every model, all pass@1 runs failed but two:


- The real-trained (Best Buy) adapter solved a single flight-search task in Noodle Flights.
- The mixed adapter solved one basic-checkout task in Megamart. That lone win was a checkout flow, exactly the skill it was trained on.


This suggests that a single small (7B) model trained on one website transfers poorly to unrelated domains. That is not the same as saying a model this size cannot handle many websites at once: UI-TARS trains a 7B GUI agent across a broad pool of sites and reports strong cross-site results ([UI-TARS](https://arxiv.org/abs/2501.12326) ). The lesson is narrower: to get transfer you need a much more diverse set of training websites. **Coverage comes from a diverse training mix, not from generalizing off a single site.**


## Takeaways


We already knew what simulated environments are good for: no risk of accidentally buying something, no bot detection to fight, and the same task replays the same way every time. What we did not know is how training in one compares to training on the real site, so we ran the comparison as controlled as we could make it: one website pair, one model, and the same recipe in every condition. The results were not symmetric. What the model learned on the real Best Buy carried over to the sim; what it learned on the sim barely reached the real site, at least for these shopping tasks. Furthermore, the winner was not a specialist at all: training on a mix of both sites beat training on either one alone, probably because the added diversity teaches the flow instead of one site's quirks. Simulators earn a place in the training mix.


## Authors


Victoria Knapp Perez


## Citation


If you find this work useful in your research, please cite it:


```text
@misc{simtoreal2026,
title={Simulated-to-Real Transfer for Web Agents: A small scale study},
author={Victoria Knapp Perez},
year={2026},
url={https://halluminate.ai/blog/sim-to-real-webagents}
}


```
