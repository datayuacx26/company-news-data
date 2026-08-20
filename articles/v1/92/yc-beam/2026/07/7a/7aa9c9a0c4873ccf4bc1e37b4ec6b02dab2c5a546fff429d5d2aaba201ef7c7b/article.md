---
schema_version: "1.0.0"
document_id: "7aa9c9a0c4873ccf4bc1e37b4ec6b02dab2c5a546fff429d5d2aaba201ef7c7b"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/sandboxes-reinforcement-learning"
published_at: "2026-07-05T22:13:32+00:00"
first_seen_at: "2026-07-22T21:28:59.137691+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:967a82ef5b406a04cd9b6b96f00914ccb0774c699cad3f26fa0beb7fbf938ae9"
---

# Sandboxes for Reinforcement Learning

[← All posts](https://www.beam.cloud/blog) Engineering


# Sandboxes for Reinforcement Learning


Tim Huynh


July 5, 2026


6 min read


A sandbox for reinforcement learning is an isolated environment where an agent acts, state advances, and a reward comes back — built so you can run thousands of copies at once without them touching each other. The working pattern: build the environment once, snapshot it, restore the snapshot into every rollout, and tear each one down when the episode ends. Here is that whole loop in code.


```text


```


Each` Sandbox(...).create()` call boots an isolated container in one to three seconds, every rollout starts from the identical snapshotted state, and the failed episodes can't corrupt the ones running next to them. Swap the toy episode for your real task — a coding problem, a terminal session, a tool-use trace — and this is the shape of a production rollout worker.


## Why RL needs sandboxed environments


An RL training loop is a factory for experience. Every policy update consumes a batch of rollouts, so the loop runs the same environment thousands of times per step, scores each run, and starts over. That workload has three properties that rule out running episodes as bare processes on the training box.


- **The actions are untrusted.** In LLM and agent RL, the policy writes code, runs shell commands, and edits files. Mid-training, much of that output is garbage — infinite loops, deleted directories, requests to places it shouldn't reach. Isolation is not optional when the thing generating the actions is the thing you're still training.
- **Episodes must start identical.** A reward is only a clean signal if every rollout begins from the same state. One episode leaking a file or a background process into the next skews the batch, and you'll spend days finding out why training curves wobble.
- **The volume is brutal.** A single GRPO or PPO sweep can burn through tens of thousands of episodes. Environments have to spin up, run, and disappear constantly, and any per-episode setup cost — reinstalling dependencies, re-cloning repos — multiplies by the episode count.


A sandbox handles all three: hard isolation around untrusted actions, snapshot-perfect resets, and churn measured in seconds rather than minutes.


## How to run RL environments in sandboxes


The pattern from the hero snippet breaks into five steps. The guiding idea is to pay every setup cost once, then clone.


1. **Build the environment image.** Put everything an episode needs — Python packages, the repo under test, datasets, a running service — into a sandbox and configure it with` process.run_code` or` process.exec` . This is your slow step, and you only run it once.
2. **Snapshot the filesystem as a template.**` create_image_from_filesystem()` captures the prepared state and returns an image ID. Restoring from it with` Image.from_id(template)` gives every rollout the same starting point without re-running setup. For environments where warm process state matters — a server already listening, a REPL mid-session —` snapshot_memory()` goes further and captures running processes and exposed ports, restored with` create_from_memory_snapshot()` . The[stateful sandbox guide](https://www.beam.cloud/blog/best-stateful-sandbox-code-execution-2026) covers when each snapshot type earns its keep.
3. **Restore one sandbox per rollout.** Each episode gets its own container. Concurrency is a thread pool and a loop; there's no scheduler to build, because the platform is the scheduler.
4. **Run the episode and parse the reward.** The agent's actions execute inside the sandbox —` run_code` for Python,` exec` for shell — and the reward comes out however you define it: a printed score, a test suite's exit status, the diff between expected and actual files pulled back with` fs.download_file` .
5. **Terminate and churn.**` terminate()` ends the episode. For long-horizon environments that should outlive a single request — a persistent game server, a multi-day training curriculum — set` keep_warm_seconds=-1` and shut them down explicitly.


When the environment itself needs acceleration — a reward model scoring outputs, an agent calling a local model, a physics simulator — give the sandbox a GPU at creation:` Sandbox(cpu=4.0, memory="16Gi", gpu="A10G")` . The[serverless GPU](https://www.beam.cloud/blog/serverless-gpu) economics matter here, because an accelerator attached to every rollout is where sweep budgets go to die: on Beam an A100 80GB is $1.30/hr and an H100 is $1.74/hr, billed per second, so a thirty-second episode costs a fraction of a cent in GPU time.


## Where the reward signal comes from


The sandbox isn't just where the agent acts — for most modern setups it's also where the reward is computed. Three patterns cover nearly everything.


- **Verifier rewards.** Run the agent's output against ground truth inside the environment: unit tests pass or fail, the compiler accepts or rejects, the answer matches. This is the mechanism behind reinforcement learning with verifiable rewards (RLVR), the recipe that trained the current generation of reasoning models. The sandbox is the verifier — it executes the code and the tests, and the binary outcome is the reward.
- **Reward models.** A learned scorer runs inside the environment on its own GPU and rates each trajectory. Keeping it in the sandbox avoids a network round-trip on every step of every episode.
- **State scoring.** Score the environment itself after the episode: did the file end up correct, did the service stay healthy, how far did the game state advance. Pull artifacts out with the filesystem API and compute the reward in your training loop.


Mixing patterns is normal — a verifier for correctness plus a reward model for style is a common combination in code RL.


## Choosing infrastructure for RL sandboxes


There are four broad ways to run RL environments, and the honest answer is that the right one depends on your scale and what you already operate.


Approach Isolation Parallelism ceiling GPU in the environment Ops burden


Bare processes + nsjail on the training box Process-level One machine Shares the trainer's GPUs Low at first, grows fast


Kubernetes pods Container Your cluster size Yes, via device plugins You run the cluster


Managed sandbox platform gVisor / microVM Thousands of concurrent runs Platform-dependent Low


RL environment hubs Inherited from the backing sandbox Varies Varies Low, less control


At small scale — one machine, CPU-only environments, a research prototype — bare processes with nsjail are fine, and adding a platform would be ceremony. Kubernetes makes sense if your team already runs a cluster and wants rollouts on hardware you own. The managed platforms earn their place when the rollout count outgrows a machine but you don't want to staff an infra team for the loop; they differ sharply on GPU support, session limits, and whether you can run them in your own cloud — E2B, for instance, offers no GPUs and caps sessions at 24 hours, while Beam runs GPU sandboxes with no session cap and an open-source runtime you can[self-host or run in your own VPC](https://www.beam.cloud/blog/how-to-self-host-code-sandbox) . The full field — Beam, Northflank, Modal, E2B, Daytona — is ranked criterion by criterion in the[best sandbox providers for reinforcement learning](https://www.beam.cloud/blog/best-sandbox-providers-reinforcement-learning-2026) guide.


## FAQ


**How do you reset an RL environment between episodes?** Don't reset — replace. Restoring a fresh sandbox from a filesystem snapshot is faster and stricter than cleaning up a used environment, because there's nothing to clean: every episode starts from the identical captured state, and the used sandbox is simply terminated. In-place resets always eventually leak state; snapshot restores can't.


**Can the environment keep state across steps within an episode?** Yes. A sandbox persists for as long as the episode runs — files written in step three are there in step ten, background processes stay up, and a multi-turn agent can work in the same container throughout. State isolation applies between rollouts, not within one. For environments that need to live past a single session, disable the idle timeout and terminate explicitly.


**Can I run Gymnasium or custom Python environments in a sandbox?** Yes — a sandbox is a container, so anything pip-installable runs. Bake` gymnasium` and your environment package into the image, then either run whole episodes inside the sandbox and return the total reward, or expose the environment's step/reset interface over a port and drive it remotely from your trainer.


**Do rollouts have to run next to the training job?** No. Rollouts and policy updates communicate through small payloads — trajectories and rewards — so the environments can run on separate infrastructure from the trainer, or even in a different cloud. The exception is tight online loops where per-step latency matters; there, keep rollouts in the same region as the learner, or run both in your own account.


Run your first rollout in a couple of minutes.[Get started free on Beam](https://www.beam.cloud/?utm_source=usecase&utm_campaign=sandboxes-reinforcement-learning) — new accounts include $30 in monthly credit.


Tim Huynh


Published July 5, 2026


Keep Reading


## More from the Beam blog


[Engineering Tinker Model Pricing: What Fine-Tuning Costs in 2026 See what fine-tuning costs on Tinker, including worked cost examples and where renting GPUs gets cheaper. Tim Huynh](https://www.beam.cloud/blog/tinker-model-pricing)[Engineering What Is a Container, Really? Five Years of GPU Infrastructure Five years of GPU infrastructure at Beam — from ECS and Knative cold starts to a custom container runtime, FUSE lazy-loading, and a trustless binary. Luke Lombardi](https://www.beam.cloud/blog/what-is-a-container-really)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
