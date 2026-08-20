---
schema_version: "1.0.0"
document_id: "aba731343efad1910474b86b02b0b7fd0fcc3161327be492eca5a89fc3214d93"
company_key: "yc-brainboard"
company: "Brainboard"
source_id: "yc-brainboard-news-import-c1cb64b4ba70"
canonical_url: "https://www.brainboard.co/blog/what-is-a-kubernetes-hpa"
published_at: "2026-04-09T00:00:00+00:00"
first_seen_at: "2026-07-23T04:03:09.323194+00:00"
fetched_at: "2026-07-28T22:15:57.305389+00:00"
content_hash: "sha256:4a9102839c9b972de8474ae1bc7ed7d8ae1beb5c0c8040cc8ef94cd9d3a51526"
---

# What is a Kubernetes HPA?

There are times when the load on an application spikes suddenly. And at those moments, the last thing you want to do is manually add pods and monitor metrics. Fortunately, there is Kubernetes HPA (Horizontal Pod Autoscaler), which solves this problem automatically. It monitors the current load and independently adjusts the number of running pods. More traffic means more pods; when the load drops, the number of pods decreases. This is the foundation for scalable, fault-tolerant applications in Kubernetes.


## What Is Kubernetes Horizontal Pod Autoscaler


Kubernetes Horizontal Pod Autoscaler is a built-in Kubernetes mechanism that automatically adjusts the number of pod replicas in a Deployment, ReplicaSet, or StatefulSet based on observed metrics.


Simply put, you set the rules - how many pods should run at what load - and the HPA ensures they are followed without your intervention.


In the Kubernetes architecture, the Kubernetes Horizontal Pod Autoscaler exists as a separate controller that operates in a cycle: it collects metrics, compares them to target values, and decides whether to change the number of replicas. This cycle repeats every 15 seconds by default.


It’s important to understand how HPA differs from vertical scaling (VPA). VPA increases the resources of a single pod - CPU and memory. HPA, on the other hand, adds new pods to distribute the load horizontally. This makes it an ideal tool for stateless applications that scale well horizontally: web servers, APIs, microservices.


Kubernetes Horizontal Pod Autoscaler has been supported by Kubernetes since version 1.2, and more advanced features with custom metrics were introduced in version 1.6+.


## How Kubernetes HPA Works


Let’s break down the mechanics using a specific K8s HPA example. Suppose you have an API service running with 2 pods, and you’ve configured the HPA with a target CPU utilization of 50%.


Here’s what happens step by step:


1. **Metrics collection.** The Metrics Server collects CPU usage data for each pod every 15 seconds.
2. **Evaluation.** The HPA controller looks at the average CPU load across all pods. If it reaches 80% with a target of 50%, it’s time to scale up.
3. **Calculating replicas.** HPA calculates the required number of pods using the formula: current replicas × (current metric / target metric). In our case: 2 × (80 / 50) = 3.2 - rounded up to 4 pods.
4. **Scaling up.** Kubernetes launches new pods, the load is distributed, and CPU usage returns to normal.
5. **Scaling down.** When traffic drops, and metrics remain below the threshold for several minutes, the HPA reduces the number of pods.


This is the basic K8s HPA example in action: no manual intervention, the system adapts on its own. All you have to do is enjoy the result.


## Key Metrics Used by Kubernetes HPA


Kubernetes HPA metrics fall into three categories, and choosing the right one is half the battle in configuring autoscaling:


- **CPU and memory (Resource Metrics).** The most common metrics. CPU is particularly well-suited for scaling: as load increases - the CPU gets busier - the HPA adds pods. Memory is used less frequently because it is less predictable: pods can consume a lot of memory even at low load due to caches.
- **Custom Metrics.** These are the next level of Kubernetes HPA metrics. You can scale based on the number of requests per second, message queue length, the number of active connections - any metric that is specifically important for your application. This requires the Custom Metrics API and an adapter (such as the Prometheus Adapter).
- **External Metrics.** These allow you to scale based on data from external systems - such as cloud queues or external monitoring tools. For example, if an SQS queue is growing, the HPA adds worker pods to process it.


The choice of metric directly affects the quality of scaling. An incorrectly chosen metric causes the HPA to react too late or too early.


## How to Check HPA Status Using kubectl


To view the current status of the autoscaler, run ‘kubectl get hpa’ command. The output will look something like this:


```text
NAME          REFERENCE        TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
api-service   Deployment/api   48%/50%   2         10        3          5d
```


Here you can see all the most important information: which Deployment the HPA is attached to, the current metric value vs. the target (48% of 50%), the minimum and maximum number of pods, and how many replicas are currently running.


For more detailed information, use: kubectl describe hpa api-service. This command will show the scaling event history, the reasons for recent changes, and the current conditions. It is indispensable for debugging when the HPA behaves unexpectedly.


## Understanding HPA Scaling Behavior


Understanding how the HPA makes decisions helps avoid unstable or unexpected behavior.


**Thresholds and cooldown.** HPA does not react to every metric fluctuation. Scaling up happens quickly - by default, after just 3 minutes of consistently exceeding the threshold. Scaling down is slower, with a 5-minute delay. This protects against situations where pods are constantly being added and removed due to short-term spikes.


**The ‘kubectl get HPA’ command** , when used in conjunction with ‘describe’, allows you to track these specific events: when scaling last triggered and for what reason.


**Limits.** You always specify minReplicas and maxReplicas. The HPA will never exceed these limits, even if metrics require more. This is an important lever for controlling costs and stability.


In Kubernetes 1.18+, it became possible to fine-tune behavior via the ‘behavior’ field in the HPA specification - separately for scale-up and scale-down. This allows, for example, aggressive scaling up and smooth scaling down.


## Horizontal Pod Autoscaler YAML Configuration


The basic horizontal pod autoscaler YAML looks like this:


```text
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
name: api-service-hpa
spec:
scaleTargetRef:
apiVersion: apps/v1
kind: Deployment
name: api-service
minReplicas: 2
maxReplicas: 10
metrics:
- type: Resource
resource:
name: cpu
target:
type: Utilization
averageUtilization: 50
```


The scaleTargetRef field specifies which Deployment will be managed. minReplicas and maxReplicas set the upper and lower bounds for the HPA scale. The metrics field specifies which metric is used to make decisions and the target value. This minimal configuration is already fully functional and covers most basic scenarios.


## Benefits of Using Kubernetes HPA


Why has Kubernetes HPA become the standard for production environments? There are several compelling reasons.


- **Automatic adaptation to load.** The application handles traffic spikes on its own - without an engineer on duty or manual intervention. This is especially valuable for services with unpredictable traffic, such as e-commerce during sales and media services during live broadcasts.
- **Infrastructure cost savings.** During periods of low load, HPA reduces the number of pods - you pay only for what you actually use. For cloud environments, this translates to direct budget savings.
- **Improved reliability.** When the load is distributed across a larger number of pods, each one operates within its optimal range. This reduces the risk of overload failure.
- **Less manual work.** The operations team is freed from constant monitoring and routine scaling tasks - allowing them to focus on more important matters.


If you’re designing a cloud infrastructure and want to see the entire stack - from Kubernetes configurations to network dependencies - in a single visual space,[Brainboard](https://www.brainboard.co/) will help you build a clear architecture with automatic Terraform code generation.


## Common Challenges and Limitations of HPA


Kubernetes HPA is a powerful tool. But nothing is perfect, and it also has weaknesses that are important to know in advance:


- **Response delay.** HPA is not instantaneous. There is a time lag between a load increase and the appearance of new pods: metric collection, evaluation, pod launch, and its readiness to accept traffic. For applications with sharp and short spikes, this can be critical.
- **Dependence on metric accuracy.** If the Metrics Server is unstable or the horizontal pod autoscaler YAML is configured with incorrect target values, the HPA will scale incorrectly. A CPU threshold that is too low will lead to constant pod growth; one that is too high will lead to overload.
- **Limitations for stateful applications.** Databases and other stateful services are more difficult to scale horizontally - here, HPA performs worse or is not suitable at all.


**Practical recommendations:** Start with conservative thresholds and adjust them based on the application’s actual behavior. Test load scenarios before production. Set up alerts in case HPA reaches maxReplicas - this is a signal that limits need to be reviewed.


[Brainboard](https://www.brainboard.co/use-cases) helps systematically build such processes - a platform where infrastructure decisions are made with full context and built-in security checks.


### Why Kubernetes HPA Matters for Modern Apps


Kubernetes HPA is not an optional feature, but an essential element of any production environment with dynamic load. It allows applications to be both cost-effective during quiet periods and resilient during peak times - exactly what modern teams need.
