---
schema_version: "1.0.0"
document_id: "18c7c85bd077e2336a020f61cec77872cc0bba731dc7bddf9417603cb1538850"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/waterline-interfaces/"
published_at: "2025-09-21T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:8c5014368c182e45867a592a6824af1bdcfc9418240aee18682b6ef28a151895"
---

# Waterline Interfaces

If you missed the previous article, we explored the world of Waterline adapters—how they bridge your Sails app to different databases and what makes a great adapter.


Check it out[here](https://blog.sailscasts.com/waterline-adapters) for a deeper understanding of the adapter landscape before diving into interfaces.


When you write this in a Sails app:


```text
await   User.  create  ({ name:   'Ada Lovelace'   });
```


it feels deceptively simple. But under the hood, an entire *contract system* decides whether that line will succeed, fail, or even be allowed in the first place.


That system is built on **Waterline interfaces** .


As earlier discussed in[What is Waterline](https://blog.sailscasts.com/what-is-waterline) , Waterline is Sails’ first-party ORM. It sits between your app and your database adapters, promising a consistent developer experience whether you’re using PostgreSQL, MongoDB, or MySQL.


The reason that consistency is even possible is because Waterline defines **interfaces** —capability contracts that every adapter must satisfy.


But here’s the important twist: **Waterline doesn’t enforce these contracts at runtime.** That job falls to **` sails-hook-orm`** , the Sails core hook that boots Waterline, inspects your adapters, and either unlocks or forbids features in your app’s userland code.


This article is a deep dive into those interfaces—what they are, how they’re tested, and how` sails-hook-orm` enforces them.


## Interfaces


An interface in Waterline is a formal promise. If an adapter says *“I support the semantic interface,”* that means it implements the required CRUD methods:


- ` create()`
- ` find()`
- ` update()`
- ` destroy()`


Without those methods, you can’t even call` .create()` on a model.


This isn’t guesswork. Each interface is backed by a **dedicated test suite in` waterline-adapter-tests`** , the official adapter validation library.


## The Six Core Interfaces


Inside Waterline’s codebase, you’ll find six main interface definitions:


- **semantic**
- **queryable**
- **associations**
- **migratable**
- **sql**
- **scalable**


Let’s walk through them one by one.


### 1. Semantic


The **semantic interface** is the foundation. It covers the core CRUD operations every adapter must implement.


- ` create()` – Insert a record.
- ` find()` – Query by criteria.
- ` update()` – Modify one or more records.
- ` destroy()` – Delete records.


In the test suite, you’ll see checks like:


- Creating with missing required fields should fail.
- Updating should return the new values.
- Destroying should actually remove the record.


If an adapter doesn’t pass **semantic** , it’s not a usable adapter.


### 2. Queryable


The **queryable interface** expands basic CRUD into richer query capabilities.


It requires methods like:


- ` compileStatement()` – Compile Waterline’s query syntax into database-native SQL or a Mongo query object.
- ` sendNativeQuery()` – Run a raw query against the database.
- ` parseNativeQueryError()` – Normalize DB-specific errors into predictable ones.


Tests check things like:


- Criteria queries (` where` ,` limit` ,` sort` ).
- Case sensitivity.
- Proper error handling when invalid queries are sent.


This interface is why you can do:


```text
await   User.  find  ({ age: {   '>'  :   21   } }).  limit  (  10  ).  sort  (  'createdAt DESC'  );
```


and trust it’ll work across adapters.


### 3. Associations


The **associations interface** introduces relationships between models.


It covers methods like:


- ` addToCollection()` – Add related records.
- ` removeFromCollection()` – Remove related records.
- ` replaceCollection()` – Replace an entire relation.


Tests check for:


- ` hasMany` ,` belongsTo` , and` manyToMany` behaviors.
- Cascade operations.
- Populating associations (` .populate()` ).


Example:


```text
await   Pet.  addToCollection  (petId,   'owners'  ).  members  ([ownerId]);
```


Without this interface, you don’t get` .populate()` or any relationship helpers.


### 4. Migratable


The **migratable interface** is all about schema definition and teardown.


It includes methods like:


- ` define()` – Create or update tables/collections.
- ` drop()` – Remove them.


Tests cover:


- Auto-migrations when models change.
- Rollbacks and redefinitions.


If your adapter doesn’t support` migratable` , you can’t use Sails’ schema migrations (` sails.config.models.migrate = 'alter'` ).


### 5. SQL


The **sql interface** is unique—it’s only relevant for SQL adapters.


It provides tools for:


- Escaping identifiers.
- Generating parameterized queries.
- Guarding against SQL injection.


Tests confirm safe query generation and proper handling of edge cases.


If you’re on Mongo, this interface doesn’t apply. But if you’re writing a PostgreSQL adapter, it’s essential.


### 6. Scalable


The **scalable interface** is less commonly discussed but important.


It targets adapters that need to support partitioning, sharding, or large-scale horizontal scaling. Its methods ensure that Waterline queries can be distributed safely.


This is forward-looking: not all adapters implement it, but it’s part of Waterline’s long-term design.


## Enter` sails-hook-orm` : The Enforcer


So far, we’ve talked about Waterline defining interfaces and` waterline-adapter-tests` verifying them. But when you actually lift a Sails app, it’s **` sails-hook-orm`** that takes center stage.


Here’s what it does:


- **Boots Waterline** inside Sails.
- **Inspects adapters** to see which interfaces they really implement.
- **Exposes methods in userland** —like` .transaction()` ,` .sendNativeQuery()` , or` .addToCollection()` — **only if the adapter passed the checks** .
- **Throws clear errors** if you try to call a method the adapter can’t support.


For example:


```text
await   sails.  getDatastore  ().  transaction  (  async   (  db  )   =>   {
await   User.  create  ({ name:   'Ada'   }).  usingConnection  (db);
});
```


If your adapter doesn’t support the **transactional contract** (like Mongo),` sails-hook-orm` will fail fast with:


> “Adapter` sails-mongo` does not support the` transactional` interface. Please use a different adapter or avoid` .transaction()` .”


That’s what keeps Sails predictable. You either get a safe, guaranteed feature—or a clear stop sign.


## The Test Infrastructure: Fixtures & Bootstraps


All this is made possible by **fixtures** (test model definitions) and **bootstraps** (setup/teardown scripts) inside` waterline-adapter-tests` .


They spin up databases, run interface tests, and tear them down again—ensuring that adapters behave consistently no matter the backend.


This explains why writing a new adapter is non-trivial: you must implement the right interfaces and pass the test suite.


## Why This Matters for You


Even if you’re not writing an adapter, understanding interfaces matters:


1. **Switching databases** → You’ll know what features you gain or lose.
2. **Debugging errors** → Many “why doesn’t this work?” questions boil down to *“this interface isn’t supported.”*
3. **Reading Sails source** → Suddenly the logic in` sails-hook-orm` makes sense.
4. **Adapter development** → You’ll know exactly what you’re signing up for.


## A Mental Model


Think of it like this:


- **Waterline** = the rulebook (defines what CRUD, queryable, associations mean).
- **Adapters** = the players (implement those rules for each DB).
- **` waterline-adapter-tests`** = the exam (check if they actually did it right).
- **` sails-hook-orm`** = the referee (enforces at runtime what you can/can’t call in your app).


No contract → no feature. No interface → no exposure in userland.


## Closing


Waterline’s interfaces aren’t just internals—they’re the foundation of why Sails apps feel reliable across databases. They make sure your app either works as promised or fails early with clarity.


In the next article, we’ll zoom into the **[Waterline features](https://blog.sailscasts.com/waterline-features)** .
