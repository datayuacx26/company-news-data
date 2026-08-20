---
schema_version: "1.0.0"
document_id: "403481d8b423e9e9425e380c74018d6ce0eaa96659d61a809f92921d4bbe790b"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/coding-agent-horror-stories-the-command-you-already-approved/"
published_at: "2026-08-18T13:00:00+00:00"
first_seen_at: "2026-08-18T14:21:58.307655+00:00"
fetched_at: "2026-08-18T14:21:59.723143+00:00"
content_hash: "sha256:7ab08e91f614731fb75db780e7e76ce36170d045d3a420a48575c030a79738d6"
---

# Coding Agent Horror Stories: The Command You Already Approved

This is Part 5 of our AI Coding Agent Horror Stories series, a look at real security incidents involving AI coding agents, and how[Docker Sandboxes](https://www.docker.com/products/docker-sandboxes/) contain agent execution at the boundary rather than at the command line.


In[Part 1](https://www.docker.com/blog/ai-coding-agent-horror-stories-security-risks/) , we walked through six categories of AI coding agent failures and why they keep happening. The agent runs as you, with your filesystem permissions and your credentials, and nothing sits between the model’s decision and the shell’s execution.[Part 2](https://www.docker.com/blog/coding-agent-horror-stories-the-rm-rf-incident/) went deep on the` rm -rf ~/` incident.[Part 3](https://www.docker.com/blog/coding-agent-horror-stories-the-agent-that-deleted-production/) moved the same problem into a production cloud environment.[Part 4](https://www.docker.com/blog/coding-agent-horror-stories-the-29-million-secret-problem/) followed the credentials themselves through a supply chain attack.


This one is about the safety net. Most teams running a coding agent today have some version of a list of commands the agent may run without asking, and the assumption underneath it is that anything dangerous will show up as a prompt you can refuse. In January, researchers at[Pillar Security](https://www.pillar.security/blog/the-agent-security-paradox-when-trusted-commands-in-cursor-become-attack-vectors) showed that the assumption doesn’t hold.


## Today’s Horror Story: The Approval That Ran Something Else


On January 14, 2026, researchers at[Pillar Security](https://www.pillar.security/blog/the-agent-security-paradox-when-trusted-commands-in-cursor-become-attack-vectors) disclosed CVE-2026-22708, a flaw in Cursor. When the agent ran in Auto-Run Mode with an allowlist enabled, a handful of shell built-ins executed without appearing in that allowlist and without asking for approval. Anything that could get text in front of the agent, a README or a dependency or an issue comment, could use them to change environment variables silently. A command the developer then approved, something as ordinary as` git branch` , would run the attacker’s code instead.[Cursor rated it High](https://github.com/cursor/cursor/security/advisories/GHSA-82wg-qcm4-fp2w) and patched it in version 2.3.


No memory corruption was involved here and no permission was escalated. The developer was shown an accurate prompt, approved a command that was genuinely harmless, and got arbitrary code execution anyway, because the meaning of that command had been changed a minute earlier by something they were never shown.


In this issue, you’ll learn:


- How shell built-in slipped past an allowlist that was working exactly as designed
- Why the attack still worked when the allowlist was completely empty
- What Docker Sandboxes contain here, and the two things they do not
- How kits, organisation policy and audit logs cover what a per-laptop allowlist misses


*Caption: Comic illustrating how an injected instruction changes environment settings without triggering an approval prompt, so that a command the developer legitimately approves runs the attacker’s payload instead.*


## The Problem


Typically, programs read settings from their environment when they start up. Git checks one called` PAGER` to work out which program displays its output, and Python checks one called` PYTHONWARNINGS` . Nobody thinks about these, which is rather the point. The commands that change them are shell built-ins, and Pillar’s research names[export , typeset and declare](https://www.pillar.security/blog/the-agent-security-paradox-when-trusted-commands-in-cursor-become-attack-vectors) specifically, a detail[reported independently at disclosure](https://www.scworld.com/news/cursor-vulnerability-enables-stealthy-rce-via-indirect-prompt-injection) . Built-ins are not programs sitting on disk, and the checker was looking for programs on disk, so they went through without ever being surfaced.


Which means the whole attack is two lines.


```text
# This one runs silently. You are never asked.
export PAGER="open -a Calculator"


# This one you are asked about, and you say yes, because obviously.
git branch


```


Git looked up` PAGER` to work out how to show the branch list, found the attacker’s command sitting in it, and ran that instead. Pillar notes this worked even with a completely empty allowlist, which is the most restrictive setting on offer.


An allowlist checks whether the command in front of it is on the list, which is fine for cutting down interruptions, and nobody wants to approve` ls` for the ninetieth time in a morning. But the name of a command does not tell you what that command will do. The check reads the name, waves it through, and the setting that decides what actually happens was changed a minute earlier by something the check was never shown.


Cursor’s documentation now describes the allowlist as best-effort and warns that bypasses are possible. Pillar went further and argued that agents should be handed full command execution inside an isolated environment, and that the industry ought to deprecate allowlists altogether.


## The Scale of the Problem


None of the underlying trick is new. Pillar’s write-up points back to[Elttam’s 2020 research on environment variables](https://www.elttam.com/blog/env) , which showed how these settings could be turned into code execution.


It sat there for six years without troubling anybody very much. Pulling it off meant already being on someone’s machine, setting several things in the right order, running each step yourself, and anyone with that much access had faster ways to cause damage.


Then coding agents arrived and removed every one of those obstacles at once. They act on instructions found in files they were told to read, they run several steps in a row without stopping to check, and they run as you. A technique that used to need somebody sitting at your keyboard now arrives in a repository you cloned this morning.


It is the same shape as the s1ngularity attack from[Part 4](https://www.docker.com/blog/coding-agent-horror-stories-the-29-million-secret-problem/) . There, a poisoned package borrowed an agent that was already logged in. Here, poisoned text borrows a command that was already approved. Neither one breaks anything. Both of them use permission that was handed over deliberately, for something nobody intended.


## Technical Breakdown: How the Attack Works


*Caption: Diagram showing how an injected instruction changes the shell environment out of sight, so that an allowlisted command carries the attacker’s payload when the developer approves it.*


The attack has two halves, and the split between them is the entire trick.


### 1. The half you never see


The agent reads a file it was asked to read, and that file contains an instruction meant for the agent rather than for you. Built-ins then quietly set the environment. Nothing appears on your screen.


Pillar demonstrated a longer version of this, chaining several settings together,` PYTHONWARNINGS` ,` BROWSER` , and` PERL5OPT` among them, so that every later` python3` command on that machine would run attacker code. The details differ, but the principle is the same: change what a program reads at startup, and you change what it does.


### 2. The half you approve


Then you run` git branch` or` python3 script.py` , or the agent runs it for you under your allowlist. These are the commands people add to allowlists to stop the constant interrupting, so the better tuned your list is, the more reliably the trigger fires. The payload runs with your permissions.


Some variants skip the approval altogether. One writes extra lines into` ~/.zshrc` , so the code runs again every time you open a terminal. You could finish the project, delete the repository, and still be running it next month.


## The Impact


The full chain in Pillar’s research ends with the victim’s SSH private keys leaving the machine.


Work backwards and the whole thing started with a piece of text in a file, read by an agent doing exactly what it was asked to do. No memory bug. No privilege escalation. Nothing in any log that looks the slightest bit out of place.


Pillar reported it in August 2025 and the fix shipped that January. Cursor engaged with the report and made a real change, so anything the parser cannot classify now requires approval, which closes the paths that were demonstrated. Five months is a fair measure of how awkward this is to fix at the layer where it was found rather than a complaint about the vendor.


The wider problem has not gone anywhere, because it was never really about shell built-ins. It is about a check that studies the command while somebody rearranges the furniture around it.


*Caption: Diagram showing the same payload running inside the microVM, and what it can and cannot reach from there.*


## How Docker Sandboxes Contain This at the Execution Layer


[Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) run AI coding agents in isolated microVMs, each with its own kernel, filesystem, and deny-by-default network, so a compromised dependency an agent pulls cannot reach the host, its credentials, or other workloads. Inside that box the agent can run anything, including with sudo, which is exactly what Pillar recommends. There is no allowlist to slip past. We made the longer argument for why a shared kernel is the wrong shape for this in[The Untrusted Autonomous Workload](https://www.docker.com/blog/untrusted-autonomous-workload-ai-sandboxes/) .


So run the same attack again, this time in a sandbox, and watch where it gets to.


The injection still lands. The environment gets changed,` git branch` still triggers it, and the payload runs. Nothing about a sandbox stops that. Then the payload goes looking for your SSH key and does not find one. Your home directory sits on the other side of the boundary, so there is no` ~/.ssh/id_rsa` inside the box to copy.


It can still use the key. Sandboxes forwards an SSH agent socket into the box so that ordinary work like` git push` keeps working, which means code inside can ask that agent to authenticate on its behalf. It cannot take the key anywhere, but it can borrow it for as long as the sandbox runs. Your network policy is what limits that, since SSH needs a rule naming the exact destination address and port before it connects to anything.


The` ~/.zshrc` trick fails outright, because that file lives on your host and a poisoned copy written inside the box disappears along with the box.


Getting data out is harder than people expect. HTTP and HTTPS leave only through a proxy on your host that checks every request against your rules, anything else over TCP needs a rule naming the address and port, and UDP and ICMP are blocked outright.


Two caveats, both stated plainly in[Docker’s security documentation](https://docs.docker.com/ai/sandboxes/security/) . The first is your workspace, which is live on your host by default, so Git hooks and` Makefile` targets are still within reach and a poisoned hook will not turn up in` git diff` . Running with` --clone` hands the agent its own copy.


The second is the shared agent skills store. Supported agents mount the same host-side store read-write unless you opt out at creation time, which is what lets an agent refine a skill and keep it. Every sandbox sharing that store sits inside one trust boundary, so a skill modified inside one becomes an input to the next that loads it. The store is sandbox state though, and a modified skill does not by itself execute on your host, so the risk runs sandbox to sandbox rather than sandbox to host.


Isolation has its own seams. In July, Pillar published[a series of sandbox escapes](https://www.pillar.security/blog/prompt-injection-leads-to-rce-and-sandbox-escape-in-antigravity) across four coding agents, and the mechanism was never a broken sandbox but a file written inside one that a tool outside later trusted. Both caveats above are that shape.


None of this stops the injection. It changes what the injection can get to, which is the only part of this problem with a dependable answer.


## Codify the Boundary with Kits


*Caption: Diagram showing how a kit declares an agent’s tools, files and network rules, while real credentials stay on the host and are injected by the forward proxy on the way out.*


The allowlist failed here partly because it is a list, edited on each laptop, that an injection can reach around.[Kits](https://docs.docker.com/ai/sandboxes/customize/kits/) are Docker’s answer to the editing-on-each-laptop half of that.


A kit is a declarative YAML artifact that[extends a sandbox agent with credentials, network policies, environment variables, startup commands and files](https://docs.docker.com/reference/cli/sbx/kit/) . Rather than every developer maintaining a personal allowlist, you write the boundary once, deny-by-default network plus only the destinations a task genuinely needs, and hand the same kit to everybody. It gets reviewed, versioned and diffed like any other file in the repository. The[kit spec reference](https://docs.docker.com/ai/sandboxes/customize/kit-reference/) covers the fields, and[docker/sbx-kits-contrib](https://github.com/docker/sbx-kits-contrib) has working examples.


This lands directly on the SSH question above. A forwarded SSH agent is a live credential limited only by network policy, so leaving that policy to whoever remembers to run` sbx policy deny` is the same per-laptop weak point this whole post has been complaining about. A kit can bake the network rule in, so untrusted work has no SSH egress unless the destination was declared up front.


## What This Looks Like in Practice


The vulnerability is in the editor, so what you want is the setup that puts the editor’s terminal inside the box. Cursor is built on VS Code and connects the same way, over Remote – SSH, with the editor staying on your machine while files, terminals and extensions run in the sandbox. You will need Docker Sandboxes 0.37.0 or later, SSH access configured, and Cursor’s Remote – SSH support installed. The[Cursor integration guide](https://docs.docker.com/ai/sandboxes/integrations/cursor/) has the full walkthrough.


```text
# One-time setup: configure your SSH client for sandboxes.
sbx setup ssh
# Check the sandbox is reachable, then open the Command Palette,
# run Remote-SSH: Connect to Host, and enter &lt;name&gt;.sbx
ssh demo.sbx
# See what this sandbox is currently allowed to reach.
sbx policy ls
# Shut egress down and open only what the task needs.
sbx policy deny network "**"
sbx policy allow network "github.com,registry.npmjs.org"


```


Those last two commands come with a catch. If your organisation has[governance](https://www.docker.com/products/ai-governance/) switched on, the org policy replaces local policy and` sbx policy allow` and` sbx policy deny` will have no effect on your machine. You can spot it in the output of` sbx policy ls` , which begins with a` Governance: Managed by <org>` line when that is the case. Depending on how admins scope things, some rule types may be delegated back to local control, but a local allow will never beat an organisation-level deny.


Same editor, same agent, same allowlist, same payload. All that changed is which machine the terminal is on.


What happens On your laptop Inside a sandbox


The payload runs Yes Yes


Where it runs Your machine, as you A microVM with its own kernel


Your SSH key file Can be read and copied Not there


SSH authentication Available, key included Available, key stays outside


The ~/.zshrc trick Persists indefinitely Gone with the sandbox


Sending data out Open by default Only where policy allows


Who sets the rules Each developer The organisation


Evidence afterwards None A logged policy decision


## Making This Hold Across a Team


A kit gets the boundary out of one developer’s head and into a file the team shares, but a file can still be ignored or edited on the machine that matters.[Docker AI Governance](https://www.docker.com/products/ai-governance/) moves the settings up one more level. Network and filesystem rules are defined once by your admins and reach developers through the login they already use, so there is nothing to configure per machine and nobody quietly reopening what security closed. A shared kit is the boundary as a suggestion. Governance is the boundary as a ceiling.


The part that matters most for this story is the record it keeps. What made CVE-2026-22708 work was that the first half was invisible, with no prompt and nothing written down anywhere you would think to look. Under governance every policy decision produces an event carrying the user, the timestamp and the rule that fired, and those events[stream into whatever SIEM your security team already uses](https://www.docker.com/blog/docker-ai-governance-audit-logs-now-where-your-security-team-already-works/) .


So an attack that succeeds inside the sandbox and then reaches for somewhere it should not leaves a trail behind it. That is a good deal better than a check that finds nothing wrong and mentions it to nobody.


## Best Practices


1. **Treat**` **export**` **like any other command.** Anything that changes environment settings can change what your next command does, even when that next command is on your allowlist.


2. **Do not mistake an allowlist for a boundary.** It reduces interruptions. The vendor documentation now says outright that it is best-effort and not a security guarantee.


3. **Isolate before the first command, not after something looks wrong.** Untrusted means anything you did not write and have not read, which covers most of a dependency tree.


4. **Use**` **--clone**` **for code you have not vetted, and opt out of the shared skills store.** Otherwise Git hooks and build scripts stay live on your host, a poisoned hook will not appear in` git diff` , and a skill modified inside one sandbox is waiting for the next sandbox that loads it.


5. **Remember a forwarded SSH agent is a live credential.** The key file staying on your machine is not the same as the key being unusable, so restrict egress for untrusted work.


6. **Read your own policy.** Run` sbx policy ls` . Deny-by-default with a long allow list is closer to allow-by-default than it looks.


## Take Action


- **Install Docker Sandboxes.** Visit the[Docker Sandboxes documentation](https://docs.docker.com/ai/sandboxes/) to install` sbx` and run your first agent inside a microVM.
- **Connect your editor.** The[Remote – SSH integration](https://docs.docker.com/ai/sandboxes/integrations/cursor/) puts your terminals inside the boundary while the editor stays where it is, so your workflow does not really change.
- **Codify the boundary with a kit.**[Define the network and credential rules](https://docs.docker.com/ai/sandboxes/customize/kits/) your team needs once, and hand the same artifact to everybody instead of a personal allowlist.
- **Read the security model.**[The documentation](https://docs.docker.com/ai/sandboxes/security/) is straight about what is isolated and what is not, including the workspace and shared skills store behaviour that` --clone` and the opt-out flag change.
- **Turn on audit logging.**[Docker AI Governance](https://www.docker.com/products/ai-governance/) streams policy decisions into your SIEM, which turns a silent bypass into something somebody can actually investigate.


## Conclusion


The uncomfortable thing about CVE-2026-22708 is that nobody in the story did anything wrong.


Cursor built an allowlist, which is what everyone asked for. The developer approved` git branch` , which any of us would have approved. The check inspected the command and found it acceptable, which is exactly its job. The attack worked anyway.


Getting an agent to correctly judge every instruction it reads is a problem that gets harder as agents get more capable, and it has no clean ending. Limiting what an agent can reach is a problem we solved a long time ago. The more useful move is to stop needing the first one, and to write down what the agent may reach as an artifact you can review, rather than a list each laptop keeps for itself.


*Coming up in our series: Issue 6 looks at the ClawHub infostealer campaign, where malicious skills reached developer machines through a marketplace ranking exploit, and at what sandboxed skill execution, and that shared skills store, change about a registry you cannot personally audit.*


### Learn more


- **Run agents safely with Docker Sandboxes:**[Visit the Docker Sandboxes documentation](https://docs.docker.com/ai/sandboxes/) to get started.
- **Understand the security model:**[Trust boundaries and isolation layers](https://docs.docker.com/ai/sandboxes/security/) , including what is and is not isolated by default.
- **Customize sandboxes with kits:**[Extend an agent with tools, credentials, and network rules](https://docs.docker.com/ai/sandboxes/customize/) as a declarative artifact.
- **Govern agents across a team:**[Docker AI Governance](https://www.docker.com/products/ai-governance/) for centrally defined sandbox, network and MCP policy with structured audit events.
- **Read the MCP Horror Stories series:**[Start with Issue 1](https://www.docker.com/blog/mcp-security-issues-threatening-ai-infrastructure/) to understand the protocol-layer security risks that complement the agent-layer risks covered here.
