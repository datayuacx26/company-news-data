---
schema_version: "1.0.0"
document_id: "d407e373dc46b863ea6b3d308033ccf46cb9b1383a448a57aad688bbc289f567"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/residential-proxies"
published_at: "2026-06-11T15:09:22+00:00"
first_seen_at: "2026-07-20T03:32:39.128259+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:fda696bf47a9d08c8efedc5bced0f7871368c06c6f8d84eaa12cd4385242bf45"
---

# Who's Really Using Your Home Internet Connection?

By[Jan Rubín](https://www.gendigital.com/blog/authors/jan-rubin)


Since January 2026, we have detected 7.4 million malicious incidents tied to residential proxy traffic, affecting 572,000 users in our telemetry. In this model, the household whose connection is being used is often not the buyer of the traffic. It is the exit point.


That is the uncomfortable part of residential proxy networks. A home router, laptop, phone, smart TV, or other always-on device can quietly become part of a commercial network that routes strangers’ internet traffic through ordinary consumer IP addresses. To the websites on the other end, that traffic looks as if it came from a real household, because it did.


The technology itself is not inherently malicious. Businesses use residential proxies for market research, ad verification, brand protection, security testing, and services that need to see the web as a real user would. But the same feature that makes these networks useful also makes them attractive for abuse: traffic from a household IP address is hard to block without risking false positives against legitimate users.


The problem for consumers is simpler. Many people do not realize their device or connection can be enrolled into this kind of network at all. Some users opt in through pay-for-bandwidth apps. Others are enrolled through “free” VPNs, bundled software development kits inside unrelated apps, or unwanted software that installs proxy components without meaningful consent.


Here is how the market works, who pays for it, and what it can cost the household that unknowingly hosts it.


## What is a residential proxy


A residential proxy is an internet connection – typically a home laptop, phone, smart TV, or router – that has been enrolled into a third-party network and is being used to route someone else's traffic. To the rest of the internet, that traffic looks like it is coming from an ordinary household IP address assigned by a consumer ISP, not from a datacenter or VPN provider.


Residential proxy networks are sold commercially. The model is two-sided: some people pay to tunnel their traffic out through someone else's connection (much like a VPN, except the exit point is a real home), while others get paid to provide those exit endpoints. Some are structured as paid peer-to-peer networks, where any participant's connection can serve as an exit for any other.


Buyers pay for access to pools of millions of these "real user" IPs because they are extremely difficult to block, or rather, blocking them carries real consequences: the traffic originates from genuine households, so any block risks shutting out a legitimate user – effectively a false positive.


Common buyers include market research firms, price-comparison services, brand-protection vendors, security teams testing their own defenses, and consumer privacy tools. At the abusive end of the market, the same infrastructure used by operators running automated purchasing bots, scrapping campaigns, ad fraud, credential stuffing, phishing and other attacks where hiding the true origin of the traffic matters. Demand is also being shaped by large-scale web scraping, including[scraping for AI training data](https://blog.includesecurity.com/2026/06/the-smart-tv-in-your-livingroom-is-a-node-in-the-aiscraping-economy/) . As anti-bot systems become better at identifying traffic from datacenters and known VPN providers, residential IP addresses become more attractive because they look like ordinary users. That does not make every residential proxy user malicious. It does mean consumers may be carrying traffic for businesses and actors they have never heard of.


## How consumer devices end up in these networks


There are several ways a home device can become part of a residential proxy network. Some are explicit. Others are easy to miss.


The most obvious route is the pay-for-bandwidth model. Apps such as Honeygain, IPRoyal Pawns, and PacketStream pay users to share their internet connection. The user opts in, but they may not understand who will buy access to that connection later, what kind of traffic will pass through it, or what happens if that traffic triggers abuse reports.


A second route is the “free” VPN or proxy app. Instead of charging a subscription, the app monetizes the user’s connection by reselling bandwidth. In practice, the user may think they are getting a privacy tool while also becoming infrastructure for other people’s traffic.


A third route is through bundled software development kits, or SDKs. A developer adds a third-party component to an otherwise unrelated app, such as a flashlight, wallpaper, game, or utility. The developer gets paid when the app is active. The user sees the host app, not the proxy network behind it.


The fourth route is malware or potentially unwanted software. In those cases, the proxy component is installed silently or deceptively, with no meaningful user consent at all.


## Why the "consent" is rarely meaningful


Even when consent technically exists, it is often not the kind of consent most people would recognize.


The disclosure may be buried in a long terms-of-service document. The wording may be vague, with phrases such as “share your unused bandwidth,” “support the app,” or “join our network.” These phrases do not clearly tell the user that arbitrary third parties may route traffic through the user’s home IP address.


There is also the question of downstream use. Users are rarely told who buys access to the network or what those buyers do with it. Their connection could be used for benign market research. It could also be used for scraping, automated account creation, ad fraud, credential stuffing, or phishing infrastructure.


The problem gets worse when the proxy component sits inside another app. The host app’s privacy policy may mention a partner SDK by brand name, but that still does not make it clear to a normal user that their device can become a proxy endpoint.


Uninstalling the visible app may not always resolve the issue cleanly either, especially when proxy components are installed separately or bundled with other unwanted software.


## The risks consumers actually carry


When a household device becomes a residential proxy node, the user takes on risks created by someone else’s traffic.


The first risk is attribution. Abusive, fraudulent, or illegal activity routed through the device appears to come from the household’s IP address. ISP abuse notices, platform bans, fraud checks, and law enforcement requests point first to the subscriber, not to the person who bought the proxy traffic.


The second risk is performance. Continuous third-party traffic can consume bandwidth, slow the home network, hit data caps, or violate ISP terms of service. The user may only notice that the internet feels slower, a bill is higher, or a service suddenly blocks access.


The third risk is security exposure. Proxy clients run persistent background processes and maintain network connections so they can relay traffic. That adds always-on networking software to the device, often with limited visibility for the user.


The fourth risk is reputation. A home IP address used for abuse can end up on blocklists used by banks, retailers, streaming services, and online platforms. The result can be account friction, failed logins, extra verification steps, or outright blocking from services the household actually uses.


There is also a family angle. A child or another household member may install a free game, VPN, or utility without understanding the consequences. The legal, financial, and reputational risk still lands on the account holder.


## What our telemetry shows


Since the beginning of 2026, we have detected 7.4 million malicious traffic incidents associated with residential proxy traffic, affecting 572,000 unique users in our user base.
This is not a measurement of the entire residential proxy market. It reflects what we observe in our telemetry: malicious traffic, detections, and related activity affecting protected users. The real footprint is far larger, because residential proxy networks span major consumer platforms and many devices outside our visibility.


The activity is global, but it is concentrated in a smaller set of countries. India leads by a wide margin, with roughly 83,000 affected users, followed by Vietnam with 61,000 and Brazil with 39,000. The Philippines (23,000), Poland (18,000), Indonesia (18,000) and Thailand (14,000) form a second tier, with Egypt, Spain, Ukraine, Argentina, Mexico and Italy each accounting for 10,000 to 13,000. The spread across South and Southeast Asia, Latin America, and Eastern and Southern Europe reflects where the host apps that carry this software are most widely installed.


That geographic spread likely reflects a combination of app distribution, local install patterns, user appetite for free or pay-for-bandwidth apps, and the economics of bandwidth resale. It should not be read as a simple ranking of where residential proxy abuse originates.


The chart below breaks down the threats we observed in this proxy-related traffic. Two categories dominate: phishing and malicious advertising. Behind them sits a long tail of consumer-facing fraud, including e-shop scams, generic scams, financial scams, and fake-tutorial pages, alongside outright malware such as trojans and other malicious traffic.


In the malicious proxy traffic we observed, scams and malware dominate. The important point for consumers is not only that their connection may be used by someone else. It is that, when abuse happens, their household IP address can be the return address.


## What consumers can do


Most people will not identify a residential proxy component by name. But there are practical warning signs.


Review “free” VPNs, proxy apps, bandwidth-sharing apps, and utilities from unknown publishers. Check whether they mention bandwidth sharing, peer-to-peer networking, traffic routing, or network participation in their terms. If an app offers payment for leaving your device online, assume your connection may be resold unless the provider explains exactly how traffic is vetted and who can buy access.


On phones and computers, remove apps you do not recognize or no longer use. On home routers, check the list of connected devices and look for anything unfamiliar. If your ISP sends abuse notices, websites start blocking you, or your home IP address suddenly fails fraud checks, a proxy component is one possible explanation.


Security software can also help by detecting unwanted proxy clients, bundled components, and related malicious activity. But this should not be a guessing game for consumers. Software that turns a household connection into a commercial exit node should be named clearly and made easy to remove.


## The takeaway


With residential proxiesthe user’s connection becomes an infrastructure for someone else’s business.


The issue is not that residential proxy technology exists. It has legitimate uses. The issue is whether people know when their devices are part of it, understand what kind of traffic may pass through their connection, and have a real choice to opt out.


The fix is not to pretend consumers will read every EULA. This software should be named clearly, shown plainly, and made easy to remove before a household connection becomes someone else’s infrastructure.


More on this topic


[Fake invoices are moving from inboxes to shopping apps – From Research](https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps)


[Inside Vidar’s ABE Bypass: From Memory Scanning to APC Injections – From Research](https://www.gendigital.com/blog/insights/research/inside-vidar-abe-bypass)


[Your flight was cancelled. Is the refund message real? – From Research](https://www.gendigital.com/blog/insights/research/flight-cancelled-refund-message-scam)


[Jan Rubín](https://www.gendigital.com/blog/authors/jan-rubin)


Threat Research Team Lead


Follow us for more
