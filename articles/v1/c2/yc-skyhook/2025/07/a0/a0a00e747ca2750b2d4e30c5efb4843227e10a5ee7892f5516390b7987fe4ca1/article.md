---
schema_version: "1.0.0"
document_id: "a0a00e747ca2750b2d4e30c5efb4843227e10a5ee7892f5516390b7987fe4ca1"
company_key: "yc-skyhook"
company: "Skyhook"
source_id: "yc-skyhook-news-import-0a5e972b70e5"
canonical_url: "https://www.skyhook.io/blog/argo-cd-vs-terraform-for-kubernetes-add-ons"
published_at: "2025-07-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:39:47.906327+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:bcf58d03d4284917d6fbacace576b952864fd9972a6163948f110ccfd22a56b3"
---

# Managing Kubernetes Add-ons: Argo CD or Terraform?

## Why this matters


A big part of the magic of Kubernetes, and maybe the main reason it has exploded in popularity, is its extensibility through standardized APIs for resources and Custom Resource Definitions (CRDs). The ecosystem has exploded with tools or ”add-ons” like cert-manager, Argo Rollouts, External Secrets Operator, Loki, Prometheus, and Grafana, just to name a few, that significantly enhance the core Kubernetes capabilities.


These add-ons are not quite infrastructure, and not quite applications. Choose the wrong tool to manage them and you can end up fighting drift, breaking CI, or scrambling in production. Below you will see two of the most popular approaches to managing add-ons - Argo CD and Terraform - and why one is starting to pull away as the favorite among DevOps teams..


## TLDR


Use Terraform to bootstrap cloud and cluster. Use Argo CD to operate in-cluster add-ons. Argo CD is natively designed for this, solving a lot of problems that Terraform is ill-suited to handle.


## Key differences at a glance


Area Argo CD Terraform


Primary lifecycle role Day-2 app/add-on operations inside K8s Day-1 cloud/K8s infra provisioning


Scope In-cluster Kubernetes resources and Helm charts via Git Cloud infra plus Kubernetes via providers


Operational model Pull: continuous reconciliation and drift detection + auto-heal Push: plan and apply on demand, drift ignored until next run


Source of truth Desired state in Git, live state in the Kubernetes API server Desired state in Git (.tf), last known state in remote state (.tfstate)


Centralization Decentralized controllers per cluster, smaller blast radius Centralized pipelines and shared state


Typical ownership Platform/DevOps for setup, then application teams Platform/Infra teams (often too risky for non-experts)


Visibility Built-in UI and CLI show health, history, and diffs CLI output unless paired with other tools


Rollbacks Built-in rollback to previous revisions No native rollback - revert Git and re-apply


Multi-cluster ApplicationSet generators and App-of-Apps pattern Workspaces, modules, and pipelines


Failure modes to watch Bad commit breaks sync but is easy to revert State lock, provider errors, partial applies


Best fit Add-on upgrades, rollbacks, canary or blue/green releases Bootstrapping clusters, VPCs, IAM, initial add-on install


## Argo CD in action


```text
# Minimal cert-manager Application
apiVersion  :   argoproj.io/v1alpha1
kind  :   Application
metadata  :
name  :   cert-manager
namespace  :   argocd
spec  :
project  :   default
destination  :
server  :   https://kubernetes.default.svc
namespace  :   cert-manager
source  :
repoURL  :   https://charts.jetstack.io
chart  :   cert-manager
targetRevision  :   v1.15.0
helm  :
values  :   |
installCRDs: true
syncPolicy  :
automated  :
prune  :   true
selfHeal  :   true
syncOptions  :
-   CreateNamespace=true
```


**What you get**


- Immediate drift repair. If someone edits a resource with kubectl, Argo CD reverts it within seconds.
- Multi-cluster fan-out. ApplicationSet can generate identical apps for many clusters.
- Audit trail. Every change is a Git commit that can be rolled back with a click or PR revert.


## Terraform in action


```text
terraform   {
required_providers   {
helm          =   { source   =   "hashicorp/helm"  ,        version   =   "~> 2.13"   }
kubernetes    =   { source   =   "hashicorp/kubernetes"  ,  version   =   "~> 2.29"   }
}
}


provider   "kubernetes"   {
host                     =   var  .  cluster_endpoint
cluster_ca_certificate   =   base64decode  (var  .  cluster_ca)
token                    =   var  .  cluster_token
}


provider   "helm"   {
kubernetes   {
host                     =   var  .  cluster_endpoint
cluster_ca_certificate   =   base64decode  (var  .  cluster_ca)
token                    =   var  .  cluster_token
}
}


resource   "helm_release"   "cert_manager"   {
name               =   "cert-manager"
repository         =   "https://charts.jetstack.io"
chart              =   "cert-manager"
version            =   "v1.15.0"
namespace          =   "cert-manager"
create_namespace   =   true


set   {
name    =   "installCRDs"
value   =   "true"
}
}
```


**What you get**


- One workflow for everything. The same tool provisions VPCs, EKS or GKE, and Helm charts.
- Predictable plans. terraform plan shows a diff before anything changes.
- Remote state backends. S3, GCS, or Terraform Cloud keep state consistent across teams.


**Trade-off** : drift inside the cluster is invisible until the next plan. Fixes may require a replace if resources diverged.


### Two real-world drift moments


- **Someone deletes the cert-manager webhook**


- Argo CD: Application goes OutOfSync then heals automatically.
- Terraform: nothing happens until the next plan or scheduled run, then a replace may be required.


- **External Secrets Operator changes a CRD schema between chart versions**


- Argo CD: bump the chart version in Git, watch health, roll back instantly if custom resources fail to reconcile.
- Terraform: plan shows the Helm upgrade, but CRD behavior changes surface only at apply, sometimes with manual cleanup.


## Use the right tool for the job


### Day 1 with Terraform


- Provision VPC, cluster, node pools, IAM or OIDC roles.
- Install Argo CD.
- Output Argo CD URL and the bootstrap Git repo for later steps.


### Day 2 with Argo CD


- Manage every add-on as an Argo CD Application.
- Use PRs for upgrades and rollbacks.
- Watch diff and health views in daily stand-ups.


### Quick decision checklist


- Need to manage cloud primitives or create clusters. Choose Terraform.
- Need frequent add-on upgrades, rollbacks, and clear daily visibility. Choose Argo CD.
- Mixed needs. Bootstrap with Terraform, operate add-ons with Argo CD.
- Regulated environment with strict windows. Use Terraform plans for infra and PR reviews for Argo CD apps.


## How we implemented this in Skyhook


Skyhook ships with the hybrid model built in.


### Day 1 bootstrap


- One-click Terraform module spins up VPC, cluster, node pools, and IAM or OIDC roles.
- Run the module via Skyhook or commit it to your Git repository so you keep ownership.


### Day 2 and beyond


- Skyhook installs and configures Argo CD automatically - no manual YAML or Helm values to manage.
- Select which clusters should run Argo CD from a single dashboard.
- Browse a catalog of add-ons (cert-manager, Argo Rollouts, External Secrets, Loki, Prometheus, and more) and enable them per cluster.
- When you select an add-on, Skyhook opens a PR with the matching manifests in your repo - you approve, it deploys.
- Updates, rollbacks, drift repair, and multi-cluster fan-out all happen through Argo CD under the hood. Skyhook surfaces health, sync status, and history in one place.


The result: you get the power of Terraform for infrastructure and Argo CD for in-cluster operations, without the boilerplate and drift wars that come with wiring it all together yourself.


## Further reading


- Argo CD App-of-Apps pattern:[https://argo-cd.readthedocs.io/en/stable/operator-manual/app-of-apps/](https://argo-cd.readthedocs.io/en/stable/operator-manual/app-of-apps/)
- CNCF GitOps Working Group best practices:[https://opengitops.dev/](https://opengitops.dev/)
