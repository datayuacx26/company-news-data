---
schema_version: "1.0.0"
document_id: "c760bc03b62643af8e23b8dbf1976fda08f20642461274042f60384e1d8fe485"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/post-mortem-photoroom-api-incident-october-11-2024"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:858b2d4cf4ca8436947118c5b2306375f443db2871e87658c09ba665b7c2a796"
---

# Post-mortem: Photoroom API service degradation – October 11th, 2024

For the first time in years of existence, the Photoroom API experienced a major outage. In this post, we’ll cover the causes as well as the action we’re taking to ensure it doesn’t happen again.


Beginning on Friday, October 11th, 2024 at 15:33 UTC our backend services experienced a disruption that degraded performance and caused a temporary unavailability of our Background Removal API and Image Editing API for approximately 45 minutes.


The issue originated from an unexpected latency spike in one of our internal services that handles monitoring and analytics. Our backend workers became unresponsive as they waited for responses from the impacted service. As a result, new requests were queued, leading to increased wait times and eventual service unavailability.


## Root Cause


The latency spike was traced back to a call made to an auxiliary monitoring and analytics service. While our code was designed to handle errors from this service, it did not include a strict enough timeout. This caused our backend workers to remain in a waiting state, unable to process other requests.


Moreover, our API includes a buffer queue to absorb temporary spikes. In case all workers are busy, requests wait in a queue. This aggravated the problem as requests piled up in the queue.


## Resolution


Once we identified the root cause, we quickly shut down the auxiliary service. However, as requests were waiting in the buffer queue, this did not resolve the issue immediately. We purged the queues by disabling the traffic coming from our apps, favoring the traffic from our API customers.


The issue was fully resolved by 16:10 UTC, with normal operations resuming shortly thereafter.


## Next steps


To prevent similar issues from occurring in the future, we are implementing the following actions:


1.


**Timeouts** : We will enforce strict timeout settings for all dependencies and services to prevent workers from being blocked for too long.


2.


**Testing** : Tests have already been added to ensure the API remains responsive even if non-essential services become slow or unavailable.


3.


Deployment procedure: While the timeout behavior was specified before the implementation, it was not tested in a real-life scenario. When adding dependencies on external services, we will more thoroughly test high-latency and failure behaviors to ensure they conform with our architecture design.


We take the reliability of the API as seriously as our customers do: the Photoroom apps - used by tens of millions - are also powered by the Photoroom API and this outage affected our users. We sincerely apologize for any inconvenience caused to our API customers.


**Timeline of Events (UTC time):**


-


**15:33** : Latency spikes begin affecting backend services.


-


**15:37** : First system alert received.


-


**15:50** : Issue is identified, and the analytics service is shut down.


-


**16:05** : System begins recovering.


-


**16:10** : Services return to normal.
