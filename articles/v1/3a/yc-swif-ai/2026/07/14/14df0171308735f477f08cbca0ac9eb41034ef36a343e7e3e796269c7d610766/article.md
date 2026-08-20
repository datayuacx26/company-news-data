---
schema_version: "1.0.0"
document_id: "14df0171308735f477f08cbca0ac9eb41034ef36a343e7e3e796269c7d610766"
company_key: "yc-swif-ai"
company: "Swif.ai"
source_id: "yc-swif-ai-news-import-789a3488158e"
canonical_url: "https://www.swif.ai/blog/wordpress-security-statistics"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-22T15:23:23.396754+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:93cab03386a052e2b01a9c5a9f484c2335a4186fb419ad65a92eef14b172b7a2"
---

# WordPress Security Statistics for 2026 ft: wp2shell

On July 17, 2026, WordPress shipped emergency 6.9.5 and 7.0.2 core releases, with forced automatic updates enabled, to close a pre-authentication remote code execution flaw that security researchers at[Assetnote, part of Searchlight Cyber](https://slcyber.io/research-center/wp2shell-pre-authentication-rce-in-wordpress-core) , disclosed under the name wp2shell. The flaw sits in WordPress core itself, so an anonymous attacker could run code on a fully default installation with no plugins at all. Every site on versions 6.9.0 through 6.9.4 and 7.0.0 through 7.0.1 was affected, and the[WordPress.org advisory](https://wordpress.org/news/2026/07/wordpress-7-0-2-release/) describes the root cause as a REST API batch-route confusion and SQL injection issue leading to remote code execution. The researchers are withholding technical details for now, no exploitation had been reported in the first 24 hours, and site owners who cannot update immediately are advised to block anonymous access to the /batch/v1 REST route.


wp2shell is a dramatic exception to the broader pattern, not the pattern itself. WordPress powers 41.5% of all websites as of July 2026, according to[W3Techs](https://w3techs.com/technologies/overview/content_management) , which makes it the single largest attack surface on the web, and the overwhelming majority of that risk lives outside core.[Patchstack's State of WordPress Security in 2026](https://patchstack.com/whitepaper/state-of-wordpress-security-in-2026/) counted 11,334 new vulnerabilities in the WordPress ecosystem in 2025, a 42% increase over the year before, with 91% of those vulnerabilities found in plugins. Attackers are moving faster too. Among the most heavily exploited flaws, the weighted median time from public disclosure to mass exploitation was just five hours. This page collects the most current, primary-sourced WordPress security statistics and vulnerability data for 2026, so IT teams, security leaders, and journalists covering the space can cite the numbers with confidence.


**Key WordPress security statistics at a glance**


- **11,334 new vulnerabilities** were discovered in the WordPress ecosystem in 2025, up 42% from 7,966 in 2024, according to Patchstack's 2026 whitepaper.
- **91% of new WordPress vulnerabilities were found in plugins** and 9% in themes, per the same Patchstack report. Only six were reported in WordPress core in 2025, all low risk.
- **Highly exploitable vulnerabilities rose 113% year over year** , and Patchstack found more high-severity flaws in 2025 than in the previous two years combined.
- **46% of disclosed vulnerabilities had no patch available** from the developer at the time of public disclosure, Patchstack reports.
- **The median time to mass exploitation is 5 hours** for the most heavily targeted vulnerabilities in Patchstack's data, and roughly half of high-impact flaws are exploited within 24 hours.
- **Typical hosting defenses blocked only 26% of vulnerability exploit attempts** in Patchstack's 2025 pentests of popular hosts, and just 12% of WordPress-specific attacks.
- **WordPress runs 41.5% of all websites** and roughly 59% of sites with a known CMS as of mid 2026 (W3Techs).
- **Exploited vulnerabilities became the top initial access vector for data breaches** for the first time in 19 years, at 31% of breaches, per the 2026 Verizon Data Breach Investigations Report.
- **The average data breach costs $4.44 million globally** and a record $10.22 million in the United States, per IBM's 2025 Cost of a Data Breach Report.
- **Automated traffic passed 51% of all web traffic** in 2024, with malicious bots alone accounting for 37%, per Imperva's 2025 Bad Bot Report.


Show Image


**The WordPress attack surface in 2026**


The scale of WordPress is the starting point for every security conversation about it.[W3Techs](https://w3techs.com/technologies/overview/content_management) reports that WordPress is used by 41.5% of all websites as of mid 2026, and by roughly 59% of all sites that run a detectable content management system. The next largest platforms, Shopify and Wix, each hold only around 4 to 5% of the web. For attackers, that concentration is an economic gift: one working WordPress exploit can be sprayed across hundreds of millions of installations.


The good news is that the core software itself is broadly current. W3Techs data shows that about 92% of WordPress sites now run version 6 or newer, a dramatic improvement over the mid 2010s, when surveys regularly found most installations running outdated core versions. The platform is also mid-transition: WordPress 7.0 "Armstrong", the first major release of 2026, shipped on May 20, 2026, and the emergency 6.9.5 and 7.0.2 releases followed on July 17, 2026 to close the wp2shell flaw described above, pushed out through forced automatic updates. That auto-update machinery, expanded steadily over the past decade, is the reason a core-level RCE can be patched across the fleet in hours, and it has largely fixed the "unpatched core" problem that defined early WordPress security coverage.


What has not shrunk is the extension ecosystem.[WordPress.org](https://wordpress.org/plugins/) lists nearly 60,000 free plugins in its official directory, before counting the tens of thousands of premium plugins and themes sold on third-party marketplaces. Every one of those components is third-party code running with deep access to the site, and that is exactly where the risk has moved.


**WordPress security vulnerabilities statistics: where the flaws come from**


The clearest finding in[Patchstack's State of WordPress Security in 2026](https://patchstack.com/whitepaper/state-of-wordpress-security-in-2026/) is the source breakdown: of the 11,334 new vulnerabilities logged in 2025, 91% were in plugins and 9% were in themes. WordPress core accounted for just six vulnerabilities all year, all rated low priority. A decade ago, industry studies attributed roughly half of WordPress vulnerabilities to plugins; that share has now climbed to more than nine in ten. Core flaws remain rare but not extinct, as July 2026's wp2shell disclosure showed; the difference is that core issues get fixed and force-updated within hours, while plugin flaws linger.


Volume is growing at an alarming rate. The ecosystem went from 5,948 new vulnerabilities in 2023 to 7,966 in 2024 and 11,334 in 2025, and highly exploitable vulnerabilities grew 113% year over year. Of the 2025 total, 4,124 flaws (36%) represented an actual exploitable threat, and 1,966 (17%) carried a high severity score, meaning they were likely to be used in automated, mass-scale attacks.


Two findings from the same report deserve special attention from anyone who buys commercial plugins:


**Premium does not mean safer.** Patchstack's focused research on premium marketplaces produced 1,983 valid vulnerability reports for paid or freemium components, 29% of all reports received. Of those, 59% were high-priority flaws usable in automated mass attacks, and 76% were exploitable in real-world conditions. Premium components also had three times more known exploited vulnerabilities than free ones, largely because closed code receives less independent security review.


**Patches often never arrive.** 46% of vulnerabilities disclosed in 2025 had no fix available from the developer at the time of public disclosure. In other words, "just keep your plugins updated" fails as a defense nearly half the time, because there is nothing to update to when the details go public.


**How fast attackers move**


Speed is the defining WordPress security statistic of 2026. When Patchstack measured the gap between public disclosure and the first observed exploitation attempt for the most heavily targeted vulnerabilities, the weighted median was five hours. Around 20% of these flaws were attacked within six hours, roughly half within 24 hours, and 70% within seven days. A site owner who patches weekly is, statistically, patching after the attack wave has already passed through.


Attackers do not abandon old vulnerabilities either. Of the ten most-attacked flaws Patchstack blocked in 2025, only four were actually published that year; the rest dated back to 2023 and 2024, including well-known issues in caching, page builder, and e-commerce payment plugins. Unmaintained sites keep old exploits profitable indefinitely.


The infrastructure meant to absorb these attacks is struggling. In two 2025 penetration-testing studies of popular hosting companies, Patchstack found that common defenses such as internal web application firewalls and CDN-level filtering blocked only 26% of vulnerability exploit attempts overall, and just 12% of WordPress-specific attacks. Broken access control, the most exploited vulnerability class of the year, is particularly hard for traditional firewalls to catch because the malicious requests look like normal authenticated traffic.


Automation supplies the volume behind all of this.[Imperva's 2025 Bad Bot Report](https://www.imperva.com/resources/resource-library/reports/2025-bad-bot-report/) found that automated traffic exceeded human traffic for the first time in a decade, reaching 51% of all web traffic, with malicious bots alone accounting for 37%, up from 32% in 2023. For a WordPress site, that translates into a constant background hum of automated probing against login pages, XML-RPC endpoints, and known plugin paths, whether or not the site has ever been specifically targeted.


**What a compromise actually costs**


WordPress security statistics matter because vulnerability exploitation is no longer a niche breach path; it is the main one. The[2026 Verizon Data Breach Investigations Report](https://www.verizon.com/business/resources/reports/dbir/) found that exploited vulnerabilities became the leading initial access vector in breaches for the first time in the report's 19-year history, at 31% of breaches, up from 20% the year before. Credential abuse fell to 13% as an initial vector, though stolen credentials still appear somewhere in 39% of breaches once attackers are inside.


The same report shows why exploitation keeps working: defenders are falling behind on patching. Only 26% of vulnerabilities on CISA's Known Exploited Vulnerabilities list were fully remediated during the study period, down from 38% a year earlier, and the median remediation time stretched from 32 to 43 days. Set that 43-day patching median against Patchstack's five-hour exploitation median and the mismatch is stark.


The financial stakes are documented in[IBM's 2025 Cost of a Data Breach Report](https://www.ibm.com/reports/data-breach) . The global average breach cost $4.44 million in 2025, down 9% from the prior year and the first decline in five years, driven mainly by faster AI-assisted detection. United States organizations moved the other way, hitting a record average of $10.22 million per breach. Organizations took an average of 241 days to identify and contain a breach, and healthcare remained the most expensive sector at $7.42 million per incident. For a small business running its storefront or patient portal on WordPress, even a small fraction of those averages is existential.


**Emerging trends and what's new in 2026**


Several developments separate the 2026 threat landscape from what older WordPress security statistics pages describe:


**Malware is getting stealthier and more persistent.** Malware intelligence firm Monarx, whose data appears in the same Patchstack whitepaper, processed nearly 9 trillion file signals across global hosting infrastructure in 2025. Its analysis shows attackers increasingly injecting malicious code into legitimate core, plugin, and theme files rather than dropping standalone malicious files, which makes automated "delete the bad file" cleanup ineffective. The Lock360 malware family goes further, running in server memory and reinfecting cleaned files such as index.php the moment they are restored.


**Cloaking now targets AI crawlers.** Dominant campaigns like Parrot TDS serve clean content to search engine bots, security scanners, and now AI training crawlers, while redirecting human visitors to phishing pages and fraudulent stores. Infections stay invisible to the tools most site owners rely on until search rankings collapse or customers complain.


**Attacks surge when staffing dips.** Monarx observed malicious file uploads nearly tripling during November and December 2025, a deliberate holiday-season spike timed to peak consumer traffic and minimal IT coverage. Security calendars should treat Q4 as the highest-risk window of the year. **‍**


**The 2026 release cycle raises the stakes.** WordPress 7.0 shipped in May 2026 with real-time collaboration and deeper AI integration, and within two months core needed the emergency wp2shell patches of July 17, 2026, delivered as[WordPress.org](https://wordpress.org/news/2026/07/wordpress-7-0-2-release/) forced updates. Notably, the vulnerable code only existed from version 6.9 onward, released in December 2025, meaning the sites at risk were the most up to date ones. New surfaces such as AI-oriented APIs expand what defenders need to watch, and the episode is a reminder that the parts of a WordPress stack without auto-update coverage, meaning most plugins and themes, are the ones that fall behind.


**Regulation is arriving.** Under the EU Cyber Resilience Act, commercial WordPress plugin vendors serving European users must have vulnerability disclosure processes in place, with obligations to report actively exploited flaws taking effect from September 2026. Patchstack expects VDPs to become a de facto standard across the ecosystem, which should improve disclosure hygiene but will also flood small vendors with reports, including a rising tide of low-quality AI-generated submissions.


**AI is reshaping both sides.** Patchstack points to AI-generated plugin code introducing new vulnerabilities as "vibe coding" merges with WordPress development, while attackers use AI to find and exploit flaws faster. IBM's data adds the organizational angle: shadow AI, meaning unsanctioned AI tools inside companies, was involved in 20% of breaches in 2025 and added an average of $670,000 to breach costs where usage was high.


The through line for 2026 is that WordPress security has become an operations problem. The core platform is mature and rarely the weak point. The risk lives in the sprawl: tens of thousands of third-party components, five-hour exploitation windows, defenses that block a quarter of attacks, and teams that patch in weeks. The organizations that stay safe are the ones with continuous visibility into what is installed, where, and whether it is current.


That operational reality extends past the website itself to every laptop and browser that logs into it. swif.ai helps IT and security teams keep the endpoints that administer their web properties patched, monitored, and compliant, with device CVE reporting and pre-built policies mapped to SOC 2, ISO 27001, and HIPAA.[Explore swif.ai's device management platform](https://www.swif.ai/products/unified-endpoint-management-uem) to see how it works.


‍


‍
