---
schema_version: "1.0.0"
document_id: "ad37910ac5638228f136eda2ee0d5d98739173295ad9e5734566555ecf1a22b1"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/argocd-in-production-patterns-that-actually-matter"
published_at: "2026-03-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:585924d5c6b1f4138522aa0107a44e61f9655b0472a4a07aec809453fc275095"
---

# ArgoCD in Production: Patterns That Actually Matter

Installing ArgoCD takes 10 minutes. Getting your first app syncing from Git feels like magic - you push a commit, ArgoCD detects the change, and your cluster converges to the desired state. Clean, elegant, done.


Then reality hits.


Your second cluster needs the same addons but different resource limits. A developer accidentally deletes an ApplicationSet and takes down 12 services. Your team of 40 engineers is filing Jira tickets asking DevOps to "please update the replica count." And you're drowning in YAML files that are 80% identical across environments.


The gap between "ArgoCD installed" and "developers can actually ship" is enormous. This post covers the ArgoCD best practices and production patterns that bridge it.


**TL;DR** : ArgoCD is free and powerful, but running it for a real team requires patterns it doesn't ship with - separating addons from app services, Kustomize base/overlays to eliminate YAML duplication, ApplicationSet generators for multi-cluster auto-discovery, three-layer deletion protection, sync waves for ordered deployments, and a generation layer so developers never touch YAML directly. We cover all of them with working examples.


## Why GitOps, Why ArgoCD


The GitOps model is simple: Git is your source of truth. Every change is a commit - auditable, reviewable, reversible. Rollback is` git revert` . Drift detection is automatic. No more SSH-ing into a cluster to` kubectl apply` a hotfix that nobody documents.


ArgoCD has become the default GitOps engine for good reasons. The built-in UI gives you real-time visibility into sync status across clusters. ApplicationSets let you template deployments across dozens of clusters from a single definition. The ecosystem is massive - hundreds of integrations, active development, and battle-tested at scale by companies like Intuit (where ArgoCD originated), Red Hat, and dozens of CNCF adopters.


But ArgoCD is a tool, not a platform. It gives you a powerful engine - what you build on top of it determines whether your team ships faster or drowns in YAML.


This is part of our three-part ArgoCD series. Start with[How to Set Up ArgoCD on Kubernetes](https://www.skyhook.io/blog/how-to-set-up-argocd-on-kubernetes) if you haven't installed it yet, or see[ArgoCD Multi-Cluster Architecture](https://www.skyhook.io/blog/argocd-multi-cluster-architecture) for choosing between centralized and per-cluster deployments.


## Centralized vs Per-Cluster ArgoCD


The first architectural decision: do you run one ArgoCD instance managing all clusters, or one per cluster?


Centralized Per-Cluster


**Visibility** Single pane of glass across all clusters Separate UI per cluster


**Auth complexity** High - needs credentials for every remote cluster Low - only manages local cluster


**Blast radius** One misconfiguration affects everything Isolated - failure stays local


**Operational overhead** One instance to maintain N instances to upgrade, monitor, patch


**Network requirements** Needs network path to all clusters No cross-cluster networking needed


**Our take** : Per-cluster is the safer default for production - credential isolation and blast radius containment are hard to retrofit later. Centralized works for small setups (2-3 clusters, single team, same network) but you'll likely outgrow it. We go deep on this decision - including how an orchestration layer above ArgoCD changes the calculus - in[ArgoCD Multi-Cluster Architecture: Centralized vs Per-Cluster](https://www.skyhook.io/blog/argocd-multi-cluster-architecture) .


Either way, the patterns in this post apply regardless of which architecture you choose. They're about what ArgoCD manages, not how many instances you run. For visibility across per-cluster ArgoCD instances without the blast radius of centralized, tools like[Radar](https://radarhq.io/vs/lens) give you a GitOps-aware view of sync status across clusters.


## Addons vs Application Services: Different Beasts


This is where most ArgoCD setups start to creak. Teams treat cert-manager and their user-facing API the same way - same ArgoCD project, same sync policies, same review process. But they're fundamentally different:


Platform Addons Application Services


**Examples** cert-manager, ingress-nginx, external-dns, Prometheus user-api, payment-service, frontend


**Owned by** Platform / DevOps team Application teams


**Change frequency** Monthly or quarterly Multiple times per day


**Rollout strategy** Careful, coordinated, often cluster-by-cluster Fast, per-service, canary or rolling


**Risk profile** Breaking cert-manager breaks TLS for everything Breaking one service affects one service


**Config source** Helm charts from external repos Your own repo with Kustomize overlays


Mixing these creates real problems. A developer pushing a frontend change shouldn't need to understand why their sync is queued behind a Prometheus upgrade. An addon upgrade shouldn't be blocked because a dev team has an unresolved sync error on their service.


**Separate them** . Use distinct ArgoCD AppProjects for addons and application workloads. Give them different sync policies - addons get` automated: false` with manual promotion, application services get` automated: true` with` selfHeal: true` . Different RBAC rules, different notification channels.


A minimal AppProject for isolating a team's workloads looks like this:


```text
apiVersion  :   argoproj.io/v1alpha1
kind  :   AppProject
metadata  :
name  :   team-payments
namespace  :   argocd
spec  :
sourceRepos  :
-   "https://github.com/your-org/payment-*"
destinations  :
-   server  :   https://kubernetes.default.svc
namespace  :   "payments-*"
clusterResourceWhitelist  : []     # No cluster-scoped resources
roles  :
-   name  :   deployer
policies  :
-   p, proj:team-payments:deployer, applications, sync, team-payments/*, allow
-   p, proj:team-payments:deployer, applications, get, team-payments/*, allow
```


This restricts the team to their own repos and namespaces - they can't accidentally deploy into another team's namespace or sync from an unauthorized repo.


The directory structure should reflect this:


```text
argocd/
├── addons/                          # Platform team owns this
│   ├── security/
│   │   ├── cert-manager/
│   │   └── sealed-secrets/
│   ├── observability/
│   │   ├── prometheus/
│   │   └── grafana/
│   └── networking/
│       └── ingress-nginx/
│
└── workloads/                       # App teams own this
├── user-api/
├── payment-service/
└── frontend/


```


## Eliminating YAML Duplication Across Clusters


You have 3 clusters - dev, staging, prod. You have 8 addons. That's 24 near-identical sets of YAML if you're not careful. Add a fourth cluster and you're copying files again, hoping you don't miss a value.


### Base + Overlays with Kustomize


Kustomize's overlay pattern is the foundation. One` base/` directory holds the shared manifests. Each cluster gets an` overlays/` directory with only the differences.


```text
addons/cert-manager/
├── base/
│   ├── kustomization.yaml
│   ├── deployment.yaml            # Shared: image, ports, health checks
│   └── rbac.yaml                  # Shared: same permissions everywhere
└── overlays/
├── dev/
│   ├── kustomization.yaml     # resources: [../../base]
│   └── patch-resources.yaml   # memory: 256Mi, replicas: 1
├── staging/
│   ├── kustomization.yaml
│   └── patch-resources.yaml   # memory: 256Mi, replicas: 2
└── prod/
├── kustomization.yaml
└── patch-resources.yaml   # memory: 512Mi, replicas: 3


```


The overlay patches are small - just the delta from base:


```text
# overlays/prod/patch-resources.yaml
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   cert-manager
spec  :
replicas  :   3
template  :
spec  :
containers  :
-   name  :   cert-manager
resources  :
requests  :
memory  :   "512Mi"
limits  :
memory  :   "512Mi"
```


When you need to bump the cert-manager image version, you change it once in` base/deployment.yaml` . Every cluster gets the update.


### ApplicationSet Auto-Discovery


The second layer of duplication elimination: don't manually create an ArgoCD Application for each cluster. Use an ApplicationSet with a Git file generator that auto-discovers clusters.


```text
apiVersion  :   argoproj.io/v1alpha1
kind  :   ApplicationSet
metadata  :
name  :   cert-manager
namespace  :   argocd
spec  :
generators  :
-   git  :
repoURL  :   https://github.com/your-org/infrastructure
revision  :   HEAD
files  :
-   path  :   "clusters/*/config.json"
template  :
metadata  :
name  :   "cert-manager-{{cluster.name}}"
spec  :
project  :   platform-addons
source  :
repoURL  :   https://github.com/your-org/infrastructure
path  :   "addons/cert-manager/overlays/{{cluster.name}}"
destination  :
server  :   "{{cluster.server}}"
namespace  :   cert-manager
syncPolicy  :
automated  :
prune  :   false
selfHeal  :   true
```


Each cluster has a config file at` clusters/{name}/config.json` :


```text
{
"cluster"  : {
"name"  :   "prod-us-east"  ,
"server"  :   "https://prod-us-east.example.com"  ,
"provider"  :   "gke"  ,
"region"  :   "us-east1"
}
}
```


Add a new cluster? Create its config file and overlay directory. The ApplicationSet picks it up automatically. No copy-pasting Application manifests.


### Group by Capability, Not by Cluster


Organize addons by what they do, not where they run. Group` cert-manager` and` sealed-secrets` under` security/` . Group` prometheus` and` grafana` under` observability/` . This keeps related configuration together and makes it obvious what's deployed where.


## Letting Developers Self-Serve Without Losing Control


Here's the uncomfortable truth: you can set up ArgoCD perfectly - clean directory structure, ApplicationSets, Kustomize overlays - and developers still can't ship without filing a ticket.


Why? Because the interface to ArgoCD is YAML files in a Git repo. Developers need to know which directory to edit, which fields to change, what values are valid, and how Kustomize patches work. That's not developer self-service. That's "we replaced the Jira ticket with a Git commit that's equally likely to break things."


Real self-service means developers interact with something that generates the right YAML for them - whether that's a CLI tool, an API, or a UI. The generated files land in a branch, go through a PR review, and merge to trigger ArgoCD sync. Developers get autonomy. Operators get a review gate. Git stays the source of truth.


The pattern looks like this:


1. **Developer** requests a change (new service, config update, scaling change)
2. **Generation layer** produces valid Kustomize manifests and commits them to a feature branch
3. **PR review** by the platform team (or automated policy checks) before merge
4. **ArgoCD** picks up the merged change and syncs to the cluster


The generation layer is the hard part. It encodes your organization's conventions - naming standards, resource limits, required labels, network policies. Without it, you're relying on developers to read a wiki page and get the YAML right. They won't, and you can't blame them.


## Deletion Protection: Three Layers Deep


One pattern that's non-negotiable in production: deletion protection. ArgoCD is powerful enough to delete everything it manages, and a single misconfiguration can cascade fast.


Layer 1 - **ApplicationSet level** : Prevent the ApplicationSet from nuking all its Applications if it's accidentally deleted.


```text
spec  :
preserveResourcesOnDeletion  :   true
```


Layer 2 - **Sync policy level** : Prevent ArgoCD from deleting resources that are removed from Git. This catches the "someone removed a file by mistake" scenario.


```text
spec  :
syncPolicy  :
automated  :
prune  :   false      # Don't auto-delete resources missing from Git
```


Layer 3 - **Resource annotation level** : Protect individual critical resources from deletion even during intentional cleanup operations.


```text
metadata  :
annotations  :
argocd.argoproj.io/sync-options  :   Delete=false
```


Use all three. Layer 1 protects against ApplicationSet deletion. Layer 2 protects against accidental file removal. Layer 3 protects your most critical resources (AppProjects, namespaces, CRDs) from any deletion path. They're additive and each catches scenarios the others miss.


## Sync Waves: Coordinating Ordered Deployments and Database Migrations


Teams migrating from a PaaS to Kubernetes often hit this surprise: on a PaaS, deployment ordering is built in. You define "run migrations after deploy" and it just works. In GitOps, everything applies at once by default.


ArgoCD solves this with sync waves and hooks. Sync waves assign an integer to each resource - lower numbers deploy first, higher numbers wait. Hooks run jobs at specific lifecycle points (PreSync, PostSync, SyncFail).


Here's a real scenario: a team migrating from a PaaS needs their deployment to follow a specific order. First, prepare config and secrets. Then deploy the service. Then run the database migration. Finally, clear the cache.


```text
# Wave 0: Config and secrets (must exist before the app starts)
apiVersion  :   v1
kind  :   ConfigMap
metadata  :
name  :   api-config
annotations  :
argocd.argoproj.io/sync-wave  :   "0"
data  :
DATABASE_URL  :   "postgres://db.internal:5432/app"
CACHE_ENDPOINT  :   "redis.internal:6379"
---
# Wave 1: Main deployment (waits for config to be ready)
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   api
annotations  :
argocd.argoproj.io/sync-wave  :   "1"
spec  :
replicas  :   3
selector  :
matchLabels  :
app  :   api
template  :
metadata  :
labels  :
app  :   api
spec  :
containers  :
-   name  :   api
image  :   ghcr.io/org/api:v2.1.0
envFrom  :
-   configMapRef  :
name  :   api-config
```


For the database migration and cache cleanup, you use PostSync hooks - they only run after the main deployment succeeds:


```text
# Wave 2: Database migration (runs after deployment is healthy)
apiVersion  :   batch/v1
kind  :   Job
metadata  :
name  :   db-migrate
annotations  :
argocd.argoproj.io/hook  :   PostSync
argocd.argoproj.io/sync-wave  :   "2"
argocd.argoproj.io/hook-delete-policy  :   BeforeHookCreation
spec  :
template  :
spec  :
containers  :
-   name  :   migrate
image  :   ghcr.io/org/api:v2.1.0
command  : [  "./migrate"  ,   "--direction"  ,   "up"  ]
restartPolicy  :   Never
backoffLimit  :   1
---
# Wave 3: Cache cleanup (runs after migration completes)
apiVersion  :   batch/v1
kind  :   Job
metadata  :
name  :   cache-flush
annotations  :
argocd.argoproj.io/hook  :   PostSync
argocd.argoproj.io/sync-wave  :   "3"
argocd.argoproj.io/hook-delete-policy  :   BeforeHookCreation
spec  :
template  :
spec  :
containers  :
-   name  :   flush
image  :   ghcr.io/org/api:v2.1.0
command  : [  "./cache-cli"  ,   "flush"  ,   "--prefix"  ,   "v2.0-"  ]
restartPolicy  :   Never
backoffLimit  :   1
```


The` BeforeHookCreation` deletion policy cleans up the previous Job before creating a new one on the next sync - otherwise you'll get name conflicts.


This works, but notice the effort. Four YAML files, specific annotations, an understanding of wave ordering and hook lifecycle. On a PaaS, this was a three-line config. That's not a criticism of ArgoCD - it's a more powerful model. But it's a real gap in developer experience that someone has to fill.


## The Missing Layer


Every pattern in this post works. Thousands of teams use them in production. But here's what they all have in common: someone has to build and maintain them.


The Kustomize base/overlay structure doesn't create itself. ApplicationSets need to be designed, tested, and updated as your needs evolve. Deletion protection has to be applied consistently - miss one resource and you're exposed. Sync waves require developers to understand ArgoCD's annotation model. And the self-service generation layer? That's a full internal product.


This is the real cost of ArgoCD in production. The tool is free. The platform layer you need on top of it - the conventions, guardrails, generation tooling, and developer experience - is months of work. And it's ongoing work, because the platform evolves with your team.


That's what[Skyhook](https://skyhook.io/) gives you out of the box. ArgoCD runs under the hood, configured with the patterns described here. Developers deploy through a UI and CLI without touching YAML. Addons and workloads are separated with proper isolation. Deletion protection is on by default. And when a team needs sync waves for an ordered deployment, it's a configuration option - not a week of YAML engineering.


If you want to build this yourself, you now have the blueprint. If you'd rather skip straight to shipping -[give Skyhook a try](https://skyhook.io/) .


## Further Reading


- [How to Set Up ArgoCD on Kubernetes](https://www.skyhook.io/blog/how-to-set-up-argocd-on-kubernetes) - Step-by-step installation guide, from zero to your first synced application
- [ArgoCD Multi-Cluster Architecture: Centralized vs Per-Cluster](https://www.skyhook.io/blog/argocd-multi-cluster-architecture) - Choosing the right deployment model for your team
- [Managing Kubernetes Add-ons: Argo CD or Terraform?](https://www.skyhook.io/blog/argo-cd-vs-terraform-for-kubernetes-add-ons) - Deep comparison of when to use each tool
- [Ship Without Fire Drills: Canary, Blue-Green, and Rolling Deploys](https://www.skyhook.io/blog/ship-without-fire-drills-canary-blue-green-and-rolling-deploys) - Progressive delivery strategies that pair well with ArgoCD
- [ArgoCD Sync Waves Documentation](https://argo-cd.readthedocs.io/en/stable/user-guide/sync-waves/) - Official reference for sync phases and waves
- [ApplicationSet Generators](https://argo-cd.readthedocs.io/en/stable/operator-manual/applicationset/Generators/) - Full list of generator types for multi-cluster patterns
