---
schema_version: "1.0.0"
document_id: "73b79426d6fbbbe17fca8bddbe7033c9cbf7d21e45fc91f85011215a16897c06"
company_key: "netscout-systems-inc-common-stock"
company: "NetScout Systems Inc."
source_id: "netscout-systems-inc-common-stock-news-import-adeff42e5f7b"
canonical_url: "https://www.netscout.com/blog/asert/ddos-hire-and-evolving-use-ai"
published_at: null
first_seen_at: "2026-07-25T16:47:01.498587+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:46ac56f07f3a979ca07f29fd65bb060587976537695093954e1d4e9b16914318"
---

# DDoS-for-Hire and the Evolving Use of AI

## Executive Summary


Since our[seven-part analysis](https://www.netscout.com/blog/asert/2024-ddos-hire-landscape-part-1) of the DDoS-for-hire landscape in December 2024, the integration of[artificial intelligence (AI) into the booter/stresser ecosystem](https://www.netscout.com/blog/asert/when-chatbots-become-attack-coordinators-ai-revolution-ddos) has accelerated significantly. What was then an emerging trend has now become an operational reality across multiple stages of the attack lifecycle.


This update examines developments through Q4 2025, focusing on three key areas: **the proliferation of malicious large language models (LLMs) purpose-built for offensive operations, AI-generated tools explicitly designed for botnet recruitment, and the measurable impact on attack volume and sophistication.** These developments represent a fundamental shift in how DDoS-for-hire services operate and who can access them.


## Key Takeaways


- **Malicious LLMs now offer tiered access:** From free tools (KawaiiGPT) to premium platforms (Xanthorox), AI-assisted attack development is available at every price point.
- **AI is accelerating botnet recruitment:** The KuroCracks scanner demonstrates explicit use of ChatGPT to optimize exploitation tools for botnet growth.
- **Underground AI adoption is quantifiable:** 219 percent increase in malicious AI tool mentions, 52 percent increase in jailbreaking discussions.
- **Attack metrics reflect expanded threat actor population:** Consistent acceleration in attack volume and sophistication across vendor reports.
- **Evasion is evolving:** AI-powered traffic mimicry and adaptive attack patterns challenge signature-based detection.
- **Law enforcement faces a resilience problem:** AI enables rapid reconstitution of seized infrastructure.


## The Malicious LLM Ecosystem Matures


The underground market for malicious AI tools has evolved beyond simple jailbreaks of commercial models such as ChatGPT. Threat actors now have access to purpose-built platforms designed specifically for offensive cyber operations, with DDoS tool development among their advertised capabilities. The implication for distributed denial-of-service (DDoS) operations is clear: The time from “idea” to “weaponized script” continues to compress.


### GhostGPT: Purpose-Built for Speed


In January 2025,[Abnormal Security documented](https://abnormal.ai/blog/ghostgpt-uncensored-ai-chatbot) **GhostGPT** , a Telegram-distributed malicious chatbot marketed for rapid exploit development. **Priced at just $50/week** , GhostGPT specifically advertises malware creation and exploit code generation with no ethical guardrails. Unlike Xanthorox’s complex modular architecture, GhostGPT prioritizes accessibility and speed, with users reporting functional attack scripts within minutes of initial prompts.


### WormGPT Resurfaces with New Variants


Despite the original WormGPT developer’s announcement that they were shutting down operations in 2023, the tool has proven resilient.[Unit 42 researchers](https://unit42.paloaltonetworks.com/dilemma-of-ai-malicious-llms/) documented **WormGPT 4** advertisements appearing on Telegram channels in late September 2025, with subscriber counts exceeding 500 users. CATO Networks separately identified[new WormGPT variants](https://www.csoonline.com/article/4008912/wormgpt-returns-new-malicious-ai-variants-built-on-grok-and-mixtral-uncovered.html) built on open-source foundations, including Mixtral and xAI’s Grok models.


Analysis from[Moxso](https://moxso.com/blog/wormgpt-on-the-loose) confirms that WormGPT retains capabilities directly relevant to DDoS operations, including the ability to generate “malicious code designed to take down websites and infrastructure by overloading them with traffic.” **The pricing model remains accessible: $50 per month/$175 annually/$220 lifetime** , placing sophisticated script generation within reach of virtually any threat actor.


### Xanthorox AI: A Modular Offensive Platform


The most significant new entrant in 2025 is **Xanthorox AI** , which first appeared on Telegram in October of 2024 and then on darknet forums in late Q1 2025.


Xanthorox operates as a modular platform with five specialized AI models spanning code generation, image analysis, social engineering content, voice interaction, and real-time reconnaissance via more than 50 search engines.


The subscription model reportedly runs **$300/month or $2,500 annually** on dark web channels, positioning Xanthorox as an “enterprise-grade” offensive AI platform.


**Important caveat:**[Trend Micro’s technical review](https://www.trendmicro.com/vinfo/us/security/news/cybercrime-and-digital-threats/the-devil-reviews-xanthorox-a-criminal-focused-analysis-of-the-latest-malicious-llm-offering) of Xanthorox identified significant limitations, including reliance on Google infrastructure and missing advertised features. As with many underground tools, marketing claims may exceed actual capabilities. However, even partially functional offensive AI tools lower barriers meaningfully.


### KawaiiGPT: Free and Accessible


At the other end of the spectrum,[Picus Security](https://www.picussecurity.com/resource/blog/malicious-ai-exposed-wormgpt-malterminal-and-lamehug) and[Palo Alto’s Unit 42](https://unit42.paloaltonetworks.com/dilemma-of-ai-malicious-llms/) documented **KawaiiGPT** , an anime-themed malicious LLM that “democratizes cybercrime by allowing any amateur to generate professional-grade attacks for free.” The July 2025 v2.5 release is available on GitHub, with setup taking less than five minutes.


## AI-Optimized Vulnerability Scanners for Botnet Recruitment


Perhaps the most directly relevant development for DDoS infrastructure is the emergence of AI-assisted tools explicitly designed to accelerate botnet growth. However, it’s important to note that AI-enhanced vulnerability scanning is not exclusive to malicious actors; offensive security (offsec) professionals and penetration testers also leverage similar tools for white-hat purposes, such as identifying vulnerable devices before attackers can exploit them and helping organizations remediate weaknesses in their infrastructure.


### The KuroCracks Scanner


[S2W TALON’s threat intelligence](https://www.s2w.inc/en/resource/detail/825) documented a January 2025 post on the Cracked Forum by a threat actor using the handle “KuroCracks.” The post advertised an open-source scanner for **CVE-2024-10914** (a remote code execution vulnerability) with the explicit title:


> CVE-2024-10914 SCANNER - REMOTE CODE EXECUTION - GREAT FOR BOTNETS - OPEN SOURCE


The scanner combined Masscan automation with exploit delivery, but what makes it significant is that KuroCracks explicitly stated the tool was **optimized using ChatGPT** . The threat actor shared prompt engineering techniques used to elicit exploit code from AI models, providing a template for others to follow.


This represents a concrete example of AI accelerating the vulnerability-to-botnet pipeline. Rather than manually developing exploitation tools, threat actors can now iterate rapidly using commercial LLMs, even when those models have guardrails in place.


Separately, S2W TALON documented threat actors targeting LLM infrastructure itself, with one BreachForums user offering exploits for the Google Gemini API, indicating criminal communities view AI platforms as high-value targets.


## Underground AI Tool Adoption Accelerates


Multiple threat intelligence providers have quantified the surge in underground AI activity.


## KELA’s Findings


[KELA’s 2025 AI Threat Report](https://www.infosecurity-magazine.com/news/dark-web-mentions-malicious-ai/) found the following:


- **219 percent increase** in mentions of malicious AI tools and tactics on monitored dark web sources
- **52 percent increase** in discussions specifically about jailbreaking legitimate AI tools


The trajectory is notable: KELA tracked 4,167 mentions of jailbreaking techniques in 2024, up from 2,747 in 2023. Among the most frequently shared: “DAN” (Do Anything Now) prompts and techniques for bypassing content filters in ChatGPT and Gemini to generate network flooding scripts. This represents not just awareness, but active skill-building within criminal communities. Threat actors are systematically documenting and sharing techniques to extract DDoS-relevant code from commercial models, even when those models have guardrails in place.


### Flashpoint’s Analysis


[Flashpoint’s research](https://cybernews.com/cybercrime/generative-ai-threat-intelligence-flashpoint/) , based on analysis of more than 2.5 million AI-related posts from more than 100,000 illicit sources, identified active communities sharing “bypass builders” that specialize in defeating ChatGPT and Gemini guardrails.


**The implication is clear:** Even without access to purpose-built malicious LLMs, threat actors are actively developing techniques to weaponize commercial AI tools for offensive purposes, including DDoS attack development.


## The Measurable Impact: 2025 Attack Statistics


Industrywide data confirms the acceleration. NETSCOUT’s ATLAS platform observed more than 8 million DDoS attacks globally in[the first half of 2025](https://www.netscout.com/threatreport) , with peak attacks reaching 3.12Tbps in bandwidth and 1.5Gpps in throughput.[ENISA’s October 2025 Threat Landscape report](https://www.enisa.europa.eu/sites/default/files/2025-10/ENISA%20Threat%20Landscape%202025_0.pdf) found that DDoS accounted for 77 percent of all reported cyber incidents in the EU. These metrics reflect an expanded threat actor population enabled by lowered technical barriers.


Although no single metric can be attributed solely to AI adoption, the consistent pattern of acceleration across vendors suggests that lowered barriers are expanding the threat actor population.


### The Acceleration Effect


[Unit 42’s incident response research](https://www.paloaltonetworks.com/blog/2025/05/unit-42-develops-agentic-ai-attack-framework/) demonstrated how AI compresses attack timelines. Researchers simulated a complete ransomware attack, from initial access to data exfiltration, **in 25 minutes using AI assistance at every stage** . This represents approximately a 100x speed increase over traditional methods.


The same acceleration principles apply to DDoS operations: Reconnaissance that once took days can now be completed in minutes, and custom attack scripts can be generated on demand rather than developed over weeks.


## AI-Enhanced Evasion Techniques


Beyond tool development, AI is increasingly applied to attack execution.


Imperva’s 2025 Bad Bot Report notes that “AI is fueling bot attacks, making them more intelligent and more evasive than ever before.” The report found simple bot attacks increasing from 34 percent to 52 percent in the travel sector, “supporting the theory that AI is fueling a surge in simple bot activity” that overwhelms traditional defenses through sheer volume and variation. **Capabilities now include automated traffic shaping to mimic legitimate users and dynamic parameter adjustment based on observed defensive responses.**


## Law Enforcement Continues Pressure


Law enforcement sustained pressure throughout 2025. Key enforcement actions that started in 2024 and continued into 2025 include:


- **2024:**[27 domains seized, 18 booter services disrupted](https://www.theregister.com/2024/12/12/operation_poweroff_ddos_takedowns/) , including zdstresser.net, starkstresser.net, and orbitalstress.net; three administrators arrested, 300 users identified ([BleepingComputer](https://www.bleepingcomputer.com/news/security/operation-poweroff-shuts-down-27-ddos-for-hire-platforms/) ).
- **May 2025:** Nine domains seized and[six DDoS-for-hire platforms disrupted](https://www.bleepingcomputer.com/news/security/police-takes-down-six-ddos-for-hire-services-arrests-admins/) ; Poland arrested four administrators of platforms, including Cfxapi, Cfxsecurity, neostress, jetstress, quickdown, and zapcut ([DOJ](https://www.justice.gov/usao-cdca/pr/law-enforcement-seizes-9-ddos-hire-webpages-part-global-crackdown-booter-and-stresser) ).
- **July 2025** ( **Operation Eastwood** ): Europol coordinated a 12-country operation targeting **NoName057(16)** , the pro-Russian hacktivist group. The operation disrupted more than 100 servers, issued seven international arrest warrants (six for Russian nationals), and made two arrests in France and Spain. Notably, authorities contacted more than 1,100 DDoSia contributors about potential criminal liability, a deterrence strategy aimed at the group’s crowd-sourced attack model ([Europol](https://www.europol.europa.eu/media-press/newsroom/news/global-operation-targets-noname05716-pro-russian-cybercrime-network) ).
- **August 2025:** DOJ charged a 22-year-old Oregon man for operating **RapperBot** , a botnet responsible for more than 370,000 DDoS attacks against 18,000 victims in more than 80 countries. The botnet controlled between 65,000 and 95,000 infected devices with an attack capacity of between 2 and 6 Tbps ([DOJ](https://www.justice.gov/opa/pr/us-citizen-charged-operating-rapperbot-ddos-botnet) ).


UK National Cyber Crime Unit’s Frank Tutty noted that “booter services are an attractive entry-level cybercrime, and users can go on to even more serious offending,” a trajectory that AI tools now accelerate.


## Conclusion: The Adaptive Imperative


The developments documented here, from tiered malicious LLM access to AI-optimized botnet recruitment to **accelerating attack volumes** , represent the current state of AI integration into DDoS-for-hire operations. Every prediction in our[earlier analysis](https://www.netscout.com/blog/asert/when-chatbots-become-attack-coordinators-ai-revolution-ddos) has materialized: AI-enhanced attacks now analyze defensive responses in real time, attack scripts are generated on demand via natural language interfaces, and the barrier between intent and capability continues to collapse.


When one platform is seized, the underlying tools, techniques, and customer base migrate to successors within days rather than months.


For defenders, the trajectory demands equally intelligent capabilities deployed proactively. Static, signature-based defenses face an increasingly untenable position against adaptive, AI-enhanced attacks. Organizations must assume that the threat actor population will continue expanding, that attack sophistication will continue increasing, and that the window between vulnerability disclosure and weaponized exploitation will continue shrinking.


The adaptive imperative is no longer theoretical; it’s operational.
