---
schema_version: "1.0.0"
document_id: "69e72f213c18a7ebb3dae18a42989e341e5f3714f6790a54c303af475f73b5c0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/check-it-out"
published_at: "2021-03-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:4a116ddbdbf04141a008f399df70d77bc80bd242e95200d40f8e1d66b36d46d9"
---

# Check it out: How permissions are answered in SpiceDB and Authzed

**NOTE:** After the open sourcing of[SpiceDB](https://github.com/authzed/spicedb) , this blog post has been updated to match current SpiceDB/Authzed terms and schema configuration


This is part 1 of a multi-part blog series. Read[part 2](https://authzed.com/blog/check-it-out-2) !


## What to ask


Authentication and Authorization are searching for the answers to questions.


For Authentication, the question is "Who are you?". The software requests to know who the calling user (or machine or entity) is to make decisions, assign ownership or log usage. Authentication is about *identity* .


On the other hand, authorization is concerned with a different question: "What do you want (to do)?" The software requests to know what the calling user (or machine or entity) wants to do to verify and validate that the calling user can take such an action. Authorization is about *permission* .


## Framing the question


Imagine a simple permissions situation: A user ("Fred") attempts to open a document. We've already answered the Authentication question here ("Who are you?” -> “Fred!"), and now we are attempting to answer the Authorization question: "What do you want to do?” -> “I want to view the document".


This question can be simplified:


> Can Fred view the document?


Many developers think of such permission questions as "easy" or even as an afterthought; after all, how hard can it be to answer questions like "Can Fred view the document?". However, within the shadows of that simple question lies a host of potential complexity, performance bottlenecks, and security concerns.


At Authzed, our[authorization platform as a service](https://authzed.com/) and[open source implementation](https://github.com/authzed/spicedb) of Zanzibar are designed to answer these kinds of permissions questions efficiently and with minimal overhead while providing tooling to handle the bulk of the complexity.


## Defining a permission


Within the SpiceDB API, these basic permissions questions are called[Checks](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) . Here we'll shine a light on the process that SpiceDB uses to answer these queries and how doing so allows for more powerful permissions models.


Let's take our example from above: "Can Fred view the document?" Or perhaps another example: Can this machine delete this row in the database? Both examples express a question with the same form:


> "Does` <subject>` have` <permission>` on the` <resource>` ?"


In our first example, the` resource` is the specific document being accessed, the` permission` is` view` and the` subject` is the user` Fred` : We want to know whether Fred has *permission* to **view** the document.


Similarly, in our second example, the` permission` is` delete` , the` resource` is the specific row to be deleted, and the` subject` is the *account for the machine making the call* : We want to know whether the *machine* has permission to delete the row.


However, permissions systems are rarely about a single subject, permission or resource. Instead, we need to know how we can generically answer this kind of question for all combinations of subjects, permissions and objects.


## Relating objects


At their core, permissions are decided by relationships: Fred can read the document if the correct type of relationship between Fred and the document. How these relationships are formed depends on the permissions system being used. For example, in[RBAC](https://docs.authzed.com/authz/rbac) , these relationships are expressed as *roles* , such as` reader` ,` writer` , or` admin` , each defining a set of permissions for subjects In contrast,[ABAC](https://docs.authzed.com/authz/abac) , as another example, these relationships are expressed as *attributes* , such as` can_read` .


When using SpiceDB, developers define the kinds of relationships and the concrete ones that exist via a combination of[schema](https://docs.authzed.com/reference/schema-lang) and[relationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#WriteRelationships) .


A **schema** defines the kinds of objects (resources and subjects) and how they might relate to other object.


A **relationship** is the concrete data that realizes the actual link between specific objects using one of the possible relations.


### An example


While SpiceDB is not restricted to a specific kind of permissions model (because developers have the freedom to create their own), for convenience, we will here demonstrate an RBAC-like model with three different roles:` reader` ,` owner` and an` admin` on the parent "organization" that owns the document.


The kinds of objects (and how they can relate) are expressed in the Authzed[schema language](https://docs.authzed.com/reference/schema-lang) .


Each` definition` defines a kind of object and has zero (or more)` relation` s, which defines a *possible* relationship between objects of that type and other objects.


Let's define the object types and relations for our example:


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


definition    organization    {
relation    admin  :  user
}


definition    document    {
relation    org  :  organization


relation    owner  :  user
relation    reader  :  user
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


definition    organization    {
relation    admin  :  user
}


definition    document    {
relation    org  :  organization


relation    owner  :  user
relation    reader  :  user
}


```


In the above schema, we've defined three kinds of objects (` user` ,` organization` , and` document` ) and some` relation` 's to indicate the kinds of relationships that **can** exist between the objects.


We also would like permissions to check, so let's add those as well:


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


15


16


```text
definition    user    {  }


definition    organization    {
relation    admin  :  user


permission   can_admin  =   admin
}


definition    document    {
relation    org  :  organization


relation    owner  :  user
relation    reader  :  user


permission   view  =   reader  +   owner  +   org -  >can_admin
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


15


16


```text
definition    user    {  }


definition    organization    {
relation    admin  :  user


permission   can_admin  =   admin
}


definition    document    {
relation    org  :  organization


relation    owner  :  user
relation    reader  :  user


permission   view  =   reader  +   owner  +   org -  >can_admin
}


```


Finally, we also need to *realize* the relationships between these objects. In Authzed, we do so by writing one or more[relationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#WriteRelationships) , defining *edges* from one object to another:


yaml


1


2


3


4


5


```text
document:somedocument#reader@user:sean                    # Sean is a reader on somedocument
document:somedocument#reader@user:fred                    # Fred is a reader on somedocument
document:somedocument#owner@user:jill                     # Jill is the owner of somedocument
organization:theorg#admin@user:hannah                     # Hannah is the admin of the organization
document:somedocument#org@organization:theorg             # `theorg` is the organization for the document


```


yaml


1


2


3


4


5


```text


```


Together, the schema and the relationships form a **graph** of a permissions system, and it is this graph that SpiceDB uses to compute answers to permissions questions.


*The permissions system in the Authzed Playground*


## Can Fred view the document?


Let's look at the graph formed by the schema and relationships defined above.


First, we apply the schema, which defines the *shape* of the graph:


The graph shows a set of paths from the resource (` document` ), through the` view` permission, to the relations defining our roles (` reader` ,` owner` ,` organization` ,` admin` ) and finally to the subjects (` user` ,` organization` ).


Note that in this graph, the paths represent the *possible* paths of relationships between documents and users, but nothing yet concrete; the real paths emerge only once we apply the defined relationships:


Now we can see a fully realized graph, with possible paths between a specific document, via specific relations, to specific users.


## Answering the question


To[Check](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) whether Fred can view the document, we issue the following API call:


proto


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


```text
CheckPermissionRequest {
resource: ObjectReference {
object_type: 'document'
object_id: 'somedocument'
}
permission: 'view'
subject: SubjectReference{
object: ObjectReference {
object_type: 'user'
object_id: 'fred'
}
}
}


```


proto


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


```text
CheckPermissionRequest {
resource: ObjectReference {
object_type: 'document'
object_id: 'somedocument'
}
permission: 'view'
subject: SubjectReference{
object: ObjectReference {
object_type: 'user'
object_id: 'fred'
}
}
}


```


How does SpiceDB answer this[Check](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) question? It searches for a path from the` resource` (here:` somedocument` ) through the` permission` (` view` ) to the subject (` fred` ). If such a path is found, we know` fred` is "reachable" via the` view` permission so he can view the document.


Let's take our[Check](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) above and follow the resolutions. We begin with the resource (` somedocument` ) and search for the matching permission (` view` ). Next, we walk from` view` to any relations (or other permissions) defined for that permission, which gives us these relations:


c


1


```text
reader + owner + org->can_admin


```


c


1


```text
reader + owner + org->can_admin


```


Further walking outward to the relation` reader` , we find these users:


yaml


1


2


```text
user:sean
user:fred


```


yaml


1


2


```text
user:sean
user:fred


```


And... we're done! We've found` fred` !


### Visualizing


As we saw above, there does indeed exist such a path for Fred, and so the answer to our question is "yes":


If we instead attempted to ask this question for another (say,` adam` ), we can see that we find no such path, indicating that the answer is "no":


# What about Jill?


In our relationships defined above, we defined that the user` jill` has the role of` owner` on the document. Does this mean she can also view the document?


Let's run our[Check](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) call, starting at` somedocument` . We begin by checking` view` , which gives us the relations previously found:


c


1


```text
reader + owner + org->can_admin


```


c


1


```text
reader + owner + org->can_admin


```


` jill` isn't in the` reader` relation, so we'd terminate lookup and return` no` ... except we have two more` relation` paths to check!


Therefore, the next step in our walk of the graph would be to list those subjects related by` owner` , which gives us:


yaml


1


```text
user:jill


```


yaml


1


```text
user:jill


```


Once again, we've found the user we want, thus resulting in a "yes" answer:


As we can see, because the` view`` permission` is defined to check **multiple**` relation` 's, any path found suffices to grant permission!


## Who's the boss?


Our example schema also defines another role, that of the` admin` of organizations. Logically speaking, any admin of the organization should be able to view all the documents found "within" that organization. Since we defined` hannah` as being the admin of the organization` theorg` , and` theorg` as being the organization for the document, let's see how a[Check](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) for` view` on the document for Hannah might work:


The path used to reach` hannah` from the document certainly appears to be longer, but once again, we can see that the resolution is a reasonably straightforward graph walk: We were able to walk from the document, through` view` , then from the document to the organization (through an[arrow](https://docs.authzed.com/reference/schema-lang#--arrow) ), to the` can_admin` permission, and finally through` admin` to Hannah, showing that, in the end, she's the boss.


## In summary


As we've seen above, answering Authorization[Check](https://buf.build/authzed/api/docs/main/authzed.api.v1#CheckPermission) questions is accomplished within SpiceDB using a reasonably straightforward graph walking mechanism. By using a walk over a graph, SpiceDB can support permissions models with arbitrary numbers of roles, arbitrary levels of hierarchies, and even complex decisions that use operators such as intersection or exclusion.


It should be noted that while the above walking examples are straightforward, the way we accomplish these Checks is, in reality, far more complex to ensure good performance and redundancies.


We've barely scratched the surface of SpiceDB resolution today. We dive deeper into the mechanisms behind these resolutions in[part 2](https://authzed.com/blog/check-it-out-2) !


Have more questions about how resolution in SpiceDB works, or need help with developing your schema?[Join the community on Discord](https://authzed.com/discord) to get answers!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- What to ask
- Framing the question
- Defining a permission
- Relating objects
- An example
- Can Fred view the document?
- Answering the question
- Visualizing
- What about Jill?
- Who's the boss?
- In summary
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
