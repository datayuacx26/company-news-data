---
schema_version: "1.0.0"
document_id: "de6ca8737ea992422bf8a69c986f72da94c237ff5c59ea2b808de61e24abc446"
company_key: "yc-canvas"
company: "Canvas"
source_id: "yc-canvas-news-import-8207a64dd502"
canonical_url: "https://www.canvas.inc/research/reward-models"
published_at: null
first_seen_at: "2026-08-09T20:23:24.393484+00:00"
fetched_at: "2026-08-09T20:23:25.334371+00:00"
content_hash: "sha256:94d467990283f974af44e3ee92faa7ae42b065f5fe86af080e6dea7230a25acc"
---

# Meta-Reward

## Motivation


The core challenge in agent post-training is defining a reward signal that captures the behavior we want the agent to learn. In domains with verifiable outcomes, this is relatively clean. Math solutions can be checked deterministically and code can be evaluated with executable tests. In cases where automatic verification isn't available, reward signals are often constructed from human judgment.


For long-horizon agent tasks, reward specification is harder because the reward must judge the full trajectory, not just the final response. A customer support, research, or workflow agent is evaluated by what information it gathered, which tools it called, what policy it applied, when it changed external state, and when it chose not to act.


Without accurate trajectory-level supervision, we risk rewarding the right outcome for the wrong reasons. For example, an agent might reach the correct final state through a lucky guess, unnecessary tool use, or an unauthorized action.


We observed this in τ³-airline as an **action bias** : the untuned judge often over-rewarded visible state-changing actions, like cancellations, compensation, and booking changes, even when policy required restraint.


Human annotation can provide this trajectory-level supervision, but labeling full agent traces is slow and expensive to scale. LLM judges offer a more scalable approximation. They can read agent traces, evaluate behavior against task criteria, and turn that judgment into a reward signal. In our previous[meta-agent](https://x.com/essamsleiman/status/2041224799746428944) work, we used LLM judges to score unlabeled agent traces during harness optimization.1 1


Sleiman et al.,[meta-agent: continual learning for agents](https://x.com/essamsleiman/status/2041224799746428944) (2026). Optimizes the agent's own harness from production traces using an LLM judge.


[Essam Sleiman @essamsleiman · Apr 6 meta-agent: continual learning for agents We built meta-agent: an open-source library that automatically and continuously improves agent harnesses from production traces. Point it at an existing agent, a stream of unlabeled production traces,…](https://x.com/essamsleiman/status/2041224799746428944)


But a judge call is not yet a reward procedure. Given a long trace and a rubric, the judge still has to infer what evidence matters, which constraints to prioritize, how to handle conflicting signals, and how to turn its reasoning into a score. Those choices determine what behavior gets rewarded. We call the system that specifies these choices the **evaluator harness** . It defines the trace view, policy context, checks, rubric, decision process, and scoring logic around the judge.


**meta-reward** optimizes the evaluator harness directly. Using a small set of trusted trajectory preferences from human annotation or task-specific labels, it tunes the evaluation procedure so the judge's scores better align with trusted preferences and generalize to unseen trajectories.
