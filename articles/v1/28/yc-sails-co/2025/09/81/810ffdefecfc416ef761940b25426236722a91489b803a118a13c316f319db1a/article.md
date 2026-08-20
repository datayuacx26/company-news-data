---
schema_version: "1.0.0"
document_id: "810ffdefecfc416ef761940b25426236722a91489b803a118a13c316f319db1a"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/what-is-waterline/"
published_at: "2025-09-18T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:e6e4a3080f497a6995c0c9b06b4cfc254191ad6915d0bb8c6844104abbbe8560"
---

# What is Waterline?

If you’ve ever built an app that needs to talk to a database, you know the drill: pick your database, pick your ORM (or query builder), and wire everything up. That works fine—until the day you want to change databases or support multiple ones. Suddenly, you’re rewriting logic, re-learning dialects, and chasing edge cases.


**Waterline** was created to solve exactly that problem.


## A Database Agnostic ORM


At its heart, Waterline is an **Object Relational Mapping (ORM) library** . But it’s not tied to a single database. Instead, Waterline defines a consistent way to describe your data models and then delegates the actual storage and retrieval to **adapters** .


That means the same code you write for a` User` model can back onto:


- MySQL or PostgreSQL (relational)
- MongoDB (NoSQL)
- Redis (key-value)
- Even in-memory storage for testing


Your business logic doesn’t change—only the adapter does.


## Why Not Just Use SQL Everywhere?


Because real apps are messy. Some parts of your system might call for relational integrity (say, billing), while others thrive with flexible document storage (like user profiles or activity feeds). Sometimes you need blazing-fast caching with Redis, and sometimes you want to prototype with in-memory data before committing to a full database.


Waterline gives you **one vocabulary for all of these worlds** .


## Models as the Source of Truth


In Waterline, you define your data structures as **models** . A model isn’t just a table schema or a collection—it’s your application’s definition of what that data means.


For example:


```text
// api/models/User.js
module  .  exports   =   {
attributes: {
email: { type:   'string'  , required:   true  , unique:   true   },
password: { type:   'string'  , required:   true   },
lastLoginAt: { type:   'ref'  , columnType:   'datetime'   }
}
};
```


From that single definition, Waterline knows how to:


- Validate data before saving it.
- Translate queries into the dialect your adapter understands.
- Keep your app consistent whether you’re using Postgres locally or MongoDB in production.


## One API, Many Backends


The magic is in Waterline’s **standardized API** . Once you define a model, you can query it the same way no matter what database you’re speaking to:


```text
await   User.  create  ({ email:   ' [email protected]  '  , password:   's3cret'   });
const   activeUsers   =   await   User.  find  ({ lastLoginAt: {   '>'  : Date.  now  ()   -   86400000   }});
```


It doesn’t matter if the data is being stored in MySQL, Mongo, or Redis. Your code doesn’t change.


## Why Waterline Matters


Waterline lets you:


- **Stay flexible** — switch databases without rewriting all your queries.
- **Prototype faster** — start with in-memory data, then move to a real DB.
- **Standardize across teams** — everyone writes queries the same way, regardless of the backend.


In short: Waterline removes the friction between your application logic and the persistence layer.


👉🏾 In the[next article](https://blog.sailscasts.com/waterline-adapters) , we’ll discuss how Waterline makes its database agnosticity possible through adapters—the translators that let your models talk to different databases.
