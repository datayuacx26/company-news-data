---
schema_version: "1.0.0"
document_id: "cd33ebd5f6887bd9ada8fac8ed92fc4d936468d139ddcb93cf48517f7336b9fe"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/relational-database-design-mistakes/"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T12:34:13.837817+00:00"
fetched_at: "2026-07-29T12:34:15.089730+00:00"
content_hash: "sha256:a96733620711bb88062903f9063c63d73072b1f5110fca32a933c09f1016bdee"
---

# Common Relational Database Design Mistakes

Most relational schema mistakes are really one mistake: trusting the application to enforce what the database should guarantee. A foreign key you “handle in the service layer,” a uniqueness rule you check with a` SELECT` before` INSERT` , a status field your ORM validates but the column does not — each one is a constraint you moved out of the engine that was built to enforce it and into code that runs only when you remember to call it. The result is data the schema would never have allowed: orphaned rows, duplicate accounts, negative prices, timestamps no one can interpret. This guide walks through the recurring design mistakes application developers make on Postgres and MySQL, why each one bites, and the fix — with a small before/after snippet for each. The examples use modern Postgres 18 syntax where it matters, but the principles apply to any relational engine.


## Key Takeaways


- Enforce integrity in the database with` FOREIGN KEY` ,` NOT NULL` ,` CHECK` , and` UNIQUE` constraints; application-only validation runs only when the code remembers to call it, and an ORM that lets you declare a relation without a database-level foreign key is deferring an integrity bug to production.
- Use a surrogate primary key (` bigint` identity or a` uuidv7()` UUID) and add a separate` UNIQUE` constraint on the natural key, so a rebrand or email change is an` UPDATE` , not a migration.
- As of PostgreSQL 18 (released September 25, 2025) you no longer need an extension for time-ordered identifiers:` id uuid PRIMARY KEY DEFAULT uuidv7()` gives index locality close to a` bigint` while staying globally unique.
- Store timestamps as` timestamptz` , not` timestamp` , and index the foreign-key and filter columns you actually join on — Postgres does not index the referencing side of a foreign key automatically.
- Treat the schema as living code: version it with migrations, prefer soft deletes (` deleted_at timestamptz` ) where other rows reference a record, and document columns with` COMMENT ON COLUMN` .


## The root of most relational database design mistakes: rules in app code, not the schema


The highest-impact mistake is enforcing data rules only in application code. A constraint in your service layer protects exactly one code path; a constraint in the schema protects every path — your API, a background job, a manual` psql` session, a botched data migration, and the next service someone writes against the same database. When the rule lives only in the app, the first writer that bypasses it corrupts the table permanently.


This is the modern ORM trap. Tools like[Prisma](https://www.prisma.io/docs) and[Drizzle](https://orm.drizzle.team/) happily model a relation in application code without ever emitting a database-level foreign key, and a` @default` or a Zod schema feels like validation. It is not. An ORM that lets you declare a relation without a database-level foreign key isn’t saving you work; it’s deferring a data-integrity bug to production, where session replay — not your test suite — is what finally catches it, because the failure isn’t a stack trace. The corrupted data surfaces instead as a review attributed to a user who no longer exists, or a duplicated order — a confusing screen the user actually sees.


The fix is to push the rule down to where it can’t be skipped. PostgreSQL’s check constraints, for example, reject bad data regardless of which client wrote it: to require positive product prices, you could use a` CHECK (price > 0)` constraint in the table definition.


```text
-- Before: nothing stops a negative payment or a NULL email
CREATE   TABLE   payments   (
id        bigint   GENERATED   ALWAYS   AS   IDENTITY   PRIMARY KEY  ,
user_id   bigint  ,                   -- no FK: orphans waiting to happen
amount    numeric  ,                  -- no CHECK: negatives allowed
email     text                      -- no NOT NULL, no UNIQUE
);


-- After: the engine guarantees the invariants
CREATE   TABLE   payments   (
id        bigint   GENERATED   ALWAYS   AS   IDENTITY   PRIMARY KEY  ,
user_id   bigint   NOT NULL   REFERENCES   users (id)   ON DELETE   RESTRICT,
amount    numeric   NOT NULL   CHECK   (amount   >   0  ),
email     text   NOT NULL   UNIQUE
);
```


A foreign key is not a performance tax — it’s the only thing standing between you and orphaned rows that render as a review for a user who no longer exists. As of Postgres 18 you can also stage constraints on busy tables: ALTER TABLE can now set the NOT VALID attribute of NOT NULL constraints, letting you add the constraint without an immediate full-table scan and validate it later under a weaker lock.


## Skipping normalization — or over-applying it


Normalization is the practice of storing each fact once. First normal form (1NF) means no repeating groups and no multi-value fields; third normal form (3NF) means every non-key column depends on the key and nothing else. For application schemas, 3NF is the sensible default — see the[Postgres data-definition docs](https://www.postgresql.org/docs/current/ddl.html) for the mechanics. The two classic violations are comma-separated values in one column and repeating columns like` payment_1 … payment_12` .


Comma-separated values in a single column is a denormalization you will regret on the first` WHERE … LIKE '%,42,%'` ; the fix is a child table or a junction table, not a cleverer string query.


```text
-- Before: tags as a delimited string, phone numbers as columns
CREATE   TABLE   users   (
id       bigint   PRIMARY KEY  ,
tags     text  ,             -- 'admin,beta,vip' — unqueryable, unconstrained
phone1   text  , phone2   text  , phone3   text     -- repeating group, runs out
);


-- After: a junction table for tags, a child table for phones
CREATE   TABLE   user_tags   (
user_id   bigint   NOT NULL   REFERENCES   users (id)   ON DELETE CASCADE  ,
tag       text     NOT NULL  ,
PRIMARY KEY   (user_id, tag)
);
CREATE   TABLE   user_phones   (
id        bigint   GENERATED   ALWAYS   AS   IDENTITY   PRIMARY KEY  ,
user_id   bigint   NOT NULL   REFERENCES   users (id)   ON DELETE CASCADE  ,
phone     text     NOT NULL
);
```


The inverse error is over-normalization: splitting a single` address` into six joined tables, or modeling a one-to-one as two tables for no reason. Each extra join is query and cognitive cost. Normalize until each fact lives once, then stop.


## Using business fields as primary keys


Don’t use a business field like` email` ,` username` , or` company_name` as a primary key; use a surrogate key and add a separate` UNIQUE` constraint on the natural value, so a rebrand or address change is an` UPDATE` , not a migration. Business values change, and when a primary key changes, every foreign key referencing it has to cascade — a value that should never have been an identity becomes a schema-wide migration.


The 2007-era advice to avoid surrogate keys is stale for application developers. The modern default is a surrogate primary key plus a real` UNIQUE` constraint on the natural key — you get a stable identity *and* the uniqueness guarantee.


```text
CREATE   TABLE   companies   (
id   uuid   PRIMARY KEY   DEFAULT   uuidv7(),     -- stable surrogate identity
name   text   NOT NULL   UNIQUE                   -- natural key still enforced
);
```


### UUID vs bigint, decided


As of Postgres 18 you no longer need an extension for time-ordered identifiers: Postgres 18 adds the` uuidv7()` UUID-generation function, whose value is temporally sortable, along with a` uuidv4()` alias for explicitly generating version 4 UUIDs. UUIDv7 — defined in[RFC 9562](https://www.rfc-editor.org/info/rfc9562/) (May 2024, which obsoletes RFC 4122) — embeds a timestamp prefix, so new rows append to the right of the index instead of scattering like random v4 UUIDs. Note that for older Postgres,` gen_random_uuid()` (v4) has been in core since version 13; the[pgcrypto extension](https://www.postgresql.org/docs/current/pgcrypto.html) is no longer required.


Key type Storage Index locality Globally unique Notes


` bigint` identity 8 bytes Excellent (sequential) No Smallest, fastest; leaks row counts; needs central allocation


` uuidv4()` 16 bytes Poor (random inserts) Yes Distributed-friendly; index fragmentation under heavy write


` uuidv7()` 16 bytes Good (time-ordered) Yes Time-sortable; leaks creation time; the strong default in PG18


Default to` bigint` when ids stay inside one database, and to` uuidv7()` when you generate ids client-side or across services.


## No deliberate indexing strategy


Index the columns you actually filter and join on — especially foreign keys, which Postgres does *not* index automatically — but stop short of indexing every column, because each index is write amplification on every insert and update. This is the single most common performance regression in application schemas, and it is easy to get wrong in both directions.


The trap is specific to Postgres: it auto-indexes the *referenced* (parent) side of a foreign key but not the *referencing* (child) column. Because indexing the referencing columns is not always needed and there are many choices on how to index, declaring a foreign key constraint does not automatically create an index on the referencing columns. (MySQL’s InnoDB *does* auto-create one — so this footgun is Postgres-specific.)


```text
-- payments.user_id is a FK but unindexed: every join and every
-- DELETE on users does a sequential scan of payments
CREATE   INDEX   ON   payments (user_id);
```


Index the FK and filter columns; skip indexes on low-selectivity columns and on tables you rarely query by that field.


## One table doing many jobs


A single table forced to model many domains — the generic entity-attribute-value (EAV) table, or one` notifications` table holding alerts, audit events, and system logs — trades schema clarity for a false flexibility. You lose type safety, you can’t apply meaningful constraints, and every read becomes a filtered, self-joined mess. Use separate tables per domain so each gets its own columns, types, and foreign keys.


```text
-- Before: one bag of everything, typed as text, no real constraints
CREATE   TABLE   entity_attributes   (
entity_id   bigint  , attr_name   text  , attr_value   text
);


-- After: real tables with real columns and constraints
CREATE   TABLE   user_alerts    (id   bigint   GENERATED   ALWAYS   AS   IDENTITY   PRIMARY KEY  ,
user_id   bigint   NOT NULL   REFERENCES   users(id),
level   text   NOT NULL  , body   text  );
CREATE   TABLE   audit_events   (id   bigint   GENERATED   ALWAYS   AS   IDENTITY   PRIMARY KEY  ,
actor_id   bigint   REFERENCES   users(id),
action   text   NOT NULL  ,
occurred_at   timestamptz   NOT NULL   DEFAULT   now  ());
```


If you genuinely need flexible attributes, reach for a typed` jsonb` column on the real table — not a string-typed EAV that defeats every constraint the engine offers.


## Inconsistent naming


Pick one naming convention and apply it everywhere. For Postgres and most ORMs that means:


- ` snake_case` (unquoted identifiers fold to lowercase);
- plural or singular table names chosen once;
- ` created_at` /` updated_at` for timestamps;
- no metadata in names (` tbl_users` ,` col_varchar_address` );
- no spaces or quotes; and
- no[reserved words](https://www.postgresql.org/docs/current/sql-keywords-appendix.html) like` user` ,` order` , or` group` as bare identifiers.


Naming is the cheapest thing to get right at table-create time and the most expensive to change once data and queries depend on it.


## Treating the schema as final


A schema is living code, not a one-time artifact. Manage every change through versioned, reviewed, reversible migrations — your ORM’s migration tool (Prisma Migrate,[Drizzle Kit](https://orm.drizzle.team/docs/kit-overview) ) or a standalone runner — so the schema’s history is reproducible across environments.


History also dictates how you delete. Prefer a soft delete (a` deleted_at timestamptz` ) over a hard` DELETE` when other rows reference the record, because a hard delete either orphans children or, with` ON DELETE CASCADE` , quietly erases history you’ll later be asked to produce.


```text
ALTER   TABLE   orders   ADD   COLUMN deleted_at   timestamptz  ;
-- "Active orders" becomes a filter, not a destructive operation
CREATE   VIEW   active_orders   AS   SELECT   *   FROM   orders   WHERE   deleted_at   IS   NULL  ;
```


## Storing datetimes without time zones


Store timestamps as` timestamptz` , not` timestamp` — a column without a time zone silently records whatever the server happened to think “now” was, and that ambiguity becomes unrecoverable the moment your servers span regions.` timestamptz` stores an absolute point in time (normalized to UTC) and renders it in the session’s zone;` timestamp` stores a wall-clock reading with no anchor. See the[Postgres date/time docs](https://www.postgresql.org/docs/current/datatype-datetime.html) . For schedules and bookings, Postgres 18 also added temporal constraints: it supports non-overlapping` PRIMARY KEY` ,` UNIQUE` , and foreign key constraints, specified with` WITHOUT OVERLAPS` and` PERIOD` . (Temporal foreign keys do not support` ON DELETE` /` ON UPDATE` cascade actions, so don’t rely on cascades there.)


## No documentation or schema tests


An undocumented, untested schema is a mistake that compounds silently. Magic values are the classic symptom: a` status_code` of 1–3 that breaks the day someone adds 4, with no record of what any number means. The remedies are concrete:


- document intent in the database itself with` COMMENT ON COLUMN` ;
- keep an ER diagram in the repo; and
- test migrations the way you test code — apply, roll back, and assert against fringe and invalid input.


```text
COMMENT ON COLUMN orders.status IS
'  enum: pending|paid|shipped|cancelled — see app/orders/status.ts  '  ;
```


A` CHECK (status IN (...))` constraint plus a comment turns a fragile integer into a self-describing, self-enforcing column.


## Conclusion


Every mistake here collapses into the same fix: make the database — not the application, and not the ORM — the authority on what data is valid. Add the foreign key, the` NOT NULL` , the` CHECK` , and the` UNIQUE` constraint before you write the query that assumes them. Start with your most-written table: list the invariants you currently enforce in code, and move each one into the schema where it can’t be skipped.


## FAQs


When should I use a UUID primary key instead of a bigint identity column?


Use a bigint identity when ids are generated inside a single database and never need to be created by clients, because it is 8 bytes, indexes sequentially, and is the fastest option. Use a UUID, specifically uuidv7() in Postgres 18, when you generate ids client-side or across multiple services and need global uniqueness. UUIDv7 keeps near-sequential index locality because it embeds a timestamp prefix, unlike random UUIDv4.


Does declaring a foreign key in my ORM also create a database-level constraint?


Not necessarily. Tools like Prisma and Drizzle can model a relation in application code without emitting a database-level FOREIGN KEY constraint, so the relationship exists only in the ORM's understanding and not in the engine. Without the database constraint, a background job, manual query, or another service can create orphaned rows the ORM never anticipated. Confirm that your migration actually generates a REFERENCES clause rather than trusting the relation declaration alone.


What is the difference between timestamp and timestamptz in PostgreSQL?


timestamptz stores an absolute point in time normalized to UTC and renders it in the session's time zone, while timestamp stores a wall-clock reading with no time-zone anchor. A plain timestamp silently records whatever the server considered local time, and that ambiguity becomes unrecoverable once servers span regions. Use timestamptz for application datetimes so a value always refers to one unambiguous instant regardless of where it is read.


Do I still need the pgcrypto or uuid-ossp extension to generate UUIDs in Postgres?


No. The gen_random_uuid() function, which produces a version 4 UUID, has been part of core PostgreSQL since version 13, so no extension is required. The PG18 pgcrypto documentation now marks its own gen_random_uuid() as obsolete because it calls the core function. As of Postgres 18 you also get built-in uuidv7() for time-ordered identifiers and a uuidv4() alias, all without installing any extension.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
