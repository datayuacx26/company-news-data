---
schema_version: "1.0.0"
document_id: "7c892c94163abef4f42472a5f85ddead6ee7d89b3d8254fa6c384b44ca3c0717"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/opinion/why-ai-sandboxes-suck"
published_at: "2026-05-15T00:00:00+00:00"
first_seen_at: "2026-07-21T20:54:28.432730+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:68e8f0d1ee2bf663c5d2505e35128e259e191a4cb6eded59df737f4c7cc458a9"
---

# Why AI sandboxes Suck

The problem with sandboxes is not that isolation is bad. Isolation is good. Please isolate the untrusted code, the user-submitted code, the model-written code, and the third-party package it found while trying to make your chart prettier.


The problem is that the word "sandbox" has become a polite way to say: *we gave the agent a weird half-computer and hoped it would not notice.*


Sometimes that half-computer is a JavaScript isolate. Sometimes it is a fake filesystem with` grep` ,` cat` ,` ls` , and a few bash-shaped commands. Sometimes it is a container with enough missing privileges that the first real workload turns into a product negotiation. Sometimes it is a hosted code runner where the happy path is great and every other path is a support ticket.


These are useful products. They are not useless. But they are usually built around a premise that breaks the moment agents become capable:


> We can predict what the agent will need.


We cannot.


## Just bash is not bash


Vercel recently introduced[bash-tool](https://vercel.com/changelog/introducing-bash-tool-for-filesystem-based-context-retrieval) , which runs on[just-bash](https://justbash.dev/) and gives agents filesystem-oriented context retrieval. It is a good idea for context search. Large prompts are bad. Letting an agent run` find` ,` grep` ,` jq` , and pipes over a local tree is a much better interface than dumping a repository into the context window.


But the interesting line in the announcement is the escape hatch: "If you need a real shell, a real filesystem, or custom binaries, you can run the same tool against a Sandbox-compatible API for full VM isolation."


That is the whole argument hiding in one sentence. If you need a real shell, use a real shell.


` just-bash` is useful for the slice of work where the task is to search files and return context. The trouble starts when people confuse that slice with the job. An agent that can only inspect a codebase can help you understand it. An agent that can install dependencies, start a database, run a browser, reproduce a failing test, attach a debugger, generate a PDF, compare screenshots, and push a branch can actually do the work.


Those are different products.


## The OS is the interface


There is a tempting product instinct here: expose only the clean parts. Give the model a` readFile` tool, a` writeFile` tool, a` runTests` tool, maybe a` search` tool. Keep the messy operating system away from it. Make the agent use your beautiful API instead of the terrifying API called Linux.


This sounds responsible until the task escapes the shape of the tools.


The user says the PDF is ugly, so the agent needs a renderer. The test failure only appears with the native package installed, so it needs` apt` . The frontend bug only appears in Chromium, so it needs a browser. The app uses Redis, Postgres, and a dev server, so it needs services. The deploy script shells out to` git` ,` node` ,` python` , and` openssl` , so it needs binaries you did not think to wrap. The library's docs are wrong, so it needs to build a tiny reproduction and inspect what happens.


This is why[I argued before](https://www.freestyle.sh/blog/opinion/youre-starving-your-agent) that capable agents need capable harnesses. The lesson is not "give every model infinite power." The lesson is that the harness should be designed around the boundary you want, then give the agent as much computer as fits inside that boundary.


A sandbox usually does the reverse. It starts by deciding which abilities the provider feels comfortable exposing, then asks users to contort real work into that interface.


That is backwards.


## Sandboxes hide the hard parts


The hard parts of agent infrastructure are not` cat` and` grep` . The hard parts are state, networking, identity, persistence, concurrency, and debugging.


An agent working on a real project needs boring things:


- a filesystem that behaves like a filesystem
- processes that keep running after one command returns
- package managers that can install native dependencies
- ` systemd` or some equivalent for background services
- real users and permissions
- real networking and port binding
- SSH when the abstraction breaks
- snapshots when you need repeatability
- forks when you want parallel exploration
- hibernation when the user goes away


Most sandbox products try to turn those into platform features. "Use our package install API." "Use our preview API." "Use our process API." "Use our persistence API." "Use our logging API."


At some point you are not using a computer anymore. You are integrating with a proprietary operating system that happens to run code.


VMs are the boring alternative. The interface is Linux. Your process model is the process model. Your permissions are Linux permissions. Your package manager is the package manager. Your escape hatch is SSH. Your debugging story is every debugging tool that already exists.


This is less cute, which is why it works.


## VMs are the right primitive


The usual argument against VMs is that they are too slow, too expensive, and too heavy. Historically, fair. If the choice is between a VM that takes minutes to boot and a lightweight isolate that starts in milliseconds, the isolate is obviously attractive.


But that is an implementation detail, not a law of nature.


[Freestyle VMs](https://www.freestyle.sh/products/vms) start from memory snapshots, provision in under a second, and support live forking, pause/resume, SSH, systemd, users and groups, configurable networking, custom base images, and integrations for runtimes like Node.js, Python, Bun, Ruby, Java, Postgres, and more. The[VM docs](https://www.freestyle.sh/docs/vms) put it plainly: these are full Linux virtual machines designed for speed and flexibility.


That changes the tradeoff. You do not have to choose between "fast but fake" and "real but unusably slow." You can give every agent a real machine and still treat it like an elastic primitive.


That matters because the VM boundary is the correct security boundary. A VM can be isolated from the host while still giving the guest a real OS. A fake shell removes power by removing reality. A VM contains power by putting it behind a stronger boundary.


Those are not the same thing.


## VMs enabled us to create RigKit


This is why we built[RigKit, the open-source repo at github.com/freestyle-sh/rigkit](https://github.com/freestyle-sh/rigkit) , and why[Rig](https://rig.freestyle.sh/) can be more than another command runner.


RigKit is not just a "run this command" wrapper. A RigKit workspace can be a real development machine. We use RigKit to work on this exact website: its` rig.config.ts` builds a VM image with system packages, GitHub CLI, Node, Bun, git, build tools, and a real checkout. It snapshots that setup. Creating a workspace restores from the snapshot, checks out an isolated branch, starts the dev server, and opens the environment in` cmux` over SSH.


That workflow depends on VM-shaped things:


- snapshot a prepared machine, not just a directory
- preserve toolchains and package caches
- run a long-lived dev server
- expose localhost from inside the VM
- SSH into the machine when you want a real terminal
- give each workspace its own isolated Linux environment


You can fake parts of that with a sandbox. You can approximate a few more with enough provider APIs. But eventually the abstraction becomes a worse computer than the one Linux already gives you.


RigKit works because the primitive underneath it is a computer.


## Sandboxes make agents dumber


The weird thing about modern agents is that the model is often no longer the obvious bottleneck. The bottleneck is the world we put around it.


Give a strong coding model a repository, bash, internet access, a browser, a package manager, and tests, and it behaves like a junior engineer with a weirdly high typing speed. Give the same model three bespoke tools and a simulated shell, and it starts behaving like a chatbot again.


That is not because it forgot how to code. It is because you took away the feedback loops that make coding possible.


Software engineering is mostly observing reality:


- install the thing
- run the thing
- see the error
- search the code
- change the code
- run it again
- inspect the artifact
- repeat until reality agrees


Sandboxes often interrupt that loop at exactly the places where the work becomes real. They are optimized for the provider's control plane, not the agent's job.


## The right question


The right question is not "sandbox or no sandbox?"


Of course you need isolation. Of course you need limits. Of course you need audit logs, permissions, budget controls, network policy, identity, and human approval for dangerous operations.


The right question is: **what are you isolating?**


If you isolate a toy environment, you get a safe toy. If you isolate a real computer, you get a safe worker.


For narrow code execution, a lightweight sandbox can be perfect. If the job is "run this JavaScript function with no filesystem and no network," use an isolate. If the job is "evaluate this snippet and throw it away," use the smallest thing that works.


But for agents doing open-ended work, sandboxes suck because they make the unknown impossible. They require you to know the dependency graph before the task starts. They require you to decide which binaries matter before the model discovers the bug. They require you to convert a general-purpose computer into a product-specific checklist.


VMs are good because they let the agent discover what the task requires inside a boundary you control.


That is the future of agent infrastructure: not less isolation, better isolation. Not smaller computers, faster real ones.
