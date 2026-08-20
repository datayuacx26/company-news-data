---
schema_version: "1.0.0"
document_id: "2a899deea1f45bdce755a77372273b0b033a2f611e7532e055fe421ab75755b3"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/check-it-out-2"
published_at: "2022-03-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:68fb60f1ef7dc4f3e561323f5102d6f8bd6cc4b2302159af930a85a762e47f60"
---

# Check it out #2: How intersections and exclusions are computed in SpiceDB and Authzed

This post is the second in a series of posts about how[SpiceDB](https://github.com/authzed/spicedb) resolves permissions decisions. See[Check it out](https://authzed.com/blog/check-it-out/) for the first post in the series!


In[my previous post](https://authzed.com/blog/check-it-out/) , I discussed how SpiceDB walks a graph formed by[relationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#WriteRelationships) and[schema](https://docs.authzed.com/reference/schema-lang) to determine whether a subject (typically a user) has a specific permission on a resource.


Following the publication of that blog post, we’ve had a question come up frequently:


> Okay, I get how that works for simple permissions… but how are[intersections](https://docs.authzed.com/reference/schema-lang#-intersection) and[exclusions](https://docs.authzed.com/reference/schema-lang#--exclusion) computed?


## Why intersection and exclusion?


Permissions used and defined by applications are typically *additive* : For example, a user may gain permission to view a document if they have a reader role or a writer role or are the administrator of the document’s organization. This process is also known as the user being on an **allowlist** , typically defined by one or more roles.


In our experience, while most permissions are defined by the requirement of a **single** resolution path for a given user, some require more complex calculations.


In fact, in some cases, whether a user has a permission can be defined not just by what they are allowed to do but also by what they are not.


To support both of these cases, SpiceDB supports not only the standard union operator (` +` ) but also the intersection and exclusion operators. These operators allow schemas to define that a subject has a permission if either the user is found on **both** paths (intersection:` &` ) or **only** on one path, but not the other (exclusion:` -` )


### Example: Intersection


Intersection is often used when a permission is based on **multiple** other permissions or roles.


For example, imagine a permission on a document that says a comment can be deleted only if a user is both an editor and a commenter.


For this use case, the **intersection** operator (` &` ) can be used, to create an allowlist that only applies if the user has *both* roles:


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


definition    document    {
relation    commenter  :  user
relation    editor  :  user


permission   edit  =   editor
permission   comment  =   commenter
permission   delete_comment  =   comment  &   edit
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


definition    document    {
relation    commenter  :  user
relation    editor  :  user


permission   edit  =   editor
permission   comment  =   commenter
permission   delete_comment  =   comment  &   edit
}


```


### Example: Exclusion


As another example, imagine an application that allows any user to comment on a post. Such an application might want the ability to ban problematic users from posting while still not requiring the need to grant a comment posting role to every user.


For this use case, the **exclusion** operator (` -` ) can be used, to create a **denylist** of only those problematic users, while still allowing for general access:


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


```text
definition    user    {  }


definition    post    {
relation    commenter  :  user  :*
relation    banned  :  user


permission   comment  =   commenter
permission   post_comment  =   comment  -   banned
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


```text
definition    user    {  }


definition    post    {
relation    commenter  :  user  :*
relation    banned  :  user


permission   comment  =   commenter
permission   post_comment  =   comment  -   banned
}


```


In this schema, only users found in the` banned` relation will be expressly prohibited from having the` post_comment` permission; any other user (via the[wildcard](https://docs.authzed.com/reference/schema-lang#wildcards)` user:*` ) will have permission.


## Breaking into subproblems


As we saw in the[Check it out](https://authzed.com/blog/check-it-out/) blog post, permissions in SpiceDB are answered by walking the directed graph formed by both the[relationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#WriteRelationships) and the[schema](https://docs.authzed.com/reference/schema-lang) , from the permission to the associated subject (user): if at least one such path exists, then the subject has the permission on the resource, and SpiceDB can return saying so.


Recall the example used in that blog post:


To check whether a user (such as` jill` ) had permission to view the document, a walk was started at the` view` permission for the document, until` jill` was encountered:


One item elided from the previous blog post is how SpiceDB computes these walks in a performant manner: each "step" in the walk is considered a problem to be solved.


By treating each step as its own problem, SpiceDB is able to both cache problems for reuse and execute these subproblems in parallel, ensuring good performance:


This makes perfect sense for computing permissions that are just additive (i.e. only include "unions"), but how does this work for intersection and exclusion?


## Intersection and Exclusion Subproblems


Intersections and exclusions in SpiceDB are handled similarly to standard additive permissions with one caveat: they are not always able to short-circuit.


In a simple addition permission, as soon as SpiceDB finds **any** subproblem that returns "has permission", we’re done: the subject has been determined to have permission, and the answers to the other subproblems, if any, do not matter. SpiceDB can successfully answer the permission query with the information found and cancel any other pending requests.


For intersection and exclusion, on the other hand, SpiceDB can only partially short-circuit the subproblems; if a subproblem returns "no permission" (for intersection) or "has permission" (for exclusion), SpiceDB must wait for the other subproblem answers before it can continue.


### Intersection


Let’s take the intersection example from earlier and compute whether a particular user (` fred` ) can delete a comment on a document.


To start, we have our schema:


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


definition    document    {
relation    commenter  :  user
relation    editor  :  user


permission   edit  =   editor
permission   comment  =   commenter
permission   delete_comment  =   comment  &   edit
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


definition    document    {
relation    commenter  :  user
relation    editor  :  user


permission   edit  =   editor
permission   comment  =   commenter
permission   delete_comment  =   comment  &   edit
}


```


And some test relationships:


yaml


1


2


3


```text
document:somedocument#commenter@user:fred
document:somedocument#commenter@user:jill
document:somedocument#editor@user:jill


```


yaml


1


2


3


```text
document:somedocument#commenter@user:fred
document:somedocument#commenter@user:jill
document:somedocument#editor@user:jill


```


This schema and relationships can be formed into the following graph:


To compute whether` fred` can delete a comment on the document, we start at the` delete_comment` permission and walk outward:


As we see above, the path for permission` comment` for` fred` returns "has permission"! However, in order to delete a comment on the document,` fred` needs both` comment` **and**` edit` permissions This means that SpiceDB had to check both subproblems and wait for all to return "has permission" (which it did not).


Fortunately, here SpiceDB was able to be **partially** short-circuit: as soon as it saw a "no permission" response for` edit` , SpiceDB was able to cancel the other` check` immediately, because an intersection requires *all* branches to be "has permission".


Let’s try for user` jill` :


As we can see for` jill` , every subproblem found shows` jill` has permission; thus, she has permission to delete comments on the document!


We can therefore see how SpiceDB resolves intersections:


1. API call to` check` for` delete_comment` on` document:somedocument` for subject` user:fred`
2. Dispatch two subproblems:


- Check permission` edit` on` document:somedocument` for` user:fred`
- Check permission` comment` on` document:somedocument` for` user:fred`


1. If **any** check is "no permission", return "no permission" (we’re done!)
2. Otherwise, return "has permission" once all checks have completed


### Exclusion


Exclusion in SpiceDB is handled as the rough inverse of intersection: Rather than waiting for every branch to return "has permission" and returning immediately if at least one does not, exclusion check all branches (except the first), and if *any* return "has permission", the entire permission returns "no permission".


Let’s take the example from earlier and compute whether a particular user (` jill` ) can post a comment on a document.


To start, we have our schema:


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


```text
definition    user    {  }


definition    post    {
relation    commenter  :  user  :*
relation    banned  :  user


permission   comment  =   commenter
permission   post_comment  =   comment  -   banned
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


```text
definition    user    {  }


definition    post    {
relation    commenter  :  user  :*
relation    banned  :  user


permission   comment  =   commenter
permission   post_comment  =   comment  -   banned
}


```


And some test relationships:


yaml


1


2


```text
document:somedocument#commenter@user:*    # Any user can comment
document:somedocument#banned@user:tom     # Tom is banned


```


yaml


1


2


```text
document:somedocument#commenter@user:*    # Any user can comment
document:somedocument#banned@user:tom     # Tom is banned


```


This schema and relationships can be formed into the following graph:


To compute whether` jill` can post a comment on the document, we start at the` post_comment` permission and walk outward:


Since` jill` has permission to` comment` (because of the[wildcard](https://docs.authzed.com/reference/schema-lang#wildcards) ) and she is not found in the` banned` relation, she can comment on the document!


Let’s try for` tom` , who is supposed to be banned:


In the case of` tom` , as soon as SpiceDB sees that he is in the` banned` relation, it does not matter if he otherwise has permission, and SpiceDB can return "no permission"


We can therefore see how SpiceDB resolves exclusions:


1. API call to` check` for` post_comment` on` document:somedocument` for subject` user:tom`
2. Dispatch two subproblems:


- Check permission` comment` on` document:somedocument` for` user:tom`
- Check permission` banned` on` document:somedocument` for` user:tom`


1. If` comment` is "no permission", return "no permission"
2. If *any* other branch is "has permission", return "no permission"
3. Otherwise, return "has permission" once all checks have been completed


## Final thoughts


Intersection and exclusion are powerful tools in a[schema](https://docs.authzed.com/reference/schema-lang) author’s toolkit, providing the ability to create more complex permissions systems expressing ideas such as[feature flags](https://play.authzed.com/s/DjE8I9pjqv6N) . They are, however, less parallelizable than simple unions, so they should be used wisely.


In the next post in this series, we'll detail about how SpiceDB uses caching to ensure the scalability and performance of checks.


Have another question or want to discuss with the community? Check out our[Discord](https://authzed.com/discord) !


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Why intersection and exclusion?
- Example: Intersection
- Example: Exclusion
- Breaking into subproblems
- Intersection and Exclusion Subproblems
- Intersection
- Exclusion
- Final thoughts
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
