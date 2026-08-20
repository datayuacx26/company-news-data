---
schema_version: "1.0.0"
document_id: "7f9b622902a24dbaf804e060de78df60786220837d8cf7cf5437bc5aba5fb967"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/permissions-systems-building-an-authorization-lexicon"
published_at: "2024-08-07T15:57:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:54621e69db01df3ce8b4a5dab80768394ec5e0c300f28c7d0762ef87fb312979"
---

# How to build a permissions system (and why it's hard)

## Why words matter


### TL;DR


- Permissions define rules; authorization enforces them.
- Authorization services abstract complex logic for apps.
- Graph-based models handle relationships better than lists.
- Building internal systems drains engineering resources.


Every field develops its own linguistic shortcuts – jargon – to streamline communication around complex ideas. Consider *browser cookies* : a term that conjures images of tasty treats, but actually refers to bits of data that can be used for tracking online activity. While convenient for insiders, such jargon can be confusing for the uninitiated.


Common language, including jargon, usually settles organically as an area of practice matures. Sometimes early innovators intentionally agree to a particular framework for convenience.


While the practice of building authorization in applications is not new, modern approaches are evolving rapidly, spurred by advancements like[Google's Zanzibar paper](https://authzed.com/zanzibar) and the increasing demand for sophisticated collaborative features.


However, the language used to describe authorization solutions hasn't kept pace. We often rely on terminology borrowed from the lexicon of identity and access management (IAM), a field focused on resource control rather than application development. This mismatch can hinder clear communication and can lead to confusion.


How do we start clearing the air? Common language helps the community converse more productively about the tradeoffs involved when comparing authorization services and permissions systems. The first step: establish a shared vocabulary, starting with foundational elements.


### Semantics


Often used interchangeably, the terms *permissions* and *authorization* represent distinct, yet related concepts. **Permissions** establish and reason about the rules and logic that govern permissible actions, defining the boundaries of what can and cannot be done under specific conditions and contexts. **Authorization** is the *process* of answering questions about permissions. In short, permissions determine whether or not an action is authorized.


## Permissions systems


To determine whether any particular action is allowed or not, a decision maker requires the following:


- A defined set of guidelines governing allowable actions
- Relevant data about the action, including the actor (who's doing it), the action itself, the object of the action, and sometimes additional context or attributes
- The method for applying those guidelines to the relevant data


In a legal trial, the court determines whether the defendant's actions violated the law. The laws and policies serve as the set of guidelines, while the facts of the case provide the information relevant to the action in question. The mechanism that applies these laws and policies to the facts of the case would be the judge or jury.


Coming back to software, an authorization decision requires similar information:


- A permissions model that defines the logic and rules governing allowable actions
- Data pertinent to the decision being made, either from a datastore or provided at runtime
- Engine capable of applying logic and rules to provided data and returning a decision


These components make up a **permissions system** . The system itself may be wholly contained outside of an application, be dispersed throughout a monolithic application, or some combination of both. A system might have multiples of a component, such as multiple data sources or permissions models.


Applying SpiceDB concepts to this terminology, a SpiceDB permissions system uses a schema to structure relationship data, creating a graph database capable of returning answers about authorization.


The permissions system concept not only applies to other Zanzibar implementations, but any other system that may follow a different paradigm.


## Authorization service


While a permissions system can function independently to provide authorization to an application, an **authorization service** provides a layer of abstraction on top of a permissions system.


This service manages the business logic of authorization and permissions systems while also providing additional operational functionality such as caching decisions, auditing and logging, and managing supportive resources.


Used in context, the AuthZed Cloud authorization service deploys, manages, and provides additional operational capabilities for SpiceDB permissions systems. Other organizations choose to build their own authorization service for their SpiceDB permissions systems within their own infrastructure.


By establishing a clear and consistent lexicon for implementing authorization, we foster a shared understanding that allows us to compare disparate approaches, evaluate tradeoffs with precision, and ultimately architect more robust and secure collaborative workflows and sharing capabilities within applications.


## Glossary


**Permissions** establish and reason about the rules and logic that govern permissible actions, defining the boundaries of what can and cannot be done under specific conditions and contexts.


**Authorization** is the process of answering questions about permissions.


**Permissions systems** construct a mechanism for defining, storing, and evaluating permissions.


**Authorization services** provide a layer of abstraction on top of permission systems that provide answers to an end consumer, such as an application.


## FAQs about Building Permissions Systems


### What is the difference between permissions and authorization?


Permissions and authorization are distinct concepts that are often confused but serve different roles within an application's security architecture. Permissions refer to the static rules and logic that define the boundaries of what is allowed under specific conditions, effectively establishing the laws of the system. Authorization is the active process of evaluating those permissions against a specific request to determine if an actor is permitted to perform a requested action.


### What are the core components of a permissions system?


A robust permissions system is composed of three essential elements: a model that defines the guidelines for allowable actions, relevant data regarding the actor and the object, and an engine capable of applying the rules to that data. The engine processes the input against the defined model to return a clear decision on whether access should be granted or denied. This structure ensures that decision-making logic is consistent and separable from application business logic.


### How does an authorization service differ from a permissions system?


While a permissions system handles the fundamental logic of defining and evaluating access rules, an authorization service acts as an abstraction layer that sits on top of that system. An authorization service manages the operational aspects of security, such as caching decisions for performance, maintaining audit logs for compliance, and handling the deployment of the underlying permissions engine. This separation allows developers to manage business logic independently from the infrastructure required to scale authorization.


### Why is using a graph-based model effective for permissions?


Graph-based permissions models, often inspired by scalable paradigms like Google's Zanzibar, structure authorization data based on the relationships between users and resources. This approach is highly effective because it allows systems to traverse complex hierarchies and nested relationships quickly to answer authorization queries. By using a graph database, developers can model intricate organizational structures and collaborative features more naturally than with traditional flat role-based lists.


### Should I build a permissions system or use an authorization service?


Building a permissions system from scratch requires significant engineering resources to ensure high availability, low latency, and security compliance. Utilizing a dedicated authorization service allows teams to offload the complexity of maintaining the infrastructure, caching, and database consistency required for scaling permissions. This approach enables developers to focus on core product features while ensuring that their application's access control remains robust and standard-compliant.


Originally published August 7, 2024: Added FAQs section


On this page


- Why words matter
- TL;DR
- Semantics
- Permissions systems
- Authorization service
- Glossary
- FAQs about Building Permissions Systems
- What is the difference between permissions and authorization?
- What are the core components of a permissions system?
- How does an authorization service differ from a permissions system?
- Why is using a graph-based model effective for permissions?
- Should I build a permissions system or use an authorization service?


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
