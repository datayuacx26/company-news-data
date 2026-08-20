---
schema_version: "1.0.0"
document_id: "e40dc25bd92be30fff1f571363bc96c0535ca6eb5c5219a57356b061205cddd5"
company_key: "yc-infracost"
company: "Infracost"
source_id: "yc-infracost-news-import-5e947f59e679"
canonical_url: "https://www.infracost.io/resources/blog/kubernetes-support"
published_at: "2026-08-11T01:30:00+00:00"
first_seen_at: "2026-08-13T19:09:40.314345+00:00"
fetched_at: "2026-08-13T19:09:40.985056+00:00"
content_hash: "sha256:37840f5189df12ef87c41912d31492e1d6967e3d7f1849ae357cfe398a31d746"
---

# Kubernetes cost estimates and label policies in pull requests

# Kubernetes cost & label policies, in the pull request. No cluster access required.


[Infracost prices Kubernetes workloads](https://www.infracost.io/docs/features/kubernetes/) *straight from your repo and runs your label policies over them. Manifests, Helm, Kustomize. In the editor, in the pull request, and inside the agent loop.*


Twelve lines of YAML. Every line defensible on its own. Approved in forty seconds.


```text
spec  :
-   replicas  :  2
+  replicas  :  6
template  :
spec  :
containers  :
-  name  : db
resources  :
requests  :
-               cpu  :  "1"
-               memory  : 4Gi
+              cpu  :  "4"
+              memory  : 16Gi
volumeClaimTemplates  :
-  spec  :
resources  :
requests  :
-             storage  : 100Gi
+            storage


```


```text
spec  :
-   replicas  :  2
+  replicas  :  6
template  :
spec  :
containers  :
-  name  : db
resources  :
requests  :
-               cpu  :  "1"
-               memory  : 4Gi
+              cpu  :  "4"
+              memory  : 16Gi
volumeClaimTemplates  :
-  spec  :
resources  :
requests  :
-             storage  : 100Gi
+            storage


```


```text
spec  :
-   replicas  :  2
+  replicas  :  6
template  :
spec  :
containers  :
-  name  : db
resources  :
requests  :
-               cpu  :  "1"
-               memory  : 4Gi
+              cpu  :  "4"
+              memory  : 16Gi
volumeClaimTemplates  :
-  spec  :
resources  :
requests  :
-             storage  : 100Gi
+            storage


```


That costs about **$1,110 a month** . Thirteen thousand a year.


Check it yourself. Four vCPU across six replicas at $23.08 a vCPU-month is $554. 16Gi converts to 17.18 GB, so six of those at $3.09 comes to $319. Six volume claim templates at 500Gi and $0.08 a GiB-month is another $240.


## Nobody catches this by reading it


Cost in Kubernetes isn't local to any one line. It falls out of requests multiplied by replicas multiplied by a storage class, and in a Helm repo those values may not even live in the same directory. There's no dollar figure anywhere in the manifest for a reviewer to react to.


And the arithmetic is a pain. Multiply four numbers. Convert mebibytes to decimal gigabytes. Remember that a volume claim template provisions one disk *per replica* , which is where three terabytes came from.


Nobody is doing that at 4pm with six other PRs open. So the number turns up later, on an invoice, in a conversation between people who never saw the file.


## The cheap moment to fix this is right now


Catch it while the file is open and the fix is typing a smaller number. The engineer won't even log it as a fix. They'll pick a smaller size the same way they already pick a smaller instance type when the price is sitting next to them.


Find it on the invoice instead and you're looking at a ticket, an owner hunt, a review, a deploy, and a slightly tense conversation about who approved it, all to change one character in one file.


Meanwhile the six weeks of spend doesn't come back. Nobody refunds over-provisioned memory.


## What it does


Point it at the repo holding your charts, overlays or manifests. It reads them statically, prices what each workload reserves, and puts the number where the decision gets made.


No cluster connection. No agent to install, no credentials to hand over, no billing API access to argue about with your security team. Anyone who has taken a cost tool through a security review knows what that saves.


In the example above, $240 of the monthly figure is storage.


The rest of what's in a real repo gets priced too. Deployments and ReplicaSets, DaemonSets, Jobs and CronJobs, standalone PersistentVolumeClaims rated by their storage class, and Services of type LoadBalancer. Vendored Helm subcharts as well, so a bundled Postgres turns up with its own workloads and volumes, attributed to the chart files that pulled it in.


## The model is simple on purpose


```text


```


```text


```


```text


```


Any engineer on your team can check that arithmetic in their head. Cost comments die the first time somebody senior questions a figure and gets a vague answer back, and after that nobody reads the next one either.


Same manifest, same figure, every time. So a PR reading +$1,110 skips the argument about whether the number is right and lands on whether the change is worth it.


The two rates aren't ours. They're the published defaults from[OpenCost](https://www.opencost.io/) , the CNCF's vendor-neutral project for Kubernetes cost monitoring, so the figure comes from the reference the ecosystem already shares rather than something we picked.


They're flat rather than negotiated, and that part is deliberate. A discount moves every estimate by roughly the same proportion, so it changes nothing anyone decides while writing the file. It also means the unit your engineers are slowly learning doesn't shift under them the next time Finance renegotiates.


## The same number, before there's a PR to comment on


The pull request comment is where a cost change becomes everyone's business. It's the shared artifact: reviewers see it, platform owners see it, it's attached to the commit that caused it, and it's where a policy gate can stop something before it merges.


None of it needs a cluster, though, so the check isn't confined to CI. It runs wherever the manifests are, and they exist long before anyone opens a branch.


So it also runs in the editor. Infracost prices the workload while an engineer edits the values file. The person changing` memory: 4Gi` to` 16Gi` sees the cost while they're deciding, rather than defending the decision to a reviewer tomorrow morning.


The two do different jobs. The PR comment puts an expensive change in front of the people who own the budget, with a gate behind it if you want one. The editor stops a good share of those changes being written in the first place. Show someone a hundred of these figures over a few months and they stop guessing at sizes.


If you own platform, the win is coverage. A CI check covers what reaches CI. An editor check also covers the local experiments, the branches nobody pushed, and the sizing chosen while a service is still being scaffolded.


## The next StatefulSet in your repo might not have a human author


Ask an agent for one and it picks requests, replicas and a volume size in about four seconds. It has no idea what any of that costs, and nothing about how it was trained gives it a reason to care. It optimises for working, and the examples it learned from skew generous.


Look at the diff at the top again. 500Gi across six replicas is three terabytes of provisioned disk, and a model will write that line without ever registering the multiplication. Your reviewer misses it because the arithmetic is awkward and there are five other PRs waiting. The agent never attempted the arithmetic at all.


Telling people to review more carefully was already a weak answer when people were the ones writing the manifests. It doesn't survive generation running faster than review.


So the agent can run the check itself. It prices its own change mid-loop and corrects the size before it ever opens a PR. We rebuilt the CLI for agent callers rather than wrapping the old one, and what comes back is the same estimate a human sees in the editor.


## The same tagging rules, now running on Kubernetes


Your cloud bill can see the tags on an EC2 instance. It can see nothing at all about the pod scheduled onto it. For Kubernetes the labels are the allocation signal, and when they're missing or inconsistent there's nothing downstream to reconstruct them from.


Infracost treats Kubernetes labels as tags, so the tagging policies you already run against Terraform now evaluate workloads, volumes and load balancers the same way. One set of rules, defined once, covering both halves of the estate. Or if you want completely separate polices for Kubernetes that's easy too as we've made it simple to filter which resources or IaC frameworks you want the policies applied to.


It also runs before the workload exists, which matters more for labels than it does for cost. Labels can't be backfilled. Spend that ran for two weeks unlabelled stays unattributable permanently, and no scanner, nightly job or retro-tagging bot gets it back. Author time is the only point where you can guarantee a resource never runs untagged in the first place.


Policies evaluate after Helm templating and Kustomize transformers have run, so labels applied by a chart's` _helpers.tpl` or a` commonLabels` block count as though someone had typed them into every manifest by hand. That's how most repos actually set governance labels, and it's the difference between a policy you can adopt across a fleet and one that fails on every chart you own.


Labels are read from the workload's own` metadata.labels` rather than the pod template, so that's where governance labels need to live.


One thing to plan for: if you already run an organisation-wide tagging policy, it starts evaluating Kubernetes resources the moment this is switched on. Kubernetes label conventions rarely match cloud tag conventions, so scope policies by resource type first rather than after the first wave of failures.


## What the number is


Reference rates applied to what a workload requests. Not your negotiated rates, and requests rather than actual usage, so the absolute figure won't match your bill.


The comparison is the part that holds. If your real rate runs thirty percent under the default, every estimate is thirty percent high, and a change that doubles the estimate still doubles what you pay.


## What it doesn't do


-


**No rightsizing.** Nothing watches your cluster, so there's no "these pods are oversized right now." That's a separate piece of work and it's underway.


-


**No showback or chargeback of its own.** It enforces the labels those depend on, but the allocation itself happens in whatever you already use for it.


Some figures need input from you. Autoscaled workloads that leave out` spec.replicas` get priced as a single replica, so a fleet that runs at thirty pods reads badly low until you set a steady-state value. DaemonSets assume one node. Jobs and CronJobs assume zero runtime. GPU requests aren't priced at all.


We'd rather you read that here than turn it up in week two.


**We're building the runtime half too** : comparing requests against what pods actually use. If that's the part you need,tell us and we'll bring you in early.


> **See what your next Helm change costs.**[Point Infracost at your repo](https://www.infracost.io/docs/) and the estimates start showing up in the pull request. No cluster access, no agent, nothing for security to review.
