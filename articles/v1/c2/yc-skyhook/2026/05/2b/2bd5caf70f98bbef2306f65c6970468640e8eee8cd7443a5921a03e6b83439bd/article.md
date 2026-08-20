---
schema_version: "1.0.0"
document_id: "2bd5caf70f98bbef2306f65c6970468640e8eee8cd7443a5921a03e6b83439bd"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/migrating-from-ecs-to-eks"
published_at: "2026-05-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T22:15:29.906392+00:00"
content_hash: "sha256:df0cb55aa66b16ab74e2d8e06daf9f4bdae7e6c0f09b780d696194748e53f9c7"
---

# Migrating from ECS to EKS: Architecture Challenges and Solutions

ECS works. For a small surface area, it works really well. The reason teams keep migrating off it isn't ECS itself - it's that the ecosystem you actually want is on the other side of the kubectl boundary.


By 2026, almost every interesting platform tool ships as a Kubernetes-first project: Karpenter, ArgoCD, External Secrets Operator, Crossplane, Backstage, Knative, every modern service mesh, every modern CRD-driven controller. ECS sits outside that ecosystem. You can build the same things on top of it - many teams have - but you build them yourself, and the rest of the industry isn't going to help you maintain it.


This post is for the team that's run a production workload on ECS for years and is now seriously planning the move to EKS. We'll skip the marketing comparison and go straight to the parts that catch teams out: networking, IAM, service discovery, autoscaling, deployments, secrets, and logging. For each one we'll show what changes, what to do about it, and the gotchas we've watched real migrations hit.


## TL;DR


- The hardest parts of an ECS-to-EKS migration aren't the workloads. They're networking (IP exhaustion with VPC CNI), IAM (Task Roles became IRSA, then Pod Identity), and rebuilding your deployment pipeline around a controller-driven model.
- Use a strangler pattern with a shared ALB and weighted target groups. Don't cut over in a single window.
- Don't lift-and-shift the deployment process. The ECS one-shot deploy doesn't translate. Adopt GitOps from day one.
- The migration is worth doing for the ecosystem, not for the AWS console UX. EKS is more powerful and more work. Plan for both.


## Why teams are migrating in 2026


The ECS-to-EKS conversation used to be about cost and portability. In 2026 it's about three things:


1. **Ecosystem gravity.** Every tool you want to add to your platform - Karpenter for compute, External Secrets for secrets, ArgoCD for deploys, OpenTelemetry collectors, service meshes, autoscalers smarter than CPU - assumes Kubernetes. ECS-equivalent paths exist for some of them but always lag, and often die.
2. **Compute economics.** Fargate is a beautiful abstraction with a markup. At scale, EKS plus Karpenter on Spot lands somewhere between 30% and 60% cheaper for the same workload, depending on what fraction of your traffic is interruptible.
3. **Hiring.** The pool of engineers who know Kubernetes is now meaningfully bigger than the pool who know ECS. New hires already speak` kubectl` . Onboarding documentation writes itself.


None of that means ECS is wrong. If your platform is small, your team is two people, and you don't need Kubernetes' extensibility, ECS is still the right call. The teams who should migrate are the ones who keep filing tickets that look like "we want to do X, and we have to write it ourselves on ECS, and someone else already wrote it for Kubernetes."


This isn't a fringe move, either. The CNCF's 2025 survey put Kubernetes in production at 82% of container-using organizations, and a string of well-documented ECS-to-EKS migrations have made the path repeatable:


- **Figma** moved off ECS to EKS in under 12 months. Their blockers were the ones above made concrete - no StatefulSets, no Helm, no clean way to run OSS like Temporal - and they landed on three EKS clusters with Karpenter for cost and resilience ([Figma engineering blog](https://www.figma.com/blog/migrating-onto-kubernetes/) ).
- **SailPoint** migrated 100+ microservices, standardizing on Karpenter and KEDA for autoscaling and GitOps with Kustomize and ArgoCD - the same controller-driven deploy model this post argues for ([SailPoint engineering](https://medium.com/sailpointengineering/ecs-to-eks-the-great-container-migration-b0bcf41991e9) ).
- **ADN** , a streaming platform with spiky release-day traffic, reported roughly a 25% cut in operational cost and faster issue identification after moving to EKS with Karpenter ([TrackIt case study](https://trackit.io/adn-case-study-ecs-to-eks-migration/) ).


The common thread is the strangler-style, Karpenter-first approach below - not a big-bang rewrite.


## Architecture mapping


The mental model translation isn't 1:1, but it's close. Here's what maps to what:


ECS concept EKS / Kubernetes equivalent Notes


Task Definition Pod template (in Deployment, Job, etc.) Container is the same. The wrapper has more knobs.


Service Deployment + Service Two objects in K8s, one in ECS.


Cluster Cluster Same word, different scope. EKS cluster manages a control plane; nodes are separate.


Capacity Provider Node Group / Karpenter NodePool Karpenter replaces ASG-based scaling for most teams.


Fargate (ECS) Fargate (EKS) or EC2 nodes EKS Fargate exists but most teams pick EC2 + Karpenter.


Task IAM Role Pod Identity (or IRSA) Per-pod IAM. Pod Identity is the 2024+ default.


ECS Service Discovery (Cloud Map) Kubernetes Service + CoreDNS DNS-based, in-cluster.


Service Auto Scaling HPA (workload) + Karpenter (nodes) Two layers, both required.


ECS Deployment / CodeDeploy ArgoCD / Flux + Argo Rollouts GitOps becomes the deploy plane.


Parameter Store / Secrets Manager (direct) External Secrets Operator Same upstream, different consumption pattern.


FireLens (awsfirelens) Fluent Bit DaemonSet / OTel collector Per-task became per-node.


ALB target groups (IP mode) AWS Load Balancer Controller (TargetGroupBinding or Ingress) Same target groups, K8s-native config.


awsvpc network mode VPC CNI (default) Conceptually identical. Operationally different at scale.


Every row in that table is a sentence. Most rows are a half-day of work. Two of them - networking and IAM - are the rows that come back and bite you.


## Challenge 1: Networking and the ENI question


ECS in` awsvpc` mode gives every task its own ENI and its own VPC IP. EKS does the same thing, by default, via the AWS VPC CNI. So far so good.


The catch: the VPC CNI assigns IPs to pods from the same subnet as the node, and each instance type caps how many it can attach -` (ENIs × (IPs per ENI - 1)) + 2` . A` c6i.large` tops out around 29 pods, a` c6i.xlarge` around 58, a` c6i.4xlarge` around 234. The subnet itself is the harder limit: if your pod subnets are` /24` (256 IPs) and you have a node group of 30 nodes running 200 pods, you've already used up the subnet, and you'll watch new pods hang in` ContainerCreating` with` failed to assign an IP address to container` .


Three real options for fixing this:


**Option A: Bigger subnets.** The cheapest fix and the one most teams pick. Drop your pod subnets to` /20` or larger, ideally one per AZ, and budget IPs based on your peak pod count plus headroom. This is a VPC redesign, which is exactly the kind of thing you don't want to discover halfway through a migration.


**Option B: Prefix delegation.** Configure the VPC CNI to assign` /28` prefixes to ENIs instead of individual IPs. Each ENI now hosts 16 IPs instead of one. A` c6i.4xlarge` jumps from ~234 pods of IP capacity into the thousands - well past the per-node ceiling kubelet enforces anyway (AWS recommends a` --max-pods` of 110 for clusters up to 100 nodes, 250 above that), so IP exhaustion stops being the constraint.


```text
kubectl   set   env   daemonset   aws-node   -n   kube-system   \
ENABLE_PREFIX_DELEGATION=  true   \
WARM_PREFIX_TARGET=  1
```


The cost is that prefixes are allocated whole. If you only need three IPs, you still take a` /28` (16 IPs) out of your subnet. Density up, fragmentation up.


**Option C: Custom networking with a secondary CIDR.** Add a secondary` 100.64.0.0/16` CIDR to the VPC and route pod traffic through it. The nodes stay in the original CIDR. This gives you huge pod IP space without renumbering your VPC, but the routing setup is non-trivial and worth doing only if you're going to be at scale.


The mistake we see: teams plan their EKS migration with the same subnet footprint they used for ECS, hit IP exhaustion two weeks after going to production, and discover that fixing it requires changing the cluster's networking configuration in ways that need careful coordination. Solve this one before you ship workloads.


## Challenge 2: IAM (Task Roles became Pod Identity)


In ECS, a Task Definition declares a` taskRoleArn` . AWS handles the credential delivery via a metadata service the task inherits. The container calls AWS SDKs and the SDKs find the right role automatically.


EKS used to require IRSA (IAM Roles for Service Accounts), which is a federated OIDC trust between the cluster and IAM. It works, and it's still common. EKS Pod Identity (GA in late 2023) is simpler and is now the recommended path. Pod Identity is closer to the ECS Task Role model: AWS handles the credential delivery via an agent on each node, no OIDC trust, no service account annotations.


ECS:


```text
{
"taskRoleArn"  :   "arn:aws:iam::1234:role/payments-task"  ,
"containerDefinitions"  : [{   "name"  :   "api"  ,   "image"  :   "..."   }]
}
```


EKS with Pod Identity:


```text
# 1. Create a ServiceAccount
apiVersion  :   v1
kind  :   ServiceAccount
metadata  :
name  :   payments
namespace  :   payments


# 2. Associate it with an IAM role (one-time, via aws CLI or IaC)
# aws eks create-pod-identity-association \
#   --cluster-name prod-us \
#   --namespace payments \
#   --service-account payments \
#   --role-arn arn:aws:iam::1234:role/payments-task


# 3. Reference the ServiceAccount from your Pod
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   api
namespace  :   payments
spec  :
template  :
spec  :
serviceAccountName  :   payments
containers  :
-   name  :   api
image  :   1234.dkr.ecr.us-east-1.amazonaws.com/api:v1.9.0
```


The IAM role's trust policy is also simpler than IRSA's. With Pod Identity it trusts` pods.eks.amazonaws.com` , not a per-cluster OIDC provider:


```text
{
"Version"  :   "2012-10-17"  ,
"Statement"  : [{
"Effect"  :   "Allow"  ,
"Principal"  : {   "Service"  :   "pods.eks.amazonaws.com"   },
"Action"  : [  "sts:AssumeRole"  ,   "sts:TagSession"  ]
}]
}
```


If you've still got IRSA-based clusters, they keep working. For greenfield, pick Pod Identity. The mental shift for ECS migrants is small: instead of attaching the role to the task definition, you attach it to the ServiceAccount the pod uses. Same idea, slightly different plumbing.


The trap to avoid: don't run all your pods under the` default` ServiceAccount with one giant catch-all role. ECS forced you to think per-task, and EKS lets you keep that hygiene. One ServiceAccount per workload. One role per ServiceAccount. Least privilege survives the migration.


## Challenge 3: Service discovery and load balancing


ECS gives you two service-to-service patterns: AWS Cloud Map (DNS-based service discovery) and ALB target groups for ingress. Neither survives the move untouched, but both have direct equivalents.


**Internal service-to-service.** In ECS, you might have` payments-api.svc.local` resolving via Cloud Map to the running tasks. In EKS, you write a Service:


```text
apiVersion  :   v1
kind  :   Service
metadata  :
name  :   payments-api
namespace  :   payments
spec  :
selector  :
app  :   payments-api
ports  :
-   port  :   80
targetPort  :   8080
```


Other workloads in the cluster reach it at` payments-api.payments.svc.cluster.local` , or just` payments-api` from inside the same namespace. CoreDNS handles the resolution. This is one of the parts that gets simpler in EKS, not harder.


**External / ingress.** In ECS you registered tasks with an ALB target group via the service definition. In EKS, the AWS Load Balancer Controller does the same thing with two patterns:


The Ingress pattern (controller provisions an ALB):


```text
apiVersion  :   networking.k8s.io/v1
kind  :   Ingress
metadata  :
name  :   payments-api
namespace  :   payments
annotations  :
alb.ingress.kubernetes.io/scheme  :   internet-facing
alb.ingress.kubernetes.io/target-type  :   ip
alb.ingress.kubernetes.io/listen-ports  :   '[{"HTTPS":443}]'
spec  :
ingressClassName  :   alb
rules  :
-   host  :   api.example.com
http  :
paths  :
-   path  :   /
pathType  :   Prefix
backend  :
service  :
name  :   payments-api
port  : {   number  :   80   }
```


The TargetGroupBinding pattern (you bring an existing ALB and target group, the controller registers pods into it):


```text
apiVersion  :   elbv2.k8s.aws/v1beta1
kind  :   TargetGroupBinding
metadata  :
name  :   payments-api
namespace  :   payments
spec  :
serviceRef  :
name  :   payments-api
port  :   80
targetGroupARN  :   arn:aws:elasticloadbalancing:us-east-1:1234:targetgroup/payments-eks/abc
```


TargetGroupBinding is the killer feature for migrations. It lets you keep your existing ALB and route traffic to ECS and EKS simultaneously while you cut over. We'll come back to this in the migration section.


## Challenge 4: Autoscaling at two layers


ECS has one autoscaling concept (Service Auto Scaling) that scales tasks based on metrics. EKS has two: HPA scales pods, and a node autoscaler scales nodes. Both are required. You don't want to be the team that forgot the node layer and watched pods queue at` Pending` because the cluster ran out of nodes.


For pods, HPA is straightforward and direct:


```text
apiVersion  :   autoscaling/v2
kind  :   HorizontalPodAutoscaler
metadata  :
name  :   api
namespace  :   payments
spec  :
scaleTargetRef  :
apiVersion  :   apps/v1
kind  :   Deployment
name  :   api
minReplicas  :   4
maxReplicas  :   40
metrics  :
-   type  :   Resource
resource  :
name  :   cpu
target  : {   type  :   Utilization  ,   averageUtilization  :   65   }
```


For nodes, the answer in 2026 is Karpenter. The Cluster Autoscaler still works, but Karpenter is faster, smarter about instance types, and natively handles Spot. A typical NodePool:


```text
apiVersion  :   karpenter.sh/v1
kind  :   NodePool
metadata  :
name  :   default
spec  :
template  :
spec  :
nodeClassRef  :
group  :   karpenter.k8s.aws
kind  :   EC2NodeClass
name  :   default
requirements  :
-   key  :   karpenter.k8s.aws/instance-category
operator  :   In
values  : [  c  ,   m  ,   r  ]
-   key  :   karpenter.k8s.aws/instance-generation
operator  :   Gt
values  : [  "5"  ]
-   key  :   karpenter.sh/capacity-type
operator  :   In
values  : [  spot  ,   on-demand  ]
limits  :
cpu  :   "1000"
disruption  :
consolidationPolicy  :   WhenEmptyOrUnderutilized
consolidateAfter  :   30s
```


Karpenter looks at pending pods, picks the cheapest instance type that satisfies them, launches it directly (no ASG), and consolidates underutilized nodes when they free up. Teams typically see 30-50% node cost reduction over Cluster Autoscaler with mixed Spot/on-demand, which is most of why ECS-to-EKS pencils out economically at scale.


The gotcha: HPA scales reactively on metrics that are seconds-to-minutes behind. A traffic spike that overwhelms the existing pods can take a minute or two for HPA to react and another minute for Karpenter to spin up nodes. If your workload is bursty enough that you can't tolerate that, you need pod over-provisioning (always run extra capacity) or KEDA (event-based scaling) to scale ahead of the metric. Plan for which pattern you need before you cut over.


## Challenge 5: Deployments are no longer one-shot


ECS deployments are, fundamentally, an API call. CodePipeline runs` aws ecs update-service` , ECS swaps tasks based on the deployment configuration, you're done. The deployment is imperative and stateless: nothing watches your Git repo.


EKS deployments can work that way (` kubectl apply -f` is the same shape) but you'd be giving up the main reason to be on Kubernetes. The standard EKS pattern is GitOps: a controller in the cluster (ArgoCD or Flux) watches a Git repo, reconciles the cluster to match it, reports drift, and lets you roll back by reverting a commit.


The team that lifts-and-shifts the ECS deploy pipeline (CI runs` kubectl apply` directly) ends up with a worst-of-both-worlds setup: imperative deploys that can drift, no audit trail beyond the CI logs, and no native rollback. We've seen this pattern enough that we wrote a[longer comparison of ArgoCD versus Terraform for managing add-ons](https://www.skyhook.io/blog/argo-cd-vs-terraform-for-kubernetes-add-ons) , but the short version applies to your own apps too: ArgoCD owns the cluster's desired state, your CI builds images and updates manifests, the controller does the rolling update.


For progressive delivery (canary, blue-green), the move from ECS CodeDeploy is natural. Argo Rollouts is the EKS equivalent, with explicit traffic splitting through your ALB:


```text
apiVersion  :   argoproj.io/v1alpha1
kind  :   Rollout
metadata  :
name  :   api
namespace  :   payments
spec  :
replicas  :   10
strategy  :
canary  :
steps  :
-   setWeight  :   10
-   pause  : {   duration  :   5m   }
-   setWeight  :   25
-   pause  : {   duration  :   5m   }
-   setWeight  :   50
-   pause  : {   duration  :   10m   }
-   setWeight  :   100
trafficRouting  :
alb  :
ingress  :   payments-api
servicePort  :   80
```


If you ran ECS with CodeDeploy's blue-green setup, the mental model is the same. The mechanics now live in Argo Rollouts, not in CodeDeploy. We covered the deeper tradeoffs in[Ship Without Fire Drills](https://www.skyhook.io/blog/ship-without-fire-drills-canary-blue-green-and-rolling-deploys) .


## Challenge 6: Secrets and config


ECS lets you reference SSM Parameter Store and Secrets Manager directly in a Task Definition. The agent fetches them at task start and injects them as env vars. Clean, simple, no extra components.


EKS has no native equivalent. The good news: External Secrets Operator (ESO) is the universal answer and works well. The bad news: it's a controller, not a runtime injection - so secrets land in Kubernetes` Secret` objects, which means etcd, which means encryption-at-rest considerations.


A SecretStore pointing at AWS Secrets Manager:


```text
apiVersion  :   external-secrets.io/v1
kind  :   SecretStore
metadata  :
name  :   aws-secrets
namespace  :   payments
spec  :
provider  :
aws  :
service  :   SecretsManager
region  :   us-east-1
auth  :
jwt  :
serviceAccountRef  :
name  :   payments
```


A workload's secret pulled from there:


```text
apiVersion  :   external-secrets.io/v1
kind  :   ExternalSecret
metadata  :
name  :   payments-db
namespace  :   payments
spec  :
refreshInterval  :   1h
secretStoreRef  :
name  :   aws-secrets
kind  :   SecretStore
target  :
name  :   payments-db
dataFrom  :
-   extract  :
key  :   prod/payments/db
```


The pod consumes` payments-db` like any other Secret: env vars, volumes, whatever. ESO refreshes on the configured interval and updates the K8s Secret in place. Pods don't auto-restart on secret rotation by default - if you want that, add` reloader` (a small controller that watches Secrets and bounces dependent Deployments) or use a sidecar that re-reads the secret.


For people who hate having plaintext-but-encrypted secrets in etcd at all, the alternative is Secrets Store CSI Driver - it mounts secrets as volumes directly from Secrets Manager without going through a K8s Secret object. More moving parts. Use it if your compliance posture requires it, otherwise ESO is simpler.


## Challenge 7: Logging changes shape


ECS with FireLens lets you configure per-task log routing. EKS doesn't, by default. The standard pattern is a node-level Fluent Bit DaemonSet that collects everyone's logs and ships them to your destination of choice (CloudWatch, OpenSearch, Datadog, S3, anywhere).


The migration cost: per-workload log routing rules from ECS need to be re-expressed as Fluent Bit filters keyed by Kubernetes labels.


A minimal Fluent Bit values snippet that ships everything to CloudWatch but routes the` payments` namespace to a different log group:


```text
config  :
filters  :   |
[FILTER]
Name kubernetes
Match kube.*
Merge_Log On
Keep_Log Off


outputs  :   |
[OUTPUT]
Name cloudwatch_logs
Match kube.var.log.containers.*payments*
region us-east-1
log_group_name /aws/eks/prod/payments
log_stream_prefix payments-
auto_create_group true


[OUTPUT]
Name cloudwatch_logs
Match kube.var.log.containers.*
region us-east-1
log_group_name /aws/eks/prod/default
log_stream_prefix default-
auto_create_group true
```


If you're moving to OTel anyway, do it now. The OpenTelemetry Collector handles logs, metrics, and traces in one DaemonSet, and the configuration model survives the next vendor switch. Teams who set up Fluent Bit in 2026 and migrate to OTel in 2027 do roughly the same work twice.


## The migration playbook: don't cut over in one window


Lift-and-shift cutovers fail. The playbook that works is the strangler pattern with a shared ALB.


Step by step:


1.


**Stand up the EKS cluster, networking, and platform layer first.** No workloads yet. Solve the IP exhaustion question (Challenge 1) before any container runs. Install Karpenter, AWS Load Balancer Controller, External Secrets Operator, ArgoCD, Fluent Bit. Verify each one with a hello-world.


2.


**Pick one non-critical workload and rebuild it on EKS, properly.** Not a port. A rebuild from the ground up using EKS-native patterns: Deployment, Service, Ingress, ESO, Pod Identity, GitOps-managed. This forces every team in your org to learn the new shape on a workload where mistakes are cheap.


3.


**Bring up your second copy of a real workload behind the same ALB.** This is where TargetGroupBinding earns its place. Your existing ALB has one target group pointing at ECS; you create a second target group pointing at the EKS pods. Both target groups listen on the same ALB. You shift traffic via ALB weighted target groups: 95% ECS, 5% EKS. Then 80/20. Then 50/50.


```text
# Listener rule on the ALB (Terraform shape)
resource "aws_lb_listener_rule" "api" {
listener_arn = aws_lb_listener.https.arn
priority     = 100


action {
type = "forward"
forward {
target_group {
arn    = aws_lb_target_group.ecs_api.arn
weight = 80
}
target_group {
arn    = aws_lb_target_group.eks_api.arn
weight = 20
}
stickiness {
enabled  = false
duration = 1
}
}
}


condition {
host_header { values = ["api.example.com"] }
}
}
```


Roll forward with confidence; roll back by changing the weights. No cutover window, no DNS TTL games.


1.


**Cut workloads over one at a time.** Each one repeats step 3. The cluster slowly fills up. The ECS side slowly empties. When the last workload is cut, the ECS side is empty, and you tear it down.


2.


**Decommission deliberately.** Before deleting the ECS cluster, audit for residual things: scheduled tasks, one-off run-tasks, CloudWatch alarms scoped to ECS metrics, IAM roles only used by ECS, security groups, log groups. None of those move themselves.


The teams that try to do this in one weekend always regret it. The teams that take three months and run both stacks in parallel are the ones who land the migration without an outage on the front page.


## What Skyhook does for the EKS side


A lot of the EKS day-one and day-two complexity above is undifferentiated. Every team migrating off ECS stands up the same set of platform components: Karpenter, ArgoCD, ESO, AWS Load Balancer Controller, Fluent Bit. Every team writes the same opinionated Helm values. Every team builds the same internal docs explaining how to deploy a service.


[Skyhook](https://www.skyhook.io/) is one option for skipping the platform-from-scratch part. We provision EKS clusters with the platform layer pre-installed and pre-wired, commit the manifests to your Git repos so you own them, and operate the cluster from a single UI: deployments, scaling, secrets, environments. For a team that wants Kubernetes' ecosystem without spending a quarter building the platform, it's a shortcut. For a team that already has a platform team and a strong opinion about which CRDs they want, it's a trial subscription you can rip out in an afternoon - the manifests are all in your repo.


We're not pretending Skyhook is the only path. What we are saying is that "stand up the platform yourself, the way every other team has done it, with the same Helm values everyone else uses" stops being a useful exercise after the hundredth time the industry has done it.


## Wrap-up


The ECS-to-EKS migration is real work, but the hard parts aren't where most teams expect. The workloads almost always come over cleanly. The pipeline rebuild is straightforward if you commit to GitOps. The bills that come due are networking (plan IPs early), IAM hygiene (Pod Identity per workload, no catch-all role), and the discipline to use the strangler pattern instead of forcing a cutover.


If you do it right, the payoff isn't just "the same workloads on a different runtime." It's that every new platform capability the industry ships in the next five years will land in your cluster as a Helm chart, not a backlog item. That's the actual reason to do it.


If you're sizing this up and want to talk through the migration without a sales pitch, we're around. And if you want to skip the platform-from-scratch step,[Skyhook](https://www.skyhook.io/) is one way.
