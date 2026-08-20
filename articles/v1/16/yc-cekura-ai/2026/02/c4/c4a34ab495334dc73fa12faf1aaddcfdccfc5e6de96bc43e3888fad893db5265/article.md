---
schema_version: "1.0.0"
document_id: "c4a34ab495334dc73fa12faf1aaddcfdccfc5e6de96bc43e3888fad893db5265"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/how-we-built-an-autoscalable-infrastructure-for-voice-ai-agents"
published_at: "2026-02-21T00:00:00+00:00"
first_seen_at: "2026-07-24T05:11:51.123297+00:00"
fetched_at: "2026-07-28T22:19:30.840186+00:00"
content_hash: "sha256:d0c792bf79e035f2ef76a2688d092fe8eb1a2c5442c4ea1c61fc284561af57fb"
---

# How We Built an Autoscalable Infrastructure for Voice AI Agents

At Cekura AI, our mission is to test voice agents with rigorous, realistic simulations. But unlike standard HTTP requests, voice simulations are **long-running, stateful, and resource-heavy** . A single test might involve a 5-minute to 60-minutes conversation with an AI, utilizing WebRTC and ML inference simultaneously.


The traffic pattern? Pure chaos. One minute we have 10 active calls; the next, a client drops a batch of 500. This creates a classic **"Thundering Herd"** problem.


We needed an infrastructure that could handle unpredictable spikes, enforce multi-tenant fairness, and scale from one to hundreds of workers without breaking the bank. Here is how we built a custom autoscaling engine using **Redis, Celery, and AWS ECS** .


## The Challenge: It's Not Just "Add More Servers"


Scaling stateful voice simulations isn't as simple as spinning up more pods behind a load balancer. We faced three specific friction points:


1. **Variable Load:** Traffic fluctuates wildly based on client QA cycles.
2. **The "Noisy Neighbor" Problem:** We serve multiple organizations. We can't let one client's massive batch starve urgent tests from another.
3. **Slow Provisioning:** Spinning up new containers takes 60–90 seconds. We needed a bridge between "request received" and "worker ready."


---


## The Architecture: Redis as the "Brain"


We moved away from standard metric-based scaling (like CPU usage) and built **Intent-Based Scaling** .


We use **Redis** as the single source of truth. It tracks:


- **Active Tasks:** Real-time counts of running simulations, both global and client level.
- **Pending Demand:** Batches waiting in the queue, again both global and client level.
- **Worker State:** A cached count of available infrastructure.


This allowed us to decouple the decision to scale from the execution of scaling.


---


## Tech Deep Dive: Celery, Boto3, and The Sync Loop


One of our key engineering wins was decoupling the "heavy lifting" of infrastructure inspection from the critical path of request handling. We achieved this using a combination of **Celery** background tasks and the **AWS Boto3 SDK** .


### The Periodic Sync


Querying AWS ECS or inspecting a Celery queue is slow (2–5 seconds). If we did this for every incoming API request, our latency would skyrocket.


Instead, we implemented a **Background Sync Loop** :


1. **Celery Beat** triggers a specialized maintenance task every 45 seconds.
2. **Worker Inspection:** This task uses the Celery SDK to` inspect()` active nodes, discovering exactly how many workers are currently consuming from the simulation queue.
3. **Infrastructure Validation:** Simultaneously, we use the` boto3` ECS client to fetch the` describe_services` output, verifying that the cloud state matches our application state.
4. **Cache Update:** The results are written to Redis with a **TTL (Time To Live)** .


**The Result:** When a user submits a batch of 500 simulations, the API doesn't query AWS. It reads the worker count from Redis in **<5 milliseconds** , allowing us to handle thousands of concurrent capacity checks without breaking a sweat.


---


## Handling Concurrency with Distributed Locks


With multiple clients submitting batches simultaneously, race conditions are a major risk. If two huge batches hit the system at the exact same millisecond, they could both trigger a scale-up event, leading to double-provisioning (and double the bill).


To solve this, we implemented **Per-Organization Distributed Locks** using Redis.


---


### How It Works


Every time a batch is processed or a capacity check is run, the system acquires a granular lock specific to that organization:


```text
lock_key =  f"org_lock: {organization_id}  "
with   redis_client.lock(lock_key, timeout= 5  ):
# 1. Check current usage for this specific org
# 2. Check global capacity
# 3. Dispatch tasks or queue them


```


This ensures that requests "get in line" nicely.


- **Fairness:** It prevents "thundering herd" scenarios where one organization's rapid-fire requests overwhelm the scheduler.
- **Stability:** It serializes the critical logic of checking capacity and decrementing slots, guaranteeing that our internal counters remain perfectly accurate even under high load.


---


## The Smart Scaling Logic


Once we have the data (thanks to Celery/Boto3) and the safety (thanks to Redis locks), the actual scaling logic applies a **4-Layer Fairness Algorithm** :


1. **Org Limit:** Is this client over their total allowed concurrency?
2. **Batch Limit:** Is this specific test run over its cap?
3. **FIFO Check:** Are older batches from this client still waiting? (We don't want new tasks jumping the queue).
4. **Global Capacity:** Does the whole system have room?


If the global capacity is insufficient, the system calculates the exact shortfall (e.g., "We need 50 slots, but only have 20"), updates the "Target Worker Count" in Redis, and triggers the ECS Scaler to bridge the gap.


---


## Zero-Data-Loss Downscaling


Scaling up is easy; scaling down is dangerous. If you kill a container running a voice simulation, the test fails, and the data is lost.


We built a **7-Phase Graceful Drain** to handle this:


1. **Identify:** We select the oldest workers to remove.
2. **Stop Consumption:** We send a signal to the specific Celery worker to stop accepting new tasks.
3. **Wait:** We monitor the worker until` active_tasks == 0` .
4. **Protect:** We use AWS "Scale-In Protection" to ensure the cloud provider doesn't kill the wrong workers during the update.
5. **Terminate:** Only once confirmed idle do we allow the termination.


---


## Handling Variable Loads and 2000+ Concurrent Calls


The true test of this architecture wasn't steady-state traffic, it was the ability to absorb shock. In production, we rarely see linear growth. Instead, we face highly **variable loads** where demand spikes from zero to maximum capacity in seconds.


By decoupling inspection from execution, our infrastructure proved it could handle these extremes. We successfully pushed the system to sustain **2000 concurrent voice simulations** .


- **Elastic Response:** When the load varied wildly, the Intent-Based Scaling (via Redis) detected the backlog immediately. Instead of waiting for CPU spikes (which are lagging indicators), the system began provisioning ECS tasks the moment the "Pending Demand" queue filled up.
- **Performance at Scale:** Even at 2000 concurrent calls, the API latency remained under 5ms because it was reading from the Redis cache rather than querying AWS.
- **Resilience:** The distributed locks ensured that despite the high concurrency, we didn't double-provision resources or hit race conditions. We successfully maintained 2000 active simulations without degrading audio quality or losing conversation state.


---


## Architecture Overview


---


## The Impact


By moving to this custom architecture, we transformed our operations:


- **Speed:** Scale-up time stabilized at **~90 seconds** (down from manual intervention).
- **Utilization:** Resource usage jumped from 40% to **75%+** (we stop paying for idle servers).
- **Reliability:** The distributed locking mechanism eliminated race conditions, ensuring that even during our busiest hours, every organization gets its fair share of resources.


### Key Takeaway for Engineers


If you are dealing with bursty, long-running workloads, don't rely solely on CPU/Memory autoscaling. **Scale based on intent** .


By centralizing state in Redis, decoupling inspection with Celery/Boto3, and protecting concurrency with distributed locks, you can build a scaler that is both incredibly fast and financially efficient.


---


Building Complex AI Infrastructure? Let's chat.[cekura.ai](https://www.cekura.ai/)


Start free trial:[dashboard.cekura.ai/overview](https://dashboard.cekura.ai/overview)


Book demo:[cekura.ai/expert](https://www.cekura.ai/expert)
