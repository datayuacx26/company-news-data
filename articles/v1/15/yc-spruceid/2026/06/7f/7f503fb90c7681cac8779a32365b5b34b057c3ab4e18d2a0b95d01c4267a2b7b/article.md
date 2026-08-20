---
schema_version: "1.0.0"
document_id: "7f503fb90c7681cac8779a32365b5b34b057c3ab4e18d2a0b95d01c4267a2b7b"
company_key: "yc-spruceid"
company: "SpruceID"
source_id: "yc-spruceid-rss-1607485977f6"
canonical_url: "https://blog.spruceid.com/digitalidentity-what-is-a-presentation-request-and-who-controls-what-gets-asked-for/"
published_at: "2026-06-05T22:30:50+00:00"
first_seen_at: "2026-07-24T02:02:18.818185+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:1da907f3893263c899c1b47bba690632f03d21925879d04a2e37ae0c673589ba"
---

# What Is a Presentation Request, and Who Controls What Gets Asked For?

When we talk about privacy in verifiable digital credential systems, most of the conversation focuses on the issuer side: what data goes into a credential, how it is signed, and how it is stored. The verifier side of the equation gets less attention, and it deserves more. Specifically, the question of what a verifier is allowed to ask for, and who governs that, is one of the more consequential design choices in any verifiable digital credential program.


A presentation request is how that question starts. It is the structured message a verifier sends to a resident's wallet when they want to see a credential or verify an attribute. And the governance of that request is, in many ways, the other half of data minimization, because a credential designed to share only what is necessary still depends on the verifier asking for only what is necessary.


## How a Presentation Request Works


When a resident approaches a service that requires credential verification (for example, checking in at a government office, accessing a benefit portal, presenting age eligibility at a point of sale) the verifier's system sends a presentation request to the resident's wallet. That request specifies what the verifier wants to see: which credential type, which attributes, and in some systems, the stated purpose for the request.


The wallet receives the request, identifies which credentials the resident holds that could satisfy it, and presents the resident with a choice. The resident reviews what is being asked for and decides whether to share those selected fields. If they proceed, the wallet returns a presentation containing only the requested information.


That flow, when it works well, is a genuinely privacy-respecting interaction. The resident understands what is being asked for. The verifier receives only what they asked for. Nothing more is transmitted.


The governance question is what shapes whether that flow actually works that way in practice.


## Three Design Questions That Matter


Not all presentation request systems are designed equally. Three questions, in particular, determine how well a system protects resident privacy at the point of request.


**What is the scope of what a verifier can request?** In some systems, a verifier can ask for any attribute the credential contains. In others, the issuer designates which attributes may be requested for which purposes, and the wallet enforces that designation. The difference is significant: a system where verifiers can freely request any attribute creates the conditions for over-collection even if the issuer issued the credential thoughtfully. As explored in[What Is Data Minimization and Why Does It Matter for Government Services?](https://blog.spruceid.com/zerotrust-what-is-data-minimization-and-why-does-it-matter-for-government-services/) , data minimization is most durable when it is enforced at the architecture level — and that includes the request layer, not just the issuance layer.


**Does the verifier provide context for their request?** Some protocols support verifiers including a stated reason alongside their request, an approach that, where available, gives residents a clearer sense of what is being asked of them and why.


**Does the system enforce minimization on what is returned?** A well-designed system returns only the attributes that were requested, not the entire credential. A system where the wallet returns the full credential and leaves it to the verifier to ignore unreleased fields is relying on the verifier's behavior rather than the protocol's design. The distinction between those two approaches is meaningful.[Verifiers: Trust at the Point of Use](https://blog.spruceid.com/verifiers-trust-at-the-point-of-use/) addresses how verifier trustworthiness is established in a credential ecosystem, and why design-level constraints matter alongside policy-level commitments.


## The Verifier's Role


Conversations about privacy in credential systems tend to focus on the issuance side, what data goes into a credential, and how it is protected. The request layer is worth considering alongside it.


Even a well-designed credential depends, in part, on what verifiers ask for at the point of use. Systems that give agencies and program managers visibility into request scope (what verifiers can ask for, and how those constraints are enforced) tend to offer a more complete picture of how data flows through a program.


[Why Privacy-Preserving Design Matters in Public Services](https://blog.spruceid.com/why-privacy-preserving-design-matters-in-public-services/) explores what this looks like in practice for programs built around residents.


## Resident Control, More Fully Considered


Resident control is often discussed as the ability to choose what information to share when presenting a credential. That choice matters, but it is only part of the picture.


The system's structure determines which choices residents encounter in the first place. The design of a data request, and the rules governing what information can be requested, shape the resident experience long before a presentation takes place. When residents know that a service can request only specific, relevant attributes, they gain a clearer understanding of how their information will be used. That predictability is an important foundation for trust.


Building those guardrails into both policy and technical infrastructure is part of what it means to create a truly resident-centric identity system. If you are thinking through how these design choices apply to your program, get in touch.


Building digital services that scale take the right foundation.


[Talk to our team](https://spruceid.com/contact-us?ref=blog.spruceid.com)


**About SpruceID** : SpruceID builds digital trust infrastructure for government. We help states and cities modernize identity, security, and service delivery — from digital wallets and SSO to fraud prevention and workflow optimization. Our standards-based technology and public-sector expertise ensure every project advances a more secure, interoperable, and citizen-centric digital future.


Share
