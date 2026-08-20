---
schema_version: "1.0.0"
document_id: "939294cde58014210d1e91af0b1568747483d5a82c56a68dce239490741eace1"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/threat-intelligence/free_tokens_for_sale/"
published_at: "2026-08-04T07:00:00+00:00"
first_seen_at: "2026-08-04T17:22:20.873737+00:00"
fetched_at: "2026-08-04T17:31:37.256081+00:00"
content_hash: "sha256:1dea43ea95706f18b6f0ef6bf92c1b1d527d624904d6f31f23c7c71e656a8274"
---

# Free tokens for sale: How fake signups drive AI fraud

## Introduction


As AI models have become vastly more capable, these multifunctional tools are being used for a wide range of tasks—from coding and analysis to software testing, research, and vulnerability hunting.


This has given rise to expansive gray and often illegal market offerings for accounts with AI providers. The demand is driven by both cost and restrictions on access. It is being satisfied in a variety of ways, one of which is through fraudulent account registrations, often capitalizing on free trials or credits.


This post will explore how free and discounted AI services are being abused with a view to how fraudulent signups can be controlled in Okta and Auth0 while imposing low friction for legitimate new signups.


## Why use the gray market?


There are several reasons why users seek out gray market or illegal services offering AI model access: cost, access, and some degree of anonymity.


**Cost** : Providers offer discounts between 70-90% off subscription prices. Offerings may be for subscription-based accounts, a certain number of tokens, or a certain number of requests (prompts). With those packages, the allotted number of requests can use an unlimited number of tokens.


**Access:** Users in China often cannot get direct access to U.S. frontier AI models. Frontier U.S. models are either[banned or blocked](https://www.chinatalk.media/p/the-grey-market-for-american-llms) by China, which has a number of[AI regulations](https://en.wikipedia.org/wiki/Interim_Measures_for_the_Management_of_Generative_AI_Services) , or not offered by providers due to national security and distillation concerns.


In September 2025, Anthropic took aim at the burgeoning gray market by[restricting](https://www.anthropic.com/news/updating-restrictions-of-sales-to-unsupported-regions) subsidiaries offering services to unsupported regions like China. Nonetheless, the market is thriving and fraudulent registration is one of the reasons. The Chinese-language offerings, which are on messaging platforms, underground forums and indexed in GitHub repos like this[detailed one](https://github.com/howardpen9/awesome-ai-api-proxy) , appear to outnumber and scale much larger than the smaller-time, English-language cybercriminal offerings.


The ChinaTalk blog[investigated](https://www.chinatalk.media/p/how-to-buy-cheap-claude-tokens-in) these Chinese-language gray-market API services, which are referred to as “transfer” or “relay” stations and are advertised on messaging services such as Taobao and Telegram. Vendors have developed sophisticated operations using AI gateways and online uptime monitoring services.


Legitimate AI model service providers are trying to counter fraudulent registration. In April 2026, Anthropic[said](https://support.claude.com/en/articles/14328960-identity-verification-on-claude) it will use Persona’s ID verification system to verify some new accounts, which involves providing a government-issued ID and a live selfie. To track the proliferation of proxy services catering the Chinese market, Anthropic developed a fingerprinting system to detect account abuse in Asian time zones, although it will be[removed](https://x.com/trq212/status/2072079729331777817) as it said it had developed detection methods.


**Performance:** Sophisticated offerings may use AI API gateways such as LiteLLM or OpenRouter to link to a variety of models from different providers, which helps minimize disruptions if accounts are shut down due to abuse.


**Operational security:** If the AI model proxy service is not forwarding the true origin IP, it acts as a way for illicit customers to hide their identity. If the illicit provider is proxying with sophistication, the customer may be able to evade detection.


**Payment privacy:** These services accept cryptocurrency, which may offer a higher degree of privacy than other payment options. Many providers do not require personal information to create accounts.


**Customer service:** Illicit AI vendors may provide customer support if an account is shut down or service is disrupted.


However, there are disadvantages:


**Operational security:** While the gray market offers some operational security over direct purchases, it cuts both ways. When services are configured as a gateway proxy, the service provider has full visibility into prompts, as those prompts must be forwarded to a model. This is a privacy concern, as the service provider could accidentally leak or sell data.


**Disruption:** Model providers may unexpectedly cut off accounts as controls to prevent fraudulent accounts are tightened, disrupting complex workflows.


**Bait and switch:** Service providers may advertise access to a frontier-model but deliver a less expensive and less capable model, which may not be evident at purchase.


**Prompts for cash:** Gray market service providers have an incentive to act as an adversary in the middle. Even if it were possible to forward input and output with zero knowledge, a gray market service provider would lose the ability to gather telemetry on their users and the ability to distill frontier models. This source of prompts is cited by ChinaTalk as a key source of additional revenue.


## Cheaper tokens


Okta Threat Intelligence found more than a half-dozen services advertisements via underground forums and messaging platforms, which represents a fraction of these services. One such site is Poison Claude:


One site, poison-claude.bitsender.top, offers “unlimited” tokens in plans metered by the number of prompts. It also offers a la carte token packages.


Fill out the form to access this content.


Advertisements for Poison Claude explain how the service can offer the cheap tokens: by taking advantage of free bonus credits, such as the US$100 bonus credit on AWS for Bedrock accounts. The service plainly states on its website that: “We add those accounts to our pool, your request is routed to a specific account under the hood (you don’t see this) and you get charged 5-15% of the official per-token price depending on the model.”


The flat-fee monthly plans available on Poison Claude are possible due to “the gap between what the upstream providers charge us under bonus credit and what we charge you.” The models offered are Opus 4.8, Opus 4.7, Opus 4.6, and Sonnet 4.6.


Poison Claude claims it can offer significantly cheaper access to Anthropic’s models due to “bonus credits” that come from buying accounts.


Fill out the form to access this content.


As of July 3, 2026, Amazon offered US$100 in AWS credits that could be applied to Bedrock and other AI services, with another $100 available based on completing certain account activity. It’s probable that access to Bedrock has been enabled through fraudulent registration.


Poison Claude accepts payment in cryptocurrencies including USD Tether, USD Coin, ethereum, litecoin, and bitcoin cryptocurrency. Once paid, a customer is provisioned an API key for an Anthropic-compatible API. Users are then instructed to set these environment variables so that their installation of Claude Code uses the Poison Claude API rather than the legitimate one.


Poison Claude’s instructions for setting environment variables for its service.


Fill out the form to access this content.


Prompts are passed from Poison Claude’s API to Anthropic, with the answers returned to the customer.


How popular is Poison Claude? A configuration mistake provides a clue. Poison Claude’s operators exposed an API route. Running a GET command on /api/status showed the number of total and active users:


--> HTTP 200: {status: operational, total_users: 881, active_users: 872}


The main domain for Poison Claude, poison-claude.bitsender\[.\]top was hosted behind Cloudflare’s CDN, obscuring its originating IP address. However, the site’s operators forgot to include the endpoint, api.claudeopus\[.\]shop, which was hosted on Hostinger in Mumbai (the exposure has been fixed).


## Startup credit


Ecomagent.in is another service offering unlimited tokens via its own endpoint for subscriptions below market price. On offer is access to Opus 4.8, Opus 4.6, Sonnet 4.6, and GPT Codex 5.5 via its custom API endpoint.


Ecomagent shows on its website an example cURL request to its own API with the phrase “Hello” and then the expected output as a .json object that contains the model’s response: “"Hello! How are you doing today? Is there something I can help you with?"


The response contained metadata about the prompt and token usage. The output contains a field labeled id: "msg_vrtx_01XNdd65BAkT82dLgksZGWD3", which is indicative of a response coming from Google’s Vertex platform, now called the Gemini Agent Enterprise Platform.


Like Amazon, Google Cloud offers credit for new accounts, although users must register a billing payment card. Google also[offers](https://web.archive.org/web/20260702222220/https://cloud.google.com/startup/ai) up to $350,000 in credits to AI startups that plan to use the Gemini Enterprise Agent Platform or its Gemini model. A test of Ecomagent showed no responses that contained an id field with msg_vrtx.


Ecomagent’s documentation indicates it used Google’s Vertex AI deployment platform to offer lower-cost AI services to Anthropic.


Fill out the form to access this content.


Ecomagent lists a Telegram username for support on the website. Ecomagent claims to be able to offer discounted Anthropic models due to its use of startup credit. Like many AI companies, Anthropic also offers free credits to startups, ranging from between US $25,000 up to $100,000. Startups must apply for these credits, and presumably, this usage would violate the terms and conditions.


Like Poison Claude, Ecomagent also exposed an unauthenticated API route showing the “total_users” and “active_users” of the service, both numbering under 1,000.


The results of a cURL request via an unauthenticated API route.


Fill out the form to access this content.


## Case study: AI video service


This year has been the gold rush for AI, with startups and established companies fiercely competing for business. Many AI services offer free trials or free access.


Okta Threat Intelligence recently observed an AI video company with a high rate of brute-force fraudulent signups that likely came from bots. This vendor offers free trial access to its service.


Between June 1, 2023, and July 1, 2026, logs show over 105,000 brute force attempts to sign up from 251 distinct IPs likely connected to bots.


Fraudulent registrations to an AI service that offers free trials were tied to proxy providers and VPNs on networks in Lebanon, Indonesia and Thailand.


Fill out the form to access this content.


Based on our data, we can say it is highly probable that China-based users are circumventing regional restrictions. Some of the top VPN providers, like QuickQ VPN, are commonly used by Chinese internet users, and the top email domain was[qq.com](http://qq.com/) , which is a popular email service in China.


That aside, the free trial enticed opportunists globally. As seen on the right-hand side of the graph below, the overall signup list contains disposable domains like dakaka.org, emailinbo.live, and ratixq.com, pointing to highly probable synthetic identity creation at scale.


The ​​overall signup list contains disposable domains like dakaka.org, emailinbo.live, and ratixq.com, pointing to highly probable synthetic identity creation at scale.


Fill out the form to access this content.


There are also indications of scammers getting in on the free trial action. Access to this particular service was advertised on cybercriminal forums. One seller contacted via Telegram claimed to have both API keys and credentials, with the credential offering allowing “full access” to the service.


A second vendor monetized fraudulent sign-ups with the same service. For US$40 in the cryptocurrency Tether, the vendor offered a “created” account. The vendor claimed this was better than a hacked account—one in which a legitimate user’s credentials had been stolen—since a hacked account was more likely to get shut down.


The seller then provided a username and a password, claiming that once the trial period was over the account would switch to a “paid membership.” In fact, this was a ruse. The vendor, likely based in Turkey, had a history of defrauding customers and was banned from the notorious BreachForums cybercriminal forum.


## How Okta Threat Intelligence is disrupting them


Prior to publication, Okta Threat Intelligence notified Cloudflare and Anthropic of the infrastructure and abuse patterns documented in this post. Cloudflare was alerted to Poison Claude's use of its CDN to obscure the service's origin, and Cloudflare subsequently placed a phishing warning in front of the site.


As of July 16, 2026, Cloudflare has declined to take action on claudeopus\[.\]shop, which uses Cloudflare as a CDN and Cloudflare Turnstile for bot protection. We also notified Amazon Web Services and Google Cloud of the apparent abuse of their startup credit programs by Ecomagent and similar services.


The relevant domains and API endpoints have been archived and submitted to abuse reporting channels. We continue to notify AI providers of signup fraud attacks against their Auth0 tenants and Okta organizations. We are sharing these findings publicly to raise awareness for CIAM administrators and AI providers alike, and will continue to monitor for new fraudulent services as the gray market for AI access matures.


## Conclusion


Bot activity is rising across the internet, particularly with the increasing deployments of AI agents. Those running bot networks also have more choice than ever with which to counter bot detection methods, such as residential proxies. Residential proxies allow malicious traffic to come from benign consumer IP connections with often little or no history of malicious activity, making it risky to block.


While a certain amount of fraudulent registration is inevitable and the downside of offering free trials, it can be countered. In the recommendations section are tools and sources for Okta and Auth0 CIAM controls that allow administrators to strike a sensible balance with signups.


## Countering signup fraud


Attackers create fake accounts for a range of reasons. As illustrated before, this includes exploiting new-user promotions, but there are many others: aging accounts to appear legitimate, overwhelming systems to block real users, fraudulent social engineering schemes, enumerating valid usernames as a stepping stone to credential stuffing or phishing and spam.


Anything short of adopting more secure factors on signup is insufficient. Auth0 and Okta's native passkey support raises the cost of account creation to a level automated attackers cannot absorb: a hardware-bound, phishing-resistant credential that cannot be farmed, shared, or bulk-provisioned. Okta Threat Intelligence has not yet observed passkeys used in any signup fraud campaign we track. AI providers should treat passkey-required signup not as a UX trade-off but as a fraud control: it will not stop a determined human from creating an account, but it makes the industrialized bot abuse feeding services like Poison Claude economically untenable.


Static API keys are the currency of the gray market. Poison Claude's business model is built on aggregating and reselling them through their centralized provider. The appeal of static keys is convenience, but on modern platforms that convenience gap has largely closed.


Auth0 and Okta’s application model replaces a static key with the OAuth 2.0 client credentials flow: the machine exchanges a client ID and secret for a short-lived access token, scoped to the permissions it needs. A leaked token expires on a configurable, short schedule by default. A leaked static key does not. Providers should issue no credential with an indefinite lifetime: any token that can be copied into a file and shared is eventually a gray-market product.


Playbooks for countering signup fraud are available for[Auth0](https://auth0.com/docs/secure/attack-protection/playbooks/signup-attack-playbook) . Okta Customer Identity administrators should implement controls like[bot protection](https://help.okta.com/oie/en-us/content/topics/itp/bot-protection.htm) ,[progressive profiling and registration policies](https://help.okta.com/oie/en-us/content/topics/identity-engine/policies/about-ssr.htm) ,[Enhanced Dynamic Network Zones,](https://help.okta.com/en-us/content/topics/security/network/about-enhanced-dynamic-zones.htm) and[Identity Threat Protection](https://www.okta.com/blog/product-innovation/identity-threat-protection-oci/) (ITP) to prevent fraudulent signups.


About the Author


[Jeremy Kirk Director, Okta Threat Intelligence Jeremy Kirk is a director based in Sydney with Okta Threat Intelligence. He is part of a team that produces actionable intelligence and advisories on emerging cyber threats with a focus on issues such as account takeover, social engineering, phishing, and AI security. He previously worked on the intelligence analysis team at Intel 471, a cyber threat intelligence firm, writing about and analyzing the evolving cybercriminal underground.](https://www.okta.com/blog/author/jeremy-kirk/)


[Mathew Woodyard Director, Okta Threat Intelligence Mathew Woodyard translates thoughtful analysis of emerging threats into business impact and product improvements. He provides strategic guidance for CIAM leaders based on deep insights into attacker behavior. His work, featured in ACM/IEEE publications, the Verizon DBIR, and Bad Packets by Okta, directly enhances Auth0's attack protection features and informs customer-facing threat response playbooks.](https://www.okta.com/blog/author/mathew-woodyard/)
