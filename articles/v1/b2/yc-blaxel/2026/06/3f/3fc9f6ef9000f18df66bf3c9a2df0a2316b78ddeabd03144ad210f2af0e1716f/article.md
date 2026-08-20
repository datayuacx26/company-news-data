---
schema_version: "1.0.0"
document_id: "3fc9f6ef9000f18df66bf3c9a2df0a2316b78ddeabd03144ad210f2af0e1716f"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/choose-the-right-storage-for-your-blaxel-agents"
published_at: "2026-06-02T07:34:57+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:49:52.299135+00:00"
content_hash: "sha256:28cb8673f45c27d9b5989807c005c8700fbfe4d803d6212cb13221c2e2373ec4"
---

# Choose the right storage for your Blaxel agents

When building agents or agentic workflows, data persistence is an overlooked - but usually critical - factor.


Whether your agents are generating code, analyzing logs, sharing context between workflows, managing tool call outputs, or processing data, chances are they're going to need somewhere to store their work. And so the questions begin: where should that data live? Is it disposable once the session finishes, or should it stick around? Will more than one agent work on the same file at the same time? Will the output of one agent be needed as the input for another?


The decisions you make about data storage can impact your agent's performance, reliability, and overall consistency. Depending on whether your agents need speed, durability, or concurrency, Blaxel offers three distinct storage options, all designed for high performance and scalability.


## Sandbox Filesystem


If your agents need fast access to in-workflow session data, generated outputs, or persistent development environments, the[sandbox filesystem](https://docs.blaxel.ai/Sandboxes/Filesystem) is usually the best choice. This is a stateful filesystem optimized for speed, and it exposes interfaces for Blaxel SDKs to manage files and directories in the sandbox.


### Features


The sandbox filesystem has some unique features:


- The filesystem is hybrid, consisting of an[in-memory writable layer with a read-only base image](https://blaxel.ai/blog/how-to-slash-sandbox-memory-usage-by-75-using-overlayfs) . As a result, all writes go directly to RAM.
- When a sandbox goes into standby, Blaxel automatically creates and saves a snapshot of its entire state (including files and running processes). When the sandbox resumes, the full state is restored.
- Sandboxes resume from standby in under 25ms and automatically scale to zero after a few seconds of inactivity.
- The filesystem is accessible over both a REST API and an MCP server.


> Think of a sandbox like your laptop: when you close the lid, your data and processes pause, but nothing is lost because they are all still in memory. Then, when you open the lid, all your data and running processes resume from memory instantly.


### Benefits


The benefits of the sandbox filesystem are:


-


Extremely fast access, because all writes go directly to RAM, without network or disk I/O. It is specifically optimized for small file reads and writes, which are most commonly seen in agentic workloads. We have observed read speeds of up to 6,600 MiB/s and write speeds of up to 2,500 MiB/s, both with buffered I/O at at 1 MiB block sizes.


-


No manual provisioning required for data storage.


-


25ms resume times and automatic scale-to-zero make it possible to keep sandboxes on standby indefinitely when not used, eliminating cold starts without complex orchestration.


### When to use it


The sandbox filesystem is ideal for resumable “working sessions”, where data needs to persist between bursts of activity. Here are some examples:


-


DevOps agent: An AI agent monitors CI logs and test failures, then spawns a sandbox with a complete dev environment on demand when it spots something worth investigating, such as a failed test run or a performance regression. The agent checks out the repository, investigates, and recommends a solution. The sandbox remains in standby. If the agent needs to follow up, or if it detects another failure in the same project, it simply resumes the sandbox and pulls the latest code, without rebuilding the entire dev environment from scratch.


-


Quantitative research agent: An AI research agent receives a request - for example, "analyze the risk-adjusted returns of this portfolio over the last 5 years." It creates a sandbox from an image that already has tools like pandas, NumPy, statsmodels, and matplotlib installed, reads the raw data supplied by the user, and writes Python scripts to compute risk metrics and ratios. Then, it analyzes the final output and emails a PDF report to the requester. The sandbox remains in standby. If the user follows up with another question on the same topic - for example, “re-analyze the data for the last 3 years” - the agent resumes the same sandbox, reuses its earlier scripts, and performs a fresh analysis without needing to re-download data or rebuild the entire context.


## Volumes


Data stored in a sandbox persists for as long as the sandbox exists in standby, but is erased when the sandbox is terminated. For data that can’t be easily rebuilt or that needs to persist for longer than a single session, volumes are the right choice. Examples of this data include databases, in-progress code, decision histories, agent memories, and so on.


Blaxel[volumes](https://docs.blaxel.ai/Volumes/Overview) provide durable, cost-effective storage. Files stored on a volume survive even if the associated sandbox is terminated, and newly-created sandboxes can access the data by attaching to the same volume.


### Features


Volumes offer rich features to meet durable storage requirements:


- They are implemented using high redundancy block storage that is decoupled from the sandbox lifecycle.
- They can be initialized from templates to pre-populate the filesystem automatically.
- Their size can be expanded after creation without any data loss.


> A volume is like an external hard drive: if your laptop crashes unexpectedly, you can recover your data by moving the external drive to another computer.


### Benefits


The benefits of volumes are:


- They provide a way to persist application or agent state beyond the sandbox lifecycle.
- They are backed up regularly, ensuring that data is protected against infrastructure failures and accidental deletion.
- Volume templates enable consistent, fully-populated environments, without requiring manual package downloads or configuration changes on every new sandbox creation.
- We have observed read speeds of up to 2,700 MiB/s with direct I/O at 4 GiB block sizes and write speeds of up to 1,200 MiB/s with direct I/O at 1 MiB block sizes.


However, relying solely on volumes for state management comes with a trade-off. If you delete a sandbox and rely on re-attaching a volume to a new one, you lose the "instant resume" benefit of the previous section. You will incur a 250ms cold-boot overhead to create the new sandbox and will need to restart your processes.


### When to use it


Volumes enable stateful environments and data retention across lifecycle events. This makes them the correct choice for critical user data, databases, or project files. For example:


-


Website builder agent: An AI application builder lets users generate full-stack applications with natural language prompts. Since typical user sessions span multiple hours or days, it provisions one volume per end-user application to store the in-progress code and installed dependencies. This volume backup is the source of truth for the project. Users can quickly resume sessions from their last checkpoint, or add features to their codebase incrementally.


-


Code review agent: An AI code reviewer maintains a clone of a customer’s GitHub repository in a volume. When a code review request is received, the system creates a sandbox, mounts the volume with the repository, pulls all the latest changes into it, and starts an agentic code review session. This improves session turnaround time, as fully cloning the entire repository from scratch on each agent run is time-consuming. Using a volume (instead of the sandbox filesystem) to store the repository state is more efficient; you don’t need to keep the entire environment on standby and can optimize your storage costs by using “cold” storage (volumes) instead of “warm” snapshots.


## Agent Drive


When multiple agents or sandboxes need to access the same files concurrently, or when data must be shared across workflows, Blaxel[Agent Drive](https://docs.blaxel.ai/Agent-drive/Overview) (currently in private preview) is the right fit.


Unlike volumes, which are storage devices attached at sandbox creation to a single sandbox, Agent Drive is a shared, distributed filesystem that can be mounted to multiple sandboxes or agents at any time.


### Features


Agent Drive offers a number of unique features:


- Multiple sandboxes can mount the same Agent Drive simultaneously with full concurrent read-write access.
- Drives expose an S3-compatible interface (coming soon!) and a FUSE filesystem mount for a POSIX-compliant interface. The POSIX interface supports all common Linux filesystem functions (mandatory locks and POSIX ACLs are not supported at the time of writing).
- An optimized FUSE client built specifically for this filesystem is pre-installed in every sandbox.
- Drives can be attached to already-running sandboxes at any mount path, without needing to recreate the sandbox.
- Unlike eventually-consistent object stores, Agent Drive behaves like a normal filesystem. File writes either succeed or fail, and written files are immediately readable.
- Built-in replication and backup is included.


> If a sandbox is like a laptop and a volume is like an external drive, Agent Drive is like a cloud drive for sandboxes. It allows multiple agents to collaborate across sandboxes and simultaneously access the same files.


### Benefits


The benefits of using Agent Drive are:


- It enables true multi-agent collaboration. Agents can run in separate sandboxes but still share files, context and memory.
- No manual provisioning or run-time resizing is required. Drives scale automatically with no fixed capacity limits.
- It has built-in replication, and monitoring to automatically detect and repair under-replicated and over-replicated volumes.


We have observed read speeds of up to ~1,200 MiB/s at 4 GiB block sizes and write speeds of up to ~950 MiB/s at 1 MiB block sizes, both with direct I/O.


### When to use it


Agent Drive is highly optimized for the small and medium-sized file operations typical of agentic workflows. It is the ideal solution for sharing data between long-running agents. Here are some examples:


-


Multi-tenant agentic platform: A multi-tenant AI agent platform needs to spin up multiple agents per tenant and share context between them. The platform creates a separate Agent Drive per tenant, which stores all state related to that tenant. Multiple agents running for the same tenant can now read and write tenant-related files concurrently. They can also store per-tenant memories, transcripts, tool call outputs and context histories for use in subsequent runs. New agents can be added to in-flight workflows and immediately access the full shared context simply by mounting the drive.


-


Multi-agent workflow with shared profile data: An AI test runner service needs to spin up multiple browser-use agents per customer. Each customer has a specific Chrome browser profile that must survive restarts. The service creates an Agent Drive with a separate profile folder for each customer. Multiple browser-use agents running in parallel for the same customer can concurrently mount the profile folder at startup for automatic self-configuration. New agents can be hot-attached to the drive at any time without restarting existing agents.


Other uses of Agent Drive are to pass files from one sandbox to another directly without needing intermediary storage or services, or to create a filesystem cache of package dependencies that is shared between agent/sandbox deployments to reduce cold-start times.


## Choosing the right storage


So those are three very distinct storage options. Which one should you use, and when?


Here's a handy table to help you decide:


Sandbox Filesystem Volume Agent Drive


Persistence Until sandbox deletion Survives sandbox deletion Survives sandbox deletion


Durability Medium (lost on sandbox deletion or crash) High (block storage, replicated) High (distributed storage, replicated)


Access type Single Single Multiple


Hot-attach N/A No Yes


Interface POSIX POSIX POSIX + S3


Size ~50% of sandbox memory User-defined at creation, expandable No size limit


Optimized for Data that requires sustained high throughput access Data that requires durability with high performance Data that is shared across multiple agents or sessions


Typical uses “Bursty” work sessions, non-critical agent outputs, development environments Code repositories, agent decision artifacts Shared agent context, shared memory layers, dependency caches


> Note: each storage class was benchmarked with the I/O mode agents actually use in practice.
