---
schema_version: "1.0.0"
document_id: "fe3fbc261fd149d1edbcf8d8f74ea70eb0a9ca72b7754be2f08a83c7c560caa0"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/smolmachines-microvms-for-ai-agents"
published_at: null
first_seen_at: "2026-08-11T19:19:56.191169+00:00"
fetched_at: "2026-08-11T19:19:57.200737+00:00"
content_hash: "sha256:44bdd6b1ae863eedb56aac4365037d25e4b823679b0748bde4c32896fc30b78f"
---

# Smol Machines: MicroVMs for AI Agents

Coding agents need a place to run code that is fast, isolated, and disposable when the job is done. Containers are familiar, but they share a host kernel. Full virtual machines are isolated, but they are often too slow and heavy for workflows where an agent might spin up many short-lived environments.


Today we're excited to announce our partnership with[Smol Machines](https://smolmachines.com/) , a platform for fast, hardware-isolated Linux microVMs, now available on Orthogonal. Smol Machines gives agents a clean runtime for untrusted code, browser work, CI-style jobs, and long-running development environments without forcing teams to wire another infrastructure provider by hand.


## What is Smol Machines?


Smol Machines builds the` smolvm` open-source runtime, smol cloud, and the` smol` SDK. The same` .smolmachine` artifact can run locally, in the hosted cloud, or on self-hosted infrastructure.


The product is designed around a useful middle ground: container-like ergonomics with a real VM boundary. Smol Machines runs workloads in hardware-isolated Linux VMs with their own guest kernel, supports portable` .smolmachine` artifacts, and exposes cloud lifecycle operations through a REST API.


That makes it a natural fit for agents. A code agent can start from an OCI image or packed machine, run a command, keep state when needed, mount a volume, and tear everything down after the task. A product team can keep the same control flow whether it is running a sandbox for one user action or a fleet of persistent agent environments.


## Using Smol Machines with Orthogonal


Smol Machines is available through the same Orthogonal SDK your agents already use. One Orthogonal key gives you access to Smol Machines alongside the rest of your agent infrastructure stack.


### Create a Machine


Create a cloud machine from an OCI image. Network access is explicit, and resources can be scoped to the workload. Use open networking when the machine needs to pull a public image from a registry; use blocked networking when you provide a local or packed machine artifact.


` import


Orthogonal


from


"@orth/sdk"


;


const


orthogonal


=


new


Orthogonal


(


{


apiKey


:


process


.


env


.


ORTHOGONAL_API_KEY


,


}


)


;


const


machineResponse


=


await


orthogonal


.


run


(


{


api


:


"smolmachines"


,


path


:


"/v1/machines"


,


body


:


{


name


:


"agent-sandbox"


,


source


:


{


type


:


"image"


,


reference


:


"python:3.12-alpine"


}


,


resources


:


{


cpus


:


1


,


memoryMb


:


1024


,


diskGb


:


4


}


,


network


:


{


mode


:


"open"


}


,


ttlSeconds


:


3600


,


workdir


:


"/workspace"


}


}


)


;


const


machine


=


machineResponse


.


data


as


{


id


:


string


;


state


:


string


}


;


console


.


log


(


machine


.


id


)


;


console


.


log


(


machine


.


state


)


;


`


### Run a Command


Execute a command inside the machine. Use argv form when the agent does not need shell parsing.


` const


resultResponse


=


await


orthogonal


.


run


(


{


api


:


"smolmachines"


,


path


:


\`


/v1/machines/


${


machine


.


id


}


/exec


\`


,


body


:


{


command


:


\[


"python3"


,


"-c"


,


"print('hello from smol machines')"


\]


,


timeoutSeconds


:


30


}


}


)


;


const


result


=


resultResponse


.


data


as


{


stdout


:


string


;


exitCode


:


number


}


;


console


.


log


(


result


.


stdout


)


;


console


.


log


(


result


.


exitCode


)


;


`


### Keep a Session


Sessions preserve working directory and environment across related exec calls. This is useful when an agent is iterating in a repo and should not restate context on every request.


` const


sessionResponse


=


await


orthogonal


.


run


(


{


api


:


"smolmachines"


,


path


:


\`


/v1/machines/


${


machine


.


id


}


/sessions


\`


,


body


:


{


cwd


:


"/workspace"


,


env


:


{


NODE_ENV


:


"test"


}


}


}


)


;


const


session


=


sessionResponse


.


data


as


{


id


:


string


}


;


const


testRunResponse


=


await


orthogonal


.


run


(


{


api


:


"smolmachines"


,


path


:


\`


/v1/machines/


${


machine


.


id


}


/sessions/


${


session


.


id


}


/exec


\`


,


body


:


{


command


:


"python3 -c \\"import os; print(os.environ.get('NODE_ENV'))\\""


,


timeoutSeconds


:


120


}


}


)


;


const


testRun


=


testRunResponse


.


data


as


{


stdout


:


string


}


;


console


.


log


(


testRun


.


stdout


)


;


`


### Run Code Directly


For small tasks, agents can submit code directly to a machine instead of uploading a script first.


` const


codeResponse


=


await


orthogonal


.


run


(


{


api


:


"smolmachines"


,


path


:


\`


/v1/machines/


${


machine


.


id


}


/code


\`


,


body


:


{


language


:


"python"


,


code


:


"import platform\\nprint(platform.platform())"


,


timeoutSeconds


:


10


}


}


)


;


const


codeResult


=


codeResponse


.


data


as


{


stdout


:


string


}


;


console


.


log


(


codeResult


.


stdout


)


;


`


### Check Machine Status


Fetch the machine status when an agent needs to confirm the current state before deciding whether to execute more work or let the machine expire.


` const


statusResponse


=


await


orthogonal


.


run


(


{


api


:


"smolmachines"


,


path


:


\`


/v1/machines/


${


machine


.


id


}


\`


}


)


;


const


status


=


statusResponse


.


data


as


{


state


:


string


;


lastActivityAt


:


string


|


null


;


}


;


console


.


log


(


status


.


state


)


;


console


.


log


(


status


.


lastActivityAt


)


;


`


## Using x402 Protocol


Smol Machines on Orthogonal also supports[x402](https://www.x402.org/) for native HTTP payments. Agents can pay for individual API calls with USDC, no separate Smol Machines key required.


` import


{


wrapFetchWithPayment


}


from


"x402-fetch"


;


import


{


privateKeyToAccount


}


from


"viem/accounts"


;


const


account


=


privateKeyToAccount


(


process


.


env


.


PRIVATE_KEY


as


\`


0x


${


string


}


\`


)


;


const


fetchWithPayment


=


wrapFetchWithPayment


(


fetch


,


account


)


;


const


response


=


await


fetchWithPayment


(


"https://x402.orthogonal.com/smolmachines/v1/machines"


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


source


:


{


type


:


"image"


,


reference


:


"alpine:latest"


}


,


network


:


{


mode


:


"blocked"


}


,


ttlSeconds


:


900


}


)


}


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


.


id


)


;


`


## Using MPP


Smol Machines also works through[MPP](https://mpp.dev/) , the open standard for machine-to-machine payments co-authored by Tempo and Stripe. Agents can pay for Smol Machines calls with stablecoins, cards, or Bitcoin on the same endpoint.


` import


{


privateKeyToAccount


}


from


"viem/accounts"


;


import


{


Mppx


,


tempo


}


from


"mppx/client"


;


Mppx


.


create


(


{


methods


:


\[


tempo


(


{


account


:


privateKeyToAccount


(


"0x..."


)


}


)


\]


,


}


)


;


const


response


=


await


fetch


(


"https://mpp.orthogonal.com/smolmachines/v1/machines"


,


{


method


:


"POST"


,


headers


:


{


"Content-Type"


:


"application/json"


}


,


body


:


JSON


.


stringify


(


{


source


:


{


type


:


"image"


,


reference


:


"alpine:latest"


}


,


ttlSeconds


:


900


}


)


}


)


;


const


data


=


await


response


.


json


(


)


;


console


.


log


(


data


.


id


)


;


`


The` mppx` client handles the 402 challenge, signs a credential with your wallet, and retries with proof of payment.


## What Agents Can Do with Smol Machines


Coding agents can run generated code inside a hardware-isolated VM instead of directly on a host machine. That is the obvious use case, but it is not the only one. Agents can also run browser automation, install dependencies, execute test suites, process files, or keep a persistent development environment alive between tasks.


Smol Machines is especially useful when the environment matters. If a task needs Python, Node, Docker-in-a-machine, GPU-adjacent workflows, or a packed` .smolmachine` artifact with dependencies already baked in, the agent can start from that machine and spend less time rebuilding the world.


## Why We Partnered with Smol Machines


Agents are only as useful as the environments they can safely act inside. Smol Machines gives builders a practical isolation layer for code execution and long-running agent work: fast startup, portable artifacts, explicit networking, persistent volumes, and a cloud API that maps cleanly to agent workflows. That fits Orthogonal well. Builders can combine execution, data, and payments through one API layer, then pay per request instead of negotiating yet another account and key.


## Try It Today


Sign up for Orthogonal and get $10 free credits to try Smol Machines and dozens of other APIs. One key, hundreds of APIs, pay per request.


[Get Started](https://orthogonal.com/sign-up) |[View on Orthogonal](https://orthogonal.com/discover/smolmachines) |[Smol Machines Website](https://smolmachines.com/)
