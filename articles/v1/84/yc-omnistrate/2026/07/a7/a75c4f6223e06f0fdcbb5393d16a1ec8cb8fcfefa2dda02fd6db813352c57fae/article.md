---
schema_version: "1.0.0"
document_id: "a75c4f6223e06f0fdcbb5393d16a1ec8cb8fcfefa2dda02fd6db813352c57fae"
company_key: "yc-omnistrate"
company: "Omnistrate"
source_id: "yc-omnistrate-news-import-f408a10afc71"
canonical_url: "https://omnistrate.com/blog/beyond-operators-the-enterprise-control-plane-layer"
published_at: "2026-07-14T09:00:00+00:00"
first_seen_at: "2026-07-22T07:13:55.392025+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:03ead23225f2777ad4cda2f165441c20b8203ef56597ac07fc80f2c3b0f6929b"
---

# Beyond Operators: The Enterprise Control Plane Layer

## Understanding the Kubernetes Operator Pattern


Scripts, Helm charts, Terraform modules, and runbooks are useful for installation and update tasks, but they are typically invoked at a point in time. An operator adds a reconciler that keeps comparing declared state with observed state.


Kubernetes Operators encode application-specific operations in Kubernetes-native control loops. Typical operator responsibilities include reconciliation, upgrade sequencing, backup and restore mechanics, status reporting, and application-specific failure handling.\[1\]


In Kubernetes terms, an operator normally combines a Custom Resource Definition, or CRD, with a controller. The custom resource stores the desired state; the controller watches those resources and moves the observed system toward that state.\[1\]\[2\]\[3\]


As an example, for a PostgreSQL service, the custom resource might define instance count, storage size, backup policy, image version, credentials, and bootstrap behavior. The operator then performs the PostgreSQL-specific work required to reconcile that specification.\[2\]


The pattern applies to workloads whose desired state extends beyond a single Deployment manifest. Common examples include databases, messaging systems, search engines, object storage, AI/ML infrastructure, model-serving platforms, GPU-backed inference services, and other stateful distributed systems.


The value is clearest for workloads whose operational requirements extend beyond container scheduling. They may involve persistent identity and data, distributed topology, dependency sequencing, specialized hardware, failover and recovery logic, version compatibility, and application-specific safety rules that a generic Pod or Deployment does not capture.


An operator can automate application management, but it does not usually model product-level concerns to deliver as a managed service.


## The Complementary Nature of Operators and Enterprise Control Planes


The relationship becomes clear when deploying the application is only one part of operating the complete service. An operator can reconcile the application inside Kubernetes, but it does not typically provision the surrounding cloud infrastructure, expose customer-facing lifecycle APIs, govern access, collect usage, manage billing, or coordinate operations across the full service stack.


Example control-plane responsibilities include (but are not limited to):


- Deploying services across clouds, customer accounts, Kubernetes clusters, and on-premises environments
- Provisioning and managing the required infrastructure
- Enforcing tenant isolation across deployment models
- Establishing secure connectivity into customer environments
- Validating service inputs before translating them into Kubernetes resources
- Coordinating transaction-safe infrastructure changes, rollbacks, and versioned upgrades
- Managing identities, credentials, secrets, audit logs, metrics, logs, and events
- Exposing lifecycle actions such as create, modify, stop, backup, restore, failover, and delete
- Tracking usage, entitlements, costs, invoicing, and billing
- Giving SREs a consistent way to operate across deployments without tenant-specific automation


These responsibilities extend the operator rather than replace it. The operator contributes the domain-specific logic required to run the application correctly; the control plane provides the infrastructure, product, operational, and commercial capabilities required to deliver it as a complete service.


### Control-Plane Responsibilities


A practical boundary is shared responsibility:


**The operator owns:** Kubernetes CRDs, controller logic, reconciliation, workload topology, backup and recovery mechanics, failover behavior, application health and status, and domain-specific upgrade rules.


**The control plane owns:** tenant and subscription lifecycle, service APIs and customer portals, infrastructure provisioning and management, deployment orchestration, versioned rollouts, drift detection, governance, observability, metering and billing, Day-2 automation, and support for deployment models such as single-tenant, cellular multi-tenant, BYOC, and air-gapped environments across clouds and on-premises.\[6\]


This boundary keeps the operator focused on application-specific state while giving the platform a consistent surface for provisioning, operations, governance, and service delivery.


### Five Core Integration Principles for Operator-Driven Service Platforms


To maximize this collaborative potential, an operator-backed service plan should adopt integration strategies that build upon the existing strengths of the operator while streamlining product delivery.


1.


**Shared Standards for Communication.** Use the same Kubernetes-native primitives (CRDs, Status, Events) for both layers. When the control plane and operator speak the same Kubernetes dialect, the control plane can natively observe the operator’s state and act on it without requiring bespoke or brittle integration logic.


2.


**Orchestrated Reconciliation.** Use the control plane to handle the *when* (e.g., scheduling a backup during off-peak hours) and the operator to handle the *how* (executing the specialized backup routine). This allows the control plane to orchestrate complex, multi-stage lifecycle workflows that the operator was not designed to manage independently.


3.


**Tenant-to-Instance Mapping.** The control plane should manage tenant identity, entitlements, and billing, translating those platform-level concepts into straightforward operator inputs. This provides the operator with the specific configuration it needs to function, keeping its internal logic focused on workload health rather than customer metadata.


4.


**Layered Security.** The control plane manages external authentication, access control, and auditing at the API surface. It then provides the operator with scoped credentials or policies to execute tasks locally within the cluster. This creates a secure, layered model where the control plane protects the perimeter and the operator protects the workload.


5.


**Unified Fleet Governance.** Use the control plane to define placement policies, global versioning, and environment constraints across multiple clusters. By offloading these fleet-wide responsibilities to the control plane, the operator can remain laser-focused on consistent application behavior within its local environment, regardless of the cloud or region.


## The Omnistrate Approach: Layering Control Planes Over Operator Models


Given that boundary, Omnistrate's operator guide models an operator-backed service plan with customer inputs, operator installation, runtime lifecycle workflows, and optional provider-defined custom workflows.\[6\]


In that model, System Workflows use an Argo Workflow-style structure with entrypoints, parameters, DAG tasks, and Kubernetes resource templates. Resource tasks can apply, patch, or delete Kubernetes resources and use successCondition and failureCondition checks against live resource status.\[4\]\[6\]\[8\]\[9\]


Use System Workflows for platform lifecycle operations that are part of the service API: create, modify, delete, start, stop, scale, backup, restore, and delete-backup. A system workflow should translate validated platform inputs into Kubernetes resources, wait on explicit status conditions, and return selected outputs to the platform record.\[6\]


Use Custom Workflows for provider-defined operations that are not standard lifecycle APIs, such as diagnostics, compaction, repair, credential rotation, model reloads, or vector-index rebuilds. Keep each workflow narrow: declare the inputs, target resource mutation, completion criteria, and returned outputs.\[6\]


Implementation-wise, a custom workflow utilizes the same internal engine as a system workflow. To implement one: define the workflow under CustomWorkflows.<operationName>.workflow, bind parameters via arguments.parameters, and use resource tasks to manage Kubernetes state. Crucially, adding this operation to the Supported Operations list in your service plan configuration automatically surfaces it in the Customer Portal, making it immediately available for end customers or platform operators to invoke on a running deployment instance.


The format used here is an Argo-style workflow embedded in the Omnistrate service-plan specification. Key components include:


- entrypoint: Selects the root template for the workflow.
- arguments.parameters: Binds platform variables such as $sys. *, $var.* , $secret. *, and $func.* .
- templates\[\].dag.tasks: Declares task execution order and dependencies.
- templates\[\].resource: Performs Kubernetes operations (apply, patch, or delete).
- successCondition and failureCondition: Evaluate live resource fields to determine task outcomes.
- outputParameters: Maps completed task values back into platform-visible metadata.\[6\]\[8\]\[9\]


### System Workflows Example: Orchestrating Cluster Backups


```text
systemWorkflows:
backup:
# Maps internal task status back to the platform record
outputParameters:
backupName:    "$tasks.create-backup.resource.status.backupName"
workflow:
entrypoint:    backup
arguments:
parameters:
# $sys variables are provided by the Omnistrate control plane
-    name:    namespace
value:    " {{ $sys.namespace }}  "
-    name:    instanceId
value:    " {{ $sys.instanceId }}  "
templates:
-    name:    backup
dag:
tasks:
-    name:    create-backup
template:    create-backup-resource
-    name:    create-backup-resource
inputs:
parameters:
-    name:    namespace
-    name:    instanceId
resource:
action:    apply
# Wait for the Operator to mark the backup as completed
successCondition:    status.phase    ==    completed
failureCondition:    status.phase    ==    failed
manifest:    |
apiVersion: postgresql.cnpg.io/v1
kind: Backup
metadata:
name: "{{inputs.parameters.instanceId}}-backup"
namespace: "{{inputs.parameters.namespace}}"
spec:
cluster:
name: "{{inputs.parameters.instanceId}}"


```


For implementation details, start with[Build from Kubernetes Operators](https://docs.omnistrate.com/getting-started/build-from-operators/) for the quick path and[Build with Kubernetes Operators](https://docs.omnistrate.com/build-guides/operators/) for the full service plan model.


## Real-World Example: Transforming CloudNativePG Operator into PostgreSQL Managed Service


Now, let’s take a simplified example of the CloudNativePG operator and how things flow end to end. The CloudNativePG (CNPG) example makes the boundary concrete. CNPG exposes PostgreSQL lifecycle through Kubernetes resources. The platform task is not to rewrite CNPG; it is to map service inputs and lifecycle operations into CNPG resources and status checks.\[5\]


For a public reference, see the[CloudNativePG Operator Service Plan Template](https://github.com/omnistrate-community/operator-spec-template) . Its spec.yaml shows how a PostgreSQL service can use CloudNativePG, the Barman Cloud plugin, Omnistrate backup configuration, and Argo-style systemWorkflows.\[7\]


### Step 1: Define the Product Inputs


Start with the API surface. The control plane should expose service-level inputs that are meaningful to the Customer, SRE, or platform operator. It should not require the caller to write raw Kubernetes manifests for routine operations:


- PostgreSQL version
- Storage size
- Replica count
- CPU and memory profile
- Backup schedule and retention
- Cloud provider, region, account, and deployment model


### Step 2: Map Product Inputs to the Operator Resource


Next, define the rendering boundary. Service-level inputs become operator-facing fields. For example, replicaCount becomes CNPG spec.instances, storageGiB becomes spec.storage.size, and postgresImage becomes spec.imageName.\[5\]\[6\]


#### Configuration Example: Mapping Product Inputs


```text
apiVersion:    postgresql.cnpg.io/v1
kind:    Cluster
metadata:
name:    customer-postgres
spec:
# The platform variable ${replicaCount} is substituted during deployment
instances:    ${replicaCount}
imageName:    ${postgresImage}
storage:
# Storage is dynamically sized based on customer input
size:    ${storageGiB}Gi
bootstrap:
initdb:
database:    app
owner:    app


```


### Step 3: Define the Operation Contract


Start by defining the operation contract. Using Backup as an example, this pattern decouples platform-level validation from workload-specific reconciliation:


Phase Actor Responsibility


Initiation User Requests the operation (e.g., via API or Portal).


Validation Control Plane Validates permissions, policy, and instance state.


Orchestration Control Plane Triggers the workflow and monitors the operator.


Reconciliation Operator Executes the workload-specific logic (the "how").


Finalization Control Plane Records operation metadata and confirms status. The metadata can be used later for restore operations.


#### Example: The Backup Implementation


The resulting contract—between the platform request and the operator resource—looks like this:


```text
apiVersion:    postgresql.cnpg.io/v1
kind:    Backup
metadata:
# requestId ensures each backup resource has a unique name in the cluster
name:    customer-postgres-backup-${requestId}
spec:
cluster:
name:    customer-postgres


```


The caller receives a platform-validated record without ever needing direct Kubernetes access. You can now apply this same pattern to restore, failover, credential rotation, or maintenance operations.


## Conclusion: Future-Proofing Architecture Against the "Fat Operator" Anti-Pattern


Separating control-plane orchestration from operator-based reconciliation creates a more composable architecture. The operator remains focused on domain-specific responsibilities such as CRD management, state reconciliation, workload topology, and application health. The Enterprise Control Plane manages the broader service lifecycle, including infrastructure, deployment workflows, governance, observability, metering, billing, and operational access.


This separation delivers three key architectural benefits:


1.


**Reuse:** A contract-based integration model allows teams to onboard new services without rebuilding the surrounding platform capabilities each time. Application-specific reconciliation can be defined once and combined with reusable infrastructure provisioning, lifecycle management, observability, governance, and deployment workflows across managed, BYOC, on-premises, and air-gapped environments.


2.


**Isolation of concerns:** Tenant management, metering, billing, governance, and service APIs remain outside the reconciliation loop. This avoids the “fat operator” anti-pattern, keeps reconciliation focused on application correctness, and prevents failures in peripheral systems from affecting the reliability of the application controller.


3.


**Portability:** Operators remain standard Kubernetes controllers built around CRDs and reconciliation loops, preserving portability across Kubernetes environments. The control plane adds higher-level capabilities such as placement, infrastructure orchestration, versioning, governance, and fleet operations without forcing those concerns into every operator.


Together, Operators and Enterprise Control Planes provide a clean separation between application-specific logic and service-delivery concerns. This allows each layer to evolve independently while giving teams the operational rigor required to deliver and manage production services across deployment models, environments, and customers.


## References


\[1\] Kubernetes documentation, Operator pattern.[https://kubernetes.io/docs/concepts/extend-kubernetes/operator/](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)


\[2\] Kubernetes documentation, Custom Resources.[https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)


\[3\] Kubernetes documentation, Controllers.[https://kubernetes.io/docs/concepts/architecture/controller/](https://kubernetes.io/docs/concepts/architecture/controller/)


\[4\] Argo Workflows documentation.[https://argo-workflows.readthedocs.io/en/latest/](https://argo-workflows.readthedocs.io/en/latest/)


\[5\] CloudNativePG sample manifests.[https://cloudnative-pg.io/documentation/current/samples/](https://cloudnative-pg.io/documentation/current/samples/)


\[6\] Omnistrate documentation, Build with Kubernetes Operators.[https://docs.omnistrate.com/build-guides/operators/](https://docs.omnistrate.com/build-guides/operators/)


\[7\] Omnistrate community operator spec template.[https://github.com/omnistrate-community/operator-spec-template/blob/main/spec.yaml](https://github.com/omnistrate-community/operator-spec-template/blob/main/spec.yaml)


\[8\] Argo Workflows documentation, DAG templates.[https://argo-workflows.readthedocs.io/en/latest/walk-through/dag/](https://argo-workflows.readthedocs.io/en/latest/walk-through/dag/)


\[9\] Argo Workflows documentation, Kubernetes resource templates.[https://argo-workflows.readthedocs.io/en/latest/walk-through/kubernetes-resources/](https://argo-workflows.readthedocs.io/en/latest/walk-through/kubernetes-resources/)
