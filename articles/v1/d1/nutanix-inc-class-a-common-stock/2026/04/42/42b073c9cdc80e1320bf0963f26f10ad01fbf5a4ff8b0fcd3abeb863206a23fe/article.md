---
schema_version: "1.0.0"
document_id: "42b073c9cdc80e1320bf0963f26f10ad01fbf5a4ff8b0fcd3abeb863206a23fe"
company_key: "nutanix-inc-class-a-common-stock"
company: "Nutanix Inc."
source_id: "nutanix-inc-class-a-common-stock-rss-12a2d78c04c7"
canonical_url: "https://www.nutanix.dev/2026/04/22/part-1-scale-from-zero-node-pools-in-nkp-how-it-works-and-how-to-configure-it-2/"
published_at: "2026-04-22T14:00:00+00:00"
first_seen_at: "2026-07-20T03:31:13.386524+00:00"
fetched_at: "2026-08-20T00:46:45.571645+00:00"
content_hash: "sha256:bf72c3c3d2521a99d22a863c59fbb49445be79e886bea75ba0d84ed5bc562098"
---

# Part 2 – Scale From Zero Node Pools in NKP: Testing and Validating the Setup

## Introduction


In **[Part 1 of this series](https://www.nutanix.dev/2026/04/15/part-1-scale-from-zero-node-pools-in-nkp-how-it-works-and-how-to-configure-it/)** , we explained how **Scale From Zero works in NKP** , including how Cluster Autoscaler evaluates empty node pools and how to configure the required capacity annotations.


If you missed it, it’s worth reading **[Part 1 first](https://www.nutanix.dev/2026/04/15/part-1-scale-from-zero-node-pools-in-nkp-how-it-works-and-how-to-configure-it/)** , since it covers the concepts and configuration that enable scaling from zero nodes.


In this post, we focus on **testing and validation** . We will trigger real scaling scenarios, observe how the autoscaler reacts when node pools start at zero, and verify that workloads correctly cause new nodes to be provisioned.


By the end, you’ll have a repeatable approach for confirming that your **Scale From Zero configuration works as expected in practice** .


---


## Deploy a Workload That Targets the Scale from Zero Pool


To see the behavior of Cluster Autoscaler with a scale from zero node pool, we are going to test this in three steps:


- Force an unscheduled workload. By only setting the label and not the taint
- Add the taint to trigger the worker node creation (from 0 → 1 node replica)
- Delete the workload to decommission the worker (from 1 → 0 node replica)


### **Unscheduled workload (label only)**


The workload must have the label` node-role.kubernetes.io/worker` defined. Otherwise, without the label, it will be scheduled to the default NKP node pool,` md-0` .


The following manifest is a single replica deployment asking for a node with the label` node-role.kubernetes.io/worker: <your_nodepool_name_value>`


> **Note** : you must complete the steps in part 1 before running the following command, otherwise the environment variable` $NODEPOOL_NAME` won’t be set


```text
kubectl apply -f -<<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
name: burst-workload
namespace: default
spec:
replicas: 1
selector:
matchLabels:
app: burst
template:
metadata:
labels:
app: burst
spec:
nodeSelector:
node-role.kubernetes.io/worker: $NODEPOOL_NAME
containers:
- name: app
image: ghcr.io/traefik/whoami:v1.11
resources:
requests:
cpu: "1"
memory: "512Mi"
EOF
```


When this` Deployment` is created:


- Pod is unschedulable
- Autoscaler will check` MachineDeployments` that meet the criteria
- Autoscaler won’t scale from` 0 → 1` because no matches
- No node is provisioned
- Pod remains unscheduled


Check the status of the pod to confirm it is` Pending` :


```text
kubectl get pod -l app=burst --namespace default
```


You should see a similar output:


```text
NAME                              READY   STATUS    RESTARTS   AGE
burst-workload-6f4c8ffd7c-jcxdm   1/1     Pending   0          53s
```


You can also check` MachineDeployment` to confirm that no nodes have been created yet.


```text
kubectl get machinedeployment --namespace $NAMESPACE
```


### Scheduled workload (label & taint required)


Let’s update the *Deployment* to match the tolerations to the taint.


```text
kubectl apply -f -<<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
name: burst-workload
namespace: default
spec:
replicas: 1
selector:
matchLabels:
app: burst
template:
metadata:
labels:
app: burst
spec:
nodeSelector:
node-role.kubernetes.io/worker: $NODEPOOL_NAME
tolerations:
- key: "dedicated"
operator: "Equal"
value: "${NODEPOOL_NAME}"
effect: "NoSchedule"
containers:
- name: app
image: ghcr.io/traefik/whoami:v1.11
resources:
requests:
cpu: "1"
memory: "512Mi"
EOF
```


When the Deployment is updated:


- Pod is still unscheduled
- Autoscaler scales` MachineDeployment` from` 0 → 1` (it might take a few minutes)


- In Prism Central, you can check for incoming tasks of a new virtual machine creation


- Node is provisioned
- Pod schedules


Check back the` MachineDeployment` to confirm it is` ScalingUp` .


```text
kubectl get machinedeployment --namespace $NAMESPACE
```


Once the worker node is ready, the pod should be in` Running` status.


```text
kubectl get pod -l app=burst --namespace default
```


You should see a similar output:


```text
NAME                             READY   STATUS    RESTARTS   AGE
burst-workload-bf6997679-42lm5   1/1     Running   0          3m52s
```


### Scaling back to zero


The last step would be to scale the` Deployment` down to zero replicas or completely delete it. As a best practice, we’ll keep the` Deployment` and just set the number of replicas to zero.


```text
kubectl scale --replicas=0 deployment/burst-workload --namespacec default
```


Confirm that the` Deployment` shows` 0/0` pods.


```text
kubectl get deployment burst-workload --namespacec default
```


When the` Deployment` is scaled down to zero:


- Node becomes unneeded
- After cooldown timers (default: 10 minutes)
- Machine is drained
- Replicas return to 0


After 10 minutes, check the` MachineDeployment` to confirm the node was deleted. If it isn’t deleted, check that you don’t have any workloads with labels and taints that request this node pool.


---


## Important Production Considerations


### Autoscaler Timers


Scale-down is not immediate. Expect delays controlled by flags in Cluster Autoscaler, such as:


- ` --scale-down-delay-after-add` (default: 10 minutes)
- –` -scale-down-unneeded-time` (default: 10 minutes)


This is intentional and prevents aggressive churn. NKP uses the default values.


---


### MachineHealthCheck (MHC) Interaction


If` MachineHealthCheck` is enabled:


- Ensure` nodeStartupTimeout` is not too aggressive (default: 10 minutes)
- Avoid remediation loops for ephemeral pools
- Consider excluding scale-from-zero pools from` MHC` if necessary


---


### DaemonSets Do Not Block Scale-Down


System` DaemonSets` (CNI, CSI, etc.) are ignored during scale-down evaluation. They will not prevent a node from being removed.


---


## When NOT to Use Scale From Zero


Scale from zero node pools is not a disaster recovery strategy.


In a DR architecture, the best practice is to:


- Maintain at least one active instance
- Continuously validate data replication
- Route a portion of production or synthetic traffic
- Exercise networking and storage paths


If your DR site runs at zero capacity and you fail over during an incident, you risk discovering at the worst possible moment:


- Storage attachment issues
- Secret replication failures
- Certificate expiration
- Network misconfiguration
- Image pull failures


Scale-from-zero is ideal for:


- Batch jobs
- On-demand compute
- CI pipelines
- Ephemeral workloads
- GPU burst capacity


It is not a substitute for a warm DR environment.


---


## Final Thoughts


Scale from zero node pools is one of the most effective infrastructure optimizations available in Kubernetes today. It allows you to:


- Align infrastructure cost with real demand
- Eliminate idle compute waste
- Maintain clean workload isolation
- Deliver reactive platform engineering


Used correctly, it reduces both cost and operational sprawl. Used carelessly, it can introduce reliability risk in critical systems.


The key is understanding where elasticity provides value — and where readiness must remain constant.
