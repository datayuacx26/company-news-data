---
schema_version: "1.0.0"
document_id: "07dcba1a58c0906db4ef16c32e7411f2f548cc6dfced3f306f4cc02941055494"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/build-time-bound-permissions-with-relationship-expiration-in-spicedb"
published_at: "2025-02-13T10:16:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:25d1cff957e354fced418b3b258cfefebe7a25075476d1756ac5397e76749b75"
---

# Build Time-Bound Permissions with Relationship Expiration in SpiceDB

Today we are announcing the experimental release of Relationship Expiration, which is a straightforward, secure, and dynamic way to manage time-bound permissions directly within SpiceDB.


Building secure applications is hard, especially when it comes to implementing temporary access management for sensitive data. You need to grant the right level of access to the right people for the right duration, without creating long-term vulnerabilities or drowning in administrative overhead.


Consider the last time you needed to give a contractor access to your brand guidelines, a vendor access to a staging environment, or a new employee access to onboarding materials. The usual workarounds – emailing files, uploading to external systems, or ( *please, please don’t* ) sharing logins – quickly become a tangled mess of version control nightmares, security risks, and administrative headaches. And what happened when you completed the project? How did you guarantee that access gets promptly revoked? Leaving lingering access privileges hanging around is an AppSec war room waiting to happen.


We’re helping application development teams solve this problem with this powerful new feature in SpiceDB v1.40.


*"Authorization is essential for building secure applications with advanced sharing capabilities,"* said Larry Carvalho, Principal Consultant and Founder at RobustCloud. *"SpiceDB, inspired by Google's approach to authorization, provides developers with a much-needed feature for managing fine-grained access control. By leveraging AuthZed’s expertise, developers can build the next generation of applications with greater efficiency, security, and flexibility."*


## Beyond workarounds: a first class solution


While workarounds exist – scheduling API calls with external tools like Temporal or crafting complex policies – they add complexity and can be difficult to manage and deploy at scale (think 10,000 relationships generated and refreshed every 10 minutes). SpiceDB's Relationship Expiration provides first-class support for building time-bound permissions, leveraging SpiceDB’s powerful relationship-based approach.


As the name suggests, expirations are attached as a trait to relationships between subjects and resources in SpiceDB’s graph-based permissions evaluation engine. Once the relationship expires, SpiceDB automatically removes it. Without this built-in support, conditional time-bound relationships in a Zanzibar-style schema clutter the permissions graph, bloating the system and impacting performance.


## Why you should be building time-bound permissions (with SpiceDB)


### Collaborate productively and securely


Time-bound access helps teams to collaborate securely and efficiently. By eliminating the friction of manual access management, it frees up valuable time and resources while minimizing the risk of human error. Knowing that access will automatically expire fosters a culture of confident sharing, removing the hesitation that can lead to information silos and slower project cycles. Additionally, just-in-time access with session-based privileges streamlines workflows and minimizes the risk of unauthorized access.


### Dynamic permissions


Put access control in the hands of your users: they can define expiration limits for the resources they manage, unlocking powerful workflows like time-limited review cycles or project-based access. A designer, for example, could grant edit access to a file for a specific review period, with access automatically revoked afterward. This granular control enhances security by minimizing the window of opportunity for unauthorized access and fosters a culture of security awareness. Leave a positive impression with custom permissions options that welcome a broad range of use cases.


### Optimize permissions systems


With millions of users and billions of resources, authorization can become a major performance bottleneck, especially since permissions checks sit in the critical path between user input and service response. By automatically removing expired relationships, SpiceDB reduces the size of its database and load on its system, leading to more performant authorization checks and lower costs.


## Learn more today


Want to learn more TODAY? Join Sohan, AuthZed technical evangelist, and Joey Schorr, one of the founders of AuthZed, during our biweekly Office Hours livestream at 9 am PT / 12 pm ET on February 13th! We hope to see you there.


Or, hop over to Jimmy Zelinskie’s[blog post](https://authzed.com/blog/the-evolution-of-expiration) to learn more about how to implement expiring relationships and try them out in SpiceDB today.


## Don’t let relationships linger past their expiration date!


You may have noticed that we've lined up this launch just in time for Valentine’s Day. Most relationships between humans do, sadly, have an expiration date… To recognize the (somewhat) unfortunate timing of this release, we’ve compiled a Spotify list of songs sourced from the AuthZed team just for those nursing broken hearts this season. And if you’re one of the lucky ones celebrating, hey, it’s fun music to jam to while you learn SpiceDB.


If you haven’t already, give SpiceDB a[star](https://github.com/authzed/spicedb) on GitHub, or follow us on[LinkedIn](https://www.linkedin.com/company/73024037/) ,[X](https://x.com/authzed) , or[BlueSky](https://bsky.app/profile/authzed.com) to stay up to date on all things AuthZed. Or ready to get started?[Schedule a call](https://authzed.com/call) with us to talk about how we can help with your authorization needs.


On this page


- Beyond workarounds: a first class solution
- Why you should be building time-bound permissions (with SpiceDB)
- Collaborate productively and securely
- Dynamic permissions
- Optimize permissions systems
- Learn more today
- Don’t let relationships linger past their expiration date!


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
