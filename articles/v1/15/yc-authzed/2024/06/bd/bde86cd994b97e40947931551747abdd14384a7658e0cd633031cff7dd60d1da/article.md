---
schema_version: "1.0.0"
document_id: "bde86cd994b97e40947931551747abdd14384a7658e0cd633031cff7dd60d1da"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-reflection-apis"
published_at: "2024-06-03T19:37:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:00:17.354967+00:00"
content_hash: "sha256:5f7ae3fc46fef92a10872a28565f28016582650c83b3fe5e834ceb83661ece50"
---

# Announcing Reflection APIs in SpiceDB

Today we’re happy to announce the addition of new[experimental reflection APIs](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.ExperimentalService) to SpiceDB, starting in version v1.33.0!


These four reflection APIs are the result of a[proposal](https://github.com/authzed/spicedb/issues/1505) created by the community. The new reflection APIs provide lower level information about the SpiceDB schema, including its relations and permissions, all using the same type system and schema compiler used in production, ensuring it matches SpiceDB’s own understanding.


As with every new API, we’ve added these to the experimental package so the community can start testing and providing early feedback, with the goal of eventually graduating once the API has been battle-tested and its design has stabilized.


Below we briefly summarize each reflection API, its purpose, and how it might be used to make certain use-cases with SpiceDB easier.


Let’s start with a sample schema and see how each API interacts with it:


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
relation    member  :  user
}


// resource is some kind of resource
definition    resource    {
relation    viewer  :  user
relation    editor  :  user


relation    org  :  organization


permission   edit  =   editor
permission   view  =   viewer  +   editor  +   org -  >member
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
relation    member  :  user
}


// resource is some kind of resource
definition    resource    {
relation    viewer  :  user
relation    editor  :  user


relation    org  :  organization


permission   edit  =   editor
permission   view  =   viewer  +   editor  +   org -  >member
}


```


Here we have a fairly simple resource that has two roles of` viewer` and` editor` that grant the view permission, as well allowing any` member` of the` resource` ’s organization to also` view` the resource.


NOTE: The reflection APIs are not intended to be used for parsing/understanding schema in offline tools. If you need a parsing system as part of offline tooling, please use the packages exported by SpiceDB in the[pkg package](https://github.com/authzed/spicedb/tree/main/pkg) .


#### ExperimentalReflectSchema


` ExperimentalReflectSchema` provides an API-driven means of receiving the structure of the current schema stored in SpiceDB.


It is designed primarily to allow callers to make dynamic decisions based on the structure of the schema, such as being able to see all the` permissions` defined for a particular type of` resource` .


Example: an application dynamically loads roles to be granted to a user in a permissions management UI. Instead of hardcoding the roles, you can now query the SpiceDB ReflectSchema and read the relations of a specific resource type.


Executing` ExperimentalReflectSchema` for our sample schema produces:


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


```text
ExperimentalReflectSchemaRequest{}


ExperimentalReflectSchemaResponse{
Definitions: []{
{ Name: "user" },
{
Name: "organization",
Relations: []{
{ Name: "member", SubjectTypes: []{ { Name: "user" } }, ... },
},
},
{
Name: "resource",
Comment: "// resource is some kind of resource",
Relations: []{ ... },
Permissions: []{ ... },
},
},
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


```text
ExperimentalReflectSchemaRequest{}


ExperimentalReflectSchemaResponse{
Definitions: []{
{ Name: "user" },
{
Name: "organization",
Relations: []{
{ Name: "member", SubjectTypes: []{ { Name: "user" } }, ... },
},
},
{
Name: "resource",
Comment: "// resource is some kind of resource",
Relations: []{ ... },
Permissions: []{ ... },
},
},
}


```


` ExperimentalReflectSchemaRequest` also includes support for prefix filters which can be used to filter the response to a specific subset of the schema:


1


2


3


4


5


6


7


```text
ExperimentalReflectSchemaRequest{
OptionalFilters: []{
{
OptionalDefinitionNameFilter: "a" // filter to defs starting with `a`
},
},
}


```


1


2


3


4


5


6


7


```text
ExperimentalReflectSchemaRequest{
OptionalFilters: []{
{
OptionalDefinitionNameFilter: "a" // filter to defs starting with `a`
},
},
}


```


NOTE: We strongly recommend caching the result of` ExperimentalReflectSchema` if it is being used to drive` CheckPermission` requests; while the process is not that slow, it does require a full load of the schema to compute the structure.


#### ExperimentalDiffSchema


` ExperimentalDiffSchema` provides an API-driven means of comparing the currently stored schema in SpiceDB to another schema.


This API is useful for tooling such as CI/CD that needs to determine what changes, if any, exist between the current schema and a future schema:


Example: a platform team has built a pipeline to help ship schema changes to a platform-wide centralized SpiceDB cluster. The automation in place helps the developer visualize the effective changes that are going to be deployed, like for example identifying potential breaking changes ahead of the WriteSchema call.


Calling` ExperimentalDiffSchema` against our schema above with a sample schema:


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


22


23


24


25


26


27


28


```text
ExperimentalDiffSchema{
ComparisonSchema: ```
definition user  {}


// an added comment
definition organization {
relation member: user
}


// resource is some kind of resource
definition resource {
relation viewer: user
relation editor: user


relation org: organization


permission edit = editor
permission view = viewer + editor + org->member
}
```
}


ExperimentalReflectSchemaResponse{
Diffs: []{
{ DefinitionDocCommentChanged: { Name: "organization", ... } },
{ PermissionExprChanged: { Name: "view", ... } },
}
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


22


23


24


25


26


27


28


```text
ExperimentalDiffSchema{
ComparisonSchema: ```
definition user  {}


// an added comment
definition organization {
relation member: user
}


// resource is some kind of resource
definition resource {
relation viewer: user
relation editor: user


relation org: organization


permission edit = editor
permission view = viewer + editor + org->member
}
```
}


ExperimentalReflectSchemaResponse{
Diffs: []{
{ DefinitionDocCommentChanged: { Name: "organization", ... } },
{ PermissionExprChanged: { Name: "view", ... } },
}
}


```


The difference information provided by` ExperimentalDiffSchema` exactly matches what is provided by the schema[diff package](https://github.com/authzed/spicedb/blob/main/pkg/diff/diff.go) in the` pkg` directory of SpiceDB.


#### ExperimentalDependentRelations


` ExperimentalDependentRelations` is a reflection API that provides the list of` relations` and` permissions` that are used to compute a particular permission.


Calling` ExperimentalDependentRelations` on the sample schema above:


1


2


3


4


```text
ExperimentalDependentRelationsRequest{
DefinitionName: "resource"
PermissionName: "view"
}


```


1


2


3


4


```text
ExperimentalDependentRelationsRequest{
DefinitionName: "resource"
PermissionName: "view"
}


```


Returns:


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
ExperimentalDependentRelationsResponse{
Relations: []{
{ DefinitionName: "organization", RelationName: "member", IsPermission: false},
{ DefinitionName: "resource", RelationName: "org", IsPermission: false},
{ DefinitionName: "resource", RelationName: "viewer", IsPermission: false},
{ DefinitionName: "resource", RelationName: "edit", IsPermission: true},
{ DefinitionName: "resource", RelationName: "editor", IsPermission: false},
}
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


9


```text
ExperimentalDependentRelationsResponse{
Relations: []{
{ DefinitionName: "organization", RelationName: "member", IsPermission: false},
{ DefinitionName: "resource", RelationName: "org", IsPermission: false},
{ DefinitionName: "resource", RelationName: "viewer", IsPermission: false},
{ DefinitionName: "resource", RelationName: "edit", IsPermission: true},
{ DefinitionName: "resource", RelationName: "editor", IsPermission: false},
}
}


```


Note that both` relations` and` permissions` are returned.


` ExperimentalDependentRelations` is useful for determining all the nested` relations` and` permissions` that may impact the computation of a` permission` .


#### ExperimentalComputablePermissions


` ExperimentalComputablePermissions` is the inverse of` ExperimentalDependentRelations` : it helps to determine any permissions impacted by a change to a` relation` or` permission` .


For example, given the same schema as above, calling` ExperimentalComputablePermissions` like so:


1


2


3


4


```text
ExperimentalComputablePermissionsRequest{
DefinitionName: "resource"
RelationName: "viewer"
}


```


1


2


3


4


```text
ExperimentalComputablePermissionsRequest{
DefinitionName: "resource"
RelationName: "viewer"
}


```


Will return:


1


2


3


4


5


```text
ExperimentalComputablePermissionsResponse{
Permissions: []{
{ DefinitionName: "resource", RelationName: "view", IsPermission: true},
}
}


```


1


2


3


4


5


```text
ExperimentalComputablePermissionsResponse{
Permissions: []{
{ DefinitionName: "resource", RelationName: "view", IsPermission: true},
}
}


```


` ExperimentalComputablePermissions` is useful for callers to determine the list of permissions that may be modified by updating a relationship on a particular` relation` or a change to a particular` permission` .


#### Conclusion


We hope you find these new reflection APIs useful in your authorization journey. If you have any questions, concerns or ideas on the new APIs, please do not hesitate to provide feedback in the[SpiceDB Discord](https://discord.com/invite/spicedb) .


On this page


- ExperimentalReflectSchema
- ExperimentalDiffSchema
- ExperimentalDependentRelations
- ExperimentalComputablePermissions
- Conclusion


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
