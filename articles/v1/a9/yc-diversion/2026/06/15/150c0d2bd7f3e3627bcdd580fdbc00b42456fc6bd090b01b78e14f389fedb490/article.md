---
schema_version: "1.0.0"
document_id: "150c0d2bd7f3e3627bcdd580fdbc00b42456fc6bd090b01b78e14f389fedb490"
company_key: "yc-diversion"
company: "Diversion"
source_id: "yc-diversion-news-import-29544632d9e8"
canonical_url: "https://www.diversion.dev/blog/diversion-version-control-for-an-agentic-world"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-21T16:38:14.982062+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:4122dbd0d4d13d3b1d5ceb381b9e29ae4a8ff144ecdaa8fc566dd752e05136f7"
---

# Diversion: Version Control for an Agentic World

For years, version control has been treated almost like a commodity.


Git, GitHub, GitLab, and the broader ecosystem around them standardized not only the features teams expected, but also the way developers thought about collaboration. Git became the default. For many teams, it was the gold standard, and an entire generation of developers grew up accepting its limitations as part of the job.


Of course, this was never true for every industry. It is exactly why Diversion exists - and why it has grown in game development, where large files, massive repositories, binary assets, and ease of use are not edge cases. They are the daily reality. But in the last few months, something much bigger has started to shift.


The software development lifecycle is being rethought from its foundations. More and more code is being generated, reviewed, refactored, and managed with the help of AI agents. Developers are no longer only writing code directly; they are describing intent, reviewing outcomes, coordinating changes, and relying on agents to perform more of the actual implementation work. That changes the role of version control.


**Version control is no longer just the place where code history is stored. It is becoming part of the infrastructure that supports agentic development: multiple working copies, fast experimentation, huge repositories, new types of metadata, and collaboration patterns that were not designed around the assumptions Git was built on.**


In that world, teams using[Diversion](https://www.diversion.dev/) are already ahead of the game. Many of the limitations that new tools are now trying to fix on top of Git simply do not exist in Diversion’s architecture.


‍


## Distributed is no longer the default advantage


When Git entered the scene in 2005, its ability to work in a distributed way was a major breakthrough, especially for open-source projects. Developers could clone a repository, work offline, commit locally, and later push their changes back.


But in many professional environments, the real shift was not only “distributed.” It was branching and merging done properly. Git changed how developers thought about parallel development, experimentation, and collaboration. Then GitHub popularized the pull request model. You fork an open-source project, push changes to your fork, and ask the maintainers of the original project to pull those changes in.


That model worked beautifully for open source. Inside companies, however, most developers were not really forking projects in the same way. And in many industries, especially game development, the “commit and push” model introduces friction for non-technical users. Artists, designers, and other contributors often ask a very reasonable question: why can’t I just commit my work and move on?


Fast forward to 2026, and local clones are starting to look less like a superpower and more like an inherited constraint.


**Most devices are always connected. Most development environments depend on cloud services, CI systems, AI coding platforms, and connected workflows anyway. The old promise of “work completely offline and push later” is simply less relevant for many modern teams.**


And then there is the agentic layer.


Agents often need to create multiple working copies, explore different implementation paths, test ideas, compare outputs, and move quickly between branches or tasks. In Git-based workflows, that can mean duplicating repository state, managing worktrees, or relying on advanced features that were never exactly designed to be simple or mainstream.


That is why Git worktree, once a relatively hidden power-user feature, is suddenly becoming much more relevant. Diversion does not need that workaround.


**With Diversion, there is no need to clone the entire repository history into every local working copy. The repository lives centrally, and teams can create as many working copies as they need without duplicating the full history again and again.**


Need 30 new working copies for agentic experimentation? Diversion’s model is already built for that. No local replica. No unnecessary disk pollution. No extra mental model around commit, push, pull, fetch, clone, and worktree. Just a centralized version control system designed for scalable collaboration.


And this is not a step backward. It is the same architectural direction used by some of the largest engineering organizations in the world to support gigantic monorepos at scale.


## Monorepos are becoming the natural default


There is more code being created than ever before.


Repositories are not going to get smaller. AI agents make it easier to generate new features, refactor large areas of a codebase, create tests, write documentation, and produce supporting assets. The result is simple: development output is increasing, and version control systems need to keep up.


But many Git-based workflows were not designed for gigantic trees, massive binary assets, or the kind of repository scale that modern teams increasingly need. That is especially true in game development, virtual production, simulation, and other creative technical industries, where the repository is not just source code. It includes engine files, art assets, scenes, textures, models, audio, design files, and many other large binaries.


When the version control system struggles with that reality, teams are forced to design around the tool.They split repositories. They introduce submodules. They add Git LFS. They build custom scripts. They create rules, exceptions, and workflows that exist mostly because the underlying version control system cannot comfortably support the way the project naturally wants to grow.


Sometimes submodules are the right choice. Component-oriented development is real, and there are cases where clear boundaries make sense. But often, these structures are not strategic architecture decisions. They are workarounds.


**And in an agentic world, workarounds become even more expensive.**


When development moves faster, teams need fewer artificial boundaries, not more. They need to be able to throw everything into one place, let the project grow, and trust the version control system to handle scale.


That is where Diversion’s architecture matters. Diversion is built for large repositories, large files, and teams that need to collaborate across code and assets in one workflow. Instead of forcing teams to adapt their development lifecycle to the limitations of the tool, Diversion gives them a simpler model: keep the project together, let the repository grow, and focus on building.


**The future is not smaller repositories. The future is version control that can handle what modern teams and modern agents create.**


## The cost of changing version control is dropping


In the past, even teams that knew GitHub was not the right long-term solution often stayed where they were, not because the tool was perfect, but because moving felt expensive.


The team “knew Git,” or at least someone on the team knew how to undo the more painful Git mistakes. There were scripts, CI pipelines, custom workflows, integrations, documentation, and years of accumulated habits built around GitHub or Git-based systems - so even when the pain was obvious, migration felt too risky.


**That barrier is changing. Agents make it easier to adapt scripts, update workflows, translate commands, and help users operate in unfamiliar environments. The cost of learning a new tool is lower when users can simply ask an agent how to perform an action, fix a command, or automate a process.**


That matters for version control, Diversion behaves in ways that are familiar to teams coming from Git, but without much of the operational complexity that comes with Git-based workflows at scale. From the command line, agents can understand and operate Diversion actions naturally. For users, especially non-technical contributors, the experience is even simpler through the UI.


In the past, “our team already knows Git” was one of the strongest arguments against changing version control. In an agent-assisted world, that argument becomes weaker.


**The question is no longer only, “What does the team already know?” the better question is, “What version control system is actually designed for the way we are going to build next?”**


## New primitives for a new world


This is where things get even more interesting, for decades, version control has mostly treated code changes as the main artifact worth preserving. Commits, diffs, branches, merges, and reviews were all centered around changes to files.


But when humans increasingly produce prompts, specifications, instructions, and high-level intent - while agents produce or modify the code - the important artifacts start to change. The code still matters, of course, but so does the reasoning behind it.


Why was this change made? What prompt led to it? What context did the agent use? Which alternatives were considered? What was the original intent? What assumptions were made? What should future agents know before touching this area again?


These are not small questions.


They point to a future where version control may need to preserve much more than file changes. It may need to capture prompt provenance, decisions, reasoning traces, implementation context, and metadata that helps both humans and agents understand how a project evolved.


Trying to force all of that into a Git-based model may work at first, but it is not ideal. Git is powerful, but its protocol and storage model were built for a different era. Extending it in deep ways is hard. Many future-facing features may require workarounds, hacks, or metadata stored awkwardly inside the repository itself.


[Diversion](https://www.diversion.dev/) has a different advantage. It is a full-stack version control system, not a thin layer on top of Git. That means the team controls the product end to end: the storage model, the protocol, the user experience, the collaboration layer, and the primitives that can be introduced next.


In the past, not being Git was often seen as a disadvantage. Developers had a Git-focused mindset, and many teams were reluctant to adopt anything outside that familiar ecosystem, but the world is changing.


**As agents become part of everyday development, learning a new version control system is no longer the same barrier it used to be. Users can get help instantly. Workflows can be adapted faster. Custom scripts can be rewritten more easily. Teams can focus less on preserving old habits and more on choosing the infrastructure that fits the future. That freedom is powerful.**


Diversion does not need to hack around Git in order to evolve. It can introduce new capabilities directly, quickly, and natively. And in a world where software development is changing week by week, that speed matters.


## The version control shift has already started


The rise of AI agents is not just changing how code is written. It is changing what development teams need from their tools.


They need version control that can handle more working copies.


They need repositories that can grow without constant architectural compromises.


They need workflows that are simple enough for humans and agents to use.


They need support for large files and massive projects by design, not through layers of workarounds.


And soon, they will need new primitives for tracking the intent, context, and reasoning behind agent-generated work.


**Git shaped the last generation of software development, but the next generation will not be limited by the assumptions of the last one.**[Diversion](https://www.diversion.dev/) **was built outside those assumptions - and that is exactly what makes it ready for the agentic world.**


‍
