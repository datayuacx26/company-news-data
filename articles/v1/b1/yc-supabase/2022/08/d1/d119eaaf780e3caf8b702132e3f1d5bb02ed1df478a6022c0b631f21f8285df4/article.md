---
schema_version: "1.0.0"
document_id: "d119eaaf780e3caf8b702132e3f1d5bb02ed1df478a6022c0b631f21f8285df4"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/pg-jsonschema-a-postgres-extension-for-json-validation"
published_at: "2022-08-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:c67bb380d42f8caf3089663c8948086780a4d438a71b0424cf22e6e93bf1dc45"
---

# pg_jsonschema: JSON Schema support for Postgres

Released on the Supabase platform today,[pg_jsonschema](https://github.com/supabase/pg_jsonschema) is a Postgres extension which adds[JSON Schema](https://json-schema.org/) validation support for` json` and` jsonb` data types.


## The use-case for JSON validation#


Despite Supabase being an SQL shop, even our most zealous relational data model advocates (begrudgingly) recognize some advantages to the document data model. Mainly, if some complex data will always be consumed together, a document data type can be a good fit.


**For Example** :


If our application receives data via a webhook:


`
_21


{


_21


"status_code": 200,


_21


"checksum": "89b623f6332d2b9b42b4e17eaf1bcc60"


_21


"headers": {


_21


"Content-Type": "application/json",


_21


"Last-Modified": "Tue, 09 Aug 2022 09:14:10 GMT"


_21


},


_21


"payload": {


_21


{


_21


"success": true,


_21


"content": {


_21


"account_id": "d928b484-16bd-4f10-a827-3eb959b4cc14",


_21


"event": "SUBSCRIBED",


_21


"subscriptions": \[


_21


{"subscription_id": 481, "exp": 1660050940},


_21


{"subscription_id": 121, "exp": 1660041852},


_21


\]


_21


}


_21


}


_21


}


_21


}


`


A reasonable swing at normalizing that data into tables might look like this:


That's a lot of architecting! Moreover, the query to recover the original input requires 5 joins!


A solution that aligns better with our intent would be to persist whatever we receive from the external service so long as it meets a minimum set of requirements. With Postgres'` json` data type we can achieve half of that goal.


Treating the webhook contents as a` json` document simplifies our data model. It is also robust to changing payloads and more efficient to query, update, and delete.


Now what about this part?


> so long as it meets a minimum set of requirements


## Challenges#


The flexibility of document types also comes with some downsides.


The schema of the json payload from the previous example is a little intense for a blog post, so let's instead say we intend for a table's` json` column to hold objects with a` string` attribute named` foo` and no additional attributes.


Without constraints, the setup would be:


`
_10


create table some_table (


_10


id serial primary key,


_10


metadata json not null


_10


);


_10


_10


insert into some_table (metadata)


_10


values (<SQL Input>);


`


But the resulting schema is much more permissive than our intent. When inserting a mix of correct and incorrect values:


Only 2 of our 8 test cases were handled appropriately by our data model.


A core strength of SQL databases is their ability to constrain data's[types](https://www.postgresql.org/docs/current/datatype.html) , nullability,[referential integrity](https://www.postgresql.org/docs/current/tutorial-fk.html) ,[uniqueness](https://www.postgresql.org/docs/current/ddl-constraints.html) , and even[arbitrary developer-defined rules](https://www.postgresql.org/docs/current/sql-createtrigger.html) . Those constraints are a lot to sacrifice to gain the convenience of document types.


Fortunately, the challenge of validating` json` documents isn't specific to SQL databases. NoSQL/Document databases, like MongoDB,[optionally enforce data constraints](https://www.mongodb.com/docs/atlas/app-services/schemas/) so there's plenty of prior art for us to draw from.


## JSON Schema#


[JSON Schema](https://json-schema.org/) is a specification for validating the shape and contents of` json` documents. It can describe constraints for documents similar to those applied by relational databases.


Translating our constraints from the previous example into a JSON Schema we get:


`
_12


// objects with a string attribute


_12


// named foo and no additional attributes


_12


{


_12


"type": "object",


_12


"properties": {


_12


"foo": {


_12


"type": "string"


_12


}


_12


},


_12


"required": \["foo"\],


_12


"additionalProperties": false


_12


}


`


Which is a formal and human-readable description of our intent. A tutorial on the JSON Schema language is out-of-scope for this article but you can find a full introduction in[their guide](https://json-schema.org/understanding-json-schema/index.html) .


So now we have:


✅ flexible document data type →` json`


✅ a language to describe constraints on` json` documents → JSON Schema


❌ a way to enforce JSON Schema constraints on` json` documents in Postgres


## pg_jsonschema#


[pg_jsonschema](https://github.com/supabase/pg_jsonschema) is a Postgres extension that can validate` json` and` jsonb` data types against a[JSON Schema](https://json-schema.org/) . The extension offers two functions:


`
_10


-- Validates a json *instance* against a JSON Schema *schema*


_10


json_matches_schema(schema json, instance json) returns bool


_10


_10


-- Validates a jsonb *instance* against a JSON Schema *schema*


_10


jsonb_matches_schema(schema json, instance jsonb) returns bool


`


We can use those functions in combination with a[check constraint](https://www.postgresql.org/docs/current/ddl-constraints.html) to more completely describe our data model.


`
_24


create table some_table(


_24


id serial primary key,


_24


metadata json not null,


_24


_24


check (


_24


json_matches_schema(


_24


schema :='{


_24


"type": "object",


_24


"properties": {


_24


"foo": {


_24


"type": "string"


_24


}


_24


},


_24


"required": \["foo"\],


_24


"additionalProperties": false


_24


}',


_24


instance := metadata


_24


)


_24


)


_24


);


_24


_24


insert into some_table(metadata)


_24


values


_24


(<SQL input>);


`


With that check constraint in place, we re-run the same test cases:


Now all 8 tests are handled correctly. In cases where records failed to insert, Postgres throws an error referencing the failing constraint.


> ERROR: new row for relation "some_table" violates check constraint "some_table_metadata_check"
>
>
> DETAIL: Failing row contains (1, null).
>
>
> SQL state: 23514


With these tools you can wield the flexibility of` json` /` jsonb` data types without sacrificing the guarantees of a well specified data model!


To get started with` pg_jsonschema` , fire up a new supabase project and enable the extension with


`
_10


create extension pg_jsonschema;


`


or follow the[Docker Compose instructions](https://github.com/supabase/pg_jsonschema#try-it-out) in the[repo's README](https://github.com/supabase/pg_jsonschema/blob/master/README.md) .
