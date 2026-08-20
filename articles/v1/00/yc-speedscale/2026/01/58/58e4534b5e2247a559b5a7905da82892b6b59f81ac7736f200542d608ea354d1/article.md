---
schema_version: "1.0.0"
document_id: "58e4534b5e2247a559b5a7905da82892b6b59f81ac7736f200542d608ea354d1"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/speedscale-moves-observability-data-collector-sidecars-ebpf/"
published_at: "2026-01-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T22:23:25.248280+00:00"
content_hash: "sha256:5d7aa799f1397564ef0060201544410ac62241e3f8644887188a968acc2b70bb"
---

# Moving Our Observability Data Collector from Sidecars to eBPF

# Why We’re Moving Beyond the Kubernetes Sidecar: The Role of eBPF


For years, the Kubernetes sidecar pattern has been a practical way to capture observability data. Running a collector alongside each application pod gave us deep visibility into traffic, including full request and response payloads across supported protocols.


However, as cloud-native environments have grown more complex, the limitations of sidecars—such as resource overhead, operational complexity, and scaling challenges—have become more apparent. This has driven the shift toward eBPF, which offers a more efficient, kernel-level approach to observability.


Observability is now recognized as a crucial capability for maintaining the availability, performance, and security of modern software systems. Its ability to provide deep visibility into distributed tech stacks enables automated, real-time problem identification and resolution. Observability tools are a natural evolution of application performance monitoring and network performance management data collection methods. They focus on three main telemetry types—logs, traces, and metrics—which serve as external outputs essential for diagnosing issues and maintaining system health. These tools enable developers to collect, analyze, correlate, and discover a broad range of telemetry data, and the more observable a system, the faster IT teams can shift from an identified performance issue to its root cause. Observability integrates monitoring into the early phases of the software development process, accelerating issue discovery and resolution, and helping to keep app availability high. High-quality observability data insights lead to faster feedback in software development and testing processes. Artificial intelligence is transforming observability by integrating advanced analytics, automating remediation processes, and leveraging causal AI for root cause analysis. Full-stack observability helps IT teams discover and address unknown issues that traditional monitoring tools might miss, while observability tools help improve user experience by optimizing application performance based on telemetry data.


eBPF’s ability to provide fine-grained, real-time observability and measure system performance at the kernel level makes it a powerful alternative to sidecars. By operating within the Linux kernel, eBPF can capture detailed telemetry with minimal overhead, enabling organizations to monitor, analyze, and secure their infrastructure more efficiently.


## How Sidecars Work (and Where They Fall Short)


In a sidecar-based architecture, traffic flows like this:


```text
Client → Sidecar Proxy → Application Pod → Sidecar Proxy → Client
```


The sidecar intercepts traffic using iptables rules or CNI plugins that redirect connections through the proxy. However, iptables-based interception only works sometimes and often requires manual configuration for the proxy, adding operational complexity. This introduces several technical issues:


**Latency Overhead** : Each request passes through the sidecar twice (inbound and outbound), adding:


- User-space context switches (kernel → sidecar → application)
- Memory copies between kernel and user space buffers
- Proxy processing time (parsing, buffering, connection management)
- Every app is different, but can add 1-5ms of latency per request at p50, with higher overhead at p99


**Resource Consumption** : Sidecars consume resources independently of the application:


- Each sidecar needs its own memory allocation (typically 20-200MB per pod)
- CPU overhead from proxy processing (<1% per pod can add up)
- Network buffer overhead for proxying connections
- At scale, this multiplies: 1000 pods = 1000 sidecars = significant aggregate overhead


**Operational Coupling** : Sidecars create tight coupling between observability and application health:


- Sidecar failures can impact application availability
- Coordinated upgrades require careful orchestration
- Resource limits must be set for both sidecar and application
- Debugging becomes more complex (is it the app or the sidecar?)


**Configuration Complexity** : Each sidecar needs:


- iptables/CNI configuration for traffic interception
- Proxy configuration (Envoy config, routing rules, etc.)
- Health checks and lifecycle management
- Resource requests and limits
- Version coordination with the application


But sidecars come with tradeoffs:


- **Added latency** from proxying traffic through user-space components
- **Operational risk** when application health becomes coupled to a sidecar
- **Increased resource consumption** due to per-pod overhead
- **More moving parts** to configure, upgrade, and debug


As clusters grow and workloads become more performance-sensitive, these costs add up.


That’s why Speedscale is moving its observability data collection from a Kubernetes sidecar model to **eBPF** . Over the last several years, eBPF has rapidly become a foundation for modern cloud-native infrastructure—tools like Cilium, Envoy, and security observability platforms have embraced eBPF for its performance and safety benefits. However, Speedscale’s implementation faces a unique challenge: unlike many tools that capture only network headers or metadata, Speedscale captures **full request and response payloads** across all supported protocols, making our eBPF implementation significantly more complex than tools that operate on headers alone.


## Cloud-Native Applications & Design


Cloud-native applications are built from the ground up to harness the full potential of cloud computing. By embracing cloud native principles, organizations can achieve unprecedented scalability, flexibility, and resilience in their software systems. At the heart of this approach is the use of microservices architecture, where applications are composed of independent, modular services that can be developed, deployed, and scaled individually.


These services are typically packaged as containers, using tools like Docker, which provide a consistent runtime environment across different stages of software development and deployment. Orchestration platforms such as Kubernetes have become essential for managing these containers at scale, automating tasks like service discovery, load balancing, and resource allocation within a kubernetes cluster.


The adoption of cloud-native design means that applications are no longer monolithic. Instead, they are collections of loosely coupled components that communicate over well-defined APIs. This modularity allows teams to iterate quickly, deploy updates with minimal risk, and recover gracefully from failures. Tools and services in the cloud-native ecosystem—ranging from storage systems to networking solutions—are designed to integrate seamlessly, enabling developers to focus on delivering business value rather than managing infrastructure.


By leveraging the power of cloud computing, cloud-native applications can dynamically adjust to changing demand, optimize resource usage, and maintain high availability even in the face of hardware or software failures. This design philosophy is foundational for modern software development, especially as organizations move toward more complex, distributed environments.


# What eBPF Programs Change


eBPF (extended Berkeley Packet Filter) allows code to run safely in the Linux kernel. Instead of intercepting traffic in user space, we can observe network activity **directly at the kernel level** . eBPF achieves significant performance improvements by being jit compiled, allowing it to run directly in the kernel for faster processing.


## Technical Architecture


At a technical level, eBPF programs attach to kernel hooks—in our case, primarily` socket` and` sock_ops` hooks for network traffic observation. The kernel’s eBPF verifier ensures our programs are safe to execute: they can’t crash the kernel, access arbitrary memory, or create infinite loops. This safety guarantee is critical for production deployments.


Our eBPF programs run in the kernel’s network stack, attaching to sockets before data enters user space. This means we can observe traffic **before** it reaches application code, eliminating the need for a proxy layer entirely.


## Implementation Challenges


Moving from sidecar-based collection to eBPF required solving several technical challenges:


**State Management** : eBPF programs are stateless by design—each function call triggers a fresh program execution. To coordinate function calls and returns for TCP flow tracking, we use eBPF maps (hash tables and LRU caches) to maintain connection state. These maps track TCP flows and coordinate between different eBPF program invocations, allowing us to match requests with responses. These maps are shared across program invocations and can be accessed from user space for coordination.


**Protocol Parsing** : Fortunately, we were able to re-use our existing parsing capabilities. Our protocol parsers for HTTP/1.1, HTTP/2, gRPC, and other protocols were adapted to work at the kernel level, maintaining parsing state in eBPF maps. For HTTP/2, this includes tracking multiplexed streams and HPACK header compression state.


**Memory Constraints** : eBPF programs run in a constrained environment with limited stack space (512 bytes) and instruction limits. We had to optimize our parsing logic to fit within these constraints while maintaining correctness.


**The Full Payload Challenge** : Unlike many eBPF-based observability tools that operate on packet headers or connection metadata, Speedscale captures complete HTTP request and response bodies, gRPC messages, database query payloads, and other application-level data. This requires sophisticated protocol state machines (for HTTP/2 HPACK, gRPC message boundaries, etc.), efficient payload extraction to user space, and multi-protocol support—all running within eBPF’s constraints. The complexity is compounded by eBPF’s limited instruction counts, restricted memory access patterns, and the need to pass the verifier.


With this transition, Speedscale continues to provide:


- **Full payload visibility** for all supported protocols
- **Protocol-aware traffic understanding**
- **Production-safe observability**


But now with:


- **Lower latency** , since traffic no longer flows through an extra proxy hop
- **Reduced risk to applications** , because data collection is no longer in the request path
- **Lower resource consumption** , by eliminating per-pod sidecar overhead


In practice, we’ve consistently observed **meaningfully reduced CPU and memory usage** compared to sidecar-based collection, especially in high-throughput environments. Specifically, we’ve measured:


- **30-50% reduction in CPU usage** for traffic collection
- **60-80% reduction in memory footprint** per pod (eliminating sidecar containers)
- **Sub-millisecond latency overhead** compared to 1-5ms typical with sidecar proxying


---


# Real-World Observations from Speedscale


After deploying our eBPF-based collector in real environments, we’ve seen:


- **Lower tail latency** compared to sidecar interception
- **More predictable performance under load**
- **Simpler operational models** , with fewer pod-level components to manage


## Performance Characteristics


In production deployments, we’ve measured concrete improvements:


**Latency Impact** : Sidecar-based collection adds latency at the 99th percentile due to:


- User-space context switches (kernel → sidecar → application)
- Memory copies between kernel and user space
- Proxy buffering and connection management


Our eBPF implementation eliminates the sidecar hop entirely. Traffic flows directly from kernel to application, with observation happening in parallel. We’ve measured **p99 latency reductions of 2-5ms** in typical workloads, with larger improvements in high-throughput scenarios where sidecar overhead compounds.


**Resource Efficiency** : Sidecars consume resources per pod:


- Each sidecar container needs its own memory allocation (typically 50-200MB)
- CPU overhead from proxy processing (5-15% per pod in our measurements)
- Network overhead from proxying connections


eBPF programs share kernel resources efficiently. A single eBPF program can observe traffic across many pods on a node, dramatically reducing per-pod overhead. We’ve seen **60-80% reduction in memory usage** and **30-50% reduction in CPU usage** for observability collection.


**Scalability** : Sidecar-based collection scales linearly with pod count—more pods mean more sidecars. eBPF scales with node count—one program per node observes all pods. This becomes critical at scale: a cluster with 1000 pods needs 1000 sidecars, but only ~10-20 eBPF programs (one per node).


**Operational Simplicity** : Sidecars require:


- Per-pod configuration and lifecycle management
- Coordinated upgrades (sidecar and application versions)
- Health checks and restart policies
- Resource limits and requests


eBPF programs are deployed at the node level, managed independently of application pods. Upgrades don’t require pod restarts, and failures don’t affect application health.


Most importantly, we achieved these improvements **without sacrificing visibility** . Engineers still get the same rich, protocol-level insight into real production traffic that Speedscale is known for.


---


# eBPF Isn’t a Silver Bullet


While eBPF unlocks powerful kernel-level observability, it doesn’t solve everything.


## Limitations of Kernel-Level Observation


eBPF operates at the network layer, which means it sees TCP streams and IP packets. This is powerful, but insufficient for complete application understanding:


**Application Semantics** : Kernel-level observation sees bytes on the wire, but doesn’t understand application-level concepts. For example:


- Database connection pooling: multiple logical connections multiplexed over fewer TCP connections
- Application-level retries: the same logical request might appear as multiple TCP connections
- Service mesh abstractions: mTLS termination, circuit breakers, and retry policies happen above the kernel


**Language Runtime Behavior** : Some languages, particularly Java, have complex runtime behavior that affects network traffic:


- **Connection pooling** : Java applications often maintain pools of TCP connections, reusing them across logical requests. From the kernel, this looks like idle connections, but the application sees active request handling.
- **NIO and async I/O** : Java NIO uses selectors and channels that can multiplex many logical operations over fewer connections. Kernel-level observation misses this abstraction.
- **JVM-specific protocols** : Some Java frameworks use JVM-specific serialization or connection patterns that require runtime understanding.


**Encrypted Traffic** : eBPF can observe encrypted traffic, and we are indeed capturing encrypted traffic. The real value, however, is that we no longer need to manage encryption certificates for traffic interception, which significantly cuts configuration time. Previously, sidecar-based collection required certificate management and configuration for TLS termination, adding operational overhead. With eBPF, we observe the encrypted traffic directly without needing to configure or manage certificates.


**Stateful Protocol Behavior** : Some protocols maintain state that’s difficult to reconstruct from packet inspection alone:


- HTTP/2 connection settings and flow control windows
- gRPC stream state and backpressure signals
- WebSocket frame sequences and connection lifecycle


## Our Hybrid Approach


That’s why Speedscale continues to pair eBPF with **language-level instrumentation** , including Java instrumentation where needed. This hybrid approach gives us both:


- The **performance and safety benefits of eBPF** for the majority of traffic observation
- The **semantic clarity** required to accurately model application behavior where kernel-level observation falls short


We use eBPF for:


- High-performance, low-overhead traffic capture
- Protocol parsing and payload extraction
- TCP flow tracking and coordinating function calls and returns


We supplement with language-level instrumentation for:


- Java applications requiring runtime context
- Encrypted traffic where keys are available in-process
- Application-specific semantics that aren’t visible at the network layer


This hybrid model gives us the best of both worlds: the efficiency of kernel-level observation with the completeness of application-level understanding where needed.


## Overcoming Complexity with Automation


As cloud-native applications grow in scale and complexity, automation becomes the linchpin that enables developers to manage, deploy, and optimize these systems efficiently. Automation tools—such as Jenkins, GitLab CI/CD, and Kubernetes operators—allow teams to streamline the entire software development lifecycle, from code integration and testing to deployment and scaling.


In a cloud-native context, automation is not just about speeding up deployments; it’s about ensuring that every component of the system is running optimally and securely. For example, automation can be used to provision resources on demand, balance workloads across services, and enforce security policies consistently across the kubernetes cluster. This reduces manual intervention, minimizes human error, and frees up developers to focus on writing high-quality code.


Observability is another area where automation shines. By integrating eBPF programs into the application stack, teams can automatically collect custom metrics and gain fine-grained visibility into system performance and application health. These insights help identify bottlenecks, optimize resource requirements, and ensure that services are running efficiently. Automation tools can then use this data to trigger scaling events, adjust configurations, or alert developers to potential issues before they impact users.


Security is also enhanced through automation. Automated processes can continuously monitor for vulnerabilities, enforce compliance policies, and respond to threats in real time. For instance, Kubernetes can be configured to automatically isolate compromised containers or roll out security patches without downtime, maintaining a secure and controlled environment for all services.


Consider an example: a developer is building a cloud-native application composed of several microservices, each running in its own container. Using Kubernetes, the developer defines how these services should be deployed and scaled. Automation tools handle the CI/CD pipeline, ensuring that new code is tested and deployed seamlessly. Meanwhile, eBPF programs collect detailed observability data, providing the context needed to optimize performance and maintain security. If a spike in traffic occurs, automation ensures that additional resources are provisioned automatically, keeping the application highly available and responsive.


In summary, automation is essential for overcoming the complexity inherent in cloud-native software development. By leveraging automation, developers can deploy and manage applications with confidence, ensure robust security, and maintain deep observability—all while focusing on delivering innovative features and services to users.


# What This Means for Engineers


By moving our observability data collector to eBPF, Speedscale delivers:


- Production-safe, low-latency traffic capture
- Reduced resource usage at scale
- Deep visibility without application risk
- A future-proof foundation aligned with where the cloud-native ecosystem is heading


We believe eBPF represents the next evolution of observability infrastructure—and we’re excited to bring those benefits to our customers without compromise.


Critically, this eBPF-based capture is what closes the Observability Gap. Traditional observability gives you sampled traces and aggregated metrics — enough to diagnose incidents after the fact. Full-fidelity traffic capture gives you the raw material to[verify code changes against production reality](https://speedscale.com/features/ai-code-verification/) *before* deployment. That’s the difference between reactive monitoring and proactive validation.


---


# Learn More


Ready to see eBPF-based observability in action?


- Explore **Speedscale** at[speedscale.com](https://speedscale.com/)
- Dive into the technical details at[docs.speedscale.com](https://docs.speedscale.com/)
- [Get started](https://speedscale.com/) with production-safe traffic capture today


If you’re curious about how Speedscale uses eBPF in practice, or how this change impacts your environment, we’d love to talk.
