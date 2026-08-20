---
schema_version: "1.0.0"
document_id: "90b0957e9c71400e8e5df9278afe555e800fb495bef2359e581b5d6a86aee056"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/understanding-parley/"
published_at: "2026-01-07T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:54:56.230774+00:00"
content_hash: "sha256:67615e40d2d40f63b88cd48d1230fa5dac893577cb347b546f29e845f81fea0f"
---

# Understanding Parley: The Deferred Pattern Powering Sails

If you’ve ever used Waterline models or[Sails helpers](https://blog.sailscasts.com/understanding-sails-helpers) , you’ve already been using[parley](https://github.com/mikermcneil/parley) without knowing it.


Every time you write` await User.find()` or` await sails.helpers.mail.send()` , parley is working behind the scenes to give you a consistent, powerful async experience.


## What is Parley?


Parley is a practical, lightweight flow control library for Node.js that predates native Promises. It implements a **Deferred object pattern** : a way to represent an operation that hasn’t executed yet but can be triggered in multiple ways.


Under the hood, parley uses[Bluebird](http://bluebirdjs.com/) for its Promise implementation. When you call` .then()` ,` .catch()` , or` .toPromise()` on a Deferred, parley uses Bluebird’s` promisify` to convert the callback-based execution into a Promise. This gives you access to Bluebird’s performance optimizations and advanced features like filtered` .catch()` handlers, while keeping the Deferred pattern’s lazy execution semantics.


What makes parley special is that it supports **three async paradigms simultaneously** :


```text
// Modern async/await (recommended)
const   users   =   await   User.  find  ({ active:   true   });


// Node-style callbacks
User.  find  ({ active:   true   }).  exec  ((  err  ,   users  )   =>   {
if   (err) {   return   this  .res.  serverError  (err); }
// ...
});


// Promise chaining
User.  find  ({ active:   true   })
.  then  (  users   =>   {   /* ... */   })
.  catch  (  err   =>   {   /* ... */   });
```


The same function call works with all three styles. No wrapper needed, no separate APIs to learn.


## The Deferred Object


At the heart of parley is the **Deferred object** , and understanding it unlocks how async operations work throughout Sails.


The core idea is **lazy execution** : when you call something like` User.find()` , nothing happens immediately. Instead of hitting the database right away, Waterline hands you back a Deferred, an object that holds your query logic and waits for you to say “go.” This is fundamentally different from Promises, which start executing the moment you create them.


### What is a Deferred?


A Deferred is an object that represents a **future computation** . It’s work that *will* happen, but hasn’t happened *yet* . Think of it like a movie ticket: you have the ticket in hand, but you haven’t watched the movie. The ticket gives you the *ability* to watch the movie whenever you’re ready.


When you call` User.find()` , Waterline doesn’t immediately hit the database. Instead, it returns a Deferred object, essentially a “ticket” that says: “I know how to query the database. Just tell me when.”


```text
// This does NOT query the database yet
const   deferred   =   User.  find  ({ active:   true   });


// The deferred is just sitting there, waiting
console.  log  (deferred);   // [Deferred]


// NOW it queries the database
const   users   =   await   deferred;
```


### Why Lazy Execution?


This lazy execution pattern is powerful for several reasons:


**1. Method Chaining Before Execution**


Because the query hasn’t run yet, you can keep adding to it:


```text
const   deferred   =   User.  find  ();


// Build up the query piece by piece
deferred.  where  ({ status:   'active'   });
deferred.  sort  (  'createdAt DESC'  );
deferred.  limit  (  10  );


// Execute when ready
const   users   =   await   deferred;
```


Or more commonly, chained fluently:


```text
const   users   =   await   User.  find  ()
.  where  ({ status:   'active'   })
.  sort  (  'createdAt DESC'  )
.  limit  (  10  );
```


**2. Conditional Execution**


You can decide whether to execute based on runtime conditions:


```text
const   query   =   User.  find  ({ role:   'admin'   });


if   (needsResults) {
const   admins   =   await   query;
// do something with admins
}
// If needsResults is false, no database query ever happens
```


**3. Passing Around Unexecuted Queries**


You can pass a Deferred to another function that decides when (or if) to execute it:


```text
function   maybeExecute  (  deferred  ,   shouldRun  ) {
if   (shouldRun) {
return   deferred;   // Caller will await this
}
return   null  ;
}


const   result   =   await   maybeExecute  (User.  find  (),   true  );
```


### Deferred vs Promise


You might wonder: “Isn’t this just a Promise?” Not quite. Here’s the key difference:


**Promises execute immediately:**


```text
// This starts fetching RIGHT NOW, whether you want it to or not
const   promise   =   fetch  (  'https://api.example.com/users'  );
```


**Deferreds wait for you:**


```text
// This does nothing until you call .exec(), await, or .then()
const   deferred   =   User.  find  ();
```


Parley’s Deferred becomes a Promise when you need it to (via` .then()` ,` .catch()` , or` await` ), but it doesn’t force immediate execution. This gives you control over **when** the work happens.


### The Deferred Lifecycle


Here’s what happens inside a Deferred:


1. **Creation** :` User.find()` creates a Deferred with your query logic stored inside
2. **Configuration** : Chainable methods (` .where()` ,` .timeout()` , etc.) modify the Deferred’s state
3. **Execution** : When you call` .exec()` ,` await` , or` .then()` , the stored logic finally runs
4. **Resolution** : The callback/promise resolves with the result or rejects with an error


```text
// 1. Creation - stores the "find" logic
const   deferred   =   User.  find  ({ active:   true   });


// 2. Configuration - adds timeout behavior
deferred.  timeout  (  5000  );


// 3. Execution - NOW the database is queried
// 4. Resolution - users contains the result
const   users   =   await   deferred;
```


### Anatomy of a Deferred


Under the hood, a Deferred is a carefully designed object that tracks its own state through the execution lifecycle. Understanding its internal properties helps demystify what parley is doing behind the scenes.


**Core Internal Properties:**


Property Purpose


` _handleExec` The function containing the actual logic to execute (your query, helper, etc.)


` _hasBegunExecuting` Spinlock flag preventing double-execution


` _hasFinishedExecuting` Tracks whether the operation has completed


` _hasTimedOut` Set to` true` if a timeout occurred


` _timeout` The configured timeout in milliseconds (if any)


` _omen` An Error snapshot for better stack traces


` _promise` Cached promise (created lazily when` .then()` or` .toPromise()` is called)


` _userlandAfterExecLCs` Array of` .intercept()` and` .tolerate()` handlers


**The Spinlock Pattern:**


One of the most important safety features is the spinlock, which prevents a Deferred from executing more than once:


```text
// Inside .exec() - simplified
if   (  this  ._hasBegunExecuting) {
console.  warn  (  'This Deferred has already begun executing!'  );
return  ;   // Silently ignore duplicate execution
}
this  ._hasBegunExecuting   =   true  ;
```


This protects against race conditions when the same Deferred might accidentally be awaited twice.


**Timeout Implementation:**


When you call` .timeout(5000)` , parley sets up a` setTimeout` that races against your operation:


```text
// Simplified timeout flow
const   timeoutAlarm   =   setTimeout  (()   =>   {
this  ._hasTimedOut   =   true  ;
callback  (  new   TimeoutError  (  'Took too long...'  ));
},   this  ._timeout);


// When operation completes normally, clear the alarm
clearTimeout  (timeoutAlarm);
```


**Error Negotiation Pipeline:**


When an error occurs, it flows through a pipeline of handlers:


1. First, parley checks for any` .intercept()` or` .tolerate()` handlers that match the error
2. If a match is found, the handler runs (potentially transforming or swallowing the error)
3. Finally, if there’s a` _finalAfterExecLC` (used by Machine for post-processing), it runs
4. The final error (or result) is passed to your callback


This layered approach is what makes error negotiation so powerful. Each layer can inspect, modify, or handle errors before they reach your code.


**Why This Matters:**


Understanding the Deferred’s anatomy helps you:


- **Debug timing issues** : If you see “already begun executing” warnings, you know a Deferred is being triggered twice
- **Understand timeout behavior** : Timeouts are set up at execution time, not creation time
- **Appreciate error flow** : Your` .intercept()` handlers run before Machine’s post-processing, giving you first crack at errors
- **Write better tests** : You can check` deferred._hasBegunExecuting` in tests to verify execution state


## The Sails Ecosystem Connection


Parley is the async backbone of the Sails ecosystem, but its methods are extended and augmented as they flow through different layers:


- **Parley** provides the core Deferred pattern and base methods
- **Waterline ORM** uses parley for model methods and adds query-specific chainables like` .where()` ,` .sort()` ,` .usingConnection()`
- **Machine** (the engine behind Sails helpers) uses parley and adds helper-specific methods like` .retry()` ,` .switch()` , and` .meta()`


This layered architecture means you get a consistent base experience (thanks to parley), plus specialized methods depending on what you’re working with.


## Parley’s Core Methods


These methods come directly from parley and work everywhere in Sails, on both Waterline queries and helpers.


### Execution Methods


**` .exec(callback)`** executes with a Node-style callback:


```text
User.  find  ({ active:   true   }).  exec  ((  err  ,   users  )   =>   {
if   (err) {   return   res.  serverError  (err); }
return   res.  json  (users);
});
```


**` .then()` and` .catch()`** provide Promise-style execution:


```text
User.  find  ({ active:   true   })
.  then  (  users   =>   res.  json  (users))
.  catch  (  err   =>   res.  serverError  (err));
```


**` .toPromise()`** converts to a Bluebird Promise for use with` Promise.all()` :


```text
const   [  users  ,   posts  ,   comments  ]   =   await   Promise  .  all  ([
User.  find  ().  toPromise  (),
Post.  find  ().  toPromise  (),
Comment.  find  ().  toPromise  ()
]);
```


### Flow Control Methods


**` .timeout(milliseconds)`** sets a maximum wait time:


```text
// Give up after 5 seconds
const   user   =   await   User.  findOne  ({ id:   123   })
.  timeout  (  5000  );
```


If exceeded, parley throws a` TimeoutError` . This is invaluable for preventing operations from hanging indefinitely.


### Error Negotiation Methods


**` .tolerate(rule, \[handler\])`** gracefully swallows specific errors:


```text
// Return undefined if not found, instead of throwing
const   user   =   await   User.  findOne  ({ email:   ' [email protected]  '   })
.  tolerate  (  'notFound'  );


// Or provide a fallback value
const   config   =   await   sails.helpers.fs.  readJson  (  './config.json'  )
.  tolerate  (  'notFound'  , ()   =>   {
return   { defaults:   true   };
});
```


The` rule` can be:


- A **string** matching an error code:` .tolerate('E_NOT_FOUND')`
- An **array** of error codes:` .tolerate(\['E_NOT_FOUND', 'E_TIMEOUT'\])`
- A **dictionary** filter:` .tolerate({ code: 'E_INVALID', status: 400 })`
- A **function** for complex matching:` .tolerate(err => err.code.startsWith('E_'))`


**` .intercept(rule, handler)`** catches and rethrows errors with modifications:


```text
const   user   =   await   User.  create  ({ email:   ' [email protected]  '   })
.  intercept  (  'E_UNIQUE'  , (  err  )   =>   {
return   new   Error  (  'That email address is already in use.'  );
});
```


Unlike` .tolerate()` , which swallows errors,` .intercept()` transforms them before rethrowing. Perfect for converting low-level database errors into user-friendly messages.


### Debugging Methods


**` .log()`** executes and logs the result (useful in the REPL):


```text
User.  find  ({ id:   1   }).  log  ();
// Logs the full result with util.inspect
```


**` .now()`** executes synchronously (only for sync operations):


```text
const   result   =   sails.helpers.strings.  random  ().  now  ();
```


## Machine’s Augmentations


When you use Sails helpers, you’re using the[Machine package](https://github.com/node-machine/machine) , which wraps parley and adds several powerful methods. But helpers aren’t the only thing built with Machine.


The Machine package powers any callable that follows the node-machine specification: Sails helpers, payment integrations like[Sails Pay](https://docs.sailscasts.com/sails-pay) methods, organics from` sails-hook-organics` , and any machinepack you might use.


This means that anything built with Machine automatically gets access to parley’s core methods plus these additional augmentations. Waterline queries, however, don’t go through Machine, so they only have access to parley’s core methods.


### ` .retry(rule, \[delaySeries\])`


Built-in exponential backoff for transient failures:


```text
// Retry on timeout errors with default delays (250ms, 500ms, 1000ms)
const   result   =   await   sails.helpers.http.  get  (  'https://api.example.com/data'  )
.  timeout  (  5000  )
.  retry  (  'TimeoutError'  );


// Custom delay series: retry after 100ms, then 500ms, then 2000ms
const   user   =   await   sails.helpers.users.  fetch  (userId)
.  retry  (  'E_SERVICE_UNAVAILABLE'  , [  100  ,   500  ,   2000  ]);
```


The` rule` parameter works like` .tolerate()` and` .intercept()` . The optional` delaySeries` defaults to` \[250, 500, 1000\]` milliseconds.


### ` .switch(handlers)`


Handle multiple exits explicitly with a “switchback” pattern:


```text
await   sails.helpers.users.  create  ({ email:   ' [email protected]  '   })
.  switch  ({
success  : (  newUser  )   =>   {
sails.log.  info  (  'Created user:'  , newUser.id);
return   res.  json  (newUser);
},
emailAlreadyInUse  : ()   =>   {
return   res.  badRequest  (  'That email is taken.'  );
},
error  : (  err  )   =>   {
return   res.  serverError  (err);
}
});
```


This is particularly useful when a helper has multiple named exits beyond just` success` and` error` . Each exit gets its own handler, making the control flow explicit.


### ` .meta(dictionary)`


Pass metadata to the helper’s implementation via` this` :


```text
const   result   =   await   sails.helpers.email.  send  ({
to:   ' [email protected]  '  ,
subject:   'Welcome!'
})
.  meta  ({
skipActuallySending:   true  ,    // Available as this.skipActuallySending in the helper
customSmtpHost:   'smtp.custom.com'
});
```


Inside the helper, you access metadata through` this` :


```text
// In api/helpers/email/send.js
fn  :   async   function  (  inputs  ) {
if   (  this  .skipActuallySending) {
sails.log.  info  (  'Skipping email send (meta flag set)'  );
return  ;
}
// Actually send the email...
}
```


## Waterline’s Additions (Models Only)


Waterline adds its own chainable methods on top of parley for query building. These only work on model methods.


### ` .usingConnection(db)`


Use a specific database connection (useful for transactions):


```text
await   sails.  getDatastore  ().  transaction  (  async   (  db  )   =>   {
await   User.  create  ({ name:   'Test'   }).  usingConnection  (db);
await   Profile.  create  ({ userId: newUser.id }).  usingConnection  (db);
// If anything fails, both operations roll back
});
```


### Query Building Methods


Methods like` .where()` ,` .sort()` ,` .limit()` ,` .skip()` ,` .select()` ,` .populate()` , and` .fetch()` are Waterline-specific:


```text
const   users   =   await   User.  find  ()
.  where  ({ status:   'active'   })
.  sort  (  'createdAt DESC'  )
.  limit  (  10  )
.  populate  (  'posts'  );
```


These are implemented as custom chainable methods on the parley Deferred.


## Methods Summary


Here’s a quick reference of what’s available where:


Method Parley (Core) Waterline Machine


` .exec()` ✓ ✓ ✓


` .then()/.catch()` ✓ ✓ ✓


` .toPromise()` ✓ ✓ ✓


` .timeout()` ✓ ✓ ✓


` .tolerate()` ✓ ✓ ✓


` .intercept()` ✓ ✓ ✓


` .log()` ✓ ✓ ✓


` .now()` ✓ ✓ ✓


` .retry()` ✓


` .switch()` ✓


` .meta()` ✓ ✓


` .usingConnection()` ✓


` .where()/.sort()/etc` ✓


Machine-powered callables include: Sails helpers,` sails.pay` methods,` sails-hook-organics` , and any machinepack.


## Error Negotiation in Practice


One of parley’s most practical features is error negotiation. In Sails, errors often have a` code` property that identifies the type of failure:


```text
// In your helper
fn  :   async   function  (  inputs  ) {
const   user   =   await   User.  findOne  ({ id: inputs.userId });
if   (  !  user) {
throw   { code:   'E_USER_NOT_FOUND'  , message:   'No user with that ID exists'   };
}
return   user;
}
```


Then in your action, you can handle errors declaratively:


```text
// Handle specific errors differently
const   user   =   await   sails.helpers.users.  lookup  (userId)
.  intercept  (  'E_USER_NOT_FOUND'  , ()   =>   'notFound'  )
.  intercept  (  'E_UNAUTHORIZED'  , ()   =>   'forbidden'  );
```


Or use` .switch()` for helpers with multiple exits:


```text
await   sails.helpers.users.  lookup  (userId)
.  switch  ({
success  : (  user  )   =>   res.  json  (user),
notFound  : ()   =>   res.  notFound  (),
unauthorized  : ()   =>   res.  forbidden  (),
error  : (  err  )   =>   res.  serverError  (err)
});
```


## The 15-Second Warning


In development, parley watches for Deferreds that are created but never executed. If you forget an` await` :


```text
// Oops! Missing await
User.  find  ({ active:   true   });    // This does nothing!
```


After 15 seconds, parley logs a warning:


```text
WARNING: A function that was initially called over 15 seconds
ago has still not actually been executed. Any chance the
source code is missing an "await"?
```


This catches a common mistake that would otherwise be silently ignored.


## Performance


Parley is designed for performance-critical code paths:


- **28M ops/sec** for building a Deferred
- **3.2M ops/sec** for build + execute
- Prototypal implementation (not closures) for hot code paths
- Minimal validation in production mode


For the ultimate performance, you can bypass the Deferred entirely by passing a callback directly:


```text
// Slightly faster because parley doesn't create a Deferred
User.  find  ({ active:   true   }, (  err  ,   users  )   =>   {
// Direct callback
});
```


## Building Your Own Parley-Powered Functions


If you’re building a library or want consistent async behavior, you can use parley directly:


```text
const   parley   =   require  (  'parley'  );


function   fetchUser  (  id  ) {
return   parley  ((  done  )   =>   {
// Your async logic here
db.  query  (  'SELECT * FROM users WHERE id = ?'  , [id], (  err  ,   rows  )   =>   {
if   (err) {   return   done  (err); }
if   (  !  rows.  length  ) {
return   done  ({ code:   'E_NOT_FOUND'  , message:   'User not found'   });
}
return   done  (  undefined  , rows[  0  ]);
});
});
}


// Now consumers can use any style:
const   user   =   await   fetchUser  (  123  );
// or
fetchUser  (  123  ).  exec  ((  err  ,   user  )   =>   {   /* ... */   });
```


You can even add custom chainable methods:


```text
function   buildQuery  () {
let   criteria   =   {};


return   parley  ((  done  )   =>   {
db.  find  (criteria, done);
},   undefined  , {
where  :   function  (  clause  ) {
criteria.where   =   clause;
return   this  ;
},
limit  :   function  (  n  ) {
criteria.limit   =   n;
return   this  ;
},
sort  :   function  (  field  ) {
criteria.sort   =   field;
return   this  ;
}
});
}


// Use it like Waterline:
const   results   =   await   buildQuery  ()
.  where  ({ status:   'active'   })
.  limit  (  10  )
.  sort  (  'createdAt DESC'  );
```


## Why “Parley”?


The name comes from the nautical term for a conference between opposing parties. It’s fitting for a library that negotiates between different async paradigms, and it’s also a nod to Sails’ nautical theme.


## Conclusion


Parley is one of those foundational packages that makes the Sails developer experience smooth without demanding attention. The Deferred pattern it implements gives you lazy execution, method chaining, and flexibility in how you handle async code, whether you prefer` await` , callbacks, or promises.


The next time you chain a` .timeout(5000)` onto a slow database query or use` .tolerate()` to handle a missing file gracefully, you’ll know exactly what’s making that possible, and why that` User.find()` doesn’t hit the database until you tell it to.
