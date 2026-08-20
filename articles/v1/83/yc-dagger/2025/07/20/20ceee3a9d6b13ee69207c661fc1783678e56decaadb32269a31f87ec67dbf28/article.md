---
schema_version: "1.0.0"
document_id: "20ceee3a9d6b13ee69207c661fc1783678e56decaadb32269a31f87ec67dbf28"
company_key: "yc-dagger"
company: "Dagger"
source_id: "yc-dagger-news-import-d3f1ddf31a06"
canonical_url: "https://dagger.io/blog/gemini-cli/"
published_at: "2025-07-03T00:00:00+00:00"
first_seen_at: "2026-07-21T15:47:35.732544+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:369d115970c7daabfe18c8790c8e895df1c0d89ce6870f44e87112949b41df66"
---

# Gemini CLI: First Impressions and Experiments with Parallelization

The Gemini team launched Gemini CLI launched last week (June 25, 2025), and is already taking off as one of the most popular AI agents. Gemini CLI is a CLI interface to Gemini LLMs that provides easy configuration for authentication, MCP servers, project settings, and more.


Based on my limited testing so far, here are my highlights for Gemini CLI:


- The UI is intuitive and helpful. I’m able to easily get help with interacting with the agent, see my current settings like model, context, sandboxing, workspace, memory, and MCP servers. There are command hints all over the UI that help me quickly understand how to use Gemini CLI. For the full set of options, I can type` /help` and get detailed information about the available commands.


- When I’m done with a session, Gemini CLI outputs a helpful summary of my session. It includes information about the turns, token usage, and duration.


-


I can easily configure rules for my current workspace as well as globally for my whole machine with settings in my ~/.gemini directory as well as the workspace .gemini directory. This allows me to commit project specific settings to my repository and additionally set my own preferences for everything on my machine.


-


Gemini CLI has built in sandboxing with options to sandbox with macos seatbelt or docker. This sandboxing provides an isolated execution environment to perform operations like running systems tools, running tests, or building your app. With the sandbox feature, filesystem edits are still made on your host filesystem, so I want to use[container-use](https://github.com/dagger/container-use) instead to give my agent a fully isolated environment for both the execution environment and git worktree. Integrating container-use was simple because I can select which core tools to include as well. For example, in my actual configuration included below, I specify that the only core tool I want to include is` ReadManyFilesTool` . I found this tool helpful because it lets me use` @` to reference specific files in my prompts to have those files included as context. I omit the other core tools because I want Gemini to do all of its file operations and shell commands in the container-use environment.


```text
{
"selectedAuthType"  :   "gemini-api-key"  ,
"theme"  :   "Dracula"  ,
"coreTools"  : [  "ReadManyFilesTool"  ],
"mcpServers"  : {
"container-use"  : {
"command"  :   "cu"  ,
"args"  : [
"stdio"
],
"timeout"  :   60000  ,
"trust"  :   true
}
}
}
```


- The block style output for each tool call gives me enough context to understand the work the agent is doing. In the screenshot below, I can see what the agent is thinking, tools it’s running, and the output of those tools in a clean interface.


Overall, Gemini CLI looks really promising already and I’m excited to see how it evolves!
