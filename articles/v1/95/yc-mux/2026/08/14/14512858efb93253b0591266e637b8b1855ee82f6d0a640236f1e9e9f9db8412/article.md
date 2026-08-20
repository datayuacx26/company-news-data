---
schema_version: "1.0.0"
document_id: "14512858efb93253b0591266e637b8b1855ee82f6d0a640236f1e9e9f9db8412"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/how-mux-chooses-a-cdn-in-9-9ms"
published_at: "2026-08-03T18:21:00.003+00:00"
first_seen_at: "2026-08-03T21:55:49.846400+00:00"
fetched_at: "2026-08-03T22:52:40.984273+00:00"
content_hash: "sha256:f8e43d9fd9239dcdb66ff2fc9e526850ac2357dd1a73969b93e0af6e0628c5a6"
---

# How Mux chooses a CDN in 9.9ms

Published on


August 3, 2026


# How Mux chooses a CDN in 9.9ms


By[Mark Franceschini](https://www.mux.com/team/mark-franceschini) • 10 min read •[Engineering](https://www.mux.com/blog/category/engineering)


---


**TL;DR** Which CDN is best?" is the wrong question the right one is "which CDNs are good enough right now?" This is the story of four attempts at CDN selection at Mux and the solution we finally built to answer it in 9.9ms.


CDN selection sounds simple: pick the best CDN for each viewer. But “best” is where it gets tricky. What does best even mean? Cheapest for the business? Fastest for the customer? Most reliable? Or, the one with the best support team so that when things eventually go wrong (and they will), you're not waiting days for a response? CDN selection is easy until it's not.


Since joining Mux five years ago, I’ve been involved in many attempts to revamp our CDN selection process. None have been perfect, but each one moved us forward and improved how we make decisions.


### A brief history of CDN selection at Mux


In the early days, our CDN selection ran on a third-party RUM (real user metrics) service. You’d send it an IP address, and it would tell you which CDN to use. It was eventually acquired and sunset, which pushed us to another RUM service. The new service worked similarly, with a few extra levers for traffic control. Switching between the two felt natural. They shared the same interface. Send an IP, get the “best” CDN back.


However, we quickly realized there were a few problems with this approach.


First and most noticeably we didn’t have fine-grained levers we could pull during outages to move traffic. At that point, our only option was to globally drain a CDN. Widespread CDN outages are rare; more often, we see smaller issues limited to a specific location. So we built more levers at both the location and customer level. That way, we could drain a CDN in specific locations or for specific customers. This logic lived after our calls to the RUM service. We always had the final say in which CDN was used.


The RUM data itself was also an issue. It’s collected from opt-in websites that embed scripts. On page load, the script makes requests for an image across several CDNs. The results are recorded and used to power CDN selection. But those images are permanently cached at the CDN edge, so the tests don’t measure CDN → origin, only CDN → website. If a CDN has an outage that affects its ability to reach origin, the RUM service can miss it. More importantly, these tests don’t hit our origins at all. We’ve seen cases where a specific CDN couldn’t communicate with our origin without it being a broader, service-wide outage.


We decided to take matters into our own hands. What started as a hackweek project evolved into our first fully Mux-owned CDN selection process. The service took the request IP, mapped it to a location, and looked up CDN performance for that location. The goal was to pick the CDN with the lowest latency based on our logs. After some testing, we ran into major issues and had to abandon the project.


Using CDN logs as our only data source was a mistake. It created a snowball effect. Once the service decided to use CDN A, all logs came from CDN A and none from CDN B, which meant the system only had data about CDN A. Also, CDN logs are reliable for latency but not for availability. There’s no way to tell from the logs whether an error came from a CDN outage versus an origin issue versus something in our edge code.


We were also overly focused on getting the BEST CDN for each request, where “best” meant lowest latency. If CDN A had a p99 latency of 80ms and CDN B had a p99 latency of 78ms, we’d always pick CDN B. That led to lopsided selection ratios in cases where customers would never notice the difference.


Finally, we calculated p99 latency on the fly for every request. Selection was too slow, sometimes taking up to 700ms, and the extra load slowed downstream services.


No one can feel the difference between 78 and 80ms. Chasing the "best" it cost us: we leaned too hard on one provider, which made us less resilient, and recomputing latency on every request slowed selection enough to drag on other services.


## Enter the CDN Router


Inevitably, CDN selection resurfaced as a priority, and we decided to try again. The first thing I did was review our past attempts and list what we’d learned from each one.


1. CDN log data is good for latency, but it’s a poor indicator of CDN availability.
2. We can’t rely solely on data that’s directly affected by the CDN selection service. We need an independent source that keeps collecting signals even when we’ve routed Mux video customer traffic away from a CDN.
3. Availability scores should be pre-calculated instead of computed on the fly. That lets selection do a quick database/cache lookup rather than a costly, complex calculation on every request.
4. The goal shouldn’t be to find the single best CDN for a request. It should be to rule out CDNs that aren’t meeting an acceptable bar, then do a weighted random shuffle to decide which CDN to use. This allows us to provide a better experience to more viewers.


At the same time, we'd added a third CDN to our pool. A and B are the traditional kind, the ones you picture when you hear “CDN” and that show up on the first page of Google. C is a collaboration with a partner to set up our own POPs, so it’s smaller, we don’t share it’s cache with other customers, and it has edge POPs in fewer regions. Most importantly, unlike A and B, it can’t scale infinitely. We decide when and how to scale it, and that isn’t instant. If we send it too much traffic, we’ll overwhelm the servers allocated to us and start dropping requests. That added a whole new dimension to the design.


The business context plays a role as well. Costs, performance, features, quality of support, and more all play a role. Now, when performance is equal (and C is at capacity), we’d rather use A than B.


With all of this in mind, we started designing the system. These were the requirements:


- Selection must be fast. Previous RUM solutions took 100-200ms per request. That was acceptable, but not great.
- We need an independent source of data generated from requests to our services that isn’t affected by our own CDN selection decisions (no snowball effect).
- The service needs to be expandable. New data sources should be easy to onboard.
- The service should automatically route traffic away from CDN C when we’re nearing capacity.
- The service should only pick CDN C when the viewer is in the United States, Canada, or Europe, and there is capacity.
- The service must let us manually route traffic based on the following dimensions, while supporting more dimensions in the future:


- Country
- Continent
- Customer ID


The CDN selection service has three components:


**1. Health probes**


HTTP requests we make against our CDNs. We run two test types: one that goes all the way through the CDN to origin, and one that’s answered at the CDN edge. Each test runs against every CDN in multiple locations around the world (and the origin test against all 4 of our origins) every 60 seconds. Results are recorded in our database.


**2. Database**


We use CockroachDB, a distributed SQL database that runs across multiple regions and clouds. It stores raw probe results, routing decisions, and CDN C's real-time usage metrics.


**3. CDN router**


The router runs in two modes, leader and server mode.


**Leader mode** runs three asynchronous jobs, each on its own interval:


- Gather CDN health-probe results and write them to the database (every minute).
- Gather CDN C capacity metrics and write them to the database (every 20 seconds).
- Evaluate recent probe results to decide which CDNs are healthy in each location (every minute).


Each decision pass looks back over the last 5 minutes of probe data, and we rerun it every minute. That rolling window keeps decisions stable against a single noisy probe, while still letting us detect issues and route new traffic away within about a minute.


**Server mode** decides which CDN a given request can use:


1. Turn the viewer’s IP into a location.
2. Look up the pre-calculated decisions for that location to see which CDNs are healthy.
3. For CDN C, also check the location allow list and current capacity; drop C if it doesn’t match or we’re near capacity.
4. Weighted-shuffle the survivors (weights reflect our ideal traffic split) and return the list.


The caller serves the video from the first CDN.


The result is super-fast CDN selection with a p99 of 9.9ms.


## Redundant streams


Our CDN selection runs when a viewer first loads a video. That means someone watching a long video could be assigned a healthy CDN at the start, only for that CDN to run into trouble by the time they reach the end. To address this, we released[redundant stream support](https://www.mux.com/blog/survive-cdn-failures-with-redundant-streams) over 6 years ago. But because it was opt-in, most customers never used it. Recently, we flipped redundant streams to be opt-out instead of opt-in. Combined with our CDN selection, this means we’ll start you on the CDN we think is best while still giving your player the ability to switch mid-stream if something goes wrong.


The following image is from our redundant stream testing captured during a minor cdn outage. The purple line represents views that were using redundant streams. The orange represents views that were not.


## What’s next


The CDN router is live and fast, but we’re not done. There are two things we want to add. First, more data sources. We want to layer in additional data sources like CDN logs and real-time info from Mux Data. Second, content steering, which would let the player shift CDNs on its own and build even more resilience on top of our redundant stream support.


## Wrapping up


Four attempts in, what finally worked wasn’t a cleverer way to find the single best CDN. It was accepting that “best” is fuzzy and building a system that rules out CDNs that aren’t good enough, then spreading the remaining traffic on our terms. Owning the data end to end, keeping it independent of our own routing decisions, and pre-computing availability so the request path stays cheap is what got us to a 9.9ms p99.


## Written By


### [Mark Franceschini – Senior Software Engineer](https://www.mux.com/team/mark-franceschini)


Previously worked on IBM's Kubernetes service. Loves a good peanut butter, raisin, and chocolate chip sandwich. Hates wearing shorts.


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
