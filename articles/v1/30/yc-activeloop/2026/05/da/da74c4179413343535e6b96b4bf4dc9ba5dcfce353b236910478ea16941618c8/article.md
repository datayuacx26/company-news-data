---
schema_version: "1.0.0"
document_id: "da74c4179413343535e6b96b4bf4dc9ba5dcfce353b236910478ea16941618c8"
company_key: "yc-activeloop"
company: "Activeloop"
source_id: "yc-activeloop-news-import-11accf1de1c3"
canonical_url: "https://deeplake.ai/blog/agentfield"
published_at: "2026-05-30T00:00:00+00:00"
first_seen_at: "2026-07-24T14:16:27.843975+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:1d96f94ff9d79e2f35ce551eba8b35fa2d82ab50320b763b3c40f9a0d6b2e15d"
---

# A Deployable Annotation Service for Robotics Datasets

Robotics teams are collecting more multimodal demonstrations than ever, but turning those demonstrations into train-ready datasets still requires a large amount of manual interpretation. A raw episode may contain images, states, actions, timestamps, and a short task instruction, yet the fields that make the data useful for training and debugging often live outside the dataset: goals, phases, segment boundaries, quality signals, modality disagreements, and review status.


Roboscribe-AF shows how Deeplake and AgentField can be combined into a practical annotation layer for robotics data. Deeplake provides the versioned multimodal dataset layer, while AgentField runs the reasoning workflows that inspect episodes, create structured annotations, flag uncertainty, and write results back into dataset branches. The result is a deployable pattern for transforming robot demonstrations into queryable, reviewable, and train-ready data.


---


Robot datasets increasingly need more than a task string.


[Open X-Embodiment](https://arxiv.org/abs/2310.08864) showed the value of pooling robot demonstrations across embodiments and tasks.[OpenVLA](https://arxiv.org/abs/2406.09246) trains a vision-language-action policy on robot demonstration data.[ECoT](https://arxiv.org/abs/2407.08693) goes further and trains VLAs to reason over intermediate plans, sub-tasks, motions, object boxes, and end-effector positions before predicting actions.


That literature points to a practical gap. Many demonstrations arrive as video, state, action, timestamps, and a sparse instruction. The useful training record often wants richer fields: episode goal, segment boundaries, phase labels, modality agreement, anomaly flags, and review status.


[Roboscribe-AF](https://github.com/Agent-Field/roboscribe-af) is our open-source example of that middle layer: a deployable annotation service built from two infrastructure pieces.


Layer Role


[Deeplake](https://docs.deeplake.ai/latest/core/) Multimodal dataset, branches, tensors, embeddings, queries


[AgentField](https://github.com/Agent-Field/agentfield) Reasoners, deterministic skills, async execution, workflow trace


The separation matters.[Deeplake](https://docs.deeplake.ai/latest/core/) stores the corpus and annotation versions.[AgentField](https://github.com/Agent-Field/agentfield) runs the reasoning graph that produces new annotation rows.


## The annotation


For each episode,[Roboscribe-AF](https://github.com/Agent-Field/roboscribe-af) loads keyframes and action/state trajectories, runs a visual thread and an action thread, segments the episode, checks whether the modality stories agree, embeds the scene summary, and writes the result to a Deeplake branch.


The output is deliberately plain:


json


```text
{
"episode_id"  :   0  ,
"episode_goal"  :   "Push the gray T-shaped block into the green target outline"  ,
"segments"  : [
{  "start_frame"  :   0  ,   "end_frame"  :   21  ,   "phase"  :   "approach"  },
{  "start_frame"  :   21  ,   "end_frame"  :   40  ,   "phase"  :   "manipulate"  }
],
"visual_phase"  :   "manipulate"  ,
"action_phase"  :   "approach"  ,
"consistency_score"  :   0.2  ,
"human_review_recommended"  :   true
}
```


The mismatch is treated as data. The visual reasoner and trajectory reasoner stay separate until a verifier compares them. If they disagree, the disagreement becomes a dataset field that can be queried, reviewed, or filtered.


## The data layer


[Deeplake](https://docs.deeplake.ai/latest/core/) 's documented surface covers the artifact we need to store: images, embeddings, tensors, text, vector search, versioning, and PyTorch/TensorFlow streaming ([core docs](https://docs.deeplake.ai/latest/core/) ). Its LeRobot guide shows robot telemetry, frames, state/action arrays, episode indices, and task descriptions as queryable, streamable data ([LeRobot integration](https://docs.deeplake.ai/4.5/examples/lerobot-integration/) ). Its VLA guide uses data stored in Deeplake for fine-tuning ([VLA fine-tuning](https://docs.deeplake.ai/4.5/examples/lerobot-finetuning/) ).


[Roboscribe-AF](https://github.com/Agent-Field/roboscribe-af) keeps raw and derived fields in the same schema:


python


```text
{
"episode_id"  : Int32,
"keyframes_png"  : Sequence(Bytes),
"actions"  : Array(Float32,   2  ),
"states"  : Array(Float32,   2  ),
"lang_episode_goal"  : Text,
"visual_phase"  : Text,
"action_phase"  : Text,
"consistency_score"  : Float32,
"human_review_recommended"  : Bool,
"scene_embedding"  : Embedding(  size  =  1024  ),
"annotation_version"  : Text,
}
```


That makes branch-level annotation practical. Raw data can remain on` main` ; a first annotation pass can write to` roboscribe-v1` ; a stricter verifier can write to` roboscribe-v2` ; a reviewed subset can become a train-ready branch.


It also keeps queries close to training decisions:


sql


```text
SELECT   episode_id, lang_episode_goal, consistency_score
WHERE   visual_phase   =   'manipulate'   AND   consistency_score   >   0  .  6
```


## The execution layer


The service is complex at runtime but small in code shape.[Roboscribe-AF](https://github.com/Agent-Field/roboscribe-af) registers 16 reasoners and 8 skills. A corpus run fans out into loaders, per-keyframe object detectors, scene reasoners, action reasoners, boundary judges, segment narrators, verifiers, embedding calls, Deeplake writes, and branch comparisons.


The developer surface is just named units:


python


```text
@router.skill  ()
async   def   commit_annotation_to_branch  (...):
...


@router.reasoner  ()
async   def   visual_thread  (...):
...
```


Skills do deterministic work: load frames, compute velocity summaries, query Deeplake, write branches. Reasoners do model-backed judgment: detect objects, classify phases, judge boundaries, reconcile modalities.[AgentField](https://github.com/Agent-Field/agentfield) exposes both as callable targets and tracks parent-child executions through its control plane, as described in its[how-it-works docs](https://agentfield.ai/docs/learn/how-it-works) .


The fan-out remains ordinary Python:


python


```text
visual, action   =   await   asyncio.gather(
composer_router.call(  "roboscribe-af.visual_thread"  ,   keyframes_b64  =  frames),
composer_router.call(  "roboscribe-af.action_thread"  ,   states  =  states),
)
```


That is the useful property: the deployed system has a real workflow graph, async API, and UI trace, but the implementation is still a set of small domain functions.


## What exists now


The repository currently includes:


- PushT and Aloha-style task adapters.
- Parallel visual and action modality threads.
- Segment narrator fan-out based on detected segment count.
- Cross-modal consistency scoring.
- Deeplake ingestion and annotation branch writes.
- TQL examples, semantic search over scene embeddings, and branch comparison.
- Docker Compose deployment for the AgentField control plane and[Roboscribe-AF](https://github.com/Agent-Field/roboscribe-af) agent service.


Run it locally:


bash


```text
cd   code/examples/roboscribe-af
cp   .env.example   .env
# Add OPENROUTER_API_KEY.
docker   compose   up   --build
./scripts/run_demo.sh
```


During the run,[AgentField](https://github.com/Agent-Field/agentfield) shows the reasoning DAG. Deeplake holds the resulting branch.


## Where this pattern goes


In a robotics lab, the same architecture becomes a data engine rather than a one-off annotator.


First, reactive annotation.[AgentField](https://github.com/Agent-Field/agentfield) documents webhook triggers, schedules, memory triggers, async execution, and workflow DAGs in its[production capabilities](https://agentfield.ai/docs/learn/features) . A lab can ingest new teleoperation episodes into Deeplake, trigger an annotation worker, write low-confidence rows to a review queue, and promote approved rows into a train-ready branch.


Second, curation.[BridgeData V2](https://arxiv.org/abs/2308.12952) and[Open X-Embodiment](https://arxiv.org/abs/2310.08864) are reminders that scale and diversity matter, while[Re-Mix](https://arxiv.org/abs/2408.14037) and recent work on[demonstration curation](https://arxiv.org/abs/2603.09056) point toward selecting better training mixtures rather than treating every trajectory equally. An[AgentField](https://github.com/Agent-Field/agentfield) reasoner can query Deeplake for phase disagreements, rare tasks, unusual action embeddings, or weakly represented environments and create a curation queue.


Third, lab automation. A robotics lab already has events: a teleop session finished, a nightly training run failed, an eval policy regressed on contact-rich tasks, a reviewer approved a batch. Those events can become backend triggers. Deeplake holds the versioned data state;[AgentField](https://github.com/Agent-Field/agentfield) runs the small pieces of reasoning and bookkeeping around it.


The broader pattern is not a shared chatbot sitting beside the dataset. It is background agents attached to the pipeline itself: guided by schemas, branches, triggers, and review policies; autonomous enough to inspect new data, enrich it, flag uncertainty, and prepare train-ready branches without waiting for a human to manually query every corpus change. Deeplake provides governed data access and versioned state.[AgentField](https://github.com/Agent-Field/agentfield) turns that access into autonomous background work.


---


Roboscribe-AF is a small example, but the pattern is broader: robotics datasets should not be passive storage buckets. As robot learning pipelines scale, the dataset layer needs to support continuous enrichment, review, curation, and promotion of higher-quality training subsets.


Deeplake provides the versioned multimodal foundation for that workflow. AgentField adds the execution layer for background reasoning, deterministic data operations, workflow tracing, and review-aware automation. Together, they make it possible to build robotics data pipelines where new episodes can be ingested, annotated, checked, queried, and promoted without turning every dataset update into a manual labeling project.


For labs building vision-language-action systems, this turns annotation from a one-off preprocessing step into an operational loop: collect data, enrich it, inspect disagreements, curate useful subsets, and keep the training corpus aligned with the realities of the robot pipeline.


---


## References


- [Open X-Embodiment: Robotic Learning Datasets and RT-X Models](https://arxiv.org/abs/2310.08864)
- [OpenVLA: An Open-Source Vision-Language-Action Model](https://arxiv.org/abs/2406.09246)
- [Robotic Control via Embodied Chain-of-Thought Reasoning](https://arxiv.org/abs/2407.08693)
- [BridgeData V2: A Dataset for Robot Learning at Scale](https://arxiv.org/abs/2308.12952)
- [Re-Mix: Optimizing Data Mixtures for Large Scale Imitation Learning](https://arxiv.org/abs/2408.14037)
- [Quality over Quantity: Demonstration Curation via Influence Functions for Data-Centric Robot Learning](https://arxiv.org/abs/2603.09056)
- [Deeplake Core docs](https://docs.deeplake.ai/latest/core/)
- [Deeplake LeRobot integration](https://docs.deeplake.ai/4.5/examples/lerobot-integration/)
- [Deeplake VLA fine-tuning with LeRobot data](https://docs.deeplake.ai/4.5/examples/lerobot-finetuning/)
- [AgentField: How it works](https://agentfield.ai/docs/learn/how-it-works)
- [AgentField production capabilities](https://agentfield.ai/docs/learn/features)
