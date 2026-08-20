---
schema_version: "1.0.0"
document_id: "4efe66bf4f80de0d854ebd2afc9980423b249469cca7b27d2812a2106ccc6e38"
company_key: "yc-blueshoe"
company: "Blueshoe"
source_id: "yc-blueshoe-news-import-28975b8749e8"
canonical_url: "https://www.blueshoe.io/blog/gefyra-hands-on/"
published_at: "2026-04-09T10:00:00+00:00"
first_seen_at: "2026-07-24T11:21:12.712803+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:277bc5b90bdc730952108a9db43c2885115cfef876a606cde84c4bb48cac5f0c"
---

# Act, Don't Isolate: The Blueshoe Approach to Kubernetes Development

## Table of Contents


- Why "Just Run It Locally" Doesn't Work for Us
- Our Setup: One Cluster, Many Developers
- The Architecture
- How We Actually Work: A Real Scenario
- Traffic Matching Options
- Why WireGuard
- How We Handle Shared State
- Team Setup
- Lessons We've Learned
- When We Don't Use This
- From Isolation to Participation
- Quick Reference


---


## Meet the Author


[Carl-Christian Neundorf](https://www.blueshoe.io/team/carl-christian-neundorf/) The foundation of our workflow is a shared development cluster. Not a cluster per developer. Not a cluster per feature branch. One cluster holding the canonical state of the application.


Last updated:


2026-04-09


---


Kubernetes


Gefyra


Development


---


2026-04-09


# How we use Gefyra in our daily lives


Act, Don't Isolate: The Blueshoe Approach to Kubernetes Development


Develop locally, test in the cluster - without CI/CD wait times. Gefyra promises the holy grail of Kubernetes development: full-speed coding with real cluster context. In our case study, you’ll learn why this tool is a game-changer for your daily operations and how to efficiently integrate it into your workflow


*Γέφυρα (Gefyra) - ancient Greek for "bridge." Ortega y Gasset wrote "Yo soy yo y mi circunstancia" - I am I and my circumstance. Code, like identity, doesn't exist in isolation. It emerges from its environment. This is how we stopped simulating and started participating.*


---


## Why "Just Run It Locally" Doesn't Work for Us


There's a promise embedded in containerization: "Build once, run anywhere." Docker was supposed to end the "works on my machine" era. For simple applications, it did. But somewhere between a single container and the systems we actually build for clients, the promise quietly broke.


A typical client project at Blueshoe involves Django, Celery with multiple worker types, PostgreSQL, Redis, sometimes Elasticsearch, an Nginx reverse proxy, and at least one external service integration — a payment provider, an ERP system, a shipping API — that only accepts traffic from whitelisted IP ranges.


Here's what "running this locally" actually looks like:


```text
Our Local Development Reality (Before):
┌───────────────────────────────────────────────────────────────┐
│  Developer's MacBook (16GB RAM)                               │
│                                                               │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐  │
│  │ Django  │ │ Celery  │ │ Celery  │ │ Celery  │ │  Nginx  │  │
│  │   App   │ │ Default │ │  Email  │ │  Heavy  │ │  Proxy  │  │
│  │  800MB  │ │  400MB  │ │  400MB  │ │  600MB  │ │  100MB  │  │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘  │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐              │
│  │Postgres │ │  Redis  │ │ Elastic │ │LocalSt. │    ...       │
│  │   1GB   │ │  200MB  │ │   2GB   │ │  Mock   │              │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘              │
│                                                               │
│  Total: ~6-8GB RAM, fans spinning, laptop as space heater     │
│                                                               │
│  And still:                                                   │
│  ✗ Payment API rejects requests (IP not allowlisted)          │
│  ✗ No service mesh (Istio behaviors invisible)                │
│  ✗ Network policies don't exist                               │
│  ✗ Secrets are stale .env files                               │
│  ✗ Database has synthetic data, not real edge cases           │
└───────────────────────────────────────────────────────────────┘


```


The isolation that makes Docker useful becomes the problem. Our local environments are hermetically sealed from reality. The database doesn't have the data that triggers the bug. The mocked payment provider doesn't timeout the way the real one does. The single-node setup doesn't exhibit the network partitions we see in production.


We were developing against a fiction. A useful fiction, sometimes — but a fiction nonetheless.


### The Workload Problem


Our client projects aren't simple CRUD apps. They involve:


- **Multiple worker types** — Celery queues with different concurrency settings, different resource limits, different behaviors under load. The default queue handles quick tasks; the heavy queue processes large file uploads with different memory constraints. Locally, they all run the same way.
- **Background jobs** — scheduled tasks that touch the same data our request handlers do. A nightly aggregation job that only breaks when it encounters six months of accumulated data. A webhook processor that only fails when three arrive simultaneously.
- **Service mesh sidecars** — Istio injecting mTLS, retries, circuit breakers, traffic splitting. Behavior we can't see or replicate locally because there's no mesh to inject into.
- **Init containers** — migrations, cache warming, dependency checks that run before the app starts. Locally, we skip these or run them manually. In the cluster, they're part of the startup contract.
- **Network policies** — traffic restrictions that simply don't exist on a laptop. The backend can only talk to the database, not directly to Redis. The worker can reach the external API, but the web pod can't. These constraints shape how we architect, but they're invisible in local development.
- **Secrets management** — Vault integrations, external secrets operators, sealed secrets. Our local Docker Compose knows nothing about these. We copy values into` .env` files and hope they're still valid.


Each of these is a circumstance our code depends on. Each is absent from a local setup. Each is a source of bugs that only manifest "in the real environment" — which means they manifest in front of clients, or worse, in production.


We've all had that moment: the code works perfectly on your machine, passes in CI, deploys successfully — and then fails in staging because of something you couldn't possibly have tested locally. A timeout. A network policy. A race condition that only appears under realistic load. The circumstances were wrong, so the code was wrong, even though it looked right.


### The Feedback Loop Problem


The alternative we tried: CI-driven ephemeral environments. Push code, wait for the pipeline, get a fresh environment spun up, test there.


It works. It's correct. The environment matches reality because it *is* reality — a fresh deployment of the whole stack.


It's also slow.


A fifteen-minute feedback loop for a one-line change. Wait for the pipeline to trigger. Wait for images to build. Wait for the environment to provision. Wait for pods to become ready. Wait for health checks to pass. Then, finally, test your change.


Multiply that by the dozens of small iterations real debugging requires. "Is it this line? No. This one? No. What if I log this variable? Still not clear. Let me try..."


That's not development. We were corresponding — sending letters and waiting for replies. The conversation between developer and code, which should be immediate and fluid, became stilted and slow.


We tried optimizations. Faster CI runners. Cached base images. Parallel jobs. They helped at the margins. But the fundamental problem remained: we were pushing code to a remote place and waiting for it to tell us what happened. The gap between writing and knowing was still too wide.


We needed something else. Not a better simulation. Not a faster pipeline. A bridge between where we write code and where it actually runs.


So we built one.


---


## Our Setup: One Cluster, Many Developers


The foundation of our workflow is a shared development cluster. Not a cluster per developer. Not a cluster per feature branch. One cluster holding the canonical state of the application.


This is a philosophical choice as much as a practical one. Instead of every developer carrying a miniature fiction on their laptop, we maintain one shared reality.


The traditional model assumes isolation is a feature: your environment is yours, sealed off, unable to interfere with anyone else's. But that isolation is also a lie. Your local Postgres doesn't have real data. Your mocked Stripe doesn't return realistic errors. Your Docker network doesn't have the latency of a real VPC. You're protected from interference, but you're also protected from truth.


Our cluster runs the truth. The same container images that go to staging. The same network policies. The same service mesh. When one of our engineers connects, they're not simulating a production-like environment — they're participating in one.


### How We Organize It


Each client project gets its own namespace (or set of namespaces for larger systems). Our CI/CD pipeline keeps these populated with the latest stable build from the main branch — this is the "base state," the version of the application that works.


When one of our engineers starts working on a feature, they don't create a new isolated world. They connect to the existing one. The cluster is the shared context. Gefyra is the mechanism that lets multiple developers operate within that context without stepping on each other — thanks to personal bridges and shadow workloads.


```text
Our Namespace Strategy:
│                  Kubernetes Development Cluster               │
│                                                               │
│  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐  │
│  │  client-a-dev   │ │  client-b-dev   │ │  internal-dev   │  │
│  │                 │ │                 │ │                 │  │
│  │ • Django app    │ │ • FastAPI       │ │ • Our tools     │  │
│  │ • Celery workers│ │ • PostgreSQL    │ │ • Experiments   │  │
│  │ • Redis         │ │ • Redis         │ │                 │  │
│  │ • PostgreSQL    │ │ • External APIs │ │                 │  │
│  │ Alice & Bob     │ │ Carol bridges   │ │                 │  │
│  │ bridge here     │ │ API here        │ │                 │  │
│  │ simultaneously  │ │                 │ │                 │  │
│  └─────────────────┘ └─────────────────┘ └─────────────────┘  │
│                                                               │
│  CI/CD maintains base state │ Developers connect to work      │


```


The key insight: the cluster isn't the destination for our code. It's the starting point for our development. We work *inside* the reality, not toward it. The code travels to us, in a sense — the environment is already there, waiting, and we bridge into it rather than trying to recreate it.


This changes how we think about development. We're not building something locally and hoping it works remotely. We're working remotely with local tools. The distinction matters.


---


## The Architecture


Gefyra is the bridge that makes this connection possible:


```text
KUBERNETES CLUSTER
┌────────────────────────────────────────────────────────────────────────────┐
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │                     client-project-namespace                         │  │
│  │                                                                      │  │
│  │  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐                 │  │
│  │  │   backend   │   │   celery    │   │    redis    │                 │  │
│  │  │     Pod     │   │   worker    │   │             │      ...        │  │
│  │  │             │   │             │   │             │                 │  │
│  │  │ ┌─────────┐ │   └─────────────┘   └─────────────┘                 │  │
│  │  │ │CARRIER2 │ │ ◄── Rust proxy, routes traffic by matching rules    │  │
│  │  │ └────┬────┘ │                                                     │  │
│  │  └──────┼──────┘                                                     │  │
│  │         │                                                            │  │
│  │  ┌──────┴──────┐                                                     │  │
│  │  │   SHADOW    │ ◄── Duplicate workload, handles unmatched traffic   │  │
│  │  │  WORKLOAD   │     (original image, cluster upstream)              │  │
│  │  └─────────────┘                                                     │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│  ┌──────────────────┐                                                      │
│  │     STOWAWAY     │ ◄── WireGuard server, manages developer connections  │
│  │   (Deployment)   │     Exposed via NodePort or LoadBalancer             │
│  └────────┬─────────┘                                                      │
│           │                                                                │
└───────────┼────────────────────────────────────────────────────────────────┘
│
│  ════════════════════════════════════════════
│   WireGuard Tunnel
│   • UDP, Curve25519 encryption
│   • Single round-trip handshake
│   • Native roaming (survives network changes)
│
┌───────────┼────────────────────────────────────────────────────────────────┐
│  ┌────────┴─────────┐                                                      │
│  │      CARGO       │ ◄── WireGuard client + DNS resolver                  │
│  │ (Docker container)│     Makes cluster services reachable locally        │
│           ▼                                                                │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                      LOCAL CONTAINER                                │   │
│  │                                                                     │   │
│  │   Our code running locally, but:                                    │   │
│  │   • Resolves cluster DNS (postgres.namespace.svc.cluster.local)     │   │
│  │   • Connects to real databases, caches, queues                      │   │
│  │   • Outbound traffic exits via cluster NAT (IP allowlists work)     │   │
│  │   • Has real env vars copied from running pods                      │   │
│  │   • Source mounted for hot-reload                                   │   │
│  │   • Debugger attached via IDE                                       │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│  DEVELOPER'S MACHINE                                                       │
└────────────────────────────────────────────────────────────────────────────┘


```


Five components work together:


**Stowaway** lives in the cluster as a Deployment, typically exposed via a LoadBalancer or NodePort. It's the WireGuard server that accepts developer connections, manages cryptographic identities, and routes traffic. When you connect, it creates a` GefyraClient` custom resource — your unique identity with your own subnet. Multiple developers can connect simultaneously; each gets their own cryptographic keypair and IP range, so traffic stays isolated even though everyone shares the same tunnel endpoint.


**Cargo** runs on your machine as a Docker container. It maintains the WireGuard tunnel and, critically, acts as a DNS resolver for cluster service names. This is why we can` curl postgres.client-namespace.svc.cluster.local` from our terminals after connecting — Cargo intercepts DNS queries and resolves them through the cluster's CoreDNS. It also handles the routing table magic that makes cluster IPs reachable from your local network stack.


**Carrier2** is the Rust-based reverse proxy built on Pingora (Cloudflare's proxy framework). When you create a bridge mount, the operator patches the target pod, injecting Carrier2 alongside your application. Carrier2 handles traffic routing based on matching rules — headers, paths, or combinations — directing requests either to your local machine or to the shadow workload. It supports multiple ports simultaneously and can be reconfigured gracefully via SIGHUP without dropping connections.


**Shadow Workloads** solve the "what happens to traffic I don't want" problem. When you create a` GefyraBridgeMount` , Gefyra duplicates the target workload (e.g.,` deployment/backend` becomes` deployment/backend-gefyra` ). This shadow runs the original image and serves as the "cluster upstream" — any traffic that doesn't match your personal routing rules goes there. Nothing gets lost. Other developers, automated tests, and monitoring systems continue to work against the shadow while you intercept only the requests you care about.


**The Operator** orchestrates everything. It's a standard Kubernetes operator that watches for Gefyra custom resources (` GefyraClient` ,` GefyraBridgeMount` ,` GefyraBridge` ) and reconciles the cluster state accordingly. It handles the lifecycle: installing Stowaway, creating client identities, managing mount state machines (` REQUESTED → PREPARING → INSTALLING → ACTIVE → RESTORING → TERMINATED` ), patching pods to inject Carrier2, creating shadow workloads, and cleaning up when developers disconnect. Validating webhooks ensure consistency — only one mount per workload, bridges must reference active mounts, and immutable fields stay immutable.


The result: our laptops become part of the cluster network. Our code runs locally but exists within the cluster's circumstances. We get the fast iteration of local development with the fidelity of the real environment.


---


## How We Actually Work: A Real Scenario


Let's walk through a scenario from last month in detail.


Alice is one of our backend engineers, working on a payment integration for a client. The payment provider is a European fintech with strict security requirements — they only accept API requests from a whitelist of IP addresses, and our cluster's NAT gateway is on that list. Our office IP is not. Alice's home IP is definitely not.


Bob is building the frontend checkout flow that consumes Alice's endpoint. They're both working in the same namespace, on the same feature, with a demo scheduled for Friday.


### Step 1: Establish the Connection


Alice opens her terminal. She's done this hundreds of times:


```text
gefyra   connection   create   -f   alice-client.yaml


```


For teams, the operator is pre-installed and administrators distribute client configuration files to developers. The` gefyra up` command exists as a convenience shortcut that installs the operator, creates a client, and connects — perfect for individual developers or quick experiments. But in team setups,` connect` is what developers use daily.


Under the hood, several things happen:


1. Cargo starts locally, receives the peer configuration, and establishes the WireGuard tunnel.
2. Stowaway accepts the connection and activates Alice's` GefyraClient` resource.
3. Traffic routing is established between Alice's machine and the cluster network.


Within about five seconds, Alice sees:


```text
Gefyra client connected.
You can now run containers that have access to the Kubernetes cluster.


```


Her laptop is now network-reachable inside the cluster. She can verify this immediately:


```text
# Cluster DNS resolution works from her terminal
$   nslookup   redis.client-project.svc.cluster.local
Server:      172.17.0.2
Address:     172.17.0.2#53


Name:        redis.client-project.svc.cluster.local
Address:     10.100.47.93


```


No containers running yet. No code deployed. Just connectivity. Her machine is now, from a networking perspective, inside the cluster's VPC.


### Step 2: Run Locally with Cluster Context


Now Alice starts her development container:


```text
gefyra   run   \
-i   blueshoe/client-backend:dev   \
-N   alice-backend   \
-n   client-project   \
--env-from   deployment/backend/api   \
-v   $(  pwd  )  /src:/app/src   \
-p   8000:8000


```


Let's break down what each flag does:


Flag What It Does


` -i blueshoe/client-backend:dev` Uses our development image, which includes debug tools, ipdb, and dev dependencies not present in the production image


` -N alice-backend` Names the container` alice-backend` so we can reference it later when bridging


` -n client-project` Targets the` client-project` namespace in the cluster


` --env-from deployment/backend/api` Copies all environment variables from the running` api` container in the backend deployment


` -v $(pwd)/src:/app/src` Mounts her local source code into the container for hot-reload


` -p 8000:8000` Exposes port 8000 so she can attach her debugger


The` --env-from` flag is essential to our workflow. It reaches into the cluster, finds the specified deployment and container, extracts all its environment variables, and injects them into the local container. This includes:


- Database connection strings (pointing to the cluster's Postgres)
- Redis URLs (pointing to the cluster's Redis)
- API keys and secrets (the real ones, from Vault or wherever they're sourced)
- Feature flags (the current configuration, not a stale copy)
- The payment provider credentials (which Alice needs for her integration)


Not a` .env` file from three weeks ago that someone forgot to update. Not a` docker-compose.override.yml` with hardcoded values. The actual, current, live configuration that the running system uses.


Alice's container starts. Django's development server spins up with hot-reload enabled. But unlike normal local development, when her code tries to connect to` postgres.client-project.svc.cluster.local` , the connection succeeds. The DNS resolves through Cargo to the cluster's CoreDNS, and the TCP connection routes through the WireGuard tunnel to the actual PostgreSQL pod.


When her code enqueues a Celery task, it goes to the real Redis broker. When the task executes, it runs on the real Celery workers in the cluster.


And crucially — when her code calls the payment provider's API, the HTTP request exits through the cluster's NAT gateway. The IP is on the allowlist. The integration actually works.


**For about 80% of our development work, this is all we need.** Connect, run, and work directly against the cluster. No bridge required. Alice can hit her local endpoint directly (` curl localhost:8000/api/payments/` ), run management commands against real data, debug with full IDE integration, and see changes instantly via hot-reload.


### Step 3: Bridge the Traffic with Personal Routing


But Alice and Bob need to test the integration end-to-end. Bob's frontend needs to call Alice's backend — and Alice's changes aren't deployed yet. This is where personal bridges come in.


First, Alice creates a bridge mount to prepare the infrastructure:


```text
gefyra   mount   create   \
--target   deployment/backend/api   \
--namespace   client-project   \
--name   backend-mount


```


This triggers a state machine:


```text
REQUESTED → PREPARING → INSTALLING → ACTIVE


```


Behind the scenes:


1. Gefyra creates a shadow workload (` deployment/backend-gefyra` ) running the original image
2. The original pod gets patched with Carrier2 injected
3. Carrier2 starts but routes all traffic to the shadow (no bridges yet)


The mount is now` ACTIVE` . Traffic continues flowing normally — everything goes to the shadow, which behaves exactly like the original deployment.


Now Alice creates her personal bridge with traffic matching rules:


```text
gefyra   bridge   create   \
--mount   backend-mount   \
--local   alice-backend   \
--ports   8000:8000   \
--match-header-exact   x-gefyra:alice


```


This is the moment of substitution — but only for Alice's traffic. Carrier2 reconfigures (gracefully, via SIGHUP):


- Requests with header` x-gefyra: alice` → Alice's laptop via the tunnel
- All other requests → Shadow workload (cluster upstream)


From the perspective of every other component in the cluster, nothing has changed. The` backend` service still resolves to the same pod IP. Requests to port 8000 still get responses. The ingress controller still routes traffic the same way. Celery workers still call the backend API the same way.


But now, requests with Alice's header travel through the tunnel to her laptop. Her local Django process handles them. Her breakpoints trigger. Her hot-reload works. Her log statements appear in her terminal.


Bob configures his frontend dev server to add the header:


```text
// In Bob's frontend dev config
const   headers   =   {   'x-gefyra'  :   'alice'   };


```


The bridge is complete — and it's personal to Alice.


### Step 4: The Moment It Pays Off


It's Wednesday afternoon. Bob has been building the checkout UI. He's integrated Alice's endpoint, the form looks good, and he's testing the flow.


He fills in test card details. Clicks "Pay." The page shows an error: "422 Unprocessable Entity."


In our old workflow, this would unfold predictably:


1. Bob screenshots the error, posts it in Slack
2. "Hey Alice, getting a 422 on checkout"
3. Alice: "What request are you sending?"
4. Bob opens DevTools, copies the request payload, pastes it
5. Alice tries to reproduce with curl. It works for her.
6. "Can you check the exact headers?"
7. Twenty minutes of back-and-forth
8. Eventually: "Oh, you're sending the payment method as a string, not an object"
9. Debate about whose code is wrong
10. Another twenty minutes


Here's what actually happened:


Bob sends Alice the URL to the dev environment in Slack:` https://dev.client-project.blueshoe.io/checkout` . She opens it in her browser. She's looking at the same page Bob is looking at.


She opens PyCharm, finds the payment endpoint handler, and sets a breakpoint on the first line.


"Okay, refresh the page and submit again."


Bob refreshes. Fills in the form. Clicks "Pay."


The request flows:


- From Bob's browser to the ingress
- From the ingress to the backend service
- From the backend service to the pod — which is running Carrier2
- Carrier2 checks: header` x-gefyra: alice` ? Yes.
- Through the WireGuard tunnel
- Through Cargo on Alice's laptop
- Into her local Django container


Her debugger pauses. She's looking at the actual request Bob just sent. His exact payload. His exact headers. His exact session state.


She steps through the code. The payment method comes in as` "credit_card"` — a string. Her code expects` {"type": "credit_card", "provider": "stripe"}` — an object. The validation fails. 422.


She fixes the handler to accept both formats for backwards compatibility. Saves the file. Django's hot-reload picks it up. The server restarts in under a second.


"Try again."


Bob refreshes. Fills in the form. Clicks "Pay." Success.


Total time from "I'm getting an error" to "It's fixed": four minutes. Same environment. Same data. Same circumstances. The bug existed in shared reality, so we debugged it in shared reality.


Meanwhile, Carol — another backend engineer — is also working on the same service. She has her own bridge:


```text
gefyra   bridge   create   \
--mount   backend-mount   \
--local   carol-backend   \
--ports   8000:8000   \
--match-header-exact   x-gefyra:carol


```


Now Carrier2 routes:


- ` x-gefyra: alice` → Alice's laptop
- ` x-gefyra: carol` → Carol's laptop
- No matching header → Shadow workload


Three versions of the backend running simultaneously against the same cluster state. The QA team's automated tests (no special header) hit the shadow workload. Everyone works simultaneously, no interference.


We call this "collaborative debugging instead of archaeological debugging." We're not excavating artifacts from a distant environment, trying to reconstruct what happened. We're both standing in the same place, looking at the same thing, in real time.


---


## Traffic Matching Options


The matching rules are flexible and can be combined:


```text
# Path-based routing
--match-path-exact   /api/payments
--match-path-prefix   /api/v2/
--match-path-regex   ^/admin/.  *


# Header-based routing
--match-header-exact   x-gefyra:alice
--match-header-prefix   x-team:backend
--match-header-regex   x-user:.  *  test.  *


# Combinations (AND within a rule, OR across rules)
--match-header-exact   x-gefyra:alice   --match-path-prefix   /api/payments


```


For multi-port services, Carrier2 handles multiple port mappings:


```text
gefyra   bridge   create   \
--mount   my-mount   \
--local   my-container   \
--ports   8080:3000   \
--ports   9000:5000   \
--match-header-exact   x-gefyra:alice


```


---


## Why WireGuard


The choice of tunneling protocol might seem like an implementation detail, but it shapes the entire developer experience.


We evaluated several options early on: SSH tunnels, OpenVPN, IPSec, and WireGuard. The requirements were clear: fast connection establishment, reliable reconnection after network changes (laptops sleep, WiFi switches, VPNs toggle), and low overhead for the kinds of latency-sensitive traffic developers generate.


```text
┌──────────────┬──────────────┬─────────────────┬──────────────┐
│              │ OpenVPN      │ IPSec           │ WireGuard    │
├──────────────┼──────────────┼─────────────────┼──────────────┤
│ Codebase     │ ~100k LOC    │ ~400k LOC       │ ~4k LOC      │
│ Handshake    │ Multi-round  │ IKE (complex)   │ 1-RTT        │
│ Roaming      │ Reconnect    │ Complex         │ Native       │
│ Crypto       │ Configurable │ Configurable    │ Curve25519   │
│ Kernel       │ Userspace    │ Varies          │ In-kernel    │
└──────────────┴──────────────┴─────────────────┴──────────────┘


```


WireGuard won decisively. It's roughly 4,000 lines of code — small enough to audit, simple enough to reason about. It lives in the Linux kernel (and has mature userspace implementations for macOS and Windows). Connections establish in a single round-trip because there's no negotiation: the cryptographic parameters are fixed (Curve25519 for key exchange, ChaCha20-Poly1305 for encryption).


Most importantly for our use case, WireGuard handles roaming natively. The protocol is stateless from the perspective of connection management — if your IP address changes, the next packet just comes from the new address, and the peer accepts it (assuming the cryptographic authentication still validates). There's no session to re-establish, no handshake to repeat.


The practical effect:` gefyra connect` takes seconds. Our engineers close their laptops for a meeting, open them an hour later, and the connection is just... there. They switch from office WiFi to phone hotspot mid-debug-session and nothing breaks. They work from home, from coffee shops, from trains, and the tunnel survives.


We've been burned by "correct but slow" solutions before. Tools that technically work but impose just enough friction that developers route around them. Speed in developer tooling isn't a luxury — it's the difference between a tool that becomes part of the workflow and a tool that gets abandoned within a month.


A bridge only matters if people cross it daily. WireGuard makes sure they do.


---


## How We Handle Shared State


The obvious question: if everyone shares a cluster, what about the database? What about conflicting changes? What about one developer's broken code affecting everyone else?


Fair questions. Shared state is the hard part. Here's how we've learned to manage it:


### Migrations


Database migrations run through our CI pipeline before hitting the shared namespace. The base state is always migration-current — when CI deploys a new version to the dev namespace, it runs migrations first. This means the schema is always consistent with the deployed code.


When someone's working on a feature that requires schema changes, they have two options:


1. **For additive changes** (new columns with defaults, new tables): they can test locally by running the migration against the shared database. Gefyra's volume mounting lets you run` python manage.py migrate` locally while connected to the cluster — you're modifying the real schema, with real data.
2. **For breaking changes** (column removals, type changes): they use a branch-specific database or a dedicated namespace. We spin these up on demand for complex features. The key is that schema-breaking work doesn't touch the shared environment until it's ready to merge.


In practice, most day-to-day development involves no migrations at all, so this rarely comes up. But when it does, the tooling supports it.


### Test Data Isolation


We use conventions, not tooling. All test records created during development are prefixed:` alice-test-*` ,` bob-test-*` . This is low-tech, but it works remarkably well.


The benefits are simple:


- Everyone can see what test data exists
- Nobody accidentally modifies production-like records
- Cleanup is easy:` DELETE FROM orders WHERE id LIKE 'alice-test-%'`
- Debugging is easier: you know which records are real and which are test


We tried more sophisticated approaches — automatic test data isolation, shadow tables, per-developer schema prefixes. They added complexity without adding much value. The convention works because everyone understands it and follows it.


### Destructive Operations


Some development tasks require destructive operations: truncating tables, resetting sequences, bulk-deleting records, running data backfills. These don't happen in the shared namespace.


If someone needs to` TRUNCATE TABLE users` to test their import script, they do it in their own sandbox namespace. We keep a few of these around, and developers can claim one when they need it. The shared namespace is for normal development — code changes, debugging, feature work. It's not a playground for experiments that might affect other people.


### What About Bridge Conflicts?


With personal bridges, this is largely solved. Multiple developers can mount the same service — each with their own` --match-header-exact x-gefyra:` . Carrier2 routes correctly based on the matching rules. Traffic without a matching header goes to the shadow workload. No coordination needed beyond choosing unique identifiers.


Gefyra also shows which mounts and bridges are active:


```text
gefyra   mount   list
gefyra   bridge   list


```


And if something goes wrong, the symptoms are obvious immediately — requests go to the wrong place and fail. It's self-correcting because you notice and fix it.


---


## Team Setup


For individual developers experimenting,` gefyra up` handles everything — it installs the operator, creates a client, and connects in one command. For teams, you want more control:


**Operator Installation** (once, by admin):


```text
gefyra   install


```


**Client Creation** (by admin, per developer):


```text
gefyra   client   create   --name   alice
gefyra   client   config   alice   >   alice-client.yaml
gefyra   client   create   --name   bob
gefyra   client   config   bob   >   bob-client.yaml
# Distribute these files securely to developers


```


**Daily Developer Workflow** :


```text
# Connect using provided client file
gefyra   connection   create   -f   alice-client.yaml


# Run local container with cluster connectivity (80% of work stops here)
gefyra   run   -i   myimage   -N   mycontainer   -n   mynamespace   \
--env-from   deployment/app/main   \
-v   $(  pwd  )  /src:/app/src


# If needed: create mount and bridge with personal routing
gefyra   mount   create   --target   deployment/app/main   -n   mynamespace   --name   my-mount
gefyra   bridge   create   --mount   my-mount   --local   mycontainer   --ports   8000:8000   \
--match-header-exact   x-gefyra:alice


```


**Lifecycle Management** :


```text
# See what's active
gefyra   list
gefyra   mount   list
gefyra   bridge   list


# Clean up
gefyra   bridge   delete   my-bridge
gefyra   mount   delete   my-mount    # State: ACTIVE → RESTORING → TERMINATED
gefyra   connection   disconnect


```


---


## Lessons We've Learned


After two years of using this workflow across dozens of client projects, some patterns have emerged.


### Start Simple


When we first built Gefyra, we imagined elaborate use cases: per-request routing based on headers, multiple simultaneous bridges, automated environment provisioning. We built those features. But here's the thing: **about 80% of our development needs only` gefyra run`** — connect and run a local container with cluster connectivity. No bridge required.


The bridge with personal routing rules handles the remaining 20% — when we need to intercept traffic from frontends, webhooks, or other services. Start with` run` . Add mounts and bridges only when you actually need to intercept traffic that originates elsewhere.


### The Cluster Is Infrastructure, Not Magic


Running a shared development cluster requires the same care as any other infrastructure. Someone needs to monitor it, update it, manage its costs, handle its incidents. We have alerting when the dev cluster is unhealthy, and we treat dev cluster outages with appropriate (though not production-level) urgency.


If your dev cluster is down, your developers can't work. That's a real cost. Budget accordingly.


### Not Everything Needs the Bridge


The most common mistake we see (and made ourselves early on): reaching for Gefyra when a simpler tool would suffice.


If you're changing CSS, use your frontend dev server's hot module replacement. It's faster than any bridge.


If you're writing a pure function with no external dependencies, write a unit test. Run it locally. You don't need a cluster for that.


If you're fixing a typo in a template, just fix it and push. The CI pipeline will catch actual problems.


The bridge is for when the circumstances matter — when the bug lives in the interaction between your code and its environment. Use it for those cases. For everything else, use simpler tools.


### The Feedback Loop Is Everything


The most important metric for developer tooling is: how long between making a change and knowing if it works?


For local-only development, this is instant: save file, see result.


For CI-driven development, this is minutes to tens of minutes: push, wait, check.


For Gefyra-based development, this is seconds: save file, hot-reload picks it up, bridged request uses new code.


We optimize ruthlessly for this loop. Every second of latency is a second of broken focus. Every minute of waiting is a minute of context lost. The bridge matters because it keeps the feedback loop tight while expanding the scope of what you can test.


---


## When We Don't Use This


We believe in being honest about tools, especially ones we build.


Scenario Use Gefyra? Why


CSS tweaks No Local dev server + HMR is faster


Pure functions No Unit test locally


Template typos No Just push, CI will catch it


New endpoint (can call directly)` run` only No traffic to intercept


Celery task debugging` run` only Enqueue tasks directly


External API integration` run` only Need cluster NAT, not interception


Frontend-backend integration Full bridge Need to intercept frontend requests


Webhook debugging Full bridge Need to intercept external callbacks


Service mesh debugging Full bridge Can't simulate Istio locally


Data-dependent bugs Full bridge Need to see the whole system


Multi-developer on same service Personal bridges Each dev gets their own routing


The heuristic we use: **if you can call it directly, just use` run` . If traffic originates elsewhere, create a bridge with your personal routing rules.**


---


## From Isolation to Participation


Ortega's insight was that identity doesn't exist in isolation. "I am I and my circumstance" — the self emerges from engagement with the world, not separation from it. You are not some abstract essence that happens to be surrounded by context. You are constituted by that context, shaped by it, inseparable from it.


We've found the same holds for software. Code doesn't exist in the abstract. It exists in conversation with databases, networks, services, configurations, constraints. A function that works perfectly in isolation might fail catastrophically when composed with other functions. An algorithm that's elegant in theory might be disastrous in practice. The circumstances aren't incidental to the code — they're essential to it.


The history of development environments is a history of trying to escape this truth — simulating circumstances instead of engaging with them. We mock databases so we don't need real ones. We stub APIs so we don't need network access. We approximate networks with localhost so we don't need infrastructure. Each simulation trades fidelity for speed, truth for convenience.


For a long time, that trade-off made sense. Real environments were slow to provision, expensive to maintain, and complex to manage. Simulation was the pragmatic choice.


Kubernetes changed the equation. Not by making things simpler — it emphatically did not — but by making infrastructure programmable. If your production environment is defined in code, managed by operators, and reproducible from version control, then your development environment can participate in that code rather than simulating it from the outside.


Gefyra — γέφυρα, the bridge — collapses the distance between simulation and participation. The network is real. The DNS is real. The services are real. The circumstances are intact. Only the code is local, changeable at the speed of thought.


With personal bridges and shadow workloads, multiple developers participate simultaneously. Each intercepts their own traffic. The shadow handles everything else. No one steps on anyone else's work.


The best development environment isn't the one that most accurately simulates production. It's the one that *is* production — with personal routing rules and a fast undo button.


After two years of crossing this bridge daily across dozens of client projects, we can't imagine going back. The fictions we used to develop against — the mocked services, the synthetic data, the approximate networks — feel like trying to learn swimming on dry land.


Act, don't isolate.


---


## Quick Reference


```text
# === Individual Developer (Quick Start) ===
gefyra   up                                      # Install operator + connect


# === Team Workflow ===
# Admin: Install operator once
gefyra   install


# Admin: Create client configs
gefyra   client   create   --name   alice
gefyra   client   config   alice   >   alice-client.yaml


# Developer: Connect
gefyra   connection   create   -f   alice-client.yaml


# === Run Local Container (80% of work) ===
gefyra   run   \
-i   <  imag  e  >   \
-N   <  container-nam  e  >   \
-n   <  namespac  e  >   \
--env-from   deployment/  <  nam  e  >  /  <  containe  r  >   \
-v   $(  pwd  )  /src:/app/src   \
-p   <  por  t  >  :  <  por  t  >


# === Bridge Traffic (when needed) ===
# Step 1: Create mount (prepares infrastructure)
gefyra   mount   create   \
--target   deployment/  <  nam  e  >  /  <  containe  r  >   \
--namespace   <  namespac  e  >   \
--name   <  mount-nam  e  >


# Step 2: Create bridge with personal routing
gefyra   bridge   create   \
--mount   <  mount-nam  e  >   \
--local   <  container-nam  e  >   \
--ports   <  cluster-por  t  >  :  <  local-por  t  >   \
--match-header-exact   x-gefyra:  <  your-i  d  >


# === Traffic Matching Options ===
--match-path-exact   /api/users
--match-path-prefix   /api/
--match-path-regex   ^/admin/.  *
--match-header-exact   x-gefyra:alice
--match-header-prefix   x-team:backend
--match-header-regex   x-user:.  *  test.  *


# === Inspection ===
gefyra   list
gefyra   mount   list
gefyra   bridge   list


# === Cleanup ===
gefyra   bridge   delete   <  bridge-nam  e  >
gefyra   mount   delete   <  mount-nam  e  >
gefyra   connection   disconnect


```


---


*[Gefyra](https://github.com/gefyrahq/gefyra) is open source. Built and maintained by[Blueshoe](https://www.blueshoe.io/) . If you want this workflow for your team but aren't sure where to start,[we can help](https://www.blueshoe.io/contact/) .*


---


## Do you have questions or an opinion? With your GitHub account you can let us know...


---


### Here are a few articles that you might also find interesting:


[django-o365: A Modern Mail Backend for Django and Exchange Online](https://www.blueshoe.io/blog/django-o365-a-modern-mail-backend-for-django-and-exchange-online/)


[django-o365](https://github.com/Blueshoe/django-o365) is an open-source mail backend for Django that enables sending emails via Exchange Online using OAuth 2.0. Born from real-world project requirements, this package is aimed at teams using Microsoft 365 who want to implement their email dispatch securely, maintainably, and without workarounds. This article provides an overview of the motivation, features, and usage.


by Robert Gutschale


[Mastering Django Caching: The Ultimate Guide from Cachalot to the Low-Level API Is your Django application slower than it should be? Long loading times not only frustrate your users but are also penalized by search engines. The solution often lies in a smart caching strategy. But where do you start? by Jonathan Kölbl](https://www.blueshoe.io/blog/django-caching-cachalot-performance-guide/)


[Handoff from Designer to Developer: What We Need for Frontend Implementation The transition from design to development is a critical moment in any frontend project. A well-thought-out handoff can save weeks of development time, while an incomplete handoff leads to endless questions, delays, and compromises. by Lukas Rüsche](https://www.blueshoe.io/blog/designer-to-developer-handoff/)


[Why Our Maintenance Reports Are a Key to Successful Collaboration For us, a maintenance report is more than a table of numbers: it is a tool for communication, planning, and trust. It shows what we have done, where potential lies, and what topics are on the agenda for the next quarter - so our customers know their systems are in good hands. by Robert Gutschale](https://www.blueshoe.io/blog/quarterly-maintenance-reports/)


[Our Roadmap for Your Software: How We Plan, Develop, and Deploy Updates Software updates are crucial for the security, efficiency, and feature set of our products. But how do we ensure that every update is a real step forward for our customers? by Robert Gutschale](https://www.blueshoe.io/blog/how-we-plan-software-updates/)


[More Blog Articles Discover exciting insights and practical tips in our latest blog posts. From Python and Rust to Kubernetes, Django security, and frontend topics – find valuable knowledge for your projects here. Click to explore all articles!](https://www.blueshoe.io/blog/)


## These companies trust Team Blueshoe


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
