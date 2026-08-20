---
schema_version: "1.0.0"
document_id: "29dbef02ce361d7510df85d373fb3c14949e2fdc3bfce38f289fd6f79331971c"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/fable-5-the-next-shape-of-app-building"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-21T18:00:00.368523+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:52bfc84ca8f5d09a10bf807b338e5ef3c0da0f3b44ed5a914ce55db2b360c47e"
---

# Fable 5: The Next Shape of App Building

Fable 5 became the rare model release that left the AI world both excited and unsettled. It went live on Emergent on June 9, drew intense attention for its long-horizon coding ability, and then, on June 12, was caught in a US export-control directive that briefly suspended access and left teams unsure what would happen next. With those controls now lifted and Fable returning, the useful question is what it actually changed inside real Emergent builds.


Update: the June 12 export-control directive that suspended access to Fable 5 and Mythos 5 has been lifted. Anthropic began redeploying Fable 5 globally on July 1 with updated safeguards. The analysis below reflects the first Emergent usage window before the interruption. See[Anthropic's update](https://www.anthropic.com/news/redeploying-fable-5) .


This post looks at that signal across controlled evals and the first real usage on Emergent: where Fable 5 helped enough to justify choosing it over Opus 4.8, and what that tells us about building better agent systems.


‍


First signal


## **The real question**


Fable 5 is Anthropic's first model from the Mythos class, which sits above Opus in their lineup. We wanted to know where that extra capability shows up in real product work, so we carried one question through the analysis: does the model reduce the user's effective next step?


The public reaction has centered on long-horizon work: whether Fable can stay coherent across a messy build, keep track of prior decisions, and make better calls when the next mistake is expensive. On the same very large task, Sonnet may complete roughly half, Opus can get much closer, and Fable is the one that most often approaches the full problem statement. It keeps carrying context, checking its own work, and pushing toward completion. That persistence is where the real difference starts. The point is not that every task needs Fable. Its advantage grows with task length, ambiguity, and the cost of retrying. For narrower work, Opus still gives most teams the cleaner default balance of quality, speed, and cost.


‍


PRE-RELEASE


## **The long-horizon test**


We evaluated Fable 5 on Emergent's model harness: a representative dataset of real app briefs designed to stress planning, implementation, repair, and cost. The clearest signal came from long-horizon builds, where the agent has to keep the product shape intact as context grows.


That slice gave us the first concrete tradeoff:


- Fable delivered 45 features across the set against Opus 4.8's 41, and used fewer steps doing it (330 vs 368 on average).
- The uncapped Fable run cost about 2.5x the Opus run.
- Re-running the same build with a 325k task budget brought cost down ~17% and active build time down ~11%, giving up a few tail-end features.


**Figure 1 - Long-horizon builds.** Features built, steps, and active build time for Fable, budgeted Fable (325k), and Opus 4.8.


‍


We also swept Fable's task budget across the same builds. A task budget is not a hard token cap. It gives the model an allowance to pace the whole agent loop against. More budget gives Fable room to keep exploring, checking, and finishing late features. Less budget makes the run cheaper and faster, but can make the model stop before the full product shape is complete. The useful setting was the one that narrowed that long tail without forcing the model to drop late features. In this slice, 375k was that balance.


**Figure 2 - Fable cost per build at each task budget.**


EXAMPLES


## **What showed up in the apps**


The examples below come from the eval set. They are not the evidence by themselves, but they make the eval results easier to see: Fable was more likely to ship apps with real flows, seeded data, and fewer missing pieces after the first pass.


‍


Fable 5


A model's portfolio. The generated photography keeps one consistent face across the editorial, awards, and runway sections, with beautiful motion and a marquee that make the page feel alive. The image placement carries the layout. It reads premium, not templated.


Fable 5


A cursor-reactive 3D meadow that leans toward your pointer, with three scenes behind one nav: a grass field in the wind, still water with drifting petals, and fireflies after dark.


Fable 5


A playable voxel game. World generation, first-person movement, a hotbar, crafting.


EARLY USAGE


## **The first real usage on Emergent**


From the first night, we compared every Fable and Opus builder job on the platform. As the cohort grew from early experiments to thousands of builds, a few patterns held:


~8% fewer


new bugs on Fable builds (vs Opus 4.8)


‍


### **First builds: more work upfront, not a faster clock**


On the first build, step counts are close but Fable puts more work into each one:


- Steps stay nearly even, but active time is a median of 16 minutes on Fable against 10 on Opus. The extra time is extra work, not slowness.
- More per build: about 80% more credits and 1.7x the output tokens, with more parallel tool calls and more work handed to sub-agents.


The full distribution of first-build times shows where that extra time goes.


**Figure 3 - Active build time to the first finished build.** Opus front-loads: more of its first builds land in the 5-20 minute bands. Fable's mass sits later, in the 10-40 minute bands (52% vs 43%), longer, busier first builds.


### **Fewer new bugs on Fable builds**


Fable reported fewer new bugs on matched builds. Per user turn, 6.6% of Fable turns flagged a new bug against 7.2% on Opus 4.8.


**Figure 4 - User sentiment on bug turns.** Frustration runs lower on Fable, around 9% of bug turns against 13% on Opus 4.8, and Fable's bug turns skew more neutral. The gap holds on matched builds.


### **The model used the agent differently**


Fable took verification screenshots on 5.1% of its steps against 4.1% on Opus, and it routed 23.5% of its steps through specialist sub-agents against 14.6%.


Fable chose to think on fewer steps, 49% against 54%. It used explicit thinking slightly less often than Opus, but spent more time on the harder plan-then-implement moments where the next move mattered.


**Figure 5 - Per-step behavior.** Fable delegates 1.6x more and self-checks visually more often, while choosing to think on fewer steps. Tool failure rates are essentially identical.


Tool failure rates are effectively the same on both models. The difference is what happens after a failure: Fable takes fewer steps overall and is less likely to hand the same bug back to the user twice.


ECONOMICS


## **The cost story is context depth**


Fable's credit rate is 2x Opus's, mirroring Anthropic's API pricing, but a finished build does not cost twice as much. On matched paid builds the median Fable project ran about 66 credits against 44 on Opus 4.8, roughly 1.5x. Fewer steps and rounds absorb part of the rate gap, though not all of it: Fable carries a real but moderated premium.


Where does the spend go? In an agent loop, the cost of a round still tracks the size of the context it carries. Anthropic's prompt caching keeps the major chunk of repeated context cheap, but deeper sessions still get more expensive because each step carries more project state.


This is why the budget helps. A task budget is advisory, not a hard cap on the context window. A tighter budget did not make the model weaker in the range we measured; it nudged the model to spend less thinking effort across the run and finish before the context ballooned.


Across budget settings, the same pattern held: tighter runs used fewer total tokens, fewer steps, and less active build time. That does not mean Fable is terse. Its responses are as long or longer than Opus's. The saving comes from the loop around the response: fewer agent steps, slightly fewer rounds on average, and fewer repeated bugs returning to the user.


HARNESS ENGINEERING


## **The harness mattered as much as the model swap**


Swapping the model name was the easy part. Two harness changes mattered more than anything else we did for this release.


The first was persona work. From Opus 4.8 onward, the persona and instructions we give the agent flow into its thinking traces: the model narrates them to itself while deciding how to approach a step. That makes deliberation surprisingly hard to steer directly. Tell the agent plainly to think less and it does the opposite, it starts narrating about thinking less, reasoning through why it should keep the step short, and spends more tokens rather than fewer. It is a reminder that a prompt tuned for an earlier model is not automatically right for a smarter one: the same instruction that worked before can backfire once the model starts reasoning about it. Getting deliberation to actually drop on trivial steps took different prompt-tuning ideas rather than the usual direct instructions that used to work.


The second was parallel tool calls. We rebuilt prompts and tooling so the agent batches its work: instead of one tool call per round trip, it can emit several in a single response. Fable picked this up far more aggressively than Opus, 20.1% of its production steps are parallel batches against 8.1%. Each batch saves a round trip, and at deep context a round trip means re-reading a few hundred thousand tokens.


Caching covers the rest. About 19 of every 20 input tokens in the cohort were served from prompt cache, a 93-95% hit rate on both models, so the harness serves most of the deep context back at a cheaper cached rate.


One more thing the harness does quietly: it keeps fallback smooth. On under 5% of sessions, by Anthropic's reporting, Fable's safety classifiers route a response through Opus 4.8 instead. Because both run the same agent loop, tools, and context format on Emergent, the job continues with no restart. It also makes the analysis more conservative: some turns credited to Fable were actually answered by Opus, which shrinks every measured gap slightly. After the June 12 export-control disruption and July redeployment, Anthropic added a stricter classifier that may flag benign coding and debugging requests more often, so this fallback rate should be treated as a pre-redeployment baseline.


HEAD TO HEAD


## **The same brief, both models**


Three eval prompts, each run on both models, side by side.


### **An interactive solar system**


Opus 4.8


Fable 5


**Same prompt.** Fable's reading is the more realistic of the two.


### **A wedding-planning workspace**


Opus 4.8


Fable 5


**Same prompt.** Opus made a polished landing page but missed catching and fixing the cropped hero image. In similar apps, Fable caught visual issues earlier and shipped a seeded demo path into the planner.


### **An abstract painter's portfolio**


Opus 4.8


The stronger overall composition of the two, and the better navigation: working anchor links and catalogue filters.


Fable 5


Clean and editorial, with a modern scrolling marquee above the footer.


**Same prompt.** The closest pair in the set, and this round arguably goes to Opus. On creative one-pagers the models trade wins.


‍


MAX MODE


## **Max Mode is for highly complex tasks and issues**


There is also a setting above the default. Max Mode runs Fable 5 at its highest reasoning effort: maximum deliberation per step, full context headroom, no shortcuts. It is not the default: on an ordinary build, extra thinking is just extra spend.


Where it earns its keep is the edge cases: multi-system debugging, architecture migrations, the builds where both Opus and standard Fable stall and need hand-holding. We do not yet have enough eval volume at the highest effort setting to quote numbers.


ARCHITECTURE


## **Where it leads to**


If you ask what the 2x actually buys, the production answer is judgment: deciding what to inspect, when to delegate, when to verify, when to stop. And judgment is a small fraction of the tokens in a build.


That is why Fable is better understood as a judgment layer than as the default worker for every request. Put it where ambiguity, context, or repair cost is high. Let Opus and Sonnet carry the routine work where speed and volume matter more.


One product shape is an Advisor tool: Fable helps plan, debug, review, and decide the next move, while the normal build work can still run on a cheaper model. The user gets a more intelligent planning layer without paying Fable's rate for every routine step.


That is the real product implication of Fable 5. The value is not just a smarter answer. It is a model that can sit above the work, hold the shape of the project, and keep the next step from becoming the user's problem.


E3 is the bigger version of that idea. E3 acts as the orchestrator: it decides what needs to happen, when to inspect, when to split the work, and when to ask for a fix. E1 remains the executor underneath, and E1 can run on Opus or Sonnet depending on the task. Fable belongs in the higher judgment layer, where its lead in planning, review, and long-context repair matters most.


Fable already behaves this way when left alone, routing a quarter of its steps to cheaper specialists on its own initiative. E3's job is to turn that tendency into an architecture. The interesting thing about a Mythos-class model is not only that the top model is stronger, but that it changes how the cheaper models should be used.


One pattern holds throughout the data: the longer and more complex the task, the larger Fable's lead, with the gains concentrated on the builds you could not easily do by hand, where its judgment matters most.
