---
schema_version: "1.0.0"
document_id: "a5686ac27e7bdc3f445d6a99a20fc69a1ccaf6bca60e0d0568528494466eff4b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authz-primer"
published_at: "2023-09-06T15:37:08.588+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:f2712e79f049784aac15c5ecb30cc9ff9b950df7743a72f18f6da5633220fd99"
---

# Authorization (AuthZ): Quick Start Guide for Enterprise Permissions

## **Introduction**


A business operating complex software environments needs to provide end-user experiences that enable and delight users, but never at the cost of a poor security stance. In 2022, a lapse in security fetched[on average 9.44M for a data breach in the US, and $4.35M globally according to IBM](https://www.ibm.com/reports/data-breach) .


This post aims to help companies with their authorization decisions and systems and share what we see in the market through conversations with companies looking to solve their authorization challenges, specifically authorization that impacts end-user interaction with their products. I’ll cover how authorization became an issue for most companies, the different approaches, an introduction to Google Zanzibar–the solution the market is converging on, the prominent use cases driving businesses to adopt this new approach, and what you can expect when moving to a relationship-based access control system (ReBAC).


## **What is AuthZ?**


Because the Internet is, by definition, a networked system that connects users, most Internet-facing software is designed with a multi-user experience in mind. These environments require constructs to facilitate a natural experience that protects users’ data: **authentication** , commonly referred to as authN, is the key that verifies an application-specific identity, typically called a user, and **authorization** , commonly referred to as authZ, dictates what doors that key can open for a user.


AuthZ plays a pivotal role in software security by ensuring that users have the appropriate level of access to different resources and functionalities within a system. At its core, authZ involves granting or denying access rights to specific resources or actions based on the identity and privileges of the user. It is crucial in preventing unauthorized access and verifying that only authorized users can perform specific actions within a system. It serves as a gatekeeper that protects sensitive data and functionalities. By implementing robust authorization mechanisms, developers can control the level of access granted to different users or roles, thereby safeguarding the system from potential security breaches.


Collectively in the context of a product’s end users, authN and authZ are called Customer Identity and Access Management (CIAM). Authorization is such a foundational part of the digital experience that its underlying design principles have become[99% invisible](https://99percentinvisible.org/about/the-show/) , even to developers, leading to fundamental challenges as a business scales.


## **Why AuthZ is an Issue**


A company typically starts by aggregating all user requests into a single piece of software that tightly couples application logic with authZ. As the company’s product gains traction and its user base grows, the focus shifts to distributing the software and scaling infrastructure components, often ignoring a much-needed change to the authorization system. This further embeds an authorization construct not meant to handle a growing number of requirements.


From the business perspective, the two key limitations of a legacy authorization system are:


- **Permissions are inflexible** : There isn’t a way to easily add additional constructs like[user-defined roles](https://authzed.com/blog/user-defined-roles) , recursive relationships,[attribute-based access control (ABAC)](https://authzed.com/blog/abac-on-spicedb-enabling-netflix-complex-identity-types) , or[fine-grained authorization (FGA)](https://authzed.com/blog/fine-grained-access-control) .
- **Siloed permissions:** as a company grows, it scales revenue by offering additional products; application teams then build bespoke authorization implementations that are hard to reason about and don’t consider a holistic user experience, especially at large scale.


Google set out to fix this problem, along with several “[unique challenges involving data consistency and scalability](https://zanzibar.tech/2AT-rbOOg7:R:1h) .”


## **Google’s Solution to Authorization: Google Zanzibar**


The confluence of business requirements driving the adoption of[zero-trust architectures](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf) and[94% of consumers wanting more control of their data](https://qonsent.com/static/pdf/qonsent-consumer-insights-june2022.pdf) in near real-time galvanized the search for a modern authorization system. Google’s response was a modern approach that can scale with your business and maintain strict security requirements, now known as[Google Zanzibar](https://authzed.com/blog/what-is-google-zanzibar) .


Among the requirements set forth by the Google Zanzibar team is “[support for a rich set of access control policies as required by both consumer and enterprise applications](https://zanzibar.tech/2MUghfOn_Y:A:1n) ” and “\[establish consistent semantics and user \[developer\] experience across applications\]([https://zanzibar.tech/2I7pd_DGm2:2C.49Q9UoJW_:J).”](https://zanzibar.tech/2I7pd_DGm2:2C.49Q9UoJW_:J).%E2%80%9D) Zanzibar powers authorization across hundreds of Google Products, including Google Calendar, Cloud, Drive, Maps, Photos, and YouTube. Notably, it unlocks unique experiences like[cross-product authorization checks,](https://zanzibar.tech/2Ry8nS2U5I:0:1t) e.g., Slack’s Gmail extension can check if a recipient has access to a Google Doc, unlocking growth through reduced friction points while maintaining user privacy.


Google Zanzibar is a relationship-based access control system (ReBAC), meaning that permissions are derived from the existence of a relationship between digital objects and users. This has positive performance implications, especially for recursive permissions, but, importantly, it’s a natural extension of sharing in the real world, which makes it intuitive for most developers.


## Use-Cases Driving Adoption of Google Zanzibar


### **Product-Led Growth**


To share something is a user choice and subsequent action. Companies we’re speaking with have learned that facilitating frictionless sharing can help onboard additional users to their platforms. For instance, a hiring platform we’re working with implements fine-grained authorization (FGA) so enterprise recruiters are comfortable exposing permissions to hiring managers related to the positions the managers are looking to fill. Hiring managers, in turn, can proactively engage with candidates and increase activity on the platform.


Another example is adding capabilities that foster more in-app experiences. For instance, a sharing economy company is boosting engagement with its platform by bringing resource management for users into their native applications instead of relying on third-party applications. Most companies we speak with share similar product-led growth initiatives built atop robust authorization.


### **Breaking into the Enterprise**


Enhancing security and compliance is a key requirement for B2B companies looking to scale revenue within an enterprise market segment, and OWASP’s 2021 report cites[broken access control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) as a top security concern. One of the main risks is “exposure of sensitive information to an unauthorized actor,” which is a core tenet of the Google Zanzibar paper; the system "[must ensure consistency of access control decisions to respect user intentions.](https://zanzibar.tech/22_E1zFxjc:0.4BawKBUjM:1K) ”


Enterprise users also require increased flexibility; these manifest as the following requirements for product teams:


1. **Fine-grained authorization (FGA):** the ability to control resources down to a granular level, e.g., a page in a document, though there is a balance, see[Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
2. **User-defined Roles and Permissions:** beyond a typical application-defined Role-Based Access Control (RBAC) system, product teams need to allow end-user admins to create roles and associated permissions for delegating to internal teams.
3. **Recursive relationships:** at a certain scale, teams start owning teams. This is challenging for a traditional authorization system dealing with permissions stored in a relational database alongside application data.
4. **Attributes-Based Access Control (ABAC):** support for dynamic time-bound or otherwise caveated access.


## **What to Expect When Adopting Relationship-Based Access Control (ReBAC)**


A crucial part of ReBAC systems like Google Zanzibar and our own[Zanzibar-inspired authorization system, SpiceDB,](https://authzed.com/products/spicedb) is storing permissions data in a separate database; product-specific data (e.g. content of a social media post) is stored in the application database, while the data that drives who can edit that data live in the permissions database. If you have an existing authorization flow, you’ll have to translate that data into permissions data.


Modeling data is probably the most fun and intuitive part. SpiceDB, like other solutions, has a domain-specific language (DSL) called the[SpiceDB Schema Language](https://authzed.com/docs/reference/schema-lang) for defining the objects you want to create an authorization system for. The permissions schema defines the objects, e.g., users and documents, how they relate to each other, and the permissions those relationships define.


Since you’re writing permissions data, integration is a big part of the journey. A ReBAC authorization system is delivered over a gRPC or HTTP API; SpiceDB has libraries available in multiple languages to help developers get up to speed quickly. You’ll want to make sure whatever solution you choose delivers a solid developer experience.


Google Zanzibar doesn’t mention policy, but we’ve learned through our collaboration building[Attribute-Based Access Control (ABAC) for Netflix](https://authzed.com/blog/abac-on-spicedb-enabling-netflix-complex-identity-types) that pairing policy with ReBAC is a powerful paradigm. An example of this capability is SpiceDB Caveats:[Caveats: A Scalable Solution for Policy](https://authzed.com/blog/caveats) .


A common practice is to organize a core team of developers tasked with architecting and executing an overhaul to your authorization system. The effort must be cross-functional; you’ll want platform engineers, application engineers, and product managers to work together to ensure smooth adoption.


## Get Started


Given how popular Google's approach to authorization has become, there are a number of new companies and projects looking to provide Zanzibar-aaS. At AuthZed, we've created a faithful open-source implementation of Google Zanzibar called[SpiceDB](https://github.com/authzed/spicedb) , and offer managed commercial offerings that make it easy to get into production. Join the community on[Discord](https://authzed.com/discord) or[schedule a call](https://authzed.com/call) to learn more!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


Originally published September 6, 2023


On this page


- Introduction
- What is AuthZ?
- Why AuthZ is an Issue
- Google’s Solution to Authorization: Google Zanzibar
- Use-Cases Driving Adoption of Google Zanzibar
- Product-Led Growth
- Breaking into the Enterprise
- What to Expect When Adopting Relationship-Based Access Control (ReBAC)
- Get Started
- Additional Reading


## Related


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)
