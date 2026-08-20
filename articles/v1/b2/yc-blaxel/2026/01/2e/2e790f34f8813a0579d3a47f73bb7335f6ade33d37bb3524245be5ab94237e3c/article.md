---
schema_version: "1.0.0"
document_id: "2e790f34f8813a0579d3a47f73bb7335f6ade33d37bb3524245be5ab94237e3c"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/spawn-labs-enables-real-time-previews-for-coding-agents-using-blaxel"
published_at: "2026-01-27T19:10:33+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:b1e96f2251cbf5c4148718c304ac5efd144f6a93a4d94761330659b821917762"
---

# SpawnLabs enables real-time previews for coding agents using Blaxel

At Blaxel, our[perpetual sandbox platform](https://blaxel.ai/vm) provides AI agents with secure, isolated micro-VMs for code execution, featuring <25ms resume times from infinite standby. Our infrastructure supports a wide range of teams building innovative AI-driven products.


One such team is[SpawnLabs](https://spawnlabs.ai/) , a team building AI-powered solutions for EPC (Engineering, Procurement, Construction) companies working in the oil and gas industry. SpawnLabs operates a platform that lets users solve business problems and automate enterprise workflows by "spawning" coding agents to build customized, full-stack software tools. Founded in September 2025 by Ifechukwudeni (Teddy) Oweh, the SpawnLabs team includes engineers from Apple, Microsoft, NASA, and LinkedIn.


> "Blaxel's support is amazing and the overall integration experience was smooth." - Teddy Oweh, Founder, SpawnLabs


## SpawnLabs is a multi-agent platform to build domain-aware software


High-operations industries (like the oil and gas sector) lack AI-native software capable of handling complex workflows that require deep domain expertise. There is a need for custom tools that encompass workflows, automations, engines, and agents, and for a platform that can build, maintain, scale and continuously monitor these tools in production.


SpawnLabs solves this problem with multiple agents - backend, frontend, DevOps, QA - that work in parallel to create bespoke full-stack software tools. While many AI tools focus on simple UI generation, SpawnLabs provides an "army" of specialist agents that write code, deploy it, run tests, and handle ongoing monitoring and security audits...all based on simple natural-language descriptions of how things should work.


> "The hardest problems within businesses have always been solved by software. We're doing that, but with AI-native tools." - Teddy Oweh, Founder, SpawnLabs


## SpawnLabs agents need to validate code without breaking production


SpawnLabs agents build deep technical architecture, including user interfaces, microservices, APIs, and background jobs. As these agents generate hundreds of lines of code across the full stack, a critical challenge arises: how can an agent verify that its code actually works before it reaches a human reviewer or a production environment?


Traditional CI/CD pipelines are often too slow or rigid for the iterative nature of autonomous agents. For SpawnLabs, the agents need a way to "see" their work in real time, run quality assurance (QA) tests, and check logs in an environment that perfectly mirrors production but remains entirely isolated.


More specifically, SpawnLabs agents need the ability to:


- Deploy code to a live URL endpoint, allowing both the agent and the human customer to see changes instantly.
- Iterate on complex back-end services - such as Python-based APIs - in isolated dev environments without affecting the stable production state.
- Run continuous security evaluations and code tests to ensure code is robust before deployment.


SpawnLabs uses Blaxel sandboxes to support these goals. Once an agent writes a new feature or fix, it provisions a Blaxel sandbox (using[Blaxel's Python SDK](https://github.com/blaxel-ai/sdk-python) ) to serve as a development and test instance. Agents can now use this sandbox to iterate on code, run unit tests and security checks, and prepare for deployment.


> "The agent starts a dev instance on Blaxel, and we go from there. If it's an API, we get live URL endpoints and we just run QA on that. If it's a website, our browser agents go through the entire website, clicking buttons, checking flows, and also checking browser logs. Because the agent has access to logs and the codebase, it can start to iterate on things in real time." - Teddy Oweh, Founder, SpawnLabs


## Blaxel sandboxes provide fast, secure preview environments for agents


The SpawnLabs team evaluated several sandbox and compute providers before selecting Blaxel. While other providers offered compute, they lacked the specific "killer feature" SpawnLabs required: instant, real-time[previews](https://docs.blaxel.ai/Sandboxes/Preview-url) . This was critical because when workflows are tightly interconnected, it's essential for users to be able to see changes before they go live.


While previews were the most important reason for SpawnLabs to select Blaxel sandboxes, they weren't the only reason. For example, isolation and secure code execution are key concerns for enterprise customers. Blaxel uses Firecracker micro-VMs to ensure that agent-generated code cannot escape the sandbox.


SpawnLabs requirement Blaxel solution


Instant web access Built-in preview URLs for every sandbox


Agentic control Robust Python SDK for automated provisioning


Speed Fast startup times for dev servers and back-end services


Security VM-level isolation for executing untrusted agent code


> "I personally love the previews. They really give us the quality of service that we need. That is the most important thing for us." - Teddy Oweh, Founder, SpawnLabs


## Scaling to other industries


Today, SpawnLabs is using Blaxel to deliver AI-generated software to a range of oil and gas companies, with plans to expand to other industries. The ability to "spawn" safe, previewable environments on demand remains a core component of its technical advantage. By offloading the complexity of this preview infrastructure to Blaxel, SpawnLabs can focus on refining the logic of its multi-agent platform.


Like SpawnLabs, you can start building agent-native applications today on[https://app.blaxel.ai](https://app.blaxel.ai/) , or use our Python SDK to launch your first sandbox:


python


` import


asyncio


from


blaxel


.


core


import


SandboxInstance


async


def


main


(


)


:


# Create a new sandbox


sandbox


=


await


SandboxInstance


.


create


(


{


"name"


:


"my-sandbox"


,


"image"


:


"blaxel/base-image:latest"


,


# public or custom image


"memory"


:


4096


,


# in MB


"ports"


:


\[


{


"target"


:


3000


}


\]


,


# optional; ports to expose


"region"


:


"us-pdx-1"


# optional; if not specified, Blaxel will choose a default region


}


)


# Create public preview


preview


=


await


sandbox


.


previews


.


create


(


{


"metadata"


:


{


"name"


:


"my-preview"


}


,


"spec"


:


{


"port"


:


3000


,


"public"


:


True


,


"responseHeaders"


:


{


"Access-Control-Allow-Origin"


:


"https://example.com"


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


# Get preview URL


url


=


preview


.


spec


.


url


if


__name__


==


"__main__"


:


asyncio


.


run


(


main


(


)


)


`
