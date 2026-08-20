---
schema_version: "1.0.0"
document_id: "8486cbf374321144d3ee43836d8ad49e4e3de48010a564890b28b34ea4b4e15c"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/corvera-scales-up-enterprise-cpg-operations-with-blaxel"
published_at: "2026-06-15T07:36:48+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:48:58.638835+00:00"
content_hash: "sha256:68d4ad8931d0e7ee725b84931904ed255256565983026b0763a6f74928975e31"
---

# Corvera scales up enterprise CPG operations with Blaxel

[Corvera](https://www.corvera.ai/) is a YC startup (YC W26) building an agentic workforce for consumer packaged goods (CPG) brands. It focuses on automating back-office operations with AI that have traditionally relied on manual data entry and Enterprise Resource Planning (ERP) management. Since launching in early 2026, Corvera has grown its monthly recurring revenue significantly, serving brands that require reliable, autonomous support for tasks like demand planning and sales forecasting.


At Blaxel, we provide agent-native compute infrastructure designed for scale. Our[perpetual sandboxes](https://docs.blaxel.ai/Sandboxes/Overview) utilizes Firecracker micro-VMs to provide hardware-level isolation (unlike containers) without the latency penalties of traditional VMs. By[co-hosting agents](https://docs.blaxel.ai/Agents/Overview) with compute, we eliminate the latency bottlenecks that typically hinder autonomous workflows.


> "Other providers take minutes to spin up a sandbox. Blaxel takes milliseconds." - Dirk Breeuwer, Co-founder, Corvera


## Legacy CPG operations rely on manual data entry


Consumer brands often struggle with moving data between disparate systems. Even when companies utilize modern ERPs, the actual operations - moving data into the systems, extracting reports, and executing workflows - still rely on human intervention. This manual overhead creates bottlenecks in demand planning and purchase order management.


Corvera aims to replace this manual layer with an “agentic workforce”. These agents do not function as simple chatbots; they are autonomous entities that can be invoked across multiple channels, including Web, Slack, or even other coding agents. Using a combination of human instructions and organizational rules, they create and manage workflows, such as preparing sales forecasts or opening purchase orders up to a certain threshold, without human oversight.


> “Many brands still do these processes manually. And even when they have ERPs, they still rely on humans for these operations. We are the agentic workforce for these brands, and we automate their entire operational processes.” - Dirk Breeuwer, Co-founder, Corvera


## Corvera agents require fast, stateful execution environments


An autonomous agentic workforce requires agent-native compute: a secure, stateful execution environment where micro-VMs persist filesystem changes and networking configurations indefinitely.


Initially, Corvera utilized a different provider, but the technical challenges became a barrier to growth. Provisioning times for new environments took minutes rather than seconds, creating long wait times for users. In addition, agents could not easily persist and share information across different chat sessions within the same organization.


With Blaxel, Corvera found a solution to these challenges:


Corvera requirement Blaxel solution


Instant provisioning <25ms cold starts and standby resume


Shared state Blaxel Agent Drive for shared volumes and persistent files


Scalability Support for 1,000+ sandboxes deployed daily


Technical support Rapid, knowledgeable support from experienced engineers


> “These are coding agents. They need a full environment that includes storage and networking. And they need to spin up really quickly.” - Dirk Breeuwer, Co-founder, Corvera


## Blaxel sandboxes unlocked a major Corvera product feature


Blaxel unlocked a major feature for Corvera: the ability to spin up an entirely new, isolated sandbox instance for every single user chat session. Corvera's agents need to respond to user chat messages and backend triggers almost instantaneously, and Blaxel's[perpetual sandboxes](https://docs.blaxel.ai/Sandboxes/Overview) have millisecond start times, allowing Corvera to support near-instant agentic reactions without sacrificing speed or persistence. By using warm-standby snapshots, Blaxel effectively eliminates the cold start problem that plagues traditional container-based sandboxes.


To scale its agentic workforce efficiently, Corvera needed an infrastructure layer that kept compute environments available for intermittent work without incurring unnecessary costs. Blaxel maintains the efficiency of the Corvera platform by managing sandboxes in standby through a specific[three-state lifecycle](https://blaxel.ai/blog/understand-the-lifecycle-of-a-blaxel-sandbox) .


Blaxel’s sandboxes also come with an extensive library of[built-in code generation tools](https://docs.blaxel.ai/Sandboxes/Codegen-tools) and[networking](https://docs.blaxel.ai/Sandboxes/Proxy) features, allowing agents to execute arbitrary code and securely connect to external services with minimal configuration.


> “A major feature that Blaxel unlocked for us was isolated sandboxes per conversation. As soon as a chat starts, the agent needs to have a sandbox available for use. Blaxel makes this possible.” - Dirk Breeuwer, Co-founder, Corvera


## Blaxel provides persistent storage for agents


However, with isolated chat histories, agents cannot share information easily and different employees cannot collaborate with each other and with their agents. To solve this, Corvera needed a robust, shared file system so that it could continuously append conversation histories to a common file and make it accessible to multiple agents simultaneously, without any risk of data corruption. Blaxel's[Agent Drive](https://docs.blaxel.ai/Agent-drive/Overview) provides a shared, persistent filesystem that bridges separate sandbox sessions. By utilizing Agent Drive, Corvera agents can share executable scripts, automated workflows, and customized organizational rules. This data is maintained separately from individual chat histories, enabling newly provisioned agents to also instantly access and use previous workflows.


By moving beyond isolated chat histories and establishing a persistent and secure "organizational memory” for each customer, Corvera’s agents are able to respond effectively and reliably across all user channels.


> “There could be multiple employees of the same company, all spinning up individual agents, which are all going into the same drive and writing files and changing input and output into those drives. That’s why Agent Drive has been super helpful for us!” - Dirk Breeuwer, Co-founder, Corvera


### Corvera received support for implementation, testing and rollout


Throughout implementation and production rollout, Corvera received fast and knowledgeable support from Blaxel's engineering team via Slack. Bugs were quickly analyzed and squashed, and the two teams worked closely together to ensure the stability of Corvera's innovative, AI-forward product. This responsiveness made a significant difference during Corvera's early adoption.


> "On a 1 to 10 scale of support, the Blaxel team is 100. The gap to other vendors is significant. They take our bugs seriously and have been supportive throughout." - Dirk Breeuwer, Co-founder, Corvera


### Corvera agents are outperforming, enabling faster scale-up


Today, Corvera deploys approximately 1,000 sandboxes per day on Blaxel. By offloading infrastructure management to Blaxel, Corvera has achieved 2x faster performance than its competitors across real-world agent workflows. This has enabled the Corvera team to scale their MRR by 130% week-on-week.


Like Corvera, you can start building agent-native products today on[app.blaxel.ai](https://app.blaxel.ai/) , or use our TypeScript SDK to launch your first sandbox:


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
