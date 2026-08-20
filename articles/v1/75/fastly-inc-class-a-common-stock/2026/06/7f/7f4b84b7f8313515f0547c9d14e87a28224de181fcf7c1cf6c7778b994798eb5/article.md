---
schema_version: "1.0.0"
document_id: "7f4b84b7f8313515f0547c9d14e87a28224de181fcf7c1cf6c7778b994798eb5"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/we-built-proxy-behind-firefoxs-new-built-in-vpn-heres-how-it-works/"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T22:07:09.326483+00:00"
content_hash: "sha256:71428af5aedf4dc64146292315eb2c381aab2f621d56d851222b8d46de9a8496"
---

# We Built the Proxy Behind Firefox's New Built-In VPN — Here's How It Works

We were proud and excited when Mozilla shipped Firefox 149 on March 24th with a free, built-in VPN. The headlines focused on the user-facing story: no extra software to install, a single toggle to mask your IP address. We wanted to share the infrastructure underneath, the proxy network that makes the privacy guarantee actually hold.


That proxy is ours. And we think it's worth explaining how it works, why the architecture matters, and what it means for the future of privacy on the web.


## **The Architecture**


The IETF-standardized MASQUE (Multiplexed Application Substrate over QUIC Encryption) protocol forms the basis of our proxy infrastructure.


### **MASQUE: The Transport Foundation**


MASQUE is built on top of QUIC, and it defines how a client can tunnel arbitrary Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) traffic through an intermediary without that intermediary being able to correlate what the client is doing. Traditional HTTPS stacks TLS on top of TCP. TCP is a reliable, ordered byte stream, and if a single packet gets lost or delayed, TCP stops delivery of everything behind it in the queue until that packet is retransmitted and acknowledged. When streaming a video, you see the *buffering* spinner in the middle of your stream. TCP is missing 1 or more packets and is holding the data at the TCP layer because it will not give the application layer data out of order. Once TCP retransmits and receives the packet, the stream will continue playing.


QUIC's connection multiplexing means multiple independent streams can flow through a single connection with no head-of-line blocking, critical for browser traffic, which generates dozens of parallel requests per page load. A multiplayer game is constantly sending many types of data simultaneously, player positions, health updates, chat messages, sound events, inventory changes, hit registration. Over TCP, these share one ordered stream. A single dropped packet for any of them freezes everything else. QUIC lets you open multiple streams within one connection, and each stream is independent. A dropped packet in the player position stream doesn't block the health update stream or the hit registration stream. Each type of game data moves forward on its own, so one piece of lost data doesn't cascade into a full freeze.


When Firefox routes your traffic through our MASQUE proxy, your browser opens a connection to Fastly using HTTP CONNECT. Instead of interpreting and forwarding your HTTP request, CONNECT tells the proxy to open a raw TCP tunnel to the destination. Once established, the proxy becomes transparent and just passes bytes back and forth without understanding them.


A key factor in this is end-to-end encryption. Without CONNECT, a proxy has to decrypt and re-encrypt HTTPS traffic to forward it ([man-in-the-middle style)](https://csrc.nist.gov/glossary/term/man_in_the_middle_attack) . With CONNECT, the TLS handshake happens directly between the client and the destination. The proxy never sees the plaintext. The proxy only knows the destination hostname and port, nothing about what's being transmitted. Because CONNECT creates a raw TCP tunnel, anything can run through it. TLS, Web sockets, custom protocols, even other proxies. The proxy doesn't need to understand whatever protocol runs inside the tunnel.


CONNECT is actually the ancestor of MASQUE. Traditional HTTP CONNECT only tunnels TCP. MASQUE extends the concept to UDP and IP traffic, and moves it to QUIC instead of TCP, which is exactly why it's useful for VPN-style use cases like Firefox's built-in VPN. MASQUE is essentially *HTTP CONNECT but for the modern internet.*


Firefox’s initial built-in VPN rollout uses standard HTTP CONNECT over HTTP/2. We started with a deliberate, low-risk approach for the initial deployment phase. This gave Mozilla and Fastly a chance to confirm the system's design, its performance in real-world conditions, and how users would interact with it, all while operating at a significant scale. The architecture is designed to evolve toward full MASQUE support, and Firefox plans to leverage MASQUE over HTTP/3 in Fastly’s proxy infrastructure later this year.


## **Why Fastly is the Right Proxy Partner**


The proxy role sounds simple: receive, protect the requesting IP address, and forward. But the operational requirements are significant.


**Scale demands are enormous.** Firefox has hundreds of millions of active users. Even at modest adoption rates, the proxy needs to handle traffic volumes that would overwhelm purpose-built infrastructure. Our network is already carrying a significant fraction of the internet's traffic. Adding the proxy workload extends what we already do, not a new operational category.


**Neutrality matters.** Mozilla needed a proxy operator with no commercial incentive to correlate user behavior, no ad business, no data brokerage, and no interest in knowing what Firefox users are browsing. Our business is moving bytes fast and securely, full stop.


## **A Note on What We Don't Do**


Our role in this system needs to be explicit, as we believe transparency contributes to the architecture's trustworthiness. We do not log the destinations of proxied requests. Nor do we subject proxy traffic to deep packet inspection. We do not share connection metadata with Mozilla or any third party beyond what is required for abuse prevention and network operations.


## **The Bigger Picture**


Firefox's built-in VPN is a signal that browser-native privacy infrastructure, not extensions, not separate applications, not third-party services, is becoming table-stakes. MASQUE is the protocol making that possible at scale. We've been building toward this for years. The VPN launch is a significant deployment of our MASQUE proxy, and we are proud to partner with Mozilla. The web's privacy model is being rebuilt from the protocol layer up, and we are glad to be a part of it.
