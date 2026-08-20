---
schema_version: "1.0.0"
document_id: "04183b3cce409c15089baa62e5593f39cd34c7109c83396bd8a2ffcd2874df93"
company_key: "yc-instant-labs"
company: "Instant Labs"
source_id: "yc-instant-labs-news-import-684f5a1736da"
canonical_url: "https://instantdomainsearch.com/learn/updates/how-we-keep-your-searches-private"
published_at: "2026-03-18T00:00:00+00:00"
first_seen_at: "2026-07-25T09:44:30.721881+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:f556e6cd1bbee1b9033497dc837c0cac767fdc7aa30b3642401c90dabddfd9f0"
---

# Introducing Direct Registry Partnerships for 380+ TLDs

Our domain availability data is getting a major accuracy upgrade. We now pull data directly from registries like VeriSign, Google Registry, Radix, and Identity Digital. No middlemen. No retail registrar APIs. Just source-level data flowing into our own systems.


This means better accuracy, faster results, and something we care deeply about: we never send your searches to third-party retail registrar APIs.


## What are direct registry partnerships?


Most domain search tools check availability by querying a retail registrar's API. You type a name, the tool forwards it to GoDaddy or Namecheap's backend, and the registrar returns a result. That registrar now knows what you searched for.


We skip that step entirely.


Registries are the organizations that actually manage TLDs. VeriSign runs` .com` and` .net` . Google Registry operates` .dev` ,` .app` , and` .new` . Radix manages` .fun` ,` .online` ,` .site` ,` .store` , and others. Identity Digital runs hundreds of extensions like` .live` and` .world` . These are the sources of truth for domain data.


We now partner with these registries directly to get:


- **Zone files** from VeriSign, ICANN's Centralized Zone Data Service (CZDS), and hundreds of[gTLD](https://instantdomainsearch.com/learn/guides/what-is-a-top-level-domain) providers. These are daily snapshots of active delegated names for a given extension. We download fresh copies every night.
- **Reserved, blocked, and trademark lists** for 380+ TLDs, with more coming soon. These tell us which domains are unavailable for reasons beyond simple registration, like registry holds or trademark protections.
- **EPP (Extensible Provisioning Protocol) checks** for hundreds of TLDs. When we need real-time verification, we query registries directly using the same protocol that registrars use to register domains. For` .com` , this means a direct EPP query to VeriSign.


All of this data gets processed, indexed, and distributed across our servers worldwide. When you search, your query runs against our local index. No retail registrar is contacted. No retail registrar sees what you typed.


## How a search works


Here's what happens when you type a domain name on our site:


1.


**Your browser connects to our nearest server.** We run on Google Cloud with servers distributed globally. A load balancer routes you to the closest data center so latency stays low regardless of where you are.


2.


**Your query hits our local index.** The search runs against our pre-built zone file and registry data across 800+ TLDs. No retail registrar is contacted. This is what makes results come back in under 25 milliseconds.


3.


**We verify with registries when needed.** For domains that may have changed status since our last nightly index, our check service runs EPP queries directly with registries for hundreds of TLDs over TLS. The registry does see the queried domain name during this check, but registries manage the zone, they don't sell domains. Registrars sell domains. Big difference.


4.


**For the few TLDs without zone file access, we compile availability from DNS.** A small number of extensions don't publish zone files. For those, we run DNS lookups through our resolver infrastructure, which queries authoritative nameservers on our behalf. Similar to EPP, the relevant DNS operators can see the queried domain, but these are infrastructure-level queries, not retail registrar searches.


5.


**Results stream back instantly.** We use[JSON streaming](https://instantdomainsearch.com/learn/research/streaming-json-jsons) to deliver results before the full response is even complete. Your browser connection uses HTTPS, and registry EPP sessions use TLS.


At no point does a retail registrar learn what you searched for. For live verification, the relevant registry operator or DNS resolver can see the queried domain, but these are infrastructure providers that manage the zone, not companies in the business of selling you domains or tracking your search behavior.


## The only time a registrar knows


There's exactly one moment when a registrar learns about your domain interest: **when you click a link to buy it.**


We partner with registrars like Namecheap, GoDaddy, Spaceship, Hostinger, Porkbun, Network Solutions, and Dynadot. When you find a domain you want, we link you to whichever registrar supports that extension at the best price. We call this[smart partner routing](https://instantdomainsearch.com/learn/updates/smart-partner-routing) .


This is how we keep the product free. We earn a referral commission from the registrar when you complete a purchase. That commission is paid by the partner, not added to your price. We don't sell your search data. We don't sell ads targeted to your searches. Our goal is to help you compare registrars and find the cheapest price for whatever extension you want.


## We're ICANN-accredited and privately held


Instant Labs, Inc. is an[ICANN-accredited](https://www.icann.org/registrar-reports/accredited-list.html) domain registrar. ICANN accreditation requires meeting strict technical, financial, and operational standards. For us, it means direct access to registry data, the ability to participate in the domain ecosystem at the highest level, and accountability to the standards that govern how domain data is handled.


We're also a privately held company. We don't answer to outside investors pushing us to monetize user data or sell search analytics. Our incentives are simple: build the best domain search tool, keep it free, and earn commissions when people buy domains through our partner links.


We've been running this service since 2005. Nearly 50 million people have searched for domains on our site. We've never front-run a domain search, never sold search data, and never will.


## We will never register domains you search for


This comes up a lot:


> If I search for a domain on your site, will someone register it before I can?


No. We don't preemptively register or warehouse domains that people search for. Our entire business model depends on you trusting us enough to search here first and then buying through one of our partner registrars. Undermining that trust would be self-destructive.


Millions of domains are registered and expire every day. If you search for a domain and find it registered shortly after, it's almost certainly a coincidence.


## Why this matters


Domain searches reveal business intent. If you're searching for` acmefinance.com` , you're probably exploring a financial services company called Acme. That's competitive intelligence if it ends up in the wrong hands.


We built this architecture because we believe the domain search process should be transparent. You should be able to explore ideas freely, compare prices across registrars, and only share your intent when you're ready to buy. Going direct to registries is how we make that possible.


Questions? Reach out athello@instantdomainsearch.com .
