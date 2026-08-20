---
schema_version: "1.0.0"
document_id: "92bea46047345f0138db7d8c6e4601ee63b3c80e9c4921097995387cb682907d"
company_key: "yc-blacksmith"
company: "Blacksmith"
source_id: "yc-blacksmith-news-import-a006191a9a76"
canonical_url: "https://www.blacksmith.sh/blog/blacksmith-x-tailscale"
published_at: "2025-12-18T00:00:00+00:00"
first_seen_at: "2026-07-23T03:46:48.319296+00:00"
fetched_at: "2026-07-28T22:24:55.411240+00:00"
content_hash: "sha256:0e2c3f80127f93c09d41e0af1b642e928d8c5ff3441d46ece52551faa887233b"
---

# How Blacksmith survives ISP degradation with Tailscale Services

## **The wake-up call**


On Thanksgiving day, we woke up to an outage:[GitHub Actions](https://docs.github.com/en/actions) jobs running on our infrastructure were not able to check out repositories. A large percentage of CI (continuous integration) jobs also interact with other GitHub endpoints, such as packages, API, and[ghcr.io](https://github.com/features/actions) , and they were all timing out as well. The` actions/checkout` steps that normally complete in seconds were timing out after two minutes.


The error messages reported in our support issues offered little insight:


```text
Failed to connect to github.com port 443 after 134053 ms: Couldn't connect to server
```


The problem wasn't with GitHub. It wasn't in our datacenter infrastructure that we had direct control over. It was somewhere in between: an upstream ISP in the path from our infrastructure to one of GitHub's edge nodes had degraded routing. Connections from our datacenter were experiencing five to 20-second HTTP stalls on roughly 7-10% of requests. Meanwhile, the exact same operations from other regions, hitting the same set of GitHub edge nodes, were performing normally.


At our scale, when 7% of GitHub connections stall, that's hundreds of customers reporting issues in their pipelines.


## **Why we couldn't just "fix the network"**


The instinctive response is to find the bad ISP, call them, escalate, and get the routing fixed. That’s easier said than done when your datacenter uses a blend of ISPs, and you have no visibility into which one is degraded. We had to disable them one by one, check if the problem reproduced, then repeat until we found the culprit. That process took 16 hours. But even after identifying the problematic ISP, routing issues like peering disputes,[BGP](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) misconfigurations, and congested links can take days to fully resolve. These aren't problems you solve with a phone call on a holiday weekend.


We needed a disaster recovery solution that:


1. **Works immediately** when routing degrades
2. **Requires zero changes** from customers
3. **Only affects GitHub traffic** , while everything else stays on the default network path


## **Transparent proxying with iptables**


The goal was to be able to flip a switch and route GitHub-bound traffic from our VMs through a pool of proxies, running in a network stack, with direct GitHub peering. During an ISP degradation in our network stack, this would allow us to bypass the problematic hops entirely and reach GitHub over a well-maintained network path.


Here's what we built ( *illustration courtesy of Tailscale, used with permission by Blacksmith* )


When a packet leaves a VM destined for a GitHub IP, the kernel rewrites the destination to our` ProxyManager` process running on the bare metal host. The` ProxyManager` then uses the` SO_ORIGINAL_DST` socket option to recover where the packet was *actually* headed, opens an` HTTP CONNECT` tunnel through Squid, and pipes bytes back and forth. Our cluster of Squid proxies sit inside our tailnet and are addressable by a single stable IP provided by a[Tailscale Service](https://tailscale.com/kb/1552/tailscale-services) . The key insight: we only intercept traffic destined for GitHub. Everything else takes the normal path.


To do this efficiently, we use an` ipset` . This is a Linux kernel data structure that stores a set of IP addresses or network ranges and lets you match packets against the entire set in O(1) time. Without` ipset` , you'd need a separate` iptables` rule for each of GitHub's ~50 IP ranges, and the kernel would check each rule sequentially. With` ipset` , you load all the ranges into a single set, and matching a packet is a single hash lookup. It's the difference between a linear scan and a hash table. We populate our` ipset` with GitHub's published[CIDRs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-githubs-ip-addresses) .


When the ranges change, we swap the` ipset` atomically. Packets in flight continue matching against the old set until the swap completes, then instantly start matching against the new one. With this mechanism there are no dropped connections, and no race conditions.


## **Tailscale Services: load balancing without the load balancer**


The traditional approach to implementing this fix would be: put a load balancer (HAProxy, NGINX, a cloud provider's network load balancer) in front of the Squid pool, expose a static IP, and configure agents to connect to that IP.


We did something different. **Tailscale Services** gave us load balancing, health checking, and encryption, without running a separate load balancer.


Each Squid instance registers itself with the service:


```text
tailscale serve --service=svc:git-proxy --tcp=443 tcp://127.0.0.1:3128
```


That's it. The instance is now part of the` git-proxy` service. Tailscale handles:


- **Health checking** : Unhealthy backends are automatically removed
- **Load distribution** : Traffic spreads across available instances
- **Encryption** : All traffic flows over WireGuard tunnels
- **Authentication** : Only devices in our Tailnet can reach the service


From the agent's perspective, it just connects to` git-proxy` . No IP addresses to manage. No certificates to rotate. No security groups needing holes punched.


The Squid instances have **no public IP addresses for proxy traffic** . They're only reachable via Tailscale. Even if someone discovered the proxy, they couldn't connect without being on our tailnet.


## **What’s next?**


We now have a disaster recovery story for GitHub connectivity. When an ISP has a bad day, traffic automatically flows through the proxy. Customers don't notice. Jobs complete. That's the baseline we wanted. We're actively load testing the proxy so we're better prepared for when this happens again.


The architecture is flexible enough that we could extend it to other critical services if needed. ECR, Docker Hub, package registries—anything that uses a known set of IPs could be routed through a similar proxy in a pinch.


‍
