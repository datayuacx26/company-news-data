---
schema_version: "1.0.0"
document_id: "ce165deb06073bf630d39ecc18fd5945cdc010cfa9b8ac4b922c77683acc52e1"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/runwork-accelerates-collaborative-enterprise-ai-workflows-by-10x-using-blaxel"
published_at: "2026-06-25T09:52:08+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:48:18.651537+00:00"
content_hash: "sha256:97151e6f519e117234b211c0ebd43efbcd100e8028c9fad9cb99d2e6736ffebf"
---

# Runwork accelerates collaborative enterprise AI workflows by 10x using Blaxel

[Runwork](https://runwork.ai/) is the shared cloud that makes a team AI-native. It connects the AI tools people already use (ChatGPT, Claude, Cursor, Codex) to one workspace, onboards and continuously trains each person on them, and syncs the team's skills, MCP servers, integrations, data, and instructions to every agent, so that what one person sets up, every teammate's AI can use. On top of that workspace, Runwork runs the automations, scheduled jobs, full applications, and autonomous agents that teams build, turning scattered individual AI use into a coordinated, measurable team capability.


At Blaxel, we provide a[perpetual sandbox](https://docs.blaxel.ai/Sandboxes/Overview) platform designed for AI agents that need to execute code in production environments. The platform provides isolated, secure serverless compute runtimes on automatic standby alongside co-hosted agent logic to minimize end-to-end network latency. Our infrastructure enables engineering teams to deploy, scale, and manage autonomous AI agents across diverse enterprise environments.


> "Blaxel is the best in speed and feature sets." - Oytun Tez, CEO, Runwork


## Traditional AI tools isolate automation within individual environments


When non-technical departments like marketing or operations adopt AI tools, the knowledge and configurations they generate remain trapped within individual browser sessions or local desktop clients. If an employee creates a specific prompt sequence or teaches an AI agent an operational workflow, there is no native mechanism to share that skill across an entire enterprise team.


In addition, moving from basic text generation to true workflow automation requires technical expertise. Building reliable workflows that interface with external APIs typically requires traditional software engineering. Non-technical team members lack the skills and tools to deploy autonomous code that executes reliably in the background.


> "With the current AI tools, whatever you do is stuck in your computer. It's very difficult to share anything you teach your AI with your teammates." - Oytun Tez, CEO, Runwork


## Runwork builds a shared internal platform for team-wide AI tools


To bridge this gap, Runwork gives a team and their agents one shared workspace, embedded in 50+ agents and reachable from the desktop app, the web, an MCP server, a CLI, and an API. A teammate just talks to their own agent, Claude or Codex, and it uses Runwork's cloud to do the real work: running apps, workflows, automations, and integrations on the team's behalf.


To execute these user-generated workflows safely and efficiently, Runwork relies on Blaxel sandboxes across three primary operational architectures:


- **Vibe-coded applications:** Runwork spins up a secure sandbox where customer AI agents can write and test applications within the Runwork framework based on natural language instructions. The finished applications are deployed to production environments like Cloudflare Workers. This feature leverages Blaxel's native preview URLs, allowing users to preview generated applications in real-time with zero additional configuration
- **Shared workspace computers:** Teams share a continuous, long-running workspace sandbox to execute lightweight, time-based automations (e.g. "remind me at 8am," "send this email tonight"). All changes are version-tracked by Runwork so the sandbox can always be restored from a clean image.
- **Autonomous agent execution:** Runwork runs specialized agents inside isolated sandboxes to complete specific tasks, such as collecting data or preparing reports. Blaxel sandboxes are built on microVMs rather than standard containers to guarantee air-tight tenant isolation, eliminating container-escape vulnerabilities.


> "Basically, we spin up a new sandbox, let the customers' AI agents talk to that sandbox, and build whatever they want to build within the Runwork framework." - Oytun Tez, CEO, Runwork


## Blaxel sandboxes eliminate cold starts without sacrificing persistence


Traditional virtual machines and standard container clusters introduce significant latency during cold starts, which disrupts the responsiveness of Runwork's AI agents. Blaxel sandboxes solve this problem by providing persistent environments that wait on standby indefinitely when not used, eliminating cold starts without complex orchestration.


When a Runwork agent finishes a task, the sandbox automatically suspends to minimize resource consumption. When a new user request or webhook triggers the agent, the sandbox resumes from the exact state in less than 25 milliseconds. Unlike many other sandbox providers, Blaxel enables unlimited standby with a 15-second scale-to-zero trigger, providing the continuity of a long-running system without the associated idle compute costs.


> “Speed is a big differentiator. Creating automatic snapshots and resuming them later is a very good feature." - Oytun Tez, CEO, Runwork


## Blaxel custom images eliminate dependency installation delays


Blaxel also makes it easy to provision sandboxes based on[custom images](https://docs.blaxel.ai/Sandboxes/Templates) . Each Runwork agent is provisioned with a sandbox generated from a custom image where all runtime dependencies (including database, file storage, and workflow engine) are pre-installed. This architecture eliminates the need for post-boot installations, allowing the agent to become fully operational immediately upon initialization.


> "We install everything while we prepare the image so all of the dependencies are ready when we bring it up. It almost feels like we don't have any cold boots." - Oytun Tez, CEO, Runwork


## Blaxel persistent volumes enable stateful agent collaboration


To support shared workspaces and autonomous agent executions, Runwork leverages Blaxel's[volumes](https://docs.blaxel.ai/Volumes/Overview) . Instead of spinning up entirely disconnected runtimes, Runwork allocates distinct directories within a shared persistent volume for different agents belonging to the same organization. This allows agents to access shared files, monitor peer processes, and chain execution outputs sequentially.


Beyond agent-to-agent coordination, persistent volumes have also improved human collaboration within Runwork's customer teams. Multiple team members can now resume a shared workspace exactly where a colleague left off, without rebooting or rebuilding the sandbox state from scratch.


> "We keep one volume for all the agents of a customer. Every agent uses a different folder, different directory, and agents can access each other's processes or each other's outputs. Another plus of persistent memory: it enables ‘resume-where-you-left-off’ among multiple team members so they can collaborate more seamlessly while building apps/tools." - Oytun Tez, CEO, Runwork


## Blaxel’s automatic scale-to-zero model is cost-effective


The scale-to-zero model also delivers direct maintenance and commercial benefits for Runwork. By guaranteeing sandbox availability, Blaxel reduces the maintenance burden on Runwork’s engineering team. And Blaxel’s per-second compute cost billing also changes the economics significantly: Runwork can pass through sandbox usage costs to customers billed by the second or minute, rather than absorbing idle compute costs themselves.


“This fine-grained cost model is especially encouraging for our customers to experiment with sandboxes and learn how to develop with them.“ - Oytun Tez, CEO, Runwork


## Scaling agentic workflows across the enterprise


Blaxel's platform is a perfect fit for Runwork's requirements:


Runwork requirement Blaxel solution


Low-latency execution for interactive workflows Sub-25ms micro-VM resume times


Multi-agent file and process coordination Shared directory mapping via persistent volumes


Elimination of cold boot installation delays Support for customized micro-VM images


Fine-grained cost model for customers Per-second billing via automated scale-to-zero


Cost minimization for sporadic automation events Automated scale-to-zero lifecycle management


The impact on Runwork's development speed has been measurable: compared to their previous sandbox provider, environment boot time dropped from 20 seconds to just 2–3 seconds - a roughly 10x improvement. This acceleration comes from two factors: Blaxel's sub-25ms micro-VM resume times and persistent memory snapshots.


By offloading low-level micro-VM orchestration to Blaxel, the Runwork engineering team has been better able to focus on refining its framework and onboarding enterprise clients. Going forward, Runwork is also considering Blaxel's[Agent Drive](https://docs.blaxel.ai/Agent-drive/Overview) for multi-agent coordination use cases.


> "The Blaxel team adds new features very fast, and this makes me feel very safe. Even if I didn't implement them yet, they were features that I really needed. This product empathy by Blaxel is a very good sign." - Oytun Tez, CEO, Runwork


Like Runwork, you can deploy secure, low-latency infrastructure for autonomous agents. Get started today on[app.blaxel.ai](https://app.blaxel.ai/) , or use our TypeScript SDK to launch your first sandbox:


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
