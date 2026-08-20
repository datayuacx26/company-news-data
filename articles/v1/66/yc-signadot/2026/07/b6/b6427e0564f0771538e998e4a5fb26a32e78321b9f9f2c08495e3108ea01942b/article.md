---
schema_version: "1.0.0"
document_id: "b6427e0564f0771538e998e4a5fb26a32e78321b9f9f2c08495e3108ea01942b"
company_key: "yc-signadot"
company: "Signadot"
source_id: "yc-signadot-rss-2aab0f6a68b5"
canonical_url: "https://www.signadot.com/blog/catch-performance-regressions-with-k6-and-signadot-plans/"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:42.188410+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:795405f4606aedb86d8937c9e915e156b29f860f1b0ddd6e8cee15ad44b78e29"
---

# Catch Performance Regressions Before the Pull Request With k6 and Signadot Plans

A rate-limiting change that passed every local check added six seconds of p95 latency to a live dispatch path. Here is how a reusable k6 plan caught it inside the cluster, before the change ever became a pull request. This post walks through the tutorial video above.


This tutorial pairs Grafana k6, the open source load-testing tool, with[Signadot Plans](https://www.signadot.com/product/plans/) , reusable validation workflows that run against a live Kubernetes environment, to catch a regression that never shows up locally.


Some performance regressions slip past every check you run. The build passes, go vet is quiet, the unit tests are green, and the diff reads clean in review. The real cost only shows up when concurrent traffic hits the change, and by then it has usually reached staging or production, where finding it costs an incident instead of an iteration.


The video above walks through one of those regressions end to end. The app is HotROD, a ride-sharing demo running in Kubernetes. On every dispatch request, its frontend calls the location service twice, which makes the dispatch path latency sensitive and worth guarding. In about ten minutes, a coding agent authors a k6 load test from a single sentence, captures a healthy reference run, takes on a realistic change that passes every local check, and catches a six-second latency cliff inside the cluster. This post covers the same ground in text, with the details you would want before trying it on your own services.


## Why local checks cannot see this class of bug


The change in the video starts from a reasonable ticket: rate-limit location resolution so a traffic spike cannot overwhelm the location service. The developer reaches for a sync.Mutex, locks the function body, and defers the unlock. The reasoning sounds right, and the code is wrong.


```text
var   resolveMu   sync  .  Mutex


func   ResolveLocations  (  ctx   context  .  Context  ,   req   DispatchRequest  ) (  Locations  ,   error  ) {
resolveMu.  Lock  ()
defer   resolveMu.  Unlock  ()
// two round trips to the location service per dispatch
pickup   :=   locationClient.  Get  (ctx, req.PickupID)
dropoff   :=   locationClient.  Get  (ctx, req.DropoffID)
return   combine  (pickup, dropoff),   nil
}
```


A mutex does not rate-limit calls. In this placement it serializes them. ResolveLocations makes two round trips to the location service per request, and one lock covers both, so every request holds the lock for the full round trip. Under load, ten concurrent requests stop running in parallel and queue behind a single lock.


Nothing on the laptop flags it. Unit tests run one call at a time, so nothing ever contends for the lock. Build and vet have no opinion on lock placement. The only honest way to see the cost is to put concurrent load on the real path, with the real location service behind it.


## A plan is a load test you author once


This is what Signadot Plans are for. A plan is a small, reusable validation workflow that runs against a live environment in your Kubernetes cluster. Plans are composed of actions, typed building blocks your platform team governs through a catalog, so agents assemble validation from a vocabulary you control. The built-in catalog includes HTTP request checks, assertions, browser flows through Playwright, and load scenarios through k6.


You do not write the plan by hand. The signadot-plan skill has a coding agent draft it from natural language: it reads your action catalog, writes the k6 script and plan spec, runs the plan against live services to confirm it works, then tags it as a versioned artifact. In the video, the entire specification is one sentence: ten virtual users for thirty seconds, posting a dispatch to the frontend service, with two gates.


```text
export   const   options   =   {
vus:   10  ,
duration:   '30s'  ,
thresholds: {
http_req_duration: [  'p(95)<1500'  ],
http_req_failed: [  'rate<0.01'  ],
},
};
```


The first gate caps p95 latency at 1500 milliseconds. The second caps the HTTP failure rate at 1 percent. When either threshold is breached, k6 exits non-zero and the plan run fails. The agent tags the result hotrod-dispatch-load, and from that point the plan guards every future change to this path. Anyone, human or agent, can rerun it by name.


```text
signadot   plan   run   --tag   hotrod-dispatch-load
```


## First, capture a reference run


Before trusting a gate, run it against unchanged code. The reference run in the video drives 496 requests over thirty seconds from ten virtual users. p95 latency lands at 680 milliseconds, well under the 1500 millisecond budget, and the failure rate is 0 percent. Both gates pass.


That number matters beyond the green check. It is the healthy shape of the dispatch path, the reference every future change gets measured against. When a later run drifts, you know what it drifted from.


## Run the change on the real path, not through a tunnel


To measure latency honestly, the changed code has to run inside the cluster, next to its real dependencies, with nothing extra in the request path. The signadot-validate skill handles that. It reads the diff, picks the matching plan by its selection hint, builds the modified frontend into an image, and creates a Sandbox that forks the in-cluster frontend deployment to run that image.


The fork is isolated by a routing key, an opaque value generated for the Sandbox. Requests that carry the key in their headers route to the forked frontend, and everything else in the cluster falls through to the shared dependencies, the unchanged services every Sandbox reuses. The k6 script keeps targeting the frontend by its normal in-cluster address and injects the routing key on each request, so the only new latency in the measurement is the code under test. A tunnel back to a laptop would add network time on every hop and distort a millisecond threshold, which is exactly why the load generator runs in-cluster too. For a deeper look at driving k6 at scale inside Signadot, see[how to run scalable performance tests with Grafana k6 and Signadot](https://www.signadot.com/blog/how-to-run-scalable-performance-tests-using-grafana-k6-and-signadot/) .


## Reading the failed run


The plan run against the fork fails. k6 exits with code 99, its signal that a threshold was crossed, and the latency gate is the one that breaks: p95 jumps from 680 milliseconds to 6.37 seconds.


The failure rate holds at zero. Every request still succeeds, it waits. Throughput collapses from sixteen requests a second to under two, and the virtual users drain as they queue behind one lock.


> The failure rate held at zero. Nothing errored, everything waited. That is exactly the shape of regression a functional test can never see.


This is worth pausing on. A correctness suite would have passed this change, because every response is still correct. The regression only exists as a distribution, visible when concurrent load meets serialized execution. The plan caught a change that passed every local check and would have quietly broken the live dispatch path.


## Revert, rerun, and keep the plan


The plan did its job, so the mutex gets reverted. The lock and its import come out, and the local checks stay clean. A real fix for the original ticket would reach for a token-bucket limiter or a bounded semaphore, either of which caps the rate of calls without serializing every request behind one lock.


One more run against the reverted code confirms the path is healthy: p95 back at 682 milliseconds, failure rate still zero, both gates green. Then the Sandbox gets torn down. The regression never left the development loop. It never became a pull request, and no reviewer had to catch a lock placement by eye.


The lasting asset is the plan itself. It was authored from one sentence, it lives in version control as a tagged artifact, and the next change to the dispatch path reruns it with nothing new to write. To set this up on your own cluster, start with the[plan-based validation quickstart](https://www.signadot.com/docs/tutorials/quickstart/plan-based-validation) , browse the[action catalog](https://github.com/signadot/actions) your plans compose from, and read more about[Signadot Plans](https://www.signadot.com/product/plans/) and the[agent skills](https://www.signadot.com/docs/integrations/coding-agents/agent-skills) that drive this workflow.
