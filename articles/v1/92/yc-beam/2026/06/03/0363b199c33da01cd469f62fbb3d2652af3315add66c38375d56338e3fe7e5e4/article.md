---
schema_version: "1.0.0"
document_id: "0363b199c33da01cd469f62fbb3d2652af3315add66c38375d56338e3fe7e5e4"
company_key: "yc-beam"
company: "Beam"
source_id: "yc-beam-news-import-8ae061011ee3"
canonical_url: "https://www.beam.cloud/blog/how-to-self-host-code-sandbox"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-24T09:37:22.766203+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:1fed35b004eea43b8e7b2ec965b5ab8d462f879116633f0ba28840f4ca702957"
---

# How to Self-Host a Code Execution Sandbox for AI Agents (2026)

[← All posts](https://www.beam.cloud/blog) Tutorials


Engineering


# How to Self-Host a Code Execution Sandbox for AI Agents (2026)


Hassaan Qadir


June 16, 2026


10 min read


When an AI agent runs model-generated code, that code executes somewhere. If "somewhere" is a managed SaaS sandbox, you are sending potentially sensitive code, data, and credentials to a third party — and paying their per-second rate, inside their session limits, on their hardware. Self-hosting a sandbox puts the execution layer inside your own perimeter: your VPC, your compliance boundary, your GPU reservations, your cost basis.


This guide covers the practical options for self-hosting a code execution sandbox in 2026 — starting with Beam, whose open-source` beta9` runtime gives you the same sandbox API self-hosted or managed — and walks through what self-hosting actually requires before you commit to one.


## Key Takeaways


- **Self-hosting is mostly an isolation-plus-orchestration problem, not a "run a container" problem.** You need per-execution isolation, a scheduler, persistent storage, and a network egress policy. Beam packages all four into one AGPL-3.0 runtime (` beta9` ) you can deploy with Helm.
- **The strongest isolation is a microVM (its own kernel); the most operable is a mediated user-space kernel.** E2B's self-host stack uses Firecracker microVMs; Beam uses gVisor + runc, trading a slice of VM-level separation for faster custom-image starts and simpler operations.
- **"Self-hostable" ranges from \`docker run\` to a full Nomad cluster.** Microsandbox runs locally with no server; Beam and Daytona deploy into Kubernetes; E2B's` e2b-dev/infra` is a Terraform/Nomad/Consul deployment that expects infrastructure expertise.
- **If you need GPUs in the sandbox, the field narrows fast.** Beam and Daytona support GPU workloads self-hosted; most lightweight sandboxes (Microsandbox, DifySandbox, Judge0) do not.
- **Self-hosting is how you turn the vendor's price floor into a price ceiling.** Run Beam's runtime on AWS/GCP/Azure/Hetzner credits you already hold and the managed per-second rate becomes the most you would ever pay, not the least.


## What "self-hosting a sandbox" actually requires


Before comparing tools, it helps to name the four things every production sandbox platform has to solve. These are the evaluation criteria used throughout this guide.


- **Isolation model.** How is one execution kept from reading another's memory, escaping to the host, or exhausting shared resources? Options, strongest to lightest: microVM (own kernel — Firecracker, libkrun) → user-space kernel (gVisor) → container (runc/Docker) → process isolation (seccomp + chroot, namespaces).
- **Orchestration.** Something has to schedule sandboxes onto hosts, enforce CPU/RAM/GPU limits, and reclaim them. This is the part teams underestimate — it is the difference between "I can boot a microVM" and "I run 200 concurrent sandboxes with autoscaling."
- **Storage.** Where do the filesystem, uploaded files, and (if supported) snapshots live? Production deployments back this with object storage (S3-compatible) rather than local disk so sandboxes are not pinned to one node.
- **Networking and egress.** Model-generated code will try to reach the internet. You need per-sandbox egress controls, port exposure for previews, and a way to reach internal services without opening the host.


A self-hosted sandbox is only as good as its weakest layer. A microVM with no egress policy is still a data-exfiltration risk; perfect isolation with no orchestration does not scale past a demo.


## The options, ranked


### 1. Beam (` beta9` )


**The same sandbox API, self-hosted or managed.** Beam's runtime,[\`beta9\`](https://github.com/beam-cloud/beta9) , is open source under AGPL-3.0. You can run it locally, deploy it into your own Kubernetes cluster via Helm, or bring your own cloud — AWS, GCP, Azure, and Hetzner are supported, as is connecting your own hardware.


- **Isolation:** gVisor + runc — a user-space kernel intercepts guest syscalls before they reach the host. Stronger than a plain container; lighter than a full microVM.
- **Orchestration:** Built in. The runtime schedules sandboxes, autoscales, and reclaims them — this is what` beta9` *is* , not an add-on.
- **Storage:** S3-compatible object storage (JuiceFS-backed distributed filesystem), so sandboxes are not pinned to a single node.
- **GPU:** Yes — the same runtime that serves managed GPU workloads runs self-hosted (your own GPU nodes or cloud GPU instances).
- **License / cost:** AGPL-3.0; self-host for free, or use the managed cloud. BYOC means you spend on your own cloud credits, not a markup.


The reason Beam leads this list for self-hosting is the **API parity** : the code your agent runs is identical whether you point the SDK at Beam's managed cloud or your own self-hosted gateway. You prototype on managed, then move the same code in-house without a rewrite.


```text


```


**Best for:** teams that want one isolation + orchestration + storage + GPU stack they can self-host with Helm today and not re-platform later.


### 2. E2B (` e2b-dev/infra` )


**The microVM purist's self-host — powerful, and operationally heavy.** E2B's SDK is Apache-2.0, and its infrastructure is genuinely open source at[\`e2b-dev/infra\`](https://github.com/e2b-dev/infra) , deployed with Terraform.


- **Isolation:** Firecracker microVMs — each sandbox boots its own kernel. This is the strongest isolation tier in this guide.
- **Orchestration:** Nomad + Consul. You are standing up and operating a scheduling cluster, not just a service.
- **Cloud support:** GCP is fully supported; AWS is in beta; Azure and bare-metal Linux are planned.
- **GPU:** Not supported in the sandbox runtime.
- **License:** Apache-2.0.


**Approach:** E2B optimizes for kernel-level isolation of adversarial code, and the self-host path reflects that — Firecracker, KVM, and a Nomad/Consul control plane. The README documents the setup, but expect a real infrastructure project: this is not a` helm install` .


**Best for:** teams that specifically require microVM (own-kernel) isolation, already run Nomad/Consul or are happy to, and primarily deploy on GCP.


### 3. Daytona


**Self-hostable, persistence-first.** Daytona is AGPL-3.0 and built around long-lived, pause/resume workspaces.


- **Isolation:** Docker container by default; Kata Containers or Sysbox can be opted in for microVM-level isolation (added deployment complexity).
- **Persistence:** Its core strength — pause/archive lifecycle with a persistent filesystem across stops.
- **GPU:** Supported (H100, RTX PRO 6000 on managed; GPU nodes self-hosted).
- **License:** AGPL-3.0.


**Best for:** agents that need a sandbox to *survive between turns* — persistent filesystem and processes you stop and resume — when you can accept Docker-default isolation or take on Kata/Sysbox.


### 4. Microsandbox


**The no-server option.** Microsandbox (Apache-2.0) runs locally and rootless using libkrun microVMs over KVM — each run gets its own kernel, with no control plane to operate.


- **Isolation:** libkrun/KVM microVM — strong, own-kernel.
- **Orchestration:** None required; it is local-first by design.
- **GPU:** Not documented.
- **Status:** Beta; expect breaking changes. The sub-100 ms boot claim is self-reported in the README with no published benchmark — verify before relying on it for an SLA.


**Best for:** strong microVM isolation on a single machine or developer laptop, with no cluster to run.


### 5. Judge0


**Self-host for grading, not agents.** Judge0 (GPL-3.0) executes code via the` isolate` binary (Linux namespaces + cgroups) inside a container that requires` --privileged` .


- **Isolation:** Process-level (namespaces + cgroups); shared host kernel; privileged Docker is a documented host-level risk (sandbox-escape CVEs were patched in v1.13.1, April 2024).
- **Languages:** 47 active languages — its real strength.
- **Limits:** Per-submission CPU caps (2–15 s), unsuitable for long-lived agent sessions.


**Best for:** competitive-programming, education, and automated grading — not general-purpose agent sandboxing.


### 6. DifySandbox


**The lightest self-hosted option.** DifySandbox (Apache-2.0) uses seccomp syscall whitelisting plus` chroot` , running as a` docker run` on Linux.


- **Isolation:** Process-level inside a shared, persistent container — lowest overhead, weakest separation. Not appropriate for adversarial or multi-tenant code.
- **Languages:** Python and Node.js only.
- **GPU:** None.


**Best for:** high-throughput, short-lived, trusted executions inside a self-hosted LLM workflow where per-task VM isolation is overkill.


## Self-hosting comparison


Tool Isolation (self-hosted) Orchestration you operate GPU Deploy method Setup weight


Beam (beta9) gVisor + runc Built in Yes Helm / local / BYOC Low–medium


E2B (infra) Firecracker microVM Nomad + Consul No Terraform High


Daytona Container (Kata/Sysbox optional) Built in Yes Self-host / managed Medium


Microsandbox libkrun/KVM microVM None (local) No Local binary Very low


Judge0 namespaces + cgroups (privileged) Minimal No Docker Low


DifySandbox seccomp + chroot None No docker run Very low


*All facts verified 2026-06-16; see Sources.*


## Why Beam stands out for self-hosting


### One runtime instead of four layered tools


The reason most "self-host a sandbox" projects stall is that teams assemble isolation, orchestration, storage, and GPU scheduling from separate pieces.` beta9` ships them together: it is the same engine Beam runs in production, released open source. You deploy one Helm chart, not a Nomad cluster plus an object-store integration plus a GPU operator.


### True bring-your-own-cloud


Self-hosting and BYOC are different things, and Beam supports both. You can run` beta9` entirely on your own hardware (air-gapped, on-prem), *or* run Beam's control plane against your AWS/GCP/Azure/Hetzner account so the workloads land on credits you already hold. \[docs.beam.cloud/v2/self-hosting, 2026-06-16\] The practical effect: the managed per-second price becomes a ceiling you can always undercut by running on your own committed-use discounts.


### GPU in the sandbox, not just CPU


If your agent runs inference *inside* the sandbox — a vision model, a local LLM, an embedding step — most self-hostable sandboxes simply can't help; they are CPU-only. Beam's self-hosted runtime schedules GPU workloads with the same API as CPU ones, so "give this sandbox an H100" is a parameter, not a separate system.


### No rewrite from prototype to production


Because the SDK is identical against managed Beam and a self-hosted gateway, the migration path is: build on managed, flip the connection target, run in-house. That de-risks the decision — you are not betting the project on the self-host working before you have written a line of agent code.


## A realistic self-host plan


- **1. Prototype on managed Beam** so the sandbox API and your agent logic are settled before any infrastructure work.
- **2. Stand up \`beta9\`** — locally first to validate, then via Helm into a Kubernetes cluster in your account. Configure S3-compatible storage (JuiceFS) so sandboxes aren't node-pinned. \[docs.beam.cloud/v2/self-hosting/aws, 2026-06-16\]
- **3. Attach compute** — CPU nodes for code execution, GPU nodes if your sandboxes run models. With BYOC this is your existing reserved capacity.
- **4. Lock down egress** — apply per-sandbox network policy before you let model-generated code run. Isolation protects the host; egress policy protects your data.
- **5. Point the SDK at your gateway** and re-run the same agent code you prototyped. No application changes.


## Security considerations


Self-hosting moves the isolation decision from the vendor to you — own it deliberately:


- **Match isolation to threat model.** Running *your own* trusted code? gVisor (Beam) or even container isolation is fine. Running *adversarial, multi-tenant* code from untrusted users? Prefer microVM (E2B's Firecracker self-host, or Microsandbox/libkrun) so a kernel exploit in one sandbox can't reach another.
- **gVisor is a mediated kernel, not a microVM.** Beam's model puts a user-space kernel between guest syscalls and the host — much smaller attack surface than a shared container, but the gVisor process itself is the boundary. Know which tier you're buying.
- **Privileged Docker is a host risk.** Judge0's` --privileged` requirement grants near-host capabilities; weigh that against your environment.
- **Egress is not optional.** The most common self-hosted-sandbox incident is not a kernel escape — it's trusted code with unrestricted internet access exfiltrating secrets. Set network policy first.


## FAQ


**Can you self-host E2B?** Yes — E2B's infrastructure is open source at` e2b-dev/infra` (Apache-2.0) and deploys via Terraform with Nomad, Consul, and Firecracker. GCP is fully supported, AWS is in beta. It is a real infrastructure project, not a one-command install. If you want microVM isolation and can operate a Nomad cluster, it's viable; if you want a fast path, a Helm-deployable runtime like Beam's` beta9` is lighter.


**What's the easiest sandbox to self-host?** For a single machine with no cluster, Microsandbox (local, rootless, libkrun) or DifySandbox (` docker run` ). For a production deployment that still has to scale, Beam's` beta9` via Helm gives you orchestration, storage, and GPU support without assembling them yourself.


**Do I need Kubernetes to self-host a sandbox?** Not always. Microsandbox and DifySandbox run without it. Beam can run locally for development and deploys to Kubernetes for production via Helm. E2B's infra uses Nomad rather than Kubernetes.


**Which self-hosted sandboxes support GPUs?** Beam and Daytona. Most lightweight options (Microsandbox, DifySandbox, Judge0) are CPU-only. If your agent runs models inside the sandbox, this is usually the deciding factor.


**Is self-hosting cheaper than managed?** It can be, but the savings come from running on compute you already pay for (reserved instances, committed-use discounts, on-prem GPUs), not from the software — most of these runtimes are free and open source. Beam's BYOC model is designed around exactly this: same API, your cloud bill.


**What isolation does Beam use when self-hosted?** gVisor + runc — the same user-space-kernel model as the managed product. It's stronger than a plain container and lighter to operate than a microVM. For adversarial multi-tenant code where own-kernel isolation is a hard requirement, evaluate a microVM option instead.


## Run your sandbox where you want it


Self-hosting a code execution sandbox is a solved problem in 2026 — the question is how much infrastructure you want to operate to get there. Beam's` beta9` gives you isolation, orchestration, storage, and GPU support in one AGPL-3.0 runtime, with the same API self-hosted or managed, so you can start in minutes and move in-house without a rewrite.


[Get started free](https://www.beam.cloud/) — build on managed Beam with $30/month in free credits, then self-host the same code when you're ready.


## Sources


- Beam runtime source (` beta9` ), AGPL-3.0: github.com/beam-cloud/beta9 — verified 2026-06-16
- Beam self-hosting + BYOC: docs.beam.cloud/v2/self-hosting, docs.beam.cloud/v2/self-hosting/aws — verified 2026-06-16
- Beam Sandbox SDK: docs.beam.cloud/v2/sandbox/overview — verified 2026-06-16
- E2B infrastructure (self-host), Apache-2.0: github.com/e2b-dev/infra — verified 2026-06-16
- Daytona source, AGPL-3.0: github.com/daytonaio/daytona — verified 2026-06-16
- Microsandbox source: github.com/microsandbox/microsandbox — verified 2026-06-16
- Judge0 source + CHANGELOG: github.com/judge0/judge0 — verified 2026-06-16
- DifySandbox source: github.com/langgenius/dify-sandbox — verified 2026-06-16


Hassaan Qadir


Published June 16, 2026


Keep Reading


## More from the Beam blog


[Engineering Tinker Model Pricing: What Fine-Tuning Costs in 2026 See what fine-tuning costs on Tinker, including worked cost examples and where renting GPUs gets cheaper. Tim Huynh](https://www.beam.cloud/blog/tinker-model-pricing)[Engineering What Is a Container, Really? Five Years of GPU Infrastructure Five years of GPU infrastructure at Beam — from ECS and Knative cold starts to a custom container runtime, FUSE lazy-loading, and a trustless binary. Luke Lombardi](https://www.beam.cloud/blog/what-is-a-container-really)


$30 free credit


refreshed monthly


## Start shipping on infra
you won’t outgrow.


Run sandboxes and GPU workloads on your cloud, and scale out to ours when you need to. No infra to manage.


[Start Building](https://platform.beam.cloud/)[Read the docs](https://docs.beam.cloud/)
