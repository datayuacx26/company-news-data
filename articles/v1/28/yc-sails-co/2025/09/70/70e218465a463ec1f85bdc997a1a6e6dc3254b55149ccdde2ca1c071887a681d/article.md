---
schema_version: "1.0.0"
document_id: "70e218465a463ec1f85bdc997a1a6e6dc3254b55149ccdde2ca1c071887a681d"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-disk-transactions/"
published_at: "2025-09-20T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:bf18ce1f4b9b1f3fd9d3605bc452f908e8c1c50e82bfa63392a6efed577c280b"
---

# How I Implemented Transactions in sails-disk (Without Native DB Support)

When Waterline (Sails.js’ ORM) introduced transaction support, adapters had to implement four methods:


- ` leaseConnection`
- ` beginTransaction`
- ` commitTransaction`
- ` rollbackTransaction`


For production adapters like **sails-postgresql** or **sails-sqlite** , this was easy—they just delegated to the database’s native transaction engine. But for **sails-disk** , which is backed by **NeDB** (an embedded JavaScript database without ACID transactions), I had a much bigger problem:


How do you give developers proper transactions when the database itself doesn’t support them?


This post is the story of how I solved that problem with a **snapshot-based transaction system** —a solution that prioritizes correctness and simplicity, even if it means sacrificing performance.


## The Problem: No Transactions in NeDB


[sails-disk](https://github.com/balderdashy/sails-disk) is a lightweight adapter, mostly used in **development or testing** , and it runs on top of[NeDB](https://github.com/sailshq/nedb) . NeDB is great for quick prototyping—no setup, everything stored as JSON files—but it lacks **native transaction support** .


That meant developers could not rely on commit/rollback semantics when development and testing with sails-disk, which made the development experience inconsistent compared to when they switch to a real databases like PostgreSQL or SQLite with their respective[sails-postgresql](https://github.com/balderdashy/sails-postgresql) and[sails-sqlite](https://github.com/sailscastshq/sails-sqlite) Waterline adapters.


In fact I find I had to use` sails-postgresql` adapter in development and add the extra layer of complexity of starting up a PostgreSQL server whenever I want to work on codebases that utilizes transactions and that was too much pain.


I needed to bring some form of transaction guarantees **without rewriting NeDB** or overcomplicating sails-disk.


## The Solution: Snapshot-Based Rollback


Instead of trying to simulate full-blown[ACID](https://en.wikipedia.org/wiki/ACID) semantics, I chose a simple but effective approach:


- **When a transaction begins** → take a deep snapshot of all collections.
- **During the transaction** → apply operations directly to NeDB.
- **On commit** → just mark the transaction as complete.
- **On rollback** → wipe the database and restore from the snapshot.


This way, I could guarantee **correct rollback** without introducing logs, locks, or database internals.


Here’s the core of` beginTransaction` :


```text
beginTransaction  :   function   beginTransaction  (  datastoreName  ,   options  ,   cb  ) {
const   datastore   =   datastores[datastoreName];
if   (  !  datastore) {
return   cb  (  new   Error  (  'Unrecognized datastore: `'  +  datastoreName  +  '`'  ));
}


const   connection   =   options.connection;
if   (  !  connection   ||   !  connection.transactionContext) {
return   cb  (  new   Error  (  'Invalid connection provided to beginTransaction.'  ));
}


const   transactionContext   =   connection.transactionContext;
const   snapshotTasks   =   [];


_.  each  (datastore.dbs,   function  (  db  ,   tableName  ) {
snapshotTasks.  push  (  function  (  next  ) {
db.  find  ({}).  exec  (  function  (  err  ,   docs  ) {
if   (err)   return   next  (err);
transactionContext.snapshots[tableName]   =   _.  cloneDeep  (docs   ||   []);
return   next  ();
});
});
});


async.  parallel  (snapshotTasks,   function  (  err  ) {
if   (err)   return   cb  (err);
transactionContext.isActive   =   true  ;
return   cb  ();
});
}
```


The critical detail here is` _.cloneDeep()` . Without deep cloning, changes inside the transaction would leak into the snapshot and break rollback.


## Transaction Context


Each transaction keeps its own context object, attached to the connection:


```text
const   connection   =   {
identity:   'sails-disk-transaction'  ,
datastoreName: datastoreName,
transactionContext: {
id:   'txn_'   +   Date.  now  ()   +   '_'   +   Math.  random  ().  toString  (  36  ).  substr  (  2  ,   9  ),
snapshots: {},
isActive:   false
}
};
```


The` id` helps with debugging,` snapshots` hold rollback data, and` isActive` tracks the lifecycle.


## Lifecycle: Begin → Commit → Rollback


### Commit (Simple by Design)


Because writes go straight to NeDB, committing is trivial:


```text
commitTransaction  :   function   commitTransaction  (  datastoreName  ,   options  ,   cb  ) {
const   connection   =   options.connection;
connection.transactionContext.isActive   =   false  ;
return   cb  ();
}
```


No two-phase commit. No write-ahead log. Just mark inactive.


### Rollback (The Heavy Lifting)


Rollback means restoring snapshots:


```text
rollbackTransaction  :   function   rollbackTransaction  (  datastoreName  ,   options  ,   cb  ) {
const   datastore   =   datastores[datastoreName];
const   connection   =   options.connection;
const   transactionContext   =   connection.transactionContext;


if   (  !  transactionContext.isActive) {
return   cb  (  new   Error  (  'Transaction is not active and cannot be rolled back.'  ));
}


const   rollbackTasks   =   [];
_.  each  (transactionContext.snapshots,   function  (  snapshot  ,   tableName  ) {
const   db   =   datastore.dbs[tableName];
rollbackTasks.  push  (  function  (  next  ) {
db.  remove  ({}, { multi:   true   },   function  (  removeErr  ) {
if   (removeErr)   return   next  (removeErr);
if   (snapshot.  length   >   0  ) {
db.  insert  (snapshot, next);
}   else   {
return   next  ();
}
});
});
});


async.  parallel  (rollbackTasks,   function  (  err  ) {
transactionContext.isActive   =   false  ;
if   (err)   return   cb  (  new   Error  (  'Rollback failed: '   +   err.message));
return   cb  ();
});
}
```


The steps:


1. Remove all current records.
2. Restore snapshot data.
3. Reset state.


## Trade-offs and Limitations


Like any engineering solution, there are trade-offs:


-


**Memory Usage:** Snapshots duplicate all data in memory. With large datasets, this is expensive.


-


**Performance:**


- Begin: O(n) (snapshot everything).
- Commit: O(1).
- Rollback: O(n).


-


**Isolation:** This behaves like **READ UNCOMMITTED** . Other operations see changes before commit.


And that’s okay—because **sails-disk is for development and testing** , not production workloads.


## Error Handling


I added defensive checks everywhere:


- Validate connections.
- Prevent rollback on inactive transactions.
- Handle async errors gracefully.


Clear error messages matter a lot when debugging transactions.


## Testing the Implementation


I built tests that verified:


- Transactions correctly roll back data.
- Commits persist changes.
- Mixed operations (create, update, delete) behave correctly.


For example:


```text
// Rollback the transaction
Adapter.  rollbackTransaction  (datastoreName, { connection: connection },   function  (  err  ) {
assert.  strictEqual  (connection.transactionContext.isActive,   false  );
Adapter.  find  (datastoreName, { using:   'user'  , criteria: { where: {} }},   function  (  err  ,   records  ) {
assert.  equal  (records.  length  ,   1  );
assert.  equal  (records[  0  ].name,   'Existing User'  );
});
});
```


## Lessons Learned


1. **Simplicity beats premature optimization.** Snapshotting is memory-heavy, but it works reliably.
2. **Deep cloning is critical.** Shallow copies led to silent corruption.
3. **Error messages save hours.** Developers need actionable feedback.
4. **Testing edge cases is mandatory.** Transactions are stateful and easy to break.


## Final Thoughts


Implementing transactions in sails-disk was a fun challenge. Without native database support, I had to simulate transactions at the adapter level. The snapshot-based approach turned out to be the **right balance** : it’s simple, reliable, and developer-friendly—even if it’s not the most efficient.


For development and testing, this gives sails-disk feature parity with production adapters, making the dev experience smoother.


If you’re running sails-disk in production with huge datasets (not recommended 😅), this won’t scale. But if you’re building and testing locally, it gives you all the transaction semantics you’d expect—commit and rollback included.


Sometimes, the best engineering isn’t about matching the “perfect” production system, but about picking trade-offs that make life easier for developers. And for sails-disk, snapshots were exactly that.
