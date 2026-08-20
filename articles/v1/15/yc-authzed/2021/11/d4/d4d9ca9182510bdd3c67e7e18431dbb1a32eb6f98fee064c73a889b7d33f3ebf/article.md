---
schema_version: "1.0.0"
document_id: "d4d9ca9182510bdd3c67e7e18431dbb1a32eb6f98fee064c73a889b7d33f3ebf"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/zed-import"
published_at: "2021-11-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:d36d2324a18cf722f9e1d3bde85db5072c886565254b5be6a28c5e0e6cade2f3"
---

# Migrating from PostgreSQL to Centralized Authorization with SpiceDB

[SpiceDB](https://github.com/authzed/spicedb) is a database designed to store relationships and compute authorization decisions. SpiceDB decouples authorization data from your applications and stores it in a centralized service, freeing you to focus on writing your applications while leaving the crunchy authorization calculations to SpiceDB.


When writing a new application, it is easy to hit the ground running with SpiceDB - import one of the[client libraries](https://docs.authzed.com/reference/clients) into your application and start hacking.


Today, we're also simplifying the integration of existing applications into SpiceDB.


## Introducing` zed import`


The latest version of[zed](https://github.com/authzed/zed/releases/tag/v0.2.0) introduces a new subcommand` import` to help you get external data into SpiceDB.


If you have been working on a SpiceDB schema in the[playground](https://play.authzed.com/) , you can now import the schema (and test relationships) into a SpiceDB instance with:


sh


1


```text
zed import https://play.authzed.com/s/iksdFvCtvnkR/schema


```


sh


1


```text
zed import https://play.authzed.com/s/iksdFvCtvnkR/schema


```


Just paste a "share" link from the playground and` zed` will do the rest.


` zed` also supports importing directly from a gist or from pastebin:


sh


1


```text
zed import https://pastebin.com/8qU45rVK


```


sh


1


```text
zed import https://pastebin.com/8qU45rVK


```


sh


1


```text
zed import https://gist.github.com/ecordell/8e3b613a677e3c844742cf24421c08b6


```


sh


1


```text
zed import https://gist.github.com/ecordell/8e3b613a677e3c844742cf24421c08b6


```


Check` zed import --help` for more examples, including importing from a running` spice serve-devtools` instance or a local file. Don't see a service that you'd like to see supported?[File an issue](https://github.com/authzed/zed/issues/new) and let us know!


## Postgres Import


The latest release of` zed` also introduces an experimental way to transform and import data from a PostgreSQL database into SpiceDB.


Importing from PostgreSQL is a great way to get data from an existing application into SpiceDB for testing or bootstrapping a transition away from bespoke, unmaintainable application authorization.


` zed experiment import postgres` will connect to PostgreSQL, generate an example SpiceDB schema and a config file that maps PostgreSQL data into SpiceDB, and then sync that data into SpiceDB relationships. There are a few ways you can try it out:


### Self-Driving


This is good for getting some real data into SpiceDB for testing purposes. The importer reads the schema of the PostgreSQL database and makes a best-effort mapping config based on foreign-key relationships. For simple schemas, this may be all you need.


sh


1


```text
zed experiment import postgres --dry-run= false    "postgres://postgres:secret@localhost:5432/mydb?sslmode=disable"


```


sh


1


```text


```


- Prints out a zed schema + a config mapping from PostgreSQL to SpiceDB
- Appends the generated zed schema to SpiceDB's schema
- Mirrors all relationships into SpiceDB according to that config


### Dry-Run


Without any flags specified, the import will be a dry-run. It will print the generated SpiceDB schema and mapping to stdout and log relationships that would have been written to SpiceDB to stderr.


This can be a good place to start writing your own config file.


sh


1


```text
zed experiment import postgres  "postgres://postgres:secret@localhost:5432/mydb?sslmode=disable"   > config.yaml


```


sh


1


```text


```


- Prints out a zed schema + a config mapping from PostgreSQL to SpiceDB
- Logs relationships that would have been written to SpiceDB


## Custom Config


You might want to write your own importer config if:


- You have a complex schema that the importer doesn't generate a good mapping for
- You want to generate relationships that differ from the foreign key relationships in PostgreSQL (i.e. if you have a join table)
- You want to generate relationships that don't correspond to foreign key constraints at all
- You want to change the generated object type and relationship names


You can tell the importer exactly how to map rows from PostgreSQL into relationships in SpiceDB. Running the importer first as a dry-run is a good way to get an example config for your database.


sh


1


```text
zed experiment import postgres --config=config.yaml --append-schema= false    "postgres://postgres:secret@localhost:5432/mydb?sslmode=disable"


```


sh


1


```text


```


- Uses the provided` config.yaml` to write relationships into SpiceDB
- If the required schema is already in SpiceDB, skip appending it with` --append-schema=false`


#### Example` config.yaml`


yaml


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


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


```text
schema:    |2
definition    customer   {}


definition    contact   {
relation customer:    customer
}


definition    article   {
relation tags:    tags
}


definition    tags   {
relation article:    article
}
tables:
# for each row in the contacts table
-    name:    contacts
relationships:
# generate a relationship contact:<contact_id> customer customer:<customer_id>_<customer_name>
-    resource_type:    contact
resource_id_cols:
-    contact_id
relation:    customer
subject_type:    customer
subject_id_cols:
-    customer_id
-    customer_name
# for each row in the article_tag table (a join table from articles <-> tags)
-    name:    article_tag
relationships:
# generate a relationship article:<article_id> tags tag:<tag_id>
-    resource_type:    article
resource_id_cols:
-    article_id
relation:    tags
subject_type:    tags
subject_id_cols:
-    tag_id
# generate a second relationship tags:<tag_id> article article:<article_id>
-    resource_type:    tags
resource_id_cols:
-    tag_id
relation:    article
subject_type:    article
subject_id_cols:
-    article_id


```


yaml


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


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


```text
schema:    |2
definition    customer   {}


definition    contact   {
relation customer:    customer
}


definition    article   {
relation tags:    tags
}


definition    tags   {
relation article:    article
}
tables:
# for each row in the contacts table
-    name:    contacts
relationships:
-    resource_type:    contact
resource_id_cols:
-    contact_id
relation:    customer
subject_type:    customer
subject_id_cols:
-    customer_id
-    customer_name
# for each row in the article_tag table (a join table from articles <-> tags)
-    name:    article_tag
relationships:
# generate a relationship article:<article_id> tags tag:<tag_id>
-    resource_type:    article
resource_id_cols:
-    article_id
relation:    tags
subject_type:    tags
subject_id_cols:
-    tag_id
# generate a second relationship tags:<tag_id> article article:<article_id>
-    resource_type:    tags
resource_id_cols:
-    tag_id
relation:    article
subject_type:    article
subject_id_cols:
-    article_id


```


### Caveats


The PostgreSQL importer comes with some caveats, which is why we've labeled it an` experiment` :


- The queries it emits are simple, and may be insufficient for very large datasets
- Object IDs can only be generated from a set of column values, each interpted as the PostgreSQL` ::text` type.


If you'd like to discuss your use-case and potential solutions to these issues, feel free to[file an issue](https://github.com/authzed/connector-postgresql) or come[chat with us](https://authzed.com/discord) in discord.


## The Road to Connectors


` import` solves the problem of bootstrapping an application with existing data. After bootstrapping, the application should switch to reading and writing relationships in SpiceDB directly. For many cases, this is enough.


But there may be times when the data or application you want to use is not entirely under your control or when transitioning to SpiceDB clients would be too difficult to coordinate.


For those cases, we intend to support first-class connectors: services that run continuously to sync data from an external source into SpiceDB.


Over the coming months, we plan to explore the solution space for syncronizing external data into SpiceDB. If you'd like to discuss these problems with us, please[drop us a line](https://authzed.com/discord) in discord. We'd love to hear from you!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Introducing zed import
- Postgres Import
- Self-Driving
- Dry-Run
- Custom Config
- Example config.yaml
- Caveats
- The Road to Connectors
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
