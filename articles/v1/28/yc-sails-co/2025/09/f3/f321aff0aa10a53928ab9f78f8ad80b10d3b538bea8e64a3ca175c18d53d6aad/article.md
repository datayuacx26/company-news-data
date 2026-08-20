---
schema_version: "1.0.0"
document_id: "f321aff0aa10a53928ab9f78f8ad80b10d3b538bea8e64a3ca175c18d53d6aad"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/waterline-adapters/"
published_at: "2025-09-19T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:ff2b4eb890d727bbca3915883ded6d75ce57f12c6843653bf6c843e4d11df2c3"
---

# Waterline Adapters

In the[first part of this series](https://blog.sailscasts.com/what-is-waterline) , we introduced Waterline as a database-agnostic ORM for Node.js. You define models once, and they can talk to multiple databases without rewriting your logic.


But how does Waterline actually “speak” to those databases? That’s where **adapters** come in.


## What Are Adapters?


Adapters are translators. Your Waterline models speak a single, consistent API — methods like` find()` ,` create()` ,` update()` , and` destroy()` . But MySQL, MongoDB, Redis, and PostgreSQL each have their own way of handling queries.


An adapter sits in the middle, turning Waterline’s universal API calls into the dialect of the target database.


So when you call:


```text
await   User.  find  ({ age: {   '>'  :   30   } });
```


- With the MySQL adapter, that becomes a` SELECT` query.
- With the MongoDB adapter, it turns into a JSON query object.
- With Redis, it maps to the right key/value operations.


## A Note on the Adapter Pattern


If you’ve studied software design patterns, the term adapter might sound familiar. Waterline adapters are a textbook example of the[Adapter Pattern](https://en.wikipedia.org/wiki/Adapter_pattern) : taking the interface your application expects and translating it into the interface a database understands.


This is why you can switch from MySQL to MongoDB without rewriting business logic — the adapter handles the translation.


## Available Adapters


Waterline works with a variety of adapters, including:


- [sails-mysql](https://github.com/balderdashy/sails-mysql) – for MySQL and MariaDB
- [sails-postgresql](https://github.com/balderdashy/sails-postgresql) – for PostgreSQL
- [sails-mongo](https://github.com/balderdashy/sails-mongo) – for MongoDB
- [sails-sqlite3](https://github.com/sailscastshq/sails-sqlite) – for SQLite
- [sails-disk](https://github.com/balderdashy/sails-disk) – a simple file-based adapter for development and prototyping. It also has a memory mode, great for testing.


There are also community-contributed adapters for services like Microsoft SQL Server, Oracle, Elasticsearch, and Neo4j.


## Why Adapters Matter


Adapters give Waterline its unique flexibility:


- **Portability** – Switch databases without rewriting business logic.
- **Consistency** – Use the same methods across projects.
- **Extendability** – Write a new adapter if your database isn’t covered.


This makes Waterline a safe bet for projects where storage strategies may change over time.


## How Adapters Work Internally


Every adapter follows a contract (we’ll cover this in detail in the[next article on Waterline Interfaces](https://blog.sailscasts.com/waterline-interfaces) ). At a high level, an adapter needs to:


1. Implement the core CRUD methods (find, create, update, destroy).
2. Handle connections and configuration.
3. Translate Waterline criteria into database-specific queries.


It’s essentially the bridge between the universal and the specific.


## Wrapping Up


Adapters are the reason Waterline can work with so many databases. They do the heavy lifting of translation, letting you focus on writing application logic instead of database syntax.


In the[next article](https://blog.sailscasts.com/waterline-interfaces) , we’ll explore **interfaces** — the contracts that every adapter must follow.
