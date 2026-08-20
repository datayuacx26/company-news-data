---
schema_version: "1.0.0"
document_id: "b7335e69a941d634b1a5be9646e5a1bf6f844ee88815aea1f1b26487a48e15f2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-postgres-fdw"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:54:21.606053+00:00"
content_hash: "sha256:c41039be737095d7d698a4574e85ec85edda3d02f97cb03aeb2d4bc4c9e97552"
---

# Interact with SpiceDB Seamlessly from within PostgreSQL using the SpiceDB Foreign Data Wrapper

Today we're happy to introduce the SpiceDB Foreign Data Wrapper (FDW) for PostgreSQL, a new experimental way to bring real-time authorization context from SpiceDB into Postgres queries, all without duplicating data, rewriting policies, or embedding authorization logic where it doesn't belong.


Starting with SpiceDB v1.49.0, the SpiceDB FDW allows recent versions of PostgreSQL to query SpiceDB at runtime, exposing permissions and relationships as foreign tables backed by SpiceDB's APIs, with near-seamless integration.


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


18


19


20


21


22


23


```text
-- First, create a local table with document metadata
CREATE TABLE   document (
id text  PRIMARY KEY  ,
title text  NOT NULL  ,
contents text  NOT NULL
);


-- Insert some documents
INSERT INTO   document (id, title, contents)  VALUES
( 'firstdoc'  ,  'Document 1'  ,  'Contents of document 1'  ),
( 'seconddoc'  ,  'Document 2'  ,  'Contents of document 2'  ),
( 'thirddoc'  ,  'Document 3'  ,  'Contents of document 3'  );


-- Join local documents with permissions
-- to find which documents a specific user can view
SELECT   document.id, document.title
FROM   document
JOIN   permissions  ON   permissions.resource_id  =   document.id
WHERE   permissions.resource_type  =    'document'
AND   permissions.permission  =    'view'
AND   permissions.subject_type  =    'user'
AND   permissions.subject_id  =    'alice'
ORDER    BY   document.title  DESC  ;


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


18


19


20


21


22


23


```text
-- First, create a local table with document metadata
CREATE TABLE   document (
id text  PRIMARY KEY  ,
title text  NOT NULL  ,
contents text  NOT NULL
);


-- Insert some documents
INSERT INTO   document (id, title, contents)  VALUES
( 'firstdoc'  ,  'Document 1'  ,  'Contents of document 1'  ),
( 'seconddoc'  ,  'Document 2'  ,  'Contents of document 2'  ),
( 'thirddoc'  ,  'Document 3'  ,  'Contents of document 3'  );


-- Join local documents with permissions
-- to find which documents a specific user can view
SELECT   document.id, document.title
FROM   document
JOIN   permissions  ON   permissions.resource_id  =   document.id
WHERE   permissions.resource_type  =    'document'
AND   permissions.permission  =    'view'
AND   permissions.subject_type  =    'user'
AND   permissions.subject_id  =    'alice'
ORDER    BY   document.title  DESC  ;


```


## Bringing Authorization to Where Your Data Is Accessed


Many applications already store their core application data in PostgreSQL, but authorization decisions are often needed in a variety of other locations, such as in application code, or microservices. As a result, it is often important to have authorization running as a distinct service, one designed to be decoupled, centralized, and consistent.


However, this separation can introduce friction when teams need to write both application data and permissions data, or run permission-aware queries over their application data. Requiring developers to interact with two different systems through two distinct APIs also raises the learning curve for new team members, sometimes adding operational overhead.


## A Better Approach: Query Authorization, alongside Application Data


The SpiceDB FDW takes a different approach by providing a single, unified access point for both application data and permission data.


Once configured, the FDW exposes virtual tables that map directly to SpiceDB's APIs for relationships and permissions.


From PostgreSQL's perspective, these virtual tables behave much like remote versions of standard tables. Under the hood, however, every SQL operation is translated into live calls to the SpiceDB API, fully leveraging SpiceDB's scalability and built-in optimizations.


That all sounds great in theory... but let's see how it works in practice!


## Checking Permissions


As an example, to check a permission in SpiceDB, the following API call can be made:


text


1


```text
CheckPermission(document:1234, view, user:tom)


```


text


1


```text
CheckPermission(document:1234, view, user:tom)


```


Using the FDW, the same permission can be directly checked within PostgreSQL by making a query against the` permissions` virtual table:


sql


1


2


3


4


5


6


7


```text
SELECT    *
FROM   permissions
WHERE   resource_type =  'document'
AND   resource_id =  '1234'
AND   permission =  'view'
AND   subject_type =  'user'
AND   subject_id =  'tom'


```


sql


1


2


3


4


5


6


7


```text
SELECT    *
FROM   permissions
WHERE   resource_type =  'document'
AND   resource_id =  '1234'
AND   permission =  'view'
AND   subject_type =  'user'
AND   subject_id =  'tom'


```


Note that each parameter of the API call to SpiceDB is exposed as a virtual column, on the virtual table.


## Looking up Resources


To find **all** documents that a user might be able to view, the **resource_id** column can simply be elided:


sql


1


2


3


4


5


6


```text
SELECT    *
FROM   permissions
WHERE   resource_type =  'document'
AND   permission =  'view'
AND   subject_type =  'user'
AND   subject_id =  'tom'


```


sql


1


2


3


4


5


6


```text
SELECT    *
FROM   permissions
WHERE   resource_type =  'document'
AND   permission =  'view'
AND   subject_type =  'user'
AND   subject_id =  'tom'


```


## Looking up Subjects


To find all users that can view a document, the **subject_id** can be elided:


sql


1


2


3


4


5


6


```text
SELECT    *
FROM   permissions
WHERE   resource_type =  'document'
AND   resource_id =  '1234'
AND   permission =  'view'
AND   subject_type =  'user'


```


sql


1


2


3


4


5


6


```text
SELECT    *
FROM   permissions
WHERE   resource_type =  'document'
AND   resource_id =  '1234'
AND   permission =  'view'
AND   subject_type =  'user'


```


## Writing Relationships


In addition to querying for permissions, **relationships** in SpiceDB can be managed directly through the FDW via the` relationships` virtual table.


Writes use` INSERT` , which is treated as a` CREATE` :


sql


1


2


3


4


5


6


7


8


```text
INSERT INTO   relationships (
"resource_type",
"resource_id",
"relation",
"subject_type",
"subject_id"
)  VALUES   ("document", "4567", "viewer", "user", "amy")
RETURNING consistency


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


```text
INSERT INTO   relationships (
"resource_type",
"resource_id",
"relation",
"subject_type",
"subject_id"
)  VALUES   ("document", "4567", "viewer", "user", "amy")
RETURNING consistency


```


` TOUCH` can be performed by inserting and ignoring the duplicate:


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


```text
INSERT INTO   relationships (
"resource_type",
"resource_id",
"relation",
"subject_type",
"subject_id"
)  VALUES   ("document", "4567", "viewer", "user", "amy")
RETURNING consistency
ON   CONFLICT DO NOTHING


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


```text
INSERT INTO   relationships (
"resource_type",
"resource_id",
"relation",
"subject_type",
"subject_id"
)  VALUES   ("document", "4567", "viewer", "user", "amy")
RETURNING consistency
ON   CONFLICT DO NOTHING


```


Note the` RETURNING consistency` : this allows the ZedToken for the insert to be returned, without any other calls!


## Reading Relationships


To read relationships, a SQL` WHERE` can be specified on the` relationships` virtual table:


sql


1


2


3


4


```text
SELECT    *
FROM   relationships
WHERE   resource_type =  'document'
AND   resource_id =  '1234'


```


sql


1


2


3


4


```text
SELECT    *
FROM   relationships
WHERE   resource_type =  'document'
AND   resource_id =  '1234'


```


## Deleting Relationships


Deletes use a` DELETE` :


sql


1


2


3


```text
DELETE    FROM   relationships
WHERE   resource_type =  'document'
AND   resource_id =  '8901'


```


sql


1


2


3


```text
DELETE    FROM   relationships
WHERE   resource_type =  'document'
AND   resource_id =  '8901'


```


## Does It Solve Dual Writes?


One common pain point with SpiceDB is[the dual-write problem](https://authzed.com/blog/the-dual-write-problem) . Unfortunately, the FDW doesn't solve it entirely.


Writes to relationships via the FDW are executed in SpiceDB's own transaction rather than as part of the same PostgreSQL transaction. This is a limitation of the[foreign data wrapper protocol](https://www.postgresql.org/docs/current/postgres-fdw.html) , which currently does not support two-phase commit — an issue that applies to all PostgreSQL FDWs today.


That said, if a write to SpiceDB fails, the surrounding PostgreSQL transaction will be rolled back, which significantly reduces the impact of dual writes.


We're hopeful this can be fully addressed in the future if and when PostgreSQL adds native two-phase commit support for FDWs.


## Please Try It Out!


The SpiceDB FDW today is experimental, but it has been fairly well tested and designed with real-world use cases in mind. It represents an early step toward making permissions feel like a first-class part of the data layer.


We plan to continue improving the FDW over time, refining performance, smoothing rough edges, and expanding its capabilities as we learn more.


If this sounds useful, we encourage you to try it out and share your feedback. The documentation covers everything you need to get started, including installation, configuring the connection to your SpiceDB instance, and setting up the foreign tables. Real-world experimentation and community input will play a big role in shaping where the FDW goes next!


On this page


- Bringing Authorization to Where Your Data Is Accessed
- A Better Approach: Query Authorization, alongside Application Data
- Checking Permissions
- Looking up Resources
- Looking up Subjects
- Writing Relationships
- Reading Relationships
- Deleting Relationships
- Does It Solve Dual Writes?
- Please Try It Out!


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)
