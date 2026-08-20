---
schema_version: "1.0.0"
document_id: "acb4d0419ead45a0d624237791427a1287cc33ad5a30d8423e8f23ad16ffa08d"
company_key: "yc-arc-prize-foundation"
company: "ARC Prize Foundation"
source_id: "yc-arc-prize-foundation-atom-3536270edb25"
canonical_url: "https://arcprize.org/blog/arc-agi-3-gpt-5-5-opus-4-7-analysis"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:48.897807+00:00"
fetched_at: "2026-07-28T20:51:46.128682+00:00"
content_hash: "sha256:b18c37b94a0a6b1dc617902b568c2dae784c93d4d82c3afda8f9a82c1d47e979"
---

# Analyzing GPT-5.5 & Opus 4.7 with ARC-AGI-3

# Analyzing GPT-5.5 & Opus 4.7 with ARC-AGI-3


AI benchmarks can be incredible tools, but they usually only tell you if a model passed or failed. With ARC-AGI-3, however, we can see the thought process *behind* the score, not just the outcome.


This week we went through 160 replays and reasoning traces from[OpenAI’s GPT-5.5](https://openai.com/index/introducing-gpt-5-5/) and[Anthropic’s Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7) attempting novel, long-horizon environments. The scores were just one data point, but the interesting story is *how* they achieved their score.


Today we’re open-sourcing our analysis package.


Model ARC-AGI-3 Score* Public Demo Replays


GPT-5.5 0.43%[Link](https://arcprize.org/scorecards/model/openai-gpt-5-5-2026-04-23-high)


Opus 4.7 0.18%[Link](https://arcprize.org/scorecards/model/anthropic-opus-4-7-high)


** Scores tested with the semi-private dataset*


With ARC-AGI-3 we can replay every action alongside the model's reasoning to see where it formed a hypothesis, where it abandoned a correct one, where it locked onto a wrong idea and couldn't let go.


We found 3 common failure modes:


- **True Local Effect, False World Model** - The models understand which action produced a change, but they fail to translate the effect into a global rule
- **Wrong Level of Abstraction From Training Data** - The models mistake an ARC-AGI-3 environment for another game
- **Solved The Level, Didn’t Learn The Game** - Even if a model beat a level, it’s unable to use that reward signal to enforce the correct actions


## ARC-AGI-3 as an analysis tool


[ARC-AGI-3](https://arcprize.org/arc-agi/3) is a series of 135 novel environments. Each was hand-crafted by a human to test the ability of AI models to adapt to novelty.[Play them](https://arcprize.org/tasks/ls20) yourself or watch our[launch video](https://www.youtube.com/watch?v=f_xT45Pi0UQ) .


The test-takers, whether human or AI, are *not* given instructions on how to play an environment. To make progress they must:


- Explore unfamiliar interfaces
- Infer rules from sparse feedback (aka world model)
- Form & test hypotheses
- Recover from wrong assumptions
- Transfer what they learned from one level to the next (aka continual learning)


Each environment is built *without* the cultural knowledge a model would usually lean on. This means the environments isolate abstract reasoning.


You can think of ARC-AGI-3 as the lowest common denominator across novelty, ambiguity, planning, and adaptation. These are the same demands real-world tasks make of agents.


ARC-AGI-3 Public Demo environments:[ar25](https://arcprize.org/tasks/ar25) ,[lf52](https://arcprize.org/tasks/lf52) ,[sb26](https://arcprize.org/tasks/sb26)


## Failure modes on ARC-AGI-3


ARC-AGI-3 was built with testing and model auditing in mind. Each AI run is recorded along with its reasoning traces. We’ve had[over 1,000,000 games](https://x.com/arcprize/status/2049878321576743263?s=20) played on ARC-AGI-3 so far.


For this analysis we:


- Download all the logs/reasoning/steps from every public game run for GPT-5.5 and Opus 4.7
- Write a strategy for each game to serve as our ground truth answer
- Ask Codex/Claude Code to analyze the reasoning steps against the level strategy to find failure modes
- Do a meta-analysis across games for a single model. Repeat the meta-analysis *across* models.
- Validate findings, by hand, with a human


This led us to discover why each model passed/failed, which failure modes were shared and which were unique to each model.


### Failure mode 1: true local effect, false world model


The first failure mode we saw was the most dominant pattern. Models were able to perceive a local effect:


> When I press ACTION3, this object rotates.


but they weren’t able to translate that into a world model:


> ACTION3 rotates the object, and rotation controls which side gets a new value, so I should orient the object to match the target before acting.


Put another way, the models don’t fail because they don’t observe anything...they fail because they can't anchor their observation in a world model.


For example, Opus, when playing` cd82` , knew that` ACTION3` rotated the container by[step 4](https://arcprize.org/replay/67ca6a82-fc59-4bcb-8646-e66edd66d2eb?frame=4&quote=rotated&quoteFrame=4&quotePrefix=Interesting%21+ACTION3+&quoteSuffix=+the+middle-bottom+15+container+into+a+diamond+shape.+Let+me+try+ACTION4.%0A%0AACTIO&reasoning=decision) , and by[step 6](https://arcprize.org/replay/67ca6a82-fc59-4bcb-8646-e66edd66d2eb?frame=6&quote=ACTION5+is+pouring+15s+from+the+bottom-right+container+into+the+0+container&quoteFrame=6&quotePrefix=I+see+-+&quoteSuffix=+below+it.+The+15s+fall+down.+Let+me+continue+the+flow+and+see.%0A%0AACTION5&reasoning=decision) it saw` ACTION5` pour/dip paint but it never converted that into "orient bucket, then dip to recreate the top-left target."


Opus 4.7 understood that ACTION3 rotates the objects but failed to grasp the game concepts.
[Opus 4.7 Playing cd82](https://arcprize.org/replay/67ca6a82-fc59-4bcb-8646-e66edd66d2eb) . Game Score: 0%


Or in` cn04` , Opus found a successful rotate-then-place interaction (this is the correct hypothesis,[step 23](https://arcprize.org/replay/c99b68f5-0d61-46ff-a7b3-3491ef886f36?frame=23&quote=Let+me+move%2Frotate+to+find+where+to+place+this+new+shape.&quoteFrame=23&quotePrefix=ertical+with+8+markers+on+left%29.+The+original+source+location+is+now+marked+12.+&quoteSuffix=%0A%0AACTION3&reasoning=decision) ) but optimized for whole-shape overlap (incorrect) and fake top-row progress ([step 60](https://arcprize.org/replay/c99b68f5-0d61-46ff-a7b3-3491ef886f36?frame=60&quote=Looking+at+the+progress%3A+the+0-bar+is+at+16%2F32+cells+%2850%25%29&quoteFrame=60&quoteSuffix=.+I+need+to+convert+the+bottom+Y-shape+%2814-colored%29+to+0.+Let+me+click+firmly+on&reasoning=decision) ).


[Opus 4.7 Playing cn04](https://arcprize.org/replay/c99b68f5-0d61-46ff-a7b3-3491ef886f36) . Game Score: 0%


### Failure mode 2: wrong level of abstraction from training data


The second failure mode came from an incorrect level of abstraction from a model’s training data. Across the runs, the models repeatedly explained unfamiliar mechanics by mapping them to known games: Tetris,[Frogger](https://arcprize.org/replay/3beb8811-6f2a-4ce1-a1b6-bf53d1b4ad1d?frame=477&quote=-+A+grid+with+paths+%280%2C0%2C0%29+and+stepping+stones+%282%2C2%2C2%29+forming+a+Frogger-like+puzzle&quoteFrame=477&quotePrefix=+rows+27-29%2C+cols+27-29%0A-+A+target+%2814%2C14%2C14+pattern%29+at+rows+46-48%2C+cols+45-47%0A&quoteSuffix=%0A-+The+player+needs+to+navigate+to+the+target%0A%0AAcross+the+frames%2C+the+player+app&reasoning=decision) ,[Sokoban](https://arcprize.org/replay/70ecc44d-2fda-4c15-8e3f-c9d29e067943?frame=1028&quote=this+appears+to+be+a+sokoban-like+puzzle+where+I+need+to+move+the+9-shape+through+the+maze.&quoteFrame=1028&quotePrefix=ze+%28from+rows+14-18+to+rows+8-12%29.+The+indicator+in+the+top-left+also+changed+-+&quoteSuffix=%0A%0ALet+me+continue+trying+to+navigate.+I%27ll+try+ACTION5+again+to+see+if+it+contin&reasoning=decision) , Powder Toy, Flood-It, MiniGrid, CoinRun, Breakout, Pong, Boulder Dash, and others. While recalling abstractions from core prior knowledge is helpful in theory, the literal analogies from the model’s training data hijacked action selection.


The problem is that a local visual resemblance becomes a full gameplay theory, then the model wastes actions testing the wrong affordances.


In` cd82` , GPT-5.5 anchored on sand/physics/Flood-It mechanics.` ls20` became Breakout instead of key combinations.


GPT-5.5 mistakes` ls20` for other games.[GPT-5.5 Playing ls20](https://arcprize.org/replay/672f4ae0-c481-4e10-aa35-55a71e412ec5?frame=11) . Game Score: 0%


### Failure mode 3: solved the level, didn’t learn the game


The final failure mode we saw was that even if a model beat a level, that reward did *not* translate into further success. This shows us that beating a level is not the same as understanding it.


Two Opus runs make this especially clear. On` ka59` , Opus[solved Level 1 in 37 actions](https://arcprize.org/replay/1dfbb619-2f6a-484a-89c8-bbe64abc977b?frame=43) , but its working theory of the click (teleporting the active character) was wrong. Although it looked like a clean win, this was a coincidence between a misread primitive and a forgiving level.


When Level 2 demanded the real mechanic (shape-matching and pushing), Opus’s mislabel theory hardened into "[click each target to fill it](https://arcprize.org/replay/1dfbb619-2f6a-484a-89c8-bbe64abc977b?frame=251&quote=Let+me+click+on+the+4s+in+the+box+at+rows+47-48%2C+cols+44-45&quoteFrame=251&quotePrefix=Looking+at+remaining+4s+inside+14-bordered+boxes.+&quoteSuffix=.%0A%0AACTION6+44+47&reasoning=decision) ," and the run never recovered.


Opus 4.7 gets stuck in a click-fishing loop.[Opus 4.7 playing ka59](https://arcprize.org/replay/1dfbb619-2f6a-484a-89c8-bbe64abc977b) . Game Score: 2.04%


` ar25` shows the same pattern at a different abstraction level. Opus cleared Level 1 with a correct read of mirrored movement ([step 4](https://arcprize.org/replay/7752f433-a3a4-4026-ae8c-6c32b4afcf9d?frame=4&quote=They+mirror+around+the+10-divider&quoteFrame=4&quotePrefix=s%29+at+specific+positions%0A-+The+4-shape+is+the+%22complete%22+version+%28no+holes%29++%0A-+&quoteSuffix=%0A-+The+right+column+fills+with+5s+each+action+%28maybe+action+counter%29%0A-+ACTION6+t&reasoning=decision) ), then in Level 2 actually discovered the new movable-axis mechanic ([step 227](https://arcprize.org/replay/7752f433-a3a4-4026-ae8c-6c32b4afcf9d?frame=227&quote=ACTION3+moved+the+10-column+left+by+3%21&quoteFrame=227&quoteSuffix=+Let+me+continue+with+ACTION3%3A%0A%0AACTION3&reasoning=decision) ), but it still drifted into hallucinated rules of punching holes and needing a flip.


[Opus 4.7 Playing ar25](https://arcprize.org/replay/7752f433-a3a4-4026-ae8c-6c32b4afcf9d) . Game Score: 0.15%


In both cases the Level 1 success masked a missing or distorted primitive, and the partial win became a confident scaffold for the wrong Level 2 strategy.


This also shows us that early level progression can be a noisy signal of comprehension. Without an explicit check on *why* the prior level was won, models will carry their misconception into the next level.


## Opus 4.7: wrong compression, GPT-5.5: failure to compress


As we compare the runs of GPT-5.5 and Opus 4.7 we’re able to see they failed in *different* ways. This is important because **aggregate scores alone would hide this distinction** .


Opus had the wrong compression, GPT-5.5 failed to compress.


Opus 4.7 is stronger at short-horizon mechanic discovery. On` ar25` it identifies the mirror structure almost immediately and clears Level 1. On` ka59` it reads the two-character, two-target layout and executes the short Level 1 sequence even with an incomplete world model.


The flipside is that Opus is also more likely to latch onto a false invariant and execute it aggressively. On` cn04` it leans into a fake progress/timer/conversion theory and spends the opener click-fishing inside that story ([step 60](https://arcprize.org/replay/c99b68f5-0d61-46ff-a7b3-3491ef886f36?frame=60&quote=Let+me+click+firmly+on+its+body&quoteFrame=60&quotePrefix=s+at+16%2F32+cells+%2850%25%29.+I+need+to+convert+the+bottom+Y-shape+%2814-colored%29+to+0.+&quoteSuffix=.%0A%0AACTION6+48+27&reasoning=decision) ). It does form a working theory, it's just the wrong one.


GPT-5.5 sits at the other end. Its hypothesis generation is wider, which makes it more likely to articulate the right idea but less likely to turn it into a plan. On` ar25` it names the mirror effect but keeps reopening the genre space, drifting through Tetris, Frogger, Pong, and Tower of Hanoi instead of committing to reflection. On` ka59` it reaches the correct object ontology, two target outlines and a switchable second character, but never commits to it.


The difference comes down to compression. Opus compressed its observations into a confident-but-wrong theory. GPT-5.5 had difficulty compressing at all.


## ARC-AGI-3 measures agent autonomy


Each ARC-AGI-3 environment has been[solved by at least two humans](https://arcprize.org/blog/arc-agi-3-human-dataset) without special training. What makes them hard for agents is that they require something closer to real intelligence through encountering an unfamiliar environment, forming a working theory, testing it, updating it when the evidence disagrees, and carrying forward what was learned.


That same level of meta-learning is also what real-world agents will need.


Real-world agents will not just operate inside clean benchmark prompts or memorized task templates. They will face unfamiliar websites, internal tools, dashboards, forms, APIs, workflows, and edge cases that were not described in advance. In those settings, failure will often look exactly like the failure modes in ARC-AGI-3.


That's why ARC Prize Foundation will continue auditing every major frontier release. Scores tell you what a model achieved. Replays tell you whether or not the reasoning is likely to generalize.


If you’d like to help us audit the frontier of AI with ARC-AGI-3, come[join the team](https://arcprize.org/jobs) .


---


### Analysis notes


- With our default tests, GPT-5.5 did not return its reasoning traces. The qualitative analysis above therefore uses an` analysis mode` run, which is an alternate testing setup intended to elicit more explicit descriptions of how the model is interpreting the environment and deciding what to try next. The official GPT-5.5 score is not from that mode. All reported scores use the standard ARC-AGI-3 harness, matching the setup used for Opus 4.7.
