---
schema_version: "1.0.0"
document_id: "06bfddcf228e1565d2c4af709e6d95215e020c4a3a048dba9bf35580de4053c1"
company_key: "check-point-software-technologies-ltd-ordinary-shares"
company: "Check Point Software Technologies Ltd."
source_id: "check-point-software-technologies-ltd-ordinary-shares-rss-6929dc8f3e59"
canonical_url: "https://blog.checkpoint.com/ai-security/ai-security-threats-in-2026-insights-from-check-point-research/"
published_at: "2026-07-14T01:00:42+00:00"
first_seen_at: "2026-07-20T04:35:32.274009+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:c83c09bba1dac94ba4175de858be6147011e450a92059328538d31dfc8329182"
---

# AI Security Threats in 2026: Annual Insights from Check Point Research

Share


-
-
-
-
-


##### Key Takeaways


- Vulnerability response times have collapsed from days to hours. AI can reason about code well enough to generate working exploits at scale, so defenders now face patch windows of 12 to 72 hours instead of the traditional timeframe
- Your exposed AI infrastructure is being actively probed right now. Model servers, inference endpoints, and agent control panels are facing the internet, and most security teams don’t know they’re there
- Data leakage through approved AI use doubled in one year. Employees sharing context with generative AI to get useful answers are exposing credentials and source code in ordinary workflows, no attack required
- In a single operation earlier this year, one attacker ran Claude Code and GPT-4.1 in parallel to breach nine Mexican government agencies and extract 400 million records. The AI ran the operation with minimal human direction between steps
- Speed, visibility, and governance have to move together. The defense has to operate at machine speed, find what it cannot see, and control how employees use AI every day


For years, the cyber security industry tracked AI as a force multiplier: something that made existing attack techniques faster, cheaper, and more accessible. That framing was accurate. But the annual[AI Security Report 2026](https://engage.checkpoint.com/ai-security-report-2026) from[Check Point Research](https://research.checkpoint.com/) documents a transition that goes further. AI has crossed from assistant to operator. Where it once helped attackers prepare, it now runs the operation. What follows is a structured review of the report’s key findings, grounded in original incidents and case studies from the past twelve months.


##### How is AI being used to power attacks


AI now participates directly at every stage of the attack chain, from writing malware to executing commands inside live networks with minimal human direction between steps. The clearest example from the past year was the breach of nine Mexican government agencies \[1\] between late 2025 and early 2026. A single operator ran Claude Code and GPT-4.1 in parallel, one handling live exploitation across 34 sessions, the other analyzing stolen data and automatically tasking follow-on activity. The human set the architecture in motion. The AI ran the operation, producing more than 5,000 executed commands and exposing roughly 400 million records.


AI has also industrialized the criminal tooling market. Phishing-as-a-service platforms \[2\] now embed language models directly into the attack workflow, automatically scanning stolen accounts, mimicking the victim’s writing style, and generating convincing follow-on scam emails. Voice fraud platforms \[3\] run fully automated AI agents that walk targets through scripted account-recovery calls to steal one-time passcodes, with no human caller involved. The jailbreak is built into the product, so a buyer needs no AI skill at all to run a sophisticated, multi-step attack.


##### What has AI done to the vulnerability window


AI is now capable enough at reasoning about code that it speeds up both sides of the race simultaneously. Google’s Threat Intelligence Group[reported](https://cloud.google.com/blog/topics/threat-intelligence/ai-vulnerability-exploitation-initial-access) the first AI-assisted zero-day built for mass exploitation, while[other research](https://cybersecuritynews.com/new-study-shows-gpt-5-2-can-reliably/) showed frontier models producing working zero-day exploits at scale. The practical effect is compression: a vulnerability disclosure that once gave defenders days to respond now gives them hours. US Government CISA[responded](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) by requiring agencies to remediate the highest-risk vulnerabilities within three days. India’s CERT-In went further,[advising](https://cybersecuritynews.com/cert-in-asks-patch-vulnerabilities-12-hours/) organizations to patch critical systems within 12 hours.


##### How has AI itself become an attack surface


As organizations embedded AI into email, documents, code, and core business workflows, the AI stack became a target in its own right.[Indirect prompt injection](https://www.lakera.ai/blog/indirect-prompt-injection) where malicious instructions are hidden inside content an AI reads as part of its normal work, has moved from proof-of-concept to operational threat.[Check Point AI Security](https://www.checkpoint.com/ai-security/) recorded a roughly fivefold increase in detections of large malicious[prompt-injection](https://www.checkpoint.com/cyber-hub/cyber-security/what-is-cyber-attack/what-is-a-prompt-injection-attack/) payloads between March and May 2026, consistent with indirect injection becoming a routine attack path rather than a theoretical one.


Figure 1: Malicious prompt detection rate by payload size


AI infrastructure has also become a target through conventional means. A critical flaw in Ollama \[4\] left roughly 300,000 internet-facing model servers leaking prompts, keys, and environment variables. GreyNoise \[5\] recorded around 91,000 attack sessions probing LLM deployments in a single quarter. The AI software supply chain has proven equally exposed: the Shai-Hulud worm in November 2025 compromised hundreds of widely used code packages and tens of thousands of repositories, stealing developer credentials as it spread automatically through build pipelines.


##### What has AI done to digital identity


Voice, face, documents, and real-time video can all now be convincingly synthesized, meaning none of them can stand alone as proof of identity. Over the past year \[6\], real-time face-swap moved from nation-state operations into industrialized fraud. Document forgery commoditized to the point where one[service](https://www.justice.gov/usao-sdny/pr/creator-onlyfake-charged-and-pleads-guilty-selling-more-10000-digital-fake) sold more than 10,000 AI-generated fake IDs capable of passing bank KYC checks across 56 countries. A North Korean-linked[group](https://home.treasury.gov/news/press-releases/sb0230) took this furthest, using AI-fabricated personas to get operatives hired inside Western companies as legitimate remote employees, generating close to 800 million dollars for the regime’s weapons programs.


Figure 2: Generative identity threats by media type and maturity (Check Point Research, 2025)


##### How much sensitive data is leaking through enterprise AI tools


High-risk GenAI prompts doubled from 2 percent to 4 percent over the past year. The average organization runs 10 AI applications per month, many without formal approval. Business Services had the highest rate of any industry, nearly one in every 17 AI interactions carried a real risk of sensitive data exposure, by May 2026, this climbed to 1 in 14 AI interactions. Most of this exposure comes not from attacks but from ordinary approved use, where employees share more context than they realize to get a useful answer.


Figure 3: High-risk prompts by region


##### How does Check Point address these threats


The risks in this report fall into three categories, and each one calls for a different kind of defense: protecting AI itself, matching the speed of AI powered attacks, and governing how AI actually gets used across the workforce.


##### Protecting AI systems


Most security teams cannot see the riskiest part of their own AI attack surface, so protection starts with visibility and extends into how agents behave once they are live.


- [AI Agent Security](https://www.checkpoint.com/ai-security/ai-agent-security/) governs how agents interact with prompts, tools, data, and actions in real time, preventing manipulation through prompt injection, poisoned configurations, and unsafe tool use
- [AI Red Teaming](https://www.checkpoint.com/ai-security/ai-red-teaming/) tests whether AI applications can be tricked into exposing sensitive data or bypassing policies, before attackers get the chance, and validates security again after every meaningful change to models, prompts, or permissions
- [WAF](https://www.checkpoint.com/cloudguard/waf/) , powered by a dual layer ML engine, blocks prompt injections and unsafe content at the perimeter without needing signatures or causing downtime
- [AI Factory Security](https://www.checkpoint.com/ai/factory-firewall/) delivers a layered defense across hardware, workloads, containers, inference APIs, and endpoints for organizations building their own AI infrastructure
- [Exposure Management](https://www.checkpoint.com/exposure-management/) discovers every internet facing AI asset, including exposed model servers and agent control panels, and flags newly exposed infrastructure the moment it appears


##### Matching the pace of AI powered attacks


Intrusions now span dozens of targets at once, with AI handling the operational work between check ins. Security teams working at human speed simply cannot keep that pace, which is why the defense has to run on AI too.


- ThreatCloud AI runs at two speeds simultaneously, generating continuous background intelligence while answering real time queries from Check Point sensors around the world, connected across networks, email, endpoints, mobile, and cloud
- The[Frontier AI Models Readiness Program](https://blog.checkpoint.com/security/check-point-frontier-ai-models-readiness-program-security-update/) , including Check Point’s internal, model agnostic BLAST technology, proactively uncovers and resolves vulnerabilities in the frontier models that power Check Point’s own defenses, closing the gap before it can be weaponized elsewhere


##### Governing AI use from the inside


Much of the exposure in this report never came from an attack at all. It came from ordinary, approved use, where employees shared more than they realized just to get a useful answer.


- [Workforce AI Security](https://www.checkpoint.com/ai-security/ai-workforce-security/) discovers both sanctioned and unsanctioned AI applications across the organization and applies real time data loss prevention to GenAI prompts, so credentials, source code, and customer data stop leaving through everyday AI use
- Exposure Management extends that same visibility to the external surface, ranking exposures by what is genuinely exploitable rather than by scanner volume, and through its Brand Protection and Threat Intelligence layers, it also catches phishing pages, cloned sites, and stolen credentials, including AI service logins, already circulating on the deep and dark web


To read the full findings, access the AI Security Report 2026 from Check Point Research[here](https://engage.checkpoint.com/ai-security-report-2026)


##### Further Reading and Sources


\[1\] Gambit Security, “A Single Operator, Two AI Platforms, Nine Government Agencies: The Full Technical Report” https://gambit.security/blog-posts/a-single-operator-two-ai-platforms-nine-government-agencies-the-full-technical-report


\[2\] Sekoia, “New Widespread EvilTokens Kit: Device Code Phishing-as-a-Service” https://blog.sekoia.io/new-widespread-eviltokens-kit-device-code-phishing-as-a-service-part-1


\[3\] Abnormal Security, “ATHR: AI Voice Phishing and TOAD Attacks” https://abnormal.ai/blog/athr-ai-voice-phishing-toad-attack


\[4\] Cyera, “Bleeding Llama: Critical Unauthenticated Memory Leak in Ollama” https://www.cyera.com/research/bleeding-llama-critical-unauthenticated-memory-leak-in-ollama


\[5\] GreyNoise, “Threat Actors Actively Targeting LLMs” https://www.greynoise.io/blog/threat-actors-actively-targeting-llms


\[6\] Malwarebytes, “Scam Compounds Hiring AI Models to Seal Deal in Deepfake Video Calls” https://www.malwarebytes.com/blog/news/2026/03/scam-compounds-hiring-ai-models-to-seal-deal-in-deepfake-video-calls
