---
schema_version: "1.0.0"
document_id: "488bc1686dfd787b88bc2eed08532690d3e4f38ed6e61d7e3566f0c8ddea93d4"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/abac-example"
published_at: "2024-03-26T10:30:54.256+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:b093e3023617219a2a6ba4973f6baabd16342ef99bb291444b9e32a32d8db331"
---

# ABAC by example: basic permissions and dynamic access control

Selecting the correct system for managing access is critical to the overall security of any application being developed today: broken forms of access control are the number one issue on[OWASP’s Top Ten list](https://owasp.org/www-project-top-ten/) of security concerns for application and system developers.


Over the past few years, a number of approaches to modeling access have become standard, including role-based access control (RBAC), relationship-based access control (ReBAC) and, the focus of this post, attribute-based access control (ABAC). While each approach has its positives and negatives, this post will discuss the benefits of ABAC and how, with SpiceDB, ABAC can be combined with ReBAC to provide the benefits of both, with little to no downside.


## What is ABAC?


Attribute-based access control, or “ABAC,” is a system for access control that uses attributes found on objects alongside a set of rules or functions to determine how to secure access and how permissions should be granted. ABAC is often also referred to as a policy engine for permissions.


Unlike role-based access control, standard ABAC allows for defining almost arbitrary sets or categories of information to use for making decisions.


Unlike relationship-based access control, standard ABAC defines the attributes on the objects themselves, rather than as relationships between objects.


## An ABAC example: document attribute rules


To define whether a user can view a document using an ABAC approach, the following attributes might be defined on the document:


Attribute Name Attribute Value


viewers \[user1, user2, user3, …\]


editors \[user1, user2, user3, …\]


public_days \[tuesday\]


In the above table, the multiple attributes of “viewers” and “editors” are being used to define roles (in a way similar to RBAC), while “public_days” is being used to add a custom role, which is then enforced using the following policy to check whether a particular user has access to view the document:


1


2


3


```text
func can_view_document(document: Document, user: User) {
current_week_day in public_days or user.id in document.viewers or user.id in document.editors
}


```


1


2


3


```text
func can_view_document(document: Document, user: User) {
}


```


Here we can see how ABAC is used to enforce access: the attributes (or data) defined on resources or objects or subjects are used to enforce the rules (or a policy) to make a permissions decision, including whether the document is public today, and, if not, how a user may have access to the document.


In essence, we are executing a program, defined in a custom language over a subset of data to return a single result: true if the user has the requested permission.


## ABAC is everything?


Attribute-based access control is considered by some to be a superset of other approaches to access control: after all, while it can simulate RBAC (as we saw above), RBAC and ReBAC cannot, in turn, operate on attributes. Many therefore assume that they should just use ABAC since it can, in theory, do anything that other forms of access control can do.


This idea rapidly breaks down once scale and performance requirements are introduced: Since ABAC is running code, it is quite difficult to effectively scale while maintaining performance guarantees. This was one of the major reasons, in fact, that Google invented Zanzibar as a ReBAC solution: it was the only design capable of addressing these issues over Google’s existing policy-based access control.


So we should not use ABAC for everything, but we also (at times) would like to have the ability to make permission decisions based on dynamic or mutating data… how might we approach combining the best features of ABAC (custom policy) with those of ReBAC?


## An example of conditional ABAC


Recognizing the above problem, SpiceDB invented and introduced the concept of[caveats](https://authzed.com/docs/spicedb/concepts/caveats) : ABAC applied to specific subsets of permissions decisions, such that we can leverage the flexibility of attribute-based decision making with the power and efficiency of Zanzibar.


Taking the previous ABAC example used above, we can translate this into SpiceDB’s schema like so:


zed


1


2


3


4


5


6


7


8


9


10


11


12


```text
definition    user    {  }


caveat   is_public_today (  current_week_day string, public_days list<string> )    {
current_week_day in public_days
}


definition    document    {
relation    viewer  :  user     |    user  :*    with   is_public_today
relation    editor  :  user


permission   view  =   viewer  +   editor
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


```text
definition    user    {  }


current_week_day in public_days
}


definition    document    {
relation    viewer  :  user     |    user  :*    with   is_public_today
relation    editor  :  user


permission   view  =   viewer  +   editor
}


```


Here, we can see the basic roles of “viewer” and “editor” are defined in the SpiceDB schema as standard relationships between the document and the user(s). The new addition is the “with is_public_today”, which states that the a relationship between a document and “user:*” (indicating the document is public) will only exist if is_public_today returns true: we’ve caveated the existence of the public relationship on a tiny ABAC-like piece of policy called “is_public_today.”


## The differences between Caveats and typical ABAC


Like ABAC, the attributes used to compute the result, in this case “public_days”, can be stored. Unlike ABAC, however, it is stored not on the document itself, but on the relationship written between the document and the subject. This allows for more flexibility than even ABAC, as different relationships can contain different data!


The real benefit, however, comes in how these checks are computed: like checks without caveats, SpiceDB can make use of parallelization, structural graph walking and event[aggressive caching](https://authzed.com/blog/how-caching-works-in-spicedb) to compute answers. SpiceDB caches the computed caveat expressions themselves, ensuring that the cache can even be used when the incoming context changes!


With caveats you therefore get all the power of ReBAC and Zanzibar, but with most of the flexibility of a traditional ABAC system!


## Three types of conditional ABAC patterns


Each caveat evaluates a small piece of context, such as a timestamp or IP address, and returns true only when its conditions are met. This makes it possible to keep the structure and scalability of a ReBAC system while still using attribute-driven checks where they matter most.


Here are three[examples of common patterns.](https://authzed.com/blog/top-three-caveat-use-cases)


### 1. Time-based access (temporal ABAC)


You can condition access on time-related attributes such as when a permission was granted and how long it should remain valid:


zed


1


2


3


```text
caveat   temporal_grant (  grant_duration duration, current_timestamp string, grant_timestamp string )    {
timestamp (  current_timestamp )    -   timestamp (  grant_timestamp )   < grant_duration
}


```


zed


1


2


3


```text
}


```


Applied to a relation:


zed


1


```text
relation    reader  :  user     with   temporal_grant


```


zed


1


```text
relation    reader  :  user     with   temporal_grant


```


This ABAC pattern checks duration-based attributes to decide whether access is still valid.


### 2. IP-restricted access


Caveats can enforce rules based on network attributes such as allowing access only when the client IP falls within an approved CIDR range. This lets you embed an allow list as an ABAC condition and create network-aware access rules without baking them into application code.


### 3. Session or token-bound access


You can require that certain attributes on an incoming session or token, such as an aud claim or session ID, match expected values. This ABAC pattern ties the permission decision to identity or session attributes rather than static roles and allows access to adapt to changing context.


SpiceDB caveats let you use ABAC-style checks in a focused way while still taking advantage of the structure and performance of a relationship-based system.


## A real-world ABAC example: Netflix’s complex identity model


You can build an ABAC rule inside a graph-based permissions engine. This came in handy when[Netflix needed to support application-identities with rich attributes](https://authzed.com/blog/abac-on-spicedb-enabling-netflix-complex-identity-types) when authorizing access to sensitive resources. This includes attributes like region, environment, account, and stack. Using SpiceDB,[they combined their attribute-based requirements with the graph-based relationships](https://authzed.com/customers/netflix) typical of ReBAC.


In this scenario, they defined a caveat such as:


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


```text
caveat   match_fine (
expected_accounts list<string>,
expected_regions list<string>,
expected_stacks list<string>,
...
observed_account string,
observed_region string,
observed_stack string,
...
)    {
observed_account in expected_accounts  &  &
observed_region in expected_regions  &  &
observed_stack in expected_stacks  &  &   …
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


```text
caveat   match_fine (
expected_accounts list<string>,
expected_regions list<string>,
expected_stacks list<string>,
...
observed_account string,
observed_region string,
observed_stack string,
...
)    {
observed_account in expected_accounts  &  &
observed_region in expected_regions  &  &
observed_stack in expected_stacks  &  &   …
}


```


On top of that, they created a relationship such as:


zed


1


2


3


4


```text
definition    movie    {
relation    replicator  :  app     with   match_fine
permission   replicate  =   replicator
}


```


zed


1


2


3


4


```text
definition    movie    {
relation    replicator  :  app     with   match_fine
permission   replicate  =   replicator
}


```


This effectively models ABAC-like logic (matching attributes like region and stack) within a relationship-based framework by letting the relationship between the user/application and the resource carry the attribute check (“with match_fine”).


The result? They got the flexibility of ABAC (checking attributes dynamically) with the scalability and efficiency of a ReBAC engine.


### Next Steps


Explore our managed service:[AuthZed Cloud](https://authzed.com/products/authzed-cloud) . Set up quickly, scale easily.


**Related reading:**


- [“Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System”](https://authzed.com/blog/exploring-rebac)
- [“Fine-Grained Access Control: Can You Go Too Fine?”](https://authzed.com/blog/fine-grained-access-control)
- [“Policy-Based Access Control (PBAC) vs Google Zanzibar: When You Should Use One or the Other”](https://authzed.com/blog/policy-based-access-control)


## FAQs about ABAC


### What advanced access control model considers various attributes to make decisions?


Attribute-Based Access Control (ABAC) is the model that evaluates subject, resource, operation, and environmental attributes together to reach an authorization decision. Unlike role-centric approaches, ABAC handles fine-grained, context-sensitive rules that scale well across complex systems.


[SpiceDB](https://authzed.com/docs/spicedb/getting-started/discovering-spicedb) implements ABAC-style logic through[caveats](https://authzed.com/docs/spicedb/concepts/caveats) , which are CEL expressions bound to relationships and evaluated at request time. Static attributes are best modeled directly in the permission graph per AuthZed's[attributes guidance](https://authzed.com/docs/spicedb/modeling/attributes) , reserving caveats for dynamic context like IP or time-of-day. Netflix uses this approach for complex identity matching, as detailed in this[AuthZed case study](https://authzed.com/blog/abac-on-spicedb-enabling-netflix-complex-identity-types) .


Teams already using OPA can keep SpiceDB as the permissions source of truth while delegating broad policy logic elsewhere. AuthZed's[OPA migration guide](https://authzed.com/docs/spicedb/getting-started/coming-from/opa) clarifies that division. Centralizing permission logic and following AuthZed's[best practices](https://authzed.com/docs/best-practices) keeps authorization auditable and consistent.


### What is Attribute-Based Access Control (ABAC)?


Attribute-Based Access Control (ABAC) is a security model that grants or denies access permissions based on specific characteristics assigned to users, objects, and the environment. Unlike static role assignments, ABAC evaluates dynamic attributes—such as a user's department, a document's sensitivity level, or the current time of day—against a defined set of policy rules to make real-time access decisions.


### Why is scaling traditional ABAC difficult for performance?


Standard ABAC models often encounter performance bottlenecks at scale because they rely on executing complex code or rules for every permission check in real-time. As the number of attributes and access requests grows, this computational overhead makes it challenging to maintain the low-latency guarantees required by high-performance applications. A more structural approach to access control is often required.


### How do SpiceDB Caveats solve ABAC scalability issues?


[SpiceDB Caveats](https://authzed.com/docs/spicedb/concepts/caveats) bridge the gap between flexibility and performance by allowing developers to attach attribute-based logic to specific relationships within a[ReBAC](https://authzed.com/blog/exploring-rebac) graph. This hybrid approach leverages the massive scalability and caching capabilities of the Zanzibar architecture for the majority of the check while applying dynamic ABAC rules only where necessary, ensuring that permission systems remain both fast and context-aware.


### Can ABAC be combined with Relationship-Based Access Control (ReBAC)?


Yes, ABAC can be effectively combined with ReBAC to create a system that offers both structural efficiency and fine-grained control. In this model, the underlying access graph handles the relationships between users and resources, while conditional attributes—like checking if a document is public on a specific day—are enforced as caveats on those relationships, providing the best features of both access control paradigms.


### What is a practical example of using ABAC with Caveats?


A practical example of using ABAC with caveats involves managing temporary public access to a document based on the day of the week. In this scenario, a "viewer" relationship is established between the public and a document, but it is conditional upon a caveat that checks if the current day matches a "public_days" attribute. If the condition evaluates to false, access is denied even if the relationship exists in the graph.


Originally published March 26, 2024: Added a FAQs and ABAC patterns sections


On this page


- What is ABAC?
- An ABAC example: document attribute rules
- ABAC is everything?
- An example of conditional ABAC
- The differences between Caveats and typical ABAC
- Three types of conditional ABAC patterns
- 1. Time-based access (temporal ABAC)
- 2. IP-restricted access
- 3. Session or token-bound access
- A real-world ABAC example: Netflix’s complex identity model
- Next Steps
- FAQs about ABAC
- What advanced access control model considers various attributes to make decisions?
- What is Attribute-Based Access Control (ABAC)?
- Why is scaling traditional ABAC difficult for performance?
- How do SpiceDB Caveats solve ABAC scalability issues?
- Can ABAC be combined with Relationship-Based Access Control (ReBAC)?
- What is a practical example of using ABAC with Caveats?


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
