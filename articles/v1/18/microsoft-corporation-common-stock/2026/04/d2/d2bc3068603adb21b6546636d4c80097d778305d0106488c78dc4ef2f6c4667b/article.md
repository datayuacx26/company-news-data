---
schema_version: "1.0.0"
document_id: "d2bc3068603adb21b6546636d4c80097d778305d0106488c78dc4ef2f6c4667b"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0db2ae40c128"
canonical_url: "https://opensource.microsoft.com/blog/2026/04/09/how-drasi-used-github-copilot-to-find-documentation-bugs/"
published_at: "2026-04-09T15:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.025355+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:ac9380bc56319a5def83fdc07b1c82bafaf52635374e3a384e2f32829f1e9099"
---

# How Drasi used GitHub Copilot to find documentation bugs

For early-stage open-source projects, the “Getting started” guide is often the first real interaction a developer has with the project. If a command fails, an output doesn’t match, or a step is unclear, most users won’t file a bug report, they will just move on.


[Drasi](https://drasi.io/) , a CNCF sandbox project that detects changes in your data and triggers immediate reactions, is supported by our small team of four engineers in Microsoft Azure’s Office of the Chief Technology Officer. We have comprehensive tutorials, but we are shipping code faster than we can manually test them.


[Detect and react to your first database change using Drasi](https://drasi.io/drasi-kubernetes/getting-started/)


The team didn’t realize how big this gap was until late 2025, when GitHub updated its Dev Container infrastructure, bumping the minimum Docker version. The update broke the Docker daemon connection, and every single tutorial stopped working. Because we relied on manual testing, we didn’t immediately know the extent of the damage. Any developer trying Drasi during that window would have hit a wall.


This incident forced a realization: **with advanced AI coding assistants, documentation testing can be converted to a monitoring problem** .


## The problem: Why does documentation break?


Documentation usually breaks for two reasons:


### 1. The curse of knowledge


Experienced developers write documentation with implicit context. When we write “wait for the query to bootstrap,” we know to run \`drasi list query\` and watch for the \`Running\` status, or even better to run the \`drasi wait\` command. A new user has no such context. Neither does an AI agent. They read the instructions literally and don’t know what to do. They get stuck on the “how,” while we only document the “what.”


### 2. Silent drift


Documentation doesn’t fail loudly like code does. When you rename a configuration file in your codebase, the build fails immediately. But when your documentation still references the old filename, nothing happens. The drift accumulates silently until a user reports confusion.


This is compounded for tutorials like ours, which spin up sandbox environments with Docker, k3d, and sample databases. When any upstream dependency changes—a deprecated flag, a bumped version, or a new default—our tutorials can break silently.


## The solution: Agents as synthetic users


To solve this, we treated tutorial testing as a simulation problem. We built an AI agent that acts as a “synthetic new user.”


This agent has three critical characteristics:


1. **It is naïve** : It has no prior knowledge of Drasi—it knows only what is explicitly written in the tutorial.
2. **It is literal** : It executes every command exactly as written. If a step is missing, it fails.
3. **It is unforgiving** : It verifies every expected output. If the doc says, “You should see ‘Success’”, and the command line interface (CLI) just returns silently, the agent flags it and fails fast.


### The stack: GitHub Copilot CLI and Dev Containers


We built a solution using GitHub Actions, Dev Containers, Playwright, and the GitHub Copilot CLI.


Our tutorials require heavy infrastructure:


- A full Kubernetes cluster (k3d)
- Docker-in-Docker
- Real databases (such as PostgreSQL and MySQL)


We needed an environment that exactly matches what our human users experience. If users run in a specific Dev Container on GitHub Codespaces, our test must run in that **same** Dev Container.


### The architecture


Inside the container, we invoke the Copilot CLI with a specialized system prompt ([view the full prompt here](https://github.com/drasi-project/learning/blame/main/.github/prompts/tutorial-evaluation.md) ):


This prompt using the prompt mode (-p) of the CLI agent gives us an agent that can execute terminal commands, write files, and run browser scripts—just like a human developer sitting at their terminal. For the agent to simulate a real user, it needs these capabilities.


To enable the agents to open webpages and interact with them as any human following the tutorial steps would, we also install Playwright on the Dev Container. The agent also takes screenshots which it then compares against those provided in the documentation.


### Security model


Our security model is built around one principle: **the container is the boundary** .


Rather than trying to restrict individual commands (a losing game when the agent needs to run arbitrary node scripts for Playwright), we treat the entire Dev Container as an isolated sandbox and control what crosses its boundaries: no outbound network access beyond localhost, a Personal Access Token (PAT) with only “Copilot Requests” permission, ephemeral containers destroyed after each run, and a maintainer-approval gate for triggering workflows.


### Dealing with non-determinism


One of the biggest challenges with AI-based testing is non-determinism. Large language models (LLMs) are probabilistic—sometimes the agent retries a command; other times it gives up.


We handled this with a three-stage[retry with model escalation](https://github.com/drasi-project/learning/blob/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/workflows/tutorial-evaluation-scheduled.yml#L95) (start with Gemini-Pro, on failure try with Claude Opus),[semantic comparison for screenshots](https://github.com/drasi-project/learning/blame/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/prompts/tutorial-evaluation.md#L26C7-L26C7) instead of pixel-matching, and verification of core-data fields[rather than volatile values](https://github.com/drasi-project/learning/blame/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/prompts/tutorial-evaluation.md#L25) .


We also have a[list of tight constraints](https://github.com/drasi-project/learning/blame/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/prompts/tutorial-evaluation.md#L49-L49C12) in our prompts that prevent the agent from going on a debugging journey,[directives to control](https://github.com/drasi-project/learning/blame/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/prompts/tutorial-evaluation.md#L31) the structure of the final report, and also[skip directives](https://github.com/drasi-project/learning/blame/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/prompts/tutorial-evaluation.md#L43-L43C17) that tell the agent to bypass optional tutorial sections like setting up external services.


### Artifacts for debugging


When a run fails, we need to know why. Since the agent is running in a transient container, we can’t just Secure Shell (SSH) in and look around.


So, our agent preserves evidence of every run, screenshots of web UIs, terminal output of critical commands, and a final markdown report detailing its reasoning like shown here:


These[artifacts are uploaded](https://github.com/drasi-project/learning/blob/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/workflows/tutorial-evaluation-scheduled.yml#L156) to the GitHub Action run summary, allowing us to “time travel” back to the exact moment of failure and see what the agent saw.


### Parsing the agent’s report


With LLMs, getting a definitive “Pass/Fail” signal that a machine can understand can be challenging. An agent might write a long, nuanced conclusion like:


To make this actionable in a CI/CD pipeline, we had to do some prompt engineering. We explicitly instructed the agent:


In our GitHub Action, we then simply[grep for this specific string](https://github.com/drasi-project/learning/blob/b826ab8a7e1116a274d3a670a270516d9eaa81d2/.github/workflows/tutorial-evaluation-scheduled.yml#L143C11-L154C13) to set the exit code of the workflow.


Simple techniques like this bridge the gap between AI’s fuzzy, probabilistic outputs and CI’s binary pass/fail expectations.


### Automation


We now have an[automated version of the workflow](https://github.com/drasi-project/learning/blob/main/.github/workflows/tutorial-evaluation-scheduled.yml) which[runs weekly](https://github.com/drasi-project/learning/actions/runs/22225944239/) . This version evaluates all our tutorials every week in parallel—each tutorial gets its own sandbox container and a fresh perspective from the agent acting as a synthetic user. If any of the tutorial evaluation fails, the workflow is configured to file an issue on our GitHub repo.


This workflow can optionally also be run on pull-requests, but to prevent attacks we have added a maintainer-approval requirement and a \`pull_request_target\` trigger, which means that even on pull-requests by external contributors, the workflow that executes will be the one in our main branch.


Running the Copilot CLI requires a PAT token which is stored in the environment secrets for our repo. To make sure this does not leak, each run requires maintainer approval—except the automated weekly run which only runs on the \`main\` branch of our repo.


## What we found: Bugs that matter


Since implementing this system, we have run over 200 “synthetic user” sessions. The agent identified 18 distinct issues including some serious environment issues and other documentation issues like these. **Fixing them improved the docs for everyone, not just the bot** .


- **Implicit dependencies** : In one tutorial, we instructed users to create a tunnel to a service. The agent ran the command, and then—following the next instruction—killed the process to run the next command.
**The fix** : We realized we hadn’t told the user to keep that terminal open. We added a warning: *“This command blocks. Open a new terminal for subsequent steps.”*
- **Missing verification steps** : We wrote: “Verify the query is running.” The agent got stuck: “How, exactly?”
**The fix** : We replaced the vague instruction with an explicit command: \`drasi wait -f query.yaml\`.
- **Format drift** : Our CLI output had evolved. New columns were added; older fields were deprecated. The documentation screenshots still showed the 2024 version of the interface. A human tester might gloss over this (“it looks mostly right”). The agent flagged every mismatch, forcing us to keep our examples up to date.


## AI as a force multiplier


We often hear about AI replacing humans, but in this case, **the AI is providing us with a workforce we never had** .


To replicate what our system does—running six tutorials across fresh environments every week—we would need a dedicated QA resource or a significant budget for manual testing. For a four-person team, that is impossible. By deploying these **Synthetic Users** , we have effectively hired a tireless QA engineer who works nights, weekends, and holidays.


Our tutorials are now validated weekly by synthetic users.[try the Getting Started guide yourself](https://drasi.io/drasi-kubernetes/getting-started/) and see the results firsthand. And if you’re facing the same documentation drift in your own project, consider[GitHub Copilot CLI](https://github.com/features/copilot/cli) not just as a coding assistant, but as an agent—give it a prompt, a container, and a goal—and let it do the work a human doesn’t have time for.


[Get started with Drasi](https://drasi.io/drasi-kubernetes/getting-started/)
