---
schema_version: "1.0.0"
document_id: "def1752eff995222cf1e53ef180cc60eea3557e625a578a49c91705949aef958"
company_key: "yc-firezone"
company: "Firezone"
source_id: "yc-firezone-news-import-75fbee21a5d6"
canonical_url: "https://www.firezone.dev/blog/devlog/2025-09"
published_at: "2025-09-30T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:10.796715+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:42b352fefde98defc939fc85e70cb625ce1c0f081a949e340788c0793d6d7f1a"
---

# September 2025 Devlog

September brought major improvements to Firezone's reliability, performance, and administrative capabilities. Our focus this month was on making connections more resilient, helping admins manage client versions more effectively, and optimizing our networking stack for real-world conditions.


## Smarter Admin Tools


Managing a fleet of Clients just got easier. The admin portal now shows client versions directly in the clients table with a sortable version column.1 When planning Gateway upgrades, admins can quickly identify which clients might lose connectivity due to version incompatibility.


We've also enhanced the "outdated Gateway" email notification system, which is now enabled by default for all accounts.2 These emails now include a count of clients that will be affected by the Gateway upgrade, with a direct link to view them in the portal.


For organizations with large user bases, we added batch upsert operations for directory sync.3 This lays the groundwork for more efficient directory synchronization that can handle thousands of users without overwhelming the system.


A new` /account` API endpoint provides programmatic access to billing details and seat usage.4 This enables better integration with capacity planning and billing automation tools.


## Rock-Solid Connections


Connection reliability saw significant improvements across the board. We implemented graceful connection shutdown using a new peer-to-peer control protocol message.5 When Gateways restart for maintenance or upgrades, Clients now immediately fail over to alternative Gateways instead of waiting through a 15-second ICE timeout.6


This graceful handling also applies when Clients sign out, making it much easier to distinguish between actual network problems and normal disconnections in the logs.


We fixed a synchronization issue where Clients and Gateways could disagree on authorization state.7 Now when a Client receives an ICMP "prohibited" error from a Gateway, it automatically re-authorizes access for that Resource. This keeps both sides in sync even when authorization states drift out of alignment.


DNS resource handling got more reliable too. Proxy IP assignments for DNS resources now persist across Client sessions.8 Previously, restarting the Client could reassign` 100.96.0.1` to a different resource, breaking applications that cached the IP address. Now these mappings stay consistent for the lifetime of the tunnel process, fixing issues with long-lived connections like SSH sessions.


## Better DNS Resolution


The Gateway switched from using the system's` libc` resolver to the` hickory-resolver` library for A and AAAA record resolution.9 This change enables proper TTL-based caching instead of the previous hardcoded 30-second cache. DNS responses are now cached for exactly as long as the authoritative nameserver specifies, reducing unnecessary queries while ensuring timely updates.


## Performance Under Load


We optimized the event-loop to prevent starvation of lower-priority inputs.10 The tunnel now batch-processes input from all sources rather than prioritizing high-traffic sources to completion. This prevents timeout checks and DNS resolution from being delayed when UDP sockets or the TUN device are extremely busy.


Linux systems got a significant performance boost through better socket buffer management.11 Clients and Gateways now attempt to set UDP receive and send buffer maximums to 128 MB and 16 MB respectively at startup. The default 200KB buffers were causing packet drops during high throughput scenarios, directly correlating with buffer errors visible in` nstat` .


We also limited the number of optimistic ICE candidates to prevent CPU spikes.12 When clients advertise many IPv6 addresses, the previous unlimited candidate generation could cause performance issues. Optimistic candidates are now disabled entirely for IPv6 and limited to 2 for IPv4, focusing on the scenarios where they're most beneficial.


## Relay Infrastructure Advances


Our relay infrastructure received important eBPF improvements. The relay can now handle relay-to-relay candidate pairs in the eBPF kernel, which occur when both Client and Gateway allocate from the same relay.13 Previously these packets would need to traverse userspace or fail entirely.


We restructured the eBPF code into modular components that handle all cross-stack translation cases, including IPv4↔IPv6 transformations. A comprehensive integration test with double symmetric NAT validates this behavior in continuous integration.


We also fixed several edge cases in the eBPF layer, including properly handling DNS replies14 and re-populating the channel map when TURN channels are refreshed.15


## Platform-Specific Improvements


The Android client now launches authentication in a CustomTab instead of the default browser.16 This fixes a Firefox bug where only the first browser tab could intercept the custom URI scheme, making subsequent sign-ins fail until users manually closed the old tab. CustomTabs ensure only one sandboxed authentication instance exists at a time. It also results in a nice UX improvement where the "You have been signed-in" tabs are no longer lingering around.


macOS developers got quality-of-life improvements making the workspace build correctly on macOS with appropriate stubs and conditional compilation.17


The macOS client now detects and alerts users when multiple instances are running, preventing interference with tunnel state.18


## Infrastructure and Tooling


All CI runners now use Ubuntu 24.04 to match production relay environments.19 This ensures builds and tests run on the same kernel version as production, catching platform-specific issues earlier.


The docker-compose test environment now uses realistic network topology with separate subnets for Clients, Gateways, relays, and backend.20 Each component has a dedicated router container performing NAT and firewall rules, enabling proper testing of relayed connections with port randomization.


Database performance improved with the addition of missing indexes on foreign key columns.2122 These indexes ensure efficient cascade deletes now that hard-delete has been fully rolled out.


## Notable Bug Fixes


Several important bugs were squashed:


- Gateway re-joins Phoenix channel topic on send errors to prevent message loss23
- Fixed poll-after-completion panics in Client session event-loop24
- Fixed DNS resource NAT reset when Client reassigns proxy IPs after sign out25
- Relay filters traces by log filter to respect OTEL configuration26
- Internet site no longer counts against Starter plan resource limits27


---


That's September in a nutshell. See our[changelog](https://www.firezone.dev/changelog) for a more compact version of the above or view the[full diff on GitHub](https://github.com/firezone/firezone/compare/61e0a228869f646d4ae7b4d5ee19ce57f5697df8...f07d2932dcc04441bc16f01febe011131523a39a) .


## About Firezone


Firezone is an open source platform for securely managing remote access to your organization's networks and applications. Unlike traditional VPNs, Firezone takes a granular, least-privileged approach with group-based policies that control access to individual applications, entire subnets, and everything in between.[Get started for free](https://app.firezone.dev/sign_up) or[learn more](https://www.firezone.dev/kb) about how Firezone can help secure your organization.


---


#### Footnotes


## Footnotes


1.


[feat(portal): show outdated clients](https://github.com/firezone/firezone/pull/10456)↩


2.


[feat(portal): enable outdated gateway email](https://github.com/firezone/firezone/pull/10281)↩


3.


[feat(portal): batch_upsert and delete_unsynced functions](https://github.com/firezone/firezone/pull/10369)↩


4.


[feat(api): GET /account API](https://github.com/firezone/firezone/pull/10302)↩


5.


[feat(connlib): gracefully shutdown connections](https://github.com/firezone/firezone/pull/10076)↩


6.


[feat(clients): gracefully close connections on shutdown](https://github.com/firezone/firezone/pull/10400)↩


7.


[feat(connlib): create flow on ICMP error "prohibited"](https://github.com/firezone/firezone/pull/10462)↩


8.


[feat(connlib): persistent DNS resource records across sessions](https://github.com/firezone/firezone/pull/10104)↩


9.


[feat(gateway): use hickory resolver to resolve A/AAAA queries](https://github.com/firezone/firezone/pull/10373)↩


10.


[refactor(connlib): improve fairness of event-loop](https://github.com/firezone/firezone/pull/10347)↩


11.


[feat(linux): try to set rmem_max and wmem_max on startup](https://github.com/firezone/firezone/pull/10349)↩


12.


[fix(connlib): limit the number of optimistic candidates](https://github.com/firezone/firezone/pull/10367)↩


13.


[fix(relay): handle relay-relay candidate pairs in eBPF](https://github.com/firezone/firezone/pull/10286)↩


14.


[fix(relay): XDP_PASS DNS replies](https://github.com/firezone/firezone/pull/10330)↩


15.


[fix(relay): re-add eBPF channel map entry on refresh](https://github.com/firezone/firezone/pull/10291)↩


16.


[fix(android): launch auth in CustomTab](https://github.com/firezone/firezone/pull/10371)↩


17.


[chore: improve macos dev experience](https://github.com/firezone/firezone/pull/10363)↩


18.


[fix(apple): Enforce single Firezone instance](https://github.com/firezone/firezone/pull/10313)↩


19.


[ci: bump Ubuntu runners to 24.04](https://github.com/firezone/firezone/pull/10288)↩


20.


[ci: create a more realistic network setup](https://github.com/firezone/firezone/pull/10301)↩


21.


[chore(portal): add non-composite indexes](https://github.com/firezone/firezone/pull/10396)↩


22.


[chore(portal): add remaining simple indexes](https://github.com/firezone/firezone/pull/10403)↩


23.


[fix(gateway): re-join topic in phoenix-channel on error](https://github.com/firezone/firezone/pull/10397)↩


24.


[fix(connlib): fuse event-loop future inside client session](https://github.com/firezone/firezone/pull/10399)↩


25.


[fix(gateway): reset DNS resource NAT if proxy IPs change](https://github.com/firezone/firezone/pull/10310)↩


26.


[fix(relay): filter traces by log filter](https://github.com/firezone/firezone/pull/10317)↩


27.


[fix(portal): don't count internet site in limits](https://github.com/firezone/firezone/pull/10336)↩
