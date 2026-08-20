---
schema_version: "1.0.0"
document_id: "e37431ff245d329e5d3c934431e0081bf661dbae6c9e1c53f13c7abff9be99d0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/casbin"
published_at: "2024-09-23T17:09:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:59:50.514445+00:00"
content_hash: "sha256:c367fb05c15d7f2c14975ac069123a924208fb31f0fca94e1f042ce519db7b78"
---

# Open source authorization: Comparing Casbin and SpiceDB

When designing your authorization service, should you embed a permissions library directly into your application or adopt an external centralized permissions system such as Google Zanzibar?


Both approaches come with trade-offs. Let’s explore two popular open-source permissions systems that represent each approach: Casbin and SpiceDB (an open source implementation of Google Zanzibar).


## What is Casbin?


Casbin is an open source access control library that integrates directly into an application, allowing developers to embed customizable access control models like RBAC (Role-Based Access Control), ABAC (Attribute-Based Access Control), and ACLs (Access Control Lists) directly into the codebase.


## Casbin: Pros and Cons


This in-app approach provides developers with a high degree of flexibility to implement bespoke authorization designs crafted to their exact usecase.


However, this also means that the development team is responsible for designing, implementing, and maintaining the entire permissions system, which can introduce complexity as the application scales. Casbin has a collection of extensions to help.


## Deep Dive


#### Flexibility


##### Model adaptability


Casbin includes support for RBAC, ABAC, and ACL models, making it useful in a variety of authorization usecases.


However, integrating multiple models together (e.g., combining RBAC with ABAC) requires additional effort from developers, and custom roles or domain-specific logic may need to be defined manually.


##### Compatibility


Casbin is compatible with many programming languages, including Go, Java, Python, Node.js, PHP, .NET, and Rust.


However, developers must carefully manage language compatibility, dependency conflicts, and version upgrades across multiple services.


### Scalability


##### Performance


Casbin’s in-memory policy evaluation works well when handling a small number of policies, but as the policy set expands or more complex checks are required, developers must address performance challenges by implementing their own sharding, multi-threading, or even distributed policy evaluation.


While Casbin does support batch enforcement to optimize performance by evaluating multiple permissions at once, this still requires significant developer involvement as the system scales.


##### Complexity


Setting up Casbin is relatively straightforward, requiring no external infrastructure, so teams can get started quickly by defining access control policies and embedding enforcement logic directly in the application.


However, as applications scale, managing and synchronizing policies across distributed services becomes more complex. Adding or updating permissions often requires manually modifying policy files or databases, a process that can slow down development as the system grows.


### Security


##### Correctness


Casbin operates as a stateless library, meaning it does not maintain state or synchronization across multiple invocations.


In distributed environments, this lack of inherent state management increases complexity, as developers must ensure that policy changes are consistently propagated and enforced across all services.


Correctness of permission checks largely depends on how well the developer manages policy storage and synchronization.


##### Auditibility


Casbin defines permissions in policy files or databases written as code. Because these permissions are distributed across applications, auditing them can be challenging.


Casbin lacks built-in auditing tools, so developers need to create custom solutions for logging permission changes and tracking policy enforcement, which increases development overhead.


## What are the benefits of using Google Zanzibar vs Casbin?


Google Zanzibar, Google’s Consistent, Global Authorization System, uses relationship-based access control (ReBAC) to define permissions. We have written extensively about Google Zanzibar and ReBAC; Google Zanzibar has multiple open-source implementations, including SpiceDB.


## Comparing SpiceDB and Casbin


SpiceDB is an open source, centralized permissions system that externalizes authorization decisions from the application codebase. Built on concepts from Google’s Zanzibar system, SpiceDB excels at handling complex relationships and permissions across large-scale, distributed systems.


Instead of embedding logic into the application, SpiceDB separates authorization into a service layer that manages permission checks based on dynamic relationships and hierarchical structures. This centralized model reduces the operational overhead for development teams, allowing them to focus on core features while relying on SpiceDB to manage the intricacies of authorization at scale.


### Flexibility


##### Model adaptability


SpiceDB primarily defines permissions in a ReBAC (relationship-based access control) model with extended ABAC functionality using Caveats, making it easy to define permissions with dynamic, hierarchical relationships or multiple levels of nesting. SpiceDB's expressive schema language allow developers to intuitively model even the most complex systems.


##### Compatibility


SpiceDB operates as an external service accessible via gRPC or REST APIs, making it largely agnostic to the tech stack of the client applications.


Client applications simply consume the service without being constrained by specific technology decisions.


SpiceDB clients are available for many languages, including Go, .NET, Java, Python, Rust, and TypeScript, making it highly versatile across various environments.


### Scalability


##### Performance


SpiceDB, built for large-scale applications, uses graph traversal algorithms and relationship-based querying to evaluate complex or deeply nested relationships efficiently. Including native caching and bulk permission checking capabilities, SpiceDB handles high query volumes while maintaining strong performance with little development effort.


##### Complexity


SpiceDB, though requiring more initial effort to configure, provides a centralized permissions system that can be reused across multiple applications and services.


Setting up SpiceDB involves configuring a datastore and writing the schema for managing permissions, but once deployed, it simplifies ongoing permissions management.


New permissions or access rules can be added by updating the schema without needing to modify application code or synchronize policies across services.


### Security


##### Correctness


SpiceDB is a stateful service designed to maintain correctness across distributed systems by managing state internally. SpiceDB provides tunable consistency settings per check request, allowing developers to opt for eventual consistency for high-performance scenarios or full consistency when strict correctness is required.


Developers can avoid the tradeoff between consistency and performance altogether by implementing zed tokens, an analogue for the zookie concept from Google Zanzibar that protects users from the New Enemy Problem.


##### Auditibility


SpiceDB uses a centralized schema to define permissions, which makes it easier to audit permissions across multiple services and systems. The schema language is flexible and expressive, allowing developers to intuitively model complex permission hierarchies, increasing transparency policy audits. for audits.


SpiceDB includes built-in tools like the explain command and structured logging, which enable developers to trace how permissions decisions were made. These tools simplify auditing, providing detailed trails that security teams can use to verify that access controls are correctly enforced and compliant with regulations.


## Conclusion


Embedding an authorization library like Casbin provides direct control and customization, making it ideal for smaller projects or those requiring bespoke access control models.


However, as the system grows, the complexity of maintaining and expanding the system increases.


Adopting an externalized, centralized system like SpiceDB offers a scalable, streamlined approach to managing permissions across distributed environments. With built-in tools for correctness, auditing, and performance, SpiceDB allows teams to focus more on feature development and less on the intricacies of authorization.


Ultimately, whether you choose to embed or adopt, the decision will shape the future flexibility, security, and scalability of your authorization strategy.


On this page


- What is Casbin?
- Casbin: Pros and Cons
- Deep Dive
- Flexibility
- Model adaptability
- Compatibility
- Scalability
- Performance
- Complexity
- Security
- Correctness
- Auditibility
- What are the benefits of using Google Zanzibar vs Casbin?
- Comparing SpiceDB and Casbin
- Flexibility
- Model adaptability
- Compatibility
- Scalability
- Performance
- Complexity
- Security
- Correctness
- Auditibility
- Conclusion


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
