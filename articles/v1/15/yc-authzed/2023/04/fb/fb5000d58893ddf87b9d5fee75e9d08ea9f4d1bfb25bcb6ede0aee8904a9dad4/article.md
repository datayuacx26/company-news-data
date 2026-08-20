---
schema_version: "1.0.0"
document_id: "fb5000d58893ddf87b9d5fee75e9d08ea9f4d1bfb25bcb6ede0aee8904a9dad4"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/pitfalls-of-jwt-authorization"
published_at: "2023-04-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:25f5ea8db5a3ee590a6488ab5a69afef194085c53849390f9c3d19dcaee3da51"
---

# Pitfalls of JWT Authorization

At AuthZed, we believe there’s a time and place for every piece of technology; the tricky part is determining if your use case actually is the time and place. For many years, there’s been a strong argument by domain experts against using JWTs for web sessions. While this campaign has succeeded to help improve the security of the web frontend, there hasn’t been an equivalent campaign for the backend. While building[SpiceDB](https://github.com/authzed/spicedb) , we’ve surveyed many backend developers only to find that many don’t know the pitfalls of JWTs or even that alternatives exist. SpiceDB is an open source project that implements one such alternative called *centralized authorization* . Because of this, I’ll be sure to include exactly how a centralized strategy accounts for the pitfalls with JWTs, too!


## Refresher: What is a JWT?


JWT (often pronounced “jot”) stands for JSON Web Token (JWT). JWTs are part of a set of specifications for encrypting and signing JSON called[JOSE](https://datatracker.ietf.org/wg/jose/documents/) . Each component has its own specification; JWT is defined in[RFC7519](https://www.rfc-editor.org/rfc/rfc7519) . How come no one ever talks about the other JOSE components (e.g.[JWA](https://www.rfc-editor.org/rfc/rfc7518) ,[JWE](https://www.rfc-editor.org/rfc/rfc7516) ,[JWK](https://www.rfc-editor.org/rfc/rfc7517) ,[JWS](https://www.rfc-editor.org/rfc/rfc7515) )? Because JWT stands out from the other components—it’s the payload format that can be optionally protected with encryption strategies specified by the other JOSE[components](https://www.rfc-editor.org/rfc/rfc7520) . At the end of the day, JWT is an encoded JSON object with some extra metadata (headers and a signature) to ensure integrity. The properties on this object are called “claims” and some are standardized for declaring when the token expires (` exp` ), who issued the token (` iss` ), who the audience token was (` aud` ), and more. The most common claim used for authorization is called` scope` and actually doesn’t even come from any JOSE specification; it comes from the specification for[OAuth2 Token Exchange](https://www.rfc-editor.org/rfc/rfc8693#name-scope-scopes-claim) . Backend developers often piece together the most useful combination of these ideas to get something that works for them.


## JWTs Don’t Pass the[New Enemy Problem](https://authzed.com/blog/new-enemies/) Test


JWTs have a fundamental flaw: they can't be revoked when their claims or scopes expire for reasons other than the expiration stored in the JWT. If you’ve read about JWTs usage for web sessions, you might already be familiar with the issue. For web sessions, the inability to revoke a JWT leads to insecure implementations of “logging out” of a web application. Because you cannot revoke existing JWTs that are valid before the “log out”, you’re forced to wait until they expire or hope that the client faithfully discards any old JWTs when they perform a “log out”.


For backend services using JWTs for authorization, the inability to revoke JWTs can become equally, if not more dangerous. Because you cannot revoke a JWT, a user could have their access revoked on the server, but still have an old-but-valid JWT that contains a claim for accessing content they no longer should be able to access. The ability to use a stale authorization token to access newly restricted data is called the[New Enemy Problem](https://authzed.com/blog/new-enemies/) . This scenario was first described in[Google’s Zanzibar paper](https://zanzibar.tech/2Pb5itZdZ:0:L) and is a litmus test for the security of authorization schemes. Centralized authorization strategies solve this problem by having a central service that is contacted to perform permission checks. This service is used to provide a *consistent* view of permission changes to enforce consistency and protect against the New Enemy Problem. This is a really hard and interesting distributed systems problem and is worth reading about on its own:[The New Enemy Problem](https://authzed.com/blog/new-enemies/) .


## JWT Scopes Can’t Provide Fine-Grained Authorization


While the original claims for JWTs are well specified, everything else is actually quite murky. Remember that OAuth2` scope` claim? It’s actually quite vague. The specification where` scope` is defined is actually in[yet another spec](https://www.rfc-editor.org/rfc/rfc6749#section-3.3) and it doesn’t offer much additional context other than some requirements around what characters form valid strings. If you read through all of the specifications, you’ll find examples of values for this claim like` email profile phone address` . At a glance this might seem reasonable; it’s listing properties on a user’s profile. Developers typically add some more context (e.g.` profile:admin` ) and call it a day. However, this *still* lacks context. What data exactly are we talking about here? The whole website? Only the current user’s profile? You can easily find high-profile web apps out in the wild that have done this; it has plagued GitHub’s REST API for years!


Modern applications need Fine-Grained Authorization down to individual items (e.g.` issue/authzed/spicedb/52:author` instead of just` issue:author` ). Considering users can have access to thousands, millions, or even billions of software objects; it is unrealistic to think they can all fit into an access token that is passed from service to service over a network.


On the other hand, centralized authorization keeps any data required for authorization in one (central) location and allows services to query specific, granular, and up-to-date authorization data on-demand. By decoupling the data, you are free to scale your authorization system as needed. SpiceDB specifically is built upon a ReBAC (Relationship-Based Access Control) foundation that can effectively scale to extraordinarily fine-grained data while also being powerful enough to model popular design patterns such as RBAC, ABAC, and User-Defined Roles. If you're now scratching your head wondering how fine-grained your data model should be,[we've got a whole post on it](https://authzed.com/blog/how-small-is-too-small/) .


## JWT Authorization Requires an Oracle to Predict Necessary Scopes


Let's pretend that you could get away with using very few JWT scopes. You'll eventually hit a wall, but let’s just do so anyway for the sake of argument. How do you know what scopes need to be in your JWT for a given request? JWTs are often created by an ingress or API gateway at the beginning of a request flowing into a service-oriented architecture, so that means the creator of the JWT needs to know exactly what services are going to be downstream of the request and what permissions they might require. This is impossible to predict for anything outside of very simple systems, but if you’re already split into microservices it’s very unlikely that your model is simple.


Additionally, any token with more scopes than necessary that is passed along to the next service is a huge target for attackers attempting to escalate privileges and gain access to sensitive data. This exact problem led to the creation of an alternative token to JWTs called[Macaroons](https://blog.gtank.cc/macaroons-reading-list/) , which one of the AuthZed engineers,[Evan Cordell](https://evancordell.com/posts/macaroons-101-contextual-confinement/) , built for the Python ecosystem. Macaroons allow for tokens to be attenuated to reduce their scope before being shared with the next service. Sounds great in theory, but in practice, properly using Macaroons is so complex that there hasn’t been much adoption and those that have adopted them[often argue it wasn't worth it](https://youtu.be/MZFv62qz8RU) .


Centralized authorization systems acknowledge that it's impossible to know ahead of time all the permissions a request will need in order to be processed. Instead, they allow for ad-hoc queries when necessary. This does have overhead of reaching out to the centralized system, but systems like SpiceDB are optimized to keep data in-memory, so latency looks similar to reaching out to any other cache like redis or memcache.


## When is JWT Authorization Actually Recommended?


After reading all this, you might be thinking that there really isn’t *any* time or place for JWTs! However, there is one recommendation that is often repeated: one-time grants where access cannot be revoked. I'm convinced that this is an exceedingly rare situation, nonetheless, it is a valid one.


If you'd like to learn more about topics like ReBAC and centralized authorization, feel free check out the rest of our blog.


- [What is Google Zanzibar - Jake Moshenko](https://authzed.com/blog/what-is-google-zanzibar/)
- [New Enemies - Jake Moshenko](https://authzed.com/blog/new-enemies/)
- [Zanzibar Implementations - Sam Kim](https://authzed.com/blog/zanzibar-implementations/)
- [Google Zanzibar Through Our Eyes - Jake Moshenko](https://authzed.com/blog/annotated-zanzibar-launch/)
- [The Architecture of SpiceDB - Jake Moshenko](https://authzed.com/blog/spicedb-architecture/)
- [Caveats: A Scalable Solution for Policy - Jimmy Zelinskie](https://authzed.com/blog/caveats/)
- [How small is too small? - Jake Moshenko](https://authzed.com/blog/how-small-is-too-small/)


For additional content covering JWTs that’s mostly (but not exclusively) about web sessions, I recommend the following:


- [API Tokens: A Tedious Survey - Thomas Ptacek](https://fly.io/blog/api-tokens-a-tedious-survey/)
- [JSON Web Tokens Suck - Randall Degges (DevNet Create 2018)](https://youtu.be/JdGOb7AxUo0)
- [No Way, JOSE! Javascript Object Signing and Encryption is a Bad Standard That Everyone Should Avoid - Scott Arciszewski](https://paragonie.com/blog/2017/03/jwt-json-web-tokens-is-bad-standard-that-everyone-should-avoid)
- [Stop using JWT for sessions - Sven Slootweg](http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/)


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)


On this page


- Refresher: What is a JWT?
- JWTs Don’t Pass the New Enemy Problem Test
- JWT Scopes Can’t Provide Fine-Grained Authorization
- JWT Authorization Requires an Oracle to Predict Necessary Scopes
- When is JWT Authorization Actually Recommended?
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
