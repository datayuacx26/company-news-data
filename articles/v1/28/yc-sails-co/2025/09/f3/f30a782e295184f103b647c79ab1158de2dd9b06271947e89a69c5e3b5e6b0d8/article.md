---
schema_version: "1.0.0"
document_id: "f30a782e295184f103b647c79ab1158de2dd9b06271947e89a69c5e3b5e6b0d8"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/waterline-features/"
published_at: "2025-09-22T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:7f4fd4b7c603cc2b3d4cefcd443f81728a66691cd398884f796856cca1e4bb7d"
---

# Waterline Features

In the[previous article on Interfaces](https://blog.sailscasts.com/waterline-interfaces) , we looked at **the baseline contract** every Waterline adapter must fulfill. Those interfaces — like` semantic` ,` queryable` , and` associations` — are non-negotiable. Without them, an adapter can’t even claim to be Waterline-compatible.


But databases are not all created equal. Some have special powers: PostgreSQL can enforce unique constraints in the database, MySQL can generate sequential IDs automatically, and PostGIS can store actual maps and shapes.


That’s where **Features** come in.


If Interfaces are like a **driver’s license test** (everyone must pass to drive on the same roads), then Features are like **optional endorsements** — motorcycle license, pilot’s license, deep-sea diving certificate. Not every adapter supports them, but when they do, they unlock powerful, database-specific capabilities.


## Available Features


Here’s the snapshot of Features Waterline defines today:


- **Unique**


- Database-level uniqueness enforcement (` E_UNIQUE` errors).


- **Auto-increment**


- Automatic primary key or field value assignment.
- **Sub-feature:**` autoIncrement.sequential` → guarantees strictly sequential IDs.


- **Composite Unique**


- Multi-column uniqueness constraints (e.g.` (email, orgId)` ).


- **Composite Primary Key**


- Multi-column primary key support.


- **Spatial**


- Geospatial data types (` POINT` ,` LINESTRING` ,` POLYGON` ).


- **Cross-adapter**


- Associations and queries across multiple database adapters.


## Deep Dive into Features


### 1. Unique – Database Constraint Enforcement


While Waterline models can do app-level validation, the` unique` feature goes further: it ensures **true database-level guarantees** .


Think of it like a nightclub with a bouncer. You can put a sign outside saying *“one entry per person”* (model validation), but the bouncer at the door (the database constraint) is the one who really enforces it.


- Prevents duplicate inserts
- Ensures updates won’t create duplicates
- Persists across migrations and restarts


This differs from Interfaces because it’s not about **universal ORM behavior** — it’s about leveraging the database’s own enforcement.


### 2. Auto-increment – Automatic Value Generation


The` autoIncrement` feature validates automatic unique value assignment.


Databases like MySQL and PostgreSQL can generate IDs for you without your app having to think about it. This is especially important for primary keys.


- Assigns unique IDs when none are provided
- Allows manual overrides when needed
- Works across migrations


**Sub-feature:` autoIncrement.sequential`** Some databases guarantee values are not just unique but also sequential. This is like having numbered raffle tickets handed out one after another — no gaps unless someone takes a manual leap forward.


### 3. Composite Unique – Multi-Column Constraints


Imagine you’re running a university. Students can reuse the same email across different campuses, but within the same campus it must be unique. That’s where **composite uniqueness** comes in: uniqueness is enforced across a combination of fields, like` (email, campusId)` .


Currently this feature is marked as` describe.skip` in Waterline’s test suite — meaning not fully implemented yet, but it shows the intent.


### 4. Composite Primary Key – Multi-Column Primary Keys


Most databases (and Waterline models) use a single` id` column as the primary key. But some use **composite primary keys** , e.g.` (countryCode, phoneNumber)` together form the unique identity of a record.


This feature ensures an adapter can handle those setups. It’s also currently skipped in Waterline’s tests, but important for certain database designs.


### 5. Spatial – Geographic Data Support


The` spatial` feature is about geospatial data — storing maps, points, and polygons natively in the database.


For example, PostgreSQL with PostGIS can store the coordinates of a delivery zone, or the outline of a city.


Waterline’s spatial feature tests:


- Storage of geometries (` POINT` ,` LINESTRING` ,` POLYGON` )
- SRID-aware coordinate systems (e.g. EPSG:4326)
- Round-trip integrity (save → query → get the same geometry back)


This is a perfect example of a database-specific superpower exposed cleanly through Waterline.


### 6. Cross-adapter – Multi-Database Operations


Most ORMs assume you’re only talking to one database. But in the real world, you might have users in MongoDB and orders in MySQL.


The` cross-adapter` feature validates that Waterline can:


- Handle associations across different adapters
- Perform queries that touch multiple connections
- Keep connections isolated while still working together


It’s ambitious, but it shows how far Waterline pushes database abstraction.


## Interfaces vs Features


To tie it back:


- **Interfaces** = the *universal rules* (everyone must implement them).
- **Features** = the *bonus powers* (adapters may or may not support them). Together, they form a flexible system: every adapter behaves consistently where it matters, but still has room to showcase what makes its database special.


## Conclusion


Waterline Features give us a way to benefit from **database-native strengths** — whether that’s true uniqueness constraints, spatial data, or cross-adapter queries — without losing the portability and consistency guaranteed by Interfaces.


By separating the *mandatory contract* (Interfaces) from the *optional enhancements* (Features), Waterline strikes a balance between **universal ORM behavior** and **database-specific power** .


Next up in this series, we’ll zoom out from interfaces and features to explore **connections and datastores** — how Waterline adapters actually bind to live databases and how multiple datastores coexist in a single app.
