---
schema_version: "1.0.0"
document_id: "23283908007e21257e8b9ef99bde54fb72bcf3de38c28da22ece3c93d6f3b831"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authz-in-a-distributed-system"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:cfb3565bed72597c1dab2b10a2514ec3721585bc31861532ab24140ad46cd316"
---

# Authorization in a Distributed System

## Overview


Authorization in a distributed system carries a different set of challenges than authorization in a monolithic application. We'll talk about the authorization problems you'll face when moving from a monolith to a distributed system, three broad approaches you can take to solve those problems, and when you might reach for one over another.


## Setting the Table


Let's imagine you're working on a system like the one in the[Dual-Write Problem](https://authzed.com/blog/the-dual-write-problem) post, where the product is for sharing files, à la Dropbox or Box. Let's suppose there is a UI in the application with a list of files, and that list of files includes the owner of the file:


We can glean two product requirements from this: a user should be able to see a list of files they have access to, and they should be able to see some information about the users who own those files (the user's name and profile picture).


In a monolithic application, you have all of this information in your primary application database, and you'd be able to get all of the information you need in a single datastore query. Assuming you decide to model permissions with roles, a SQL query for this data might look something like this:


sql


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


15


16


17


```text
SELECT   (
-- Get some stuff about the file
files.id,
files.name,
files.description,
files.url,
-- Get some stuff about the user
users.name,
users.profile_url,
)
FROM   files
-- Only get the users that own the files in question
JOIN   users  on   files.owned_by  =   users.id
-- And only get the files where there's a many-to-many row  in an `access` table
-- corresponding to the querying user's access
JOIN   access  on   files.id  =   access.file_id
WHERE   (access.user_id  =   ?  AND   access.role  =    'viewer'  )  OR   files.owned_by  =   ?;


```


sql


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


15


16


17


```text
SELECT   (
-- Get some stuff about the file
files.id,
files.name,
files.description,
files.url,
-- Get some stuff about the user
users.name,
users.profile_url,
)
FROM   files
-- Only get the users that own the files in question
JOIN   users  on   files.owned_by  =   users.id
-- And only get the files where there's a many-to-many row  in an `access` table
-- corresponding to the querying user's access
JOIN   access  on   files.id  =   access.file_id


```


> A querying user is able to view details about another user if that other user owns a file that the querying owner can view.


But what happens when we decide to split out a File service from a User service? This seems like a relatively clean domain split, but now the authorization logic crosses domains. Information about who owns a file would be held in the File service, but we're also asking the User service for details about the user. How should the User service make decisions about whether a querying user is able to view a particular user?


To make it concrete, let's imagine that in the new system we're building, we have a GraphQL gateway sitting in front of our services, and it has a schema that looks something like:


graphql


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
type   User  {
id  :   string !
name  :   string
profile_url  :   string
}


type   File  {
id  :   string !
owner  :   User !
description  :   string
url  :   string !
}


```


graphql


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
type   User  {
id  :   string !
name  :   string
profile_url  :   string
}


type   File  {
id  :   string !
owner  :   User !
description  :   string
url  :   string !
}


```


The User service responds to requests for Users, and the File service responds to requests for Files. Note that the only handle that the File service has on Users is an ID that it holds for the` owner` .


When a request comes in, the resolver will ask the File service for a File, and if the calling user can view the File, the resolver will then have a User ID that it can use to query the User service.


This presents a problem, though: how does the User service know whether it should authorize a request for a given User's information? This particular example may be a little contrived, and there are some architectural assumptions we're making, but the general question still remains:


> How do you answer authorization questions when those questions span domain and service boundaries?


This is the motivating question, and roughly speaking there are three different approaches you can take.


## Every Service For Itself


The first is the naive approach: let every service implement its own authorization logic and collect the requisite information. In this model, the File service would maintain its own authorization logic and state for whether a calling user could see a File, and the User service would maintain its own logic and state for whether a calling user can see a User.


For example, we could have the User service reach out to the File service and ask "Are any files for which the calling user is a viewer and the given user is an owner?" using an RPC. This works, but if you're trying to minimize inter-service RPC traffic it's not ideal. It also means that the authorization logic in the User service is coupled to the File service's API, and ideally we want to minimize that kind of coupling.


You might instead use a pattern like[Event-Carried State Transfer](https://martinfowler.com/articles/201701-event-driven.html#Event-carriedStateTransfer) to let the User service maintain a relatively up-to-date view of the pieces of state from the File service that the User service would need to answer these questions. In this case, that would mean that the User service would copy over information about the File's ID, owner ID, and associated accesses into its own database, and then query its own database to figure out whether a user can view a particular User.


Notice that in either case, we're duplicating some authorization logic: both the File service and the User service need to implement the entire authorization query in their own code, regardless of how they're collecting the information required to answer that query. This means that there are two implementations, twice as many opportunities for bugs, and an opportunity for drift or disagreement between the services. In a system with more services this problem is only compounded.


Can we do better?


## Gateway Authorization


I'm using the term "gateway authorization" to refer broadly to approaches that figure out what a user has access to at the edge of your system and then annotate the request somehow with that information. One common implementation of this is RBAC through[OAuth 2.0 scopes](https://oauth.net/2/scope/) , where the authorization information is carried on the` scopes` claim of the JWT contained in the` Authorization` header of the request:


1


2


3


4


5


6


7


8


```text
{
"sub": "alice@mycompany.com",
"name": "Alice A",
"roles": ["admin", "editor"],
"scopes": ["file:123:owner", "user:bob:viewer"],
"iat": 1734623882,
"exp": 1734627482
}


```


1


2


3


4


5


6


7


8


```text
{
"sub": "alice@mycompany.com",
"name": "Alice A",
"roles": ["admin", "editor"],
"scopes": ["file:123:owner", "user:bob:viewer"],
"iat": 1734623882,
"exp": 1734627482
}


```


Once the scopes are attached to the JWT, there's not much work that the service receiving the request has to do to authorize the request: it simply checks whether the scopes provided in the token give access to a given resource. It works very well for Role-Based Access Control, especially if that access is relatively coarse (e.g. a user is the administrator of a workspace, or the viewer of a server).


In our system, you might construct scopes that look like` resource:id:role` , such as` file:123:owner` or` user:abc:viewer` . You would implement syncing logic so that the system minting the JWTs would have a list of these scopes available at request time, and that list would go in the JWT and the` User` and` File` service would look for their respective resources.


However, for a user with a sufficiently large number of files that can see a sufficiently large number of users, that JWT becomes unwieldy:


1


```text
scopes: [file:123:owner file:234:owner file:345:owner ... user:abc:viewer user:bcd:viewer user:cde:viewer ...(ad nauseum)]


```


1


```text


```


This will eventually carry significant network overhead. There are techniques that could reduce the size of the JWT, such as having the authorization service make guesses about which scopes out of the list of scopes will be necessary for a given request, but this adds complexity and coupling to the system, and in a system of sufficient size is more of a band-aid than a fix.


## Centralized Authorization Services


The third approach, and the one we recommend for distributed systems with fine-grained authorization requirements, is a Centralized Authorization Service. In this model, anytime a domain-level service has a question about whether a user can view a particular object, it makes a request to an Authorization service, and that service responds with a "yes" or "no" accordingly.


[SpiceDB](https://github.com/authzed/spicedb) is the authorization service we've built. In our opinion, the combination of a ReBAC core with an ABAC layer on top (implemented with[Caveats](https://authzed.com/docs/spicedb/concepts/caveats) ) strikes a good balance between flexibility, safety, and performance.


For the example we're working with, a schema may look something like this:


Remember that this is the logic we want to express:


This logic is expressed in our schema in the` view` permission:


- A user can view themselves, and
- A user can be the owner of a file, and that file can be viewable by another user.


This requires some data to be fed into SpiceDB: whenever a file is added, modified, or deleted, there are two relationships that need to be created or deleted accordingly. If it's` file:123` and it's owned by` user:alice` , those two relationships are:


1


2


```text
user:alice#owner_of@file:123
file:123#owner@user:alice


```


1


2


```text
user:alice#owner_of@file:123
file:123#owner@user:alice


```


Once that's done, to determine whether a request by` bob` to the User service should return Alice's information, the User service can issue a request to SpiceDB:


1


```text
user:alice#view@user:bob


```


1


```text
user:alice#view@user:bob


```


That's it! The User service doesn't need to know anything directly about the File service's state, because that's held and handled by SpiceDB.


You make some tradeoffs when you centralize like this, namely that you need to solve a[Dual Write Problem](https://authzed.com/blog/the-dual-write-problem) when feeding data into SpiceDB, and that the service becomes a single point of failure. However, if you're already in a distributed system, you probably already have the Dual-Write problem as a result of business logic crossing domains, and in a manner similar to authentication, authorization becoming a single point of failure should be weighed against the benefits of centralizing the logic.


When you centralize your authorization logic, your entire system gets a coherent and consistent view of a given user's authorization status for a given resource. You can't have conflicting results from different services or privilege escalation as a result of different systems disagreeing about a user's privileges.


Your calling services get a simpler handle on authorization as well. There's a single implementation of your authorization logic, represented by your schema, and the calling service can ask a simple question: "Can this user perform this action on this resource?" and it doesn't need to know any of the intermediate details. This means that you can refactor your authorization logic more easily: adding a layer to your ReBAC logic or refactoring a permission often requires changes to at most one service.


Note that on some level SpiceDB isn't special here: any service that can play the role of a centralized authorization service could work. However, we do think that if a centralized authorization service makes sense for your system, SpiceDB is a good choice:


- Compared to a hand-rolled system, SpiceDB already exists and is ready to use. It also encodes patterns and principles that we've arrived at through years of iteration.
- Compared to a Policy Engine like Open Policy Agent, your services get to ask simpler questions. A policy engine would let you ask "given this information that I have about these users and their files, can` bob` view` alice` ?" But this still requires your services to collect that information before submitting the question to the Policy Engine, which takes precious milliseconds and can be error-prone.
- Compared to other Zanzibar implementations like OpenFGA, SpiceDB offers better performance and scalability ceilings, and is the only implementation where a version of the Zanzibar paper's[Leopard Cache](https://authzed.com/zanzibar/2IoYDUFMAE:0:T) is currently available, which becomes necessary for use cases like authorized search.


## To Sum Up


We've seen that authorization becomes a difficult problem when building a distributed system. A relationship-based access system like SpiceDB is a powerful and flexible tool in solving this problem. If this sounds like a tool you'd like to try out, we'd encourage you to check out our[Playground](https://play.authzed.com/) , and if you'd like to see how it works in practice, check out our[Cloud Platform](https://authzed.com/products/authzed-cloud) . Broken access control doesn't have to be a headache for you or your product.


On this page


- Overview
- Setting the Table
- Every Service For Itself
- Gateway Authorization
- Centralized Authorization Services
- To Sum Up


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
