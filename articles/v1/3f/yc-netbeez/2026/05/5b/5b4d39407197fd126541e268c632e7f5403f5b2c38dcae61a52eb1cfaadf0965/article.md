---
schema_version: "1.0.0"
document_id: "5b4d39407197fd126541e268c632e7f5403f5b2c38dcae61a52eb1cfaadf0965"
company_key: "yc-netbeez"
company: "NetBeez"
source_id: "yc-netbeez-rss-1a1c74f0723d"
canonical_url: "https://netbeez.net/blog/ttl/"
published_at: "2026-05-27T20:01:11+00:00"
first_seen_at: "2026-07-20T23:24:05.097992+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:bafb6fbe993a926b3e54e31d69b0c48b1db80cb0cd15d7dd1921ea9f80e0313a"
---

# ttl: A Modern Traceroute Tool for Network Engineers

Traceroute has been a staple of network diagnostics for decades. Whether it’s` traceroute` on Linux,` tracert` on Windows, or the more capable` mtr` , these tools have barely changed. They probe the path to a destination, show hops, report RTT, and flag timeouts.


But as networks got more complex with ECMP load balancing, MPLS tunnels, carrier-grade NAT and multi-AS paths the classic tools started showing their age.` mtr` is a big improvement over` traceroute` , but it still doesn’t understand ECMP paths, can’t distinguish rate-limited hops from real packet loss, and has no enrichment beyond reverse DNS.


[ttl](https://github.com/lance0/ttl) fills that gap. It’s an open-source, Rust-based traceroute with a real-time CLI, per-hop statistics, ASN and GeoIP enrichment, ECMP detection, MPLS label parsing with a rate-limiting packet loss detection heuristic.


## How It Compares to Existing Tools


` mtr` is the obvious benchmark here. Both tools show a live, updating view of the path to a destination. But ttl goes significantly further:


**ECMP awareness.** Standard traceroute and mtr treat the network as a single path. In practice, most ISP and enterprise backbones use Equal-Cost Multi-Path routing, meaning your packets may be distributed across multiple parallel links. ttl’s` --flows flag` (Paris/Dublin-style probing) sends probes with varying source ports to discover multiple ECMP paths simultaneously. We’ve covered ECMP path analysis in depth in[our post on Dublin Traceroute](https://netbeez.net/blog/linux-tracepath-analysis-dublin-traceroute/) , and ttl brings that same capability to a live interactive tool.


**Intelligent loss classification:** A perpetual frustration with mtr is the loss% column: you’ll see 50% loss at a transit hop, panic, then notice the destination is perfectly reachable. Is that real loss or just an ICMP rate limiter? With mtr, you’re guessing. With ttl, you don’t have to.


**Rich enrichment:** Per-hop ASN lookup (via Team Cymru DNS), reverse DNS, optional GeoIP with MaxMind GeoLite2, and Internet Exchange detection via PeeringDB – all inline in the CLI.


**Protocol flexibility:** ICMP, UDP, and TCP probing modes, with auto-fallback if raw sockets aren’t available. TCP mode is particularly useful for tracing through firewalls that drop ICMP.


## Smart Rate Limiting Detection


The most noteworthy feature in ttl is how it handles the classic “is this real loss or ICMP rate limiting?” problem. ttl uses three heuristics to make the call automatically:


**Isolated hop loss.** If hop N shows 50% loss but all downstream hops (including the destination) show 0% loss, that’s rate limiting since real congestion would affect everything downstream too.


**Uniform flow loss.** In multi-flow mode (` --flows` ), if all flows lose packets equally at a hop, it’s likely hop-level ICMP rate limiting rather than path-specific congestion.


**Stable loss ratio.** Rate limiting produces consistent loss over time (e.g., a steady 50%). Real congestion tends to fluctuate.


When ttl detects rate limiting at a hop, it appends an RL suffix to the loss percentage in the CLI, so instead of seeing an alarming` 50%` and wondering, you see` 50%RL` on the Loss column and know to move on. This alone saves real time during incident triage.


## ECMP Detection in Practice


When you run ttl` --flows 8 -p tcp cloudflare.com --src-port 33000` , ttl sends eight parallel probe streams with different source ports. Because ECMP routers hash on the 5-tuple (src IP, dst IP, src port, dst port, protocol), different source ports can land on different paths through the network.


ttl marks hops that follow multiple paths. For example, in the screenshot below at hop 8 we can see the ECMP paths detected, along with other statistics. This is conceptually identical to what Dublin Traceroute does, but interactive and real-time.


## Common Use Cases


**ISP path debugging:** Run` ttl 8.8.8.8 --flows 4 -p udp` to see whether your traffic is load-balancing across multiple uplinks, or if all flows are taking the same congested path.


**Loss triage during incidents:** When alerted to packet loss on a path, ttl’s RL detection tells you immediately whether you’re dealing with a real problem or just a router rate-limiting ICMP TTL-exceeded responses.


**QoS validation:** The` --dscp` flag lets you send probes with specific DSCP markings and observe whether they’re treated differently hop-by-hop which is useful for validating voice or video traffic prioritization.


**Multi-target tracing:** Pass multiple hostnames or use` --resolve-all` to trace all A records simultaneously which is handy for CDN or anycast path comparisons.


## Installation


ttl ships as pre-built binaries for Linux (x86_64, aarch64), macOS (Intel and Apple Silicon), with Homebrew and Cargo installs also supported:


```text
brew install ttl        # macOS


cargo install ttl       # Any platform with Rust
```


Raw socket privileges are required (run as root or with` CAP_NET_RAW` on Linux).


## NetBeez and Path Analysis


For teams that need continuous, automated path monitoring rather than on-demand CLI tracing, NetBeez provides built-in traceroute tests alongside Path Analysis, a feature built upon Dublin Traceroute that maps ECMP paths between agents and targets over time. Where ttl excels for interactive, hands-on investigation, NetBeez runs those same diagnostics continuously from distributed agents and alerts when loss increases, or latency degrades.


Get your free trial


now


Monitor your network from the user perspective


You can share


[Twitter](http://twitter.com/share?url=https://netbeez.net/blog/ttl/)[Linkedin](https://www.linkedin.com/sharing/share-offsite/?url=https://netbeez.net/blog/ttl/)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://netbeez.net/blog/ttl/) Copy link


Link copied
