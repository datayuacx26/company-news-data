---
schema_version: "1.0.0"
document_id: "135d14d31d0b88d48115520be31697e6551ea37184fd12fd3d6f506bf53aec08"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/mendral-builds-the-first-24-7-ai-dev-ops-engineer-using-blaxel"
published_at: "2026-01-21T00:19:08+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:54:46.350230+00:00"
content_hash: "sha256:2f0c61652a367f5f6ee49e6e553b49328673b2eb06ff1cd0a08f5e9eb5ad68d2"
---

# Mendral builds the first 24/7 AI DevOps engineer using Blaxel

At Blaxel, we provide agent-native compute infrastructure designed for scale. Our[perpetual sandbox platform](https://blaxel.ai/vm) lets you keep infinite, secure sandboxes on automatic standby while co-hosting your agents for near instant latency. Our infrastructure supports a wide range of teams building innovative AI-driven products.


One such team is[Mendral](https://www.mendral.com/) , an AI DevOps engineer that monitors and fixes all CICD problems in real time. Mendral is co-founded by[Sam Alba](https://github.com/samalba) , former VP of Engineering at Docker and co-founder at Dagger, and[Andrea Luzzardi](https://github.com/aluzzardi) , former Lead Architect at Docker and co-founder at Dagger. Both of them have deep experience in DevOps and CI/CD and they are now using their expertise building Mendral, powered by Blaxel’s perpetual sandbox platform, to improve software delivery capabilities using AI.


> "We spent an hour experimenting with Blaxel and we immediately realized it had everything we needed. It was great!" - Sam Alba, Co-founder, Mendral


## Coding agents are overwhelming traditional DevOps


AI coding agents have increased the volume of code being produced, shifting pressure from development teams to DevOps pipelines and teams. Earlier, a team might produce 5-10 PRs in a day. Now, a single coding agent can generate hundreds of PRs and thousands of lines of code in the same amount of time.


This exponential ease in code generation has come at a cost: agent-generated code needs to be tested as thoroughly as human-generated code, but traditional DevOps is unable to keep up with the tidal wave of new PRs. The bottleneck is no longer writing code: it's validating and deploying it.


The solution? Hire an AI DevOps Engineer.


## Mendral is an AI DevOps agent that never sleeps


Mendral is an AI DevOps engineer designed to manage the entire software delivery process: everything from code to Production. Unlike traditional chatbots, Mendral operates as an autonomous agent that monitors events, logs, and code changes to suggest improvements and remediate failures in real time.


Mendral constantly ingests and analyzes information from a project's CI environment, CI logs, code changes, links code reviews to CI performance data, "remembers" best practices and tool conventions, and independently produces insights and recommendations for improvements. It operates continuously, capable of investigating flaky tests, performance regressions and security vulnerabilities as soon as they occur.


For example, if a new test suite increases the total build time by 30%, Mendral identifies this as a DevOps regression rather than just a successful test pass. This is already critical information, but Mendral doesn't stop there: it analyzes the code to understand the regression and opens a PR to fix it, providing detailed context along the way for the human reviewer.


> “Our agent can just jump in as soon as a problem occurs, search the logs in the past, and run an investigation for you and submit a fix. The reactivity and the amount of work that it can produce in an instant is much greater than any human being.” - Sam Alba, Co-founder, Mendral


## Mendral agents need fast, secure and isolated compute environments


To function as a true DevOps engineer, every Mendral agent requires a complete development environment where code can be checked out, system dependencies can be installed, and commands can be executed.


When an agent identifies an issue - such as failing tests or potential credential leakage - it provisions an environment to run investigations and submit a fix.


Mendral leverages Blaxel's[perpetual sandboxes](https://blaxel.ai/vm) to provide these environments. These sandboxes are an infrastructure layer that utilizes Firecracker-forked micro-VMs to provide AI agents with isolated, stateful execution environments. Agent-generated code running inside these sandboxes is fully isolated, making them ideal for agents that need access to an operating system to run commands with no risk of escaping.


## Blaxel sandboxes provide agent-native compute with sub-25ms cold starts and secure execution


For Mendral, the core requirements for their agentic execution environment are:


- Persistent state: The ability to maintain state between agent loops, ensuring that files created in one step are available for the next.
- Fast provisioning: The ability to start the execution environment with minimal delay, to ensure the agent remains responsive.
- Efficient execution: The ability to run processes and filesystem operations with maximum efficiency.
- Lifecycle management: The ability to easily snapshot, suspend, restart and destroy the environment at will


In their search for the perfect solution, Mandrel evaluated several alternatives before Blaxel, but only Blaxel sandboxes ticked all their boxes:


**Mendral requirement** **Blaxel solution**


Persistent state Volumes, persistent filesystem, auto-snapshotting


Fast provisioning <25ms cold-starts from standby


Efficient execution RAM-based filesystem, real-time event streaming, process auto-restart


Lifecycle management Scale-to-zero during idle, TTL-based lifecycle


> “One killer feature is the TTL. The fact that the sandbox auto-suspends is really, really good - when you don’t need the runtime, it automatically snapshots the root filesystem. The next time I run a command, it restarts from the snapshot and it’s almost like the sandbox was long-running.” - Sam Alba, Co-founder, Mendral


## Scaling Mendral with Blaxel’s AI-native infrastructure


By using Blaxel, Mendral avoids the overhead of managing complex infrastructure like Firecracker micro-VMs or Kubernetes clusters directly. This allows their team to focus on the agent's logic and continuously improve its ability to solve complex DevOps problems without worrying about scalability or reliability issues.


Mendral's experience with Blaxel's support and documentation is also positive. The Mendral team had numerous questions related to Firecracker-specific implementation details during their implementation phase, and Blaxel's responsiveness made a significant difference during early adoption.


Today, Mendral is used in production by 15 teams (including Blaxel!). Its autonomous recommendations and pull requests have a 75% acceptance rate, instantly proving its value in improving velocity and software quality.


Like Mendral, get started today on[https://app.blaxel.ai](https://app.blaxel.ai/) , or use the TypeScript snippet below to instantly launch a secure sandbox:


typescript


` import


{


SandboxInstance


}


from


"@blaxel/core"


;


// Create a new sandbox


const


sandbox


=


await


SandboxInstance


.


create


(


{


name


:


"my-sandbox"


,


image


:


"blaxel/base-image:latest"


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


// optional; ports to expose


labels


:


{


env


:


"dev"


,


project


:


"my-project"


}


,


// optional; labels


region


:


"us-pdx-1"


// optional; if not specified, Blaxel will choose a default region


}


)


;


`
