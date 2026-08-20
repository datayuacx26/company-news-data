---
schema_version: "1.0.0"
document_id: "551c471045c8fa7bf0dd88bed03c79433d4c4a50bde21ba016cd307d22ac9f91"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/agent-container-use/"
published_at: "2025-06-14T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:a70c4d77cf06c80acf9c2d0c844ffc05f5304a262f69fd7ad2a3148304bff2e1"
---

# Containing Agent Chaos: Run Coding Agents in Parallel without Destroying Everything

One AI coding agent is magic. You ask it to refactor a function, write a unit test, or build a simple endpoint, and it just does it. The productivity boost is real. So, naturally, you think: what if I had ten?


Chaos ensues. Suddenly, you find yourself babysitting a team of hyperactive, uncoordinated robots. They step on each other’s toes, overwrite files, and create a tangled mess of dependencies.


You’re left with two frustrating options for scaling your agent workforce:


1.


**“YOLO Mode”** : You run all the agents in your local environment and hope for the best. What could possibly go wrong? (Spoiler: everything). Your machine becomes a battlefield of conflicting changes, and you spend more time cleaning up the mess than getting work done.


2.


**“Prompt and Pray Mode”** : You use a fully managed, all-in-one agent service. It’s a black box. You submit a request, wait for a pull request, and pray the result is what you wanted. When it’s not, you have to comment on the PR and repeat the cycle again, hoping for a better outcome on the next turn… You can’t easily intervene, use your own tools, or run it on your own infrastructure. You lose control.


There has to be a better way. We need a system that gives us background work, guardrails, efficient intervention, and optionality. The core technologies to build this—containers for isolation and Git for state management—already exist.


At[Dagger](https://dagger.io/) , we’ve spent years building a platform to solve exactly this kind of problem: turning complex software engineering tasks into programmable, portable workflows. We already had the engine to manage containerized operations as code. It was the perfect foundation to build the environment that agents were missing.


That’s why we built and open-sourced[Container Use](https://github.com/dagger/container-use) : an MCP (Model Context Protocol) server that gives each of your coding agents its own isolated, containerized development environments. It turns chaos into controlled, parallel execution.


## Container use for agents: Isolated dev environments for every task


Container Use is a simple command-line tool that plugs into any MCP-compatible agent like Claude Code or Cursor. It gives your agents a new superpower: the ability to create and manage their own dev environments on the fly, so they can work on multiple tasks or multiple versions of the same task in parallel.


Here’s an example where Claude Code uses Container Use to run two versions of the same task:


Here’s what container use enables:


### True isolation for parallel work


Each agent or task (if you ask) gets a fresh container, allowing it to parallelize development. Run multiple agents on different tasks simultaneously, and they won’t conflict. Or have the same agent try multiple attempts in parallel for the same task, so you could choose and merge the best one.


Imagine you want to refactor the backend while another agent updates the frontend dependencies:


```text
# Agent 1 starts refactoring the API in its own environment
> "Refactor the user authentication service"


# Agent 2 simultaneously works on the frontend
> "Upgrade all frontend npm packages to their latest stable versions"
```


### Full visibility with cu watch


See a complete, real-time log of what your agents are actually doing, not just what they claim. Every command and its output is recorded, giving you a perfect audit trail.


```text
# Watch all agent activity in real-time
# Basically `watch git log --remotes=container-use --oneline --graph --decorate`
cu   watch


# Review all commands used by the agent
git   log   --remotes=container-use   --notes=container-use
```


### Direct intervention when you need it


Is an agent stuck in a loop? Drop directly into its containerized terminal, see its exact state (files, processes, environment variables), and take control to fix the problem.


```text
# Jump into the agent's exact environment


cu terminal <environment_name>
```


### A git-backed workflow


Each containerized environment is backed by a Git branch, creating a persistent, versioned history of the agent’s work. This gives you a durable record that you can inspect and manage with familiar tools.


```text
# Inspect the code the agent wrote
git   log   --patch   container-use/  <  environment_nam  e  >
git   diff   container-use/  <  environment_nam  e  >


# Check it out to your local repository
git   checkout   <  environment_nam  e  >   &&   git   pull


# Happy with the result? Merge it into your main branch
cu   merge   <  environment-nam  e  >
```


In all, container use makes working with coding agents a much more pleasant and productive experience.


### Getting started


Container Use requires Docker and Git, and comes with Dagger under the hood. Install it with a single command:


```text
curl   -fsSL   https://raw.githubusercontent.com/dagger/container-use/main/install.sh   |   bash
```


Then, integrate it with your preferred AI coding assistant.


**For Claude Code:**


```text
cd /path/to/your/project
npx @anthropic-ai/claude-code mcp add container-use -- cu stdio
curl https://raw.githubusercontent.com/dagger/container-use/main/rules/agent.md >> CLAUDE.md
```


For other MCP-compatible agents and IDEs like Cursor, see integration guides in the[README](https://github.com/dagger/container-use) .


Container Use is in early development. We are actively working on improving stability and streamlining the user experience. Feedback, contributions, and[discussion](https://discord.com/invite/dagger-io) are all welcome!


## Powered by Dagger


While software engineers can use Container Use as a simple CLI tool with no Dagger experience required, for platform engineers it serves as a concrete example of the powerful workflow automations you can build with Dagger.


Container Use leverages Dagger’s core primitives to orchestrate these agent environments:


- **Composable Functions** : Agent environments are defined with code, not static images, allowing them to be dynamically composed with the exact tools needed for a task.
- **Portability** : The same environment an agent uses on your laptop can be executed identically in your CI system, because it’s all powered by the Dagger Engine.
- **Intelligent Caching** : Dagger’s content-addressable caching ensures that common operations are fast, even across dozens of parallel agent environments.


This is what Dagger is designed for: giving you the building blocks to create powerful, portable, and reliable workflows for software delivery and development.


[Get Container Use on GitHub](https://github.com/dagger/container-use)


[Learn more about the Dagger Engine](https://dagger.io/)
