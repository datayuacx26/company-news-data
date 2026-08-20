---
schema_version: "1.0.0"
document_id: "e0e97dc9a8abf0754aa67650ec7d40c94f0c1f8543f7274e85c0edcec14b5106"
company_key: "yc-plivo"
company: "Plivo"
source_id: "yc-plivo-rss-7fc8cee78b57"
canonical_url: "https://www.plivo.com/blog/low-latency-global-connections/"
published_at: "2022-02-17T00:00:00+00:00"
first_seen_at: "2026-07-21T11:30:36.100142+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:8f19e98fcc4184743a8b292de9f1b636ddc67e0317f994b792fd9069210ee9eb"
---

# How Plivo Maintains Low Latency for Global Connections

One of the advantages of Plivo’s Premium Communications Network is having points of presence (PoP) on five continents around the globe. To understand why that’s important, it helps to think about how data gets around the big ol’ network that we call the internet.


## How the internet works


Have you ever thought about how web pages get to the browser running on your desktop device, or how you’re able to make a voice or video connection with someone across town or around the world?


Each device — and each application on those devices — has an address, and the network they connect to moves packets of information from one address to another.


In order for that to happen, every device on the network must speak a common language. It’s similar to the way we address postal mail: We specify a country, a state, a town, a street, a specific house on that street, a specific apartment in that house, and a specific person in that apartment to get a letter just where the sender wants it to go, and everyone who handles the letter agrees to use whatever parts of that address they need to know about to move the letter along.


In computing terms, that common language is a *protocol* . A protocol consists of some agreed-upon rules and definitions, including how to address data.


The internet uses a protocol called, logically enough, Internet Protocol, or IP. When an application sends data over the internet, it appends an IP header to each packet of data. The header contains address information that network devices — routers and switches — use to forward the packet to its destination.


A packet starts off on a single device — your laptop, for instance. Suppose you click on a link to set up a video call with a relative. Your browser, or whatever application you’re using, knows you want to make a connection, and it knows how to turn your click into a request. Additional software components on your computer turn that request into IP packets, and send them out from your computer to your local network through a physical network interface — a cabled or Wi-Fi network adapter.


The next stop for a packet is your local router or internet access point. It serves as an interface between your local network and the internet at large. It has (at least) two physical network interfaces — one for your local network and one that leads to your internet service provider (ISP).


If your request is for a device on your local network, your router can send the packets there quickly without having to forward them to the internet. If not, they pass through the router and upstream to your internet service provider (ISP).


The ISP’s routers examine the packet header and determine where the next stop is. If its destination is another device serviced by the same ISP, it can forward the packet to that device. Chances are that’s not the case, though — the destination is probably serviced by a different ISP. That means the packet has to pass through another router.


At each interface point, the volume of traffic increases. Your laptop handles packets from several applications — not much work, almost no delay. Your local router handles traffic from several computers in your house or business, so it needs to be a little faster. Your ISP handles traffic from thousands of customers in your area, so its routers and switches need to be faster still. Eventually, an ISP’s traffic has to leave its network.


## What is an IXP?


ISPs exchange traffic with each other at internet exchange points (IXP) — a physical location where multiple organizations connect their networks. ISPs and content delivery networks (CDN) place their own network devices at IXP locations so they can make sending network traffic as fast as possible. Each of these placements is called a point of presence (PoP).


At a global level, IXPs connect with each other using extremely fast networking technology to minimize delays and get packets to their destinations as quickly as possible.


Once it reaches the ISP that services the account of the person you want to communicate with, the packet goes back down the stack — to a local area, to your recipient’s local router, and to the application on their client device. After a few more exchanges of packets back and forth, a connection is made, and you’re talking with your mother-in-law.


One of the key goals in networking is to get packets from one place to another quickly. On any given connection, speed is limited by physical constraints — the speed of light on an optical fiber cable, for instance. Each handoff between networks takes time and introduces delays, or *latency* . ISPs and device manufacturers strive to reduce latency so that network traffic can move as quickly as possible.


Low latency is especially important for connections like voice and video calls. Packets have to move in near real time, because people can perceive (and be annoyed by) delays of even fractions of a second. Any provider that can’t offer low latency comparable to that of a phone call is at risk of losing customers.


Latency is measured in milliseconds, and for most cellular connections latency is about 100-200 ms. So the goal for telecom providers is to keep latency lower than that. But remember, each time a packet goes through a router, it experiences added latency.


## Plivo’s PoPs


To minimize latency, Plivo maintains PoPs at IXPs in seven locations:


- Northern California — North America
- Virginia — North America
- Frankfurt, Germany — Europe
- Mumbai, India — Asia
- Singapore — Asia
- Sydney — Australia
- São Paulo, Brazil — South America


Network traffic moves quickly within Plivo’s[Premium Communications Network](https://www.plivo.com/blog/premium-communications-network/) in each region, and by having local PoPs in these regions,[Plivo](https://www.plivo.com/) can minimize latency for traffic that passes between regions. That’s how Plivo maintains connections with low round trip times.


[Sign up](https://console.plivo.com/accounts/register/) today to get started!
