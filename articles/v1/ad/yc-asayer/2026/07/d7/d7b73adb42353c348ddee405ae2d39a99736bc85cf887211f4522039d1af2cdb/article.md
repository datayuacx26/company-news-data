---
schema_version: "1.0.0"
document_id: "d7b73adb42353c348ddee405ae2d39a99736bc85cf887211f4522039d1af2cdb"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/javascript-cleanup/"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-30T14:41:35.298132+00:00"
fetched_at: "2026-07-30T14:41:37.626549+00:00"
content_hash: "sha256:e6ee6bb48cf3043900d53400dae0a7689c615895c518a74e30af60218828d2d6"
---

# How JavaScript Finally Learned to Clean Up After Itself

JavaScript’s new Explicit Resource Management feature — the` using` and` await using` declarations backed by` Symbol.dispose` and` Symbol.asyncDispose` — gives the language deterministic, scope-based cleanup of non-memory resources like file handles, sockets, locks, and database connections. This is not garbage collection. Garbage collection reclaims *memory* on its own non-deterministic schedule, and it never closed your files, released your locks, or unsubscribed your event listeners. Those are exactly the resources` using` cleans up, predictably, the moment a scope exits.


If you’ve been hand-writing` try/finally` blocks to close connections and release locks, this feature replaces that boilerplate with a single keyword. This article explains what` using` does, how the sync/async split works, how to write your own disposables, how to coordinate several with` DisposableStack` , how to retrofit libraries that don’t support it yet, and how disposal errors surface through the new` SuppressedError` . (` WeakRef` and` FinalizationRegistry` are the GC-adjacent tools for memory; this is a different mechanism entirely.)


## Key Takeaways


- A` using` declaration calls an object’s` \[Symbol.dispose\]()` method when the enclosing block exits;` await using` calls` \[Symbol.asyncDispose\]()` and awaits it, so promise-returning teardown completes before the scope closes.
- When several resources share a scope, they are disposed in **reverse** declaration order, so a dependent resource tears down before the dependency it relies on.
- Explicit Resource Management is a finished TC39 proposal standardized in ES2026, shipping natively in Chrome 134 (V8 13.8), Firefox 141, and Node.js 24 (V8 13.6); Safari is still catching up — the` Symbol.dispose` symbol reached desktop Safari 26.4, but not yet Safari on iOS.
- ` DisposableStack.prototype.move()` transfers registered resources into a fresh stack and marks the original disposed without disposing anything — the pattern for handing half-built resources safely out of a constructor.
- If disposal throws while your code is already throwing, JavaScript wraps both in a` SuppressedError` , so neither the original failure nor the cleanup failure is lost.


## The cleanup problem` try/finally` never solved well


Every resource you open — a file handle, a stream reader, a database connection, a lock — has to be closed, and the only language-level tool for guaranteeing that was` try/finally` . It works, but it’s verbose and it scales badly. Consider a[Web Streams reader](https://v8.dev/features/explicit-resource-management) : if an error occurs during the reading process, and you forget to call` releaseLock()` before the error propagates, the stream remains locked. The fix is to wrap the read loop in a` try` block and put` reader.releaseLock()` in` finally` so the lock is always released.


That pattern is fine for one resource. With two or three interdependent resources it becomes nested` finally` blocks where ordering matters and a throw inside one cleanup can skip the others. The mechanical part — “this thing needs teardown, run it on the way out, in the right order, even on the error path” — is exactly what the language now handles for you.


## How the` using` keyword works


The` using` keyword declares a block-scoped, non-reassignable binding (like` const` ) whose value must be` null` ,` undefined` , or an object carrying a` \[Symbol.dispose\]()` method; when the variable goes out of scope, that method runs automatically. Use` using` for synchronous cleanup. Use` await using` when teardown returns a promise — it declares block-scoped variables that are asynchronously disposed, the value must have a` \[Symbol.asyncDispose\]()` or` \[Symbol.dispose\]()` method, and that method is called and awaited when the variable goes out of scope. Because` await using` also accepts plain sync disposables, you can reach for it whenever you don’t know which kind you have.


```text
import   fs   from   "  node:fs/promises  "  ;


async   function   readConfig  () {
await using   file   =   await   fs  .  open  (  "  config.json  "  ,   "  r  "  );
const   {   buffer   }   =   await   file  .  read  ();
return   buffer  .  toString  ();
// file[Symbol.asyncDispose]() is called and awaited here, on every exit path
}
```


Node.js` FileHandle` objects already implement the async disposable protocol, so this works without a manual wrapper. Note the two` await` operations:` await fs.open()` awaits acquisition and unwraps the promise into a` FileHandle` , while` await using` awaits disposal when the variable goes out of scope.


The ordering rule matters most when resources depend on each other. If a scope contains multiple` using` or` await using` declarations, all disposers run in sequence in the reverse order of declaration, regardless of declaration type, and all are guaranteed to run much like a` finally` block.


```text
await using   db   =   await   openConnection  ();     // disposed second
await using   tx   =   await   db  .  beginTransaction  ();   // disposed first
// tx tears down before db, so the connection is still alive when the
// transaction is finalized
```


On scoping:` using` is valid inside any block, function body, static initialization block,` for` header, and at the top level of a *module* — but not at the top level of a *script* , because script scopes persist and the disposer would never fire.` await using` at the top level is module-only, since it depends on top-level` await` . The spec-aligned rules are documented on the[MDN using reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/using) .


## Writing your own disposable


Any object becomes disposable by implementing` \[Symbol.dispose\]()` for synchronous cleanup or` \[Symbol.asyncDispose\]()` for asynchronous cleanup — there is no base class or registration step. The well-known symbol *is* the contract:[an object is disposable if it has a \[Symbol.dispose\]() method](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/dispose) , and the` using` declaration looks up that symbol on the initializer for the method to call when the variable goes out of scope.


Here’s a database connection wrapper carried through the rest of this article:


```text
class   DatabaseConnection   {
#client;
constructor  (  client  ) {
this  .  #client   =   client  ;
}
query  (  sql  ,   params  ) {
return   this  .  #client  .  query  (  sql  ,   params  );
}
async   [  Symbol  .  asyncDispose  ]() {
await   this  .  #client  .  end  ();
}
}


async   function   getUser  (  pool  ,   id  ) {
await using   db   =   new   DatabaseConnection  (  await   pool  .  acquire  ());
return   await   db  .  query  (  "  SELECT * FROM users WHERE id = $1  "  , [  id  ]);
// db[Symbol.asyncDispose]() runs here — the client is released on every path
}
```


A synchronous disposer follows the same shape with` \[Symbol.dispose\]()` instead. A sync disposer should not return a promise, because promises returned by` \[Symbol.dispose\]()` are not awaited by` await using` ; to declare async disposables, use` Symbol.asyncDispose` .


## Coordinating resources with DisposableStack


When a single object owns several resources — or when you’re acquiring resources in a loop —` DisposableStack` and` AsyncDisposableStack` collect them and dispose the whole group at once, in reverse order. Both structures provide[methods like use() , adopt() , and defer()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DisposableStack) to add resources or disposal actions, and a` dispose()` or` asyncDispose()` method to trigger the cleanup. They themselves carry` \[Symbol.dispose\]()` /` \[Symbol.asyncDispose\]()` , so they work with` using` and` await using` .


The three registration methods cover different shapes:


Method Registers Use when


` use(resource)` A resource that already has a dispose symbol The object is itself disposable


` adopt(value, onDispose)` A non-disposable value plus a cleanup callback The resource has a` close` /` abort` -style method but no symbol


` defer(onDispose)` A standalone cleanup callback, no resource You need to run arbitrary teardown (e.g.` clearInterval` )


```text
function   startWorker  () {
using   stack   =   new   DisposableStack  ();
const   handle   =   setInterval  (  poll  ,   5000  );
stack  .  defer  (()   =>   clearInterval  (  handle  ));
stack  .  adopt  (  openSocket  (), (  s  )   =>   s  .  close  ());
// both cleanups run, in reverse, when the block exits
}
```


The non-obvious power is` move()` , which solves a real construction-safety problem. Sometimes function scope isn’t enough — a class or object owns several resources that should be grouped like` using` , but as a class field or closure, which is what` DisposableStack` and` AsyncDisposableStack` are for.` DisposableStack.prototype.move()` transfers every registered resource into a fresh stack and marks the original disposed *without* disposing the resources, which lets you build up resources locally, and hand them out only if construction fully succeeds:


```text
function   openResources  () {
using   cleanup   =   new   DisposableStack  ();
const   a   =   cleanup  .  use  (  openA  ());
const   b   =   cleanup  .  use  (  openB  ());   // if this throws, `a` is disposed automatically
const   moved   =   cleanup  .  move  ();     // success: ownership transfers out, nothing disposed
return   moved  ;                     // caller now owns disposal of both
}
```


If` openB()` throws, the block exits and` cleanup` disposes` a` — no leak. If everything succeeds,` move()` hands the resources to the caller intact. The` move()` semantics are documented in V8’s[Explicit Resource Management writeup](https://v8.dev/features/explicit-resource-management) .


## Retrofitting libraries that don’t support disposal yet


You don’t need a library to adopt` Symbol.dispose` to use it with` using` — attach the symbol yourself with` Object.assign` . A library client that exposes a` close()` or` end()` method but no dispose symbol becomes disposable in one line. Assigning a` Symbol.asyncDispose` method onto the client means you can use it in` await using` declarations and with` AsyncDisposableStack#use()` , and if you later upgrade to a version that implements the protocol itself, you’ll get an error reminding you to delete the shim.


```text
const   client   =   await   MongoClient  .  connect  (  url  );
Object  .  assign  (  client  , {
async   [  Symbol  .  asyncDispose  ]()   {
await   client  .  close  ();
},
});


async   function   run  () {
await using   db   =   client  ;   // now disposes cleanly
// ...
}
```


The` Object.assign` shim is the bridge for the current ecosystem: most third-party clients haven’t shipped dispose symbols yet, and it lets you opt them in today.


## Where this helps on the frontend and in Node


On the client, the resources that leak are event listeners,` IntersectionObserver` /` ResizeObserver` instances,` navigator.locks` , locked Web Streams, and IndexedDB transactions — all things that have an explicit release step that’s easy to skip on an error path. Wrapping them in a disposable ties their lifetime to a scope. The V8 team’s canonical example is a stream reader: a` using` declaration over an object whose` \[Symbol.dispose\]()` calls` reader.releaseLock()` removes the need to remember the release at all.


Orphaned observers and unreleased locks are precisely the leaks that hide from a quick local test. An orphaned observer or an unreleased lock costs nothing on a page you reload every few seconds, but across a long single-page-app session they accumulate into gradual memory growth and interaction jank — the slow-burn degradation that a full session replay surfaces when a one-off reload never reproduces it. Scoping those resources with` using` is the structural fix for that class of bug.


On the server, Node.js` FileHandle` objects from` fs/promises` and many connection types already participate, and custom wrappers like the` DatabaseConnection` above cover the rest.


## Handling errors during disposal with SuppressedError


Disposal can fail, and it can fail *while your code is already throwing* — without a defined behavior, one error would silently clobber the other. JavaScript solves this with` SuppressedError` . All errors thrown during disposal, including the initial error that caused the scope exit, are aggregated inside one` SuppressedError` — each earlier exception as the` suppressed` property and the later exception as the` error` property — and it is thrown after disposal completes.


So if your block throws and then a disposer throws too, you catch a single` SuppressedError` whose` .error` is the disposal failure and whose` .suppressed` is the original. When multiple disposers throw, they nest, so every failure is reachable:


```text
try   {
using   a   =   makeDisposableThatThrowsOnDispose  (  "  a  "  );
using   b   =   makeDisposableThatThrowsOnDispose  (  "  b  "  );
throw   new   Error  (  "  body failed  "  );
}   catch   (  e  ) {
// e is a SuppressedError; b disposes first and a disposes last, so
// e.error is a's disposal error, and e.suppressed nests the rest
// (b's disposal error, then the original "body failed")
}
```


` SuppressedError` is the part that makes` using` safe to rely on in error-heavy code.


## Current support: shipped, not “coming soon”


Explicit Resource Management is a finished TC39 proposal standardized as part of ES2026 — not a Stage 3 experiment requiring transpilation everywhere. As of mid-2026 the picture is:


Runtime Status Notes


Chrome / Edge / Opera Shipped` using` /` await using` since Chromium 134 (V8 13.8); the` Symbol.dispose` symbol alone has been present since Chrome 125


Node.js Shipped Native` using` /` await using` in[Node.js 24](https://nodejs.org/en/blog/release/v24.0.0) (V8 13.6); symbols since 18.18.0


Firefox Shipped Since Firefox 141 (current stable 152)


Safari Partial The[Symbol.dispose symbol](https://caniuse.com/mdn-javascript_builtins_symbol_dispose) reached desktop Safari 26.4; Safari on iOS does not support it yet


TypeScript Shipped` using` syntax since 5.2; current stable 6.0


One trap is worth flagging: a defined` Symbol.dispose` symbol does not mean the engine parses` using` . The symbol routinely lands ahead of the declaration — Node exposed the well-known symbols back in the v18 line (18.18.0) but only made the declaration syntax native in Node 24, and Chrome shipped the symbol around v125 yet didn’t parse` using` until 134. So on an older engine you can reference` Symbol.dispose` and still get a syntax error or a “not disposable”` TypeError` — and a` Symbol.dispose` support table (like the one linked above) runs ahead of real` using` support. Full` using` support is an engine-version question, not a polyfill question.


For Safari and older targets, transpile. TypeScript requires setting your compilation target to ES2022 or below and configuring` lib` to include` "esnext"` or` "esnext.disposable"` (TypeScript supports the syntax from[version 5.2](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html) ), and you can polyfill the globals with` core-js` or the` disposablestack` package.


The mechanical, error-prone cleanup code you’ve been writing by hand now has a language primitive: add` \[Symbol.dispose\]` or` \[Symbol.asyncDispose\]` to anything that needs teardown, declare it with` using` or` await using` , and the runtime guarantees disposal in the right order on every exit path. Start by wrapping the one resource you most often forget to close — a connection, a lock, a reader — and let the scope do the rest.


## FAQs


What is the difference between using and await using?


A using declaration calls the object's synchronous Symbol.dispose method when the block exits, while await using calls Symbol.asyncDispose and awaits it, so promise-returning teardown completes before the scope closes. Reach for using when cleanup is synchronous and await using when teardown returns a promise. Because await using also accepts plain synchronous disposables, you can use it whenever you are unsure which kind of disposer an object carries.


Why does using throw a 'not disposable' error on Node 18 even though Symbol.dispose exists?


A defined Symbol.dispose symbol does not mean the engine can parse the using declaration. Node exposed the well-known Symbol.dispose and Symbol.asyncDispose symbols from version 18.18.0, but the full using and await using declaration syntax only became native in Node 24, which ships V8 13.6. On older Node you can reference the symbol yet still get a syntax error or a TypeError on built-in handles. Full using support is an engine-version question, not a polyfill question.


Does Explicit Resource Management replace garbage collection?


No. Garbage collection reclaims memory on its own non-deterministic schedule and never closed files, released locks, or unsubscribed event listeners. Explicit Resource Management handles those non-memory resources deterministically, running their cleanup the moment a scope exits. The two mechanisms address different problems. WeakRef and FinalizationRegistry are the GC-adjacent tools for memory, whereas using and await using give scope-based teardown of file handles, sockets, locks, and connections.


How do I use the using keyword with a library that has no dispose method?


Attach the symbol yourself with Object.assign. A client that exposes a close or end method but no dispose symbol becomes disposable in one line by assigning an async Symbol.asyncDispose method that calls the existing close method. After that you can declare the object with await using or register it with an AsyncDisposableStack use call. If you later upgrade to a library version that implements the protocol natively, the reassignment triggers an error reminding you to remove the shim.


Open-source session replay


## Complete picture for complete understanding


Capture every clue your frontend is leaving so you can instantly get to the root cause of any issue with **OpenReplay** — the open-source session replay tool for developers. Self-host it in minutes, and have complete control over your customer data.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
