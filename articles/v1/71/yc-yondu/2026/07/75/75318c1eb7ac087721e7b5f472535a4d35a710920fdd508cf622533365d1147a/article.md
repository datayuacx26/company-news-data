---
schema_version: "1.0.0"
document_id: "75318c1eb7ac087721e7b5f472535a4d35a710920fdd508cf622533365d1147a"
company_key: "yc-yondu"
company: "Yondu"
source_id: "yc-yondu-news-import-cb001de43134"
canonical_url: "https://www.yondu.ai/blog-posts/agentic-subtask-labeling-for-robot-video"
published_at: "2026-07-06T00:00:00+00:00"
first_seen_at: "2026-07-26T06:13:50.682599+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:77eb4424aa4facf2fcd42d9f01c75a563f45a28fd905ca752aa78ef07ca0ec57"
---

# Agentic Subtask Labeling for Robot Videos

## TL;DR


- We realized VLM-powered event and subtask labeling was missing the agentic-loop trick that is now common in AI coding tasks.
- So we built it: a visual subtask labeler for robot videos that separates task specification from episode labeling.
- First, it builds a fixed task taxonomy: subtasks, mistakes, boundary cues, task variants, and visual references.
- Then it labels each episode against that taxonomy with a bounded VLM loop that can request more timestamps when the initial sample misses important boundary evidence.


Robot datasets often contain synchronized videos, actions, states, and a coarse task name. What they usually do not contain is the dense timeline we want when inspecting, filtering, training, or debugging. Creating this timeline in a fast, automated, and accurate way is difficult without relying entirely on human annotators.


The difficulty lies in finding the few frames where subtask trajectories change: the first stable grasp that is not a mistake, the first frame of a drop, or the transition from recovery back into the intended task. When the right boundaries are not found, poor grounding and hallucinations can propagate through the rest of the labeling procedure.


We previously relied on human annotation, and an obvious but key realization was that human annotators do not watch a video uniformly from beginning to end. They scrub and jump around. They inspect one camera, switch to another, slow down near transitions, and stop when the boundary is good enough.


That observation shaped the method we used to create our labeler. Instead of forcing a vision-language model to annotate sampled video frames in one shot, we made annotation a bounded tool-use loop. The model sees an initial sample, decides whether evidence is missing, requests more timestamps, receives those frames, and then revises its segmentation.


**In short: the model labels videos the way a good annotator uses a scrubber.**


**Figure 1.** The core loop. The first frame sample does not need to be perfect; it only needs to show the model where more evidence may be needed.


## Dense Subtask Labels Are Useful


A high-level task name is often too coarse, and raw actions are often too low-level. A demonstration labeled only as "fold cloth" or "move object to target" contains several qualitatively different phases. Treating the whole video as one unit hides the structure that makes it useful.


Dense subtask intervals help in several places:


1. **Inspection.** Find episodes with drops, collisions, incomplete starts, or failed placements.
2. **Filtering.** Remove invalid recordings or isolate successful segments.
3. **Training.** Use subtasks as auxiliary supervision, curriculum structure, language-motion targets, or segment-conditioned examples.
4. **Reward learning.** Use segmented demonstrations as dense supervision when scalar rewards are expensive or ambiguous.
5. **Debugging.** Localize a policy failure to grasp, transport, placement, alignment, recovery, or another stage.


This lines up with a broader direction in robot learning: using intermediate structure between high-level goals and low-level actions.[RT-H](https://arxiv.org/abs/2403.01823) uses language-level motions between task commands and low-level actions.[Embodied Chain-of-Thought](https://arxiv.org/abs/2407.08693) trains policies to reason through plans, subtasks, motions, and visual evidence.[Subtask-Aware Visual Reward Learning from Segmented Demonstrations](https://arxiv.org/abs/2502.20630) uses segmented demonstrations for dense reward learning.[Vision Language Models are In-Context Value Learners](https://arxiv.org/abs/2411.04549) shows how visual examples can help models reason about progress through robot trajectories.


## One-Shot Labeling Is Brittle


The obvious automated baseline is to sample frames at a fixed rate, send them to a VLM, and ask for intervals. Sometimes interval boundaries are snapped to the nearest sampled frame. This by itself is not too bad. But because the VLM can miss certain key transition events, it may trick itself into believing a subtask is much longer than it actually is, or that it ends prematurely.


The opposite approach is to show many more frames. But this has its own issues. Long videos across multiple cameras can exceed practical request-size limits. Packing frames into mosaics or strips can address coverage, but it compresses local detail, which can cause grasps or slips to be overlooked.


## Do Annotation as Looped Tool Use


We came up with this idea by asking: what if a coding or computer-use agent had access to our annotation tool? It could scrub the timeline of a video and keep collecting frames until it had enough information to figure out what subtask intervals should be.


Almost all agent harnesses now work this way. They continue looping, inspecting their own state and using tools until some stopping condition is met. As background, LangChain's[The Art of Loop Engineering](https://www.langchain.com/blog/the-art-of-loop-engineering) describes agents as models calling tools in loops, with verification loops wrapped around the work. Jeremy Theocharis makes the same point from the human-process side in[All You Need is Loops (and Humans)](https://www.theocharis.dev/blog/loops-are-all-you-need/) : stop giving the model only a destination; give it a process and a definition of done.


Our eureka moment was realizing we did not need to force one-shot correctness. Just give the VLM access to the fundamental "tool" a human annotator has: jump to specific timestamps.


```text
request_more_timepoints(timestamp_s, reason)
```


That action allows a one-shot classifier to search locally around suspected boundaries, effectively turning it into an automated annotator.


## Stage 1: Build the Taxonomy


To make this work, first we made sure the model could not invent labels independently for every video. If it did, one episode might use "pick up object," another might use "lift object," and another might use "extract object" for the same behavior.


Our pipeline first builds a local taxonomy for the task family. It describes:


- task variants, such as folding shirts versus pants,
- allowed subtasks and their corresponding task variant,
- common mistakes,
- detailed descriptions of subtasks for grounding,
- start and end cues for more grounding,
- expected motion and contact patterns,
- visual reference frames for subtask starts and ends,
- ambiguity policies,
- guidance for incomplete or invalid recordings.


The taxonomy can be built automatically, manually, or in a hybrid mode. The VLM can look at a collection of episodes to infer the subtask menu itself, or we can provide the menu. Hybrid mode is usually strongest. A human provides starting hints, and the model uses sampled visual evidence to refine those hints into descriptions, cues, and references.


Example guidance can look like this:


```text
{
"notes"  : [
"Use these as starting hints, not fixed labels."  ,
"Keep every hinted mistake as a possible mistake label, even if sampled evidence does not show it."  ,
"A retry is not a mistake by itself; retrying remains part of the original subtask being attempted."  ,
"Create a separate mistake interval only for the visible mistake event itself."
],
"preliminary_subtasks"  : [
{
"subtask"  :   "reach toward the object"  ,
"examples"  : [
{
"episode"  :   212  ,
"start_timestamp_s"  :   9.9  ,
"end_timestamp_s"  :   12.95
}
]
}
],
"preliminary_mistakes"  : [
{
"mistake"  :   "mistake: slip or improper grasp causing failure to pick up"  ,
"examples"  : [
{
"episode"  :   212  ,
"start_timestamp_s"  :   16.6  ,
"end_timestamp_s"  :   17.8
}
]
}
]
}
```


## Visual References Matter


We found that if the subtask menu also comes with visual references, selected automatically or manually, fine boundary detection greatly improves. It can be the difference between ending a subtask at near completion and ending it exactly when the defining state is reached.


During taxonomy generation, the model selects representative start and end frames for each subtask when possible. Those frames are passed into later episode-labeling calls as visual anchors. This is a practical annotation use of the multimodal in-context-learning pattern studied in[Vision Language Models are In-Context Value Learners](https://arxiv.org/abs/2411.04549) : instead of only telling the model what a label means, we show it examples of what the boundary looked like in this dataset.


Reference frames can include multiple camera views. A main camera may show the global object state, while a wrist or side camera shows the decisive contact. The taxonomy records the timepoint, camera slots, rationale, and confidence for each reference.


## Visual Sampling


We choose not to stitch images together. Earlier experiments used strips and grids, but stitching made images very wide and risked losing detail by compressing key frames into a composite. The current approach keeps camera frames separate.


For each selected timepoint, the prompt can include multiple views, such as` main` ,` left` , and` right` . Each image is resized only to fit within a configured maximum dimension while preserving aspect ratio. Each image also carries concise metadata:


- ordinal id,
- timestamp,
- camera slot.


For taxonomy generation, sampling has two sources:


- required timepoints from human timestamp hints,
- additional timepoints selected to cover representative videos under the visual budget.


For seed labeling, sampling is controlled by a configurable timepoint budget or cadence. The model can then request additional timestamps around ambiguous boundaries.


Recent video-understanding work also arrived at the conclusion that the representation sent to the VLM matters.[Apollo](https://arxiv.org/abs/2412.10360) treats sampling and representation as central design choices for video-LMMs.[Streaming Dense Video Captioning](https://arxiv.org/abs/2404.01297) frames long-video understanding as temporally localized event description. Scale Labs'[large-scale dense video captioning writeup](https://labs.scale.com/blog/path-to-large-scale-dense-video-captioning) makes the same point in a manipulation-video annotation setting.[Adaptive Keyframe Sampling](https://arxiv.org/abs/2502.21271) and[Q-Frame](https://arxiv.org/abs/2506.22139) both focus on selecting the frames most likely to preserve task-relevant evidence.


## Stage 2: Label Each Episode


Once a taxonomy is accepted, videos can be labeled independently and in parallel.


The seed-labeling prompt receives:


- labeling directions,
- the accepted taxonomy,
- compact task-variant guidance,
- compact local subtask menu entries,
- taxonomy reference images,
- sampled episode frames in chronological order,
- a structured response schema.


Episode frames are sent in order. There is no group randomization and no stitching. The model first describes what is visible at each sampled timepoint to ground itself, then either returns final intervals or asks for more frames.


**Figure 2.** A review view for derived labels. The automated pipeline writes interval labels that can be inspected against synchronized video.


## The Timepoint-Request Loop


The model has two response modes:


1. ` request_more_timepoints`
2. ` final_labels`


On the first pass, the model sees a sparse set of frames. If the evidence is enough, it returns final labels. If not, it requests specific timestamps and explains why each timestamp matters.


Examples:


- "The grasp begins somewhere between 12s and 15s; request 13.5s."
- "The object is suspended at 43s and supported by the target at 45s; request 44s."
- "The object may have slipped between sampled frames; request 31.8s and 32.2s."
- "The main camera is ambiguous; request all configured cameras at 69s."


We usually allow the initial call plus two follow-ups to stay within budget. This is where we differ from most agentic loops. It is more of a` for` loop than a` while` loop. In practice, most labeling runs return by the first follow-up call.


Conceptually:


```text
accepted taxonomy
-> initial sampled frames
-> model labels or requests more timepoints
-> sampled requested frames
-> model revises labels or requests more timepoints
-> sampled requested frames
-> final labels
-> validation
-> review/  export
```


This is the annotation-specific version of iterative frame search.[VideoAgent](https://arxiv.org/abs/2403.10517) uses an agent loop to retrieve video evidence for question answering.[Universal Video Temporal Grounding with Generative Multi-modal Large Language Models](https://arxiv.org/abs/2506.18883) targets the related problem of localizing language-described moments in video. Here, the model retrieves more evidence for boundary finding, but the output is not a natural-language answer. It is a validated interval segmentation over a fixed taxonomy.


## A Concrete Run


We recently used this on an in-house robot-video dataset collected for a partner workflow. The task family involved configuring a metal chain so it fit a predefined alignment.


The taxonomy pass sampled representative episodes, incorporated human-provided subtask and mistake hints, proposed a local subtask menu, and then ran a critique pass against additional visual evidence.


**Figure 3.** Example playback of automatically generated subtask intervals over robot video.


‍


The request loop mattered in practice. Many episodes did not need extra evidence, but many did. Many videos needed at least one follow-up to pinpoint a transition.


## Room for Human Review


This method substantially reduces the need for manual annotation, but it does not make human review irrelevant. Underlying datasets can contain messy or unclean episodes with one-off events or issues. Those can be routed to a focused review process.


Each episode can be marked with review-oriented metadata such as:


- normal,
- incomplete,
- failure or retry,
- human intervention,
- mid-task start,
- truncated end,
- offscreen manipulation.


This metadata proved useful for dataset cleanup and a small amount of targeted post-labeling review.


## Alternative Design Choices


We originally experimented with initial sampling biased toward robot state events: gripper transitions, large end-effector accelerations, and large joint-state changes. However, uniform sampling had a similar spread, and the VLM could often infer when state events would occur and request those frames. There is still room here for more efficient sampling.


We decided to treat mistakes as "subtasks" themselves. There is an argument that it may be better to treat them as their own labels, and it is an argument I like because that is what our RL pipeline has been doing:` valid/good` states for useful task progress, recovery, or efficient action, and` invalid/bad` states for mistakes, inefficient action, or behavior that should not be reinforced. For ease of implementation and to save on calls, we merged the two labeling tasks for now. But there is definitely room to have another loop purely for mistake or "am-I-taking-efficient-actions" detection.


## Conclusion


Subtask annotation should be a loop, not a one-shot prediction.


Robot videos contain small, high-stakes temporal events. A sparse first pass will miss some of them. A human annotator handles that by scrubbing to the right moment. The labeler gives a VLM the same basic ability through a structured` request_more_timepoints` action.


The result is a practical pipeline for turning raw videos into timestamped subtask intervals: define the taxonomy once, label episodes against it, let the model ask for missing evidence, validate every output, and route uncertain cases to humans.


The breakthrough was not a longer prompt. It was changing the shape of the job from "answer from these frames" to "search for enough evidence to label the episode."


## References and Further Reading


1. Suneel Belkhale et al.,[RT-H: Action Hierarchies Using Language](https://arxiv.org/abs/2403.01823) , 2024. Motivates language-level intermediate structure between high-level tasks and low-level robot actions.
2. Michal Zawalski et al.,[Robotic Control via Embodied Chain-of-Thought Reasoning](https://arxiv.org/abs/2407.08693) , 2024. Uses grounded intermediate reasoning over plans, subtasks, motions, and visual evidence for robot control.
3. Changyeon Kim et al.,[Subtask-Aware Visual Reward Learning from Segmented Demonstrations](https://arxiv.org/abs/2502.20630) , 2025. Uses segmented demonstrations to learn dense visual rewards.
4. Yecheng Jason Ma et al.,[Vision Language Models are In-Context Value Learners](https://arxiv.org/abs/2411.04549) , 2024. Shows VLMs can reason about progress through robot trajectories using visual in-context examples.
5. Sydney Runkle,[The Art of Loop Engineering](https://www.langchain.com/blog/the-art-of-loop-engineering) , 2026. Frames agent reliability as the result of tool-use loops, verification loops, and outer improvement loops.
6. Jeremy Theocharis,[All You Need is Loops (and Humans)](https://www.theocharis.dev/blog/loops-are-all-you-need/) , 2025. Argues for giving LLMs a process and a definition of done rather than only a target output.
7. Xiaohan Wang et al.,[VideoAgent: Long-form Video Understanding with Large Language Model as Agent](https://arxiv.org/abs/2403.10517) , 2024. A closely related agentic frame-retrieval approach for long-form video understanding.
8. Xi Tang et al.,[Adaptive Keyframe Sampling for Long Video Understanding](https://arxiv.org/abs/2502.21271) , 2025. Studies selective keyframe retrieval for long-video understanding.
9. Shaojie Zhang et al.,[Q-Frame: Query-aware Frame Selection and Multi-Resolution Adaptation for Video-LLMs](https://arxiv.org/abs/2506.22139) , 2025. Focuses on preserving query-relevant evidence under frame limits.
10. Zeqian Li et al.,[Universal Video Temporal Grounding with Generative Multi-modal Large Language Models](https://arxiv.org/abs/2506.18883) , 2025. Directly relevant to timestamp localization from language descriptions.
11. Orr Zohar et al.,[Apollo: An Exploration of Video Understanding in Large Multimodal Models](https://arxiv.org/abs/2412.10360) , 2024. Discusses design choices such as video sampling and representation for video-LMMs.
12. Xingyi Zhou et al.,[Streaming Dense Video Captioning](https://arxiv.org/abs/2404.01297) , 2024. Frames dense video captioning as temporally localized event description over long videos.
13. Jade Choghari et al.,[The Path to Large Scale Dense Video Captioning](https://labs.scale.com/blog/path-to-large-scale-dense-video-captioning) , 2026. A practical manipulation-video captioning writeup emphasizing representation choices and visual grounding failures.
14. Kanata Suzuki et al.,[Proprioception Enhances Vision Language Model in Generating Captions and Subtask Segmentations for Robot Task](https://arxiv.org/abs/2512.20876) , 2025. Explores captioning and segmentation with robot motion/state evidence.
15. Dharmendra Sharma et al.,[RoboSubtaskNet: Temporal Sub-task Segmentation for Human-to-Robot Skill Transfer in Real-World Environments](https://arxiv.org/abs/2602.10015) , 2026. A robotics-focused reference on temporally grounded subtask segmentation.


‍
