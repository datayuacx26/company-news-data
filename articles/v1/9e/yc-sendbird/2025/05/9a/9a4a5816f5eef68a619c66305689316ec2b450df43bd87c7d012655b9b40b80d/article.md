---
schema_version: "1.0.0"
document_id: "9a4a5816f5eef68a619c66305689316ec2b450df43bd87c7d012655b9b40b80d"
company_key: "yc-sendbird"
company: "Sendbird"
source_id: "yc-sendbird-news-import-8f7056c4fa29"
canonical_url: "https://sendbird.com/blog/global-edge-proxy"
published_at: "2025-05-22T17:00:00+00:00"
first_seen_at: "2026-07-24T13:12:22.102175+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:a928833947be33689d57640390f0ecd04c54f1ed4a0997ef0ea838d34bf08fd9"
---

# Sendbird Global Edge Proxy Architecture: Testing & Results

### What is a global edge proxy?


A **global edge proxy** is a distributed network service that sits between end users and your application’s origin servers. It routes traffic through a network of edge locations around the world to deliver faster, more secure, and more reliable experiences. By handling requests at the edge—closer to users—it reduces latency, accelerates content delivery, and offloads work from your backend servers. A global edge proxy can also provide built-in security features like DDoS protection, web application firewalls, and TLS termination. Whether you're serving static assets, dynamic content, or API traffic, a global edge proxy helps optimize performance, ensure high availability, and protect your infrastructure—all at global scale.


## Why have we standardized global edge access for chat?


[Low latency](http://sendbird.com/developer/tutorials/what-is-low-latency) plays a critical role in the performance of the Sendbird[Chat API](http://sendbird.com/products/chat-messaging) . When the physical distance between an end user and our servers is significant, latency can degrade the chat experience.


In the past, we configured latency-reducing solutions like[Cloudflare](https://www.cloudflare.com/) and[AWS Global Accelerator](https://aws.amazon.com/global-accelerator/) on a per-customer basis. While effective, this approach introduced challenges in terms of cost, operational complexity, and scalability.


To address this, we standardized our global edge network endpoints to provide fast and reliable access from many geographic locations. This ensures that chat users can connect to our service with minimal delay, regardless of where they are.


In this post, I’ll walk through the design considerations behind our **Global Edge Proxy** and share some of the key challenges we encountered along the way.


## How physical distance and TCP overhead hurt real-time performance


When delivering a global service like[Sendbird Chat](http://sendbird.com/products/chat-messaging) , the physical distance between users and servers inevitably introduces network latency. Even though data travels through optical cables at the speed of light, long distances still create unavoidable delays. For instance, a round trip between Korea and Brazil can take several hundred milliseconds, severely impacting user experience for real-time applications.


Most internet communication, including Sendbird Chat, relies on[TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) . Establishing a TCP connection involves a **3-way handshake** :


-


**SYN:** Client → Server connection request


-


**SYN-ACK:** Server → Client acknowledgment


-


**ACK:** Client → Server final confirmation


This handshake requires **at least 1.5 RTTs (Round Trip Times)** to complete. So if the average RTT between Korea and the western U.S. is 150ms, the handshake adds around **225ms** of delay. When using HTTPS, the[TLS](https://sendbird.com/learn/what-is-tls-ssl-encryption) negotiation can add **another 2 RTTs** , bringing the total connection setup time even higher.


These connection establishment delays are one of the primary reasons users experience a sense of slowness when first interacting with a service. Although these delays only occur during the initial connection, they still degrade the perceived responsiveness. To improve this experience, we explored and implemented infrastructure changes, ultimately leading to the standardization of our global edge network.


**Leverage omnichannel AI for customer support**


[Deploy AI agents](https://delight.ai/contact-sales)


## Solving chat latency with a global edge proxy


To address the latency caused by physical distance, we implemented a strategy of deploying proxy servers (which we call Edge Nodes) near end users. We refer to this system as the Global Edge Proxy, or Gedge.


Gedge performs several critical functions:


-


**Quick TCP connection** : Establishes a fast TCP connection from a nearby location.


-


**Efficient relay** : Maintains communication with the origin server on the user’s behalf.


-


**TLS offloading** : Absorbs the TLS handshake and initial request load.


-


**Latency reduction** : Minimizes user-perceived latency and improves overall UX.


In short, Gedge reduces the effects of physical distance by distributing entry points and bringing the network edge closer to the user.


## Sendbird global edge proxy requirements


The Sendbird Global Edge Proxy system was designed with the following key requirements:


-


Deliver performance on par with services like[Cloudflare](https://www.cloudflare.com/) and[AWS Global Accelerator](https://aws.amazon.com/global-accelerator/)


-


Offer significantly improved cost efficiency over existing solutions


-


Be easy to apply across all customers without custom configurations


-


Support essential protocols at the edge level


-


Accommodate customers with strict security needs, such as fixed IP requirements


-


Minimize operational and maintenance overhead


-


Enable rapid rollback in the event of a failure


-


Provide clear metrics to monitor system health and performance


## Architecture design and selection


Since Sendbird was already operating an[Istio/Envoy](https://istio.io/latest/docs/reference/config/networking/envoy-filter/) -based infrastructure, we prioritized reviewing the option of expanding it to configure a Global Edge Proxy. We conducted performance tests comparing[Cloudflare](https://www.cloudflare.com/) ,[CloudFront](https://aws.amazon.com/cloudfront/) , and our existing service provider.


*Latency measurement (in ms) using Cloudflare, CloudFront, Control, and Envoy across Europe, US, APAC, and Korea locations.* *Cross-region latency measurement (in ms) using Cloudflare, CloudFront, Control, and Envoy across between Europe, US, APAC, and Korea.*


As shown above, the Istio + Envoy combination demonstrated good performance, so we chose it without considering separate alternatives.


### Optimizing WebSocket latency across protocols


One of the main objectives was to accelerate[WebSocket](https://sendbird.com/learn/what-are-websockets) requests in addition to general HTTP requests. Therefore, WebSocket connection delays were compared in the following three cases.


-


WebSocket over HTTP/2 connect


-


WebSocket over HTTP/1.1 upgrade


-


WebSocket without proxy


When proxied through HTTP/2, there was a significant improvement in latency, but in some cases, it was actually slower when using HTTP/1.1. While there were options to use HTTP/3 or gRPC for proxying WebSocket, we chose the more familiar HTTP/2 first.


### Key design decisions


Based on the results of our tests, we arrived at the following design choices:


-


Support both[HTTP and WebSocket](http://sendbird.com/developer/tutorials/websocket-vs-http-communication-protocols) requests from end users


-


Proxy all traffic using HTTP/2 after reaching the Global Edge


-


Use the HTTP/2 CONNECT method for proxying WebSocket connections between edge nodes, instead of the HTTP/1.1 Upgrade method


-


Maintain efficient, persistent connections between the Edge Proxy and the origin server


-


Prevent connection interruptions by using appropriate TCP Keep-Alive settings


*Chat API Global Edge Network architecture diagram*


In the initial phase of Global Edge deployment, both Istio Gateway and Nginx were used as chat gateways. As illustrated above, to maintain a unified proxy layer, all cross-region connections were routed through the Global Edge.


As the gateway infrastructure was fully migrated to Istio, we updated the configuration to enable direct proxying from each local proxy to the remote gateway, as shown below.


*Chat API Global Edge Network with direct proxy architecture diagram*


**Harness proactive AI customer support**


[Launch AI agents](https://sendbird.com/ai-agent/omnipresent)


## Request flow and domain configuration


Currently, Sendbird’s[Chat API](http://sendbird.com/products/chat-messaging) uses the following domain format from the end user’s perspective:


- ` api-${appid}.sendbird.com`
- ` ws-${appid}.sendbird.com`


These App ID–based domains resolve to the actual origin region (backend server) via CNAME records:


- ` api-${sbregion}.sendbird.com`
- ` ws-${sbregion}.sendbird.com`


To support the introduction of the Global Edge Proxy, we extended the domain structure as follows:


-


Introduced a new Edge Proxy domain format:


- ` api-proxy-${awsregion}.sendbird.com`


- Mapped the existing regional domains using Geo Proximity records


-


Routed requests to the existing Gateway of the region


-


Routed requests to the nearest Proxy when the client is in a different region


In this context:


- ` ${sbregion}` refers to Sendbird’s internal server region classification
- ` ${awsregion}` refers to the[AWS](http://sendbird.com/partners) region


When applying this setup, we needed fine-grained control to proxy only specific App IDs. Additionally, clients located in the same region as the origin Gateway do not need proxy routing. To address these scenarios, we introduced two additional domain formats:


- ` api-direct-${sbregion}.sendbird.com` – for direct connections to the origin region
- ` api-gedge-${sbregion}.sendbird.com` – for routing through the Global Edge Proxy


As a result, our architecture is now divided into two strategies:


1.


**Region-wide proxying** for batch migrations


2.


**App-specific proxying** during onboarding, allowing selective adoption


### Region-wide proxying


### App-specific proxying


Through the above configuration, requests from clients located far from the Origin Gateway are directed to a nearby Regional Proxy instead of going directly to the Origin Gateway.


### Origin resolution from proxy to backend


Once a request reaches the proxy, it must be forwarded to the correct origin server. At Sendbird, each` appid` is uniquely associated with an` $sbregion` that represents the backend server’s region.


Previously, this mapping was straightforward — the` $sbregion` could be inferred directly from the` appid` domain. However, with the introduction of proxy routing, the proxy itself must dynamically determine the appropriate origin server for each` appid` .


While we could maintain a static mapping of` appid` to` $sbregion` within the proxy, this approach risks mismatches and inconsistencies. Instead, we rely on DNS resolution to ensure accuracy.


For example, querying the CNAME record for` api-${appid}.sendbird.com` will return one of two formats, depending on the routing configuration:


- ` api-${sbregion}.sendbird.com`
- ` api-gedge-${sbregion}.sendbird.com`


We parse the` $sbregion` from the returned CNAME and use that information to route the request appropriately. This parsing and origin resolution is handled via an **Envoy Lua script** running within the proxy.


During implementation, we discovered that performing traditional UDP-based DNS queries was problematic with Envoy Lua. To address this, we opted to perform DNS resolution over HTTP, using the` Host` header as the lookup key. Services like **Google DNS** and **Cloudflare DNS** (which support DNS-over-HTTPS) were used to build the solution represented below.


*Origin server discovery architecture diagram*


## Global edge network validation and load testing


For basic functional testing, we reused our existing **HealthChecker** , which simulates user service scenarios to validate core features and ensure expected behavior.


For load testing, we used a **[Grafana k6](https://grafana.com/docs/k6/latest/)** script to evaluate connection ramp-up and measure latency across different connection states. The test also verified that connections were being reused as intended, confirming efficient connection handling under load.


*WebSocket load testing with Grafana K6* *WebSocket load testing with Grafana K6 results on dashboard*


## Enhancing observability in a multi-proxy architecture


With the introduction of the Global Edge Proxy, requests now traverse multiple proxy layers before reaching the Gateway. This increased complexity makes it essential to accurately trace the path of each request when troubleshooting issues.


### Request ID propagation and metric visibility


By default, the[Istio Gateway](https://istio.io/latest/docs/reference/config/networking/gateway/) generates a new Request ID for every incoming request. In a traditional setup, this is sufficient since the Gateway is the first point of contact. However, with the Global Edge in place, the original Request ID issued by the Edge Proxy was being overwritten, making it challenging to trace requests end-to-end through the proxy chain.


To resolve this, we enabled the` preserve_external_request_id: true` option in the Istio Gateway settings. This ensures the Request ID generated by the Global Edge is retained throughout the request lifecycle.


Istio also presents challenges when tracking metrics for endpoints not registered within the cluster. To overcome this, we registered all relevant upstream services as` ServiceEntry` resources, allowing them to be monitored and managed within the mesh.


In addition, we included metadata about the requester in the **request headers** when forwarding to the upstream proxy and inserted source information into the **response headers** during the return path. This allows the request and response to be clearly attributed in telemetry data, enabling detailed visibility through[Istio’s Telemetry API](https://istio.io/latest/docs/tasks/observability/telemetry/) .


*Custom header attachment in Global Edge*


## Deployment strategy


Because this deployment impacts the entire application stack, careful planning was essential to ensure stability and minimize risk. To support a safe and controlled rollout, we followed a phased deployment process:


1. **Initial testing with the HealthChecker via Global Edge Proxy** Verified end-to-end (E2E) functionality using the HealthChecker to ensure baseline reliability through the proxy layer.


2. **Gradual rollout to selected apps** Applied the changes incrementally to a subset of applications to monitor real-world behavior and reduce blast radius.


3. **Progressive expansion to all regions** Once stability was confirmed, we scaled the deployment across all regions.


4. **Exemptions for apps with special requirements** Applications requiring static IP addresses or other specific configurations were excluded from proxy routing.


5. **DNS-based deployment and rollback control** Leveraged DNS records to manage both rollout and rollback scenarios without requiring code changes.


6. **IaC-based routing control** Used Infrastructure as Code (IaC) to precisely manage Global Edge Proxy routing at the app level.


7. **Ongoing validation with HealthChecker** Continuously monitored proxy-routed traffic to ensure correct functionality throughout the deployment.


**Automate customer service with AI agents**


[Create AI agents](https://delight.ai/contact-sales)


## Key issues and responses


During the rollout of the Global Edge Proxy, we encountered several unexpected issues. Below are the key challenges and how we addressed them.


### 1. Host header handling


Some clients, such as Unity, appended port information to the Host header. Since the Host header is critical for Proxy → Origin mapping, this variation caused routing issues we hadn’t initially accounted for.


To accommodate these client-specific differences, we configured the following Envoy options:


- ` strip_any_host_port` – removes port information from the` Host` header
- ` strip_trailing_host_dot` – normalizes the` Host` header by removing trailing dots


### 2. Connection termination from protocol errors


With fewer connections maintained between proxies, a single protocol error in one stream could cause the entire TCP connection to be dropped. This was particularly problematic when multiple clients shared a connection.


To mitigate this, we enabled the following Envoy setting:


- ` override_stream_error_on_invalid_http_message` – ensures that stream-level protocol errors don’t terminate the entire connection


### 3. WebSocket connection management


[WebSocket](https://sendbird.com/learn/what-are-websockets) connections are inherently long-lived and more sensitive to disruptions. To ensure reliability:


-


We reinforced error detection and exception handling at the application level


-


We implemented safeguards to maintain WebSocket connections as robustly as possible


### 4. Cost inefficiencies in certain regions


In regions like São Paulo, Brazil, we observed disproportionately high operational costs. To address this:


-


We adjusted the **bias values** in the **Geo Proximity DNS records** , directing traffic more efficiently to cost-effective regions


## Global edge proxy latency results


Following the deployment of the Global Edge Proxy, we achieved a **~50% reduction in initial connection latency** for both API and WebSocket requests. This significantly improved the user experience, particularly for customers in geographically distant regions.


*Global Edge apps regional request distribution results*


Throughout the rollout, we also collected extensive regional traffic metrics, which deepened our insight into global usage patterns. This data has proven valuable for further tuning and performance improvements.


Additionally, the new architecture enabled meaningful gains in **cost optimization** and **operational automation** , reducing the overhead of maintaining custom solutions per customer.


With the Global Edge Proxy in place, Sendbird is now positioned to deliver **fast and reliable chat services** more efficiently to users around the world. We remain committed to continuous improvement through advanced traffic monitoring and ongoing infrastructure optimization, ensuring top-tier performance across diverse network conditions.


If you’d like to experience the Sendbird experience, try out our[chat free trial](https://sendbird.com/form/free-trial) .
