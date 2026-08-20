---
schema_version: "1.0.0"
document_id: "578cc74521c9e318f906120022a6d120b605123c912afe886f458bbf331fc709"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-052596d63611"
canonical_url: "https://unit42.paloaltonetworks.com/ai-token-jacking/"
published_at: "2026-08-06T10:00:49+00:00"
first_seen_at: "2026-08-06T12:40:04.208679+00:00"
fetched_at: "2026-08-06T12:40:06.135100+00:00"
content_hash: "sha256:0b10faf63e7f706f6232edbb955bd10e280d92a73df1ef54e34273194f7cdc0a"
---

# Token Jacking: Cybercriminals Could Be Stealing Your AI Resources

## Executive Summary


It’s three a.m., do you know what your AI agent is doing? Unit 42 has responded to a growing number of AI token jacking cases resulting in staggering financial losses.


The financial loss comes from criminals gaining access to API keys used by legitimate developers for access to popular AI platforms. These keys are known as tokens, and their theft is called token hijacking, or token jacking for short.


The unrelenting frenzy of AI adoption and soaring costs of model access are converging into an irresistible opportunity for cybercriminals. Premium pricing on scarce AI processing power means stolen access via tokens can generate a quick and easy profit for attackers. Complex, patchwork billing management and limitless scaling by default can lead to massive financial losses in short periods.


Good security hygiene, combined with cutting-edge native AI protection tools, can prevent losses before they begin.


Palo Alto Networks customers are better protected through the following products and services:


- [Prisma AIRS AI Gateway](https://www.paloaltonetworks.com/ai-security/ai-gateway)
- [Idira Agentic Identity Security](https://www.paloaltonetworks.com/idira/agentic)
- [Koi Agentic Endpoint Security](https://www.koi.ai/product/endpoint)
- [Cortex XDR](https://docs-cortex.paloaltonetworks.com/p/XDR) and[XSIAM](https://docs-cortex.paloaltonetworks.com/p/XSIAM)
- [Cortex Cloud Identity Security](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/What-is-Cortex-Cloud-Identity-Security)
- [Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering)


The[Unit 42 AI Security Assessment](https://www.paloaltonetworks.com/unit42/assess/ai-security-assessment) can help empower safe AI use and development.


If you think you might have been compromised or have an urgent matter, contact the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) .


**Related Unit 42 Topics** **[AI](https://unit42.paloaltonetworks.com/tag/ai/) ,[LLM](https://unit42.paloaltonetworks.com/tag/llm/) ,[Supply Chain](https://unit42.paloaltonetworks.com/tag/supply-chain/)**


## How Tokens Work


Token jacking is a new AI-oriented spin on an old technique of stealing access to computing resources.


Establishing a session in service-based computing typically requires authentication, usually involving a username and password, and sometimes a secondary verification method. Many services allow an authenticated user to then generate keys that programs can use on a user's behalf to establish sessions without going through an interactive login to support automated processes. Within a session, the service provider and user have agreed on a structured way to pay to use their service to achieve a pre-defined objective.


AI — in particular, large language models (LLMs) — typically does not have pre-defined objectives. Users can and do carry on long conversations of widely varying complexity, which can consume enormous amounts of the provider’s computing resources. Automated processes also use LLMs to produce iterative content, which they then further process and return to the LLM with additional, related prompts.


To best support this freeform usage, providers typically break both the input prompt and the output data into small chunks called tokens. Regardless of the objective, billing is then based on how many of these tokens are consumed during the session.


Newer and more complex AI models charge more per token, ostensibly because more resources are required to deliver the output. To avoid interruptions in unpredictable workstreams, many providers do not limit the number of tokens an account can consume, instead tallying usage and billing on a cycle.


If an attacker can steal one of these keys, they may find themselves with unlimited programmatic access to tokens that they can then use themselves or resell to other users. Since billing occurs cyclically, the victim might not even be aware of the theft until the attacker has consumed a massive number of tokens.


## Transfer Stations


To better understand token jacking, we must understand transfer stations. Skyrocketing token costs for[frontier AI models](https://www.nvidia.com/en-us/glossary/frontier-models/) and regional usage restrictions have spawned a massive gray market of fly-by-night vendors selling AI computing capacity at a fraction of the retail cost.


Figure 1 below shows an example of these advertisements. These services are commonly called transfer stations.


Figure 1. Advertisement for gray-market frontier model access.


Third parties acting as intermediaries between official AI providers and end users sell these transfer stations. Many of these advertisements appear on Chinese-language marketplaces like Taobao. They promise access to multiple AI services with seller-issued custom credits that are purchased anonymously. Earlier this year, a researcher named Harshal Singh posted a fascinating[deep dive into this world](https://x.com/HarshalsinghCN/status/2056626175959826692) .


A large number of these transfer stations run on just a few open-source software platforms like[new-api](https://github.com/QuantumNous/new-api/blob/main/README.en.md) or[one-api](https://github.com/songquanpeng/one-api/blob/main/README.en.md) , which act as proxy services to official AI APIs. These proxy services handle:


- Obfuscation
- Rotation and authentication of real credentials
- Billing
- Model routing
- Normalization of prompts


In many cases, users of these transfer station services are developers seeking inexpensive AI access. Other use cases are less benign.


Competing nation-states can use these transfer stations' proxy services to access cutting-edge frontier models to train and refine their own models at a fraction of the cost that AI development normally incurs. Transfer stations require access to legitimate API tokens for the associated AI models. Attackers often steal or hijack these tokens from a variety of legitimate sources.


## How Transfer Stations Obtain Tokens


For transfer stations to be cost-effective, their operators require access to a large pool of discounted legitimate tokens for each frontier AI model offered. Purchasing tokens at full price to simply resell them at a discount isn’t profitable, so many operators turn to stolen credentials.


Attackers can use privileged corporate developer accounts they’ve harvested via information stealers or through phishing campaigns to perform the following activities:


- Creating new API keys
- Provisioning models
- Removing billing limits
- Disabling critical usage alerts and logging


These developer accounts are readily available for sale by access brokers on dark web marketplaces.


However, a more direct approach is to steal already provisioned access keys. Attackers can harvest these like they do credentials. They can also mine keys from improperly secured file shares or code repositories.


More recently, attackers have stolen these keys using poisoned, self-propagating npm packages downloaded by unsuspecting developers. Once installed, these packages infect any other code releases the developer builds. They steal credentials and access tokens from each environment along the way, amplifying the impact.


Particularly concerning are[npm supply chain attacks](https://unit42.paloaltonetworks.com/monitoring-npm-supply-chain-attacks/) like Shai-Hulud and Miasma. Attackers could use the huge number of credentials stolen in these campaigns to fuel transfer stations for years.


## Impact of Transfer Stations' Token Jacking


The financial impact of token jacking can be catastrophic to organizations. Transfer stations can generate tens of millions of API calls per day, resulting in hundreds of thousands of dollars in usage fees.


We’ve responded to cases where attackers stole inadvertently exposed credentials and integrated them into a transfer station within minutes. This led to nearly a million dollars in charges before discovery and containment.


In some of these cases, we connected massive numbers of malicious API queries to domains hosting the new-api proxy service. Figure 2 shows an example of a transfer station frontend marketplace hosted on an IP address running an instance of new-api and connected to an attack.


Figure 2. Webpage from a transfer station site with prices for different AI models.


Organizations impacted by token jacking have very little recourse to recover funds billed by the AI services for using their API tokens. The cost can derail budgets or even force smaller businesses into bankruptcy.


Even unsuspecting developers trying to use transfer stations for legitimate development risk having their prompts routed to inferior models. Furthermore, developers risk having their sessions monitored and mined for sensitive data that could turn them into future victims.


## Mitigation


Organizations can protect themselves against token jacking through various methods.


- Implement spending limits for AI usage


- Ensure that these limits alert organizations if usage changes drastically from an established baseline


- Review all privileged accounts that can be used to provision resources or adjust spending limits
- Migrate from long-term access keys to short-term bearer tokens to limit the potential window of damage
- Use an AI gateway in combination with a machine authentication platform


- This can help ensure that all LLM traffic is tied to a verified and managed machine identity, allowing for real-time monitoring of traffic and usage anomalies


- Ensure that compute resources include network boundaries where available


- This restricts access to corporate infrastructure, preventing compromised keys from being used in a transfer station scenario


- Tightly manage development environments to ensure malicious packages do not enter the development pipeline


## Conclusion


AI adoption is accelerating at an unprecedented pace. A mindset of “fail fast and break things” has never been more true — or more risky — than it is today.


This mindset brings with it an opportunity for cybercriminals to target vulnerable organizations through token jacking and to cause staggering losses. While innovation cannot be at the mercy of security, there are ways defenders can manage their risk.


### Prisma AIRS AI Gateway


The[Prisma AIRS AI Gateway](https://www.paloaltonetworks.com/ai-security/ai-gateway) helps provide a central control plane to secure and govern enterprise AI traffic. By managing API keys centrally, it removes sensitive credentials from developer environments and build systems. Platform teams can gain full visibility into model usage, agent actions, and token spend across teams. Security teams get integrated guardrails that can enforce access policies, prevent data leaks, and set proactive budget limits.


### Idira Agentic Identity Security


[Idira Agentic Identity Security](https://www.paloaltonetworks.com/idira/agentic) helps provide a comprehensive identity security solution for discovery, control and governance of agentic identities. It provides a central registry of agents with cryptographically verifiable identities, enforces strong authentication and zero standing privileges for agents and provides comprehensive audit trails of agent actions. It also enables agents to secretly retrieve and use secrets and API tokens just in time thereby reducing the attack surface.


### Koi Agentic Endpoint Security


[Koi Agentic Endpoint Security](https://www.koi.ai/product/endpoint) helps discover all software on your endpoints, both binary and non-binary, from installed applications to code packages and AI artifacts. From there you can govern it, whether that means removing a risky or malicious item, or holding new package versions back until they've had time to establish a reputation under public scrutiny.


### Cortex Cloud, XDR and XSIAM


[Cortex Cloud](https://www.paloaltonetworks.com/cortex/cloud) ,[XDR,](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Endpoint-protection) and[XSIAM](https://docs-cortex.paloaltonetworks.com/p/XSIAM) customers are better protected from the topics discussed within this article with cloud[runtime security](https://www.paloaltonetworks.com/cortex/cloud/runtime-security) operations monitoring their continuous integration and continuous development (CI/CD) pipelines to ensure that the latest npm packages integrated into test and production environments are monitoring for and preventing malicious code execution.


### Cortex Cloud Identity Security


Using[Cortex Cloud’s Identity Security](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/What-is-Cortex-Cloud-Identity-Security) which includes Cloud Infrastructure Entitlement Management (CIEM), Identity Security Posture Management (ISPM), Data Access Governance (DAG) as well as Identity Threat Detection and Response (ITDR), allows clients to monitor cloud identities which may have been compromised as a result of the techniques discussed in this article. Enabling these features helps protect cloud identities.


### Advanced URL Filtering


[Advanced URL Filtering](https://docs.paloaltonetworks.com/advanced-url-filtering) identifies known domains and URLs associated with this activity as malicious.


If you think you may have been compromised or have an urgent matter, get in touch with the[Unit 42 Incident Response team](https://start.paloaltonetworks.com/contact-unit42.html) or call:


- North America: Toll Free: +1 (866) 486-4842 (866.4.UNIT42)
- UK: +44.20.3743.3660
- Europe and Middle East: +31.20.299.3130
- Asia: +65.6983.8730
- Japan: +81.50.1790.0200
- Australia: +61.2.4062.7950
- India: 000 800 050 45107
- South Korea: +82.080.467.8774


Palo Alto Networks has shared these findings with our fellow Cyber Threat Alliance (CTA) members. CTA members use this intelligence to rapidly deploy protections to their customers and to systematically disrupt malicious cyber actors. Learn more about the[Cyber Threat Alliance](https://www.cyberthreatalliance.org/) .


## Indicators of Compromise


Table 1 contains indicators associated with recent token jacking activity.


**Indicator** **Context**


Go-http-client/2.0,gzip(gfe)


User Agent associated with malicious API calls


3.235.109\[.\]125


Malicious API calls


116.105.166\[.\]148


Malicious API calls


172.96.142\[.\]186


Malicious API calls


38.46.219\[.\]166


Malicious API calls


38.46.219\[.\]163


Malicious API calls


38.46.219\[.\]162


Malicious API calls


23.237.196\[.\]170


Malicious API calls


15.204.106\[.\]173


Malicious API calls


104.243.42\[.\]117


Malicious API calls


198.255.70\[.\]210


Malicious API calls


47.88.103\[.\]81


Malicious API calls


47.251.72\[.\]239


Malicious API calls


117.72.74\[.\]48


Malicious login (Credential Theft)


207.246.106\[.\]162


Malicious login (Credential Theft)


23.236.182\[.\]215


Malicious login (Credential Theft)


95.214.112\[.\]26


Malicious login (Credential Theft)


amutes\[.\]com


Transfer station infrastructure


abb1\[.\]life


Transfer station infrastructure


Table 1. Indicators of token jacking activity.


## Additional Resources


- [How Chinese Sell “Claude” Tokens at 5% Cost While Making Millions (Tutorial)](https://x.com/HarshalsinghCN/status/2056626175959826692?s=20) – X
- [The npm Threat Landscape: Attack Surface and Mitigations](https://unit42.paloaltonetworks.com/monitoring-npm-supply-chain-attacks/) - Palo Alto Networks Unit 42
