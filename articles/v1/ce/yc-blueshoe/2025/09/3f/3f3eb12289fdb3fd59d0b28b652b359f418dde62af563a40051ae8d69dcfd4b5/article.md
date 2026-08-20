---
schema_version: "1.0.0"
document_id: "3f3eb12289fdb3fd59d0b28b652b359f418dde62af563a40051ae8d69dcfd4b5"
company_key: "yc-blueshoe"
company: "Blueshoe"
source_id: "yc-blueshoe-news-import-28975b8749e8"
canonical_url: "https://www.blueshoe.io/blog/alternatives-to-django-celery-in-kubernetes/"
published_at: "2025-09-10T10:00:00+00:00"
first_seen_at: "2026-07-21T10:56:59.304644+00:00"
fetched_at: "2026-07-28T22:01:02.064378+00:00"
content_hash: "sha256:2ed010afc1e9538fd1f588f2a4075adc2197ce0f507ba6db0642b5ed26b4d358"
---

# Alternatives to Celery for Django on Kubernetes | BLUESHOE

## Table of Contents


- The Four Options in a Nutshell
- KEDA in 60 Seconds
- Comparison Specifically for Kubernetes
- Mini YAML Examples
- Kubernetes Operational Aspects That Matter
- Code Examples for Tasks
- Failure Handling in Kubernetes Clusters
- Decision Guide
- FAQ


---


## Meet the Author


[Jonathan Kölbl](https://www.blueshoe.io/team/jonathan-koelbl/) I think Celery is the undisputed number one for asynchronous jobs. That said, for new projects with less demanding requirements, alternatives like Django RQ are also worth considering.


Last updated:


2025-09-10


---


Django


Kubernetes


Development


Operations


---


2025-09-10


# Django Tasks on Kubernetes: Lean and Scalable


Alternatives to Celery for Django on Kubernetes


Are you planning background jobs in your cluster and wondering if there are lighter options than Celery?


Celery is powerful, but sometimes too heavyweight for simple setups. In this article, we'll look at alternatives like **Django RQ, Dramatiq, and Huey** , explain why **KEDA** plays a key role in autoscaling, and give you **mini-YAML examples** for clean deployment with probes and graceful shutdown.


## The Four Options in a Nutshell


*(With links to projects and repos so you can dive deeper.)*


### [Celery](https://docs.celeryq.dev/)


The classic with a large ecosystem. Supports workers and the separate scheduler, Beat. In large setups with chains and many integrations, Celery remains the reference point.


### [Django RQ](https://github.com/rq/django-rq)


A Django-friendly integration of RQ based on Redis. Workers start in the Django context. In many cases, just setting` DJANGO_SETTINGS_MODULE` is enough. Monitoring is easy via the[RQ Dashboard](https://github.com/rq/rq-dashboard) or admin integrations.


### [Dramatiq](https://dramatiq.io/)


Modern defaults with a focus on reliability. Runs with Redis or RabbitMQ. The Django integration[django_dramatiq](https://www.google.com/search?q=%5Bhttps://github.com/Bogdanp/django_dramatiq%5D(https://github.com/Bogdanp/django_dramatiq)) provides the management command` rundramatiq` .


### [Huey](https://huey.readthedocs.io/)


Lightweight with a built-in scheduler. Clean Django integration via the` run_huey` management command, including auto-discovery of` tasks.py` .


---


## KEDA in 60 Seconds


[KEDA](https://keda.sh/) is the Kubernetes Event-Driven Autoscaler. It scales deployments and jobs based on events like queue lengths and can scale down to zero when idle. KEDA complements the Horizontal Pod Autoscaler and works with it.


Why is KEDA so important?


- You **don't need polling** in the worker.
- Pods scale dynamically when there are jobs in the queue.
- When idle, you can **reduce workers to 0** and save resources.


Typical triggers for Django workers are **Redis Lists** and **RabbitMQ Queues** . Both are available as scalers.


---


## Comparison Specifically for Kubernetes


Tool Broker KEDA Trigger Advantages Disadvantages Best Choice When...


**Celery** Redis, RabbitMQ Redis List or RabbitMQ Queue **Very mature** , many integrations, worker plus Beat, large community. **K8s Plus:** many examples for queue-based autoscaling. More operational overhead, additional components like Beat, careful shutdown handling required. High load, complex chains, existing Celery experience.


**Django RQ** Redis Redis List **Very easy start** , admin integration, few moving parts. **K8s Plus:** list length as a simple KEDA trigger. Less feature depth, Redis is mandatory. Web projects with clear jobs and a fast go-live.


**Dramatiq** Redis, RabbitMQ Redis List or RabbitMQ Queue **Modern defaults** , robust retries, clean Django integration via` rundramatiq` . **K8s Plus:** easily combined with KEDA depending on the broker. Fewer ready-made Django UIs, learning curve for the actor model. Demanding but lean, focus on reliability.


**Huey** Redis Redis List **Lightweight** , scheduler included, consumer as a management command. **K8s Plus:** simple process and KEDA coupling. Smaller ecosystem, minimal monitoring. Few workers, many periodic tasks.


---


## Mini YAML Examples


### Example A: Celery Worker with RabbitMQ and KEDA


```text
# Deployment for Celery Worker
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   celery-worker
spec  :
selector  : {   matchLabels  : {   app  :   celery   } }
template  :
metadata  : {   labels  : {   app  :   celery   } }
spec  :
terminationGracePeriodSeconds  :   60
containers  :
-   name  :   worker
image  :   your-registry/app:latest
command  : [  "celery"  ,   "-A"  ,   "proj"  ,   "worker"  ,   "--loglevel=info"  ]
env  :
-   name  :   RABBITMQ_HOST
valueFrom  :
secretKeyRef  : {   name  :   rmq  ,   key  :   amqp_uri   }
---
# ScaledObject for RabbitMQ Queue
apiVersion  :   keda.sh/v1alpha1
kind  :   ScaledObject
metadata  :
name  :   celery-rabbit
spec  :
scaleTargetRef  : {   name  :   celery-worker   }
minReplicaCount  :   0
maxReplicaCount  :   20
triggers  :
-   type  :   rabbitmq
metadata  :
hostFromEnv  :   RABBITMQ_HOST
queueName  :   celery
protocol  :   amqp
mode  :   QueueLength
value  :   "20"
authenticationRef  :
name  :   rmq-auth


```


---


### Example B: Django RQ Worker plus KEDA Redis List Scaler


```text
# Deployment for RQ Worker
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   rq-worker
spec  :
selector  : {   matchLabels  : {   app  :   rq   } }
template  :
metadata  : {   labels  : {   app  :   rq   } }
spec  :
terminationGracePeriodSeconds  :   60
containers  :
-   name  :   worker
image  :   your-registry/app:latest
command  : [  "bash"  ,   "-lc"  ,   "DJANGO_SETTINGS_MODULE=config.settings rq worker default"  ]
env  :
-   name  :   REDIS_HOST
value  :   "redis:6379"
---
# KEDA ScaledObject with Redis List Trigger
apiVersion  :   keda.sh/v1alpha1
kind  :   ScaledObject
metadata  :
name  :   rq-scale
spec  :
scaleTargetRef  : {   name  :   rq-worker   }
minReplicaCount  :   0
maxReplicaCount  :   10
triggers  :
-   type  :   redis
metadata  :
addressFromEnv  :   REDIS_HOST
listName  :   default
listLength  :   "20"    # Scale up from 20 jobs


```


---


### Example C: Dramatiq under Django with Redis


```text
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   dramatiq-worker
spec  :
selector  : {   matchLabels  : {   app  :   dramatiq   } }
template  :
metadata  : {   labels  : {   app  :   dramatiq   } }
spec  :
terminationGracePeriodSeconds  :   60
containers  :
-   name  :   worker
image  :   your-registry/app:latest
command  : [  "python"  ,   "manage.py"  ,   "rundramatiq"  ,   "--processes"  ,   "2"  ,   "--threads"  ,   "8"  ]
env  :
-   name  :   REDIS_HOST
value  :   "redis:6379"
---
apiVersion  :   keda.sh/v1alpha1
kind  :   ScaledObject
metadata  :
name  :   dramatiq-scale
spec  :
scaleTargetRef  : {   name  :   dramatiq-worker   }
minReplicaCount  :   0
maxReplicaCount  :   10
triggers  :
-   type  :   redis
metadata  :
addressFromEnv  :   REDIS_HOST
listName  :   default
listLength  :   "10"


```


---


### Example D: Huey Worker with Redis


```text
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   huey-worker
spec  :
selector  : {   matchLabels  : {   app  :   huey   } }
template  :
metadata  : {   labels  : {   app  :   huey   } }
spec  :
terminationGracePeriodSeconds  :   60
containers  :
-   name  :   worker
image  :   your-registry/app:latest
command  : [  "python"  ,   "manage.py"  ,   "run_huey"  ]
env  :
-   name  :   REDIS_HOST
value  :   "redis:6379"
---
apiVersion  :   keda.sh/v1alpha1
kind  :   ScaledObject
metadata  :
name  :   huey-scale
spec  :
scaleTargetRef  : {   name  :   huey-worker   }
minReplicaCount  :   0
maxReplicaCount  :   5
triggers  :
-   type  :   redis
metadata  :
addressFromEnv  :   REDIS_HOST
listName  :   default
listLength  :   "5"


```


---


## Kubernetes Operational Aspects That Matter


### Deploying a Broker


You can get stable Redis and RabbitMQ via a[Helm Chart](https://artifacthub.io/) or an operator. For RabbitMQ, there is also an official[Cluster Operator](https://www.rabbitmq.com/kubernetes/operator/) .


### Configuring Probes


Set **Readiness, Liveness** , and, for longer startups, a **Startup Probe** . Example:


```text
livenessProbe  :
exec  : {   command  : [  "pgrep"  ,   "rq"  ] }
initialDelaySeconds  :   20
periodSeconds  :   10


```


This prevents deadlocks and traffic to pods that are not ready.


### Graceful Shutdown


Use` terminationGracePeriodSeconds` and, if necessary, a` preStop` hook so that running tasks can finish cleanly:


```text
lifecycle  :
preStop  :
exec  :
command  : [  "bash"  ,   "-c"  ,   "kill -TERM 1 && sleep 30"  ]


```


Kubernetes will terminate containers after the grace period expires, no matter what—so plan for a buffer.


### KEDA Installation


KEDA can be installed via a[Helm Chart](https://keda.sh/docs/) or YAML. After that, you define **ScaledObjects** or **ScaledJobs** for each worker.


---


## Code Examples for Tasks


So you can see the difference in the Django code as well:


### Celery


```text
from   celery   import   shared_task


@shared_task
def   send_email  (  user_id  ):
# classic Celery task
...


```


### Django RQ


```text
import   django_rq


def   send_email  (  user_id  ):
...


# Add a task to the queue
queue   =   django_rq.get_queue(  'default'  )
queue.enqueue(send_email, user.id)


```


### Dramatiq


```text
import   dramatiq


@dramatiq.actor
def   send_email  (  user_id  ):
...


# Dispatch the task
send_email.send(user.id)


```


### Huey


```text
from   huey.contrib.djhuey   import   task


@task  ()
def   send_email  (  user_id  ):
...


# Call the task
send_email(user.id)


```


---


## Failure Handling in Kubernetes Clusters


Sooner or later, it happens: a worker goes down or the broker fails. To ensure your tasks continue to run smoothly, you need some safeguards. Here's how you can proceed:


### When a Worker Crashes


Kubernetes automatically restores your pods with` restartPolicy: Always` . However, it's important that jobs are only removed from the queue once they are truly processed. Otherwise, tasks will be lost.


All four tools (Celery, RQ, Dramatiq, Huey) support built-in retries. You should actively use these.


An example of an RQ worker deployment:


```text
apiVersion  :   apps/v1
kind  :   Deployment
metadata  :
name  :   rq-worker
spec  :
replicas  :   2
selector  : {   matchLabels  : {   app  :   rq   } }
template  :
metadata  : {   labels  : {   app  :   rq   } }
spec  :
restartPolicy  :   Always
containers  :
-   name  :   worker
image  :   your-registry/app:latest
command  : [  "rq"  ,   "worker"  ,   "default"  ]


```


If a pod crashes, Kubernetes simply restarts it. The open jobs remain in Redis and are picked up by the next worker.


---


### When the Broker Fails


Nothing runs without Redis or RabbitMQ. That's why you should operate these components in a highly available manner.


#### Redis as a StatefulSet


A deployment alone is not enough if data needs to be stored permanently. A StatefulSet ensures that Redis pods have stable names and their own volume. This keeps the queue intact even after restarts.


In short: StatefulSets give pods a fixed identity and persistent storage.


#### RabbitMQ with Replicated Queues


RabbitMQ queues are normally tied to a single node. If that node or pod fails, the queue is gone. With Quorum Queues, which are replicated and fault-tolerant queues based on the Raft protocol, you are in a safer position. The RabbitMQ Kubernetes Operator makes it easier for you to set up a highly available cluster.


The advantage is clear: if one pod fails, another takes over without your workers noticing much.


---


### Kubernetes Tools You Shouldn't Forget


- **PodDisruptionBudget (PDB):** ensures that not too many pods fail at the same time during updates.
- **Readiness Probes:** ensure that only healthy pods process jobs.
- **Graceful Shutdown:** gives your workers time to finish tasks upon receiving a SIGTERM signal.
- **Backoff and Retries:** allow workers to automatically reconnect if the broker is temporarily unavailable.


---


With this setup, you are well-prepared for the most common sources of failure in the cluster, whether a worker or a broker fails.


## Decision Guide


- **Simple and fast with Redis:** Django RQ or Huey. Couple it with the KEDA Redis List Scaler.
- **Robust with a flexible choice of broker:** Dramatiq. Use Redis or RabbitMQ and connect KEDA accordingly.
- **Large setup with existing know-how:** Celery with ScaledObjects for each queue and optionally Beat or django-celery-beat for periodic tasks.


Migrations are possible: many task definitions can be ported with little effort, even if retry/ACK mechanisms are implemented differently.


---


## FAQ


### 1. What is KEDA and what do I use it for?


KEDA scales workloads based on external events like queue length and enables scale-to-zero. It complements the Horizontal Pod Autoscaler.


### 2. Can I autoscale all four tools with KEDA?


Yes. Via the Redis Lists Scaler or the RabbitMQ Queue Scaler, depending on the broker.


### 3. How do I choose the right broker?


Redis is quick to set up and is often sufficient for web projects. RabbitMQ is worthwhile for complex routing or if you have existing AMQP experience.


### 4. Do I need a separate scheduler for Celery?


For periodic tasks, Celery uses the Beat scheduler. With[django-celery-beat](https://github.com/celery/django-celery-beat) , you can maintain schedules in the Django admin.


### 5. How do I start Dramatiq cleanly in Django?


Via` python manage.py rundramatiq` from` django_dramatiq` . This command is intended for Django integration.


### 6. Are there dashboards for Django RQ?


Yes. There is[rq-dashboard](https://github.com/rq/rq-dashboard) as a standalone, as well as integrations for the Django admin.


---


## Do you have questions or an opinion? With your GitHub account you can let us know...


---


### Here are a few articles that you might also find interesting:


[Act, Don't Isolate: The Blueshoe Approach to Kubernetes Development Develop locally, test in the cluster - without CI/CD wait times. Gefyra promises the holy grail of Kubernetes development: full-speed coding with real cluster context. In our case study, you’ll learn why this tool is a game-changer for your daily operations and how to efficiently integrate it into your workflow by Carl-Christian Neundorf](https://www.blueshoe.io/blog/gefyra-hands-on/)


[django-o365: A Modern Mail Backend for Django and Exchange Online](https://www.blueshoe.io/blog/django-o365-a-modern-mail-backend-for-django-and-exchange-online/)


[django-o365](https://github.com/Blueshoe/django-o365) is an open-source mail backend for Django that enables sending emails via Exchange Online using OAuth 2.0. Born from real-world project requirements, this package is aimed at teams using Microsoft 365 who want to implement their email dispatch securely, maintainably, and without workarounds. This article provides an overview of the motivation, features, and usage.


by Robert Gutschale


[Mastering Django Caching: The Ultimate Guide from Cachalot to the Low-Level API Is your Django application slower than it should be? Long loading times not only frustrate your users but are also penalized by search engines. The solution often lies in a smart caching strategy. But where do you start? by Jonathan Kölbl](https://www.blueshoe.io/blog/django-caching-cachalot-performance-guide/)


[Handoff from Designer to Developer: What We Need for Frontend Implementation The transition from design to development is a critical moment in any frontend project. A well-thought-out handoff can save weeks of development time, while an incomplete handoff leads to endless questions, delays, and compromises. by Lukas Rüsche](https://www.blueshoe.io/blog/designer-to-developer-handoff/)


[Why Our Maintenance Reports Are a Key to Successful Collaboration For us, a maintenance report is more than a table of numbers: it is a tool for communication, planning, and trust. It shows what we have done, where potential lies, and what topics are on the agenda for the next quarter - so our customers know their systems are in good hands. by Robert Gutschale](https://www.blueshoe.io/blog/quarterly-maintenance-reports/)


[More Blog Articles Discover exciting insights and practical tips in our latest blog posts. From Python and Rust to Kubernetes, Django security, and frontend topics – find valuable knowledge for your projects here. Click to explore all articles!](https://www.blueshoe.io/blog/)


## These companies trust Team Blueshoe


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-


-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
-
