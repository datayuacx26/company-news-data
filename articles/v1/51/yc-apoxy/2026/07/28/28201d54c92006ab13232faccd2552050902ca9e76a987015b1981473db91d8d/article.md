---
schema_version: "1.0.0"
document_id: "28201d54c92006ab13232faccd2552050902ca9e76a987015b1981473db91d8d"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/enter-clrk"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:03.880414+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:ee8fae62a4940064275fb835d00f6378d60e6ac848a3d5028edd5ad1d3256ac6"
---

# CLRK: an open-source Cognitive Loop Runtime for Kubernetes

Over the past year, as models got good enough to actually trust with real work, a lot of us started running AI agents in production. And like us, you've probably developed a fresh appreciation for doing that securely and reliably. Much of how we approach it is not new, but the inherent non-determinism of LLMs adds a twist or two. Especially with the unsupervised kind: unlike Claude Code, Codex, and other popular coding harnesses, these agents poke at your production environment, talk to your customers, and run for a long time with no human watching over them.


Production agent deployments fail somewhere between 41% and 87% of the time, depending on which study you read. Step-repetition - the agent looping uselessly on the same action - is the second most common failure mode across the major frameworks, clocking in at 17.1%. Last summer, Replit's agent famously deleted a production database during a live demo (oopsies!). There are also issues with runaway costs, compromised tokens, and PII leakage.


The way we see it, there are three major challenges:


- **Agent i/o needs to be fully airgapped or at least tightly controlled.** There shouldn't be any surprise network calls or mysterious file access that hit the host system. Ideally, your observability should cover not just interactions with the inference engine but also agent's i/o with other networked systems, like internal APIs, databases, third-party services, etc.
- **All of this should work together seamlessly.** Every framework - LangChain, ADK, that thing that was hot on HN last week - reinvents its own version of credentials, tool registries, and tracing. We don't believe that credentials, egress, and budgets are library concerns. They're infra concerns because teams should have freedom to choose the framework that best fits their use case.
- **The runtime should be deployable in diverse environments.** This includes various clouds, on-prem, your home lab, or just that machine in the corner of the room.


Below you can read about our approach to each of those problems.


## gVisor as an isolation primitive


The first decision we had to make is how do we isolate the agent process itself. There are several technologies that people commonly reach for: Linux cgroup/namespace containers (Docker or Bubblewrap), or system virtualization like LXC or KVM are the typical ones. Some of the less common ones are Userspace kernels (e.g. gVisor or[UML](https://en.wikipedia.org/wiki/User_Mode_Linux) ), Unikernels, or WebAssembly. Let's look at each of these options in detail.


First, we've eliminated Unikernels and WebAssembly. In our view, these are still less mature, more technically risky options and the tooling is too cumbersome for users, like us, despite years of experience. Unikernels, in particular, require a virtualization layer to run, so it would be poor fit to ship as self-hostable option. WebAssembly, despite being fairly promising, still seems like a permanently "under construction" option to seriously consider for production use.


Using just standard Linux cgroup/namespace containers - runc or Bubblewrap types - would be a good fit for self-hosting requirement and is the most lightweight isolation option available. It also has great DX - just ship a Docker/OCI image and you're good to go, it can plug into existing container orchestration systems like Kubernetes or even some managed cloud offerings. It is, however, the least secure of all the options we considered and building a syscall interception layer using systrap/ebpf would move us close to the Userspace Kernel option anyway which we'll discuss below.


KVM or KVM-light (e.g. Firecracker and other microVMs) is a good, highly secure and performant option that has been hyped up quite a bit for agents specifically. Two reasons for not using it: first, for self-hosting in cloud you need nested virtualization support and access to` /dev/kvm` . Second, we wanted something that doesn't pay full guest OS init and comes up faster than the declared 125ms boot time (which seems to be a highly optimized best-case scenario).


That left us with gVisor as the middle-ground option - it's secure and tested - a lot of folks run totally untrusted workloads on it! It has a nice DX where a developer can provide an OCI image and gVisor's` runsc` takes care of the rest. It is trivial to run on random Linux VM on the cloud or on a Mac. It is easy to customize, and in particular its networking stack (called` netstack` ) has proven incredibly capable of adapting to custom solutions, which we've done a lot of. A perfect match!


Sandbox isolation · life of a write()


FIG. 01


### The same call, traced through two isolation models


Virtualization · Firecracker


A second kernel serves the call


1


trapped into the guest kernel


2


hardware VM exit


only if real device I/O is needed


3


forwards to the host


4


Physical hardware


Strong isolation: the host kernel is reached only across a hardware-enforced boundary. Firecracker keeps the microVM minimal, but it still boots a full guest kernel and pre-allocates its memory.


gVisor


A userspace kernel serves the call


1


Systrap · seccomp-bpf


traps the syscall - never passed through


2


filesystem op delegated to the Gofer


3


limited, filtered syscalls


a small, hardened surface


4


Physical hardware


The workload's` write()` never reaches the host kernel. A second kernel still serves it - but it runs in userspace, so resources flex at runtime and there's no guest to boot.


Click any numbered box to see what it does.


## Transparent networking call interception


Next, we had to solve the problem of observability. Our main focus was networking observability - this includes not only request/response tracing to/from LLM inference servers but also MCP tool calls and any other network calls made by the workload. For example, our customer assistant agent needed to talk to a postgres database to retrieve user data. If you can somehow intercept and observe all the network i/o made by the workload, you can not only improve reactive operations like debugging but also proactively enforce security and compliance policies.


Intercepting incoming connections such as an HTTP webhook arriving at an agent is largely a solved problem. A proxy like[Envoy](https://www.envoyproxy.io/) or[Nginx](https://www.nginx.com/) that natively supports OpenTelemetry can easily terminate HTTP/S calls and emit OpenTelemetry logs and spans for each request/response.


The more interesting case is when the workload has to make network calls to external services. In this case, you can use the same proxy to intercept and observe all the network i/o made by the workload but you have two challenges:


1. how do you direct the calls into the proxy without modifying the workload code/SDK?
2. how do you intercept a call the workload makes to an external service over a TLS-encrypted channel (HTTPS) which you obviously don't have a private key for?


gVisor allows you to plug into its` netstack` - a userspace Linux networking stack implemented in Go. We use this to transparently intercept the` connect()` syscall made by the workload and redirect it to the proxy. Easy. Problem 1 solved.


Network call · netstack intercept


FIG. 02


### Life of a` connect()` - served in userspace, handed to Envoy


In the sandbox · gVisor Sentry


1


Systrap · seccomp-bpf


traps the syscall - never reaches the host network


2


SYN routed out` eth0` · default route` 0.0.0.0/0`


3


matches no bound endpoint - falls to the catch-all handler


4


splice


leaves the Sentry


On the host · Envoy egress


5


Envoy's own` connect()` · ordinary host syscalls


6


Internet · real upstream


The workload's` connect()` never becomes a host socket. Our patched netstack loops every outbound packet back, the catch-all forwarder steals it, and the flow is bridged out as one spliced connection - handed to the external Envoy proxy.


Fails closed


Leave the forwarder uninstalled and the same SYN gets` RST` inside the stack. There is no default path off` eth0` - egress is impossible unless it goes through Envoy.


Click any numbered box to see what it does.


Now for the task of observing encrypted traffic destined for external services - we need to MITM each new connection in the (forward) proxy and then re-encrypt it on the downstream side. We do this by signing a "fake" certificate on the fly (on the first connection) using our self-signed CA certificate. That CA is injected into workload CA trust store on startup so everything looks like it's talking to the real external service. Boom. Problem 2 solved.


TLS · transparent interception


FIG. 03


### Envoy MITMs TLS on the fly, with a CA injected into the container trust store


TLS


Envoy presents a leaf for the SNI host, **signed on the fly by the injected CA** - the workload's TLS validates, no error


TLS


Envoy dials the real host over a fresh TLS session, **validating the genuine public chain**


Click the workload, its trust store, Envoy, or the upstream to see what each does.


These two tricks together allow us to parse LLM request/response bodies from external providers like OpenAI, Anthropic, etc. without any additional configuration in the agent. We also get to see calls to any other external services (for example, did you know that Claude Code makes calls to Datadog?), MCP calls or calls to internal services like our postgres database.


Since we have requests available in plain text, we can also modify them on the fly, for example, to add or remove headers, change the request body, or even redirect the request to a different URL. This is useful for model fallback when not every provider uses the same format and requests and responses may need to be re-written. Another use case is credential injection - the agent can never leak credentials it can't see. We can also redact or drop PII or PHI data and much more.


We built a tracing view where all of this can be inspected interactively, and you can also ship this data to your observability stack via CLRK's built-in OTLP exporter:


Tracing View · TaskAgent invocation


FIG. 04


### Every external call the agent made, captured as a span


Status


200 OK


Duration


10.0s


LLM tokens in / out


2.3k / 999


Spans


11 / 9 LLM


0


2s


4s


6s


8s


10.0s


Inbound HTTP


the trigger


LLM


AIProviderRoute


4 concurrent


MCP tools


MCPRoute


no calls this invocation


Network


HTTP/L4 Route


Inbound (200)


LLM call


MCP tool


Network


Error / 4xx-5xx


INBOUND


POST /


200


10.0s


Request


method


POST


url


https://claude.example.com/


An inbound HTTP request triggers this run: CLRK wraps the request as a[CloudEvent](https://github.com/cloudevents/spec/blob/v1.0.2/cloudevents/spec.md) and hands it to the Claude Code agent, and the inbound span bounds the whole invocation. From there, every call the agent makes is captured on its lane - the model completions to Anthropic through the AIProviderRoute, and the agent's own telemetry shipped out to Datadog on the Network (HTTP/L4) lane. Tap any span to inspect its captured request, status, and timing.


## Gateway API-coded configuration


We like APIs that are intuitive and easy to use, and we doubt we're alone in that. Most existing sandboxing solutions lead with an SDK, which is a reasonable choice - but we wanted to go the other way and make a declarative API the primary interface, with the SDK built on top.


We looked at a number of options along the way.[AI Gateway](https://aigateway.envoyproxy.io/) was a natural first stop since we already lean on Envoy heavily, but we ended up designing a simpler version suited to our own needs, borrowing a few good ideas from it. The config model itself is heavily inspired by[Gateway API](https://gateway-api.sigs.k8s.io/) , which we think is a solid standard for declarative configuration.


Egress API · resource topology


FIG. 05


### A transparent egress data plane, modeled on the Kubernetes Gateway API


Source · egressRefs


TaskAgent


Runs once to completion


egressRefs →


DaemonAgent


Long-running, resident


egressRefs →


Gateway


EgressGateway


egw


≈ Gateway


listeners\[\] · by protocol


HTTPS


TLS Terminate · MITM


L7


TLS


SNI passthrough


L4


TCP


Raw L4 · ORIGINAL_DST


L4


...


no route →


defaultPolicy


Routes


AIProviderRoute


≈ HTTPRoute


match` provider · model · endpoint`


Policies & filters


MCPRoute


≈ HTTPRoute


match` server · tool · method`


Policies & filters


EgressL4Route


≈ TCP/TLSRoute


match` CIDR · hostname · port`


Policies & filters


Upstream


AI Provider APIs


api.openai.com · api.anthropic.com
bedrock · vertex


MCP Servers


notion · github MCP
internal tool servers


Internet · L4


registries · databases
any dialed host:port


Gateway


EgressGateway


egw


Transparent egress proxy


Intercepts all outbound traffic from attached sandboxes and declares its interception capabilities as listeners - one per protocol layer. Traffic matching no route falls through to defaultPolicy.


Gateway API lineage


Mirrors **Gateway**


gwapiv1.AllowedRoutes


parentRef.sectionName


A Gateway by another name: listeners are addressed by routes through the standard parentRef.sectionName field, unchanged.


Spec


```text
kind  :    EgressGateway
spec  :
defaultPolicy  :    deny-all
listeners  :
-   name  :    https
protocol  :    HTTPS
tls  :    { mode: Terminate }
allowedRoutes  :
kinds  :    [AIProviderRoute, MCPRoute]
-   name  :    tcp
protocol  :    TCP
```


The design borrows Gateway API's split between the thing traffic flows through and the rules that shape it. An EgressGateway is the concrete instance - it's what an agent points at via` egressRefs` , and it owns the listeners that agent traffic actually passes through. Then the route objects (` AIProviderRoute` ,` MCPRoute` , and friends) attach to a gateway via` parentRefs` and define what happens to matched traffic - credential injection, token budgets, tool policy, deny rules. An agent just references a gateway; the policy hanging off it can evolve independently, owned by whoever manages egress rather than whoever wrote the agent. Lastly, policy objects define extra behavior applied to traffic matched by a route.


This structure allows us to configure model routing, fallbacks, and apply policies like token quotas, rate limits, credential injection and more in an intuitive way that allows for future enhancements. For those with limited Dwarf Fortress experience or just tired of looking at endless YAML printouts, we've also shipped a dashboard UI as a visual aid.


Egress console · gateway detail


FIG. 06


### The same config, navigable: listeners, routes, and the rules on them


llm-egress


Ready


EgressGateway


·


platform


·


47d


·


policy **deny-all**


Listeners


3


Routes attached


6 · 7 rules


Policies


10


Default


deny


Requests / sec


184


Listeners


3


Routes attached


4


Rules


2


Targets


No backendRef set - the request is sent to its original destination ( **api.openai.com** ).


Attached policies & filters


TokenBudget


CredentialInjection


Apparent path


https://api.openai.com:443


└─ AI


provider=openai


↳ TokenBudget: MaxTokensPerDay=20M


↳ CredentialInjection: openai-key → Header: Authorization


→ upstream api.openai.com


Click a row to drill into the next column


·


Listeners → Routes → Rules → Targets


The dashboard browses the gateway the way the API nests it - pick a **listener** , drill into a **route** , then a **rule** , and the section below resolves the **apparent path** the traffic takes: the listener it enters, the match it hits, the attached policies and filters as pills, and the upstream it is dialed to. This is the` llm-egress` gateway from the Console's own demo cluster.


## Boring orchestration technology


Lastly, we needed an orchestration system to run our workloads on. We've been using and operating Kubernetes clusters at various scales for nearly a decade now. Needless to say, we feel quite comfortable with it. Its declarative and highly pluggable API layer makes it easy for humans and machines alike to reason about and interact with it. After 11 years, we think Kubernetes has finally become a "boring technology" and the current SOTA and even open models have gotten very good at squashing the "complexity" complaint that we've all heard.


We've designed CLRK to run as an extension using[Kubernetes API Aggregation](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/apiserver-aggregation/) , not a CRD so, in truth, it doesn't actually require a Kubernetes cluster and can function standalone. This will require replacing the Worker controller with something that can fulfill the same role. We'll talk about this in more detail in the next post.


## Putting it all together


We're still early on this quest, but we think we have a good foundation to build upon and something to share:[https://github.com/apoxy-dev/clrk](https://github.com/apoxy-dev/clrk) . We've put together[docs](https://apoxy.dev/docs/clrk/getting-started) to help you get started.


The project is licensed under AGPLv3 (client libraries are Apache 2.0 so you can actually link them from your codebase) and ready for you to try it. Give it a spin and let us know what you think!


Dmitry Ilyevsky


Co-founder & CTO


Dmitry is co-founder and CTO of Apoxy. He previously built and operated infrastructure at Google, Cruise, and Mux (where he and Matt met).


[GitHub ↗](https://github.com/dilyevsky)


Matt Ward


Co-founder & CEO


Matt is co-founder and CEO of Apoxy. He previously scaled infrastructure for YouTube and Mux. Outside of work, you can find him skiing down or pedaling up a mountain if he's not at home trying to be the kind of husband and father his family deserves.


[GitHub ↗](https://github.com/therealwardo)[X ↗](https://x.com/therealwardo)


[← Back to all posts](https://apoxy.dev/blog)
