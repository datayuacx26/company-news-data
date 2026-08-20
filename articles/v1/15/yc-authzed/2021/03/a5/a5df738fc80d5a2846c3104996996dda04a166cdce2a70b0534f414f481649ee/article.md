---
schema_version: "1.0.0"
document_id: "a5df738fc80d5a2846c3104996996dda04a166cdce2a70b0534f414f481649ee"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/okta-bought-auth0"
published_at: "2021-03-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:ca7063052004b7b2666a1c7937f04b428e8c97d46a2a9da907e91b5c05893403"
---

# Okta Bought Auth0: What Does That Mean for Authzed?

By now you have probably heard the news that[Okta is buying Auth0](https://www.okta.com/press-room/press-releases/okta-signs-agreement-to-acquire-auth0/) . Auth0 is an identity provider and aggregator, giving customers a single integration point for managing their own customers’ identity data. As a permissions system as a service company with the word “auth” in our name, we often get lumped together in a category with Auth0. People keep asking me: “What is the impact on Authzed of Auth0 getting acquired?”


There is actually very little impact to Authzed! Auth is really the combination of two distinct but related concepts, and each concept has more (mostly interchangeable) names than a Dostoyevsky character. Authentication, a.k.a. authN, a.k.a. identity is the way that you figure out **who** you’re dealing with. It’s usually accomplished by collecting up a number of pieces of evidence (username, password, fingerprint, one-time passwords, certificate, etc) and making a decision about whether there is a high enough degree of certainty that the delegate with which you’re interacting (often a web browser) is being controlled by the purported entity.


Authorization, a.k.a. permissions, a.k.a. access management, on the other hand, is the science of determining **what** an entity should be allowed to see and do in a system. For example, you, the reader as a member of the general public, are allowed to see this blog post, but not edit it!


While Auth0 is primarily focused on authentication, Authzed (as our name implies), is focused on helping the world to make great *authorization* decisions. We enable this decision making by running a hosted, multi-tenant, permissions system as a service. Our customers utilize our platform to make fast and accurate access management decisions without having to do tedious, error prone, sensitive computations in their app servers. The questions often take the very simple form of “Is` <subject>` allowed to` <action>` on this` <object>` ?”


There has been some impact to our product and business though. First, we heavily use Auth0 as our identity aggregator in[Authzed](https://authzed.com/) . That’s right, we’re so removed from the identity game, that we outsource it! We think Auth0 will continue to be successful at Okta, and see no need to re-evaluate our choice of identity providers at this time.


Another way that this move impacts us, is by showing the world how valuable hosted service providers in this space can be! Our service parallels Auth0 in a lot of ways: Both services run primarily as a SaaS. Both services have a strong focus on the developer. Both services are essentially selling an API with a rich client ecosystem. There was a time when many would have questioned the wisdom of having a mission critical system such as permissions delegated to a hosted service provider. We believe Auth0 has really forged the way by showing that delegating something as important as identity (which has traditionally been done on-prem and usually bespoke) can be an accelerator for businesses! Now with a large acquisition, the market has validated with a number what many of us already knew: such services provide tremendous value.


If you’re less optimistic about the independence of Auth0 post acquisition, there are still a number of ways that you can utilize a hosted platform to manage identity in your application:


- [PingIdentity](https://www.pingidentity.com/) is probably the most direct competitor to Auth0.
- [SuperTokens](https://supertokens.io/) is an open-source identity solution with a hosted offering that competes directly with Auth0.
- Cloud providers often offer an identity service that you can integrate with your apps. AWS has[Cognito](https://aws.amazon.com/cognito/) , GCP has[Identity Platform](https://cloud.google.com/identity-platform) , and Azure has[Azure AD](https://azure.microsoft.com/en-us/services/active-directory/external-identities/)
- You can also directly integrate with an identity provider’s existing[OpenID Connect](https://openid.net/connect/) endpoints.


Authzed supports identity from all of these options! The only thing you need to be able to use Authzed is a stable identifier for the end-user. This often comes in the form of a[sub claim](https://openid.net/specs/openid-connect-core-1_0.html#Claims) on an OIDC ID token.


If you’re interested in learning more about how Authzed can help you build a better service through improved permissions fidelity, reliability, and testability, please reach out today. We’re already running in production and are onboarding our initial set of early access users!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Additional Reading


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)
