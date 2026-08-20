---
schema_version: "1.0.0"
document_id: "c253b369c28387c26858fbafc6a618ec73ebd01c9049de8c08e858f8d6a4be46"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/what-i-learned-from-building-an-ebpf-based-traffic-capture-application/"
published_at: "2025-12-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:41.172985+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:f1cb348fdbd3681b5ab33b829df801f79ddcb319ca6350c00aabf3601504e92a"
---

# What I Learned From Building an eBPF-Based Traffic Capture Application

I just finished building Speedscale’s eBPF-based component to capture and analyze network traffic in a Kubernetes cluster, and it forced me to confront some uncomfortable truths about observability. While there were certainly some challenges along the way, particularly in dealing with Go applications, the approach was relatively straightforward. So naturally one might think expanding this relatively tiny surface area outward into a broader, general-purpose observability agent would be close to the same level of effort.


Not even close.


Building a targeted traffic capture project quickly revealed why building a **general-purpose eBPF**
**observability agent** is so much harder than it looks. The challenges don’t scale linearly with scope, they grow significantly based on the language runtime, the data you’re extracting, and the production constraints you’re operating under.


Here’s what I learned about the gap between “capturing some network traffic” and “building a real observability platform.” eBPF technology now powers a wide range of eBPF applications for observability, security, and performance monitoring, enabling deep visibility and control within the Linux kernel.


🎯 Key Takeaways


- I built Speedscale’s eBPF component to capture plaintext HTTP/HTTPS from Go and OpenSSL apps without proxies, by hooking crypto/tls Read and Write and SSL_read and SSL_write.
- Targeted network capture is the easy 1x case: fixed target functions, simple byte-buffer data, stateless probes, and a stable ABI.
- General-purpose observability is 50 to 100x harder, and it doesn’t scale linearly. Function tracing hits ABI hell across Go, the JVM, and Python, uprobes get expensive in hot loops, and correlation needs goroutine IDs and per-runtime stack unwinding.
- The vendors doing full eBPF APM (Datadog, Elastic, Grafana) run dozens of runtime-specific agents with big teams. eBPF is a scalpel, not a sledgehammer.


## **Introduction to eBPF**


eBPF, or Extended Berkeley Packet Filter, is a groundbreaking Linux kernel technology that allows developers to run sandboxed programs directly within the kernel space. Unlike traditional approaches that require modifying kernel source code or loading kernel modules, eBPF programs can be dynamically injected into the running kernel, providing a safe and flexible way to extend kernel functionality. This capability has revolutionized how we approach observability, networking, and security on Linux systems.


At its core, eBPF builds on the original Berkeley Packet Filter, but vastly expands its capabilities. With eBPF, you can achieve fine-grained control over system operations, enabling you to monitor, filter, and even modify network packets and system calls in real time. These sandboxed programs are tightly controlled by the kernel, ensuring they cannot compromise system stability or security.


As a result, eBPF has become a foundational Linux kernel technology for modern observability and security tools, empowering developers to gain deep insights into system behavior without the risks and complexity of modifying kernel source code or deploying custom kernel modules.


By leveraging eBPF, organizations can implement advanced monitoring, runtime security, and performance analytics, all while maintaining the safety and reliability of their operating system. This fine-grained control and visibility are what make eBPF such a powerful tool for today’s cloud-native and microservices environments.


### **eBPF Architecture**


The architecture of eBPF is designed to provide a robust, secure, and efficient framework for extending the Linux kernel’s capabilities. At a high level, the eBPF ecosystem consists of the Linux kernel, user space, and the eBPF subsystem that bridges the two.


eBPF programs are typically written in a restricted subset of C, then compiled into eBPF bytecode. This compiled eBPF program is loaded into the kernel using the BPF system call, where it undergoes a rigorous verification process by the eBPF verifier. The verifier ensures that the program is safe to run, preventing any actions that could destabilize the kernel or introduce security vulnerabilities. Only after passing this check can the eBPF program be attached to specific kernel functions, tracepoints, or network interfaces, allowing it to observe or modify network traffic, system calls, and other kernel events.


A key feature of the eBPF architecture is the use of eBPF maps—specialized data structures that enable efficient sharing of data between eBPF programs running in kernel space and user space applications. This allows for real-time telemetry data collection, monitoring network packets, and implementing custom logic for packet filtering, load balancing, or security enforcement. User space applications can interact with these maps to retrieve collected data or provide configuration, making the entire system highly dynamic and adaptable.


The eBPF foundation and the broader Linux kernel community are actively advancing the eBPF ecosystem, introducing new tools, libraries, and helper functions to simplify the process of writing, loading, and managing eBPF programs. This ongoing innovation is enabling fine-grained control over kernel behavior and unlocking new use cases in observability, network security, and performance monitoring.


In summary, the eBPF architecture empowers developers to safely run custom code within the Linux kernel, monitor and manipulate network traffic and system events, and efficiently communicate with user space applications. As the eBPF ecosystem matures, it continues to redefine what’s possible within the operating system kernel, making it an essential technology for modern infrastructure and observability solutions.


## **What We Actually Built**


Our tool does one thing well: it captures plaintext HTTP/HTTPS payloads from both Go applications and non-Go applications that use OpenSSL. For Go applications, it does this by attaching eBPF probes to` crypto/tls.(*Conn).Read` and` crypto/tls.(*Conn).Write` . Non-Go applications using OpenSSL attach eBPF kernel probes to socket read/write syscalls and eBPF user-space probes to OpenSSL read/write calls. This is useful for a variety of reasons, but mostly it enables Speedscale to capture application traffic for analysis or traffic replay **without the need for proxies** .


The technical challenges we faced were non-trivial (see[our previous post on Go TLS and eBPF](https://speedscale.com/blog/ebpf-go-design-notes-1/) ), but they were **bounded** . We knew exactly which functions to hook, what data we needed (byte buffers), and what Go’s ABI quirks meant for register access and stack interpretation. And specifically in the case of Go applications, the limited surface area afforded us the ability to take some shortcuts that avoided some other nuanced aspects of Go’s stack management.


But here’s the thing: while what we built provides excellent observability into an application’s inbound and outbound API/network transactions, it only scratches the surface when it comes to providing a **full** observability solution. And understanding why is important if you’re evaluating eBPF for your own use cases.


## **The Observability Spectrum in the Linux Kernel: Where Complexity Explodes**


Scope What You’re Capturing Complexity Multiplier


**Network Traffic** Byte buffers from TCP/TLS functions 1x (Our baseline)


**Function Tracing** Entry/exit with arguments and return values 5-10x


**APM/Full Observability** Spans, traces, metrics, correlation, stack unwinding 50-100x


Let me break down why each jump is so painful.


Expanding monitoring capabilities with eBPF tools introduces new layers of complexity and technical challenges, as these tools enable deeper system call tracing, resource monitoring, and fine-grained observability across modern cloud-native environments.


## **Why Network Traffic Capture Is (Relatively) Easy**


When you’re capturing network traffic, you have several advantages:


1. **Fixed Target Functions:** You’re hooking a small set of well-known functions (` Read` ,` Write` ,` SSL_read` ,` SSL_write` ). No guesswork.
2. **Simple Data Types:** You’re extracting byte arrays. No complex structs, no nested pointers, no object graphs. Just` void*` and a length.
3. **Stateless Extraction:** Each probe fires independently. You don’t need to correlate a` Read` on Thread A with a` Write` on Thread B to understand what’s happening.
4. **ABI Stability:** The buffer pointer is always in a specific register (for Go) or stack location (for C/OpenSSL). It doesn’t change between minor versions unless the entire function signature changes.


These constraints make the problem tractable. You can ship this in production, handle the edge cases, and move on.


By leveraging eBPF code and features like the express data path (XDP), you can enable efficient packet processing at the network interface level, accelerating network observability and performance.


## **The First Wall: Function-Level Tracing**


Now let’s say you want to trace arbitrary functions in your application, specifically business logic like` ProcessOrder()` or` CalculateDiscount()` . Suddenly, everything gets harder. While eBPF makes it possible to trace kernel and user space program activity, tracing arbitrary functions often requires using kernel tracepoints or user space program probes to load eBPF programs for monitoring. Each approach has its own challenges, such as identifying the right hooks and managing the complexity of loading eBPF programs into the kernel for effective observability.


### **Problem 1: ABI Hell Across Languages**


**Compiled Languages (Go, C++, Rust):**


- **Go Stack vs Register Support:** Since Go 1.17, arguments are passed via registers in a version-dependent layout. If you want to extract the third argument to a function, you need to know the exact Go version, the target architecture, and whether certain compiler optimizations kicked in. This is why tools like` bpftrace` struggle with Go user-space tracing without explicit CO-RE (Compile Once – Run Everywhere) support and BTF (BPF Type Format) metadata.
- **Stripped Binaries:** Production Go binaries often use` -ldflags="-s -w"` to strip symbols and reduce size. Now your eBPF program has no idea where` ProcessOrder` even *is* . You’re back to manual reverse engineering or forcing developers to ship debug symbols (good luck with that).
- **Inlining and Optimization:** The compiler might inline your target function entirely, meaning it no longer exists as a discrete entity to hook. Congratulations, your probe does nothing.


**JIT-Compiled Languages (Java, .NET):**


- **Dynamic Addresses:** The JVM generates machine code at runtime. The address of` UserService.createUser()` is not fixed - it can change during execution as the JIT recompiles hot code paths. Standard eBPF` uprobes` can’t handle this without external help.
- **The Map File Sidecar:** You need a separate agent (like` perf-map-agent` for Java) constantly monitoring the JVM and writing` /tmp/perf-<pid>.map` files to track where methods currently live in memory.


**Interpreted Languages (Python, Node.js, Ruby):**


-


**Interpreter-Only Probes:** For a language like Python, when you’re wanting to add a probe to a function, you’re not actually dealing with *your* function - you’re dealing with` PyEval_EvalFrameDefault` , the C function that evaluates all bytecode. To extract a specific variable like` user_id` , your eBPF program must:


1. Read the` PyThreadState` struct from the interpreter’s thread-local storage
2. Follow pointers to the` PyFrameObject` representing the current call frame
3. Parse the local variables dictionary (a hash table) to find` user_id`
4. Extract the` PyObject*` , determine its type, and finally read the actual integer value


-


**USDT Probes:** Some runtimes provide **User Statically Defined Tracing (USDT)** probes - predefined hooks that expose high-level events like` node:http-server-request` . These are a godsend when available, but they’re limited to what the runtime developers decided to expose. If you need something custom, you’re back to struct-walking hell.


### **Problem 2: Performance Death by a Thousand Probes**


A` kprobe` (kernel probe) is pretty inexpensive - it’s just a software trap in kernel space. A` uprobe` (user-space probe) has the potential to be unintentionally expensive since it requires:


- A context switch from user space to kernel space
- A trap to execute your eBPF program
- A context switch back to user space


For network functions that fire occasionally, this is fine. But place a` uprobe` on a function in a hot loop (say, a utility function called thousands of times per second), and you might easily see significant latency increases just due to the aggregated context switch overhead.


For interpreted languages, it’s even worse. If you hook the interpreter’s function dispatch mechanism, your eBPF program runs **for every single function call in the entire application** . Doing this without very precise filtering will likely leave you wishing you hadn’t done this in the first place.


A great example of seeing how this kind of thing can cause significant performance concerns can be seen if you play around with Java’s JVMTI and specifically enable and use its` MethodEntry` and/or` MethodExit` event.


These fire for **every** single function call and that adds up in a hurry.


## **The Second Wall: Correlation and Context**


Okay, let’s say you’ve solved function tracing. Now you want to answer questions like:


- “Which user request caused this database query?”
- “What was the full call stack when this error occurred?”
- “How do I map this kernel event back to a specific goroutine/thread/transaction?”


Granted, these aren’t impossible. They’re just…difficult.


### **Problem 3: Stack Unwinding Across Language Boundaries**


Frame pointers are everything, particularly in a failure scenario. You need to be able to walk backwards from the point of failure to the root function call. The waters become murky when you start to cross into supporting multiple languages. You’ll need to account for stack and frame unwinding that is specific to the language itself and accept the fact that it probably won’t translate to applications that are written in different languages.


### **Problem 4: Concurrency Models Don’t Match**


**Go’s Goroutines:**


-


eBPF sees OS threads (TIDs). Go multiplexes thousands of goroutines onto a small pool of OS threads (M:N scheduling). If eBPF traces a network read on “Thread 123”, but your application logs “Goroutine 99”, you have a correlation problem.


-


**The Solution:** Your eBPF program must extract the` goid` (goroutine ID) from Go’s internal thread-local storage (` g` struct). This requires:


1. Finding the` g` pointer in the` fs` or` gs` register (architecture-dependent)
2. Reading the` goid` field at a specific offset (version-dependent)
3. Mapping it back to your application’s logical execution model


Every Go release can break this. You’re now maintaining ABI-specific mappings across Go 1.17, 1.18, 1.19, 1.20, 1.21, 1.22, 1.23…


**Java’s Virtual Threads (Project Loom):**


- The JVM now has its own M:N scheduler. Same problem, different runtime. Congratulations, you get to solve this again.


## **The Third Wall: Security and Production Readiness**


Even if you solve all the technical challenges, you’re still not done. Production deployment introduces a new class of nightmares. Enforcing robust network security policies is critical in production, and eBPF observability enables this by allowing eBPF programs run directly in the kernel to monitor, enforce, and enhance security measures in real time, ensuring secure and reliable observability.


### **Problem 5: The Cleartext Risk**


eBPF can hook functions **before encryption** . This is exactly what we do for TLS capture, and it’s incredibly powerful for debugging. But with great power comes great responsibility. It is crucial to understand that observability into normally encrypted traffic might contain PII or other sensitive information, and that properly sanitizing or redacting this information is extremely important.


### **Problem 6: The eBPF Verifier is Your Enemy**


The eBPF verifier exists to prevent kernel crashes, but it can also be maddeningly restrictive:


- **No unbounded loops:** You can’t iterate over an arbitrary-length array. Want to sanitize a 1MB HTTP body? Too bad. You need to chunk it. Note, this isn’t the case in more recent kernel releases, but if you want to support a broad range of kernel versions, you can’t exactly rely on the bleeding-edge features.
- **Stack size limits:** no` malloc()` for you - you need to have memory pre-allocated, which typically means that you need an eBPF map with reserved space for what you need. This really isn’t a problem, mind you, it’s just more of a “get used to it” scenario.


## **Where Does This Leave Us?**


After building our TLS capture tool and staring into the abyss of what a **full observability agent** would require, here’s my take:


### **eBPF is Phenomenal For:**


- **Targeted, narrow-scope instrumentation** (network capture, specific syscall tracing)
- **Infrastructure monitoring** (kernel-level metrics, container insights)
- **Security enforcement** (runtime policy checks, anomaly detection)


### **eBPF is Brutal For:**


- **General-purpose APM** across multiple languages
- **Deep application-level tracing** with full context
- **Dynamic, user-driven instrumentation** (“I want to trace this specific function I just deployed”)


The companies succeeding with eBPF observability (Datadog, Elastic, Grafana) have **massive** engineering teams solving these problems individually for each language runtime. They’re not using a single eBPF program - they’re maintaining dozens of runtime-specific agents, each with its own ABI mappings, stack unwinders, and correlation logic. Definitely not something to be taken lightly.


## **Takeaway**


Our capture tool works because the surface area we needed to deal with was small enough to allow us to solve the problem with clear boundaries.


If you’re evaluating eBPF for observability, ask yourself:


1. **What’s the smallest slice of the problem I can solve?** Unless you’re one of the major observability players out here, it’s probably not “full distributed tracing”
2. **How stable are the internal ABIs I’m depending on?** Basically, how willing are you to follow the specifics of a programming language development roadmap and then commit to supporting any changes that might show up?
3. **Can I ship this without requiring users to recompile their applications?**


As a practical consideration, remember that implementing eBPF often requires you to compile pseudo c code into eBPF bytecode using tools like LLVM, so familiarity with ebpf tools is essential for a smooth deployment process.


eBPF is the future of observability, but it’s not magic. It’s a scalpel, not a sledgehammer. Use it where it shines, and don’t pretend the hard problems don’t exist.


## Common questions


### What did Speedscale build with eBPF?


A component that captures plaintext HTTP and HTTPS payloads from Go and non-Go (OpenSSL) applications without proxies. For Go it attaches probes to` crypto/tls (*Conn).Read` and` Write` , and for OpenSSL apps it hooks socket read/write syscalls plus the OpenSSL read and write calls, so traffic can be captured for analysis or replay.


### Why is targeted eBPF traffic capture easier than full observability?


Capture has fixed target functions, simple byte-buffer data types, stateless probes, and a stable ABI, so it is a tractable 1x baseline. Full APM with spans, traces, correlation, and stack unwinding is 50 to 100x harder because the complexity grows with each language runtime, not with scope.


### What makes general-purpose eBPF function tracing so hard?


ABI differences across languages. Go passes arguments in version-dependent registers and strips symbols in production, the JVM and .NET generate code at runtime so addresses move, and interpreted languages force you to walk interpreter structs to find a single variable. On top of that, uprobes in a hot loop add real latency, and correlating a kernel event to a goroutine or thread needs runtime-specific work that breaks on every release.


### When should I use eBPF for observability?


Use it for targeted, narrow-scope instrumentation like network capture, infrastructure monitoring, and security enforcement, where the surface area is small and bounded. Avoid it as a single-program solution for general-purpose APM across many languages, deep application-level tracing with full context, or dynamic user-driven instrumentation. It is a scalpel, not a sledgehammer.
