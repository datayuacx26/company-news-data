---
schema_version: "1.0.0"
document_id: "4d96147e9a714ca89293fb7eccce62a70487a10f7af807921627593d4d918c16"
company_key: "vtex-class-a-common-shares"
company: "VTEX"
source_id: "vtex-class-a-common-shares-rss-fffff04a1e70"
canonical_url: "https://vtextech.substack.com/p/monitoring-windows-pods-with-prometheus"
published_at: "2024-02-09T11:24:00+00:00"
first_seen_at: "2026-07-20T23:19:02.969240+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:92c4e4775466067da96f4a1a8c01261bdf7b883c89ce3694027c96be071a826d"
---

# Monitoring Windows pods with Prometheus and Grafana

# Monitoring Windows pods with Prometheus and Grafana


[VTEX Tech Blog](https://substack.com/@vtextech)


and[Cezar Guimaraes](https://substack.com/@cezarguimaraes)


Feb 09, 2024


## Introduction


Customers across the globe are increasingly adopting


[Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/eks/) to run their Windows workloads. This is a result of customers figuring out that refactoring existing Windows-based applications into an open-source environment, while ideal, is a very complex task. It needs investments that usually don’t immediately translate into cost savings, and investing in this application refactoring isn’t in the best interest for the IT yearly budget. However, re-platforming the existing yet critical Windows-based applications into Windows containers makes sense from a cost-saving and modernization lens.


Tools such as


[App2Container (A2C)](https://aws.amazon.com/app2container/) have made application re-platforming easy. However, for successful day two operations, customers should consider certain infra-transformations, such as logging, monitoring, tracing, etc. As part of achieving full Windows containers observability on AWS, in 2022 we published a


[Containers post](https://aws.amazon.com/blogs/containers/centralized-logging-for-windows-containers-on-amazon-eks-using-fluent-bit/) on how customers can use an AWS-managed Windows fluent-bit container image to centralize Windows pods log in different destinations.


Prometheus and Grafana are some of the most popular monitoring stacks for Kubernetes-based workloads. Therefore, today we are launching a post focusing on how customers can centralize Windows pod metrics using


[Amazon Managed Service for Prometheus](https://aws.amazon.com/prometheus/) and


[Amazon Managed Grafana.](https://aws.amazon.com/grafana/)


## Solution overview


This post walks you through how to set up


[Windows Exporter](https://github.com/prometheus-community/windows_exporter) (A Prometheus exporter for Windows) as a Kubernetes daemonset and a PromQL (Prometheus Query Language) to enrich windows-exporter container metrics while merging with


[kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) (KSM). This lets you extend existing Linux-based Kubernetes monitoring to support Windows-based workloads.


*Figure 1. Solution workflow*


1.


Amazon Managed Service for Prometheus scrapes Windows node/container metrics, such as CPU, Memory, Disk, and Network usage from the Windows Exporter HostProcess DaemonSet.


2.


Amazon Managed Service for Prometheus scrapes KSM to map pod and container names to their container ID.


3.


Amazon Managed Grafana provides the ability to create monitoring dashboards from the collected metrics using Amazon Managed Service for Prometheus as the data source.


## Prerequisites


The following prerequisites are required to continue with this post:


-


An Amazon EKS cluster with Windows nodes up and running. See this


[step-by-step](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)


-


Amazon Managed Service for Prometheus with Amazon EKS ingestion properly setup. See this


[step-by-step](https://docs.aws.amazon.com/prometheus/latest/userguide/AMP-collector-how-to.html)


-


Amazon Managed Grafana fully integrated with Amazon Managed Service for Prometheus. See this


[step-by-step](https://docs.aws.amazon.com/prometheus/latest/userguide/AMP-amg.html)


This post’s prerequisites use AWS-managed services such as Amazon Managed Service for Prometheus with managed-collector and Amazon Managed Grafana. However, this post also applies to self-managed Prometheus, Grafana, and ADOT/Prom-server agents.


## Walkthrough


The following steps walk you through the steps described previously.


### 1. Install KSM


We now install KSM, a simple service that listens to the Kubernetes API server and generates metrics about the state of the objects. We must collect KSM to map pod and container names to their container ID.


1.1 Enter the following command to install KSM:


```text
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts


helm install kube-state-metrics prometheus-community/kube-state-metrics -n kube-system


```


### 2. Create a Windows Exporter daemonset


First, going deep into the daemonset configuration, we are setting up the


[securityContext](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.29/#windowssecuritycontextoptions-v1-core) to hostProcess:True. This means the container process has access to the host network namespace, storage, and devices, allowing us to fetch metrics for all the containers running at the host by listening to built-in Windows metrics.


The second part is the initContainer, where we set up the host firewall to allow TCP/9182 incoming traffic so that Amazon Managed Service for Prometheus can scrape the host. In the third part, we create a ConfigMap to inject


[windows-exporter](https://github.com/prometheus-community/windows_exporter) configurations and mount it to the Windows-exporter pod.


2.1 Create a file containing the following code and save it as


` windows-exporter.yaml` :


If you have any taints in the Windows nodes, then make sure you add the tolerations in the Daemonset configuration.


```text
kind: Namespace
apiVersion: v1
metadata:
name: windows-monitoring
labels:
name: windows-monitoring
---
kind: DaemonSet
apiVersion: apps/v1
metadata:
name: windows-exporter
namespace: windows-monitoring
labels:
app: windows-exporter
spec:
selector:
matchLabels:
app: windows-exporter
template:
metadata:
labels:
app: windows-exporter
annotations:
prometheus.io/scrape: "true"
prometheus.io/scheme: http
prometheus.io/path: "/metrics"
prometheus.io/port: "9182"
spec:
securityContext:
windowsOptions:
hostProcess: true
runAsUserName: "NT AUTHORITY\\system"
hostNetwork: true
initContainers:
- name: configure-firewall
image: mcr.microsoft.com/powershell:lts-nanoserver-1809
command: ["powershell"]
args: ["New-NetFirewallRule", "-DisplayName", "'windows-exporter'", "-Direction", "inbound", "-Profile", "Any", "-Action", "Allow", "-LocalPort", "9182", "-Protocol", "TCP"]
containers:
- args:
- --config.file=%CONTAINER_SANDBOX_MOUNT_POINT%/config.yml
name: windows-exporter
image: ghcr.io/prometheus-community/windows-exporter:latest
imagePullPolicy: Always
ports:
- containerPort: 9182
hostPort: 9182
name: http
volumeMounts:
- name:  windows-exporter-config
mountPath: /config.yml
subPath: config.yml
nodeSelector:
kubernetes.io/os: windows
volumes:
- name: windows-exporter-config
configMap:
name: windows-exporter-config
---
kind: ConfigMap
apiVersion: v1
metadata:
name: windows-exporter-config
namespace: windows-monitoring
labels:
app: windows-exporter
data:
config.yml: |
collectors:
enabled: '[defaults],container'
collector:
service:
services-where: "Name='containerd' or Name='kubelet'"


```


This solution uses a public, open-source Prometheus container image. It is your responsibility to perform security due diligence.


2.2 Create the Kubernetes Namespace, Daemonset and ConfigMap. Enter the following command:


```text
kubectl create -f windows-exporter.yaml
```


2.3 Check if the Daemonset pods are running. Enter the following command:


```text
kubectl get pods -n windows-monitoring
```


2.4 Once the pods are in the running status, you can check if they are accepting connections on port 9182. Enter the following command:


```text
kubectl logs windows-exporter-pod-name -n windows-monitoring
```


2.5 You should see the windows-exporter pod listening on port 9182, which is the one that is scrapped by Amazon Managed Service for Prometheus.


```text
ts=2024-01-30T00:03:22.226Z caller=tls_config.go:313 level=info msg="Listening on" address=[::]:9182
```


### 3. Visualizing Windows pods metrics in Amazon Managed Grafana


Assuming you already have Grafana knowledge, you can create panels that are relevant for your day two operation. In the following, you can find PromQL queries that automatically bring the correct data scrapped by Prometheus, merging Windows container metrics and mapping to its pod. We are setting the query to populate new data every two minutes.


Make sure you are selecting the right data source when creating panels. In this post, we are using Amazon Managed Service for Prometheus as a data source.


Check the


**[Windows Exporter GitHub repository](https://github.com/prometheus-community/windows_exporter/blob/master/docs/collector.container.md)** for a complete list of exported Windows container metrics.


For example, in the following query, we filter the total CPU usage percentage per second at the pod level. To do so, you need to create a


**custom legend** with the value pod. Furthermore, it is essential to set the Units in the panel to the ones in the following table.


Figure 2 - Grafana query panel


The milliCPU query generates the following panel:


*Figure 3 - Windows Pods – milliCPU*


The CPU Query measures Kubernetes CPU Unit usage per second multiplied by 1000 to match Kubernetes milliCPUss. This allows you to quickly and easily identify if a pod needs CPU limits/request right-sizing. A CPU second refers to


**one second** on a CPU. This is the amount of time in seconds your CPU spends actively running a process, as opposed to the elapsed time.


### 4. Visualizing Windows nodes metrics in Amazon Managed Grafana


Nonetheless, visualizing Windows nodes metrics is crucial as Windows pods metrics. In the following table, you can find PromQL queries that automatically bring the correct data scrapped by Prometheus per Windows nodes. We are setting the query to populate new data every two minutes.


Check the


[Windows Exporter GitHub repository](https://github.com/prometheus-community/windows_exporter) for a complete list of Windows nodes metrics exported.


For example, in the following query, we are filtering the total CPU usage percentage per second at the pod level. To do so, you must create a


**custom legend** with the value node. Furthermore, it is essential to set the Units in the panel to the ones in the preceding table.


*Figure 4 - Grafana query panel*


The Memory query generates the following panel:


*Figure 5 - Windows nodes memory percent usage panel*


## Conclusion


This post covered how to successfully deploy Windows Exporter as a daemonset using a hostProcess container mode. Then, we covered which Windows and KSM should be used to have a proper Grafana monitoring dashboard. You can also use these metrics to create additional panels to an existing Grafana dashboard, such as when an Amazon EKS with a mixed data plane is deployed.


In addition, see the best practices for running Windows containers on Amazon EKS in the Amazon


[EKS Best Practice guide](https://aws.github.io/aws-eks-best-practices/) .


> *This article was produced in collaboration with AWS. **[Original post](https://aws.amazon.com/blogs/containers/monitoring-windows-pods-with-prometheus-and-grafana/)** co-written with Alberto Frocht*


Thanks for reading VTEX’s Tech Blog! Subscribe for free to receive new posts.


A guest post by


[Cezar Guimaraes](https://substack.com/@cezarguimaraes?utm_campaign=guest_post_bio&utm_medium=web)


Kubernetes and rhythm games enthusiast, working on delivering the best developer experience @ VTEX


[Subscribe to Cezar](https://cezarguimaraes.substack.com/subscribe?)
