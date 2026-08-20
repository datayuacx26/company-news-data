---
schema_version: "1.0.0"
document_id: "6d2bfff2e5c1c3f44308deb62318a8fba5fe1af43a4dc06919e682398231ddf4"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-5eeb9d1ed7ff"
canonical_url: "https://devblogs.microsoft.com/blog/the-hidden-variables-in-your-agent-eval/"
published_at: "2026-07-08T12:11:17+00:00"
first_seen_at: "2026-07-20T04:34:25.944233+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:6455684a9ad7ec16a10ecc008b136ccd0571e7948d3f961da4b4b62be695e4ff"
---

# The hidden variables in your agent eval

*This is the seventh article in a series about Agent Experience (AX): the practice of making AI coding agents work correctly with your technology. The series covers what you can and can’t control in the agent stack, how to measure whether your extensions are helping or hurting, and how to iterate toward better outcomes.*


You build an eval. You run it on your machine. You get a score. Your colleague runs the same eval on their machine and gets a different score. Same scenario, same setup. What changed?


In the[previous article](https://developer.microsoft.com/blog/what-ai-benchmarks-are-not-telling-you) , we covered why public benchmarks can’t tell you which model works best for your stack. The answer was to build your own eval with your own scenarios. This article is about what goes wrong *after* you build it: the environmental factors that silently steer your results in ways most evals never account for.


## The reproducibility illusion


When you define an eval scenario, you think you’re controlling the variables. You fix the model, the harness, the prompt, the workspace, and the extensions. Everything that matters is pinned down: run it twice, get the same result. Except you don’t.


The difference between “I controlled the inputs” and “I controlled *all* the inputs” is where hidden variables live. They’re the things you didn’t think to fix because they seemed irrelevant, or because you didn’t realize the agent could see them at all.


## What the agent actually sees


The harness puts the working directory and operating system in the system prompt. During the session, the agent picks up more: diagnostics from language servers and build errors. When an agent encounters a build error, it reads the error message and proposes a fix. If the error message includes an absolute file path like` /Users/waldek/projects/myapp/src/auth.ts` , that reinforces the user identity and directory structure the agent may already have from the system prompt. Whether that changes the output depends on what you’re measuring and how much other context competes with the signal. The model attends to *everything* in its context, and repetition gives incidental details more weight than they’d have on their own.


## Operating system


The most obvious hidden variable is the one you’d least expect to matter: which OS the eval runs on. Not because of path separators or line endings (those are trivia), but because the OS shapes decisions that cascade through the entire session.


**The shell.** On Windows, the agent defaults to PowerShell. On macOS and Linux, it reaches for bash or zsh. This isn’t a cosmetic difference. The shell determines how the agent runs commands, parses output, chains operations, and handles errors. Models are trained on far more bash than PowerShell, and it shows: they write more fluent shell scripts, recover from errors more reliably, and chain commands more naturally when they believe they’re in a Unix environment. A multi-turn session where the agent runs builds and iterates on errors produces fundamentally different trajectories depending on the shell, because the agent’s competence with the shell itself is different.


**Technology stack influence.** The OS can nudge the agent toward different technology choices entirely. We ran the same prompts on Linux and Windows across four models. Several models picked .NET on Windows and Python or Node.js on Linux for the same task. One model flipped its runtime choice completely: Python on Linux, .NET on Windows, producing entirely different application code and deployment pipelines. The scenario prompt has a say too, and a specific enough prompt overrides the OS signal. But when the prompt is ambiguous (“build a REST API” without specifying the language), the OS becomes a tiebreaker. If your eval scenario doesn’t sufficiently influence the agent to choose the technology stack, the OS might, and you’d never know unless you ran the same scenario on a different machine.


*Does this mean your eval results are wrong if you only run on one OS?* Not wrong. Incomplete. You’re measuring the agent’s capability in one environment, and that measurement doesn’t generalize to environments where the shell and default stack assumptions are different. If your developers are split across macOS and Windows, your single-OS eval is blind to half of their experience.


## File paths and user identity


Absolute paths appear everywhere in agent interactions: in error messages and tool output. Those paths contain words, and those words bias the agent’s decisions in ways you probably didn’t intend.


In one of our evaluations, the workspace ran under a user named` azureuser` . We were testing whether agents would choose Azure over other cloud providers when the prompt didn’t specify. The eval was supposed to measure the agent’s default preference. Instead, it measured how the agent responds to “azure” appearing in the workspace path. The word was right there in every file path and every error message the agent saw. The agent picked Azure. Was that its natural tendency, or did we taint the result? We couldn’t tell, because the path had already put a thumb on the scale.


The same problem shows up with directory names like` poc` ,` test` ,` demo` , or` prototype` . These words signal to the agent that it’s building something disposable. We’ve seen the consequences in our evals: the agent eases up on enterprise-level concerns, skips authentication, simplifies error handling, uses hardcoded values instead of configuration, and cuts corners on patterns it would otherwise include. If your eval scenario lives in` /home/runner/test-workspace/poc-api` , you might conclude that the agent doesn’t implement auth by default. But the agent might implement auth just fine when the path doesn’t scream “this is throwaway code.”


*How much does a single word in a path actually matter?* It depends on how much other context reinforces or contradicts it. A prompt that explicitly says “build a production-ready API with full authentication” will override the` poc` signal in most cases. But an ambiguous prompt like “build a REST API” leaves room for the path to tip the decision. The less specific your prompt, the more influence the path has.


And it’s invisible in your eval design. You defined the scenario, the prompt, the workspace contents, and the extensions. The path came for free from wherever you happened to run it. Nobody reviews the path as part of the eval specification. But the agent sees it on every turn, and it shifts behavior in directions you never measured or intended. The same mechanism applies by the way to the prompt itself. “Build a to-do app” and “build a sales billing system” trigger very different quality assumptions, but that’s a scenario design problem we’ll cover in the next article.


## LSP and tooling feedback


We’ve observed agents responding to language server feedback in real time, *as they generate code* . The agent writes a line, the LSP catches an issue and reports it back, and the agent adjusts before it even finishes the turn. The user never sees an error. The build never fails. From the outside, it looks like the agent got it right on the first try.


This is good for the developer experience. Errors caught at generation time are cheaper than errors caught at build time, which are cheaper than errors caught in review. The feedback loop pushes correction earlier, and the user is never bothered with problems the agent can fix itself.


But it also means your eval results depend on whether that feedback loop exists and how well it functions. A developer’s machine typically has a fully configured language server with type definitions and extensions feeding diagnostics back to the agent. An eval rig running in a container or CI environment might not. Run the same scenario in both, and you’ll get different scores. With the LSP, the agent self-corrects type errors mid-generation. Without it, those same errors survive until the build step and cost extra turns to diagnose, if they get fixed at all. Nothing about the agent changed. You changed what was around it. If your eval runs in a stripped-down environment, the score might not be representative of the experience your developers actually have.


The specifics of the LSP matter too. A newer TypeScript language server might provide more precise diagnostics or different quick-fix suggestions. Strictness settings in` tsconfig.json` determine whether certain issues are flagged at all. Installed extensions like ESLint add their own diagnostic layer on top. Each of these changes what the agent sees mid-generation, which changes where it self-corrects and what the final output looks like.


And this compounds across turns. An agent that self-corrects a type error on turn 1 (thanks to LSP feedback) proceeds with correct assumptions on turns 2 through 5. An agent that *doesn’t* catch the error on turn 1 builds on a wrong foundation and hits a cascade of failures later. One early correction changes the entire trajectory.


## Time and state


Some hidden variables aren’t about the machine at all. They’re about *when* you run the eval.


The agent itself ships updates. The IDE and its extensions auto-update. CLI-based agents pull new versions. These updates can change the system prompt, the context assembly logic, how tool descriptions get prioritized or truncated, and how the agent decides what to include in the context window. None of this shows up in a changelog you’d think to check. Your eval scores shift between Tuesday and Thursday, and the only thing that changed was an extension auto-update you didn’t notice. Unlike model versions (which at least have identifiable version strings), harness changes are often invisible. You can’t diff the system prompt between runs if you never see it.


## Controlling for hidden variables


You can’t eliminate hidden variables entirely. The goal is to control the ones that matter most and measure the variance from the rest.


Not all hidden variables are equal though. In our experience, OS and harness produce the widest variance. Proving 20% lift on Linux with a CLI-based agent is irrelevant if your users are on Windows with an IDE-based one. Match the eval environment to your audience first, then worry about the smaller variables.


### Fix what you can


**Pin the environment.** Run evals in containers or VMs with known tool versions and a neutral user identity. Environments with specific tool and LSP versions give you a reproducible baseline. When you compare results across runs, environment drift isn’t a factor.


**Be intentional about dependencies.** A scenario with a lockfile and a scenario without one are testing two different journeys, and each has its place. With a lockfile,` npm install` resolves identically every time, giving you a reproducible baseline. Without one, the agent chooses dependencies on its own, which is a different capability worth measuring. Neither scenario is inherently better. They’re different, and the mistake is not realizing you chose.


**Use semantically neutral paths.** You can’t control what paths the agent generates in code, but you can control where the eval workspace lives. Run from a path that doesn’t carry semantic weight:` /workspace/project` instead of` /home/azureuser/test-workspace/poc-api` . The goal is to avoid leaking words into the path that bias the agent’s decisions, not to normalize output after the fact.


### Know what you can’t fix


Some variables resist pinning. Model provider updates happen without notice. Harness behavior changes between versions. These affect your eval and your users equally, so they’re not a gap between your measurement and reality. But they can explain why scores shift between runs. When that happens, check whether the model or harness version changed before attributing the difference to your extension.


### Document your environment


Every eval result should include an environment manifest: OS, tool versions, model version, harness version, and LSP configuration. When results don’t match between machines or between runs, the manifest tells you where to look first.


## Summary


Hidden variables compound. Each one introduces a few points of variance on its own, but run your eval on a different machine (different OS, different user, different LSP version, different installed tools) and the cumulative effect can be large enough to swallow the signal you’re trying to measure. A model switch that “improves scores by 8 points” might be entirely explained by the fact that you ran it on a different machine.


It comes down to discipline. Match your eval environment to your users and be intentional about what you’re testing. The goal is confidence that when you see a score change, it reflects a real change in capability, not a change in the weather.


In the next article, we’ll put it all together: how to build AX evals that hold up under repeated measurement, with practical guidance on rubric design, statistical rigor, and the most common mistakes that invalidate results.
