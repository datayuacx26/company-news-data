---
schema_version: "1.0.0"
document_id: "0936b8c0c10bd57dabb8d17d70283525286e057b8474ecb12a4d131f4020ebe2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/online-schema-migrations"
published_at: "2022-02-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:eddbf541617a7fa12effa196336b190186ba47f73b320e56fae5281814af8094"
---

# Online Schema Migrations in SpiceDB

Data migrations are a fact of life for any application. Requirements change, features are added or removed, services are merged together or split apart. As the application evolves, so too must the data behind it. In this regard, the permissions information you store in[SpiceDB](https://github.com/authzed/spicedb) is no different from any other data in any other database.


If you're unfamiliar with online migration strategies in general, there are a few[good resources](https://stripe.com/blog/online-migrations) online that cover the general topic; we're going to focus just on SpiceDB and how to perform migrations against the data and schema it holds.


## No Migrations Required


But first, let's highlight a couple of scenarios where SpiceDB shines because *no migrations* are needed:


### Service Refactoring


If the services that talk to SpiceDB are being refactored (i.e. a monolith is being split into a service oriented architecture), there is likely nothing you need to do with SpiceDB.


Because SpiceDB runs as a separate service, any new services that talk to it just need to connect and make the necessary queries. There's peace of mind knowing that authorization (typically a challenging part of changing architecture at this layer) won't be a burden or a bottleneck.


### New Permission Calculations


With SpiceDB, you *write* relationships, but you *check* permissions. Permissions are computed over the relationships that are already stored. This means that any changes you wish to make to how permissions are computed can be done without any migrations at all: simply update the schema.


For example, with this schema:


zed


1


2


3


4


5


6


7


8


```text
definition    user    {  }


definition    organization    {
relation    administrator  :  user
relation    direct_member  :  user


permission   member  =   direct_member
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


```text
definition    user    {  }


definition    organization    {
relation    administrator  :  user
relation    direct_member  :  user


permission   member  =   direct_member
}


```


You can include administrators as members and create a new "admin" permission without any migrations at all:


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


definition    organization    {
relation    administrator  :  user
relation    direct_member  :  user


permission   admin  =   administrator
permission   member  =   direct_member  +   administrator
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


definition    organization    {
relation    administrator  :  user
relation    direct_member  :  user


permission   admin  =   administrator
permission   member  =   direct_member  +   administrator
}


```


## Migration Strategies


There will always be cases where a migration is required, and when that happens you will need to plan out the migration steps. Here are some patterns to help with your online migration plan:


### A Note on Schema Consistency


All Schema writes are[fully consistent](https://docs.authzed.com/reference/api-consistency) . This means that any relationship writes that occur after the schema has been written will be evaluated under that new schema.


Any relationship *reads* , however, will use the revision of the schema that matches the revision of the read request. This ensures that checks evaluate with the correct schema to avoid[new enemy](https://authzed.com/blog/new-enemies/) problems.


### Safety First


SpiceDB enforces some rules to ensure migrations are safe:


1. New relationships and data can always be added to an existing schema.
2. A relation, permission, or object definition can only be removed from an existing schema if there are no relationships referencing it.


With these in mind, all you need to get started is an appropriate[API client](https://docs.authzed.com/reference/clients) .


### Base Schema


The following examples will assume this is the schema we're starting out with:


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


definition    document    {
relation    writer  :  user
relation    reader  :  user


permission   edit  =   writer
permission   view  =   reader  +   edit
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


definition    document    {
relation    writer  :  user
relation    reader  :  user


permission   edit  =   writer
permission   view  =   reader  +   edit
}


```


### Add a New Relation


(Jump back to theinitial schema for reference)


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
relation    writer  :  user
relation    reader  :  user
relation    owner  :  user     -  -   step 1


permission   edit  =   writer  +   owner  -  -   step 4
permission   view  =   reader  +   edit
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
relation    writer  :  user
relation    reader  :  user
relation    owner  :  user     -  -   step 1


permission   edit  =   writer  +   owner  -  -   step 4
permission   view  =   reader  +   edit
}


```


1. Roll out the schema change with the new relation
2. Any new documents that are created should include the new` owner` relation when updating spicedb.
3. Backfill the relationship as if it had been there from the beginning. In this case, we look up who created the document in the application’s database, and write the owner relation with[authzed.api.v1.WriteRelationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#WriteRelationships) . If this is done with[TOUCH](https://buf.build/authzed/api/docs/main/authzed.api.v1#authzed.api.v1.RelationshipUpdate) then it won’t conflict with the relationships written in the previous step.
4. When the data has been backfilled, confirmed valid, and new data is being written, you can include the relation in other permission calculations and update the schema. It may be safe to go ahead and update the schema right away; the permission calculations will simply change as you backfill the relationships.


### Remove a Relation


(Jump back to theinitial schema for reference)


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


definition    document    {
relation    writer  :  user
-  -   step 5 :  removed    ` relation    reader  :  user  `


permission   edit  =   writer
permission   view  =   edit  -  -   step 1 :  removed    `reader  +  `
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


definition    document    {
relation    writer  :  user
-  -   step 5 :  removed    ` relation    reader  :  user  `


permission   edit  =   writer
permission   view  =   edit  -  -   step 1 :  removed    `reader  +  `
}


```


1. Update the schema to remove any references to the relation from other permissions
2. If there are any issues discovered via testing at this point, the data is all still there and permissions can be restored by reverting the change to the schema.
3. You can stay at step 2 for as long as desired, in production, to retain your ability to roll back the change.
4. If no issues are found, remove all existing relationships for that relation with[authzed.api.v1.DeleteRelationships](https://buf.build/authzed/api/docs/main/authzed.api.v1#DeleteRelationships)
5. Update the schema to remove the relation from the definition


### Add a New Definition


(Jump back to theinitial schema for reference)


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


definition    group    {           -  -   step 1
relation    member  :  user      -  -   step 1
}                            -  -   step 1


definition    document    {
relation    writer  :  user     |   group#member  -  -   step 4
relation    reader  :  user     |   group#member  -  -   step 4
permission   edit  =   writer
permission   view  =   reader  +   edit
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


definition    group    {           -  -   step 1
relation    member  :  user      -  -   step 1
}                            -  -   step 1


definition    document    {
relation    writer  :  user     |   group#member  -  -   step 4
relation    reader  :  user     |   group#member  -  -   step 4
permission   edit  =   writer
permission   view  =   reader  +   edit
}


```


1. Add the new definition to the schema
2. Add any relations that should exist (fill in which users go in which groups)
3. Ensure that the application writes the new relations when needed
4. Reference the new definitions / relations in other relations or permissions


### Remove a Definition


1. Follow the steps toremove a relation for every relation in the definition
2. Remove the definition from the schema and update it.


## Future Work


Now that we have worked through some examples, it's clear that migrations in SpiceDB are no more challenging than migrations in a general purpose DBMS, and in some cases much simpler.


But there is no reason to stop there! We've seen how SpiceDB's focus on permission calculations makes a certain class of hard migration problems go away. We think that there are more ways we can simplify migrations in SpiceDB. If you're interested in discussing those with us (or you have a question about your own migrations), why not drop by our[discord](https://authzed.com/discord) and have a chat?


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- No Migrations Required
- Service Refactoring
- New Permission Calculations
- Migration Strategies
- A Note on Schema Consistency
- Safety First
- Base Schema
- Add a New Relation
- Remove a Relation
- Add a New Definition
- Remove a Definition
- Future Work
- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
