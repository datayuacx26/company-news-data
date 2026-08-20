---
schema_version: "1.0.0"
document_id: "90ef6eb1ad1911c71cf321cddd8beeb2706973b9ebc31d4aa9d4b1f2e5cce62a"
company_key: "yc-windmill"
company: "Windmill"
source_id: "yc-windmill-rss-12b6d71fe86e"
canonical_url: "https://www.windmill.dev/blog/launch-week-ai-sandboxes"
published_at: "2026-04-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:21:07.070377+00:00"
fetched_at: "2026-07-28T22:16:19.189952+00:00"
content_hash: "sha256:f943cb76aeb103bc4a0386232f55bb288eefb18db93c8ee1b16d34d8f4a5a7e9"
---

# AI sandboxes & volumes: isolated environments for coding agents

**Day 3 of[Windmill launch week](https://www.windmill.dev/launch-week-march-2026) .** You can now run AI coding agents like Claude Code or Codex in sandboxed environments with persistent storage, directly from your scripts and flows.


## The problem​


AI coding agents need two things that are hard to combine: isolation and persistence. You want them sandboxed so they cannot access the host filesystem or network. But you also want them to remember state across runs, produce artifacts, and pick up where they left off.


Teams end up managing Docker containers, mounting volumes manually, and writing wrapper scripts to handle session state. The orchestration layer has no opinion about where the agent runs or how its files persist.


## AI sandboxes: two annotations​


An AI sandbox is a regular Windmill script with two annotations: one for isolation, one for storage.


- TypeScript
- Python


```text
// sandbox      // volume: agent-state .agent         import   Anthropic   from     '@anthropic-ai/sdk'  ;      import     {   MessageStream   }     from     '@anthropic-ai/sdk/lib/MessageStream'  ;         export     async     function     main  (  prompt  :     string  )     {         const   client   =     new     Anthropic  (  )  ;         // The .agent directory persists across runs         const   result   =     await   client  .  messages  .  create  (  {         model  :     'claude-opus-4-6-20260401'  ,         max_tokens  :     1024  ,         messages  :     [  {   role  :     'user'  ,   content  :   prompt   }  ]  ,         }  )  ;         return   result  ;      }
```


```text
# sandbox      # volume: agent-state .agent         import   anthropic        def     main  (  prompt  :     str  )  :         client   =   anthropic  .  Anthropic  (  )           # The .agent directory persists across runs         result   =   client  .  messages  .  create  (             model  =  "claude-sonnet-4-20250514"  ,             max_tokens  =  1024  ,             messages  =  [  {  "role"  :     "user"  ,     "content"  :   prompt  }  ]  ,           )           return   result
```


` // sandbox` enables NSJAIL process isolation.` // volume: agent-state .agent` mounts a persistent volume synced to your workspace object storage. That's it.


## Persistent volumes​


Agents need to remember state across runs. Volumes handle this by syncing files to your[workspace object storage](https://www.windmill.dev/docs/core_concepts/persistent_storage) (S3, Azure Blob, GCS) between executions.


Declaring a volume is a single annotation:` // volume: <name> <mount_path>` . You can attach up to 10 volumes per script.


Each volume goes through three phases per execution:


1. **Before execution** : Windmill acquires an exclusive lease on the volume and downloads files from object storage. A per-worker LRU cache (up to 10 GB) skips the download when files haven't changed (compared by size and MD5).
2. **During execution** : the volume is bind-mounted into the sandbox. The agent reads and writes files normally.
3. **After execution** : changed files are synced back to object storage, metadata is updated, and the lease is released.


The exclusive lease (60-second TTL, auto-renewed every 10 seconds) guarantees that only one job writes to a volume at a time. If another job targets the same volume, it waits for the lease to be released.


Volume names support` $workspace` and` $args\[...\]` interpolation, so you can scope storage per workspace, per user, or per any input parameter. This makes it straightforward to give each agent session its own isolated storage.


Volumes also have fine-grained[permissions](https://www.windmill.dev/docs/core_concepts/volumes) : owner, read-only, or read-write access, assignable per user or group. A job with no permission on a volume will fail, so you control exactly which agents can access which data.


[Volumes Persistent file storage synced to object storage.](https://www.windmill.dev/docs/core_concepts/volumes)


## Process isolation​


Without isolation, an agent has access to the host filesystem, environment variables (including credentials), other running jobs, and unrestricted network. Windmill provides two levels of process isolation to prevent this.


**NSJAIL sandboxing** is the strongest option. Each execution runs in its own[NSJAIL](https://www.windmill.dev/docs/advanced/security_isolation) sandbox with:


- **Filesystem isolation** : the agent only sees its own working directory and mounted volumes. No access to the host filesystem or other jobs.
- **Network restrictions** : outbound network access can be restricted per sandbox.
- **Resource limits** : CPU, memory, and disk usage are capped per execution.


Enable it per script with the` // sandbox` annotation, or force it instance-wide for all jobs with` DISABLE_NSJAIL=false` .


**PID namespace isolation** is a lighter alternative for workloads where full sandboxing is unnecessary. It uses Linux` unshare` to create separate process namespaces, so each job gets its own process tree and cannot see or signal processes from other jobs. Enable it with` ENABLE_UNSHARE_PID=true` . Lower overhead, but no filesystem or network isolation.


[Security and isolation Process isolation, NSJAIL, and PID namespace isolation.](https://www.windmill.dev/docs/advanced/security_isolation)


## Works with any agent​


Claude Code, Codex, OpenCode, or any custom agent that operates on a local filesystem. Windmill provides the sandbox and the storage; the agent brings its own logic. A built-in Claude Code template handles session persistence and token counting out of the box.


## Built-in Claude Code template​


Windmill ships with a ready-to-use Claude Code template. It handles session persistence (the session ID is stored in the volume), agent instructions, skill files, and token counting for cost monitoring.


```text
// sandbox      // volume: claude-sessions .agent         import     {   ClaudeCodeAgent   }     from     '@anthropic-ai/claude-agent-sdk'  ;         export     async     function     main  (  prompt  :     string  )     {         const   agent   =     new     ClaudeCodeAgent  (  {         instructions  :     "You are a helpful coding assistant."  ,         }  )  ;         return     await   agent  .  run  (  prompt  )  ;      }
```


## Observability​


Every agent run is a regular Windmill job, which means full[observability](https://www.windmill.dev/platform/observability) out of the box: logs, execution history, and token usage for cost monitoring. Set up[alerts](https://www.windmill.dev/docs/core_concepts/critical_alerts) on failures or cost thresholds, and audit agent activity across workspaces.


## Use cases​


- **Persistent agent memory** : conversation history and session state survive across runs.
- **Artifact generation** : agents produce reports, code, or data files that persist in the volume.
- **Multi-step workflows** : a flow triggers an agent, waits for results, then passes artifacts to the next step.
- **Safe execution at scale** : resource limits and isolation let you run untrusted agent code without risk.


## Getting started​


1. Configure[workspace object storage](https://www.windmill.dev/docs/core_concepts/persistent_storage) (S3, Azure Blob, GCS, or filesystem).
2. Add` // sandbox` and` // volume: <name> <path>` annotations to any script.
3. Run it. Files in the volume path persist across executions.


[AI sandboxes Run agents in isolated environments with persistent volumes.](https://www.windmill.dev/docs/core_concepts/ai_sandbox)[Volumes Persistent file storage synced to object storage.](https://www.windmill.dev/docs/core_concepts/volumes)


## What's next​


Tomorrow is Day 4: **Git sync & workspace forks** . Sync with Git, stage workspaces, and deploy via CI/CD.[Follow along](https://www.windmill.dev/launch-week-march-2026) .


[Windmill](https://www.windmill.dev/) is an[open-source](https://github.com/windmill-labs/windmill) and[self-hostable](https://www.windmill.dev/docs/advanced/self_host/) developer platform to build, orchestrate, and monitor internal tools and data pipelines, combining the power of code with the velocity of low-code. We turn your scripts into internal apps and composable steps of flows that automate repetitive workflows.


You can[self-host](https://www.windmill.dev/docs/advanced/self_host/) Windmill using a` docker compose up` , or go with the[cloud app](https://app.windmill.dev/user/login) .
