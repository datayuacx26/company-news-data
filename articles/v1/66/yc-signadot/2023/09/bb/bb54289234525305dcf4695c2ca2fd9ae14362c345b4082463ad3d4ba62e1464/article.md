---
schema_version: "1.0.0"
document_id: "bb54289234525305dcf4695c2ca2fd9ae14362c345b4082463ad3d4ba62e1464"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/is-wasm-the-future-for-kubernetes-operators/"
published_at: "2023-09-12T01:51:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:1be0e4ad8ad23054afd4b10c811af4db01775f232f4629d81b1ed7785d2ccdd0"
---

# Is WASM the future for Kubernetes Operators?

A question that still feels unanswered is whether Operations and backend engineers should use WASM as part of their workflow.


## What is WASM?


Originally strongly associated with the browser, WebAssembly (WASM) is a memory-safe, sandboxed execution environment that can be implemented inside existing stack-based virtual machines. It’s designed to be part of the open web platform, maintaining the versionless, feature-tested, and backwards-compatible nature of the web. WASM also supports non-web applications, but a quick tour of[webassembly.org](http://webassembly.org/) will remind you that the browser remains the core focus.


WASM has two main advantages outside of the browser: it compiles high-performance modules that can originate in[the language of your choosing](https://github.com/appcypher/awesome-wasm-langs) . The ability to write in your language and get a high-performance module makes a strong case for using WASM in places like Envoy filters.


## Using WASM to create Envoy filters


Envoy, the high-performance L3/L4 and L7 proxy that serves as the foundation for many service mesh implementations including Istio, is designed around the core concept of connection and traffic handling, which is facilitated by network filters. These filters, when mixed into filter chains, enable the implementation of a wide array of higher-order functionalities. These functionalities range from access control to transformation, data enrichment, auditing, and more, providing a framework for network management.


With WASM you can create filters in the language you’re comfortable writing in, and deploy something that performs not quite as well as native C++, but still respectably and far more portable.


### **SDK and Classes**


Envoy Proxy runs WASM filters inside a stack-based virtual machine. The interactions between the host (Envoy Proxy) and the WASM filter are facilitated by the Envoy Proxy WASM SDK. More detail on writing a filter in C++ is in this[great tutorial from Toader Sebastian](https://banzaicloud.com/blog/envoy-wasm-filter/) .


## Building your first WASM filter for Envoy


Because of specific dependencies that will need to be versioned correctly, you’ll have a far easier time building your first filter in a docker image rather than natively on your machine


### 1. write your filter


Your filter implementation must derive from two classes: **RootContext** and **Context** . The **RootContext** is created when the WASM plugin is loaded and is used for initial setup and interactions that outlive a request. The **Context** class provides hooks for HTTP and TCP traffic, allowing you to manipulate the traffic.


### 2. Build your filter in a container


Start by creating a docker image with the C++ Envoy Proxy WASM SDK documented[in the WebAssembly for Proxies GitHub](https://github.com/proxy-wasm/proxy-wasm-cpp-sdk#docker) . Here’s the documentation if you’re[working in Rust](https://github.com/proxy-wasm/proxy-wasm-rust-sdk) .


Then create a makefile which requires the SDK and pulls in the filter code. The most simple would just be:


### 3. Deploy your filter with Istio


In order to try out our filter within our own cluster, we’ll need to


- Inject our filter as a WASM binary to be available to our containers
- Add the dependency with kubectl patch
- Add a patch config to your existing istio filter


### 3a. (alternatively) Deploy your filter to WebAssemblyHub


Injecting your filter directly into your containers can definitely start to feel clunky, even for initial development testing, and it’s definitely the step where the most stuff can go wrong. Thankfully the fine people at[solo.io](http://solo.io/) have released[WebAssemblyHub](https://docs.solo.io/web-assembly-hub/latest) , where you can deploy your binaries and grab them as requirements, very similarly to its namesake dockerhub.


Their CLI tool[wasme](https://docs.solo.io/web-assembly-hub/latest/reference/cli/) can let you skip all the steps in part 3. Wasme can even build in a few languages on its own, though I didn’t try this path for building filters.


## Is WASM the right choice for Kubernetes Operators?


While building filters for envoy is a compelling use case, let’s look at all the benefits it can deliver:


**Startup Times** : WASM’s quick startup times can be beneficial for serverless or “scale to zero” applications. If you have functions that only run occasionally, the reduced cold start times can improve responsiveness.


**Portability** : While containers have largely solved the portability issue across different Linux distributions, WASM takes this a step further by providing a consistent runtime environment. This can simplify development and deployment.


**Security Patching** : With WASM, you may not have to rebuild modules when vulnerabilities are found in your base images. This can streamline the maintenance process.


### Considerations and Skepticism


1. **Need for Speed** : If you’re satisfied with the startup time of containers, the speed advantage of WASM may not be compelling. Native performance might still be preferable for applications that run continuously. It’s arguable that most of the benefits of WASM in the Kubernetes space e.g. faster launch time, better security are also achieved with swapping containers for VMs e.g. Firecracker. With the recent work AWS has done around[snapshotting](https://github.com/firecracker-microvm/firecracker/blob/main/docs/snapshotting/snapshot-support.md) you can launch a VM in less than a second. And it’s proven to work and scale since it forms the basis for AWS Lambda/Fargate.
2. **Server vs. Browser Context** : WASM was initially designed for browsers, and its applicability to server environments is still a subject of debate. Some believe that its benefits may not translate well to server-side applications.
3. **Maturity** : WASM is relatively young, and its performance gap with native code might close in the future. However, it’s worth considering that the technology is still evolving, and some of its potential benefits may not yet be fully realized.


### Conclusion


WASM’s benefits in startup times, portability, and security patching can be attractive for specific use cases, but these advantages may not be universally applicable. Skepticism around WASM’s success in server environments is not unfounded, and careful evaluation is needed to determine if it’s the right fit for your particular needs and applications.


‍
