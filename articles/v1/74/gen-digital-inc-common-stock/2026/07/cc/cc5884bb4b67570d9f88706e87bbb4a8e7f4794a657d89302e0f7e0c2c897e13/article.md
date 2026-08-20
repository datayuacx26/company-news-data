---
schema_version: "1.0.0"
document_id: "cc5884bb4b67570d9f88706e87bbb4a8e7f4794a657d89302e0f7e0c2c897e13"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/the-phishing-link-that-died-on-purpose"
published_at: "2026-08-11T15:37:47+00:00"
first_seen_at: "2026-08-11T19:53:40.769567+00:00"
fetched_at: "2026-08-11T19:53:41.710176+00:00"
content_hash: "sha256:af69d30b2421eab40a44cc30ff0531714b496aa958cb1e57c2642bc364808ff6"
---

# The phishing link that died on purpose

By[Jan Kořínek](https://www.gendigital.com/blog/authors/jan-korinek)


The first URL was already dead.


It did not load a credential form or show a fake Microsoft login page. Instead, it returned an expired tracking response, which is exactly what the operator wanted scanners and analysts to see after the first click.


That could easily have been the end of the investigation: one reported phishing email, a burned link and no visible payload. But the expired URL still exposed enough of the underlying infrastructure to start pulling the thread. What followed led us to Mailer-Go Mission Control, a bulk phishing platform we had not seen publicly documented at the time of analysis.


Mailer-Go handled the delivery side of the campaign, including email templates, recipient management, SMTP infrastructure, proxy support and per-recipient tracking links. The final phishing stage came later, through a Cloudflare Workers page running EvilTokens, a known Microsoft OAuth Device Code phishing kit.


EvilTokens itself was not the new part. What caught our attention was the machinery around it: how victims were routed there, how the operator switched infrastructure when a domain was burned, and how much of the delivery chain was designed to disappear once a useful click had already happened.


## The link was expired, but the pattern was not


The reported email contained a tracking URL under:


coldchain-distribution\[.\]com


The URL followed a structure we later associated with Mailer-Go:


hxxps://<8-hex>.link.<domain>/t/v3.<campaign-id>.<recipient-token>


Each recipient received a unique token. Once that token was used, later requests no longer reached the live phishing chain and instead returned an expired response. A victim could therefore see the real destination, while a scanner or analyst arriving afterwards saw something very different.


That behavior became one of the more useful clues in the investigation. The link was not simply a route to a phishing page. It also acted as a gate that closed after use, limiting what could be recovered later from the same URL.


The first domain, coldchain-distribution\[.\]com, was registered on 2026-06-01. Once detections started catching up, the operator moved activity to healthprofessionals21\[.\]org, which had been registered the same day. Its tracking subdomains resolved to the same Linode IP used by the earlier campaign, and more than 40 *.link.healthprofessionals21\[.\]org subdomains were eventually observed.


## A fake blog on the front door


At the root, the replacement domain looked harmless.


healthprofessionals21\[.\]org served a technology and gaming blog with articles about consoles, VPNs and multimedia devices. The content was thin and generic, but plausible enough to give scanners and casual visitors something ordinary to index. The phishing infrastructure itself sat underneath *.link.healthprofessionals21\[.\]org.


Figure 1: The root of healthprofessionals21\[.\]org served a fake technology and gaming blog. The tracking infrastructure was hosted under *.link subdomains.


For a short time, the setup appears to have done its job. The replacement domain initially had little to no reputation-based detection while the phishing chain remained active.


By 2026-06-10, that had started to change. Fortinet marked the domain as phishing and ESET flagged it as suspicious, but the operator had already been using it as failover infrastructure after coldchain-distribution\[.\]com was burned.


## What Mailer-Go is


Mailer-Go Mission Control presents itself like a commercial email campaign platform. Its admin panel includes modules for campaign management, audience lists, email creation, SMTP providers, proxy routing, tracking, licensing and update notifications.


None of those features, viewed individually, proves malicious intent. The combination is harder to explain as a normal bulk-mail product.


Mailer-Go supports proxy routing and multiple SMTP providers, and it generates single-use tracking tokens that stop exposing the live destination after the first click. It also includes built-in license management, yet at the time of our analysis we could find no public documentation, GitHub footprint or visible legitimate product presence. The same Mailer-Go login page was exposed across wildcard campaign subdomains.


We assess with moderate confidence that Mailer-Go is purpose-built crimeware rather than a legitimate phishing simulation tool being abused. That assessment is based on the platform as a whole: proxy support, flexible sending infrastructure, token burning, controlled access and the absence of a public legitimate footprint.


Useful fingerprints include:


Mailer-Go Mission Control
/assets/js/tracking.js?v=20260522d
/assets/js/proxies.js?v=20260513b
/assets/js/sending-infrastructure.js?v=20260513b
/assets/dist/emailbuilder2/manifest.json


The observed admin panel HTML had this SHA-256:


3a026fcb5001327198ac3886e6983183e44b8a5daa0086d34c10cb65130c6f89


## Live tokens showed the real destination


The expired links were only part of the picture. URLScan captures of tokens that were still live showed a different path.


Valid Mailer-Go tracking links routed through:


mysocalledblog\[.\]com/sev/


That relay then redirected the victim to a Cloudflare Workers page. Following the EvilTokens convention, the Workers subdomain encoded the targeted recipient's email address, which we have redacted:


9sfo-bnqi-4yap.\[recipient-redacted\]-s-account.workers\[.\]dev


The page presented itself as a OneDrive shared-document flow built around a file named Confidential_Memo.docx. Captures showed several stages of the page, including a “Preparing verification...” screen, a transient “fetch failed” error and, most importantly, the page displaying a Microsoft device code.


Figure 2: The final page displays a Microsoft device code and instructs the victim to continue to Microsoft.


There was no fake Microsoft password form. The victim was sent to the real Microsoft device login page. If they entered the code and completed authentication, the attacker-controlled client could receive OAuth tokens in the background.


That has an important consequence for incident response. Resetting the user's password may not be enough if a refresh token has already been issued. We cover the response steps later in the defenders section.


Why Visa appeared in the redirect chain


Expired tokens followed another route.


Instead of reaching the EvilTokens page, they passed through:


arduinosolutions\[.\]com/sev/


and then landed on legitimate Visa sites such as visa\[.\]fr, visa\[.\]com, visa\[.\]de and others.


That initially creates a confusing picture because Visa appears repeatedly in the redirect data, yet the confirmed phishing lure impersonated Microsoft OneDrive.


The explanation was simpler. Visa acted as fallback cover for dead tokens. Once a tracking token had expired, the user was sent to a legitimate site rather than the phishing page. Someone clicking the link later could reasonably conclude that nothing malicious had happened, and an analyst reconstructing the chain after the fact could reach the same wrong conclusion.


## The emails were built to survive static scanning


We identified five phishing EML samples tied to the campaign. Their MIME-header similarity supports the conclusion that they came from the same sending platform.


The messages used generic B2B document themes, including an Autodesk-themed “Bidding Addendum” lure. We found no evidence of detailed industry reconnaissance, so the activity looks more like broad B2B targeting than a tightly tailored campaign.


The email bodies also used a simple obfuscation technique in which characters were interleaved with question marks:


T?h?i?s?


The text remains readable to a person, but the added characters interfere with keyword matching and some content-based detection systems. At the time of analysis, the observed samples had zero detections across the engines available in our internal dataset.


We also saw Microsoft SafeLinks wrapping in some of the messages. In those cases, the original Mailer-Go URL was stored in the originalsrc attribute rather than the visible href, so analysts reviewing mail traces should inspect both.


## What defenders should take from this


The most useful Mailer-Go indicators are likely to come from the delivery layer rather than the lure itself. Phishing content can be changed quickly, and a Workers URL can disappear. The tracking structure is more consistent.


In this campaign, Mailer-Go links used *.link.<domain> subdomains and a /t/v3. path containing a numeric campaign ID followed by a long hexadecimal token:


hxxps://<8-hex>.link.<domain>/t/v3.<campaign-id>.<recipient-token>


The admin panel also exposed reusable fingerprints, including the title Mailer-Go Mission Control and versioned JavaScript paths such as:


/assets/js/tracking.js?v=20260522d


/assets/js/proxies.js?v=20260513b


/assets/dist/emailbuilder2/manifest.json


For Microsoft 365 environments, a confirmed device-code phishing event should not be handled as ordinary password phishing. If the attacker received a refresh token, resetting the password may leave access intact. Response should include revoking active sessions and refresh tokens, reviewing device-code sign-ins, checking app consent, reviewing registered authentication methods and devices, and looking for new mailbox rules, MFA changes or suspicious file access after the click.


## Attribution and limits


We have not linked this campaign to a named threat actor.


The operator appears to be a customer of EvilTokens, which is commercially sold. Mailer-Go is less clear. Its licensing features and commercial-style architecture suggest controlled distribution, but we do not know how the platform is sold or shared, and we cannot rule out the possibility that the operator behind this campaign is also involved in its development.


There are other limits to what the available evidence can tell us. We observed a low-confidence PE download signal in sandbox telemetry, but no executable was recovered and the same signal appeared when sandboxing the Mailer-Go admin panel itself. Without endpoint evidence showing a PE download or execution after a victim click, we do not treat that as confirmed victim-side malware delivery.


## Conclusion


The reported link was already burned, but it still exposed the campaign’s delivery architecture. Behind the expired URL sat a Mailer-Go tracking server, a backup domain with a fake root site, relay nodes of unconfirmed ownership and an EvilTokens page abusing Microsoft’s device-code flow.


Mailer-Go is the part defenders should track beyond this campaign. The final lure can change. The Workers URL can disappear. The delivery layer leaves stronger patterns: *.link.<domain> subdomains, /t/v3 tracking paths, single-use tokens, exposed admin panels and a small set of reusable JS fingerprints.


## Sources and credits


- Website snapshots (Figures 1, 2): urlscan.io
- EvilTokens background: public research by Sekoia and Push Security


More on this topic


[Fake invoices are moving from inboxes to shopping apps – From Research](https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps)


[Inside Vidar’s ABE Bypass: From Memory Scanning to APC Injections – From Research](https://www.gendigital.com/blog/insights/research/inside-vidar-abe-bypass)


[Your flight was cancelled. Is the refund message real? – From Research](https://www.gendigital.com/blog/insights/research/flight-cancelled-refund-message-scam)


[Jan Kořínek](https://www.gendigital.com/blog/authors/jan-korinek)


Princ Security Analyst CTI


Follow us for more
