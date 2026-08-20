---
schema_version: "1.0.0"
document_id: "bb0135e02a059081cb16c4d16aa92eb558227c4cd2739b042e1b4497ef28b5d1"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/delty-builds-secure-high-context-ai-pull-request-reviews-with-blaxel-sandboxes"
published_at: "2025-12-03T13:52:09+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:641cb537b454467aa2a1217bd8b983c6dc694ad235a3ebe00190697f6d974cd3"
---

# Delty builds secure, high-context AI pull request reviews with Blaxel sandboxes

Delty is an **AI Staff Software Engineer** that integrates with an engineering team's knowledge bases to help design systems, update documentation, and provide feedback on pull requests (PRs).


To provide high-quality feedback, Delty's AI agent must analyze the entire codebase to understand the "first degree and the second degree effects" of code changes, not just the lines in the PR. This requirement created a significant security and performance challenge that traditional containers and VMs are not designed to solve, especially for Delty's enterprise customers who have large, sensitive repositories.


## The challenge: secure, low-latency execution for enterprise code


Delty's AI agent needs to check out the full repository and execute bash commands to perform its analysis. This presented two major problems for their enterprise customers.


First, security. Using a single, shared volume to process code from multiple tenants was not an option. Lalit Kundu, founder of Delty, noted their team found this approach "super insecure" for production use. Giving an AI agent bash access in a shared environment was too risky.


Second, latency. PR reviews must be delivered within one or two minutes. However, cloning a large enterprise repository, which can contain 50,000 files, can take over two minutes alone. Delty needed a solution that provided complete tenant isolation without sacrificing speed.


## The solution: persistent sandboxes for high-speed analysis


We worked with Delty to implement Blaxel, providing a true multi-tenant AI architecture by using a dedicated, isolated Blaxel sandbox for each customer tenant.


### Solving for security: isolated tenant sandboxes


Delty triggers an AI agent for each PR. The agent runs bash commands inside a Blaxel sandbox to clone and inspect the entire repository, cross-referencing code paths and dependencies to surface the direct and indirect effects of a change. Sub-second sandbox spin-up lets the agent start work immediately, while each customer runs in a dedicated sandbox for strict tenant isolation. The agent uses standard tooling inside the sandbox, so no custom runtimes are required. This immediately solved the security requirement.


### Solving for latency: sub-25ms standby resumes


To address the latency challenge, Delty implemented an intelligent lifecycle management system. Instead of cloning a massive repository for every review, Delty keeps a sandbox in standby mode with the repository already checked out.


When a new PR is submitted, Delty "wakes up" the standby sandbox: because Blaxel snapshots the running memory, this is a 25-millisecond operation. The agent then simply checks out the new branch for analysis. For parallel reviews, new sandboxes are spun up on demand. This approach eliminates the long clone operation for all subsequent PRs, enabling Delty to deliver the feedback within its target window.


## What is a Blaxel sandbox?


A Blaxel sandbox is an ephemeral yet stateful compute environment where agents can securely clone customer code and execute bash commands.


Unlike traditional containers or serverless functions, a sandbox provides:


- **Full tenant isolation** : Each sandbox is completely isolated, allowing AI agents to safely execute third-party code or bash commands without risk to other tenants.
- **Ultra-low latency** : Sandboxes idle in a "standby" state that doesn't consume compute runtime. They can be resumed in under 25 milliseconds, eliminating cold starts.
- **Stateful filesystem access** : Sandboxes provide a full, persistent filesystem — ideal for cloning git repositories or managing large files — with the option to persist data for years in volumes.


## Unlocking the enterprise market


By building on Blaxel, Delty was able to launch its AI-powered PR review feature to large B2B enterprises, a market segment that was previously inaccessible due to security constraints.


"It unlocked a lot of new customers for us," said Kundu. "Security was a hard requirement for our enterprise customers. Sandboxes with tenant isolation made that conversation straightforward."


Since launching the feature, Delty has closed over four new major enterprise contracts. The platform now analyzes approximately 5,000 PRs per month, providing the high-level, architectural feedback of a senior staff engineer that its customers require.


## Why Delty chose Blaxel


When evaluating solutions, Delty also considered alternatives. The team found the abstractions from other providers to be "very complex."


Kundu noted that with Blaxel, "the API... is very intuitive, super simple. No obscure terminology and concepts that I have to learn." This simplicity, combined with high reliability, allowed Delty to move from prototype to a secure, enterprise-ready product quickly.


Like Delty, get started today on[app.blaxel.ai](https://app.blaxel.ai/) , or get started with the snippet below to run bash commands in instant-launching secure sandboxes:


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


// ports to expose


}


)


;


// Run a command


const


process


=


await


sandbox


.


process


.


exec


(


{


name


:


"hello-process"


,


command


:


"echo 'Hello, World!'"


}


)


;


const


logs


=


await


sandbox


.


process


.


logs


(


"hello-process"


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
