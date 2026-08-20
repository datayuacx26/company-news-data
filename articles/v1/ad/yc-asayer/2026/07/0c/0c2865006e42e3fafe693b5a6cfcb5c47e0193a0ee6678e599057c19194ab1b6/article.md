---
schema_version: "1.0.0"
document_id: "0c2865006e42e3fafe693b5a6cfcb5c47e0193a0ee6678e599057c19194ab1b6"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/coding-agents-yolo-mode/"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:23da21eeca73c4efd2868774597ebe2e4944118872244866889e9eed375188dd"
---

# Running Coding Agents in YOLO Mode Without Nuking Your Laptop

Running a coding agent in YOLO mode is safe only when you contain the blast radius — you make the agent’s environment disposable, not the agent trustworthy. Bypass-permissions mode (Claude Code’s` --dangerously-skip-permissions` , Codex’s full-auto, Gemini CLI’s YOLO) trades a hundred approval clicks for an agent that can read your SSH keys, delete files, corrupt git history, and execute code it generated seconds ago — all without asking. You cannot fix that by trusting the model or by writing a tighter allowlist. You fix it by putting the agent somewhere it can’t hurt anything that matters, in layers, matched to how much you’re actually risking.


This article gives you the risk model, a tool-agnostic decision rule for which isolation layer you actually need, the official containment Claude Code now ships, and a copy-pasteable minimum-safe setup with its honest trade-offs. This space moves monthly, so flags and tool names will drift — the durable primitives (containers, microVMs, egress proxies, disposable workspaces) will not.


## Key Takeaways


- YOLO mode is safe only when you contain the blast radius — you make the agent’s environment disposable, not the agent trustworthy.
- Least-privilege allowlisting fails for coding agents because their valid action space is unbounded: one task may legitimately install packages, write arbitrary paths, and run code generated seconds earlier.
- Match the layer to the risk: built-in sandbox or auto mode for everyday edits, a non-root devcontainer for routine unattended runs, a microVM with its own kernel for fully unattended or untrusted execution, and per-pod kernel isolation only for shared CI or multi-tenant infrastructure.
- Since March 2026, Claude Code’s auto mode uses model classifiers to clear the ~93% of prompts users approve anyway and escalates to a human after 3 consecutive or 20 total denials — often removing the reason to reach for full bypass.
- Effective isolation needs both filesystem and network limits: without network isolation a compromised agent can exfiltrate SSH keys; without filesystem isolation it can escape to reach the network.


## What Claude Code’s YOLO mode actually does in 2026


Bypass-permissions mode disables the approval prompts and most safety checks that normally gate an agent’s shell commands and file writes. Per the[Claude Code permission-modes documentation](https://code.claude.com/docs/en/permission-modes) ,` --dangerously-skip-permissions` is equivalent to` --permission-mode bypassPermissions` . Introduced in Claude Code v2.1.126, that mode also skips writes to protected paths that earlier versions still prompted for — though` rm -rf /` and` rm -rf ~` continue to prompt as circuit breakers against model error, and the flag now refuses to start as root or under sudo on Linux and macOS. The root/sudo refusal is corroborated in the[sandboxed Bash tool docs](https://code.claude.com/docs/en/sandboxing) and quietly breaks every naive Docker recipe that runs the agent as root.


The reason you can’t just write a safer allowlist is structural. Least-privilege allowlisting fails for coding agents because their valid action space is unbounded: a single legitimate task may install packages, write to arbitrary paths, and execute freshly generated code. Edera frames this directly —[you cannot apply least privilege to an agent because the valid behavior space can’t be enumerated](https://edera.dev/stories/yolo-mode-for-ai-agents-without-the-yolo-running-claude-code-with-kernel-isolation) . Every command that looks destructive is also something an agent does in normal work.


The failure modes aren’t hypothetical. Anthropic’s own incident log behind auto mode describes agents[deleting remote git branches, uploading an engineer’s GitHub auth token, and attempting migrations against a production database](https://www.anthropic.com/engineering/claude-code-auto-mode) . That is the threat: not malice, but a non-deterministic process with full shell access doing something plausible and wrong.


## Contain the blast radius: the layered defense model


The working principle, borrowed from Edera, is to treat this as a resilience problem rather than a policy problem: accept that something will eventually go wrong and make sure that when it does, the failure stays contained. You don’t sandbox the agent because you expect it to misbehave on this run; you sandbox it so the run that misbehaves costs you a deleted container instead of a leaked credential set.


Containment has two axes that both have to hold. Effective isolation needs both filesystem and network limits. The Claude Code sandboxing docs make the dependency explicit: without network isolation an agent can exfiltrate SSH keys, and without filesystem isolation it can escape to reach the network. A layer that covers one axis and not the other has a hole.


No single layer covers every failure mode, which is why the useful mental model is what each layer protects — and what it doesn’t:


Layer Filesystem Network egress Credentials Git history Kernel escape


Deny-rules / allowlist Weak Weak No No No


Project-scoped working dir Partial No No Partial No


Devcontainer (non-root) Yes Configurable Yes (if not mounted) Partial No


Docker container (non-root) Yes Configurable Yes (if not mounted) Partial No


microVM (own kernel) Yes Yes (proxy) Yes Yes (if not mounted) Yes


K8s per-pod kernel isolation Yes Policy No N/A Yes


Two rows deserve a callout. A standard container narrows the damage from “my whole machine” to “this project folder,” but it shares the host kernel, so it was never designed to be a security boundary against code you don’t trust — it’s an isolation convenience, not a jail. And at the strongest tier, kernel-level isolation still won’t stop credential theft or data exfiltration; Edera says so itself, noting that hardware isolation[doesn’t cover credential misuse or network exfiltration — scope those with per-agent credentials and network policies](https://edera.dev/use-case/ai-agent-sandboxing) . Keep secrets out of the sandbox and put an egress allowlist in front of it regardless of which layer you pick.


## Which layer do you actually need?


Match the layer to the risk: built-in sandbox or auto mode for everyday edits, a non-root devcontainer for routine unattended runs, a microVM with its own kernel for fully unattended or untrusted execution, and per-pod kernel isolation only for shared CI or multi-tenant infrastructure. There’s no single tool that’s the answer. The right answer is a decision rule, because a developer babysitting a refactor and a CI job running untrusted PRs are not exposed to the same risk.


Situation Layer Why


Everyday local edits, you’re watching Built-in` /sandbox` or auto mode OS-level limits with no setup; you’re the backstop


Routine unattended runs on your own code Non-root devcontainer or Docker Bounds the agent to the project; also an onboarding win


Fully unattended, or running untrusted/generated code microVM with its own kernel (e.g. Docker` sbx` ) Sealed boundary even against kernel-level escapes


Shared CI or multi-tenant infra Per-pod kernel isolation (Edera, Kata, gVisor) One tenant’s RCE can’t reach another’s workload


This rule is editorial synthesis, not a vendor claim — but each layer’s capability in it is sourced below. The point is to stop over- and under-isolating: don’t spin up a microVM to fix a typo, and don’t run untrusted PRs in a root container because it was easy.


## Claude Code already ships several of these layers


The fastest containment is the one you don’t have to build, and Claude Code now ships several official isolation layers out of the box. Since[October 20, 2025](https://www.anthropic.com/engineering/claude-code-sandboxing) , Claude Code has shipped an OS-level` /sandbox` for its Bash tool built on Linux bubblewrap and macOS Seatbelt, and released the underlying engine as an open-source research preview,[@anthropic-ai/sandbox-runtime](https://github.com/anthropic-experimental/sandbox-runtime) (GitHub repo` anthropic-experimental/sandbox-runtime` , binary` srt` ). It’s still a research preview — v0.0.49 as of April 3, 2026 — so treat its API as subject to change. The repo’s own README demonstrates the boundary: a sandboxed` srt "cat ~/.ssh/id_rsa"` is blocked rather than dumping your private key.


```text
# Open-source sandbox-runtime demo (research preview, v0.0.49 — APIs may change)
srt   "  cat ~/.ssh/id_rsa  "     # -> denied: read outside the allowed filesystem
```


The same October 2025 release added Claude Code on the web, which runs sessions in an isolated cloud sandbox where, crucially, git credentials and signing keys are never inside the sandbox with the agent.


The genuinely new 2026 layer is **auto mode** , shipped March 24, 2026. Anthropic found that users approve about 93% of permission prompts anyway, so auto mode uses model classifiers to clear those automatically and escalates to a human after 3 consecutive or 20 total denials — a backstop against a compromised or overeager agent. It’s explicitly meant to replace` --dangerously-skip-permissions` without bringing back the interruptions, which means for a lot of everyday work it removes the reason to reach for full bypass at all. None of this replaces a sandbox for fully unattended runs, but it changes the default: reach for YOLO less often, and isolate harder when you do.


## Locking down network and credentials


Sealing the filesystem without sealing egress leaves the worst exfiltration path open, so the network layer is not optional. The clean primitive is a host-side egress proxy with a domain allowlist, so a rogue agent can’t phone home or upload what it read.[Docker Sandboxes](https://docs.docker.com/ai/sandboxes/security/policy/) (` sbx` ) ship three presets you can copy as a mental model: **Open** (all traffic allowed, CLI` allow-all` ), **Balanced** (default-deny with common dev sites allowed, CLI` balanced` ), and **Locked Down** (everything blocked unless explicitly allowed, CLI` deny-all` ). Balanced is the right default; Open is convenient but defeats the point if you’re running unattended.


Credentials need separate handling, because the simplest way to leak them is to mount them. Don’t mount` ~/.ssh` ,` ~/.aws` , or your` .env` into the agent’s environment — if the secret isn’t in the sandbox, the agent can’t read it. When the agent genuinely needs to authenticate to a model API or a service, inject the credential at the proxy rather than handing it to the agent. Docker’s` sbx` does exactly this: its host-side forward proxy injects auth headers for AI services so the raw credential values are never stored inside the VM, per the[isolation docs](https://docs.docker.com/ai/sandboxes/security/isolation/) . That MITM-style auth-header injection is the pattern to look for in any sandbox you adopt.


## Keeping git safe


Git is the failure mode people underestimate, because a mounted` .git` directory is fully writable by the agent. The mitigation is to run in branch or worktree mode so the agent commits to a disposable branch, not your main line. Andrew Lock’s[walkthrough of sbx](https://andrewlock.net/running-ai-agents-safely-in-a-microvm-using-docker-sandbox/) shows the pattern —` sbx run claude --branch my-feature/auto` puts the agent in a git worktree under` .sbx/` , which you add to your global gitignore.


The honest caveat: branch mode reduces the chance of a bad commit landing on` main` , but it doesn’t seal git. As Lock notes, the agent fundamentally has access to the git directory, so it could still corrupt the repo — which is why the only real backstop is a remote backup you can re-clone from. If the repo can’t be recovered with` git clone` , it isn’t backed up.


## A minimum safe setup you can copy


For most local work, a non-root devcontainer is the right floor: it bounds the agent to the project, doubles as one-command onboarding for new developers, and satisfies the flag’s own requirement that bypass mode not run as root. A minimal` .devcontainer/devcontainer.json` looks like this:


```text
{
"  name  "  :   "  agent-sandbox  "  ,
"  image  "  :   "  mcr.microsoft.com/devcontainers/javascript-node:22  "  ,
"  remoteUser  "  :   "  node  "  ,
"  workspaceFolder  "  :   "  /workspace  "  ,
"  workspaceMount  "  :   "  source=${localWorkspaceFolder},target=/workspace,type=bind  "
}
```


The load-bearing line is` "remoteUser": "node"` — a non-root user, because` --dangerously-skip-permissions` refuses to start as root or under sudo. Mount only the project directory; do not mount your home directory or credential paths. Install the agent inside the container, point it at the model API through an egress allowlist, and let it run.


When you’re running fully unattended, or executing code from untrusted PRs or arbitrary generated scripts, step up to a microVM with its own kernel:


```text
# Docker Sandboxes: a microVM per agent, with its own kernel and Docker daemon
sbx   run   claude          # spins up an isolated microVM and launches in bypass mode
```


The` sbx` CLI is free to use including for commercial work — only organization governance requires a paid subscription — and it now runs on macOS, Windows, and Linux, supporting Claude Code, Codex, Copilot, Gemini, Droid, Kiro, OpenCode, and more. On Linux you’ll need KVM hardware virtualization enabled and your user added to the` kvm` group (see the[get-started guide](https://docs.docker.com/ai/sandboxes/get-started/) ).


The trade-offs are real and worth stating:


- microVMs carry performance overhead even on simple projects;
- commit signing via host SSH agents doesn’t cleanly forward into sandboxes, so a common workaround is to commit unsigned inside and rebase to sign on the host; and
- a default-deny network policy needs curation before it stops getting in your way.


## Beyond the laptop: shared and CI infrastructure


Once an agent runs on infrastructure shared by other tenants or CI jobs, the laptop layers aren’t enough, because a shared host kernel means one job’s kernel exploit can reach every workload on the node. The fix at that tier is per-pod kernel isolation — giving each agent its own Linux kernel in a hardware sandbox via runtimes like Edera, Kata Containers, or gVisor. Edera reports running each workload in its own kernel while staying[within 5% of native performance](https://edera.dev/use-case/untrusted-code-execution) , with published benchmarks at[arXiv:2501.04580](https://arxiv.org/abs/2501.04580) ; those are vendor-published figures, and Edera access is gated through their team rather than a public download. This tier is overkill for a single developer’s laptop — reserve it for the case it’s built for: multi-tenant or CI infrastructure where one tenant’s compromise must not become everyone’s.


Pick the lowest layer that actually covers your risk, keep secrets out of whatever box the agent runs in, and put an allowlist in front of its network. Then the next time the agent does something plausible and wrong, you delete a container instead of rebuilding your machine.


## FAQs


What is the difference between Claude Code's auto mode and bypass-permissions mode?


Bypass-permissions mode (--dangerously-skip-permissions) disables approval prompts and most safety checks entirely, clearing every command without judgment. Auto mode, shipped March 2026, uses model classifiers to automatically approve only the routine prompts users approve anyway (around 93 percent of them) while still escalating risky actions to a human, and it stops the agent after 3 consecutive or 20 total denials. Auto mode is designed to replace full bypass for everyday work without reintroducing constant interruptions.


Does running a coding agent in a Docker container make it safe to use YOLO mode?


A standard Docker container narrows the damage from your whole machine to one project folder, but it is not a complete security boundary because it shares the host kernel and was never designed to contain code you do not trust. For routine unattended runs on your own code a non-root container is reasonable, but for executing untrusted or freshly generated code you need a microVM with its own kernel. Containers also need a non-root user, since bypass mode refuses to start as root or under sudo.


Why can't I just write a tighter allowlist instead of sandboxing the agent?


Least-privilege allowlisting fails for coding agents because their valid action space is unbounded. A single legitimate task may install packages, write to arbitrary paths, and execute code generated seconds earlier, so any rule strict enough to block damage also blocks normal work. Every command that looks destructive is something an agent does in ordinary operation. The reliable approach is containment: put the agent in a disposable environment matched to your risk rather than trying to enumerate every safe action in advance.


Will sandboxing the agent's filesystem also protect my credentials and SSH keys?


No. Filesystem isolation and network isolation are separate axes, and both must hold. Without network limits a compromised agent can exfiltrate SSH keys it can read; without filesystem limits it can escape to reach the network. Even kernel-level isolation does not cover credential theft or data exfiltration on its own. Keep secrets out of the sandbox entirely by never mounting paths like the SSH or AWS directories, inject credentials at an egress proxy when needed, and front the agent with a domain allowlist.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
