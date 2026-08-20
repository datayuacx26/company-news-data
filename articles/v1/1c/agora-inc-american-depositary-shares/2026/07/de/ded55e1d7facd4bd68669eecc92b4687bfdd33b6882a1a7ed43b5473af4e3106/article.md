---
schema_version: "1.0.0"
document_id: "ded55e1d7facd4bd68669eecc92b4687bfdd33b6882a1a7ed43b5473af4e3106"
company_key: "agora-inc-american-depositary-shares"
company: "Agora Inc."
source_id: "agora-inc-american-depositary-shares-news-import-bf245f251fd4"
canonical_url: "https://www.agora.io/en/blog/introducing-the-agora-cli"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-23T01:17:52.872297+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:dacde09a1969c64e9df622662d17643d312c2b7ff909d2a394dce9789f399e9e"
---

# Introducing the Agora CLI

*A command-line interface for developers and AI coding agents building with Agora.*


AI coding agents can create apps, but the initial setup still requires human intervention: creating a project, retrieving project credentials, creating the env file without exposing secrets to the agent context, and choosing the right quickstart so it doesn't generate a bunch of boilerplate code. Developers hit the same loop. You open the console, copy values, switch back to the terminal, create the env file, then search for the quickstart link so the agent has a good example to start with.


Today we're releasing the **Agora CLI** to move that setup path into the terminal. Developers, scripts, and agents can use the CLI to write environment files, scaffold starter repos, and check Agora environment readiness without leaving the workflow they're already in. Most CLI tools print human-readable output, which leaves coding agents parsing loose terminal text. The Agora CLI supports both humans and agents, with a JSON output mode that returns structured responses an agent can parse and act on. We built the CLI for our own agentic development workflows. Now we're releasing it so Agora developers can give their agents the same ability to provision projects, write env files, and keep setup inside the terminal.


The shortest path from install to runnable project is three commands:


```text
curl -fsSL https:  //agoraio.github.io/cli/install.sh | sh
agora login
agora init my-python-demo --template python
```


` agora init` creates or binds an Agora project, clones the selected quickstart, writes the env file that quickstart expects, records the repo-local binding in` .agora/project.json` , and prints the next steps. Templates ship for Next.js, Python, and Go.


## The Command Surface


The CLI separates account-level actions from local project setup:


```text
agora
├── init        # create or bind a project, clone a quickstart, write env
├── project     # projects, features, credentials, readiness
├── quickstart  # starter repos and template-aware env files
├── auth        # login, logout, session status
├── doctor      # install and project diagnostics
└── mcp         # run the CLI   as   a local MCP server
```


Use` init` when you want the whole onboarding path in one command. Drop into` project` and` quickstart` when you want to inspect or control each step:


```text
agora project create my-agent-demo --feature rtc --feature convoai
agora quickstart create my-python-demo --template python --project my-agent-demo
agora quickstart env write my-python-demo --project my-agent-demo
```


The CLI also covers the daily checks developers reach for:


```text
agora -v         # show the installed version
agora help       # learn commands and flags
agora whoami     # check the current login
agora doctor     # check the local install and project setup
agora upgrade    # upgrade the CLI
```


The CLI keeps the quickstart, credentials, and project binding aligned. It writes the env file the selected template expects, preserves values you've already added, and remembers which Agora project belongs to the repo so future commands don't drift into the wrong setup. Under the hood, the CLI stores repo-local project metadata in` .agora/project.json` . When you run another command from that repo, the CLI uses that binding before falling back to a global project selection.


## Built for Agents, Useful for Humans


[Agora Skills](https://www.agora.io/en/blog/agora-skills-build-voice-ai-with-your-coding-agent/) gives an AI coding assistant the context to understand Agora's products and integration patterns. The CLI gives that assistant a way to act.


That matters because setup work is full of external dependencies: project configurations, env file setup, feature readiness, quickstart bindings, and local diagnostics. A coding agent can write source code from documentation, but it can't complete a real setup unless it can also ask the platform what exists and change what needs to change.


For agent and automation workflows, use` --json` to return structured data instead of human-formatted terminal output:


```text
agora doctor --json
agora auth status --json
agora project env --json
agora introspect --json
```


In JSON mode, commands return fields a script or agent can inspect directly, such as auth state, project metadata, env values, diagnostics, and command capabilities.


` agora introspect --json` exposes the command tree in a machine-readable form, including which commands are safe for headless runs.` agora mcp serve` lets MCP clients call the CLI through local tools instead of scraping terminal text.


The console remains the visual control plane. The CLI gives developers, scripts, and agents a reliable path through the same setup work from the terminal.


## Install


```text
curl -fsSL https:  //agoraio.github.io/cli/install.sh | sh
```


On Windows, install with PowerShell:


```text
irm https:  //agoraio.github.io/cli/install.ps1 | iex
```


The installer configures PATH and shell completion by default.


After installing,` agora doctor` checks the local CLI setup, including the resolved binary, version, PATH, network access, auth state, and project readiness.


## Get Started


After you install the CLI, and run` agora login` , you can start from one of the official quickstarts:


```text
agora init my-python-demo --template python
agora init my-nextjs-demo --template nextjs
agora init my-go-demo --template go
```


Full CLI documentation is available at[Agora CLI Docs](https://docs.agora.io/en/introduction/agora-cli) .


This is the first release. If a command is missing or a setup path still forces you back into manual project work,[open an issue](https://github.com/AgoraIO/cli/issues) . That feedback should shape the next version.
