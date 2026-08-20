---
schema_version: "1.0.0"
document_id: "b6cf8e94713a53041cbf1149cfe7503a1ed9a48657c545677a44de5943c9fe0f"
company_key: "okta-inc-class-a-common-stock"
company: "Okta Inc."
source_id: "okta-inc-class-a-common-stock-news-import-144d960cd8f2"
canonical_url: "https://www.okta.com/blog/product-innovation/quantum-computing-identity-security-risks/"
published_at: "2026-08-06T07:00:00+00:00"
first_seen_at: "2026-08-06T23:07:03.574222+00:00"
fetched_at: "2026-08-06T23:07:05.298938+00:00"
content_hash: "sha256:1027edb4680513ec85aefc71077a9f5f5eee99b07353d2784a411bac73747a41"
---

# Quantum computing and identity security: Two problems, one imperative

### Topics


---


Cybersecurity


,


Non-Human Identities


,


Okta Platform


,


Security


### Table of Contents


---


---


### Share


-
-
-


---


Ready to make Identity a business advantage? Sign up today.


[Get started](https://www.okta.com/free-trial/)


Key Takeaways


Quantum computing creates two distinct identity security risks: One immediate and one long-term


Immediate risk: Active quantum workloads rely on ungoverned non-human identities (NHIs), creating immediate and exploitable vulnerability in many enterprises right now


Long-term risk: Known as “Harvest Now, Decrypt Later,” adversaries are actively collecting encrypted data and planning to decrypt it within 10 years using quantum capabilities


Security leaders can secure their quantum workloads and cryptographic foundations for the future with Okta’s Blueprint for the Secure Agentic Enterprise


Quantum computing is probably already running in your enterprise. And the identities behind those workloads? They’re likely ungoverned. This is a security problem you need to address now.


And it doesn’t stop there. You must also begin planning for post-quantum cryptographic (PQC) standards today to future-proof against tomorrow’s decryption threats. Read on to discover the two identity security concerns for quantum computing and why both deserve a seat at the CISO's table in 2026.


## What is quantum computing—and how does it affect identity security?


Quantum computing is a new class of computing that harnesses quantum physics to solve problems classical computers cannot, making them exponentially more powerful for certain tasks. This includes breaking the encryption algorithms that protect enterprise identity systems today.


Quantum computing is not just a technology topic; it’s also a security topic. And because every quantum workload authenticates with an identity, identity teams sit at the center of this issue.


## Two quantum challenges, one identity blind spot


The security challenge of quantum computing spans two distinct, yet equally critical timelines that both require the attention of security leaders today:


1. **Quantum computing workloads are currently operating commercially** with non-human identities (NHIs), but[the majority of NHIs are ungoverned](https://www.okta.com/newsroom/articles/global-ciso-insights-2026/)
2. **Attackers are harvesting encrypted data** at this moment, counting on quantum computers powerful enough to[decrypt this data in the 2030s](https://www.nacdonline.org/all-governance/governance-resources/directorship-magazine/online-exclusives/2026/q2-2026/quantum-cyber-risk/)


### Challenge 1: Governing non-human identities in active quantum workloads


Quantum computing is already an active enterprise workload. Research teams, data science groups, and optimization engineers across the financial services, pharmaceuticals, logistics, and materials science industries are actively running workloads on platforms like IBM Quantum, AWS Braket, Azure Quantum, and Google Quantum AI.


And each of these workloads authenticates using an identity.


These are not human users logging in through a browser. They're automated pipelines, Jupyter notebooks running scheduled jobs, CI/CD systems submitting circuits, and orchestration scripts moving data between classical and quantum processors. In identity terms, these are non-human identities (NHIs), and they represent one of the fastest-growing and least-governed attack surfaces in enterprise IT.


Okta’s[Businesses at Work 2026 survey](https://www.okta.com/newsroom/articles/businesses-at-work-2026/) showed 650% year-over-year growth in centrally managed service accounts used by SaaS apps, services, agents, and tools. Still, only 10% of organizations report having a well-developed strategy to manage them.


### Five critical identity gaps in current quantum workloads


By definition, NHIs don’t have a human connected to them. This makes them prone to the same risky patterns security teams have spent years trying to eliminate for human identities, such as shared credentials and excessive standing access. Those same issues for NHIs also apply to quantum computing workloads:


- **Long-lived API keys and tokens:** Some quantum service integrations use static credentials that persist for months or years, with no automatic rotation
- **No enterprise single sign-on (SSO) integration:** Some platforms lack native support for Security Assertion Markup Language (SAML) or OpenID Connect (OIDC) federation, forcing teams to manage separate credential stores
- **No support for OAuth-based access delegation:** Some platforms require long-lived API keys instead of short-lived OAuth tokens, increasing the blast radius if credentials leak
- **Limited visibility:** Security teams often lack a centralized view of which NHIs are accessing quantum resources, what permissions they hold, or when they last authenticated
- **No policy-based access controls:** Access decisions are binary (key valid or not) rather than contextual (is this the right workload, at the right time, from the right environment?)


### Challenge 2: Mitigating the "Harvest Now, Decrypt Later" cryptographic threat


While you secure your current workloads, you must also address the long-term threat to your cryptographic foundation. You don’t worry about the security of identity transactions you rely on today because encryption protects them. The encryption depends on mathematical problems that would take classical computers millions of years to solve. But a sufficiently powerful quantum computer could break today’s encryption in as little as a week. While no such computer exists today, the cryptographic community broadly agrees that this capability will emerge within the next 5 to 15 years.


This is the "Harvest Now, Decrypt Later" (HNDL) reality.[Adversaries are actively intercepting and storing encrypted traffic](https://www.nacdonline.org/all-governance/governance-resources/directorship-magazine/online-exclusives/2026/q2-2026/quantum-cyber-risk/) today, predicting that quantum computers will eventually have the processing power to break current encryption standards.


Healthcare, pharmaceutical, and financial services organizations handle sensitive data with long-term retention requirements and long-horizon timelines. For example, the development of a new drug takes an average of[10-15 years](https://phrma.org/policy-issues/research-development) . If an adversary captures encrypted authentication tokens, session data, or federated identity assertions today, they may be able to decrypt that data in 2030 or 2035. For organizations handling classified information, long-lived trade secrets, or sensitive personal data, the window for action can’t wait until the next decade.


But the risk isn’t limited to specific industries. In August 2024, the National Institute of Standards and Technology (NIST) finalized its first set of[PQC standards](https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards) . Every organization must begin planning to adopt these new PQC algorithms well before quantum computers can decrypt data in real time.


### Comparison: The two quantum identity imperatives


Risk dimension Challenge 1: Non-human identity hygiene Challenge 2: Harvest Now, Decrypt Later


**Timeline** Immediate/active quantum workloads 5-15 year decryption horizon


**Primary targets** Quantum workload APIs, automated pipelines, orchestration scripts Intercepted session data, SAML assertions, tokens


**Core vulnerability** Ungoverned static keys and standing access RSA and ECC


**Remediation standard** Federated SSO, OAuth tokens, just-in-time access NIST PQC Standards (FIPS 203, 204, 205)


## Okta's approach: Securing identity across both quantum issues


Okta occupies a unique position in this landscape. As a leading **independent, neutral identity platform** , we secure access across a variety of environments, clouds, on-premises systems, and SaaS applications. We're not tied to a single hyperscaler's ecosystem. This independence is foundational to our approach to both near- and medium-term quantum identity challenges.


As with cloud computing, organizations will work with multiple providers; no single vendor is likely to "win" the quantum space. This makes platform-neutral identity an operational requirement, not a luxury. If your identity layer is locked into a single provider's ecosystem, you inherit that provider's blind spots. Okta’s independence allows you to enforce a consistent security policy across environments as your quantum strategy matures.


### A three-part framework for non-human identity governance


Every organization should ask three foundational questions about any automated workload accessing enterprise resources, including quantum computing:


1. **Who and what is accessing which resources?** Ask your team not just who and what have access to your enterprise resources, but what they are doing with that access—including quantum computing workloads.
2. **Is that access appropriate?** Once you can see a workload, you need to map every resource it can reach, and enforce access policies such as least privilege and just-in-time credentials.
3. **What happens when something changes?** Knowing where workloads are and what they can connect to isn't enough if you can't control and cut off what they actually do. If a researcher leaves, a project ends, or an attacker compromises a credential, you need to revoke access instantly across all quantum platforms.


For most organizations today, the honest answer to all three questions about quantum computing workloads is "we don't know."


### Solving challenge 1: How Okta secures identity across classical and quantum systems


Quantum computing workloads authenticate exactly like any other NHI, which means the same enterprise-grade controls apply directly.


- **Enterprise SSO for non-human workloads:** Federate quantum computing access through your existing identity provider, reducing shadow credentials
- **Short-lived, scoped tokens:** Replace long-lived API keys with tokens that expire in minutes or hours, dramatically reducing the blast radius of credential compromise
- **Policy-based access controls:** Define who (or what) can access which quantum resources, under what conditions, with contextual signals like time, location, and risk score
- **Centralized visibility:** A single pane of glass showing all NHIs accessing quantum services across IBM, AWS, Azure, and Google
- **Lifecycle management:** Automatically provision and deprovision quantum computing access as projects start and end, teams change, or risk posture shifts


This is the cross-cloud advantage. Organizations using multiple quantum providers, which is increasingly common as teams evaluate different hardware architectures, need an identity layer that spans all of them. A platform-native identity solution from a single hyperscaler only covers that hyperscaler's quantum service. Okta covers all the leading hyperscalers.


### Solving challenge 2: Okta’s two-phase roadmap for post-quantum cryptography


Okta is also actively mitigating the long-term threat to the cryptographic foundation of identity. We’ve defined a two-phase roadmap to prepare our customers for a PQC world:


#### Phase 1: Protect our customers (edge and federation)


We are implementing Hybrid Key Exchange (HKE), which combines existing algorithms in parallel with the NIST-standardized PQC algorithms. This protects your identities today and against HNDL attacks, and, critically, maintains compatibility with your existing environment.


#### Phase 2: Enable the ecosystem


We are equipping the[Okta Integration Network](https://www.okta.com/solutions/okta-integration-network/) and our administrative tooling to support a seamless transition. This includes providing the administrative controls to update tenants to PQC-ready standards as they become the new default, helping ensure your organization can migrate seamlessly.


## Five immediate action steps for security leaders


Quantum computing's identity implications are a current priority. Here's where to start:


- **Inventory your NHIs touching quantum services.** You can't secure what you can't see. Map every API key, service account, and automated credential accessing quantum computing platforms.
- **Eliminate long-lived tokens.** Migrate to short-lived, federated credentials wherever possible. This is good hygiene regardless of quantum risk.
- **Apply the three-question framework.** For every quantum workload, ask: Who is accessing what? Is that access appropriate? What happens when something changes?
- **Audit your cryptographic exposure.** Catalog where your identity infrastructure uses RSA and elliptic curve cryptography (ECC). Understand your exposure to HNDL attacks.
- **Verify vendor PQC readiness.** Ask your vendors for their PQC roadmap. If they don't have one, that's a red flag.


The quantum era will reward organizations that treat identity as the control plane for all workloads: human and non-human, classical and quantum, today and tomorrow. The time to build that foundation is now.


### Secure your agentic enterprise


As autonomous workloads and AI agents scale across your environment at machine speed, treating them as first-class identities is the only way to close emerging security gaps. Protect your organization before they scale out of control. Map your environment and build a secure, resilient foundation with our[Blueprint for the Secure Agentic Enterprise](https://www.okta.com/solutions/secure-ai/agentic-enterprise-blueprint/) .


*Any mention in this blog of solutions, features, functionalities, certifications, authorizations, or attestations that are not currently Generally Available or have not yet been obtained may not be delivered or obtained on time or at all. We assume no obligation to deliver on such items and you should not rely on them to make your purchase decisions.*


About the Author


[David Skyberg Director, Product Management David Skyberg leads Okta Product Management for US Public Sector at Okta. He has decades of experience delivering world-class Security and IAM products. Prior to arriving at Okta, David worked for industry-leading companies such as Capital One, Ping Identity, Microsoft, and RSA Security.](https://www.okta.com/blog/author/david-skyberg/)


[Sandeep Kumbhat VP, Global Field CTO Sandeep Kumbhat serves as the VP, Global Field CTO at Okta. In this leadership role, he serves as a strategic advisor on product strategy, solution design, architecture, and engineering for various internal and external stakeholders, including Okta's top global customers. Sandeep also works with Industry Analysts, VCs, and other industry peers on the strategic advisory front. Prior to joining Okta, Sandeep worked for Oracle/Sun Microsystems, HP, Accenture, and GE, leading programs to mitigate security risks in large-scale enterprise service integrations. His multifaceted expertise in AI security, enterprise solution architecture, identity, and cybersecurity, coupled with his business acumen, enables him to engage with executives and technical champions to keep security solutions simple, up to date, and relevant.](https://www.okta.com/blog/author/sandeep-kumbhat/)


[Steve Conn Staff Product Marketing Manager - Infrastructure Steve Conn is a Staff Product Marketing Manager, Infrastructure, driving the go-to-market strategy for Okta’s core Infrastructure and Trust motion.](https://www.okta.com/blog/author/steve-conn/)
