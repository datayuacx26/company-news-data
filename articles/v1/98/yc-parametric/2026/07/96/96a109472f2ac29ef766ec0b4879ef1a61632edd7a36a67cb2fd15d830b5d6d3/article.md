---
schema_version: "1.0.0"
document_id: "96a109472f2ac29ef766ec0b4879ef1a61632edd7a36a67cb2fd15d830b5d6d3"
company_key: "yc-parametric"
company: "Parametric"
source_id: "yc-parametric-news-import-eb04a40d8d59"
canonical_url: "https://parametric.company/blog/calibration-precedes-evaluation"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-24T00:39:21.525691+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:bf54606db005d5f970f51a395d5f3f34af6aa167856baaad1b555c46c447003b"
---

# Calibration Precedes Evaluation

Training robots is an experimental process: we train candidate policies and compare them with the current baseline to determine whether each iteration improves behavior. For a fair comparison, we evaluate every policy across repeated rollouts of the same task using the same evaluation protocol and compare success rates.


But a fair comparison is not necessarily an informative one. The task must also be calibrated to a difficulty that can distinguish the policies. If it exceeds their current capabilities, success rates cluster near 0%. If it falls well within their capabilities, they cluster near 100%. At either extreme, meaningfully different policies can look equivalent.


In simulation, this is easy to correct because rollouts are cheap: adjust the task difficulty and run the evaluation again. Real-world rollouts, by contrast, are our most direct measure of deployment readiness, but each consumes skilled operator time and costly hardware. A task set at the wrong difficulty can cost days of work and thousands of dollars without yielding a clear comparison.


In our[last post](https://www.parametric.company/blog/evaluation-precedes-capabilities) , we argued that evaluation precedes capabilities: robotics progress begins by measuring the gap between what robots can do and what we want them to do. Here, we focus on a prerequisite for making that measurement useful. Calibration moves the shared task difficulty toward the policies’ capability boundaries, where outcomes are most informative and differences become easier to measure.


## Setting Up the Evaluation


We used this approach to compare four fine-tuned variants of the same base model (π0.5). Each fine-tune used the same hyperparameters and a combination of two datasets: 60 hours of towel-folding data and roughly 56 hours of t-shirt-folding data. The towel data always came from the evaluation embodiment, while the source of the t-shirt data varied.


For the **matched policy** , both datasets came from the evaluation embodiment. For **Policy A** , **Policy B** , and **Policy C** , the t-shirt data came from similar but different robots in different environments. These policies therefore paired same-embodiment data from a different task (towel folding) with cross-embodiment data from the target task (t-shirt folding). We refer to them as the **cross-embodiment policies** .


We began by evaluating the obvious task: fold a crumpled t-shirt from start to finish. However, the cross-embodiment policies almost never completed it, leaving their success rates clustered near zero. Statistically separating them there would require a cost-prohibitive number of rollouts.


So we made the task easier.


## The Calibration Staircase


Psychophysics has used adaptive staircases for decades to find the edge of human perception, making stimuli harder after correct answers and easier after incorrect ones. We adapted the same idea for robots.


The robot version is simple:


1. 01


If policies fail consistently, make the task easier.


2. 02


If the strongest policies succeed consistently, make it harder.


3. 03


When outcomes are mixed, collect enough rollouts to estimate performance.


4. 04


Once the relevant comparisons are clear, stop or advance only the policies needed for the next comparison.


Calibration makes evaluation more efficient by concentrating the rollout budget where outcomes carry the most information. For a binary outcome with success probability


, its expected information content is measured by Bernoulli entropy:


Entropy reaches its maximum of one bit at a 50% success rate. In practice, a mixture of successes and failures marks the transition between tasks a policy can reliably complete and tasks it cannot. We therefore used entropy as a signal that task difficulty was near a policy’s capability boundary.


Locating a boundary is not the same as distinguishing policies: every policy could succeed half the time. We used 95% Wilson confidence intervals to assess separation and stopped collecting once the relevant intervals no longer overlapped. Entropy helped us find the boundaries; the intervals told us when a comparison was clear enough to stop.


We applied this process across four task levels, varying the shirt’s starting state and how much of the fold the policy had to complete. From easiest to hardest, the levels were:


1. 01


Fold at least one sleeve from a flattened shirt.


2. 02


Fold both sleeves from a flattened shirt.


3. 03


Complete the fold from an already-straightened shirt.


4. 04


Complete the full task from a crumpled state.


Adaptive difficulty staircase


## Walking Up the Staircase


We evaluated all four policies blindly. Operators saw anonymous checkpoint labels and did not know the source of each policy’s training data. The descriptive names used here were assigned after evaluation. A rollout counted as a success if the policy completed the level before the timeout.


**Step one: fold at least one sleeve from a flattened shirt within 90 seconds.** The matched policy nearly maxed out the task at **49/50** . The cross-embodiment policies were more mixed: Policy A reached **34/50** , Policy B **31/50** , and Policy C **25/50** . The matched policy’s interval no longer overlapped theirs, but Policies A, B, and C still overlapped one another. The task was too easy for the matched policy and did not distinguish the cross-embodiment policies, so we stepped up.


**Step two: fold both sleeves within 90 seconds.** At the second level, Policy A began to separate from Policies B and C. The matched policy remained near the ceiling at **46/50** , Policy A reached **27/50** , and Policies B and C each fell to **11/50** .


The intervals now separated the matched policy from Policy A, and Policy A from Policies B and C. Policies B and C still could not be distinguished from each other. We had three performance groups: the matched policy, Policy A, and a group containing Policies B and C. So we stopped testing Policies B and C at harder levels and kept Policy A as the strongest reference.


**Step three: complete the fold from a straightened shirt.** The matched policy completed all **50/50** rollouts, while Policy A completed **12/50** . The comparison was clear, but the matched policy was still at the ceiling. So we stepped up one last time.


**Step four: complete the full task from a crumpled shirt.** This was the hardest level we tested. Unlike the earlier levels, the starting state was nondeterministic: every reset produced a different crumpled shirt configuration, and the policy had to recover from whatever it received. The matched policy completed **23/30** rollouts, while Policy A completed **2/30** . Step four was the first level where the matched policy did not saturate. That made it the hardest informative level we tested, the closest tested level to its current capability boundary, and a useful benchmark for the next policy we train.


Had we tested only the full task, the cross-embodiment policies would have clustered near zero. By starting easier and climbing, we exposed differences that the hardest task would not have elucidated. We also found a task where the matched policy did not saturate.


That is the point of calibration: choose the right task for the question before spending hundreds of rollouts. These results describe policy performance on the evaluation hardware, not the intrinsic quality of the training datasets.


Here are the full results:


Four folding tasks


Task 1: Folding First Sleeve


Task 2: Folding Both Sleeves


Task 3: Full Fold (from Straightened)


Task 4: Full Fold (from Crumpled)


## Accelerating the Robotics Research Loop


Progress in robotics comes from making targeted changes across the robotics stack and then evaluating their effects in the real world. Real-world evaluations provide the clearest evidence that a change works. Simulation can speed up iteration, but its results need to be calibrated against real-world performance.


Given how expensive real-world evaluations are, we need to learn as much as possible from each rollout. By calibrating task difficulty based on policy performance, we focus our rollouts where differences between policies are easiest to measure.


More efficient evaluations accelerate the research loop as a whole. They let us test more hypotheses, identify what improves performance, and scale what works across more deployments. The faster we iterate, the faster we build more capable robots and deploy them at scale.
