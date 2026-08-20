---
schema_version: "1.0.0"
document_id: "8c35ab9f7d8d95776468a17aa57031b8be3fddc5b5bf5232319db36407ed41a5"
company_key: "yc-metoro"
company: "Metoro"
source_id: "yc-metoro-news-import-a8c8033caa18"
canonical_url: "https://metoro.io/blog/introducing-kubernetes-native-rbac"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-24T04:14:18.908490+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:4a1e0b0fa8855a6ccd8575571ec92d071c8f668c2fae5fc0a9a717356e7ae546"
---

# Introducing Kubernetes-native RBAC for Metoro

Today, we’re releasing a new access-control model for Metoro: row-level RBAC for telemetry and Kubernetes Custom Resources for managing permissions as code.


As an organization grows, access to observability data becomes harder to manage well. A single platform may serve application developers, SREs, security teams, and engineering leaders across many clusters and namespaces. Those teams need a shared view of production, but they do not all need access to every log line, metric, trace, profile, Kubernetes resource, dashboard, or alert.


Broad roles are easy to create, but difficult to justify and maintain. Narrow roles are safer, but become operationally expensive when they live outside the systems teams already use to manage infrastructure. The result is often a choice between excessive access and constant manual administration.


Metoro’s new RBAC model removes that trade-off. Teams can now control access at the level of individual telemetry records, while managing those policies through Kubernetes and GitOps workflows when they choose.


## Access control at the same granularity as your telemetry


Traditional observability permissions often stop at the environment or data-source boundary. Metoro can now apply access rules within an environment, using the attributes already attached to telemetry.


A rule can grant a group access to specific signal types - logs, metrics, traces, profiles, or Kubernetes resources - and then narrow that access with match expressions. Those expressions can use standard attributes such as` namespace` ,` service.name` ,` metric.name` ,` severity` , and` status.code` , as well as custom attributes added by your organization.


This makes policies such as these possible:


- The payments on-call team can see metrics, traces, and Kubernetes resources from the` payments` namespace, but only logs marked as non-sensitive.
- An SRE group can see infrastructure logs from` kube-system` ,` istio-system` , and` monitoring` without gaining access to every application log in the cluster.
- Engineering leaders can view the cluster-level metrics behind shared capacity dashboards without receiving blanket access to application telemetry.


The filtering is part of the user’s effective permissions. Users query and explore Metoro as normal; the access policy determines which telemetry they can see.


## Kubernetes is now the control plane for Metoro access


Row-level controls solve the granularity problem. CRD-managed RBAC solves the operational one.


Metoro access policies can now be declared as Kubernetes Custom Resources and deployed alongside the workloads they govern. Policies can live in Git, go through the same review and approval process as other infrastructure changes, and roll out through the same delivery tooling.


The model deliberately follows Kubernetes’ distinction between` Role` and` ClusterRole` . Metoro provides four Custom Resources across two access-control planes:


Scope Telemetry access Metoro resource access


Namespace` MetoroNamespaceTelemetryRule`` MetoroNamespaceResourceRule`


Cluster` MetoroClusterTelemetryRule`` MetoroClusterResourceRule`


Telemetry rules determine which signals a group can view. Resource rules determine what a group can do with Kubernetes-managed dashboards, alerts, and webhooks in Metoro.


The location of a rule establishes its maximum scope. A namespace rule can grant access only within the namespace where it is created. A cluster rule can grant access across its cluster, including to namespaceless telemetry such as node-level metrics. Attribute matchers and resource paths can narrow that inherited scope, but a namespace rule cannot use them to reach into another namespace.


That constraint is important: your existing Kubernetes RBAC becomes the trust boundary for managing Metoro access. If a team is permitted to create a Metoro namespace rule in its namespace, it can delegate access within that namespace - but not beyond it. Platform teams do not need to invent and operate a parallel delegation system.


## A policy looks like the Kubernetes RBAC you already know


The following namespace-scoped rule grants the` payments-oncall` group access to metrics, traces, and Kubernetes resources in the` payments` namespace. It also grants access to logs, but only when the` is_sensitive` attribute is` false` .


```text
apiVersion  :   rbac.metoro.io/v1alpha1
kind  :   MetoroNamespaceTelemetryRule
metadata  :
name  :   payments -  team -  view
namespace  :   payments
spec  :
groups  :
-   payments -  oncall
rules  :
-    signals  :
-   metrics
-   traces
-   kubernetesResources
-    signals  :
-   logs
match  :
matchExpressions  :
-    key  :   is_sensitive
operator  :   In
values  :
-    "false"


```


Because this is a namespace rule, Metoro automatically combines each grant with the current cluster and the` payments` namespace. The team does not have to repeat those boundaries in every expression, and the rule cannot accidentally grant access outside them.


Match expressions support` In` ,` NotIn` ,` Exists` ,` DoesNotExist` , and` RegexIn` . Multiple expressions within a matcher are combined, while grants from separate rules are additive. This supports precise policies without introducing deny-rule ordering or precedence to reason about.


## Telemetry and operational resources are governed separately


Being able to see telemetry is different from being able to change the systems built around it. Metoro keeps those concerns separate.


Telemetry rules are view-only because telemetry signals are either visible or hidden. Resource rules include` create` ,` read` ,` update` , and` delete` verbs, so teams can have different levels of control over Kubernetes-managed dashboards, alerts, and webhooks.


For example, an application team can manage dashboards and alerts associated with its own namespace, while a central SRE team manages shared dashboards and webhooks. Resource paths can narrow permissions further within the namespace or cluster boundary.


This separation makes it possible to grant the access people need during normal operations and incidents without implicitly granting administrative control.


## Default-deny, allow-only, and auditable


The new model is default-deny and allow-only. A user cannot access telemetry or a Metoro resource unless a rule explicitly grants it. When a user belongs to multiple groups, their effective access is the union of the applicable grants.


An allow-only model avoids hidden interactions between allow and deny rules. It also makes policy review more direct: each rule answers who receives access, what they can access, and - where applicable - what actions they can perform.


Managing those rules as CRDs adds an operational audit trail. Teams can review policy changes before deployment, inspect the active rules with familiar Kubernetes tooling, and use Git history to understand when and why access changed. Because every CRD-managed grant is represented by one of these resources, administrators can audit the Kubernetes-managed policy set without searching for permissions scattered across workload annotations.


## Central control where needed, self-service where it works


Different environments often need different operating models. Production may require centralized approval, while development clusters benefit from team self-service. Metoro supports both without changing the policy language.


A platform team can retain exclusive permission to create all Metoro access rules. It can delegate only namespace-scoped rules to application teams. Or it can combine the two approaches - for example, allowing self-service in development while keeping production access centralized.


For engineering leadership, this means access policy can scale with the organization without losing accountability. For platform and security teams, it means least-privilege controls can be made specific enough to be useful and operationally realistic enough to maintain. For application teams, it means access to the telemetry they own can move at the same speed as the services they operate.


## Available now


Kubernetes-native RBAC and row-level telemetry controls are available in Metoro now. Start by mapping users into Metoro groups, then define namespace- or cluster-scoped rules for the telemetry and resources those groups need.


Read the documentation for[managing Metoro access with Kubernetes Custom Resources](https://metoro.io/docs/user-management/kubernetes-crds) and[configuring RBAC in Metoro](https://metoro.io/docs/user-management/rbac) .
