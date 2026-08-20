---
schema_version: "1.0.0"
document_id: "b212cd8f85afc226f5eced41766baed6f0294799b0987374663c00f716e19132"
company_key: "cerence-inc-common-stock"
company: "Cerence Inc."
source_id: "cerence-inc-common-stock-news-import-959e9bebf714"
canonical_url: "https://www.cerence.com/newsroom/blog/in-car-ai-security-zero-trust-cerence-microsoft"
published_at: "2026-07-07T00:00:00+00:00"
first_seen_at: "2026-07-21T13:07:02.447709+00:00"
fetched_at: "2026-07-28T21:22:12.115321+00:00"
content_hash: "sha256:b8f08ba2d92a2982b876daef2b4068461814edf0276603fe6e226adfd03a2eeb"
---

# In-Car AI Done Right

Today’s in-car AI assistants – highly autonomous[agentic AI systems](https://www.cerence.com/products/ai-agents) that perform tasks on behalf of drivers and passengers and orchestrate other AI agents’ functions – promise significant gains in productivity, safety, and convenience on the road. At the same time, they bring unprecedented security challenges.


[Voice-activated](https://www.cerence.com/products/speech-input-output) , enterprise-linked agents are likely to become prime targets for cyberattacks, especially as threat actors increasingly harness AI tools to craft convincing phishing lures, sophisticated malware, or attempts to “jailbreak” AI models. This is leading regulators (from the United Nations’ UNECE WP.29 vehicle cybersecurity regulations to the forthcoming EU Cyber Resilience Act) to raise the bar for security-by-design, security by default, secure in operations, and data governance in connected cars.


At the same time, as assistants begin helping drivers stay on top of work and personal tasks during their commute, their role is evolving.


Until recently, in-vehicle voice assistants were largely generic, supporting basic tasks without being tied to a specific person.


That is changing. As these systems connect to calendars, email, and other personal or work data, they begin to act on behalf of the individual in the driver’s seat, reviewing messages, suggesting actions, and in some cases carrying them out.


These interactions are more personal, and the data involved is often sensitive. Cars are not personal devices – they are frequently shared, which raises important questions about identity, permission, and control.


With these converging trends in mind, automotive OEMs, CISOs, and IT leaders must adopt a security-first architecture for in-vehicle AI – one that goes beyond LLM guardrails. The key is integrating proven Zero Trust Architecture (ZTA) and Defense in Depth principles throughout the system, while leveraging a[platform-based security approach](https://www.cerence.com/platforms/cerence-xui) enriched by global threat intelligence.


This comprehensive strategy is exemplified by[Microsoft and Cerence AI’s partnership](https://investors.cerence.com/news-events/press-releases/detail/351/cerence-ai-develops-mobile-work-ai-agent-integrating-microsoft-365-copilot-into-cars-to-enable-safer-smarter-work-on-the-go) in developing Cerence’s[Mobile Work Agent](https://www.cerence.com/products/ai-agents#mobile-work) a voice-activated in-car productivity assistant that balances automotive AI functionality with robust protection of corporate and personal data and driver safety. It enables drivers to safely review their calendar, coordinate work tasks, and stay on top of their jobs during their commutes.


**Why a Holistic Strategy Matters for Automotive**
The stakes for security and safety in vehicles are uniquely high. Automakers operate in a regulated environment where a cyberattack can have life-and-death implications, along with serious legal and reputational consequences. By adopting a comprehensive,[platform-based security strategy customized for automotive cybersecurity,](https://www.cerence.com/platforms/cerence-xui) OEMs can align with automotive cybersecurity standards (e.g. ISO/SAE 21434, TISAX) and meet regulatory compliance for secure-by-design, secure-by-default, and secure in operations via 24/7 monitoring and software updates (as mandated by UNECE WP.29 and CRA).


It’s not enough to treat the vehicle assistant as an isolated feature; it should be managed as an extension of an enterprise’s digital ecosystem, subject to the same rigorous security principles and oversight as any device or application that accesses corporate networks or data, such as a laptop or smart phone. This unified approach ensures that protections extend from in-cabin software all the way to cloud services, plugging any potential gaps between the car and the enterprise network.


**Zero Trust and Defense-in-Depth: A Security-First Foundation**
A robust security posture for an[automotive AI assistant](https://www.cerence.com/industries/automotive) starts with[Zero Trust](https://www.microsoft.com/en-us/security/business/zero-trust) – *“never trust and always verify, least privilege access, assume breach.”* No person, device, or component is inherently trusted, even if it’s inside the car or corporate network. Every interaction involving the in-vehicle assistant must be continuously authenticated, authorized, and logged.


In practice, this means strong identity and application security enforcement at every step. The Mobile Work Agent uses[Microsoft Entra ID](https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id) (formerly Azure Active Directory) for secure user and agent authentication and[Microsoft Intune](https://www.microsoft.com/en-us/security/business/microsoft-intune) to ensure the vehicle’s infotainment system is registered, and that the application is compliant with enterprise IT requirements. Multi-factor authentication, mutual Transport Layer Security (mTLS), and short-lived OAuth tokens further guarantee that only legitimate, policy-compliant users and devices can access the assistant’s services.


Zero Trust is implemented in tandem with Defense-in-Depth – multiple overlapping layers of security, so that if one mechanism fails, others still stand. Each stage between the car, the cloud backend, and the remote AI service has dedicated controls.


The Mobile Work Agent’s architecture spans numerous layers – from identity and device trust to API and network access controls, to content policy enforcement and continuous monitoring. With layered safeguards, a breach of any single component will not compromise the entire system.


**The Power of a Platform-Based Approach**
Implementing all these layers and keeping them updated is complex. That’s why the Mobile Work Agent leverages Microsoft’s integrated security platform, which unifies identity, endpoint, data, and cloud protection. This platform approach reduces gaps between disparate tools and enables dynamic defenses. For example, if a user’s account is compromised or a car is reported stolen, the platform can automatically revoke the AI assistant’s access or even remotely disable it via Intune – an instantaneous response that would be far harder to coordinate with siloed security products.


Crucially, Microsoft’s platform provides unmatched Threat Intelligence (TI). The company’s security TI platform processes more than 100 trillion threat signals each day, yielding a unique global view of emerging threats. That insight helps Microsoft’s defenses detect and block novel attacks far faster than any individual organization could manage alone. By building on a provider with this massive intelligence and automated protection, automotive enterprises inherit a level of situational awareness and threat response that would be nearly impossible to achieve independently.


**Beyond Guardrails: Securing AI Agents at Every Layer**
Early efforts to secure LLM-based assistants often focus on in-model “guardrails,” like carefully crafted system prompts and content filters to prevent disallowed outputs. While these are important, they address only one part of the risk. If the surrounding systems and processes aren’t secure, attackers can simply target the weaker links – stealing credentials, compromising the vehicle’s software, or misusing the assistant itself to access sensitive data. Effective security for agentic AI must augment model-level guardrails with traditional cybersecurity controls.


The Mobile Work Agent uses deterministic enterprise policies and data protections to constrain what the assistant can access or share. For example, if a user asks the assistant to summarize a document labeled “Highly Confidential,” the backend – leveraging[Microsoft Purview Information Protection](https://learn.microsoft.com/en-us/purview/information-protection) – will detect the sensitivity label and block the request rather than risk oversharing restricted information. Likewise, if a policy check can’t be completed (for instance, if the car is offline and can’t verify a data policy), the default is to fail safe and refuse the operation. In short, the AI assistant runs with a tightly limited identity and scope (least privilege), and critical decisions about access and data handling are made by robust external policies, not left to the AI’s own reasoning.


**Securing the AI Engine with Azure Foundry**
A key pillar of the Mobile Work Agent’s design is[Microsoft AI Foundry](https://azure.microsoft.com/en-us/products/ai-foundry/models/openai) – a dedicated, enterprise-grade environment where the agent’s generative AI model runs. The AI Foundry eliminates a major concern for businesses by ensuring no user prompts or outputs are stored or used to train the model, and by providing built-in content filtering and monitoring to detect unsafe outputs. It delivers the power of large GPT-based models with strong data privacy and policy controls baked in.


Importantly, the in-car agent invokes the AI model only through a tightly controlled interface – not via any open internet endpoint – keeping interactions isolated within the enterprise’s own cloud environment. This architecture gives organizations confidence that their data stays contained, and the AI’s responses remain within expected ethical and security boundaries.


**The Microsoft–Cerence AI Advantage**
No single company can address all these facets alone. The partnership between Microsoft and[Cerence AI](https://www.cerence.com/) demonstrates how combining complementary strengths leads to a stronger solution. Microsoft contributes a deeply integrated cloud and security platform – spanning identity, device management, threat detection, and AI infrastructure – continually refined by global threat intelligence and best practices.


Cerence AI provides deep expertise in automotive software, voice AI, and in-vehicle integration and compliance, ensuring that[all solutions meet the specialized requirements](https://www.cerence.com/services/professional-services#seamless-integration-support) of cars without compromising the user experience.


Working together, Cerence AI and Microsoft co-developed a best-in-class implementation of in-car agentic AI that preserves security and privacy without compromising functionality. With Cerence AI’s automotive expertise and Microsoft's established identity, data protection, and compliance capabilities, OEMs can accelerate deployment and reduce integration complexity, while leveraging security controls and governance frameworks that enterprises already trust.


When in-car AI is built on a secure, enterprise-grade foundation, it becomes a trusted companion, handling real, security-sensitive tasks without hesitation or risk. Drivers can interact confidently, knowing their data, identity, and actions are protected even in a shared vehicle environment. That trust is largely invisible, showing up as an experience that simply works, respects boundaries, and behaves predictably. Ultimately, security is what unlocks the full promise of in-car AI, enabling more intelligent, personalized, and safe interactions between people and their vehicles.
