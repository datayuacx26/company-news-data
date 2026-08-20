---
schema_version: "1.0.0"
document_id: "92db1171cf80bbd03b38cf03b28942d6a87daef32ce2c9d51c2bc8f1539bdd17"
company_key: "palo-alto-networks-inc-common-stock"
company: "Palo Alto Networks Inc."
source_id: "palo-alto-networks-inc-common-stock-rss-e942a00c1738"
canonical_url: "https://www.paloaltonetworks.com/blog/2026/08/prisma-airs-unified-data-protection-for-claude/"
published_at: "2026-08-05T16:35:03+00:00"
first_seen_at: "2026-08-05T17:58:33.033362+00:00"
fetched_at: "2026-08-05T17:58:34.735150+00:00"
content_hash: "sha256:b5ac21276ee817dcf0c3f09c3ec493cf8d79e19c8d4e7eee74e0de711775ce84"
---

# Prisma AIRS - Unified Data Protection for Claude

[AI Security](https://www.paloaltonetworks.com/blog/category/ai-security/)


[Products and Services](https://www.paloaltonetworks.com/blog/category/products-and-services/)


As AI adoption shifts from experimental tools to the business core, enterprises are deploying AI agents and assistants across every department. Developers are accelerating software delivery with


**Claude Code and** business functions are collaborating in


**Claude.ai** ,


**Claude** **Cowork** , and


**Claude** **Design** , using Anthropic's Claude to fundamentally rewire how they operate and drive complex, autonomous workflows across their most critical tasks.


Broad AI adoption has forced enterprise security into a difficult paradox: secure the business without stalling innovation. Today, security teams must answer three critical questions: How do we prevent sensitive data from leaking into AI models? How do we intercept runtime attacks, like prompt injection, before they execute? And how do we enforce these guardrails consistently across every AI interaction, in real-time?


We are thrilled to announce that


[Palo Alto Networks Prisma AIRS API](https://www.paloaltonetworks.com/ai-security/prisma-airs) **now directly integrates with Claude Enterprise via inference hooks** .


**Key Takeaways:**


- **Synchronous Governance at the Speed of AI:** Enforce critical runtime policy guardrails instantly before model inference occurs. By evaluating every Claude interaction in real-time, security teams can confidently enable high-speed AI innovation without introducing unacceptable risk or operational bottlenecks.


- **Extending Proven DLP to AI Workflows:** Safeguard PII, intellectual property, and proprietary code across all Claude environments (Claude Code, Claude.ai, Claude Cowork) seamlessly. By directly leveraging your organization’s existing enterprise DLP policies, you get consistent, unified data protection without the friction of managing disparate rule sets.


- **Neutralizing AI-Specific Threats Proactively:** Deploy enterprise AI with confidence by intercepting malicious activity before it executes. Proactively block sophisticated runtime threats, including prompt injections, jailbreaks, and toxic inputs, enabling your AI workflows to remain secure and your business operations uninterrupted.


**How It Works**


Setting up zero-trust governance for your Claude Enterprise environment takes just a few clicks:


- **Configure Claude Enterprise** : In your Claude Enterprise organization settings, navigate to inference hooks, set the hook endpoint URL to your designated Prisma AIRS webhook URL, and include your custom


**Prisma AIRS API key** in the custom headers (x-pan-token).


- **Activate Cryptographic Verification in Prisma AIRS** : Claude generates a unique, one-time signing secret. You simply drop this signing secret into the Prisma AIRS UI.


Prisma AIRS securely stores the signing secret in an encrypted edge Key-Value Map (KVM) to cryptographically verify the authenticity of every incoming request before processing.


**Unified Governance Across All Claude Surfaces**


Before Claude reads a prompt, Claude POSTs the payload to Prisma AIRS, which evaluates the input against the configured security profile and returns a synchronous allow or deny verdict back to Claude.


**Removing the Data Inspection Blind Spot**


This Prisma AIRS integration natively leverages


[the Palo Alto Networks Enterprise DLP](https://www.paloaltonetworks.com/sase/enterprise-data-loss-prevention) engine your organization already runs. Your current policies, custom definitions of "confidential," and compliance dashboards now instantly govern Claude. No redundant rule creation, and no new management overhead. Every prompt is classified in real-time, before anything reaches the model. Data Policy updates reach Claude the moment they reach everything else, and Claude events land in the same dashboards and audit reporting as every other channel. AI stops being the exception to your data protection strategy and becomes just another channel that is covered.


In practice, this helps prevent:


- **Accidental exposure of PII and regulated data:** a Social Security number, card number, or patient record pasted into a prompt is detected on the spot by advanced RegEx patterns and ML classifiers, with Exact Data Matching (EDM) recognizing your actual customer records. When policy requires, Prisma AIRS enforces a deny policy to help keep Claude usage stays aligned with GDPR, HIPAA, and PCI-DSS requirements.


- **Misuse of credentials in code:** Developers working in Claude Code can't inadvertently submit hardcoded API keys, private tokens, or environment credentials into prompt context.


- **Disclosure of confidential business matters.** Document classifiers flag prompts touching M&A, legal, or HR material — even when they contain no identifier a pattern could match.


**AI Safety & Runtime Threat Protection**


Simultaneously, Prisma AIRS inspects payloads for operational risks specific to large language models:


- **Prompt Injection & Jailbreak Attacks:** Intercepts malicious attempts to manipulate model context or bypass system instructions.


- **Malicious Code & Malicious URLs:** Prevents execution or processing of untrusted scripts and risky links embedded within prompts.


- **Toxic Content & Topic Guardrails:** Enforces compliance with corporate policies and custom business topics.


If a prompt violates your security policy, Prisma AIRS issues a deny verdict. Claude immediately blocks the prompt, optionally presenting a user-facing explanation while returning a unique audit correlation code to your security telemetry dashboards.


**Deploy Claude Without the Risk**


The Prisma AIRS integration with Claude transforms security from a deployment bottleneck into a business enabler. You no longer have to choose between the operational control of Palo Alto Networks and the cognitive velocity of Claude.


**Choose Your Path Forward:**


- [Attending Black Hat 2026?](https://register.paloaltonetworks.com/paloaltonetworks-blackhat-2026) Stop by the Palo Alto Networks booth to check out the Prisma AIRS live demo.


- **Ready to secure your Anthropic rollout?**[Contact Prisma AIRS AI Security Experts](https://www.paloaltonetworks.com/sase/prisma-airs-contact-us) to discuss your specific governance and deployment needs.


- **Ready to build?** Visit the


[Prisma AIRS Documentation](https://pan.dev/prisma-airs/api/airuntimesecurity/airuntimesecurityapi/) to connect your Claude inference hooks in minutes.


---


***Forward-Looking Statements***


*This blog contains forward-looking statements that involve risks, uncertainties and assumptions, including, without limitation, statements regarding the benefits, impact, or performance or potential benefits, impact or performance of our products and technologies or future products and technologies. These forward-looking statements are not guarantees of future performance, and there are a significant number of factors that could cause actual results to differ materially from statements made in this blog. We identify certain important risks and uncertainties that could affect our results and performance in our most recent Annual Report on Form 10-K, our most recent Quarterly Report on Form 10-Q, and our other filings with the U.S. Securities and Exchange Commission from time-to-time, each of which are available on our website at investors.paloaltonetworks.com and on the SEC's website at www.sec.gov. All forward-looking statements in this blog are based on information available to us as of the date hereof, and we do not assume any obligation to update the forward-looking statements provided to reflect events that occur or circumstances that exist after the date on which they were made.*
