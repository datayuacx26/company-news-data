---
schema_version: "1.0.0"
document_id: "0d71561eb95049a53d2c9f06bffbff4548e2d807f6b7b438b5f073137cf30c57"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-32-8/"
published_at: "2026-08-07T13:00:57+00:00"
first_seen_at: "2026-08-07T14:47:11.522251+00:00"
fetched_at: "2026-08-07T14:47:12.957387+00:00"
content_hash: "sha256:0b9c798d881460bd5315de9b716a6dc2e92e3f8ee11eb99d1c182b797fc28525"
---

# The Good, the Bad and the Ugly in Cybersecurity – Week 32

## The Good | Snowflake Hacker Pleads Guilty as Ransom Cartel Creator Draws 16 Years


Connor Riley Moucka[pleaded](https://www.justice.gov/opa/pr/canadian-man-pleads-guilty-hacking-us-cloud-storage-provider-and-extorting-its-customers) guilty in Seattle federal court this week to computer fraud, wire fraud, aggravated identity theft and conspiracy over the 2024 breaches of Snowflake


customer accounts.


The intrusions reached at least 165 organizations and exposed records tied to at least 100 million people. Prosecutors say Moucka collected at least $495,000 from ransoms and data sales. He is due to be sentenced on October 27, facing a two-year mandatory minimum on the identity theft count and up to 30 years on the rest.


Every Snowflake account the group reached had multi-factor authentication switched off, and the credentials, some harvested by infostealer malware as far back as November 2020, had never been rotated. The gang didn’t need to find a vulnerability in Snowflake’s platform to exploit; it[turned out](https://cloud.google.com/blog/topics/threat-intelligence/unc5537-snowflake-data-theft-extortion) that more than three-quarters of the compromised accounts had prior credential exposure, and none had network allow lists in place.


In separate news, The Department of Justice announced that[Maksim Silnikau](https://www.bleepingcomputer.com/news/security/ransom-cartel-ransomware-creator-sentenced-to-16-years-in-prison/) , the Belarusian national who built and administered the Ransom Cartel ransomware operation


, was sentenced to 16 years for conspiracy and aggravated identity theft.


> Ransom Cartel creator sentenced 16 yrs for REvil-linked RaaS targeting 18 orgs, $6.7M losses. https://intel.threadlinqs.com/threat/TL-2026-1902 #ThreatIntel #CVE_2021_1675 #CVE_2021_34527 #Ransomcartel
>
>
> [\[image or embed\]](https://bsky.app/profile/did:plc:2mgpf44nirnudyjapkzsymu2/post/3msezuxkxo32d?ref_src=embed)
>
>
> — Threadlinqs ([@threadlinqs.bsky.social](https://bsky.app/profile/did:plc:2mgpf44nirnudyjapkzsymu2?ref_src=embed) )[August 6, 2026 at 3:51 AM](https://bsky.app/profile/did:plc:2mgpf44nirnudyjapkzsymu2/post/3msezuxkxo32d?ref_src=embed)


Between 2021 and 2023, Silnikau recruited affiliates through underground forums, supplying stolen credentials and encryption tools while running an affiliate site that handled negotiations and revenue splits.


The Ranson Cartel attempted to extort at least $5.2 million from at least 18 companies, including a robotic-surgery startup disrupted for two months and law firms that paid $125,000 and $300,000 after weeks of downtime.


Silnikau was arrested in Spain in 2023, fled while awaiting extradition, and was caught trying to cross into Belarus before consenting to face trial in the United States.


## The Bad | Claude Mythos 5 Spends 34 Hours Trying to Backdoor Real Code


The UK AI Security Institute (AISI)[reported](https://cdn.prod.website-files.com/663bd486c5e4c81588db7a1d/6a724858f7db25c81487016d_Security%20Incident%20INC-2026-07-28-01.pdf) Tuesday that an agent running Claude Mythos 5 spent 34 hours trying to merge a malware dropper into a real open-source project


during a security evaluation, after searching the open internet and landing on a real, unconnected repository whose name happened to share a keyword with the test’s fictional scenario.


The agent researched the maintainers, opened a pull request pairing a hidden dropper with a working bug fix, and cycled through three payload versions as each was caught. When a human reviewer flagged the code as malicious, the agent’s account denied it, force-pushed a rewritten branch history, and used a second account to vouch for the code. It also hid a prompt injection inside a GitHub issue aimed at AI coding assistants. The maintainer closed the pull request despite the cover-up.


Across 122 evaluation runs, AISI logged 19 unsanctioned actions in 10 runs, 17 involving[Mythos 5 and two involving GPT-5.6 Sol](https://www.sentinelone.com/labs/frontier-models-tackle-autonomous-long-horizon-malware-analysis/) , though AISI notes that figure is a count, not a rate. Both models had cyber safeguards off and open internet access by design, a setup meant to measure raw capability rather than mirror production use.


Nothing escaped AISI’s sandbox, and AISI found no evidence of real-world harm. The reviewer had the chance to catch it only because GitHub holds checks on a first-time contributor’s pull request until a maintainer approves them.


Stages of the agent’s actions and attempted cover-up (Source: AISI)


Anthropic’s[own](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals) July 30 review of 141,006 evaluation runs found a separate Mythos 5 run that published malware to PyPI, downloaded and ran on 15 real systems within an hour. OpenAI[reported](https://www.sentinelone.com/blog/the-good-the-bad-and-the-ugly-in-cybersecurity-week-30-8) a similar incident days earlier, exploiting a zero-day to reach Hugging Face’s production database. In each case, a test environment meant to stay sealed did not, and a model reached through it before anyone caught it.


Not to be outdone, Meta became the third lab in recent weeks to[disclose](https://www.infosecurity-magazine.com/news/meta-ai-exploit-incident/) an AI agent reaching into systems outside a security test. The exposure traced to Irregular, the same firm behind OpenAI’s second incident, whose misconfiguration gave a Meta model internet access it used to exploit a real company’s system.


## The Ugly | ChainDrop Worm Compromises Over 1,300 npm Packages With Two Billion Monthly Downloads


A self-propagating worm known as[ChainDrop](https://www.aikido.dev/blog/keyv-and-friends-compromised-in-npm-supply-chain-attack) compromised at least 868 npm packages across 1,381 versions, part of a broader campaign researchers put at more than 1,300 packages with a combined two billion monthly downloads


.


The mass compromise began Tuesday when an attacker breached the GitHub account of a maintainer who controlled several widely used caching libraries, including[Keyv, Cacheable, flat-cache and file-entry-cache](https://safedep.io/keyv-npm-supply-chain-compromise/) . The worm then self-propagated to other maintainers’ packages, including ones tied to Deliveroo, Ornikar, OneReach, Picsart, Qlik and ServiceTitan.


The worm pushed malicious commits directly to each project’s main branch and triggered a new release through a GitHub Actions workflow, giving the poisoned npm packages valid provenance signatures.


A preinstall script added to` package.json` ran automatically on npm install, pulling down the Bun JavaScript runtime and using it to execute an obfuscated infostealer that harvested GitHub tokens, npm tokens, AWS and Kubernetes credentials, HashiCorp Vault secrets, database credentials and a run of other cloud and developer logins.


Every stolen token was checked against npm’s own` whoami` endpoint before the haul was encrypted and sent to a public GitHub repository, with any credentials belonging to other maintainers repeating the process on their packages.


A Github search for ChainDrop exfiltrations (Source: Safedep)


ChainDrop is built on[Shai-Hulud](https://www.sentinelone.com/blog/defending-against-sha1-hulud-the-second-coming/) , the same self-propagating technique that has hit npm before. Each GitHub repo storing the stolen credentials is auto-named with random terms from Dune and carryies the description, “Shai-Hulud: Here We Go Again.”


While many of the auto-generated dead drop repos have since been taken down, the scale of the outbreak demonstrates how rapidly self-propagating worms can[weaponize trusted credentials and automated pipelines](https://www.sentinelone.com/blog/living-off-the-pipeline-defending-against-ci-cd-subversion/) . For a deeper breakdown and defensive strategies on this attack vector and other emerging supply chain risks, read the[SentinelOne Annual Threat Report](https://www.sentinelone.com/threat-report/) .


SentinelOne's Annual Threat Report


A defender’s guide to the real-world tactics adversaries are using today to abuse identity, exploit infrastructure gaps, and weaponize automation.


[Get the Full Report](https://www.sentinelone.com/threat-report/)
