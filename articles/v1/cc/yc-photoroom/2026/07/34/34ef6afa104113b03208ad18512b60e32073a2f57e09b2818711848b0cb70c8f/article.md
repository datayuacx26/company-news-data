---
schema_version: "1.0.0"
document_id: "34ef6afa104113b03208ad18512b60e32073a2f57e09b2818711848b0cb70c8f"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/switching-from-t4-to-a10"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:dbfd61270f668c504f1a201ec643a8bd21c79f2a07d2bc9618cefe5214dd1fc1"
---

# How we divided our server latency by 3 by switching from T4 GPUs to A10g

At Photoroom, we build photo editing apps. One of our core features is to remove the background of any image. We do so by using deep learning models that will segment an image into different layers. We also provide inpainting models to enable our users to retouch any image they want with state-of-the-art technology.


Most of our users edit tens of pictures at a time, which means we need to provide them with the shortest inference latency possible, so that their editing process is as smooth as possible. We do so by compiling our models to **[TensorRT](https://www.photoroom.com/inside-photoroom/faster-image-segmentation-trtorch-tensorrt)** and using the latest inference GPUs. Up until last June, we were using the T4 GPU for our inference workload.


As you can imagine, every time Nvidia releases a new set of inference GPUs we are particularly excited as we know this can be the opportunity for a huge improvement for our users. So when the A10g was released, we immediately started to benchmark it for our inference workload. Currently, they are only available on AWS and Azure.


## Server-side impact of moving to the A10g


Lo and behold, moving our inference workload to the AWS-provided A10g divided our server-side p50 latency by 3, from roughly 450-500 ms to roughly 150ms. Codewise this amounted to recompiling our models to TensorRT on the A10g.


At this point, you are probably thinking this is too good to be true and that this migration must have cost us a crazy amount of money. Wrong 😀. This was almost cost-free for us.


You might also point out that with such low server latencies, most of the processing time as seen by the user must be spent sending data to the servers and receiving responses.


In the next section, we will use locust to take into account the user-perceived latency and compute the monthly prices for the T4 and the A10g at a given QPS.


## T4 vs A10g configurations for a given QPS


In order to benchmark the T4 versus the A10g we used[locust](https://locust.io/) to simulate requests coming from users and measured the latency as seen by the users. Locust is a tool that can generate realistic load patterns against a dynamic program host on a distant server.


One of Locust’s great features is the ability to define user behaviors using simple Python code:


```text
import locust
from locust import HttpUser, tag, task


# Weights tasks attributes represents the execution ratio between the tasks
WEIGHTS_1 = 12
WEIGHTS_2 = 3
...
# This locust class is used to define the behaviour of your user
class LBUser(HttpUser):
# The on_start method will be called for each simulated user when they start.
def on_start(self):
...


# Define your first task
@tag("Task 1")
@tag(WEIGHTS_1)
def task_1(self):
...


# Define your second task
@tag("Task 2")
@tag(WEIGHTS_2)
def task_2(self):
...
```


*Refer to this[page](https://docs.locust.io/en/stable/writing-a-locustfile.html) for more information on how to write your own locust file.*


Once your locust file is ready, you can use this simple command to launch the locust test:


```text
locust -f locust_file.py --host=HOST_IP
```


Given a random number of queries per second (100QPS in this experiment) we computed the latency / monthly price curves for both the T4 and A10g for various numbers of instances:


We dispatched the requests to the instances using a round-robin load balancer.


As can be seen in the graph above, using 20 T4 instances costs approximately the same price as using 7 A10gs but the p50 and p90 latencies will be lower on the latter. In this configuration, it is then strictly beneficial to switch from the T4 to the A10g.


## What’s next?


The migration to the A10g provided a significant user experience improvement. It also gave us more GPU memory to play with, bumping it up to 24GB from the T4’s 16GB. This will allow us to ship more or bigger models to provide the best editing experience to our users. However this is not the end for us, and we will stay on the lookout for new inference GPUs and better ways to serve our models.
