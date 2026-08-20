---
schema_version: "1.0.0"
document_id: "1332c893395e0c412752a89528c576130a5f00a69ed399fce7c2a5d81c299047"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/wp2shell-cve-2026-63030-cve-2026-60137/"
published_at: "2026-07-21T11:01:00+00:00"
first_seen_at: "2026-07-21T11:33:44.783558+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:029989bec930c6957ace0bb330804cf00bb59877b472f69dfd893db2ce5cc45e"
---

# wp2shell (CVE-2026-63030 + CVE-2026-60137): WordPress pre-auth RCE, now detected by Escape

wp2shell is an unauthenticated remote code execution chain in WordPress core. It combines two separate vulnerabilities: a route-confusion flaw in the REST API batch endpoint (CVE-2026-63030) that lets a request skip authentication, and a SQL injection in WP_Query (CVE-2026-60137) that it reaches once inside. No plugins, no login, no user interaction. A stock install on an affected version is enough. It hits WordPress 6.9.0 to 6.9.4 and 7.0.0 to 7.0.1, it is already being exploited in the wild, and it is fixed in 6.9.5 and 7.0.2.


Escape detects it across DAST and AI Pentesting, confirms exploitability rather than inferring it from a version number, and shows you all affected assets.


## Now detected in


- [Escape DAST](https://escape.tech/product/dast) : an active check fires the batch route-confusion path and confirms the SQL injection with a time-based blind oracle: a SLEEP payload in the author_exclude parameter on /wp/v2/categories. Live for all customers.
- [Escape AI Pentesting (Cascade)](https://escape.tech/product/ai-pentesting) : walks the full wp2shell chain from the batch endpoint through the SQL injection and returns proof-of-exploit evidence. Live for all customers.


## Wp2shell at glance


wp2shell at a glance Field Detail


CVE IDs` CVE-2026-63030` (batch route confusion) +` CVE-2026-60137` (SQL injection)


Name wp2shell: unauthenticated RCE chain in WordPress core


Type (CWE) CWE-436 (interpretation conflict / route confusion) + CWE-89 (SQL injection)


CVSS 9.8 critical (chain). Vector` CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`


Affected products WordPress core 6.9.0–6.9.4 and 7.0.0–7.0.1 (full RCE chain). 6.8.0–6.8.5 affected by the SQL injection alone.


Fixed in 6.9.5 and 7.0.2 (and 6.8.6 for the 6.8.x SQL injection). Released 17 July 2026.


Attack vector Network, unauthenticated, no user interaction, default install. Entry point` /wp-json/batch/v1` enabled by default.


Exploitation status Exploited in the wild since 18–20 July 2026. Multiple public PoCs.


Disclosure date 17 July 2026 (Searchlight Cyber)


Escape detection DAST + AI Pentesting, currently live


Detection method DAST active check with time-based blind SQLi oracle. AI Pentesting full-chain exploitation with proof.


## What is wp2shell?


wp2shell is not one vulnerability. It is a chain, and the interesting part is how ordinary each link looks on its own.


The entry point is CVE-2026-63030, a route-confusion weakness in the REST API batch handler (` WP_REST_Server::serve_batch_request_v1` ). The handler keeps requests in parallel arrays indexed by position. Send a batch request that desynchronises those arrays and a call gets processed against the wrong context, skipping the authentication check that should have stopped it. The endpoint,` /wp-json/batch/v1` , has shipped enabled by default since WordPress 5.6, so the reachable surface is essentially every modern install.


On its own that flaw is bounded. What makes it dangerous is what it can reach. CVE-2026-60137 is a SQL injection in WP_Query, exposed through the` author_exclude` REST parameter (` author__not_in internally` ). The injection is blind, so there is nothing useful in the response body.


Exploitation leans on a time-based oracle: a **SLEEP payload** whose delay tells you the query ran. From there, an attacker can read arbitrary rows, including administrator password hashes, then abuse object hydration to stand up an administrator account and reach code execution.


One caveat worth knowing: the code-execution path assumes the site is not fronted by a persistent external object cache like Redis or Memcached, which changes how the underlying query is assembled. Plenty of production sites qualify.


Credit for the discovery and disclosure goes to Adam Kues at Searchlight Cyber, who published the advisory and a live checker at wp2shell.com on 17 July 2026. The canonical records are the NVD entries for[CVE-2026-63030](https://nvd.nist.gov/vuln/detail/CVE-2026-63030) and[CVE-2026-60137.](https://nvd.nist.gov/vuln/detail/CVE-2026-60137)


## Exposure risk


The reason this one matters is reach. There are no preconditions. The vulnerable endpoint is on by default, the attack needs no account, and it works against a bare install with nothing added. That describes a very large slice of the internet.


WordPress.org took the unusual step of forcing automatic updates, which helps, but auto-update is not a guarantee. It fails quietly on hardened, managed, and air-gapped installs, and it does nothing for the WordPress properties that fell out of anyone's patching process a long time ago: campaign microsites, old landing pages, docs portals, domains picked up through acquisitions.


## Potential impact on your org


On an affected version, a successful exploit is a full compromise. The attacker gets code execution and, with it, the site: a forged administrator account, the contents of the database including credential hashes, and a foothold that is easy to turn into persistence or a pivot into whatever the site is connected to.


The technical impact is the easy part to reason about. The harder question is inventory. Most orgs cannot say, quickly and with confidence, every place they run WordPress and what version each one is on. This is something you can visualize with the help of[Escape Attack Surface Management.](https://escape.tech/product/attack-surface-management)


Assets affected by vulnerable WordPress version highlighted in Escape's Attack Surface Management.


## Why it's hard to detect


Version-string checks are unreliable here. Auto-update state varies, hardened installs hide the version, and the flaw is a request-handling logic issue rather than a plugin signature you can fingerprint. The SQL injection is blind, so there is no error message or data in the response to key on. Confirming it means safely exercising the batch endpoint and measuring a timing oracle, which most generic scanners will not do reliably.


**Escape DAST confirming the wp2shell SQL injection on a live WordPress asset via the SLEEP timing oracle (asset URL redacted).**


Escape confirms exploitability instead of inferring it from a banner. The DAST check sends the nested batch payload and measures the response delay against the injected SLEEP, so a finding means the path actually worked on your asset. The evidence below shows an 8.293 second response against a SLEEP(7) payload, consistent with time-based blind SQL injection.


## Attack chain


1. An anonymous attacker sends a crafted request to the batch REST endpoint` (/wp-json/batch/v1)` , exploiting the route confusion in` serve_batch_request_v1` (CVE-2026-63030) so the request is handled without authentication.
2. Inside the batch payload, the attacker injects into the author_exclude parameter, which reaches` author__not_in` in` WP_Query` and triggers the SQL injection (CVE-2026-60137).
3. Because the injection is blind, a time-based oracle (a SLEEP payload) confirms execution and lets the attacker read arbitrary data, including admin password hashes.
4. Object hydration is used to forge an administrator account, and from there the attacker reaches unauthenticated remote code execution and full site takeover (on installs without a persistent external object cache).


## Recommended immediate actions


**What you can do right now:**


- Update WordPress to 6.9.5 or 7.0.2 now (6.8.6 for the 6.8.x branch), and confirm the forced auto-update actually applied rather than assuming it did.
- If you cannot patch instantly, block /wp-json/batch/v1 and ?rest_route=/batch/v1 at the WAF (cover both patterns), or restrict unauthenticated access to the REST API.
- Treat any internet-facing site that was on an affected version as potentially compromised. No official IoCs have been published yet, so hunt behaviourally: rogue or unexpected admin accounts, new users, unexpected files in wp-content, and anomalous requests to the batch endpoint. Rotate secrets.


**Short term**


- Inventory every WordPress instance across your surface, including microsites, campaign sites, docs portals, and M&A-inherited domains you may not know are running. Treat your WordPress estate as continuously tested attack surface, not a set-and-forget CMS. Test on every deploy and on every newly discovered asset (for example, with[Escape's Attack Surface Management](https://escape.tech/product/attack-surface-management) )
- Add a wp2shell detection check to every scan so a restored backup or a rebuilt site cannot quietly reintroduce an affected version.
- Route findings to the team that owns each property, with the exploitability evidence and the fix attached, so remediation does not stall in triage.


## Confirm whether you're exposed, with Escape


Finding one WordPress site is a search problem. Finding every instance across marketing, docs, campaign, and acquired domains, on every version, and confirming which ones are actually exploitable, is the real job, and it is the one that decides whether wp2shell is a non-event or a breach.


Escape detects wp2shell across DAST and AI Pentesting. Point it at your surface and you get a straight answer: which of your exposed WordPress assets are affected, proof of how each one is exploitable, and a remediation path routed to the team that owns the asset. Continuous, so a restored backup or a forgotten subdomain does not quietly put you back in scope.[If you're interested to see it live, book a demo with our product expert.](https://escape.tech/book-a-demo)


## FAQ


### Is wp2shell being actively exploited?


Yes. There was no exploitation at the moment of disclosure on 17 July 2026, but in-the-wild activity began within days, and multiple public proof-of-concept exploits are now circulating.


### What WordPress versions are affected by wp2shell?


WordPress 6.9.0 to 6.9.4 and 7.0.0 to 7.0.1 are affected by the full RCE chain. WordPress 6.8.0 to 6.8.5 carry only the SQL injection component. The fixes are 6.9.5, 7.0.2, and 6.8.6, all released 17 July 2026.


### How do I know if I'm vulnerable to wp2shell?


Check your WordPress version, but do not rely on the version string alone, since auto-update state and hardening can make it misleading. The official checker at wp2shell.com tests a site directly. Escape detects wp2shell across DAST and AI Pentesting and confirms exploitability against your live assets, across every WordPress property on your surface.


### Can Escape detect wp2shell?


Yes. Escape detects it across DAST and AI Pentesting, with proof-of-exploit evidence and a remediation path.


### Is there a patch for wp2shell?


Yes. Patches were released on 17 July 2026 (6.9.5, 7.0.2, and 6.8.6), and WordPress.org enabled forced automatic updates for supported installations. If you cannot patch immediately, blocking the batch route (/wp-json/batch/v1 and ?rest_route=/batch/v1) at the WAF is the recommended interim mitigation.


### What's the CVSS score of wp2shell?


The chain is rated 9.8 critical, with vector CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H. Scoring for the individual route-confusion CVE varied across sources during early disclosure (some records list it as 7.5 high on its own), but the combined chain is unambiguously critical.
