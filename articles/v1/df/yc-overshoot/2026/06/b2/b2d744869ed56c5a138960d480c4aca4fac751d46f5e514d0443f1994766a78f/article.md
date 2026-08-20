---
schema_version: "1.0.0"
document_id: "b2d744869ed56c5a138960d480c4aca4fac751d46f5e514d0443f1994766a78f"
company_key: "yc-overshoot"
company: "Overshoot"
source_id: "yc-overshoot-news-import-dcfd2100a052"
canonical_url: "https://www.overshoot.ai/blogs/tcp-latency-optimization"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-22T07:58:59.307241+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:c7c5888118d5bc9bed034ef030a6237e8a4de5a215fdf652647472b80e818dc3"
---

# TCP for server-to-GPU communication

> **TL;DR:** For a 5MB upload over a 31ms RTT path, cold TCP delivered the payload in 325ms. Two knobs brought it to 72ms. We introduce and visualize the TCP concepts in play, and explain how warm TCP helps and why it's not always wise.


TCP was invented in 1974. Since then, it's been by far the most used protocol for reliable, ordered communication over the internet. It is the technology beneath http/1, http/2, ssh, smtp, and websockets to name only a few. Over the decades of its prevalence, it's been meticulously optimized to deliver data reliably over the messy web that is the public internet. However, for heavy, bursty inference payloads (media input, realtime requirement) TCP can become a latency bottleneck.


In this blogpost, we show how we reduced network latency for a 5MB upload from 325 ms to 72 ms.


Using an interactive visual simulation, I hope you'll see and intuit why TCP can be so slow and how you can make it as fast as the network allows, followed by some important caveats to keep in mind.


### The basics


In TCP, a sender and a receiver create a connection, which allows the sender to send a payload to the receiver over network infrastructure. During data transfer, the payload is broken up into packets. Each packet travels through the network, making hops at different routers along the way. Each router receives data from a link into a buffer, then forwards it to another link. When the packet arrives, the receiver sends out an acknowledgment or ack to the sender. The time it takes for one packet to be received and its ack to come back is round trip time RTT. TCP achieves lossless ordered payload at reception by resending unacknowledged packets until full payload is acknowledged from the receiver or the connection is closed.


Below is a simulation based on a real measured request:


[Run it yourself →](https://tcp.overshoot.ai/?preset=cold-iw10)


a 5MB payload over a network with 31ms RTT. Despite a round trip time of 31ms, the payload arrives within 350ms. Looking at the simulation, the network seems largely under-utilized.


### Reuse connections


The first thing we see is the initial 3-way handshake SYN→SYN-ACK→ACK. This serves to establish the connection between the sender and receiver. Can we do without it?


Yes! we can open a connection once (on startup or first request) and maintain it so we don't have to waste 1 RTT on handshaking each time. Both the real measurement and the simulation show the expected improvement, cutting network latency to 295ms.


#### Gotchas


- Both the sender and receiver will discard connections after some timeout window, either change that on the receiver if you own it, ask for longer timeouts, or watch for connection death and replace with new connections.
- One connection = one inflight request at a time. Meaning if you need to handle 20 concurrent requests, you should pre-warm 20 tcp connections.


[Run it yourself →](https://tcp.overshoot.ai/?preset=warm-iw10)


In this simulation above, one can see that the number of packets in-flight (aka the congestion window,` cwnd` ) starts small and gets larger with every batch. This behavior is controlled by something called a congestion-control algorithm CCA.


Simply put: if you send too many packets under bad network conditions, you clog the router buffers causing latency to increase further and requests to timeout. Send too few packets each time and you will under-utilize the link, increasing latency. CCAs solve this by adapting to network conditions as they detect them.


In this case, cubic (the most widely deployed CCA) initializes cwnd to a measly 10 packets carrying 0.0146MB of data. After that, cubic doubles cwnd with each healthy RTT (no packets dropped). If a packet drops, that means there is congestion somewhere in the path, so cubic backs off: It cuts cwnd down by 70% and stops growing exponentially for a while.


In our setup at Overshoot, we control both sender and receiver and run over a provisioned high-capacity path, raising init cwnd from 10 to 3500 packets, which cuts our upload latency from 295 ms to 72 ms.


[Run it yourself →](https://tcp.overshoot.ai/?preset=warm-iw3500-rwnd-warm)


#### Gotchas


- cwnd alone is not enough, the receiver has a receive window rwnd value, sender can send min(cwnd, rwnd) in-flight packets. I'll dive deeper into rwnd in another blog.
- When sending N concurrent requests, all with aggressive initial cwnd, we can have Nx more packets in flight than the path can handle. Better to set init cwnd to max-safe-cwnd / N concurrent requests.


### Why like this, TCP?


When working with tcp, you will find yourself fighting an uphill battle to ensure reliable low latency. This is the result of sane design philosophy for a protocol designed to run always everywhere all at once.


Before we move to the philosophy, let's start by looking at a typical path of request from a browser to a backend server:


#### TCP is blind


The internet is a multi-actor space, where honesty and compliance are not guaranteed (shocker, I know). Therefore, we cannot simply ask all the routers in a path to publish their congestion level and plan accordingly. We have to rely on signals like packets that have not been acknowledged past some time window. For our case, this means we can't perfectly anticipate congestion and redirect traffic to avoid tail network latencies, we know about congestions as they happen.


#### TCP is defensive


Routers dropping packets can create a vicious cycle as all senders have to send the next batch AND retransmit all dropped packets from the previous batch. If senders keep trying to send aggressively during a congestion, more packets are dropped, additional senders join before the old ones are finished, and the congestion grows much like a traffic jam. For this reason, congestion-control algorithms back off defensively. A short-lived burst overwhelming an ISP router causes all the senders going through that router to back-off and enter a slow recovery cycle.


#### TCP is polite


Given the fragility and shared congestion effect across router users, one overly aggressive actor might be able to improve their own performance while hurting others'. It is bad networking etiquette to start with overly large cwnd, or to recover from a congestion signal too quickly. TCP is designed with polite defaults that are needed when routers are both shared and congestion-prone.


So what is there to do for realtime media inference and other such latency sensitive, large payloads over the internet?


Own as much of the path as you can. Own the sender and receiver, then move as much of the path as you can out of the public internet. Own your network, or at least ride your cloud provider's premium network to your GPU. With ownership over routers, the network becomes a resource you partition among your payloads. Short of owning the network, keep your connections as warm as possible, provision for concurrency, and be as aggressive as your path quality justifies.
