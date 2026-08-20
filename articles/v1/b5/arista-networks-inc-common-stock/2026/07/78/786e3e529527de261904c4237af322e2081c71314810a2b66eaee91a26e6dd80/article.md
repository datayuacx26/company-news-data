---
schema_version: "1.0.0"
document_id: "786e3e529527de261904c4237af322e2081c71314810a2b66eaee91a26e6dd80"
company_key: "arista-networks-inc-common-stock"
company: "Arista Networks Inc."
source_id: "arista-networks-inc-common-stock-news-import-c50acbb14cd0"
canonical_url: "https://blogs.arista.com/blog/three-genius-ideas-for-ai-fabrics"
published_at: null
first_seen_at: "2026-07-21T07:42:03.541009+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:335e5754d5822744b8c3067287f6497b59c13d5ce9fa92133ee39c6f4e44e50b"
---

# Three Genius Ideas for AI Fabrics

Arista has had the privilege of building out some of the largest scale-out AI fabrics with the best AI companies in the world. Here, we share a few of the most clever ideas we've encountered along the way.


First is the genius of multi-planar leaf-spine networks. Leaf-spine networks are old news (Arista pioneered them 15+ years ago), but what's new here is the idea that instead of building one fully connected back-end fabric, you build as many as eight independent ones! Leaving the planes disconnected gives you better scale: for example, if you use a standard Clos topology, it takes twenty-four 512-port switches to provide 4096 ports. With an 8-plane network, you can do it with just 8 switches, a massive savings, plus the independence of the planes gives you better reliability.


You might be concerned that the planes don't connect to each other; how can hosts talk if they're on different planes? The trick is that each 800G NIC breaks out into eight independent 100G ports, providing each NIC with a 100G connection to each plane. That way, you can get from one NIC to any other NIC in eight different ways without ever needing to cross planes.


Okay, but then, what if a link fails? How does the NIC decide which plane to use? That's where the second genius idea comes in: Multipath Reliable Connection (MRC). MRC is an open protocol where endstation NICs stripe their traffic across multiple links and paths to the receiver, with out of order packets automatically handled. MRC responds to network congestion signals (ECN and packet trimming), shifting load to the best-performing paths, and avoiding links and paths that can't actually reach the destination altogether.


From there, one more genius idea is needed for the best load balancing and resilience: segment routing over IPv6 (SRv6). While MRC works fine over ordinary IP networks with ECMP, MRC works even better over switches that support SRv6: striping the traffic not just across multiple planes, but also permitting direct source routing of traffic to take advantage of many different paths in each plane. MRC monitors each path, steering around congestion, avoiding paths with link errors, and avoiding failed links. We've proven in production that this approach achieves very high fabric utilization with good load balancing, while interoperating seamlessly with scale-across and WAN networks utilizing standard dynamic routing protocols.


Using these innovations and many others, the most advanced AI companies are achieving great results with huge fabrics built with high-radix Arista Etherlink switches. We are hugely grateful for their partnership and looking forward to building out the next generation with the 7060XE7 leaf switches and 7800 AI spine.


**References**


[7060XE7 Press Release](https://www.arista.com/en/company/news/press-release/24102-pr-20260609)
[Arista MRC White Paper](http://www.arista.com/assets/data/pdf/Whitepapers/MRC-Whitepaper.pdf)
[Webinar: A New Era of Rack-Scale AI Fabrics](https://events.arista.com/rack-scale-ai-fabrics-webinar)
