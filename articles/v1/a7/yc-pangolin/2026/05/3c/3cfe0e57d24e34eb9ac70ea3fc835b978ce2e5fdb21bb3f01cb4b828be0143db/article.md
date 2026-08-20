---
schema_version: "1.0.0"
document_id: "3cfe0e57d24e34eb9ac70ea3fc835b978ce2e5fdb21bb3f01cb4b828be0143db"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/building-a-peer-to-edge-peer-reverse-proxy"
published_at: "2026-05-27T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:55:52.188627+00:00"
content_hash: "sha256:af5adf2b979fa791ec2ec37acb65166262c947c02a704221d00e83226d7c2313"
---

# Peer-to-Peer Alternative to Cloudflare Tunnels with Edge TLS Termination

## The Two Tradeoffs of Reverse Proxies


When you deploy a standard reverse proxy, you typically opt into two things: terminating SSL at the server level, and exposing your service to the public internet. Generally speaking, there is nothing wrong with this.


The first major tradeoff emerges with cloud-brokered reverse proxies like Cloudflare Tunnels or Ngrok. Because they intercept your traffic in the cloud, they act as a mandatory man-in-the-middle decrypting your SSL before passing it down a tunnel to your network edge. While this allows them to inspect and block bad traffic, it breaks strict compliance rules like HIPAA, where sensitive data must remain completely unreadable by third parties between the client and the destination.


The second tradeoff is exposure. If you are hosting a sensitive internal site, like a password manager or a web panel for a legacy hardware controller, a standard reverse proxy leaves it discoverable to anyone with a web browser. Modern tools can throw an authentication page in front of it, but your resource is never truly "cloaked" from the public internet. Undetected vulnerabilities remain exploitable.


To solve both issues simultaneously, we have to flip the architecture: combine peer-to-peer networking with an edge reverse proxy. Instead of relying on public DNS and a cloud server to terminate connections, the client connects to a virtual private network (VPN) and resolves DNS entirely over the tunnels. The HTTPS traffic travels over a blind transport layer and decrypts all the way down at the local network edge. The result is a service that is fully functional with valid SSL, yet entirely cloaked and untraceable from the public internet.


All code is available on GitHub:[https://github.com/fosrl/pangolin](https://github.com/fosrl/pangolin)


## Key details


### Cloud-Terminated vs. Edge-Terminated


Briefly, to understand the difference, look at how the lifecycle changes in these two diagrams.


Traditional Cloud-based Tunneled Reverse Proxy:


Peer-to-Peer Tunneled Edge Reverse Proxy:


### How Did We Build It?


A quick background on our architecture: Pangolin natively provides two core capabilities. First, a server-terminated, cloud-based reverse proxy (our direct alternative to Cloudflare Tunnels). Second, a client-to-site peer-to-peer VPN. In both setups, you deploy a lightweight site connector (which we sometimes nickname a Newt) on your local network to facilitate access.


To deliver this new "private HTTPS" modality, we smashed components from both products together.


Here is exactly how the layers come together under the hood to make the system work. (And yes, if you want to skip straight to the implementation, the code is all[available on GitHub](https://github.com/fosrl/pangolin) ).


### Hijack the OS DNS


To get traffic into a private tunnel without forcing the user to type in a raw IP address you have to capture their web traffic at the very beginning of its lifecycle: the DNS request.


If a user types` https://vault.pangolin.net` into their web browser, we cannot let that request escape to a public DNS server like` 1.1.1.1` or` 8.8.8.8` . If it leaks, the request fails, or worse, reveals internal domain topography to the public internet. First, we had to build a mechanism that hijacks the operating system’s DNS routing without breaking the user’s normal internet traffic.


This is easily the most brittle and complex part of the codebase because every operating system handles DNS configuration differently. To make this seamless, we built native clients for macOS, Windows, Linux, iOS, and Android to tap into platform-specific networking APIs. For example, on macOS, we leverage the native System Extensions combined with` scutil` to dynamically set our private DNS server at the top of the network interface resolution order.


### Local DNS Resolution and Logical Mapping


Once the OS-level hook is established, the Pangolin client starts a tiny local DNS server listening directly inside our virtual network interface (usually at` 100.96.128.1` ). Because it lives right on the interface, the traffic never leaves your machine


When the OS pipes a private domain query to this interface resolver, we don’t resolve it to a physical IP on the local network. Instead, the local resolver maps that friendly domain to a unique, logical overlay address (within the` 100.96.128.0/18` range) assigned to that resource. This is all under the hood and you never have to think about the overlay address space.


To the browser and the operating system, this looks like a completely standard networking event. The browser receives the logical IP, assumes it’s talking to a normal web server and initiates a standard TCP handshake. Behind the scenes, the Pangolin client intercepts those TCP packets destined to` 100.96.128.20` and preps them to be shot across our virtual private network.


### Smart P2P Routing & Anycast-style Site Selection


The core peer-to-peer transport and NAT traversal are already standard features of the primary Pangolin VPN. We can treat the connection itself as a black box, but to learn more about how that works, checkout our[explanation on how Pangolin punches through NATs and firewalls](https://pangolin.net/news/nat-holepunching) . Part of the magic for our private HTTPS modality lies in how the client chooses where to send traffic.


For high availability, a private resource can be fronted by multiple site connectors deployed across different physical offices or regions.


Before firing packets to a site, the client first sends small health probes to all sites that you configure that are available as routable for this resource. Based on uptime status and round-trip time, the client chooses to send the packets to the most ideal site.


For example, a user in New York automatically routes through the New York site connector instead of the Los Angeles connector due to the lower latency. If a connector drops offline, the client detects the failure, and fails over to the next best path.


### Push Automated Certificates to the Edge


Once the client selects the optimal site, the encrypted packets travel over the peer-to-peer tunnel to the site and land directly at your local perimeter.


To prevent browsers from throwing certificate warnings, the edge needs a valid SSL cert. However, we cannot run a standard HTTP-01 challenge at the edge because they are completely hidden from the public internet with zero inbound open ports.


How do we automatically generate valid LetsEncrypt certs and get them to the edge?


The central Pangolin control server already manages certs on behalf of your domains using standard LetsEncrypt DNS or HTTP challenges. We need these for the cloud brokered reverse proxy.


Therefore, we can issue certs at the control server level, then use a websocket to push these valid certs down to the on-prem site connectors. The edge infrastructure never faces the public internet or runs its own ACME client. It simply receives its valid certs from the cloud control plane and stores them in its memory.


### Embedded Micro-Proxy at the Perimeter


This is where everything we’ve built comes to a head. The encrypted packets have now arrived over a blind peer-to-peer tunnel, jumped over any firewalls, and landed on the site sitting in your network. Now, we have to turn that raw network stream back into a secure web session. To do that, we embedded our own, highly optimized, tiny, reverse proxy directly into the site binary itself. We built the proxy using basic Go standard library networking tools.


In the traditional reverse proxy setup, the proxy sits on the public internet, accepts connections, terminates SSL, and passes traffic inward. In our model, the tunnel terminates first, then the reverse proxy handles the payload.


Because our local client-side DNS server preserved the original domain request, the browser initiates a standard TLS handshake over the P2P connection. When those packets hit the site, the embedded proxy intercepts them, reads the incoming HTTP host header or SNI, matches it against the propagated certificates, and completes the TLS handshake. SSL terminates at the edge.


Once the traffic is safely decrypted inside your network, the small micro-proxy forwards the plain HTTP request to the downstream target resource you configured in Pangolin.


### Auditing and Logging


The micro-proxy at the edge can still log HTTP requests and enforce rules and authentication – all the same things the cloud broker reverse proxy can do. The difference is, the site collects this data, cleans it, bundles it, and pushes it back up to the cloud for auditing and analytics purposes. This way you can centralize all your edge reverse proxies under one platform.


### One Small Edge Case


Browsers like Chrome maintain internal DNS caches that completely bypass the OS routing table. If a user tries to visit` vault.pangolin.internal` before starting the client, the browser might query public DNS, get an NXDOMAIN, and aggressively cache it. This only ocurrsif the domain already exists on a public DNS server.


When the tunnel boots a second later, the browser completely ignores our local DNS server and keeps trying to hit the public internet. Because browsers don't expose an external API to force-flush this cache, there is no clean programmatic fix. The user simply has to wait out the TTL, open a new tab or session, or manually restart the browser to force it to see the new DNS hooks.


## Conclusion


Ultimately, shifting the reverse proxy from a centralized cloud data center directly to the local network edge proves that zero-trust convenience doesn't require sacrificing data transport privacy. By combining platform-native DNS manipulation, automated wildcard certificate orchestration, and localized perimeter decryption, we can completely cloak critical infrastructure at the edge from the public internet while ensuring end-to-end regulatory compliance for sensitive payloads. Security should protect your data, not act as a mandatory third-party intermediary and by reversing the traditional proxy architecture, you get the absolute best of both worlds.
