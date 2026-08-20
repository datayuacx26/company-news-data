---
schema_version: "1.0.0"
document_id: "3e2a85f695750edb72832195f1aaa046dc9ab9f29ddf110c50a34bb7c3beae40"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/enforcing-graphql-security-best-practices-with-graphos"
published_at: "2023-10-05T14:44:53+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:90173014605589db56f7aa91321cb3351bb5665943c65671b69566d2d64af17a"
---

# Enforcing GraphQL security best practices with GraphOS

GraphQL provides a self-service developer experience by enabling client teams to fetch all of the data they need with a single query. When implementing GraphQL at scale, it’s important to balance its flexibility with security measures that prevent bad actors from exploiting its self-serve nature.


A supergraph provides a unified but modular approach to GraphQL at scale. Not only does this simplify data fetching for client teams – it has immense benefits around visibility, security, and scalability for the platform teams who support these APIs. By using GraphOS, platform teams can block malicious traffic at the edge of the graph, define both cross-cutting and fine-grained security policies across any number of services, and provide defense in depth.


If you’re running a supergraph with Apollo GraphOS Enterprise, here are three ways you can use GraphOS to better secure the services in your GraphQL API:


## Centralize authorization in the router


The Apollo Router provides client teams with a central access point for any number of GraphQL services. This isn’t just convenient for client teams – it also provides a policy enforcement point (PEP) for security teams.


Rather than relying on your services to block unauthorized access, GraphOS allows you to proactively and declaratively enforce authentication and authorization centrally in the router for your supergraph. Enforcing authorization in the router reduces the request load on your services by blocking unauthorized requests at the edge of the API stack and eliminates the need for any custom APIM code used to centralize enforcement.


To set up authz enforcement in your router:


1. Ensure that your router is[authenticated with your GraphOS Enterprise credentials](https://www.apollographql.com/docs/router/enterprise-features/#enabling-enterprise-features)
2. Configure your router to add claims to requests’ contexts with the Apollo Router’s built-in support for[JWT authentication](https://www.apollographql.com/docs/router/configuration/authn-jwt) or with an[external coprocessor](https://www.apollographql.com/docs/router/customizations/coprocessor#adding-authorization-claims-via-coprocessor) .
3. Create a declarative access control model in your schema with the` @requiresScopes` and` @authenticated` directives


## Safelist registered operations


One of the most common objections to GraphQL is that it allows unbounded operations — a trait that can degrade performance or result in denial-of-service if too many complex queries are sent to the endpoint. But in the case that your GraphQL endpoint only serves first-party apps, there is no need for it to allow unbounded operations. Instead, you should reduce the surface area of your graph by only allowing the exact operations required by your applications.


GraphOS allows you to do exactly that by registering and safelisting operations that you trust. Create a list of registered operations in GraphOS Studio, and turn on safelisting in your router to block any non-registered operations. To further improve performance, you can also generate unique IDs for each operation and send only the ID in the request to your router rather than the full query string.


## Limit operation complexity


In addition to safelisting operations, it’s also prudent to enforce general complexity limits on operations to prevent any intentional or unintentional misuse of your graph. For example, allowing operations with thousands of root fields or aliases is neither efficient nor necessary.


With GraphOS, you can implement simple limits on operation depth, height, aliases, and root fields to prevent malformed operations from degrading performance.


## Learn security best practices at GraphQL summit


Join API architects, engineers, and experts from Apollo at[GraphQL Summit](https://summit.graphql.com/event/c51538f6-4b76-44e3-871e-54180c77cad8/websitePage:aa37bf00-dba5-4e19-912a-01aad46f7426) October 9-12, 2023 to discuss real-world GraphQL security challenges and best practices. If GraphQL security is your thing, these are a few sessions you won’t want to miss:


### Free GraphQL security consultations with Doyensec


Doyensec, the premier GraphQL security consultancy, will be onsite at GraphQL Summit! Stop by on Wednesday, October 11 and Thursday, October 12 for a free consultation to discuss technical security recommendations and engineering best practices with the experts who led the Apollo Router security audits.


### GraphQL Security Best Practices and Hardening


This talk will discuss, in depth, how to defend against hackers conducting Reconnaissance, Denial of Service, Information Disclosure, Authentication and Authorization Bypass, Injection, Request Forgery and Hijacking, XSS, SQLi and Code Execution.


### Understanding GraphQL Security: Protecting Your Server from Abuse


GraphQL’s “ask for what you need and get exactly that” is great, but it can also be exploited to “get what they want without your permission and crash your app”. To protect your GraphQL server, it is important to understand the different ways in which it can be abused.


### Supergraph AuthZ Security in Apollo Gateway and Router with External Co-processing


In an enterprise supergraph like T-Mobile’s, clients will require fine-grained permissions for different operations and fields. With the flexibility in Apollo Gateway and Router to add external co-processing of requests, attribute-based access control (ABAC) and other customizations can control client traffic at the supergraph entry point.


### Observability and GraphQL


This lab will provide a hands-on tour of observability tools that can be configured in a supergraph. You’ll learn how to observe the behavior and performance of your supergraph using both GraphOS reporting and cloud-native telemetry.


**For a full list of talks on GraphQL security and more, check out the**[GraphQL Summit agenda](https://summit.graphql.com/event/c51538f6-4b76-44e3-871e-54180c77cad8/websitePage:aa37bf00-dba5-4e19-912a-01aad46f7426) **. We hope to see you there!**


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
