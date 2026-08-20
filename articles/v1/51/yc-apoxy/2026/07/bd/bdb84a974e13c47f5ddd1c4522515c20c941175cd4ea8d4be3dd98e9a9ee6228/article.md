---
schema_version: "1.0.0"
document_id: "bdb84a974e13c47f5ddd1c4522515c20c941175cd4ea8d4be3dd98e9a9ee6228"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/case-studies/trainy"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:03.880414+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:3f90206ef5a55ecd9d8cc5d94c69446622adf2371549685e2ead35dbc0369146"
---

# How Trainy gives AI teams one control plane across every GPU cluster with Apoxy

## The company


Trainy builds two products for teams training AI models: an ML experiment tracker, and a GPU resource manager that schedules and allocates GPU capacity across the many Kubernetes clusters an ML team runs.


That second product is the hard one. A typical Trainy customer has locked-in GPU capacity spread across three clusters or clouds — sometimes a single cloud across regions, based on wherever the GPUs are actually available.


> Normally data is siloed within single clusters. The tooling is very good within one of those Kubernetes clusters, but in order to access GPU availability globally, people need these multi-cluster centralized controller solutions.


Andrew Aikawa, CTO, Trainy


Trainy exists to collapse those silos into one control plane and give ML engineers a way to launch jobs without thinking about placement. As Andrew puts it, they "don't really care where their jobs land, just that they land somewhere and then they can see the logs after." Managers get one view of cost and utilization across every cluster, instead of piecing it together per environment.


## The reliability breaking point


To deliver a single control plane, Trainy needed a persistent, secure connection from its central management cluster into each customer's Kubernetes environment. The first attempt was built on Tailscale and it just didn't work.


> We ran into issues specifically with Tailscale around provisioning certs. Anytime we would want to enroll a new cluster, it would take a really long time. I was trying to hand-engineer restarts and retries, and it would just never work consistently. So it was a non-starter.


Andrew Aikawa, CTO, Trainy


Every customer starts using Trainy by enrolling their clusters. If that step isn't reliable, Trainy's product can't exist.


## Why Apoxy


Trainy needed a reliable, fast, and secure network. That was the foundation for seeing and driving every customer's GPU clusters from one place. Apoxy took the hard part of standing up secure, TLS-terminated connections into each cluster from painful to trivial.


> We can spin up endpoints pretty quickly on our managed domains with TLS enabled, without having to think too much about it. And it happens really fast — in the order of seconds to minutes.


Andrew Aikawa, CTO, Trainy


And it costs less to run than the tools it replaced, across both ingress and connectivity:


> Even if ALB cost is zero, the fact that we get all the telemetry and MCP out of the box, it's a no-brainer to sign up for. I don't have to worry about managing certs, which I used to have to do on AWS ALB. So it's a lot easier to get started. And compared to Tailscale, it's a lot cheaper too — and it actually does what I need it to do.


Andrew Aikawa, CTO, Trainy


## How it works


Fig. 01 — Trainy Architecture


Reverse tunnel — dialed out from every cluster; the control plane reaches GPU clusters through it


HTTPRoute → web app — user traffic


HTTPRoute → telemetry backends — log dump from every cluster


Trainy uses Apoxy in three places:


**1. Ingress for the web app.** Trainy's stateless web application runs as microservices on EKS behind Apoxy. Replacing AWS ALB gave them faster response times, far more observability than ALB offered out of the box, and worry-free cert management. Migrating to Apoxy drove a **30–40% improvement in how fast graphs rendered** .


**2. Centralized observability.** Apoxy provides an analytics MCP server that enables the Trainy team to quickly debug their network traffic with natural language. This includes critical traffic for Trainy's own internal logs and metrics product, which relies on Apoxy ingress to expose Trainy's centralized log and metric backends to their customers' clusters.


**3. The hub-and-spoke reverse tunnel.** Trainy mints a shared-secret token; their customer uses it to install Trainy's agent in their own Kubernetes clusters. That agent opens a persistent reverse tunnel into Trainy's central management cluster via Apoxy. Trainy's control plane then drives the Kubernetes control plane inside each customer environment (effectively applying` kubectl` remotely). The result is a mesh that enables one control plane and one observability plane across every cluster and cloud a customer runs.


Crucially, this is invisible to the end customer:


> The way we use Apoxy, and how customers interact with it — they don't even know it's there, honestly. They just know that this domain is working, that they can send requests to it and bootstrap clusters to it, and it's over HTTPS.


Andrew Aikawa, CTO, Trainy


## The results


**A single control plane across clouds.** ML engineers launch jobs against one endpoint and watch them with the same Kubernetes commands they already know — there is no switching kube contexts, no per-cluster Grafana dashboards, and no manual sifting through monitoring data silos. Managers get month-end cost and utilization across every experiment, so they can decide what to divest and where to run the next "hero run."


> I think it's more than 3x better efficiency. Instead of manually going to check stuff — switching your Kubernetes context, switching your Grafana dashboard — now people can build watchers on just one endpoint, and then think about other important work or review code, rather than manually sifting through these data silos.


Andrew Aikawa, CTO, Trainy


**Faster onboarding of new clusters** — from "takes a really long time / never works" on Tailscale to TLS endpoints in seconds-to-minutes on Apoxy.


**Real money caught.** Trainy's centralized health-check telemetry feeds the SLA reports its customers file with their cloud providers — and with GPUs as expensive as they are, an unhealthy node caught late is "a thousands-of-dollars mistake over a couple of days." Centralizing that telemetry makes the reports auditable and actionable, so providers can fix issues immediately instead of chasing a vague "something's unhealthy."


**A path to self-serve.** Because standing up secure endpoints is now trivial, Trainy is moving its cluster-management product toward a fully self-serve onboarding motion.


> We're in the process of making it totally self-serve. I didn't think this was possible on other platforms.


Andrew Aikawa, CTO, Trainy


## What's next


Trainy is pushing toward agent-native operations and Apoxy's analytics MCP is a key part of that already. The team is looking forward to building a new agent that handles a ticket in their support tool by kicking off a coding-agent session and resolves the issue then they will ship a preview deployment exposed via Apoxy, reviewable from a single link on the PR. The open question is securing credentials for those agent sessions, which is exactly where Apoxy's roadmap is heading with[CLRK](https://apoxy.dev/clrk) .


> Not having to leave my terminal just saves me that extra step. Being able to do a quick digest of why I'm having a consistent 500 — having that on a cron would be great, rather than doing it reactively.


Andrew Aikawa, CTO, Trainy


Matt Ward


Co-founder & CEO


Matt is co-founder and CEO of Apoxy. He previously scaled infrastructure for YouTube and Mux. Outside of work, you can find him skiing down or pedaling up a mountain if he's not at home trying to be the kind of husband and father his family deserves.


[GitHub ↗](https://github.com/therealwardo)[X ↗](https://x.com/therealwardo)


[← Back to all posts](https://apoxy.dev/blog)
