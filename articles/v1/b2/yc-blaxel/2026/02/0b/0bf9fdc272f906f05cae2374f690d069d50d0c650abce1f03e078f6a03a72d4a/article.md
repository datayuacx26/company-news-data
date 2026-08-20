---
schema_version: "1.0.0"
document_id: "0bf9fdc272f906f05cae2374f690d069d50d0c650abce1f03e078f6a03a72d4a"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/build0-cuts-sandbox-costs-by-80-using-blaxel"
published_at: "2026-02-04T20:14:16+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:54:31.900628+00:00"
content_hash: "sha256:53c444f4bbc6f7253bc853e1007b330c947beb3b6bbb0f70a162b4ad407960d6"
---

# Build0 cuts sandbox costs by 80% using Blaxel

At Blaxel, we build the infrastructure required to run agentic AI at scale. Our platform provides secure, isolated sandboxes that launch in milliseconds and scale to zero instantly, solving the latency and cost inefficiencies inherent in traditional cloud environments. These capabilities are critical for teams building the next generation of AI-powered development tools.


One such team is[Build0](https://build0.ai/) , led by CTO Phil Dang. Build0 is currently deploying three AI-native products powered by agentic workflows:


- **Vibe coding tool:** A natural language development environment where users provide instructions and an AI agent writes code, runs builds, and generates preview URLs in real time.
- **Landing page generation:** An automated designer that constructs templates, populates content, builds assets, and uploads final outputs to object storage without human intervention.
- **Autonomous workflows:** A system for long-running tasks, such as automated pull request reviews, where agents must maintain state over extended periods while waiting for external triggers like commits or comments.


Build0 leverages Blaxel’s serverless infrastructure to provide the secure execution environments necessary for their AI agents to write, test, and deploy code.


> “I’ve tried CodeSandbox, Vercel, and Blaxel, and Blaxel is the most solid one I’ve seen. It’s the right product for the time." - Phil Dang, CTO, Build0


### Leveraging Blaxel Sandboxes across three product lines


Build0 currently utilizes Blaxel sandboxes to power three distinct products:


- **Vibe-coding tool:** Users provide natural language instructions. The AI agent sends commands to a Blaxel sandbox to write code, build the project, and generate a[preview URL](https://docs.blaxel.ai/Sandboxes/Preview-url) . The sandbox persists between prompts, allowing for an iterative developer experience.
- **Landing page generation:** An AI agent constructs a template within a sandbox, fills in the content, builds the asset, and uploads the final output to object storage.
- **Autonomous workflows:** For long-running tasks, such as a pull request reviewer, Build0 uses Blaxel to maintain state over extended periods. The sandbox executes a step, sleeps while waiting for a trigger (like a new commit or comment), and resumes exactly where it left off.


By switching to an infrastructure model that aligns with the bursty nature of AI agents, Build0 dramatically improved their margins.


When a user or agent resumes interaction, the sandbox wakes up from that snapshot almost instantaneously. This allows Build0 to maintain the user's state—files, dependencies, and shell history—without paying for compute when the agent is idle.


> "The fact that it sleeps very soon and wakes up almost instantaneously is key. I don't even feel that it's sleeping. This allows us to minimize the cost of the sandbox." - Phil Dang, CTO, Build0


### The cost of idle compute in agentic workloads


AI coding agents operate in bursts. They generate code, run a build, and then wait for user feedback or external events. This usage pattern creates a specific infrastructure challenge: handling idle time efficiently.


Traditional sandbox providers like CodeSandbox are not optimized for this intermittency. They typically rely on long "keep-alive" windows, sometimes up to 15 minutes, to avoid the latency penalty of a cold start when the user returns.


Build0 initially tested CodeSandbox and Vercel but encountered significant friction. CodeSandbox required six to ten minutes of inactivity before hibernating a sandbox, meaning Build0 paid for idle compute long after the agent finished its task. Furthermore, Build0 faced persistence issues where sandbox memory and state were deleted after a few days of inactivity, forcing users to restart their sessions from scratch. For a product relying on iterative AI coding, maintaining state is non-negotiable.


Paying for idle time of sandboxes destroys unit economics. For Build0, this meant paying for up to 10 minutes of compute time for every brief interaction. Build0 needed a solution that offered persistent statefulness combined with aggressive, sub-second scaling.


### Driving 80% cost reduction across three product lines


Build0 migrated their agentic workloads to Blaxel to leverage our sub-25ms cold start times and instant hibernation capabilities. Unlike traditional VMs or sandbox providers that take minutes to spin down, Blaxel sandboxes detect network inactivity and suspend execution immediately, snapshotting the root filesystem.


> "Blaxel allows us to save 70% to 80% of the cost compared to previous solutions." - Phil Dang, CTO, Build0


The mechanism behind the 80% cost reduction is the difference in time-to-suspend. By cutting out the 10-minute idle tail attached to every single interaction, Build0 aligned their infrastructure costs strictly with value-generating execution time.


Traditional sandbox providers Blaxel


If an agent finishes a task at T+0s, the sandbox remains active and billable until T+600s (10 minutes). The sandbox detects the end of network activity and suspends execution immediately.


### Zero latency, zero state loss


Aggressive sleeping strategies typically come with two downsides: high latency upon resumption (cold starts) and loss of volatile memory. Blaxel solves both, enabling the cost savings described above without breaking the application logic.


When a Blaxel sandbox scales to zero, it snapshots the root filesystem. When the user resumes the session—even hours later—the sandbox wakes up from that exact state in milliseconds. This persistence is vital for Build0's autonomous workflows, which require the agent to remember context, file history, and dependencies across long pauses.


Unlike competitors where long-term storage is often ephemeral or cleared after a few days of inactivity, Blaxel ensures that the agent's environment remains intact forever, allowing Build0 to run complex, multi-stage workflows without the fear of data loss or the cost of always-on servers.


### Get started with agentic infrastructure


Build0 proved that high-performance AI tools do not require high-cost infrastructure. By utilizing Blaxel's precise lifecycle management, they achieved the persistence of a long-running server with the economics of serverless functions.


You can launch a secure, persistent sandbox today on app.blaxel.ai, or use the TypeScript snippet below to create a preview URL on a sandbox:


typescript


` import


{


SandboxInstance


}


from


"@blaxel/core"


;


async


function


main


(


)


{


// Create a new sandbox


const


sandbox


=


await


SandboxInstance


.


createIfNotExists


(


{


name


:


"my-sandbox"


,


image


:


"blaxel/nextjs:latest"


,


// public or custom image


memory


:


4096


,


// in MB


ports


:


\[


{


target


:


3000


,


protocol


:


"HTTP"


}


\]


,


// ports to expose


region


:


"us-pdx-1"


// if not specified, Blaxel will choose a default region


}


)


;


console


.


log


(


"Sandbox created:"


,


{


sandbox


}


)


;


// Launch dev server on port 3000


const


longRunningProcess


=


await


sandbox


.


process


.


exec


(


{


command


:


"cd /blaxel/app && npm run dev -- --port 3000 &"


}


)


;


// Create public preview


const


preview


=


await


sandbox


.


previews


.


createIfNotExists


(


{


metadata


:


{


name


:


"app-preview"


}


,


spec


:


{


port


:


3000


,


public


:


true


,


responseHeaders


:


{


"Access-Control-Allow-Origin"


:


"https://YOUR-DOMAIN"


,


"Access-Control-Allow-Methods"


:


"GET, POST, PUT, DELETE, OPTIONS, PATCH"


,


"Access-Control-Allow-Headers"


:


"Content-Type, Authorization, X-Requested-With, X-Blaxel-Workspace, X-Blaxel-Preview-Token, X-Blaxel-Authorization"


,


"Access-Control-Allow-Credentials"


:


"true"


,


"Access-Control-Expose-Headers"


:


"Content-Length, X-Request-Id"


,


"Access-Control-Max-Age"


:


"86400"


,


"Vary"


:


"Origin"


}


}


}


)


;


// Get preview URL


const


url


=


preview


.


spec


?.


url


;


console


.


log


(


"Preview URL:"


,


{


url


}


)


;


}


main


(


)


.


catch


(


console


.


error


)


;


`
