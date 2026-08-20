---
schema_version: "1.0.0"
document_id: "0c3b2370f4d52e5fd498282b3e2f794e9081d0b4a8af38696e49cbf670237979"
company_key: "duolingo-inc-class-a-common-stock"
company: "Duolingo Inc."
source_id: "duolingo-inc-class-a-common-stock-news-import-1f1e82a2742f"
canonical_url: "https://blog.duolingo.com/reduce-cpu-usage-97-percent/"
published_at: "2026-06-22T12:00:04+00:00"
first_seen_at: "2026-07-25T02:01:49.609700+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:c34da610536c8d6385b2dccbfaca24e5a7a85a66bbe91595f183ec1ace770078"
---

# How a simple code change reduced CPU usage by 97%

Routine infrastructure work does not usually make for a dramatic story. But during a migration of one of Duolingo’s production services from Python 3.9 + Alpine to Python 3.12 + Debian Bookworm, we accidentally uncovered **a hidden inefficiency that had been wasting CPU resources for years** . This is the story of how breaking things led to a **97% CPU reduction** .


## **The migration and the alerts**


The service in question handles SMS delivery for users in China, including time-sensitive messages such as streak-related notifications, with phone numbers stored in encrypted form. It runs on Alibaba Cloud ACK and processes a nightly batch workload around 23:00 Beijing time.


After the runtime upgrade, the next scheduled batch triggered a burst of alerts: **high latency** , **upstream 5xx errors** , **and frequent pod restarts** , all within a 6-minute window. The timing made the signal hard to ignore: Something about the migration had changed the service’s behavior under load.


Kubernetes events showing repeated pod crashes across the deployment during the batch SMS window.


## **Reading the symptoms**


The pod events told the story:


```text
Liveness probe failed: Get "http://172.18.1.219:3485/health": context deadline exceeded
0/10 nodes are available: 10 Insufficient cpu.
back-off 2m40s restarting failed container
```


The` /health` endpoint was timing out, **not because the service was logically unhealthy, but because it had become completely unresponsive** . In a uWSGI service with 10 threads per worker, **if all threads are blocked, there is nothing left to serve even a health check** . Kubernetes interprets that as a dead pod, kills it, and attempts to reschedule it, which only makes things worse when the cluster is already CPU-constrained. It was a classic failure cascade.


The key question was: **What was blocking all 10 threads at once, and only during the nightly batch?**


## **Finding the culprit**


Looking at the outbound SMS code path revealed the problem:


```text
return Session().post(
self._host + self._SMS_SINGLE_SEND_API_ENDPOINT,
data=payload,
timeout=(self._CONNECT_TIMEOUT, self._TIMEOUT),
)
```


A brand-new` requests.Session()` was being created for every request. That effectively forced a new TCP and TLS connection to our SMS provider for every SMS sent. Under normal traffic, that pattern was inefficient but not immediately noticeable. Under a bursty batch workload, it became expensive enough to block all worker threads on connection setup.


The fix was straightforward: Use a thread-local persistent session so each thread could safely reuse its own connection across requests.


```text
def _get_session(self) -> Session:
if not hasattr(self._local, "session"):
self._local.session = Session()
return cast(Session, self._local.session)
```


## **The results**


Where did the CPU go? Post-fix usage was so low we had to zoom in to see it.


Peak CPU per pod during the batch window dropped from saturation to **under 3.5%** . The **minimum pod count** went from 32 (emergency scaling during the incident) down to 8 at steady state, a 75% reduction from the incident configuration and well below pre‑migration levels of 18. The connection‑pooling fix alone is responsible for an estimated **~97% drop in CPU usage at peak** versus the incident window. The service is now more efficient than it has ever been.


Zooming out: Post-fix CPU usage remains consistently below pre-migration levels, even during batch windows. The gradual increase reflects scaling pod count down toward steady-state.


## **The hidden benefit of infrastructure migrations**


The most interesting part of the story is that **this pattern had been sitting in the codebase for a long time** . The service worked, SMS messages were being delivered, and nothing obvious looked broken. On the older runtime stack, the cost stayed hidden. The migration to Python 3.12 and Bookworm changed the operating conditions just enough to expose it.


Infrastructure migrations are often framed as maintenance work: necessary, but mostly unglamorous. In practice, they can also act like a form of load testing. A new runtime, a heavier base image, or a different system library can shift the performance envelope enough to reveal assumptions that had been invisible before.


That is what happened here. The original pattern was not obviously catastrophic in a lighter environment, but it was still inefficient. Once the migration made that inefficiency visible, fixing it left the service in a much better state than before.


## **Takeaways for your own services**


- If your service makes outbound HTTP calls from a threaded server, make sure it is reusing connections.
- If a health check starts timing out during an incident, consider thread saturation before assuming the application itself is broken.
- **Don’t be afraid of infrastructure migrations breaking things. When they do, it’s often pointing at something worth fixing.**


At Duolingo, engineers have the freedom to investigate deeply, improve systems across the stack, and turn production incidents into meaningful wins. If solving problems like this sounds exciting, come and work with us.


[SEE OUR OPEN ROLES HERE](https://careers.duolingo.com/?department=Engineering&utm_source=blog.duolingo.com&utm_medium=blog&utm_campaign=CPU_blog_062226#careers)
