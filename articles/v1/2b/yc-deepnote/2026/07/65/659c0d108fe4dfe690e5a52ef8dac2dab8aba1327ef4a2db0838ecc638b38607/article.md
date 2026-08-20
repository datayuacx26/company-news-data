---
schema_version: "1.0.0"
document_id: "659c0d108fe4dfe690e5a52ef8dac2dab8aba1327ef4a2db0838ecc638b38607"
company_key: "yc-deepnote"
company: "Deepnote"
source_id: "yc-deepnote-news-import-1c599340c4cd"
canonical_url: "https://deepnote.com/blog/atomic-unit-for-reinforcement-learning"
published_at: null
first_seen_at: "2026-07-21T16:01:05.618995+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:6197f0803f3a5702a2d397ff9462afe993bfb4c6f43169480866db8ad2c1bbac"
---

# Data notebooks as the atomic unit for Reinforcement Learning

[← Back to all posts](https://deepnote.com/blog)


Reinforcement learning becomes central once a model is expected to act, not just generate. Real tasks are sequential decision processes with tool calls, partial observability, and long horizons. RL-based alignment work frames this as an optimization loop dependent on interaction data, reward signals, and reliable evaluation.


For agentic LLM training, the execution environment must be both safe to explore and auditable at scale. Treat a notebook as the unit of work *and* the unit of record:


- Sandbox = safety boundary (what can run, where, with what privileges)


- Notebook = execution trace artifact (code + tool calls + outputs + provenance + collaboration medium for agents)


This becomes the practical abstraction for: (1) RL rollouts, (2) eval harness runs, (3) world-model trajectory collection, and (4) multi-agent swarm simulations.


## **Why RL for agentic LLMs needs more than prompts and sandboxes**


Reinforcement learning becomes central once a model is expected to act, not just generate. Real tasks are sequential decision processes with tool calls, partial observability, and long horizons. RL-based alignment work frames this as an optimization loop dependent on interaction data, reward signals, and reliable evaluation.[Research that interleaves reasoning traces with environment actions](https://arxiv.org/abs/2210.03629) positions agentic behavior as an interaction problem, not a text-generation problem.


The shift is already visible in shipped production agents. Ramp's recent post-training of a 35B-A3B Qwen variant for spreadsheet retrieval is a clean example. They scoped a narrow, verifiable subtask (navigate the workbook, return the cell), built a synthetic environment with adversarial decoy sheets and obfuscated invoice identifiers, and trained against a deterministic reward where correctness dominated and smaller shaping terms favored efficient and concise trajectories. The resulting "Fast Ask" model beat Claude Opus 4.6 on exact-match accuracy by 4 points at roughly Haiku 4.5 latency, while reducing average completion time. What made this possible was not a sandbox. It was an environment that bound code execution, tool calls, dataset snapshots, and a verifier into one reproducible artifact per rollout.


That artifact is what a notebook should be.


A working RL environment in this era has three load-bearing pieces: the **task distribution** (inputs the agent sees), the **harness** (tools, state, observation interface, turn budget), and the **verifier** (what counts as success, expressed as a reward). Recent practitioner guides converge on this taxonomy and on a verifier-first authoring discipline: a small task set with a reliable verifier produces more learning signal than a large set with weak scoring. They also converge on atomic tool design, where harnesses expose small, composable surfaces (read, write, edit, bash, plus subagent and MCP primitives) instead of long enumerated APIs. Notebooks have an underappreciated property here: the verifier is just a cell, atomic tools are cell-level imports, and both are inspectable and version-controlled alongside the trajectory they shaped.


The generation-verification gap matters too. Producing candidate outputs with an LLM is cheap; deciding whether they actually solved the task gets harder as tasks open up (code with passing tests is easy, deep research is not). Programmatic checks beat LLM-as-judge wherever they fit, and a substrate that preserves intermediate state, not just final answers, gives the verifier more to grade.


Benchmarks and harnesses are converging on the same shape.[SWE-bench evaluates](https://www.swebench.com/SWE-bench/guides/evaluation/) by applying model-generated patches and running repository tests in a containerized harness.[OpenAI Evals](https://evals.openai.com/) and[EleutherAI's lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) exist precisely to standardize task definitions paired with infrastructure that runs them reliably. Arena reports large gaps between strong agents and humans on end-to-end web tasks, which only widens the case for a substrate that captures full trajectories rather than final answers.


## **Why data notebooks are a better primitive than sandboxes alone**


A sandbox is an isolation and control boundary. It limits filesystem, network, and syscall surface so trial-and-error exploration is safe enough to run at scale. Modern code-execution tools achieve this with hardened containers (gVisor's application-kernel architecture) or lightweight virtualization (Firecracker microVMs).


A notebook is execution *plus* a system of record. RL and large-scale evals for LLM agents need more than a place to run code. They need:


- Structured binding between an execution and the exact data, parameters, tools, and environment versions used


- A first-class artifact that captures the full trajectory: intermediate computations, tool outputs, validations, failure modes


- A narrative layer that makes runs auditable by humans (debugging, RLHF) and machine-usable (training signals, world-model learning, reward-model audits)


The` .deepnote` bundle should be the canonical episode container: a YAML project (optionally multi-notebook) that encodes the runnable spec (environment at project level, dependency/reactivity graph, cell IDs) and immutable provenance pointers (dataset snapshot IDs, model/policy version, connector identity and policy), with credentials excluded. Each execution produces a snapshot` .deepnote` that is runnable and contains outputs, logs, and metrics, enabling deterministic replay against the same snapshots and policies.


Each run also emits structured sidecars for training and eval ingestion:


- ` traces` : ordered tool calls, code execution steps, observations, timings, state deltas


- ` rewards` : verifier results, reward components, constraint flags


- ` artifacts` : inputs/outputs, write-sets, artifact URIs, dataset snapshot IDs


Humans inspect the snapshot notebook. RL and world-model pipelines consume the sidecars at scale. The format is parameterizable and executable programmatically; the executed notebook is itself the full record.


This composition maps onto what the field already uses. The[verifiers](https://pypi.org/project/verifiers/) library structures an environment as four LEGO-style blocks: dataset, parser, rubric, rollout. OpenEnv exposes a` reset/step/state` interface that training frameworks consume. A` .deepnote` bundle subsumes both: the project encodes the rollout harness and the rubric; the snapshot is one episode of` reset → step × N → state` ; the sidecars are the dataset hook, parser output, and rubric scores in standardized JSONL. The goal is interoperability, not reinvention. Trainers like prime-rl and distribution layers like Environments Hub should be able to ingest a notebook execution the same way they ingest a Python-only verifier package, provided the sidecar contracts are stable.


## **World models need trajectories, and notebooks are how you capture them**


World models in RL are learned models of environment dynamics: compressed state representations plus predictive transition structure. Classic work demonstrated learning generative world models, training policies inside dreamed rollouts, and transferring back to the real environment. Modern model-based RL continues this with Dreamer-style methods and[MuZero-style representations optimized for planning](https://arxiv.org/abs/2301.04104?utm_source=chatgpt.com) .[Safety-focused RL uses world models](https://arxiv.org/abs/2307.07176?utm_source=chatgpt.com) to improve sample efficiency and reduce unsafe exploration.


For[LLM agents](https://arxiv.org/abs/2602.05842?utm_source=chatgpt.com) , "state" is text plus tool state, and "dynamics" include APIs, web pages, databases, code execution results, and multi-step workflows. Recent work argues that LLM agents struggle to anticipate action consequences, and proposes explicit world-model learning signals derived from the discrepancy between an internal simulated next-state and the realized next-state.


A world model is only as good as its trajectories. For tool-using agents, the desired training data is not input-output pairs. It is full interaction traces:


- The observation presented to the agent


- The action taken (tool call, code execution step, query)


- The returned observation


- Derived signals (success checks, reward proxies, constraint violations)


ALFWorld and Arena highlight that interactive multi-step environments are crucial and success depends on action sequences, not one-shot answers. A notebook execution record aligns with the trajectory concept natively: each cell is a step or bundle of steps, outputs capture observations, and metadata stores structured action logs.


## **Architecture for notebook-based agent sandboxes at scale**


The claim that "RL and evals can run in a notebook" is true the same way "software engineering evals can run in a container" is true. The notebook is not the entire system. It is the unit of execution and the primary artifact, provided the surrounding platform supplies sandboxing, orchestration, and data governance.


### **Execution engine for notebooks as batch jobs**


A headless notebook execution layer that can inject parameters, execute deterministically (or at least reproducibly), and emit an executed notebook artifact plus structured logs. Conversion and export round out reviewer workflows. Deepnote already supports this.


### **Orchestration and horizontal scaling**


A scheduler that treats each notebook execution as a job and runs many concurrently. Kubernetes Jobs are defined for one-off tasks that run to completion. Argo Workflows handles DAG-style pipelines and parallel sweeps. Ray provides a Python-native distributed runtime via tasks and actors. JupyterHub on Kubernetes covers the multi-user interactive case with autoscaling and idle-server culling.


For RL specifically, async off-policy training (as in Prime Intellect's prime-rl) decouples rollout generation from policy updates. Rollout workers keep generating trajectories while the trainer updates weights, with importance weighting correcting for bounded staleness. The orchestration layer needs to support this rollout-trainer split, where each rollout is a notebook execution and the trainer consumes sidecar artifacts asynchronously.


### **Strong isolation, policy enforcement, and safe tool access**


Notebook execution for agent training is inherently risky. The notebook runs model-recommended code and invokes tools on real systems. Architecturally:


- Hardened container boundaries, optionally with an additional sandbox layer (gVisor)


- Or microVM isolation (Firecracker) for stronger tenant separation


- Network egress policy and secrets isolation


- Explicit action gating at the product-policy layer, matching "sandbox plus approvals" patterns


### **Data plane: versioned datasets, controlled connectors, reproducibility**


A data agent is only as valuable as its data access is reproducible. Required:


- Stable dataset snapshots or time-travel semantics (Delta Lake frames this as enabling rollbacks, audit trails, and reproducible ML experiments)


- Bounded, logged connectors to internal databases and external APIs


- Deterministic replay where possible


Run-level provenance (data version, parameters, code, artifacts) maps to MLflow, W&B Artifacts, and DVC. A notebook-centered design binds this natively: notebook metadata stores dataset snapshot IDs, connector policies, and run identifiers, while the executed notebook is the human-readable forensic record.


### **Trace capture and regression-friendly artifacts**


At scale, machine-readable traces matter more than human-readable ones. The executed notebook is a useful container, but standardized sidecars are required:


- Structured JSON logs of tool calls and outputs


- Timing and resource usage


- Reward and verifier outputs


- Diff-friendly changes


Notebook diffs are historically painful, but` nbdime` provides content-aware diffing and merging. Regression testing is evaluation over time, and diffable notebooks plus structured traces make it possible to pinpoint why success rates changed across model, environment, or policy versions.


### **Integration with eval harnesses and outcome verifiers**


Verifiers depend on the task. SWE-bench is the canonical case for code, using tests as verifiers in a containerized harness. OpenAI Evals supports custom evals on private data, and the Cookbook demonstrates running evals from a notebook directly. Notebook runs need to emit outputs in formats harnesses can ingest: numeric metrics, pass/fail signals, structured per-case logs.


### **Where a notebook substrate fits next to existing tools**


Existing layers cover slices of the problem. Gym/Gymnasium standardizes the` reset/step` API for classic RL but treats each environment as opaque Python. OpenEnv extends that contract to LLM agents and adds containerization, with wrappers for Terminal-Bench, browser automation, code execution, and embodied tasks. The verifiers library contributes the four-block composition and ships with Environments Hub for distribution. SWE-bench and Terminal-Bench focus on the verifier side, with reusable test harnesses for code and shell tasks. Prime-rl handles training, consuming rollouts produced by verifiers-style environments.


None of these natively solves: human-readable forensic record per episode, narrative documentation alongside the trajectory, deterministic replay against the same dataset snapshot, and interactive authoring by domain experts who are not Python engineers. That gap is where a notebook substrate sits. The notebook is the artifact that wraps an environment, one execution, and its trace in a format a domain expert can read end-to-end. It composes with verifiers and OpenEnv rather than replacing them.


This is the architectural punchline of the Ramp case study. They wrote their own task generator, their own tool interface, their own reward function, their own verifier, and their own training harness. That stack is exactly what a notebook substrate is meant to absorb. The same post-training run, expressed as a parameterized` .deepnote` project with structured trace and reward sidecars, would have been a configuration of an existing environment rather than a bespoke project. As small, verifiable subagents become a more common pattern (retrieval, classification, structured extraction, narrow tool-use loops), the marginal cost of building each one needs to drop. That is the workload the notebook-as-atomic-unit thesis is targeting.


## **Agent swarms and collaborative traces inside notebooks**


Real workflows are collaborative: different roles, different tools, async handoffs, partial visibility. AutoGen positions itself around multi-agent conversations and tool use as first-class. Tau²-bench focuses on collaborative dynamics and reports that performance drops substantially when agents move from solo to interactive settings.


A notebook substrate represents swarms in two patterns:


- One notebook per agent per episode, with a coordination service linking them via shared state and cross-notebook references


- One notebook per episode with multiple agent transcripts, organizing the timeline (and potentially different kernels) while emitting structured logs per agent


In both cases, the notebook's narrative layer is the audit surface. A human can inspect what each agent did, what tools were used, what data was retrieved, and which checks passed. Swarmness is a coordination and trace problem, not a UI problem. The notebook is the standardized trace artifact for each role; the orchestration layer schedules and wires up the swarm.


## **Failure modes and guardrails for a notebook-first RL/eval stack**


Notebooks without guardrails undermine reproducibility and safety.


**Hidden state and non-reproducible execution order.** Motivates headless execution, deterministic parameters, and tooling that assesses or restores notebook reproducibility.


**Security.** An executable notebook with network access or secrets is an exfiltration vector. Hence sandbox modes, approval policies, and hardened isolation layers like gVisor or Firecracker.


**Evaluation validity.** RL systems exploit reward loopholes; agent systems overfit to benchmark quirks. Recent world-model proposals for LLM agents emphasize alignment between simulated and realized states as a more robust training signal than brittle token-level next-state prediction.


**Contamination resistance.** As environments get reused across labs and benchmark cycles, training data leaks back into evaluation. Practitioner guides treat contamination resistance as a design requirement: stable dataset snapshot IDs in provenance, explicit splits per release, and verifiers that can detect memorized outputs rather than only correct ones. Notebook bundles with immutable provenance pointers are well-positioned here, since the snapshot ID lives inside the artifact rather than in a separate ops layer.


The guardrails implied by the literature are consistent:


- Sandbox plus explicit action policy


- Deterministic, parameterized notebook execution


- Trajectory capture with structured logs and versioned data snapshots


- Evals built around verifiable outcomes (tests, state checks, task success metrics), not text similarity


These are platform requirements, not notebook features. They are what make notebooks a viable substrate for RL training and evaluation at scale.


Jakub Jurovych


CEO @ Deepnote


Follow Jakub on[LinkedIn](https://www.linkedin.com/in/jakubjurovych/)
