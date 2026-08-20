---
schema_version: "1.0.0"
document_id: "758096f0d059cfe63f1332e0dd7ed1f575b685ae2fc9ae2dd97a29883d2319a0"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/access-apps-without-vpn/"
published_at: "2026-07-24T00:00:00+00:00"
first_seen_at: "2026-07-25T04:50:34.285709+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:50e231ff3ae1ad72a0274921a8e46da84799ab6bafae59bc39d22986d0f2d99d"
---

# How to Access Internal Web Apps Without a VPN

Most organizations start with a VPN when they need to give employees access to internal web applications. The VPN places the user on the internal network, and from there, they can reach the app. This approach works when the user base is small and the number of internal apps is limited. But as teams grow, compliance requirements increase, and the mix of technical and non-technical users increases, a VPN’s network-level connectivity introduces new operational and security challenges.


**Read this guide to learn:**


- Why VPN connectivity can be sufficient at first, but causes difficulty as teams grow
- How an identity-aware proxy simplifies network access while reducing security risk
- When to reconsider your organization's internal application access setup


Identity-based access through an identity-aware proxy offers a direct alternative. Instead of placing users on the network, it authenticates them to individual applications, enforces per-app authorization, and produces a single audit trail tied to named identities. In this blog, we’ll examine where VPNs fall short for internal web app access and why identity-based access tools are a stronger alternative.


## The limits of VPNs for web application access


Narrowing the scope of access for individual users on a VPN is tedious and time-consuming, and as a result, is often left undone. That has a few consequences.


**Excessive privileges:** VPN access policies tend to be coarse, with most organizations choosing to define a handful of VPN profiles (e.g., "engineering," "all-staff") and then mapping users to one. However, within those profiles, users are often able to reach every host and port the profile allows, regardless of whether they need that access for their actual work. Refining those profiles per-user or per-app requires maintaining complex firewall rules or network ACLs that[drift over time](https://goteleport.com/blog/how-teleport-simplifies-just-in-time-access/) .


**Stale credentials:** VPN credentials often live far beyond their intended usage windows, and may be shared across multiple users. This means that revoking a single user's access can require manually reissuing a shared credential for an entire group, or waiting for the next certificate rotation cycle.


**Incomplete audit data:** A VPN log entry typically records that a user connected at a certain time from a certain IP. However, these logs may not record which internal application they opened, what actions they performed, or how long they spent in any given app. Compliance or[regulatory requirements](https://goteleport.com/use-cases/compliance/) — like SOC 2 (CC6.1-CC6.3), ISO 27001 (Annex A.5.15), and others — often require HTTP requests or session events to be attributed to a named user and specific applications, which VPNs are frequently unable to do.


## VPN-based vs. identity-based access


Access without a VPN means users can stop joining the network without clear knowledge of which apps or systems they should have access to. Instead, a user (and ideally their device) can be authenticated and granted access to a specific app, but nothing more. Instead, the user opens the app in a browser, signs in with an identity they already have (biometric, machine, etc.), and never joins the underlying network. This ensures that access follows identity and policy instead of an IP address.


From a network perspective, this is a significant difference. With a VPN, the user's device gets an IP on the internal network and can reach anything routable from that subnet. Restricting access within the VPN requires maintaining additional segmentation and rules. With identity-based access, the user never receives a network-level address on the internal network. Instead, the proxy or gateway evaluates each request against policy before forwarding it to the app.


## What an identity-aware proxy for internal web app access looks like


This approach is the strongest alternative to VPN-based internal web app access: an identity-aware proxy. This requires placing a proxy in front of internal apps so that every request passes through it, allowing the proxy to verify who the user is, what they're allowed to reach, and even whether or not their device is trusted, before allowing access to the app.


Teleport's identity-aware proxy includes the following capabilities.


- **SSO with MFA.** Users log in through Okta, Entra ID, or Google Workspace using the identity they already have, removing the need to manage or store app passwords along with the security risks that come with it.
- **Browser-based access.** Connecting to a web app requires only a browser tab, with no client rollout or support tickets needed.
- **No internet-exposed app.** An agent next to the app dials outbound to the proxy, so there are no open inbound ports or exposed public IPs.
- **Per-app, least-privileged access.** Access is granted by role and can be[time-bound or requested just-in-time](https://goteleport.com/blog/how-teleport-simplifies-just-in-time-access/) instead of standing indefinitely.
- **One audit trail.** Every access event ties to a named human or non-human identity and lands in a unified audit log that exports to a SIEM.


Since the same platform already handles SSH, Kubernetes, and databases for engineers, internal web apps become one more resource type under the same system.


## Deciding what fits


The best practice is to consider your users, not your tooling. Establish which users need infrastructure access versus those who only need a web app or two; that differentiation is usually where cost and complexity accumulate. Next, consider where the audit trail is fragmented, since that's often what causes teams to reevaluate their setup. If both considerations point in the same direction, moving to VPN-free, identity-based access is a logical next step.


Teleport already covers SSH, Kubernetes, and database access for engineers; extending it to internal web apps gives you one login, one policy set, and one audit trail for the whole organization.


#### Still using bastions and VPNs?


Learn how[Teleport replaces both](https://goteleport.com/use-cases/vpn-bastion-alternative/) with identity-based access for engineers and non-technical users alike.
