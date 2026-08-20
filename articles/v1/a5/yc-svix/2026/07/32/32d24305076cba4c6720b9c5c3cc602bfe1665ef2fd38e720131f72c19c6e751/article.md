---
schema_version: "1.0.0"
document_id: "32d24305076cba4c6720b9c5c3cc602bfe1665ef2fd38e720131f72c19c6e751"
company_key: "yc-svix"
company: "Svix"
source_id: "yc-svix-news-import-06ae021bd4c1"
canonical_url: "https://www.svix.com/blog/svixsec-2026-001/"
published_at: "2026-07-17T12:00:00+00:00"
first_seen_at: "2026-07-23T05:48:40.593786+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:3f67861fd934cf7c743209c884a22043b0ba2940e160e02894a6b9d02597b362"
---

# SVIXSEC-2026-0001: Svix SSRF bypass

Svix is the enterprise ready webhooks sending service. With Svix, you can build a secure, reliable, and scalable webhook platform in minutes. Looking to send webhooks?[Give it a try!](https://www.svix.com/)


At Svix, we take our customers' security very seriously, and that includes providing full and detailed explanations when something goes wrong. This incident is the first notable security incident at Svix; this post will describe what happened, what we've done to fix it, and how it affects our customers.


This issue is still pending CVE assignment, but Svix has reserved` CAN-2026-2033646` as a temporary identifier and we're using` SVIXSEC-2026-0001` as an internal identifier. This document will be updated when a formal CVE number is assigned.


We'd like to thank external security researcher Kim Dong-Uk for reporting this. If you would like to report a security concern to Svix, you can always contact us atresponsible.disclosure@svix.com , as spelled out in our[security.txt file](https://www.svix.com/.well-known/security.txt) .


The Svix server, at its heart, is a router to send data from our direct customers to HTTP1 endpoints provided by our customers' customers. One of the things that Svix gives you is robust protection against[Server-Side Request Forgery](https://owasp.org/www-community/attacks/Server_Side_Request_Forgery) (SSRF). This vulnerability is a way for an attacker to bypass those SSRF protections in some scenarios and when targeting nodes by IP address. Before we can explain this vulnerability in detail, we need to understand what SSRF is and why it's dangerous.


Server-Side Request Forgery allows an attacker to emit network traffic (e.g., an HTTP request) from your network, targeted at an arbitrary destination.


Why is this bad? Imagine that you're a SaaS business deploying your infrastructure in production. A typical tiered architecture might include multiple different network segments: one for databases, one for in-house-developed applications, and one representing the "Internet", with firewalls between them to manage access.


This is common in Cloud environments, where you might have a single VPC with several different subnets in it, and with "internal" services like databases living exclusively on private IPs, unreachable by the world at large.


In the 90's, this would have been the extent of your security precautions; in 2026, after decades of[surveillance](https://blog.encrypt.me/2013/11/05/ssl-added-and-removed-here-nsa-smiley/) and malicious actors, most modern systems support authentication2 and network-level traffic protection with protocols like IPsec or TLS. However, companies may be at different stages in joining the[Zero Trust Architecture revolution](https://en.wikipedia.org/wiki/Zero_trust_architecture) , and it's not uncommon to still have some endpoints that exclusively rely on network segmentation for security. An SSRF bypass allows an attacker who knows where those vulnerable endpoints are to reach them from the outside world.


If, for example, a business were running both the Svix server and an internal unauthenticated WebDAV server in the same network space, an attacker who knew (or could guess) the file-server's IP address (say, 10.1.2.3) could craft an endpoint with a URL like` https://\[::ffff:0a01:0203\]/some/malicious/path` and end up creating a file on that server. This relies on the target's IP address being known or guessable, the target not having any authentication in place, and the target having valid TLS certificate (since Svix, by default, disallows sending webhooks to unencrypted` http://` endpoints).


For more information about SSRF, see our[earlier blog post](https://www.svix.com/blog/ssrf-protection/) .


### Technical Details


In the Svix server, we protect from SSRF vulnerabilities by filtering outbound connections in the application. Our backend uses[hyper](https://hyper.rs/) to make outbound connections and we secure into it in two ways:


- Providing a custom DNS resolver which filters out any blocked IPs and also protects from rebinding attacks
- Detecting when the URL will not use DNS because it contains an IP literal, and applying filtering directly
- Disregarding redirects


Our cloud environment is a bit more sophisticated, and has additional network controls in place around the workers that send outbound requests.


SVIXSEC-2026-0001 is a bug in the second of these; URLs which contained an IPv6 address literal3 enclosed in square brackets were not detected. This means that an attacker who had permission to create new[Endpoints](https://api.svix.com/docs#tag/Endpoint) could create one that would point at an arbitrary IP address; either an IPv6 address like` 2001:db8::cafe:d00d` or an IPv4-mapped IPv6 address like` ::ffff:c0a8:0001`4 . They could then cause your webhooks to be delivered there, which might have unexpected effects depending on what resides at that address


The root cause of this most likely lies in Svix's roots migrating from Python to Rust; in Python, a call like` urllib.parse.urlparse("https://\[1.2.3.4\]").hostname` yields the string` "1.2.3.4"` ; however, in both[url](https://docs.rs/url/latest/url/) and[http::uri](https://docs.rs/http/latest/http/uri/struct.Uri.html) , the equivalent call` "\[1.2.3.4\]".parse::<url::Url>().unwrap().host()` returns` "\[1.2.3.4\]"` .


This[was mitigated](https://github.com/svix/svix-webhooks/pull/2464) by detecting if the returned host value is wrapped in square brackets and trimming them; at that point, our existing IP filtering logic can take over.


Future planned improvements include more-directly overriding the network layer in` hyper` . Unfortunately, while other programming languages offer a consistent interface for overriding net work I/O (go's is called` Dial` ), Rust's ecosystem isn't quite so straightforward, and we're working on the most maintainable way to implement it.


### Legibility


This is the first CVE filed against Svix, and as part of the process we wanted to make our code more legible to security scanners and production security engineering teams. We've done this by making two changes to all of our containers:


- We now set[OCI annotations](https://github.com/opencontainers/image-spec/blob/main/annotations.md) on all of our Docker images, allowing you to see the version through` docker inspect` even if the tag isn't available
- Binaries built on Linux (including those running in Docker) now have[UAPI.8 Package Metadata](https://uapi-group.org/specifications/specs/package_metadata_for_executable_files/) embedded in them, including the (extension)[appCpe](https://nvd.nist.gov/products/cpe) field. This allows security scanners like[syft](https://github.com/anchore/syft) to detect the specific version of the binary on disk; scanners will now report CPEs like` cpe:2.3:a:svix:svix-server:1.98.0:*:*:*:*:*:*:*` . This is being done using a[linker script](https://wiki.osdev.org/Linker_Scripts) emitted from[rust-executable-metadata](https://github.com/svix/rust-executable-metadata) , a small library we've open-sourced.


In addition to the Svix server, these changes have been rolled out for the Svix CLI, Svix Bridge, and[Diom](https://diom.svix.com/) .


We hope that that will make it easier for users to inventory their Svix applications going forward.


### Applying for a CVE in 2026


Svix is not a CNA, so in order to obtain this CVE, we had to work with an existing CNA; we choose to use[MITRE](https://www.mitre.org/) , as the long-standing industry standard. Here's a glimpse at the[MITRE CVE ID request form](https://mitre.github.io/mitre-cve-roles/cve-id-request/#editor) :


If ever there was an example of a UI designed by engineers!


We have filed a request to be assigned a CVE and are still pending a response from MITRE.


---


If you have any questions or concerns, please don't hesitate to reach out tosupport@svix.com .


Stay tuned at[https://www.svix.com/blog](https://www.svix.com/blog) for future posts, follow us on[Github](https://github.com/svix) ,[Mastodon](https://mastodon.social/@svixhq) , or[RSS](https://www.svix.com/blog/rss/) for the latest updates for the[Svix webhook service](https://www.svix.com/) , or join the discussion on[our community Slack](https://www.svix.com/slack/) .


Are you interested in building build reliable, secure, data-driven applications for server-to-server communication?[Come work with us!](https://www.svix.com/careers/)


## Footnotes


1.


Well,[mostly HTTP](https://docs.svix.com/advanced-endpoints)↩


2.


Sometimes just a username and password, but, particularly in cloud environments, authentication often involves hard-to-forge machine identities↩


3.


Note that the[RFC 3986 § 3.2.2](https://www.rfc-editor.org/info/rfc3986/#section-3.2.2) allows any IP literal in square brackets; this is permitted by the Rust[http::uri](https://docs.rs/http/latest/http/uri/struct.Uri.html) library but not the[url](https://docs.rs/url/latest/url/) library, which only allows IPv6 literals↩


4.


This corresponds to 192.168.0.1↩
