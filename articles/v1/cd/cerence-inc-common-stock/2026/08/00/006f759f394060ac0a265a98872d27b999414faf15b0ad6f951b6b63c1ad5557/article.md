---
schema_version: "1.0.0"
document_id: "006f759f394060ac0a265a98872d27b999414faf15b0ad6f951b6b63c1ad5557"
company_key: "cerence-inc-common-stock"
company: "Cerence Inc."
source_id: "cerence-inc-common-stock-news-import-959e9bebf714"
canonical_url: "https://www.cerence.com/newsroom/blog/zero-trust-double-agents-weaponized-ai"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-05T14:18:08.598047+00:00"
fetched_at: "2026-08-05T14:18:09.762781+00:00"
content_hash: "sha256:c388a8dd62e8b3c57ef7ecdaf30f8901ce40edb5cd591ab335efed0f7e9c9fe9"
---

# Zero Trust for Automotive Agentic AI in the Age of Double Agents and Weaponized AI

At Cerence AI, our work with Microsoft on in-car AI—particularly through the development of our[Mobile Work Agent](https://www.cerence.com/products/ai-agents) (MWA)—has sparked an important debate. Following our[joint blog on the promise and the perils of in-car AI](https://www.cerence.com/newsroom/blog/in-car-ai-security-zero-trust-cerence-microsoft) , we received a wide range of responses. Many recognized the value of the Zero Trust Architecture and defense-in-depth architecture underpinning our Mobile Work Agent, while others questioned whether such rigor is necessary, suggesting that model guardrails alone should be sufficient.


That debate reflects a broader inflection point for the industry. As “double agents” and weaponized AI become part of the real threat landscape, the questions organizations must ask are changing. Not just: *Can the model refuse a bad prompt?* But: *Who is the agent? What can be accessed by the agent and for how long? Which actions are explicitly allowed? How is sensitive data governed? What happens when policy cannot be verified? Where is behavior monitored and contained? What about compliance when actions are executed by an agent?*


If the first wave of AI security was about *guardrails* , the next wave must be about *architecture* . As AI systems evolve from answering questions to executing tasks—calling APIs, retrieving enterprise data, and coordinating workflows—the security problem changes fundamentally.[Microsoft’s Zero Trust](https://www.microsoft.com/en-us/security/business/zero-trust) for AI guidance makes this explicit:[autonomous agents](https://www.cerence.com/products/ai-agents) introduce new trust boundaries and new risks, where agents themselves can become overprivileged, manipulated, or misaligned “double agents.”


For Cerence AI, this shift is not theoretical. In building systems like the enterprise-connected Mobile Work Agent, we see firsthand how quickly risks like “shadow AI,” silent data leakage, and unintended actions emerge when systems are not explicitly governed end-to-end.


Guardrails still play an important role but they operate at the *language layer* , and that is their limitation. They can be bypassed through jailbreaks, manipulated through indirect prompt injection, or outpaced by multi-step workflows where a benign request leads to a risky downstream action.


Once agents invoke tools, retain memory, or interact across services, the attack surface expands significantly. In systems like MWA, this is especially relevant—because the agent is not just generating responses; it is interacting with enterprise systems on behalf of the user. Guardrails may shape what the model says, but they do not govern what the system can do.


From our perspective at Cerence AI, secure agentic AI—especially in[automotive](https://www.cerence.com/industries/automotive) —must be architected as a governed system, not just a moderated model. The Zero Trust framework provides a strong foundation for this approach. It extends familiar principles—verify explicitly, apply least privilege, assume breach—across the full AI lifecycle. Its guidance for autonomous agents adds critical detail: define clear boundaries, enforce deterministic controls, require approval for high-risk actions, assign unique identities, and treat all inputs as untrusted by default.


We break this into four *layers* —model, safety system, application, and positioning. Within that framework, *the application layer is decisive* — because that is where systems like the Mobile Work Agent determine what an agent can access, what actions it can take, and how policies are enforced.


For OEMs and enterprise architects, the key question is no longer whether the assistant has guardrails. It is whether the entire system—like the architecture behind the Mobile Work Agent—is designed to verify, constrain, and monitor every meaningful action.


The Mobile Work Agent provides a concrete example of how this can be done.


From the ground up, we built the agent on Zero Trust and defense-in-depth principles. No request is trusted simply because it originates from the vehicle. User identity is anchored in[Microsoft Entra ID](https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id) , application posture is governed through[Intune](https://www.microsoft.com/en-us/security/business/microsoft-intune) , and every interaction is secured using mutual TLS and short-lived OAuth tokens.


Requests do not flow directly to the model. Instead, they pass through a Cerence AI-controlled mediation layer and an API Management (APIM) gateway before reaching[Azure AI Foundry](https://azure.microsoft.com/en-us/products/ai-foundry) . This ensures that every request is authenticated, authorized, and policy-checked before any model interaction occurs. In this architecture, the LLM is not the system; it is a controlled component within a governed system.


Equally important is how MWA handles data.


Data governance is treated as a first-class control. Enterprise content is mediated before inference, and sensitive information can be blocked using[Microsoft Purview](https://www.microsoft.com/en-us/security/business/microsoft-purview) sensitivity labels and DLP signals. If policy cannot be verified—whether due to unknown classification, connectivity issues, or offline scenarios—the system is designed to fail closed rather than take risks.


Cerence AI’s MWA also enforces strict data minimization. Only the minimum necessary data is sent to the model, and Retrieval-Augmented Generation (RAG) ensures responses are grounded in enterprise data rather than open-ended generation. This reduces hallucination risk while limiting unnecessary exposure to sensitive information.


These practical implementations of application-layer control are critical for making agentic AI systems safe, auditable, and enterprise ready.


These measures don’t diminish the importance of guardrails. To transform an AI system from one that is moderated to one that is governed requires *guardrails* *within Zero Trust* —supported by strong identity, least privilege, mediation, deterministic policy enforcement, network isolation, monitoring, and incident response.


In that sense, the bar set by Zero Trust is an enabler. It is what allows agentic systems like the Mobile Work Agent to safely bring enterprise AI into environments as complex and sensitive as the car.


And ultimately, it is what determines whether agentic AI systems are not only powerful—but trusted.


At[Cerence AI](https://www.cerence.com/) we have been providing secure, private, and compliant voice AI technology in regulated industries such as automotive for over 25 years, and the transition to agentic voice AI means extending our leadership aligned with the new threat surface. If you are evaluating agentic voice AI with enterprise-grade security and privacy, contact us to see how our Mobile Work Agent brings that architecture to life.
