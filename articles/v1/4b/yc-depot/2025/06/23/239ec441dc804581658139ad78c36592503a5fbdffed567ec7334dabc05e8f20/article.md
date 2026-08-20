---
schema_version: "1.0.0"
document_id: "239ec441dc804581658139ad78c36592503a5fbdffed567ec7334dabc05e8f20"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/now-available-egress-filtering-for-github-actions-runners"
published_at: "2025-06-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:39.872607+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:6c1dbe67579400f52762a6539dd1ed86fb1ee31865795fca0b18b81c04e71c9f"
---

# Now available: Egress filtering for GitHub Actions Runners

We're excited to launch egress filtering for[Depot GitHub Actions Runners](https://depot.dev/products/github-actions) . This gives you fine-grained control over which external services your runners can talk to so you can tighten security without giving up performance or flexibility.


Whether you're securing a critical production pipeline or just want to lock things down by default, egress filtering gives you a powerful new layer of control in your CI setup.


## What is egress filtering?


Egress filtering allows you to control what external services your Depot Github Actions Runners can connect to. It works by configuring the host running your GHA workflow to deny access to specific IPs and hosts. This can help to make data exfiltration more difficult, and overall decrease the attack surface on your infrastructure.


### How do I use it?


Heading into your organization's settings page, you'll notice a new option in the Github Actions Runners section: "Egress Rules".


By default, Depot Runners will allow outbound connections to any external service. However, you can set the default rule, "*", to either "Deny" or "Allow" by default. You can also add specific rules to allow or deny connections to specific IPs, CIDRs, or hostnames. Below is an example set of rules to get a docker build with golang working:


Access egress filtering settings in depot github actions runners


This example first applies a blanket deny rule, which blocks all outbound connections by default. Then, it allows connections to the following:


- ` auth.docker.io` and` docker.io` for Docker Hub authentication and registry access
- ` sum.golang.org` and` proxy.golang.org` for Go modules and proxy access
- ` storage.googleapis.com` for Google Cloud Storage access


Anything that resides outside of these rules will be blocked by default. This means that if you try to connect to an external service not listed in the rules, the connection will be denied.


**Note:** Egress filtering has default rules that allow connections to Depot's control plane and GitHub Actions service IPs. This ensures that your runners can still communicate with essential services while applying your custom rules.


#### Limitations of egress filtering for GitHub Actions Runners


While filtering egress rules works great for most users, there are a few limitations to keep in mind. For one, using Tailscale and egress filters at the same time is unsupported, as both modify network config in ways that are incompatible. Additionally, any process that's given root access can modify the egress filter rules, so it's important to ensure that untrusted processes don't run with higher privileges than necessary.


## How it works


### ` iptables` and` /etc/hosts`


Depot's egress filtering system operates at the network layer using iptables on Linux runners. When a runner starts up, our agent process creates a custom iptables chain called` DEPOT_FILTER` in the filter table. All outbound traffic is routed through this custom chain, where filtering rules are applied.


The filtering rules are processed in the following order:


1. **Allow all loopback traffic** (127.0.0.1, ::1) to prevent breaking localhost services
2. **Apply deny rules first** - denied IPs and CIDR blocks take precedence
3. **Apply allow rules** - explicitly permitted IP addresses and CIDR blocks
4. **Apply default policy** - either ALLOW or DENY all other traffic


One issue we ran into while implementing this feature was with servers that implemented round-robin DNS. As an example, resolving api.github.com can give multiple different IP addresses, even when resolving back to back:


```text
$   dig   +short   A   api.github.com
140.82.114.6
$   dig   +short   A   api.github.com
140.82.113.6
```


To work around this, we write a new entry to /etc/hosts after resolving each hostname in your egress rules settings. This allows us to ensure that the IPs we resolve are always the same, and that we can filter them correctly.


## Keeping everything running smoothly


To ensure that runners can still connect to some necessary services, we automatically add certain IPs and hosts to the allowlist:


- **depot.dev domains**
- **GitHub Actions service IPs**
- **AWS service IPs**


Ensuring that` depot build` works was a bit trickier. Under the hood,` depot build` asks our API for a new BuildKit machine for a project, and the API responds with that machine's IP address. The IP for each builder can be one of many that AWS allocates to EC2 instances, so we can't use the same strategy of whitelisting a couple of critical IPs. Instead, we have a process that listens for new BuildKit IPs to add to the allowlist.


## The End


Egress filtering is available today across all Linux runners for **all customers** . We're already working on what's next including monitor mode, Windows and macOS support, and even more controls for securing your pipelines.


Ready to get started? Head to your[organization's settings](https://depot.dev/sign-in) to try it out. Have feedback or ideas? Join us in the[Community Discord](https://discord.gg/XpTfcVrr46) .


## Related posts


- [Making EC2 boot time 8x faster](https://depot.dev/blog/faster-ec2-boot-time)
- [Faster GitHub Actions with Depot](https://depot.dev/blog/faster-github-actions)
- [Now available: Gocache v2 for improved Golang build performance](https://depot.dev/blog/now-available-gocache-v2-faster-improved-golang-build-performance)
- [Now available: Depot ephemeral registries](https://depot.dev/blog/depot-ephemeral-registry)
- [Building Docker Images in CircleCI with Depot](https://depot.dev/blog/build-docker-images-in-circleci)


Billy Batista


Software Engineer at Depot
