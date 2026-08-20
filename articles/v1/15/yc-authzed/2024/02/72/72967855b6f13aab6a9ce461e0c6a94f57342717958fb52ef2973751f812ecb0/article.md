---
schema_version: "1.0.0"
document_id: "72967855b6f13aab6a9ce461e0c6a94f57342717958fb52ef2973751f812ecb0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/how-to-backup-and-restore-spicedb"
published_at: "2024-02-06T12:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:c898ca8d4e92f051efedecc71d5e6b1d718bb1af3896b2b944a83695293a9cbc"
---

# Backup And Restore Your SpiceDB With zed

The new year brought some new cool features to[zed](https://github.com/authzed/zed) , specifically to backup and restore functionality, so we thought it was a good opportunity to showcase how to use it with your SpiceDB and what the new features look like.


Let's dive right in! 🏊‍♂️


## How SpiceDB Backup / Restore Works


The SpiceDB API exposes two APIs designed to support the ingestion and retrieval of large amounts of data, in addition to the existing CRUD APIs. These APIs are better optimized to support bulk import and export of system data and enable things like disaster recovery or bootstrapping new instances.


You can use the exposed gRPC APIs[BulkImportRelationships](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.ExperimentalService.BulkImportRelationships) and[BulkExportRelationships](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.ExperimentalService.BulkExportRelationships) to build any functionality you'd like on top of it, but if all you are looking for is a simple backup/restore CLI command to integrate into your workflows, we've got your covered: the` zed` CLI tool conveniently exposes functionality to create, restore, extract, and redact backups.


` zed` keeps all the data in an Apache Avro container which efficiently stores the schema, the snapshot revision of the backup, and relationships at that revision.


Your entry point to the wonders of SpiceDB backups is` zed backup` , which will show you the existing sub-commands


bash


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
Create, restore, and inspect Permissions System backups


Usage:
zed backup <filename> [flags]
zed backup [ command  ]


Available Commands:
create              Backup a permission system to a file
parse-relationships Extract the relationships from a backup file
parse-revision      Extract the revision from a backup file
parse-schema        Extract the schema from a backup file
redact              Redact a backup file to remove sensitive information
restore             Restore a permission system from a file


```


bash


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
Create, restore, and inspect Permissions System backups


Usage:
zed backup <filename> [flags]
zed backup [ command  ]


Available Commands:
create              Backup a permission system to a file
parse-relationships Extract the relationships from a backup file
parse-revision      Extract the revision from a backup file
parse-schema        Extract the schema from a backup file
redact              Redact a backup file to remove sensitive information
restore             Restore a permission system from a file


```


- ` create` will start the creation of a backup
- ` restore` will restore the backup into SpiceDB, including schema and relationships
- ` parse-*` command allows you to extract data from the backup file. You can pipe it into other processes!
- ` redact` will take a zed backup file, and redact schema definitions, relations, permissions, caveats, and relationship data


## How to create a SpiceDB Backup


Creating a SpiceDB backup starts by installing` zed` on your machine, which is described[here](https://github.com/authzed/zed?tab=readme-ov-file#installing-the-binary) .


Once the command is in place, you should define a "zed context", which defines the connection parameters to a SpiceDB instance. Let's say you have a local SpiceDB instance running:


bash


1


2


```text
zed context  set   dev localhost:50051 my_very_secret_preshared_key --insecure
zed use dev


```


bash


1


2


```text
zed context  set   dev localhost:50051 my_very_secret_preshared_key --insecure
zed use dev


```


Make sure everything is working as intended by issuing a` read schema` request:


bash


1


```text
zed schema  read


```


bash


1


```text
zed schema  read


```


Then creating a backup is as simple as running


bash


1


```text
zed backup create mybackup


```


bash


1


```text
zed backup create mybackup


```


## How to restore a SpiceDB Backup


You probably already guessed it, but restoring a SpiceDB backup is pretty simple!


bash


1


```text
zed backup restore mybackup


```


bash


1


```text
zed backup restore mybackup


```


The restore command will slice and write the backup data in batches of configurable size, and once it completes, it will show stats of the operation:


Restoring a backup is a write-heavy operation, so zed provides some options to handle errors and conflicts


### New Options In SpiceDB Backup Restore


Depending on the type and provisioned capacity of the datastore, restoring a very large SpiceDB backup can take some time, so in the face of a network error, we may lose precious time. Not only that but it can be a hassle to deal with all the relationships provisioned on the initial run, with the system refusing to restore the backup due to the conflicting relationships.


While a restore operation can be done on a live SpiceDB system, the most common scenarios we've seen are:


- seeding a brand new SpiceDB instance
- restoring a snapshot in a different instance for troubleshooting or migration purposes


To help with these tasks,` zed` has now some new tricks up its sleeve to help you in your endeavor:


- **Handle Conflicts** :` --conflict-strategy` allows you to define how to handle the conflict that arises when trying to restore a relationship that already exists. The default until now has been to fail the restore operation, but you can now also skip those over, or write them with` touch` semantics instead of` create` . The default continues to be` fail` to honor the original behavior.
- **Automatic Retries** : Retries on serialization errors are now enabled by default. When the backing datastore returns serialization errors,` zed` will now retry with a backoff by default. It's now enabled by default, but you can go back to the original behavior of failing fast with` --disable-retries`
- **Restore Subsets of a Backup** : You can now restore a portion of your backup SpiceDB instance with a prefix filter. This is useful if you have multiple teams working on the same schema and their namespaces are separated by a prefix.


bash


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
zed backup restore -- help
Restore a permission system from a file


Usage:
zed backup restore <filename> [flags]


Flags:
--conflict-strategy string       strategy used when a conflicting relationship is found. Possible values: fail, skip,  touch   (default fail)
--disable-retries                retries when an errors is determined to be retryable (e.g. serialization errors)
--prefix-filter string           include only schema and relationships with a given prefix


```


bash


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
zed backup restore -- help
Restore a permission system from a file


Usage:
zed backup restore <filename> [flags]


Flags:


```


As a bonus,` zed backup create` also supports the` --prefix-filter` , so it will only create a backup of a specific subset of relationships!


## How To Obfuscate / Redact A SpiceDB Backup


Sometimes a certain issue can only be reproduced with the state of production data. If only we could use it for your tests or benchmarking. Well, you now can! The new Backup Redaction, available from[zed v0.16.X](https://github.com/authzed/zed/releases/tag/v0.16.4) **takes an unredacted backup and obfuscates all the data present** in it so you can:


- share it with anyone
- check it into your SCM and use it as part of your CI
- use it for load-testing
- mirror production data in a different system without concerns over customer data


bash


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
zed backup redact -- help
Redact a backup file to remove sensitive information


Usage:
zed backup redact <filename> [flags]


Flags:
--redact-definitions   redact definitions (default  true  )
--redact-object-ids    redact object IDs (default  true  )
--redact-relations     redact relations (default  true  )


```


bash


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
zed backup redact -- help
Redact a backup file to remove sensitive information


Usage:
zed backup redact <filename> [flags]


Flags:
--redact-definitions   redact definitions (default  true  )
--redact-object-ids    redact object IDs (default  true  )
--redact-relations     redact relations (default  true  )


```


## How To Extract Information From A SpiceDB Backup


Once you have a SpiceDB backup file, you can inspect its contents using` zed` tool, and pipe its output into whatever shell workflows you may have built, as it's built with Unix program design principles in mind:


### How to extract the schema out of a SpiceDB backup


To print the schema of a SpiceDB backup, run the following command


bash


1


```text
zed backup parse-schema mybackup


```


bash


1


```text
zed backup parse-schema mybackup


```


### How to extract the snapshot revision out of a SpiceDB backup


To print the snapshot revision of a SpiceDB backup, run the following command


bash


1


```text
zed backup parse-revision mybackup


```


bash


1


```text
zed backup parse-revision mybackup


```


### How to extract all relationships out of a SpiceDB backup


To print all the relationships present in a SpiceDB backup, run the following command:


bash


1


```text
zed backup parse-relationships mybackup


```


bash


1


```text
zed backup parse-relationships mybackup


```


## Bonus Feature! ⭐ Context-Aware Shell Completion


This is a new backup-adjacent but cool feature that made it's way in the 0.16.0 release.` zed` already supports shell completion for the *static* parts of the CLI, mainly commands and subcommands. But now it also supports shell completion of certain dynamic parts:


- ` zed context use` will autocomplete the various context defined
- ` zed permission` and` zed relationship` will autocomplete resource types, permission names of that resource, and subject types by querying the currently configured` zed` context.


This allows user more effectively construct commands based on the existing schema in your schema. No more back and forth between terminal and schema file!


Got questions about` zed` ? Reach out to us via[Discord](https://authzed.com/discord) ,[Twitter](https://twitter.com/authzed) or[Linkedin](https://www.linkedin.com/company/authzed/) !


On this page


- How SpiceDB Backup / Restore Works
- How to create a SpiceDB Backup


## Related


[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)[Product November 2023 Product Updates As the days get shorter and the air crisper, we're stoked to bring you some warm and exciting product updates! Jess Hustace · Dec 1, 2023 · 3 min](https://authzed.com/blog/november-2023-product-updates)


[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)[Product Fall 2023 Product Update As a company, AuthZed has been building the team, onboarding new customers, and improving our product offerings. We've also been shipping tons of new features and improvements in the SpiceDB project and growing the open source community. Jess Hustace · Nov 3, 2023 · 4 min](https://authzed.com/blog/fall-2023-product-update)


[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Victor Roldan Betancort · Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)
