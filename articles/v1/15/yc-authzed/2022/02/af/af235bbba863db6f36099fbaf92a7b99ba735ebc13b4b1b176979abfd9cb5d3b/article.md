---
schema_version: "1.0.0"
document_id: "af235bbba863db6f36099fbaf92a7b99ba735ebc13b4b1b176979abfd9cb5d3b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/writing-relationships-to-spicedb"
published_at: "2022-02-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:be51722f7674ea16d531e85b294673dde67bcb602ca474659a6ba7cc59bb23b7"
---

# Writing relationships to SpiceDB

[SpiceDB](https://github.com/authzed/spicedb) is Authzed's open source,[Zanzibar](https://authzed.com/blog/what-is-zanzibar/) -inspired permissions system.


Like any permissions system, the primary goal of SpiceDB and Authzed's hosted platform is the ability to answers questions of the form:


html


1


2


3


4


```text
does
< subject  >
have  < permission  >   on  < resource  >  ? </ resource  >  </ permission  >  </subject
>


```


html


1


2


3


4


```text
does
< subject  >
>


```


To answer such questions, a permissions system must know how the` subject` and` resource` **relate** to one another: Does the subject perhaps have a role on the resource granting the permission, as in a traditional Role-Based Access Control (RBAC) system? Does there exist an *attribute* granting permission to the subject, as in an Attribute-Based Access Control (ABAC) system? Is the subject granted permission based on some policy?


SpiceDB (and Zanzibar), on the other hand, operates slightly differently.


## Forming a relationship


SpiceDB is not a role-, or attribute-, or even policy-based permissions system.


Rather, SpiceDB (and Zanzibar) are *relationship based* permissions systems (ReBAC). By defining the **relationship** between resources and subjects, SpiceDB can compute permissions by[walking a directed graph](https://authzed.com/blog/check-it-out/) , finding a path between the resource+permission and the subject for which the permission is being checked.


This model of permissions is extremely powerful, as simpler models (such as RBAC) can be represented, while also supporting more complex or unique forms of permissions resolution.


How are these relationships supplied, however, to SpiceDB?


## Why is SpiceDB… a DB?


At Authzed we've often been asked to explain the origin of the name "SpiceDB".


The "Spice" portion of the name comes from the "spice" found in[Dune](https://en.wikipedia.org/wiki/Dune_(novel)) , as the original[Zanzibar](https://authzed.com/blog/what-is-zanzibar/) project was, we believe, originally named after the novel.


The "DB" portion of the name, on the other hand, was chosen deliberately to highlight one of the most significant differences between SpiceDB and other permissions systems:


> The relationships between resources and subjects used for permissions checks are stored within SpiceDB's data store: rather than providing these relationships at "check" time, they are available to any SpiceDB node when permissions are being computed


Why is this important? Performance and[consistency](https://docs.authzed.com/reference/api-consistency) !


One of the major goals of the Zanzibar project (and SpiceDB) is to provide a horizontally scalable permissions system that can answer thousands or millions of simultaneous permissions questions in 10s of milliseconds, while **also** providing guarantees around consistency, to prevent problems such as the[new enemy problem](https://authzed.com/blog/prevent-newenemy-cockroachdb/) .


If the relationships between objects and subjects were sent to SpiceDB/Authzed on each call, the burden to provide fast, reliable and **consistent** data access would fall on the shoulders of the callers, in every language and service.


With SpiceDB storing the relationships, all of these concerns can be handled directly by SpiceDB, with the caller to[CheckPermission](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) only having to specify whether they want full consistency or allow caching to be used.


## Writing relationships


The major question therefore becomes: How do applications robustly and consistently write these relationships into both the application's database, as well as SpiceDB?


We've found that there are a number of solutions, depending on your application's design and requirements. Below we outline a few of the existing best practices.


### Two writes + commit


The most common and straightforward way to store relationships in SpiceDB is to use a[2PC](https://martinfowler.com/articles/patterns-of-distributed-systems/two-phase-commit.html) -like approach.


In an application backed by a standard relational database, the application can make use of the database's transaction system to provide a solid solution:


python


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


```text
# Open the database transaction
with   db.transaction()  as   transaction:
# Update the relationship in the database.
document = Document(
id  = "somedoc"  ,
owner=some_user,
)
transaction.add(document)


# Add the relationship(s) in SpiceDB.
resp = client.WriteRelationships(...)


# Store the ZedToken we've received.
document.zedtoken = result.written_at


# Transaction is committed on close


```


python


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


```text
# Open the database transaction
with   db.transaction()  as   transaction:
# Update the relationship in the database.
document = Document(
id  = "somedoc"  ,
owner=some_user,
)
transaction.add(document)


# Add the relationship(s) in SpiceDB.
resp = client.WriteRelationships(...)


# Store the ZedToken we've received.
document.zedtoken = result.written_at


# Transaction is committed on close


```


The above approach has some nice properties:


- On success, the[ZedToken](https://docs.authzed.com/reference/zedtokens-and-zookies) returned by the` WriteRelationships` call is written alongside the data in the relational database, ensuring proper consistency
- If the SpiceDB update fails for any reason, the database changes will never be applied
- If the database transaction fails to open, SpiceDB will not be updated
- If the database transaction fails to commit, the relationship will existing in SpiceDB, **but there will be no objects for it in the relational database** - in many schemas, this means that the relationship in SpiceDB is likely "superfluous", never being used in a CheckPermission, and therefore safe to have around (minus some extra space being taken up)


In the case where cleanup is warranted, an optional` OPERATION_DELETE` can be added to the above, to ensure that if the database transaction fails to complete, the relationship is removed from SpiceDB:


python


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


18


19


20


21


```text
# Open the database transaction
try  :
with   db.transaction()  as   transaction:
# Update the relationship in the database.
document = Document(
id  = "somedoc"  ,
owner=some_user,
)
transaction.add(document)


# Add the relationship(s) in SpiceDB.
resp = client.WriteRelationships(...)


# Store the ZedToken we've received.
document.zedtoken = result.written_at


# Transaction is committed on close
except  :
# delete the relationship(s) just written
client.DeleteRelationships(...)
raise


```


python


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


18


19


20


21


```text
# Open the database transaction
try  :
with   db.transaction()  as   transaction:
# Update the relationship in the database.
document = Document(
id  = "somedoc"  ,
owner=some_user,
)
transaction.add(document)


# Add the relationship(s) in SpiceDB.
resp = client.WriteRelationships(...)


# Store the ZedToken we've received.
document.zedtoken = result.written_at


# Transaction is committed on close
except  :
# delete the relationship(s) just written
client.DeleteRelationships(...)
raise


```


### Streaming commits


Another approach is to stream updates to both a relational database and SpiceDB via a third party streaming system such as[Kafka](https://kafka.apache.org/) , using a pattern known as Command Query Responsibility Segregation.


In this design, any updates to the relationships in both databases are published as **events** to the streaming service, with each event being consumed by a system which performs the updates in both the database and in SpiceDB. In other words, each application *commands* a change, which is in turn handled by a service *querying* for those commands and applying the changes.


In the case that the update to either system fails, the update in both databases are rolled back, and the event is requeued to be processed again.


This approach has a number of its own advantages:


- Updates are retried if the update to either database fails.
- Application only needs to know how to push events; the processor(s) take care of updating the individual databases.
- Updates can easily be logged by the streaming system.


### Not storing relationships in the relational database


Sometimes, an application does not even need to store permissions-related relationships in its relational database.


Consider a permissions system that allows for teams of users to be created and used to access a resource. In SpiceDB's schema, this could be represented as:


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


```text
definition    user    {  }


definition    team    {
relation    member  :  user
}


definition    resource    {
relation    reader  :  user     |   team#member
permission   view  =   reader
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


```text
definition    user    {  }


definition    team    {
relation    member  :  user
}


definition    resource    {
relation    reader  :  user     |   team#member
permission   view  =   reader
}


```


In the above example, the relationship between a resource and its teams, as well as a team and its members does not need to be stored in the application's database **at all** .


Rather, this information can be stored solely in SpiceDB, and accessed by the application via a[ReadRelationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#ReadRelationships) or[ExpandPermissionsTree](https://buf.build/authzed/api/docs/main/authzed.api.v1#ExpandPermissionTree) call when necessary.


The only data that the application database might need to store is metadata, such as the team's friendly name, while SpiceDB acts as the source of truth for resources and teams.


This approach also has another benefit: changes to the computation of the permissions can be done without any code changes, as the full set of relationships and their computed permissions exists completely within the SpiceDB schema.


### Asynchronous Updates of SpiceDB


**NOTE:** This should *only* be used if your application supports less rigid consistency guarantees.


If an application does not require up-to-the-second consistent permissions checking, and some replication lag in permissions checking is acceptable, then asynchronous updates of the relationships in SpiceDB can be used.


In this design, a synchronization process, typically running in the background, is used to write relationships to SpiceDB in reaction to any changes that occur in the primary relational database.


The major downside of this approach is the replication lag: If the replication process starts lagging, permissions checks could become more and more stale, which could then become an issue for more sensitive applications.


## Designs for the future


As we've seen above, there are a number of solutions today for storing relationships in both an application's relational database, as well as SpiceDB.


Moving forward, Authzed is committed to making this process even easier For example, we already have a[prototype tool](https://authzed.com/blog/zed-import/) that allows for easy migration of PostgreSQL foreign keys to relationships within SpiceDB.


Longer term, we're interested in hearing from you, the community, on how to make this process of updating easier.


Have an idea?[Join the community in Discord](https://authzed.com/discord) to discuss!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Forming a relationship
- Why is SpiceDB… a DB?
- Writing relationships
- Two writes + commit
- Streaming commits
- Not storing relationships in the relational database
- Asynchronous Updates of SpiceDB
- Designs for the future
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
