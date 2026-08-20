---
schema_version: "1.0.0"
document_id: "af4e931626872008b320ee6f4a4636ddda345dcaee308624e67db1474a48ea89"
company_key: "yc-daily"
company: "Daily"
source_id: "yc-daily-rss-fbcbe287eccb"
canonical_url: "https://www.daily.co/blog/you-dont-need-a-webrtc-server-for-your-voice-agents/"
published_at: "2025-07-22T00:51:34+00:00"
first_seen_at: "2026-07-20T23:23:39.254949+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:17f2f191236104dc316463749cd5a8182780805245212813cdc8c0cd9cfb0b7a"
---

# You don’t need a WebRTC server for your voice agents

*Summary:*


- ***There are two ways to set up WebRTC connections to voice agents running in the cloud. You can route through a WebRTC server. Or you can set up direct, “serverless” WebRTC connections. If you are deploying your own WebRTC voice agent infrastructure, you should almost certainly use the serverless approach.***
- *The most widely used serverless WebRTC transport for voice agents is the*[Pipecat SmallWebRTCTransport](https://docs.pipecat.ai/server/services/transport/small-webrtc) *, built on the Python aiortc library. The SmallWebRTCTransport ecosystem includes SDK support for*[JavaScript, iOS, Android, Python, C++](https://docs.pipecat.ai/client/introduction) *, and*[embedded systems](https://github.com/pipecat-ai/pipecat-esp32) *.*
- *Note that using a WebRTC cloud with a large geographic footprint is often the best choice for conversational AI use cases.*


## Network protocols for voice agents


Voice AI agents use WebRTC, WebSockets, or SIP connections to stream realtime audio.


- **WebRTC** is optimized for **client-server (edge-to-cloud)** network connections. Use WebRTC if your voice agent is running in a native mobile app or in a web browser.
- **WebSockets** are great for **server-to-server audio connections** . For example, your voice agent code running in the cloud is connecting to a realtime audio API for transcription. (You shouldn't use[WebSockets for edge-to-cloud realtime audio](https://voiceaiandvoiceagents.com/#websockets-webrtc) .)
- **SIP** is used for interconnection with **telephone** systems.


In this post, we'll focus on WebRTC. It's the current state-of-the-art for delivering audio reliably, at the lowest possible latency, over real-world network connections.


Voice agents at scale, for all use cases other than telephone calls, require WebRTC.


To learn more, check out the open source text[Voice AI & Voice Agents: An Illustrated Primer](https://voiceaiandvoiceagents.com/#websockets-webrtc) . (PRs welcome.) Specifically, we'll discuss different ways developers can build with WebRTC — a traditional WebRTC server approach, as well as the newer "serverless" WebRTC connection. We'll keep in mind requirements for agentic use cases, and start with a brief discussion of WebRTC architecture.


## WebRTC architecture


WebRTC was designed to handle the complexities of voice and video, at ultra low latency. A WebRTC connection automatically adjusts to changing network conditions, making audio conversations possible even on poor connections. (Typical poor connections in the real world are cellular data in a congested environment, or a user who is far away from their WiFi router.)


WebRTC is widely used for video and audio calls over the Internet. Google Meet, Microsoft Teams, Facebook Messenger, and WhatsApp all use WebRTC.


For these systems, it makes sense to route video and audio through dedicated WebRTC servers. Because …


- Calls with several participants require[routing support from a centralized server](https://www.daily.co/blog/simulcast/) .
- Calls between people who are far apart geographically benefit from[mesh routing](https://www.daily.co/blog/global-mesh-network/) .
- Routing media directly between end-user devices can be challenging. Putting a server in the middle increases call success rates and improves average latency and throughput metrics. In the early days of the Internet, most routes across the network were roughly equivalent. Today, Internet peering strongly favors connectivity to the hyper-scaler clouds (AWS, Google Cloud, Microsoft Azure, and Oracle Cloud).


A technical diagram showing how a multiparticipant call is routed through a WebRTC server


However, WebRTC connections don't need to route through a server. You can set up a direct connection between your client code and your voice agent running in the cloud.


On the left is a voice agent connection routed through a traditional WebRTC server. On the right is a “serverless” voice agent connection, directly between a web client and the agent.


## The pros and cons of routing through a server


Routing through a server is the traditional way of scaling WebRTC. Commercial WebRTC cloud providers (like[Daily](https://daily.co/) ) have spent tens of thousands of engineering hours building out reliable, flexible cloud infrastructure based on the server approach.


A WebRTC cloud:


- Enables calls with hundreds or thousands of simultaneous participants.
- Routes media packets across private network connections that are faster than long-haul public Internet routes.
- Puts servers “close to the edge” in a large number of geographic regions, for optimal first-hop latency and fast regional routing.
- Auto-scales to handle large amounts of traffic.
- Incorporates redundancy and fail-over between servers, regions, and cloud providers.


On the other hand, routing directly:


- Eliminates the network hop through the server.
- Massively reduces the complexity of building out and maintaining WebRTC infrastructure for voice agents. With a serverless approach, you don’t have to maintain any WebRTC-specific infrastructure at all, because the WebRTC code is integrated into the client and agent SDKs directly.


## Optimizing for two things: latency and reliability


Realtime voice infrastructure has to deliver **very low latency and very high reliability** . These are the two things we are most concerned with when we design, build, and manage large WebRTC systems.


### Latency


**A state-of-the-art WebRTC cloud, operating globally, will deliver better average latency than the serverless approach.**


A global WebRTC cloud can connect each client to a server very close by, then route server-to-server over private network connections that are faster than the public Internet. This is called[mesh routing](https://www.daily.co/blog/global-mesh-network/) . Daily’s WebRTC cloud operates approximately 75 points of presence in 10 geographic regions. The P50 first-hop latency to the edge of Daily’s cloud is 13ms.


The benefits of mesh routing more than make up for the extra network hop(s) through Daily’s WebRTC server(s), compared to the direct connections of the serverless WebRTC approach.


However, if you are deploying WebRTC servers without mesh routing, the calculation is reversed. The extra network hop from the WebRTC server to the agent adds 10 to 100ms, depending on exactly how you deploy your infrastructure and where your servers and users are located.


Building and managing a WebRTC cloud with mesh connectivity is a very large engineering, devops, and static infrastructure cost commitment. You must have auto-scaling clusters of WebRTC servers wherever you have users. You will have to set up and maintain VPC routes between your WebRTC server clusters. You will have to write the mesh routing code. (There is no mesh WebRTC open source implementation.) Very few teams will have the engineering resources available to build out a WebRTC cloud with mesh routing, or have the baseline traffic volume to justify WebRTC server clusters in 5 or more geographic regions.


**So, if you want to maintain your own WebRTC infrastructure rather than use a commercial WebRTC cloud, you should use the serverless WebRTC approach. Your average latency will be 10 to 100ms lower for direct, serverless WebRTC connections than for connections that route through non-mesh WebRTC servers.**


### Reliability


Delivering reliable WebRTC sessions means minimizing a large number of potential failure modes: connection setup blocked by firewalls, network issues that impact audio quality, infrastructure components that hit scaling bottlenecks, retry logic in the network code that does not work for every corner case, and many more.


A state-of-the-art WebRTC cloud, plus heavily tested client SDKs, will deliver higher reliability than serverless WebRTC. In particular, the best WebRTC servers and client SDKs have sophisticated, extensively tested code for adapting to changing network conditions.


However, managing a WebRTC cloud is a complex, specific devops job. If you have a non-trivial amount of traffic, you will need auto-scaling, service discovery, geographic routing, new version roll-out, and observability code that is unique to WebRTC workloads. None of this functionality is available in open source WebRTC servers. You will have to design, test, deploy, and learn edge case lessons about all of the Kubernetes-related and other devops components yourself.


**Here again, if you want to maintain your own WebRTC infrastructure rather than use a commercial WebRTC cloud, you will have better reliability numbers if you avoid trying to build and manage WebRTC servers at all.** You can bundle the cloud side of the WebRTC transport code into your agents, which means that you’re only managing the agent workloads themselves and you get the WebRTC devops nearly “for free.”


## When should you use a WebRTC server and when should you “go serverless?”


If you’ve read this far, you know that the basic advice for whether to use WebRTC servers or go serverless for voice agents depends on whether you want to manage all of the infrastructure yourself:


- If you are happy to use a commercial WebRTC cloud with a global footprint, you will benefit from the engineering effort that has gone into minimizing latency and maximizing total reliability.
- If you want to run your own infrastructure, you should go serverless. You’ll have lower average latency, higher reliability, and much simpler infrastructure to manage.
- Serverless WebRTC can be easier to integrate into existing software tools, or to implement for embedded hardware. See the[pipecat-esp32](https://github.com/pipecat-ai/pipecat-esp32) project for a serverless WebRTC client for the ESP32-S3 family of microprocessors. ESP32 chips are widely used in small electronics devices.


There are use cases that require WebRTC servers.


1. **For sessions with more than two participants, you will need to use WebRTC servers.** Serverless WebRTC only works for simple voice agent connections: one human and one AI agent. If you want to connect multiple people or multiple agents, you can’t use serverless WebRTC.
2. **For video, you should probably use WebRTC servers.** Video requires much more bandwidth than audio and depends heavily on the sophisticated network adaptions that WebRTC servers are good at.


[Pipecat](https://www.pipecat.ai/) is an open source voice (and video) realtime agents toolkit that gives you the flexibility to use a wide range of network transports, depending on your goals and use cases. Pipecat is vendor neutral, and supports serverless WebRTC, cloud WebRTC and SIP (Daily and LiveKit transport implementations), WebSockets (including Twilio Media Streams WebSockets), and telephony connections via Twilio, Plivo, and other providers.


You can use Pipecat’s[SmallWebRTCTransport](https://docs.pipecat.ai/server/services/transport/small-webrtc) for serverless WebRTC. The SmallWebRTCTransport is designed with all of the serverless advantages described above in mind, and has no dependencies on any external service or infrastructure. All of the Pipecat examples and getting started repos use SmallWebRTCTransport.


You can experiment with both the SmallWebRTCTransport and the[DailyTransport](https://docs.pipecat.ai/server/services/transport/daily) side-by-side. You can use serverless WebRTC for development and prototyping, and deploy to production on the Daily global WebRTC cloud with the DailyTransport. Or, of course, you can write your own transport implementation! Pipecat is completely, 100% Open Source and vendor neutral.


The[Pipecat Cloud](https://docs.pipecat.daily.co/introduction) voice agent hosting service bundles Daily WebRTC for free. You don’t pay anything extra to use Daily’s global WebRTC cloud when you deploy to Pipecat Cloud. One of the goals of Pipecat Cloud is to make it easy to use the best performing voice agent building blocks, including Daily WebRTC, the[Krisp voice isolation](https://docs.pipecat.daily.co/pipecat-in-production/krisp-noise-cancellation) and noise reduction models, and the[smart-turn semantic VAD model](https://docs.pipecat.daily.co/pipecat-in-production/smart-turn) , all of which are bundled into Pipecat Cloud at no cost.


*A special shout out to*[Sean DuBois](https://x.com/_pion) *,*[Thor Schaeff](https://x.com/thorwebdev) *, and*[Aleix Conchillo Flaqué](https://x.com/aconchillo) *for creating community momentum around the*[open source WebRTC client for the ESP32 chips](https://github.com/pipecat-ai/pipecat-esp32) *. If you’re interested in voice AI hardware, or just in contributing to a fun project, please join us and create PRs, add to the docs, or post videos of things you’re building!*


#### Never miss a story


Get the latest direct to your inbox.
