---
schema_version: "1.0.0"
document_id: "3a19ec1b64ad395ede8a93701800acd86253a9c7344ae2c8078fef63dd338af5"
company_key: "yc-integuru"
company: "Integuru"
source_id: "yc-integuru-news-import-ab81679661d6"
canonical_url: "https://www.integuru.com/blog/user-permissioned-api-access"
published_at: "2026-07-22T00:00:00+00:00"
first_seen_at: "2026-07-27T05:10:38.353575+00:00"
fetched_at: "2026-07-28T21:20:58.380206+00:00"
content_hash: "sha256:5b094f7b390412bb69969b5ba168901f501c8f159c3666188d1376c6329f5a55"
---

# User-Permissioned API Access: How Integuru Connects on Behalf of Real Accounts

The compliance question that stops enterprise deals is not "can this platform be automated?" It is: "If the platform doesn't have a public API, how do you access it without violating their terms?"


That question deserves a direct answer. Integuru's model is user-permissioned access: the customer provides credentials for their own account, explicitly authorizes Integuru to act on their behalf, and retains control over that access. The integration operates through their account, not around it. This post explains what that means in practice, how it compares to unauthorized scraping, and what enterprise buyers in regulated industries should understand before starting a compliance review.


### **What is user-permissioned API access?**


User-permissioned API access is access to a third-party platform's endpoints that happens through credentials and authorization provided by the account holder. The user is giving permission for software to act on their behalf, not a service bypassing the platform. The account holder retains ownership of the account and controls what access is granted. The integration works because a real, authorized user is directing it to act on their behalf, the same way they would direct any other software tool they install to do work on their behalf.


The terminology comes from fintech. Plaid, which connects to over 12,000 financial institutions, built the established model: a user authenticates with their bank credentials, explicitly selects which accounts to share, and consents to data sharing. The application receives access only to accounts the user explicitly permissioned during onboarding. Plaid describes this as "user-permissioned financial data": the user's authorization is the foundation of the entire model.


Integuru applies this same principle to platforms that have not built a formal consent framework: healthcare portals, carrier management systems, legal practice management tools, and fintech platforms with limited or no public APIs.


### **How user-permissioned access differs from unauthorized scraping**


User-permissioned access and unauthorized scraping differ on one axis that matters for both legal analysis and enterprise compliance review: who authorized the access, and over whose account.


Unauthorized scraping means accessing accounts or data the accessor does not own and has not been authorized to access. It may involve creating accounts specifically to harvest data, accessing credentials that were not provided by their owner, or accessing accounts beyond what any given user has legitimately permitted. This is the practice that has drawn enforcement attention under the Computer Fraud and Abuse Act and similar frameworks.


User-permissioned access is categorically different because the account being accessed belongs to the customer and the credentials were provided by the person who owns and controls that account. Integuru accesses only what that account can legitimately access, scoped to what the customer has asked us to automate.


**User-Permissioned Access**


**Unauthorized Scraping**


**Who controls credentials**


Account holder provides their own credentials


Credentials accessed without account holder's knowledge or consent


**Account access type**


Legitimate account the user owns and authorizes


Account the accessor does not own or control


**Data scope**


Limited to data the authorizing account can access


Attempts to access any available data regardless of ownership


**Legal basis**


Account holder's authorization; platform ToS review recommended


No authorization; typically violates ToS and potentially law


**Enterprise suitability**


Defensible basis for automated access; standard compliance review


Generally unsuitable; carries legal and reputational risk


*Note: Consult qualified legal counsel for platform-specific agreement review. This table represents general framing, not legal advice.*


### **How user-permissioned access relates to existing legal frameworks**


Three established frameworks are relevant context for enterprise buyers evaluating automated access. The right answer for any specific platform depends on the facts of your deployment and your legal counsel's analysis.


**OAuth** is the formal standard for consent-based access: the platform itself designed and endorsed the consent flow, the scope is bounded by what the user explicitly granted, and the access token is meant for third-party use from day one. When a platform has built an OAuth program, that is the gold standard path.


**Plaid's model** shows what user-permissioned access looks like when a platform hasn't built OAuth. Plaid connects fintech applications to banks through customer-provided credentials and explicit consent captured at onboarding. The user's active authorization at connection time is the foundation of the model. It has operated at scale across major financial institutions and became the basis for open banking regulation in several jurisdictions.


**Terms of service review** is the practical starting point for any specific integration. Platforms vary: some prohibit automated access entirely, others permit it for authorized users acting through their own accounts, many are silent. Customers should review their agreements with the target platform before deploying any automated integration, and should involve legal counsel for regulated verticals. Integuru's user-permissioned model puts the authorization decision with the account holder, but that does not eliminate the need for customers to review their own contractual commitments.


> User-permissioned access means Integuru acts on behalf of an account the customer controls and has authorized us to use, not as an independent actor accessing a platform on its own.


---


### **How Integuru's user-permissioned model works in practice**


At Integuru, we generate integrations for platforms that lack public APIs by accessing the same private` HTTP` endpoints their web interfaces already call. The model requires user participation from the start.


A customer connects a platform by authenticating through their own account. Integuru's agent captures the network requests that correspond to the workflows the customer wants to automate, using that authenticated session. The resulting integration calls those endpoints on behalf of the customer's account: the same credentials, the same access scope, with no access to other users' accounts and no data beyond what the authorizing account can reach.


Integuru accesses only platforms with authentication, deliberately. The platforms where user-permissioned access creates real value (EHR systems, carrier portals, banking platforms, legal case management tools) all require a logged-in session to do anything useful. That authentication requirement is itself a form of access control. Integuru works within it, not around it.


Authentication is handled continuously. On the Production plan, Integuru's 24/7 on-call maintenance team handles session expiry, token rotation, and 2FA challenges automatically, so the integration stays live without the customer's team managing re-authentication manually. Integuru was founded in 2024 and is backed by Y Combinator.


-


**99.9%+** reliability rate across production integrations


-


**Under 3 sec** average response time per integration call


-


**10–20 min** to generate endpoints for a new platform


-


**24/7 on-call** maintenance included on the Production plan


### **What enterprise buyers need to know about compliance**


Enterprise compliance reviews for integrations like Integuru's typically focus on four areas.


**Data security.** Integuru processes requests and returns responses without storing customer data beyond what is required to execute the integration. For healthcare deployments, this matters for HIPAA: the covered entity remains responsible for its own compliance posture, and any vendor handling PHI should be reviewed as a business associate. Request Integuru's current security documentation for your compliance process.


**SOC 2.** Enterprise buyers routinely request SOC 2 Type II reports as a baseline vendor security assessment. Raise this directly with Integuru during evaluation; the right answer is the current attestation, not a general statement.


**Platform agreement review and credential security:**


-


Customers are responsible for reviewing their own agreements with the platforms they integrate through Integuru. "Defensible" is a position to evaluate with your legal team, not a guarantee.


-


Credentials for customer-owned accounts are stored and transmitted using standard encryption practices. Specific documentation is available on request.


No integration vendor can provide blanket legal clearance for any platform you want to connect to. What Integuru provides is a technically sound, user-permissioned architecture that puts authorization in the hands of the account holder, paired with a security posture appropriate for regulated industries.


### **Industries where user-permissioned integration is most relevant**


The industries where Integuru sees the strongest demand for user-permissioned access share a common profile: complex platforms built for operational users, no public API, and real regulatory stakes attached to how data is handled.


**Healthcare.** EHR systems like Modmed, eClinicalWorks, NextGen, and Tebra were built for clinicians, not developers. They have no public APIs for the operational workflows healthcare AI companies need to automate: scheduling, prior authorization, billing, and chart management. Integuru's healthcare integrations work through provider credentials. The EHR sees an authenticated provider session; the data accessed is scoped to what that provider's account can reach. For a detailed walkthrough of EHR integration approaches, see type: entry-hyperlink id: j9Xy7uRMfsRsFPGdvhm43


.


**Fintech.** Banking platforms and financial portals often expose limited public APIs for read access but nothing for operational workflows. A fintech team connecting to a banking portal through Integuru does so through their business account. The integration acts on behalf of that account. For the specific patterns fintech teams use, see type: entry-hyperlink id: 3uChrjoZfLqgK0lg5H1Mjw


.


**Logistics.** Carrier portals and freight management platforms authenticate shippers before exposing rate data, booking workflows, and shipment tracking. Integuru integrations for logistics run through the shipper's own portal account.


**Legal.** Court filing portals, matter management systems, and legal research platforms all require authenticated sessions. Integrating these platforms through Integuru means the firm's credentials authorize the access: the same scope the firm's staff would have manually.


For an explanation of the private endpoint structure these integrations call, see type: entry-hyperlink id: 5BCYfRQ3QBBTOOrp4BNrPZ


.


### **Our Services**


For regulated industries that need access to platforms with no public API, Integuru's user-permissioned model provides the technically defensible path. Integuru generates a production-ready integration in 10 to 20 minutes, running through the customer's own authenticated account.


The fastest way to start is the CLI:


` npm install -g integuru`


Or open the web app at[app.integuru.com](https://app.integuru.com/) . For compliance discussions or to walk through your specific platform and use case,[book a call](https://calendly.com/d/cqb8-d9x-nbf/integuru) oremail us .
