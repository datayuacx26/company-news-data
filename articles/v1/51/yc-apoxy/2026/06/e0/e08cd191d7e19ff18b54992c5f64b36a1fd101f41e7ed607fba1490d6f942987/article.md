---
schema_version: "1.0.0"
document_id: "e08cd191d7e19ff18b54992c5f64b36a1fd101f41e7ed607fba1490d6f942987"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/managed-gateway"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:03.880414+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:b1c9ebb076c205689c97709c05db181babc424f8e7672bdcd1af13d47bf79a55"
---

# A cloud Gateway API for teams running more than one cluster

Teams running serious workloads on Kubernetes end up with more than one cluster. Multi-tenant Kubernetes architecture has a common compute story when doing a cluster per customer, per region, or per environment. The networking story is not told as frequently and we see many teams just accept the defaults from the infrastructure provider or whoever set it up last.


When the cross-cluster networking story does get told publicly, it tends to be a bespoke build from a very large and very experienced team.[Databricks has written](https://www.databricks.com/blog/multi-cloud-challenges-intelligent-load-balancing-and-ai-powered-workflows-databricks-srecon) about routing traffic across clusters, regions, and clouds but their answer was a client-side load balancer they wrote themselves after evaluating Istio and deciding against it.


Most teams end up with a stitched together pile of load balancers, DNS records, and ad-hoc routing logic scattered across clusters, sometimes across cloud providers. Every new tenant means another load balancer. Every routing decision lives somewhere different. A single view of where traffic actually goes is hard to come by, which means nobody can confidently change it.


We built Apoxy because our customers have hit this exact wall at production scale, and we built a single control plane that sees across all of their clusters. Today we're opening it up to everyone.


## One Gateway API, every cluster


Run our[kube-controller](https://docs.apoxy.dev/getting-started/kubernetes) in each cluster - a single` apoxy k8s install --cluster-name <name> --mirror gateway` per cluster. Your HTTPRoute objects show up in one control plane, alongside every other cluster you've connected. From there, you can see, edit, and reason about routing across the whole fleet from one place. No need to switch kubeconfigs and grep. The objects you write are still standard Gateway API resources meaning you can author them with the upstream` gateway.networking.k8s.io` group and the kube-controller's[mirror mode](https://docs.apoxy.dev/getting-started/kubernetes#mirror-mode) will replicate them into Apoxy automatically, so your existing manifests and GitOps flow can come along unchanged.


## See every request, in the tool you already use


OpenTelemetry traces and access logs fan out per-project to the destinations you are already using - Datadog, Grafana Cloud, Axiom, or any OTLP-compatible collector you run. You[set the destination once per project](https://docs.apoxy.dev/reference/core/cloudmonitoringintegrations) and the gateway will ship traces and logs there directly, with tenancy and route attributes already attached. We don't make you adopt our dashboard to see your own traffic, and we don't sit between you and the data.


## Run the data plane in our cloud or yours


There are two paths, depending on where your traffic has to go.


The self-serve cloud path runs on our globally-distributed Envoy fleet. Sign up, point a domain's DNS at us, connect your clusters, and your requests are flowing. This is what most teams want, and it's what's behind the sign up link below.


The in-VPC path runs the same data plane inside your own VPC. We can help teams integrate this when compliance or security regulations around data say the bytes can't leave their network and pass through ours. It's the same Gateway API and same telemetry but a different deployment shape.Our team can help stand it up and provide support with 24/7 on-call engineers or you can roll it yourself from our open source repository[on GitHub](https://github.com/apoxy-dev/apoxy) .


## Try it out today


Sign ups for the self-serve path are open right now. We would be happy to talk to you about in-VPC deployment which we will hand-shape with your team to meet your needs. The Gateway API we publish tracks v1 and it has been heavily tested against EKS and GKE. If you run another flavor and encounter any issues, tell us and we'll take a look.


Matt Ward


Co-founder & CEO


Matt is co-founder and CEO of Apoxy. He previously scaled infrastructure for YouTube and Mux. Outside of work, you can find him skiing down or pedaling up a mountain if he's not at home trying to be the kind of husband and father his family deserves.


[GitHub ↗](https://github.com/therealwardo)[X ↗](https://x.com/therealwardo)


[← Back to all posts](https://apoxy.dev/blog)
