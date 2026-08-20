---
schema_version: "1.0.0"
document_id: "8f0fd67b572e778fa1a08db57ed5d38dd1d24ce23511fa8d3e0e08de34b0f2a5"
company_key: "yc-netbeez"
company: "NetBeez"
source_id: "yc-netbeez-rss-1a1c74f0723d"
canonical_url: "https://netbeez.net/blog/mtr-vs-traceroute/"
published_at: "2026-07-22T16:49:51+00:00"
first_seen_at: "2026-07-22T16:58:48.783017+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:e3e306ced02ab33abdc02d688518c175bd7d4272114bdc8cfe1d2423639a7ac1"
---

# MTR vs Traceroute: Which One Should You Run First?

Every network engineer has both tools in muscle memory, and most of us reach for one out of habit rather than intent. But traceroute and mtr answer different questions, and picking the right one first can save you from staring at misleading output, or worse, opening a ticket with your ISP over packet loss that isn’t real.


We covered[mtr in depth](https://netbeez.net/blog/linux-network-path-analysis-mtr/) and[traceroute’s inner workings](https://netbeez.net/blog/traceroute/) in previous posts. This one is about the decision: which tool, when, and what mistakes to avoid when reading the results.


## What traceroute shows


Traceroute answers one question fast: where does the path go, and where does it stop? It sends probes with incrementing TTL values, records each router that replies with an ICMP Time Exceeded, and gives you a snapshot of the path with three RTT samples per hop. Here’s a trace from a cloud agent to Cloudflare’s 1.1.1.1:


```text
$ traceroute 1.1.1.1


traceroute to 1.1.1.1 (1.1.1.1), 30 hops max, 60 byte packets


1  244.5.8.179  3.832 ms  244.5.0.21  7.178 ms  244.5.5.11  6.438 ms


2  240.0.228.67  0.341 ms  240.0.228.64  0.291 ms  240.0.52.65  0.313 ms


3  242.2.120.193  0.562 ms  242.7.112.65  0.745 ms  242.2.121.199  0.761 ms


4  240.3.180.8  2.618 ms  1.940 ms  240.3.180.39  1.253 ms


5  240.64.220.131  1.284 ms  240.64.220.130  0.688 ms  240.64.220.129  1.055 ms


6  52.46.166.112  1.776 ms  52.46.166.150  1.391 ms  52.46.166.112  1.987 ms


7  52.46.166.113  0.598 ms  0.513 ms  52.46.166.151  0.770 ms


8  173.245.63.179  1.212 ms  173.245.63.215  2.028 ms  173.245.63.199  1.961 ms


9  one.one.one.one (1.1.1.1)  0.728 ms  0.866 ms  0.784 ms
```


Ten seconds later, you know the route and that the destination is reachable. (The 240.x through 244.x early hops are AWS-internal address space, which is why they look unusual. Nothing to worry about there.)


That’s also traceroute’s limit. Three probes per hop is a coin flip, not a statistic. A router dropping 5% of packets or adding occasional latency spikes will sail through a single traceroute looking perfectly healthy.


## What MTR adds


mtr runs the same TTL trick continuously, typically one probe per second, and accumulates per-hop statistics: loss percentage, average/best/worst RTT, and standard deviation (a proxy for jitter). Here’s the same path, this time as a 100-cycle report:


```text
$ mtr --report --report-cycles 100 1.1.1.1


HOST: ip-172-31-29-48       Loss%   Snt   Last   Avg  Best  Wrst StDev


1.|-- 244.5.11.111          0.0%   100    9.8  16.2   0.9 118.5  22.7


2.|-- 240.0.52.130          0.0%   100    0.4   0.3   0.3   0.6   0.1


3.|-- 242.0.202.83          0.0%   100    1.1   2.3   0.4  33.3   5.2


4.|-- 240.3.180.37          0.0%   100    2.1   2.0   1.9   2.4   0.1


5.|-- 240.64.220.128        0.0%   100    2.5   0.9   0.6   2.5   0.3


6.|-- 52.46.166.150         0.0%   100    1.5   1.5   1.4   2.9   0.2


7.|-- 52.46.166.151         0.0%   100    0.8   0.9   0.8   2.9   0.2


8.|-- 173.245.63.112        0.0%   100    1.6   3.2   0.9  20.4   3.9


9.|-- one.one.one.one       0.0%   100    1.2   1.0   1.0   1.5   0.1
```


After 100 cycles you’re no longer looking at a snapshot, you’re looking at a baseline. Every hop is at 0% loss, so the path is genuinely clean end to end. Notice hop 1, though: a Best of 0.9 ms but a Worst of 118 ms and a high standard deviation. That’s a single early spike, not a sustained problem, and reading the spread rather than just one number is a skill the snapshot can’t teach you. This is the difference between “the path looks fine” and “hop 7 drops 3% of packets every time.”


## When traceroute is enough


Reach for traceroute first when the question is binary or topological:


The destination is unreachable and you want to know where the path dies. You changed routing and want to confirm traffic takes the new path. You’re documenting the route for a ticket or a diagram. You need a quick sanity check before digging deeper.


If traceroute answers your question, you’re done. No need to babysit a live display.


## When MTR is better


Reach for mtr when the problem is intermittent or performance-related, the two things a snapshot can’t capture:


Users report occasional slowness, drops, or VoIP quality issues. You suspect loss or jitter but a single traceroute looks clean. You’re escalating to an ISP and need evidence: mtr –report –report-cycles 100 produces a shareable summary that carries far more weight than a screenshot of one trace.


A good rule of thumb: traceroute tells you where the path goes, mtr tells you how the path behaves. Reachability question, run traceroute. Behavior question, run mtr.


## Common mistakes interpreting results


**Panicking over single-hop loss.** This is the big one. Here’s a trace toward baidu.com that looks alarming in the middle:


```text
$ mtr --report --report-cycles 100 baidu.com


HOST: ip-172-31-29-48       Loss%   Snt   Last   Avg  Best  Wrst StDev


1.|-- 242.2.27.193          0.0%   100   59.3  59.8  58.3  76.9   3.1


2.|-- 15.230.88.114         0.0%   100   59.7  61.3  58.3 109.8   8.0


3.|-- 218.30.53.149         3.0%   100   60.3  66.9  58.5 176.7  25.2


4.|-- 202.97.27.209        71.0%   100  209.2 209.2 209.0 209.5   0.2


5.|-- 202.97.51.162        94.0%   100  198.8 198.8 198.7 198.8   0.0


6.|-- 202.97.24.133        89.0%   100  223.6 225.2 222.6 234.0   3.3


7.|-- ???                  100.0   100    0.0   0.0   0.0   0.0   0.0


8.|-- ???                  100.0   100    0.0   0.0   0.0   0.0   0.0


9.|-- 27.129.17.230        17.0%   100  246.2 247.7 243.3 257.4   3.2


10.|-- 222.222.85.102       12.0%   100  235.9 240.3 235.5 313.6  13.8


...


15.|-- 124.237.177.164      15.0%   100  237.5 237.1 236.4 238.2   0.4
```


Hops 4 through 6 show 71 to 94% loss, which is panic territory, until you look downstream. If those China Telecom routers were really dropping 90% of traffic, hops 9, 10, and 15 couldn’t possibly recover to 12 to 17%. That recovery is the proof: hops 4 through 6 were rate-limiting their own ICMP replies, not dropping transit traffic. Loss only counts when it persists to the end of the trace. The ??? hops in the middle are the same story, routers that don’t answer probes at all, and are equally harmless when the trace continues past them.


**Panicking over latency jumps.** RTT climbing from single digits to 200+ ms between hops, as it does above when the path crosses into China, usually means your packet crossed an ocean, not that a router is struggling. Judge latency against geography and against the final hop.


**Blaming the network when it’s your own end.** The opposite trap. Loss or wild latency that shows up on hop 1 and persists through every hop, including the destination, points at your own host or LAN, not the path. An agent on a congested Wi-Fi link produced this against the very same 1.1.1.1 that was flawless above:


```text
1.|-- _gateway              6.0%   100  482.4 950.5   4.4 9248. 2200.0


2.|-- 192.168.1.254        20.0%   100  381.3 912.4   4.5 9144. 2186.9


...


9.|-- one.one.one.one       0.0%   100   13.8 1049.  10.7 9654. 2190.0
```


The destination is at 0% loss, so the path is fine, but every hop shows second-long average RTTs against a Best of about 11 ms. A fast Internet path can’t hang off a gateway that’s already a second away. The problem is local.


**Trusting one direction.** Internet routing is asymmetric. Loss that appears near the far end of your trace may live on the return path, inside a network your forward trace never touches. When it matters, trace from both ends.


**Assuming there’s only one path.** Both tools present the network as a single chain of hops. In reality, ECMP load balancing means your probes, and your users’ traffic, may be hashed across multiple parallel paths, and a classic trace silently mixes them together. We covered how[Dublin Traceroute](https://netbeez.net/blog/linux-tracepath-analysis-dublin-traceroute/) solves this by controlling the flow hash, and newer tools like[ttl](https://netbeez.net/blog/ttl/) bring ECMP awareness and automatic rate-limit detection to an interactive mtr-style display.


## Beyond the CLI


Both tools share one more limitation: they only run when you’re at the keyboard, after users have already complained. NetBeez agents run traceroute continuously from every location you monitor, and the Path Analysis feature, built on dublin-traceroute, maps ECMP paths between agents and targets over time, flagging route changes, per-hop loss, and latency shifts before they become tickets. It’s the mtr workflow, automated and running from the user’s perspective around the clock.


Get your free trial


now


Monitor your network from the user perspective


You can share


[Twitter](http://twitter.com/share?url=https://netbeez.net/blog/mtr-vs-traceroute/)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://netbeez.net/blog/mtr-vs-traceroute/)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://netbeez.net/blog/mtr-vs-traceroute/) Copy link


Link copied
