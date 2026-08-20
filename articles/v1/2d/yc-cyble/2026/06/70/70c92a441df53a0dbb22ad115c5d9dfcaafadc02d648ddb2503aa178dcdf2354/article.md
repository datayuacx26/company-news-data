---
schema_version: "1.0.0"
document_id: "70c92a441df53a0dbb22ad115c5d9dfcaafadc02d648ddb2503aa178dcdf2354"
company_key: "yc-cyble"
company: "Cyble"
source_id: "yc-cyble-rss-726d4f5fb2f2"
canonical_url: "https://cyble.com/blog/borrowed-trust-cloud-dns-takeover-thai-gambling-seo-poisoning/"
published_at: "2026-06-12T05:36:51+00:00"
first_seen_at: "2026-07-20T23:20:21.475332+00:00"
fetched_at: "2026-07-28T21:13:18.116875+00:00"
content_hash: "sha256:c07469fbc31c3771337781dd6b87c38aba8c7cb8cf8f9f0354f451be3706d208"
---

# Borrowed Trust – Systematic Exploitation of Abandoned Cloud DNS Delegations to serve Thai Gambling SEO Content

Link copied!


[SEO Poisoning](https://cyble.com/blog/category/seo-poisoning/)[Azure DNS Misconfiguration](https://cyble.com/blog/category/azure-dns-misconfiguration/)[Cloud DNS Takeover](https://cyble.com/blog/category/cloud-dns-takeover/)[Enterprise Subdomain Hijacking](https://cyble.com/blog/category/enterprise-subdomain-hijacking/)


# Borrowed Trust – Systematic Exploitation of Abandoned Cloud DNS Delegations to serve Thai Gambling SEO Content


Cyble's latest analysis exposes 163 organizations compromised via abandoned DNS delegations in a Thai gambling SEO poisoning campaign.


June 12, 2026 · 17 min read


## Executive Summary


Cyble Research & Intelligence Labs (CRIL) has identified an active SEO poisoning campaign exploiting abandoned cloud DNS zone delegations to serve Thai-language gambling content under the domain authority of reputed enterprise organizations. The campaign has compromised 163 organizations across 30+ countries, spanning federal government agencies, national healthcare systems, financial institutions, critical infrastructure operators, and major universities.


The primary mechanism is the Azure DNS zone takeover. When enterprises decommission cloud infrastructure, NS delegations to Azure DNS zones are routinely left in place. The actor systematically identifies these abandoned delegations, claims the orphaned zones under a fresh Azure subscription, and deploys a Next.js gambling kit behind a valid Let’s Encrypt wildcard TLS certificate, all resolving cleanly under the victim’s own domain. A browser, a search engine, and a Thai user following a search result all see a page identical to a legitimate enterprise property.


This report documents the full campaign architecture: the pivot chain from initial discovery through infrastructure attribution, the DNS compromise mechanisms observed, the structured affiliate monetization layer with server-side geographic filtering and dual-tier commission tracking, and a dedicated 103-node application backend in Hong Kong tied to a single Chinese operator by twelve independent technical evidence points. At the time of publication, 161 organizations remain actively compromised.


**Initial Discovery**


During an ongoing vulnerability assessment, CRIL identified an anomalous DNS resolution on the **cardsforheroes.verizon.com** subdomain environment. What initially appeared to be a single misconfigured endpoint turned out to be **1000+** **individually named subdomains** , each serving **Thai-language online gambling** content under **Verizon’s trusted domain authority** .


Every subdomain followed a gambling brand keyword pattern with URL-encoded Thai characters confirming the intended audience. All endpoints resolved to a single IP at **OVH SAS (AS16276).** Loading a representative endpoint revealed a fully rendered Next.js gambling application with outbound affiliate redirect links.


It matched the structural similarity of 97 other enterprise subdomains across completely unrelated organizations. The affiliate referral parameter on every redirect established the monetization intent immediately: this was not defacement or[malware](https://cyble.com/knowledge-hub/what-is-malware/) delivery, but a structured operation funneling Thai search traffic to gambling registrations for commission.


The Verizon endpoint was not the campaign’s origin. Following it, 162 other compromised organizations were exposed.


Figure 1: Screenshot of the compromised endpoint cod9\[.\]cardsforheroes\[.\]Verizon\[.\]com


## Background: The Vulnerability Class


Cloud infrastructure provisioning introduces a category of DNS misconfiguration that is structurally different from the record-level errors security teams routinely audit. When an enterprise provisions Azure resources for a project environment, a standard step is to delegate a subdomain to an Azure DNS zone by adding NS records in the parent DNS zone that point to Microsoft’s nameservers for that environment. The zone lives inside the Azure subscription, the team manages its records there, and the project operates normally.


What consistently fails is the decommissioning step. When the project ends, and the Azure resources are deleted, the NS delegation in the parent DNS zone is not usually removed. It persists silently, pointing any DNS query for that subdomain to Azure nameservers that no longer serve authoritative records for it.


In several cases identified during this investigation, these delegations had been left in place for over six years.


The exploitability of this configuration depends on another condition: whether the Azure DNS zone for that subdomain is claimable by a new subscriber. Microsoft’s zone-creation model has historically allowed any subscriber to register a zone by name under their own subscription. A zone abandoned with a canceled subscription can be recreated by a third party, who then inherits the delegated DNS authority that the enterprise’s parent zone never revoked.


Our analysis of this campaign demonstrates that awareness has not translated into hygiene at an enterprise scale.


## DNS Compromise Mechanisms


Four distinct mechanisms account for the 163 confirmed victims. Each exploits the same underlying failure: a DNS delegation that outlived the infrastructure it was created to serve.


### Mechanism 1 — Azure DNS Zone Takeover (150+ Organizations)


The actor identifies subdomains whose NS records still point to Azure DNS nameservers, even though the underlying subscription has been canceled or abandoned. A new Azure subscription is created, the orphaned zone is claimed by name, a wildcard A record is added that points all subdomains to a delivery IP, and ACME HTTP-01 validation is performed through the now-controlled DNS to obtain a Let’s Encrypt wildcard certificate. One DNS record exposes every possible subdomain of the environment.


Direct authoritative DNS evidence confirms the mechanism. Querying Microsoft’s own nameserver infrastructure returns the actor’s wildcard A record. Zone SOA serials of 1 on confirmed victims establish that the zones were created from scratch by the actor, not modified from an existing state. Certificate Transparency logs confirm the dormancy periods:


- A major pharmaceutical company’s pilot subdomain: last legitimate certificate October 2019, actor certificate April 11, 2026 — a 6.5-year gap
- A global electronics company’s IoT platform: last legitimate certificate February 2023, actor certificate April 10, 2026


The bulk of actor-obtained certificates cluster in April 2026, indicating a concentrated automated exploitation window applied across targets abandoned over multiple years.


### Mechanism 2 — DigitalOcean DNS Zone Takeover (2 Organizations)


Two organizations, a US luxury furniture retailer and an Indian university, have their compromised subdomains delegated to DigitalOcean nameservers rather than Azure. Both carry wildcard records resolving to 38.127.8.49.


DigitalOcean’s zone-claiming model carries the same structural vulnerability: abandoned DNS zones in canceled accounts become available for re-registration. The actor’s tooling targets multiple cloud DNS providers.


### Mechanism 3 — Direct Wildcard Misconfiguration (2 Organizations)


A US energy company’s development subdomain and a mobile payments platform’s development subdomain both resolve via wildcard to 139.99.82.106, even though there is no cloud DNS zone delegation. Both use their own organizational nameservers, making cloud zone takeover mechanically impossible.


The wildcard records resolve from within the parent zone, indicating either an orphaned wildcard A record left in the organization’s own DNS console when a project was decommissioned, or a direct DNS management access path the actor exploited. “A wildcard A record in the DNS console of a payments platform is a more direct access path than a cloud zone takeover.


### Mechanism 3 — Direct Wildcard Misconfiguration (2 Organizations)


### Mechanism 4 — Per-Subdomain A Records (1 Organization)


The Verizon environment represents a distinct approach: 1000+ individually named A records rather than a single wildcard. Each gambling-keyword subdomain is a separately created DNS entry pointing to 51.79.199.51. This implies either an automated DNS API script with zone-write access or a compromised DNS management credential that enabled bulk record creation.


## **Technical Analysis**


**Pivoting: From One Endpoint to 163 Organizations**


**Pivot 1: 51.79.199.51 — Primary Delivery Node and First Scope Expansion**


The first pivot came from the primary delivery IP itself. Cross-referencing 51.79.199.51 against passive DNS resolution data and page fingerprint matching returned over 90 enterprise subdomains serving identical content, confirmed by:


- **Identical Next.js build ID** : QQOrXCFjoI6C9oF-4YVhl
- **Identical favicon path:** /img/ib99-hq.ico
- Outbound affiliate redirects to the same **three destination domains** across every result


Every result was a subdomain of a legitimately owned enterprise domain with a clean reputation. Standard[threat intelligence feeds](https://cyble.com/knowledge-hub/what-is-a-threat-intelligence-feed/) returned no signal on any of them. The shared page fingerprint expanded the confirmed victim set from 1 organization to over 90 in a single query.


Figure 2: Thai-language gambling page served from a compromised enterprise subdomain, advertising free credits with no deposit required.


**Pivot 2: 139.99.82.106 and 38.127.8.49 — Secondary and Tertiary Delivery Nodes**


Passive DNS resolution data against AS16276 (OVH) surfaced two additional delivery IPs operating in parallel. Both served the identical gambling kit with the same build fingerprint across a partially overlapping but distinct victim set. Government agencies, healthcare organizations, and several non-Azure takeover victims routed through these two nodes rather than the primary. Together, the three OVH nodes account for the full 163-organization victim estate. All three presented clean IP reputations, no prior association with threat actors, and standard deployment signatures at the surface level.


Figure 3: Compromised endpoint pointing to “38\[.\]127\[.\]8\[.\]49”


**Pivot 3: 38.173.56.218 — The JARM Anomaly That Exposed the Backend**


JARM TLS fingerprinting against the primary delivery node returned thousands of global matches, nearly all of them generic shared hosting providers. One result stood apart by every observable metric: 38.173.56.218 in Hong Kong, AS398478 (PEG TECH INC), presenting:


- A ThinkPHP application framework error on port 443
- A Chinese server management panel certificate identity on port 21
- MySQL 5.7.44-log on port 3306


Where every other JARM match represented commodity hosting, this node showed a purpose-built application stack managed from mainland China. This was the single point of entry from the OVH delivery layer into the backend infrastructure. Figures 2 and 3 showcase these using findings from[ODIN by Cyble](https://odin.io/) .


Affiliate Architecture: How the Campaign Monetizes


Every destination redirect carries a ?rc= affiliate code — ibiza99vip1, bigwinv1, link99, or seven77vip1 — that attributes completed registrations to the actor and triggers a commission payout from the destination platform. This is standard affiliate marketing infrastructure; legal and illegal gambling operations use it. A server-side layer beneath the visible code separates this campaign from simple redirect farms. A POST request intercepted at a live endpoint returned this before any redirect was issued:


- “ **status** “: “ok”,
- “ **msg** “: “TH – Referrer verified”,
- “ **data** “: { “referrer”: “link99” },
- “ **x-token”:** “”


The backend validates geographic origin server-side. Requests from outside Thailand are not redirected, keeping traffic focused and limiting scanner exposure. The link99 identifier is a sub-affiliate publisher ID that sits above the ?rc= codes in the platform’s tracking hierarchy.


The actor earns at both tiers: publisher-level credit under link99 for delivering Thai traffic, and a per-registration payout under the ?rc= code for each conversion. The two layers are independent — campaign codes can rotate while link99 persists as the actor’s permanent platform identity.


Figure 4: 5,547 results matching the JARM hash on ODIN (source: ODIN by Cyble)


Pivoting on the Let’s Encrypt certificate presented by this node, issued to broker-xm.com, returned 103 IPs across seven contiguous /24 subnets in 38.173.0.0/16, all within AS398478. A single certificate deployed across 103 servers in a wholesale-allocated IP block produced the complete backend fleet inventory from one certificate query.


Figure 5: Results for Cert CN->broker-xm.com showcasing 200 hits from Hong Kong with the IP range 38.173.0.0/16 (source: ODIN by Cyble)


#### **Affiliate Architecture: How the Campaign Monetizes**


- “ **status** “: “ok”,
- “ **msg** “: “TH – Referrer verified”,
- “ **data** “: { “referrer”: “link99” },
- “ **x-token”:** “”


Figure 6 : Phishing page Redirected to hxxps://link99.nova555.rest/register/ with link99 as a referral code


Four destination platforms have been confirmed — ibiza99.autos, big888.store, seven77.click, and stillsunday.pl. These domains are white-label deployments on a shared codebase, as confirmed by identical CSS hashes and JavaScript resilience logic across all three, except big888.store, which carries active Google Search Console verification, indicating the platform operator runs its own independent SEO operation beyond this campaign. Each platform maintains its own **appbox.* CDN subdomains** , a fourth infrastructure layer entirely separate from the delivery nodes, backend fleet, and victim domains.


Figure 7: Asking for banking information post login via Thailand phone number


### The Gambling Kit: What the Endpoint Delivers


An HTTP request to any compromised enterprise subdomain with Thai locale headers returns a fully rendered Next.js page that returns a legitimate enterprise web property. The page presents


- lang=”th” with Thai-language gambling platform branding
- A valid Let’s Encrypt wildcard TLS certificate matching the victim subdomain, clean browser padlock, no warnings
- Schema.org FAQ structured data with Thai-language answers about minimum deposits, withdrawal timelines, and mobile compatibility, directly targeting the queries Thai users make when researching gambling platforms
- AMP alternate links for Google mobile indexing coverage
- Static assets served from paths under the compromised domain itself, using the victim’s domain authority as a CDN


The minimum deposit advertised across all kit variants is 1 Thai Baht, approximately $0.03 USD. The 1-baht minimum ($0.03 USD) removes the deposit threshold that causes most users to abandon the registration flow before converting. The kit fingerprint is consistent across the entire victim estate, regardless of which victim domain or OVH node serves the content, confirming centralized build and deployment by a single operator.


Figure 8 – Three-tier referral commission structure (10%/3%/1%) plus 0.3% turnover rebate, embedded in the gambling kit served across all 163 compromised subdomains


Figure 9: Multi-level recruitment diagram promoting unlimited monthly commissions across three affiliate downline tiers.


### Backend Infrastructure: The Hong Kong Fleet


The three OVH delivery nodes handle content distribution, but the application layer sits entirely within a separate dedicated infrastructure in Hong Kong. The 103-node backend fleet is distributed across seven /24 subnets within 38.173.0.0/16, all under AS398478 (PEG TECH INC). Seven independent evidence points converge on single-operator control; the MD5 match across all 103 nodes on port 80 alone eliminates coincidence.


**Evidence Point** **** **Detail** **** **Weight** ****


HTTP body MD5 match 7df3d7cf3358af3f470ac7229387ef94 (615 bytes) identical across all 103 nodes on port 80 Critical


Single shared certificate broker-xm.com Let’s Encrypt cert deployed across all 103 servers High


Envoy proxy fingerprint Identical 503 error string on port 443 across all 103 nodes High


MySQL version 5.7.44-log with binary replication logging enabled uniformly High


BT-Panel provisioning[\[email protected\]](https://cyble.com/cdn-cgi/l/email-protection) , Dongguan, Guangdong, on FTP certificates across all nodes High


JARM fingerprint 07d14d16d21d21d07c42d43d000000270a013a3e21e28897e76e8fe13e2f7d uniform across fleet High


Zero PTR records No reverse DNS on any of the 103 IPs, deliberate suppression Medium


### Detection and Hunting


The campaign produces no signal in standard security controls. Valid TLS certificates, clean IP reputation, trusted domain names, and no exploit delivery mean conventional tooling produces no signal. Detection requires operating at the DNS layer.


**Certificate Transparency monitoring** is the most reliable early indicator. An unexpected Let’s Encrypt wildcard certificate on a corporate-owned subdomain, particularly following a period of no issuance, is an anomaly no legitimate provisioning scenario produces. Monitoring CT logs for wildcard certificates whose expected issuer is a corporate or premium CA would have detected every Azure takeover victim at the time of certificate issuance.


**Azure DNS zone delegation** **auditing** is a structural prevention control. Organizations should enumerate all NS delegation records in their parent DNS zones and verify that corresponding Azure DNS zones exist in subscriptions they own and actively manage. 163 organizations across 30 countries had gambling content served under their domain authority without any alert firing. One class of DNS misconfiguration enabled it.


**Wildcard DNS testing** confirms active exploitation. A randomized nonsense hostname query against any Azure-delegated subdomain that returns a resolution should be treated as a compromised zone until proven otherwise.


**Network and endpoint detection rules:**


- DNS answers resolving to 51.79.199.51, 139.99.82.106, or 38.127.8.49
- URL query parameters containing rc=ibiza99vip1, rc=bigwinv1, or rc=seven77vip1 or rc=link99
- HTTP requests to /img/ib99-hq.ico
- Outbound connections to 38.173.30.0/24, 38.173.37.0/24, 38.173.56.0/24, 38.173.57.0/24, 38.173.235.0/24, 38.173.236.0/24, or 38.173.239.0/24


**Internet scanning queries:**


- HTTP body MD5: 7df3d7cf3358af3f470ac7229387ef94
- TLS certificate CN: broker-xm.com
- ASN sweep: AS398478


## Remediation


**For Azure DNS zone takeover victims:**


1. Remove the NS delegation from your parent DNS zone immediately. This severs the attack chain regardless of what the actor maintains inside the Azure zone, which is under their control and cannot be directly modified by the victim organization.
2. Report the abandoned zone to Microsoft MSRC at[\[email protected\]](https://cyble.com/cdn-cgi/l/email-protection) with the zone name and evidence. Microsoft can force-remove zones from subscriptions holding abandoned delegations.
3. Request revocation of any Let’s Encrypt certificates issued against the compromised subdomain using the certificate serial numbers from crt.sh.
4. Audit all remaining subdomains for additional environment-style entries (dev, uat, staging, preprod, pilot, lab, test, sandbox) carrying NS delegations to cloud providers, and verify each delegation is intentional, current, and managed.


**For DigitalOcean zone takeover victims** : Remove the NS delegation from the parent zone and verify whether the DigitalOcean zone exists in an account you own.


**For direct wildcard misconfiguration victims:** Remove the wildcard A record from your DNS management console. If the origin of the record cannot be identified, treat the DNS management credential as compromised and rotate it immediately.


## **Conclusion**


This campaign demonstrates that enterprise reputational trust can be systematically harvested through a single class of DNS misconfiguration that most organizations have no visibility into. The actor did not breach any perimeter or exploit any application vulnerability. They claimed what had been left unclaimed and built a commercial affiliate operation on top of it.


The scale, 163 organizations across 30+ countries, is not the result of 163 separate attacks. It is an automated scanning process applied to a single vulnerability class. A single wildcard record beneath an abandoned Azure delegation exposes an unlimited subdomain namespace, each entry inheriting the full TLS-verified authority of the victim’s brand. The campaign lives entirely within legitimate infrastructure, OVH delivery nodes, Let’s Encrypt certificates, victim-owned domains, and organic search rankings, which is precisely why conventional controls produce no signal.


Audit your DNS estate. Every abandoned NS delegation pointing to a cloud provider is a candidate for the next version of this list. Every abandoned NS delegation pointing to a cloud provider is a potential entry in the next version of this list. The remediation is simple. The detection methodology is documented here. The gap between a decommissioned project and an actively exploited subdomain closed silently on 163 organizations. In several cases, it stayed closed for over six years.


## **MITRE ATT&CK® Techniques**


**Technique** **** **ID** **** **Description** ****


Resource Development — Acquire Infrastructure T1583.003 OVH VPS for delivery; PEG TECH INC ASN for backend fleet


Resource Development — Compromise Infrastructure T1584 Azure DNS zone takeover of legitimate enterprise subdomains


Initial Access — Drive-by Compromise T1189 Thai users directed to compromised enterprise subdomains via organic search


Persistence — Valid Accounts T1078 DNS management credentials implied by per-subdomain record creation (Verizon)


Defense Evasion — Impersonation T1656 Gambling kit served under legitimate enterprise TLS certificates


Defense Evasion — Valid Accounts: Cloud Accounts T1078.004 Azure account used to claim abandoned DNS zones


Collection — Adversary-in-the-Middle T1557 Server-side affiliate referrer verification intercepts the user session


Exfiltration — Web Service T1567 User registration data and financial transactions were exfiltrated to gambling platforms


## **Indicators of Compromise (IOCs)**


**Delivery Infrastructure**


**Indicator** **Type** **Description**


51.79.199.51 IP Primary OVH delivery node, AS16276


139.99.82.106 IP Secondary OVH delivery node, AS16276


38.127.8.49 IP Tertiary OVH delivery node, AS16276


38.173.30.0/24 CIDR Backend fleet, AS398478 PEG TECH INC


38.173.37.0/24 CIDR Backend fleet, AS398478 PEG TECH INC


38.173.56.0/24 CIDR Backend fleet, AS398478 PEG TECH INC


38.173.57.0/24 CIDR Backend fleet, AS398478 PEG TECH INC


38.173.235.0/24 CIDR Backend fleet, AS398478 PEG TECH INC


38.173.236.0/24 CIDR Backend fleet, AS398478 PEG TECH INC


38.173.239.0/24 CIDR Backend fleet, AS398478 PEG TECH INC


**Certificates and Fingerprints**


**Indicator** **Type** **Description**


d9799ca2f08af6992dc80c49f9889fef40ed27c7 SHA1 bt.default.com custom CA on primary delivery node


broker-xm.com Domain Let’s Encrypt cert across all 103 backend nodes


07d14d16d21d21d07c42d43d000000270a013a3e21e28897e76e8fe13e2f7d JARM TLS fingerprint across delivery and backend infrastructure


7df3d7cf3358af3f470ac7229387ef94 MD5 HTTP body hash, 615 bytes, port 80, all 103 backend nodes


**Campaign Kit Fingerprints**


**Indicator** **Type** **Description**


QQOrXCFjoI6C9oF-4YVhl String Next.js build ID across all delivery endpoints


/img/ib99-hq.ico URI Kit-specific favicon path


main.af42a497.css Hash Shared CSS fingerprint across all three destination platforms


pub-a4952b46ff9c4f6b8d5529cd21f9a1e3.r2.dev Domain Cloudflare R2 CDN bucket


**Affiliate Domains and Tracking**


**Indicator** **Type** **Description**


ibiza99.autos Domain Destination platform, affiliate code ibiza99vip1


big888.store Domain Destination platform, affiliate code bigwinv1


seven77.click Domain Destination platform, affiliate code seven77vip1


link99.nova555.rest Domain Destination platform, affiliate code link99


appbox.7y6texmeyy.com Domain ibiza99.autos image CDN


appbox.devh5api27.xyz Domain big888.store image CDN


appbox.55u4g5g4k2.com Domain seven77.click image CDN


322242757545449 Pixel ID Facebook Pixel, ibiza99.autos


1607473696511298 Pixel ID Facebook Pixel, ibiza99.autos


721331896825411 Pixel ID Facebook Pixel, big888.store


GTM-NP59MP3T GTM ID Google Tag Manager, big888.store


**Parallel Operations (AS398478)**


**Indicator** **Type** **Description**


99997778.com Domain Active Chinese gambling platform, AS398478


bevictor.com Domain Chinese offshore sports betting (伟德国际), AS398478


CYBLE.


AI Threat Intelligence


### Stop Executive Threats
Before They Strike


Monitor dark web chatter, detect lookalike domains, and protect your C-suite from targeted impersonation — in real time, across 50+ countries.


[Request a Demo](https://cyble.com/request-demo/)


[Azure DNS Misconfiguration](https://cyble.com/blog/tag/azure-dns-misconfiguration/)[Cloud DNS Takeover](https://cyble.com/blog/tag/cloud-dns-takeover/)[Enterprise Subdomain Hijacking](https://cyble.com/blog/tag/enterprise-subdomain-hijacking/)[SEO poisoning](https://cyble.com/blog/tag/seo-poisoning/)


[Previous Post FIFA World Cup 2026 Scams Are Already Active: Fake Domains, Phishing Sites, and How to Stay Safe](https://cyble.com/blog/fifa-world-cup-2026-scams/)[Next Post Operation FanTrap: Inside the FIFA 2026 Fraud Ecosystem](https://cyble.com/blog/operation-fantrap-fifa-2026-fraud-ecosystem/)


### More from Cyble


[View all posts](https://cyble.com/blog/)


[Threat Intelligence APTs Top the List of Most Active Threat Actors in H1 2026 Jul 27, 2026 · 4 min read](https://cyble.com/blog/most-active-threat-actors-h1-2026/)[Dark Web Monitoring Inside the Underground Economy: 5 Dark Web Trends Shaping the 2026 Threat Landscape Jul 8, 2026 · 7 min read](https://cyble.com/blog/dark-web-trends-2026-cyber-threat-landscape/)[Cyber news Mid-Year Threat Trends: What H1 2026 Signals for the Rest of the Year Jul 6, 2026 · 6 min read](https://cyble.com/blog/2026-threat-intelligence-trends/)
