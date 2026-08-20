---
schema_version: "1.0.0"
document_id: "7c79fa881d404268fe4a4698cdcc0ab6ce45d6e68fa7e0a91f21dfe31e74192a"
company_key: "netskope-inc-class-a-common-stock"
company: "Netskope Inc."
source_id: "netskope-inc-class-a-common-stock-rss-c0a3e1ef9778"
canonical_url: "https://www.netskope.com/blog/fake-captcha-real-business-traffic-distribution-for-hire"
published_at: "2026-08-04T15:00:00+00:00"
first_seen_at: "2026-08-10T01:01:10.452862+00:00"
fetched_at: "2026-08-10T01:01:12.343007+00:00"
content_hash: "sha256:ca191eb2f77a1a4cda4e3918a91ba86b9ac53c0be4e43d181aa09ad6d96082fd"
---

# Fake CAPTCHA, Real Business: Traffic Distribution for Hire

A single PDF factory has stamped out more than 12,700 structurally similar FakeCaptcha documents and parked them on Webflow’s content delivery network, where Google indexes them as ordinary “upgrade guides.” Each document is a doorway into a traffic-distribution system (TDS) that sorts visitors and routes those that qualify to multiple buyers, malware distributors, and scam operators. The operation has been running for more than 14 months, from the earliest sample we can date to lure domains registered this month, and it is still active.


Most reporting on chains like this stops at the payload. This post is about the layer above it: the distribution network that funnels victims in, how the gate decides who is worth selling, and the different ends it routes them to.


## The user journey: from a Google search to a routed click


It starts where most research starts: a search. A query as mundane as` "upgrade guide" filetype:pdf site:cdn.prod.website-files.com` surfaces these documents, and so do looser searches that happen to match the SEO padding baked into each PDF. Every one of them downloads from Webflow’s CDN, a host with a near-universally clean reputation, so nothing about the URL looks wrong.


Search results surfacing FakeCaptcha PDFs on Webflow’s CDN


The PDF shows an “I’m not a robot” panel, a fake CAPTCHA. Clicking it enters the TDS, where a` ww80` /` wwNN` traffic router (for example` ww80\[.\]tugoduzak\[.\]com` or` ww19\[.\]nurepikis\[.\]com` ) decides who you are.


The fake CAPTCHA panel embedded in each PDF


The final page contains roughly 6,000 words of SEO padding text that matches the utm_term parameter in the embedded URL, consistent with an industrial-scale factory generating variants per search term.


SEO padding filling the final page of the PDF


The gate is a custom Elixir/Phoenix stack, not a commodity PHP TDS kit, which we identified from its response headers and the structure of the JSON Web Tokens it issues. It runs three filters in series. First, an IP and autonomous-system check during the TLS handshake blocks datacenter egress with a fatal alert before any HTTP exchange; residential and corporate IPs proceed. Second, a Cloudflare Turnstile challenge, followed by a Joken JSON Web Token, rejects bots that pass the IP check. Third, the` ww80` /` wwNN` traffic router applies geo and device filtering.


Then the network splits traffic into one of two journeys. Qualifying traffic, fingerprinted as a real user in a target geography, is routed onward to malware or scam infrastructure. Non-qualifying traffic is dumped onto a search-arbitrage ad lander, where the operator still monetizes the click. Either way the network gets paid; the difference is who is paying, and what happens to you next.


The FakeCaptcha PDFs Netskope observed are a slice of a larger operation. Pivoting on the PDF’s TLSH structural fingerprint returns more than 12,700 files. A tighter vhash pivot, which keys on exact document structure rather than fuzzy similarity, isolates roughly 2,480 of those into the two clusters we analyzed directly, the earliest submitted in May 2025.


## What we observed


Across months of Netskope telemetry, these downloads arrived overwhelmingly from search: Google accounted for roughly 98% of the referrer mix, with a long tail of Bing, PDF aggregator sites, and direct navigation. Exposure skewed to the English-speaking world, led by the United States, India, Australia, the United Kingdom, and Canada: across 26 countries in total.


## A second front door: AI assistants


Search engines are not the only thing surfacing these files. In roughly 3% of the environments where we saw these downloads, the referrer was not a search results page but an AI assistant: Google Gemini, Claude, and, in one case, an enterprise assistant deployed internally by an organization.


The share is small, and referrer data alone cannot tell us whether the assistant cited the PDF in an answer or a user pasted a link it had surfaced. The structural exposure is the same either way. These documents are built for exactly the signals a retrieval layer keys on: a plausible title, a clean CDN host, and thousands of words of on-topic text. Anything that ranks documents by relevance rather than by reputation inherits the SEO poisoning that search engines already fell for.


## The several ends of the pipe


The current wave runs on rotated infrastructure. The 2025 lure domains` dutabuz\[.\]com` and` zuwufag\[.\]com` have given way to a 2026 pool:` nurepikis\[.\]com` ,` tugoduzak\[.\]com` ,` maxudijuz\[.\]com` ,` pofezaf\[.\]com` , and others. Several of these were intermediary domains in the 2025 operation, now drop-caught and reused as front-door lure entries, consolidated on the same DNS provider. Fresh paint on the same PDF factory and routing architecture.


We have observed three branches as the range of downstream ends the TDS routes to, which is what drives the traffic-distribution-as-a-service reading.


### **Malware distribution**


One branch routes to the Legion Loader distribution hub. Palo Alto Unit42[documented](https://x.com/Unit42_Intel/status/1906820885865988184) this hub in March 2025:` berapt-medii\[.\]com` serving a fake CAPTCHA page that hijacks the clipboard, telling the victim to press Win+R and paste a command that installs an MSI as “Klio Verfair Tools” and drops Legion Loader. We confirmed the routing independently: In our telemetry, hits to` berapt-medii\[.\]com` carry “dutabuz” and “zuwufag” subdomains as the HTTP Referer.


A 2026 VirusTotal submission named` Documentos\[.\]pdf.exe` (SHA256` 87b8b76762eac941c562c6c8eefb8402f48fc70fcfe360a274b12e75dd5726e2` ) carries the same` Trojan.Win32.Injuke` verdict. That verdict is generic to injector behavior and is not by itself a family link. Still, the sample fits the branch on other grounds: it is a 191 MB WinRAR self-extracting dropper with a Spanish installer UI, matching the Spanish-geo segment the TDS serves, and it contacts` binonelola\[.\]com` . That domain also appears in the 2026 lure pool, one of the few cases in this network where a front-door lure entry doubles as payload infrastructure.


### **TDS reseller gate**


A second branch routes to` yfdpco1\[.\]com` and` yfdpco4\[.\]com` , a TDS gate running` sk-park\[.\]php` . Non-qualifying visitors, including anything arriving from a datacenter IP, receive a search-arbitrage ad lander built on the Skenzo/Team Internet ecosystem; the gate operator registered that cover infrastructure under the same WHOIS identity as the “yfdpco” domains, a deliberate dual-revenue model. It is a shared hub fed by thousands of distinct referer domains, and what qualifying visitors receive is not pinned. Qualifying traffic may be routed to the Legion Loader hub, or to other buyers, as intelligence shows these TDS domains are popular among JS redirectors.


Non-qualifying traffic leading to advertisement landers


### **Premium-SMS subscription**


As for the third branch, during our research we observed a scam lure targeting Spanish-speaking users, operating in the same Spanish-geo segment that the TDS serves; the routing link to this campaign’s TDS is inferred. The scam landing page on` scorenetsystems\[.\]pro` shows a QR “verify with your mobile” fake CAPTCHA, routing through` chromovira\[.\]org` with yet another layer of FakeCaptcha to ensure the user opens the page on a mobile device.


QR code on the scam landing page


The QR leads to a phone-number-harvesting page on` solidlinkpro\[.\]info` , which fires an SMS deep link to shortcode 797079 with the body` ALTA 441` . “ALTA” is the Spanish term for signing up or registering for a service, as in *darse de alta* ; the victim subscribes to a premium-rate SMS service and is charged per message.


SMS deep link on the subscription trap page


The shared-hub pattern ties it together. Distribution hosts like` berapt-medii\[.\]com` and the` yfdpco` gate are fed by thousands of distinct referer domains, not just this campaign’s lures. One TDS, many buyers: The operator routes qualifying traffic to buyers, and the buyer decides what to do with it.


## The detection problem, by design


The delivery host is a shared CDN serving every Webflow customer, which is the point. Blocking it is not an option, and the operator is counting on that. It leaves three things to detect, in descending order of shelf life: the PDF structure, stable across both the 2025 and 2026 clusters; the` wwNN` router subdomain pattern, which survived every rotation we tracked; and the lure domains, which turn over in months. The list below is the perishable part.


## Conclusion


Fake CAPTCHAs work because the cue is familiar and the context (a search result, a clean CDN, a PDF that looks like documentation) lowers the guard. The asymmetry is the story: The operator rebuilds a front door in an afternoon by drop-catching a domain and regenerating PDFs for each search term, while defenders match on lists that go stale within weeks. Instead, defenders should look to build on what did not change in 14 months, and note that when thousands of unrelated referer domains feed one gate, blocking this campaign’s lures does not take the gate down, it only takes you out of its funnel.


## Netskope One


Netskope’s defensive coverage for this campaign spans three points in the chain: the PDF at download, the TDS infrastructure at the network layer, and the payload if a victim reaches it. At the PDF download, threat intelligence signatures block known malicious documents inline, and the Advanced Threat Protection cloud sandbox detects Patient Zero samples and rotated variants by behavior. The malicious site threat-intelligence feed blocks the lure domains and the TDS gate infrastructure, and flags Malware Distribution Points. Netskope Threat Protection covers the Legion Loader payload family should a victim reach the distribution hub.


Newly-registered lure domains are added to the threat-intelligence feed as they’re discovered; set malicious website policies to block rather than alert for the domains and infrastructure listed below so that newly observed lure entries are stopped at the gate rather than merely surfaced for triage.


## Indicators of compromise


Netskope Threat Labs has made available the full set of indicators of compromise (IOCs). Visit:[https://github.com/netskopeoss/NetskopeThreatLabsIOCs](https://github.com/netskopeoss/NetskopeThreatLabsIOCs)


Indicator Type Notes


` T1796401078D050ED3E05D43A6BD172D5C0F0A7B48C5C63AFF61661FCBBA186269D8E4AE` TLSH FakeCaptcha PDF structural fingerprint


` 9a448a3f5689bff73158220126cca1085` vhash 2026 PDF cluster; exact structural match


` 9cacf340f36958ada8f48cd21217732cf` vhash 2025 PDF cluster; exact structural match


` dutabuz\[.\]com` ,` zuwufag\[.\]com` Lure domains 2025 pool


` nurepikis\[.\]com` ,` tugoduzak\[.\]com` ,` maxudijuz\[.\]com` ,` pofezaf\[.\]com` ,` godoxevez\[.\]com` ,` vimemug\[.\]com` ,` jufewine\[.\]com` ,` binonelola\[.\]com` ,` bovetewa\[.\]com` ,` riwitamo\[.\]com` ,` gowixese\[.\]com` Lure domains 2026 pool; 2025 intermediaries reused as lure entries


` berapt-medii\[.\]com` Stage-2 / Malware Distribution Point Legion Loader hub


` yfdpco1\[.\]com` ,` yfdpco4\[.\]com` ,` yfdpco2\[.\]com` Stage-2 / TDS gate sk-park\[.\]php gate; shared hub


` scorenetsystems\[.\]pro` ,` chromovira\[.\]org` ,` solidlinkpro\[.\]info` Scam domains Premium-SMS subscription trap


` sms:797079&body=ALTA` Premium-SMS indicator “ALTA” to shortcode 797079 (Spanish premium-rate)


` 87b8b76762eac941c562c6c8eefb8402f48fc70fcfe360a274b12e75dd5726e2` SHA256 Documentos\[.\]pdf.exe, 191 MB WinRAR SFX Injuke dropper


` 212.92.104\[.\]119` IP TDS entry gate


` 185.53.179\[.\]200` IP ww80 traffic router


` 208.91.196\[.\]46` IP yfdpco gate backend


` 188.72.236\[.\]249` IP berapt-medii\[.\]com Legion Loader hub
