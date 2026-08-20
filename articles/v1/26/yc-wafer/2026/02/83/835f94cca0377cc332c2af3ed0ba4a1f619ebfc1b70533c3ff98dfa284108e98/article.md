---
schema_version: "1.0.0"
document_id: "835f94cca0377cc332c2af3ed0ba4a1f619ebfc1b70533c3ff98dfa284108e98"
company_key: "yc-wafer"
company: "Wafer"
source_id: "yc-wafer-news-import-d450341df50f"
canonical_url: "https://www.wafer.ai/blog/workspaces"
published_at: "2026-02-05T00:00:00+00:00"
first_seen_at: "2026-07-22T19:17:32.817594+00:00"
fetched_at: "2026-07-28T22:21:35.013216+00:00"
content_hash: "sha256:13802b126dfbd7f40cefefa869b00cc0e517fa5f2629dc1610c699e635f74cfa"
---

# Workspaces: GPU Compute for Your Coding Agent

Give your AI coding assistant direct GPUs, with no manual SSH, Docker, or infra babysitting.


## The Problem


AI coding assistants are writing more and more code, including GPU kernels. But there’s a fundamental disconnect: agents generally run on your local machine, and have trouble accessing compute cleanly.


So you end up with a painful manual workflow:


1. Agent writes kernel code
2. You provision a cloud GPU instance
3. Set up SSH, Docker, environment
4. Sync code to the remote machine
5. Run the kernel, capture output
6. Copy results back
7. Paste into chat
8. Pay for idle time while you iterate


This breaks the agent’s autonomy. The whole point of a coding agent is that it can iterate independently, but without GPU access, every optimization loop requires you to be the manual bridge.


## The Solution: Workspaces


**Workspaces give your coding agent direct access to GPU compute.**


One command:


```text
wafer   target   workspace   create   dev   --gpu   B200   --environment   baremetal
```


Your agent now has a B200 GPU. No provisioning. No SSH setup. No Docker configuration.


From there, the agent can work on this B200 as it if was operating locally:


```text
# Sync code to the workspace
wafer   target   workspace   sync   dev   ./my-kernel/


# Run commands remotely
wafer   target   workspace   exec   dev   --   python   train.py


# Pull results back
wafer   target   workspace   pull   dev   /workspace/results   ./results


# Or SSH in for interactive work
wafer   target   workspace   ssh   dev
```


When you’re done:


```text
wafer   target   workspace   delete   dev
```


## Pay Only for What You Use


Traditional GPU development means renting an instance and paying for every minute, even when you’re just reading code, thinking, or waiting for your agent to respond.


With Workspaces, **you pay by the second, only when commands are actually running.**


Kernel development is bursty. You write code, run it for a few seconds, analyze results, iterate. Most of your time isn’t spent executing; it’s spent thinking and coding. Why pay for a GPU that’s sitting idle?


Workspaces flip the model. Your agent spins up compute when it needs it, runs the command, and you only pay for those seconds of actual execution. No more paying for idle GPUs while you debug a memory access pattern.


## Demo


Claude Opus 4.6 autonomously creates a workspace, writes a GEMM kernel, and iterates on optimizations. No human touched infrastructure.


## What’s Supported


**GPU Types:**


- **B200:** NVIDIA Blackwell (180GB HBM3e, CUDA)
- **MI300X:** AMD Instinct (192GB HBM3, ROCm)


**Built-in Features:**


- ` wafer target workspace sync` : Push local files to workspace
- ` wafer target workspace pull` : Pull remote files to local
- ` wafer target workspace exec` : Run commands remotely
- ` wafer target workspace ssh` : Interactive shell (baremetal only)
- ` wafer target workspace list` : See all your workspaces
- ` wafer target workspace delete` : Clean up when done


## Talk to the Team


- [Book a Demo](https://cal.com/wafer/20min)
- [Contact the team](https://www.wafer.ai/cdn-cgi/l/email-protection#ec8485ac9b8d8a899ec28d85)


We can help you set up Workspaces for your team and target hardware.


## We’d love your feedback


What workflows and hardware would you like to see supported? What’s missing? What’s broken?


Reach out at[\[email protected\]](https://www.wafer.ai/cdn-cgi/l/email-protection#c3abaa83b4a2a5a6b1eda2aa) or find us on[Twitter/X](https://x.com/wafer_ai) .
