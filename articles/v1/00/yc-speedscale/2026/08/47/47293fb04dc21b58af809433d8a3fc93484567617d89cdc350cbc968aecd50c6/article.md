---
schema_version: "1.0.0"
document_id: "47293fb04dc21b58af809433d8a3fc93484567617d89cdc350cbc968aecd50c6"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/two-kinds-of-chaos-infrastructure-and-application/"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T19:29:22.668335+00:00"
fetched_at: "2026-08-18T19:29:27.598816+00:00"
content_hash: "sha256:862364d6b982cfe774437930e11079a841daf3c94d45ddd91eb758388b47d9eb"
---

# Chaos Monkey Won't Find Your Bug

We shipped a chaos feature that never caused any chaos.


Our mock server has had a fault-injection effect for years with a straightforward job: withhold the response entirely and see whether the caller copes. Last week I audited it against the actual code path. It had never withheld anything.


The handler returned early without writing a response. Go’s` net/http` then did what it is designed to do, which is synthesize a` 200 OK` and flush the recorded body. Every experiment anybody ever ran with that effect handed the application a perfectly good answer, and every one of them passed.


```text
sequenceDiagram
participant Caller
participant Handler as Mock handler
participant HTTP as net/http
Caller->>Handler: Request
Handler-->>Handler: Withhold effect fires
Handler--xHTTP: Returns without writing a response
HTTP->>Caller: Synthesizes 200 OK + recorded body
Note over Caller: Experiment "passes"


```


The bug survived because we validated it the way you validate infrastructure chaos. Did the experiment fire? It fired. The counter went up. Nobody asked what the application on the other end actually received.


That is this entire post in miniature. Infrastructure chaos and application chaos are not two intensities of the same practice. They break different layers, they surface different bug classes, and buying one does not cover you for the other.


🎯 Key Takeaways


- Infrastructure chaos (LitmusChaos, Gremlin, Chaos Mesh) tests whether your platform recovers; application chaos (Istio faults, FIT-style tools) tests whether your own error-handling code is correct, and the two find disjoint bug classes.
- 92% of catastrophic distributed-systems failures come from mishandled errors the software already detected, not infrastructure loss ([Yuan et al., OSDI’14](https://blog.acolyer.org/2016/10/06/simple-testing-can-prevent-most-critical-failures/) ).
- Netflix built Failure Injection Testing and ChAP specifically because Chaos Monkey and Chaos Kong couldn’t reach failures inside application call paths.
- Cloudflare’s November 2025 outage had no dead node and no network partition: a malformed dependency payload took down the proxy, a failure mode only application-level chaos can reproduce.
- Own the two practices separately: infrastructure chaos as a platform-team game day, application chaos as a per-commit CI check owned by the service team.


## The layer you break decides the bug you find


When you kill a pod, you are testing Kubernetes. You are asking whether the scheduler reschedules, whether the readiness gate holds traffic, whether the load balancer drains connections before the container dies. Those are good questions. They are questions about your platform.


When you make a dependency return` 503` , you are testing your code. You are asking whether the retry budget is bounded, whether the circuit breaker opens, whether the fallback returns a sane default instead of a stack trace.


```text
flowchart LR
subgraph Infra[Infrastructure chaos]
A1[Kill a pod] --> A2[Scheduler reschedules?]
A3[Drop a node] --> A4[Load balancer drains?]
end
subgraph App[Application chaos]
B1[Return 503] --> B2[Retry budget bounded?]
B3[Null field] --> B4[Fallback returns a default?]
end


```


The second category is where the incidents come from. The canonical study here is still Yuan et al.’s analysis of 198 production failures across Cassandra, HBase, HDFS, MapReduce and Redis, which found that[92% of catastrophic failures resulted from incorrect handling of non-fatal errors that the software had already explicitly signaled](https://blog.acolyer.org/2016/10/06/simple-testing-can-prevent-most-critical-failures/) . The error arrived. Something noticed it. The handler was wrong.


The same study found 35% of those catastrophic failures came from error handlers so trivially broken they violated basic practice: a handler that swallows the error, a handler that over-catches and aborts the process, a handler whose body is a` TODO` comment. And[77% of the failures were reproducible in a unit test](https://blog.acolyer.org/2016/10/06/simple-testing-can-prevent-most-critical-failures/) .


None of that is reachable by draining a node. The fault has to arrive where the code expects data.


## Netflix built the second system for a reason


This is not a new observation, and the people who invented infrastructure chaos are the ones who made it.


Chaos Monkey came first and killed instances. Chaos Kong scaled that up to regions. Then Netflix built Failure Injection Testing, which[injects request-level faults into individual microservices in production](https://medium.com/netflix-techblog/chap-chaos-automation-platform-53e6d528371f) by hooking the shared Java client libraries every service already used. Then they built the Chaos Automation Platform on top of FIT to run those experiments continuously against canary and control clusters through Spinnaker.


Read that sequence as an architecture decision, not a product history. Netflix had the best instance-killing tooling on earth and still needed a separate mechanism, at a separate layer, with a separate integration point, to reach the failures that live inside application call paths.


The tooling market has since split along the same seam. LitmusChaos, a[CNCF incubating project since January 2022](https://www.cncf.io/blog/2022/01/11/litmuschaos-becomes-a-cncf-incubating-project/) , Chaos Mesh, Gremlin, AWS Fault Injection Service and Azure Chaos Studio are strongest at the platform layer: pods, nodes, disks, network, CPU, region.[We’ve covered that list in detail before](https://speedscale.com/blog/kubernetes-chaos-engineering-tools/) . Istio fault injection, Toxiproxy, Netflix’s FIT and API-level mocking tools sit on the request path and corrupt what a service is told.


Most organizations buy from the first list, run a quarterly game day, and record chaos engineering as covered.


## The worst outage of last year was not an infrastructure failure


On November 18, 2025, Cloudflare went down for[five hours and thirty-eight minutes](https://blog.cloudflare.com/18-november-2025-outage/) . The cause was a permissions change on a ClickHouse cluster that made a query return duplicate column metadata. That query generated a Bot Management feature file. The file doubled in size, crossed a hard limit of 200 features that the consuming code assumed it would never approach, and the proxy panicked.


Every machine was healthy. Nothing was partitioned. No region was lost. A dependency returned a payload with a different shape than the consumer expected, and the consumer’s handling of that case was to die.


Infrastructure chaos cannot find that failure, because there is no infrastructure fault to inject. The fault arrives as data, through a code path that thinks it is on the happy road.


## The failures that live in the gap


There is a third category, and it is the one that makes running both layers worth the budget line.


Bronson et al. described[metastable failures](https://sigops.org/s/conferences/hotos/2021/papers/hotos21-s11-bronson.pdf) as systems that degrade under a transient stressor and then fail to recover once the stressor is gone. Retry storms, congestive collapse, death spirals. The paper’s key point is that the root cause is not a hardware fault and not a software bug. It is emergent behavior from work amplification.


That failure needs both halves to reproduce. The trigger is usually infrastructure shaped, a node lost or a brief latency spike. The amplifier is always application shaped: an unbounded retry, a client timeout longer than the server’s, a cache stampede. Test either layer alone and the system looks fine.


## Running them in tandem without doubling the budget


The two practices have different economics, and the mistake is running them on the same cadence under the same owner.


Infrastructure chaos has a blast radius that crosses team boundaries. It needs a human watching, a rollback plan, and a scheduled window. That is a platform team responsibility and a game-day rhythm, measured in weeks.


Application chaos is scoped to a single service and its dependencies. It is deterministic enough to assert on, cheap enough to run per commit, and it belongs in the service team’s pipeline next to the integration tests. Measured in minutes.


Three practical rules that have held up for me:


- **Own them separately.** If the platform team owns both, application faults get scheduled behind infrastructure faults forever and never make it into CI.
- **Assert at the caller, not at the injector.** The question is never “did the experiment fire.” It is “what did the application receive, and what did it do next.” That is the exact mistake our own no-op fault effect survived on.
- **Derive faults from real traffic.** A` 503` you invented tests the branch you imagined. A malformed payload your actual dependency sent last Tuesday tests the branch you did not.


That last point is where I have a horse in the race, so take it accordingly.[proxymock](https://speedscale.com/proxymock/) ’s chaos support works by[manipulating individual request response times, status codes and data patterns](https://docs.speedscale.com/concepts/chaos/) against recorded dependency traffic, and our own documentation has said for years that the approach is complementary to Gremlin and Chaos Monkey rather than a replacement. That framing was correct before it was convenient.


## The counter went up


If you take one thing from this: the reason our fault effect stayed broken for years was not that the test was missing. The test existed and it was green. It asserted that the injector ran.


Chaos engineering has the same failure mode at the organizational level. The game day happened. The report says the pods came back. Nobody asked what the application received, because the tool that broke things was never sitting close enough to the application to know.


Kill the pod. Then lie to the service. They are different questions, and your customers will find whichever one you skipped.
