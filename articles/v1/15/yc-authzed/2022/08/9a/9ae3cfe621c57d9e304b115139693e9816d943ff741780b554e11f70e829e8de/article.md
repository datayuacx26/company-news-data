---
schema_version: "1.0.0"
document_id: "9ae3cfe621c57d9e304b115139693e9816d943ff741780b554e11f70e829e8de"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/some-assembly-required"
published_at: "2022-08-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:0edaa4281a3d3e7ae6dc04125475f999e7ea618ee14096fa39fa74a551f1ea04"
---

# How we moved SpiceDB to run in-browser and decreased request latencies by 90%

In this post, we'll go into detail about each of these steps to highlight the work behind the newly improved[Authzed Playground](https://play.authzed.com/) experience and perhaps inspire others to experiment with WebAssembly.


*Interesting Note: Header image originally generated with DALL·E 2 with caption "purple database icon lifted by crane", slightly modified afterwards*


### TL;DR:


1. Compile SpiceDB Go source as WebAssembly.


1. Create a JavaScript interface to talk to SpiceDB from within a browser.


1. Cache aggressively.


1. Enjoy near instant responses from SpiceDB.


## Background


At Authzed, our goal is to empower authorization teams with the best tooling possible.


SpiceDB, our open source permissions database inspired by Google's Zanzibar, provides a lot of power in configuring and defining a permission system.


However, this flexibility creates a need for tooling around development, testing and validation.


In a[previous blog post](https://authzed.com/blog/learning-through-play/) I discussed how we built the[Authzed Playground](https://play.authzed.com/) to meet this need, with a section describing the server-side API provided by SpiceDB to run Playground operations against the **real** implementation of permissions resolution code: using the real SpiceDB code for the Playground was and continues to remain critical, ensuring that any operation which works in the Playground will work in a real API call (and vice versa).


While this approach has proven to be stable and relatively performant, it comes with a significant cost: the Playground relies on a shared server side component running with a distinct development API in order to be functional.


We wondered: was there a way to continue to use the real SpiceDB implementation in Playground, but without this limitation?


As SpiceDB is written in Go, Ideally, we'd run Go code directly in the user's browser, but that is not possible… or is it?


## Some Assembly Required


Many words have been written about[WebAssembly](https://webassembly.org/) (WASM) and its potential impacts, ranging from whether it will eventually replace JavaScript in the browser to its use for serverless functions.


One benefit often missed, however, is the ability to share non-JavaScript implementations between clients and servers: so long as the toolchain for your preferred language supports compiling into WASM, subsets of code can be shared with browser-based applications!


Thanks to some amazing work by the Go team starting a few years ago, Go's compiler now has support for compiling code to WebAssembly!


Creating a WASM bundle from a Go application is, on the surface, as simple as specifying the “operating system” as` JS` and the architecture as` wasm` :


shell


1


```text
GOOS=js GOARCH=wasm go build -o main.wasm


```


shell


1


```text
GOOS=js GOARCH=wasm go build -o main.wasm


```


We therefore had a viable plan: compile SpiceDB into a WebAssembly binary, load it in the browser, and make API calls directly to the now in-browser SpiceDB!


## An early roadblock


### Adjusting SpiceDB to compile under WebAssembly


Getting SpiceDB to compile for WebAssembly ran into a roadblock almost immediately: not all of SpiceDB's code is, itself, written in Go:


go


1


2


3


4


5


```text
package   github.com/authzed/spicedb/pkg/development/wasm


imports github.com/dgraph-io/ristretto
imports github.com/dgraph-io/ristretto/z
imports golang.org/x/sys/unix: build constraints exclude all Go files in / go  /pkg/mod/golang.org/x/sys@v0 .0  .0  -20220722155257  -8  c9f86f7a55f/unix


```


go


1


2


3


4


5


```text
package   github.com/authzed/spicedb/pkg/development/wasm


imports github.com/dgraph-io/ristretto
imports github.com/dgraph-io/ristretto/z


```


For performance reasons, SpiceDB includes a select number of native libraries that rely upon platform-specific compilation.


One of these libraries,[Ristretto](https://github.com/dgraph-io/ristretto) , is a very high performance cache used to cache results from various subproblems in SpiceDB.


Ristretto gets part of its performance by implementing platform-specific optimizations and, unfortunately, did not have a WASM implementation.


Thus, our first task was to extract out our caching code behind[an interface](https://github.com/authzed/spicedb/blob/main/pkg/cache/cache.go) , and then implement a version solely for use when compiling under WASM.


As the caching code in SpiceDB is only used for production servers, we could simply change our cache into a no-op:


go


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


```text
package   cache


import   (
"fmt"
)


// NewCache returns an error for caching.
// This is used because ristretto cannot be built under WASM.
func    NewCache  (config *Config)    (Cache,  error  ) {
return    nil  , fmt.Errorf( "caching is currently unsupported in WASM"  )
}


```


go


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


```text
package   cache


import   (
"fmt"
)


// NewCache returns an error for caching.
// This is used because ristretto cannot be built under WASM.
func    NewCache  (config *Config)    (Cache,  error  ) {
return    nil  , fmt.Errorf( "caching is currently unsupported in WASM"  )
}


```


By implementing the file as` cache_wasm.go` with the` _wasm.go` suffix, the Go compiler automatically only compiles the file for the` wasm` platform.


With the interface implemented, the next step was to now tell Go to ignore the Ristretto-based implementation when compiling for WASM: normally this could be accomplished with a similar platform-specific suffix on the source file, but as this implementation is intended for **all** platforms besides WASM, a different approach was necessary.


This was therefore accomplished by using the Go conditional compilation comments:


go


1


2


```text
//go:build !wasm
// +build !wasm


```


go


1


2


```text
//go:build !wasm
// +build !wasm


```


The above conditional compilation comments tell the Go compiler to ignore the source file if` wasm` is the specified platform.


## Loading SpiceDB in JavaScript


With SpiceDB now compiling into a WASM binary, the next step was to load the WASM code into the JavaScript runtime and determine a means of invoking the necessary functions for the development environment.


Here, the Go developers have helpfully provided a library to do just that:[wasm_exec.js](https://github.com/golang/go/blob/master/misc/wasm/wasm_exec.js) .


As seen in the[wasm_exec example](https://github.com/golang/go/blob/master/misc/wasm/wasm_exec.html) , the library is included and then a call to` instantiateStreaming` is used to load the compiled Go binary:


html


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
< script    src  = "wasm_exec.js"  >  </ script  >
< script  >
// Based on: https://github.com/golang/go/blob/master/misc/wasm/wasm_exec.html
if   ( WebAssembly  ) {
// WebAssembly.instantiateStreaming is not currently available in Safari
if   ( WebAssembly   && ! WebAssembly  . instantiateStreaming  ) {
// polyfill
WebAssembly  . instantiateStreaming   =  async   (resp, importObject) => {
const   source =  await   ( await   resp). arrayBuffer  ();
return    await    WebAssembly  . instantiate  (source, importObject);
};
}


const   go =  new    Go  ();
WebAssembly  . instantiateStreaming  ( fetch  ( "main.wasm"  ), go. importObject  ). then  (
( result  ) =>   {
go. run  (result. instance  );
}
);
}  else   {
console  . log  ( "WebAssembly is not supported in your browser"  );
}
</ script  >


```


html


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
< script    src  = "wasm_exec.js"  >  </ script  >
< script  >
// Based on: https://github.com/golang/go/blob/master/misc/wasm/wasm_exec.html
if   ( WebAssembly  ) {
// WebAssembly.instantiateStreaming is not currently available in Safari
if   ( WebAssembly   && ! WebAssembly  . instantiateStreaming  ) {
// polyfill
WebAssembly  . instantiateStreaming   =  async   (resp, importObject) => {
const   source =  await   ( await   resp). arrayBuffer  ();
return    await    WebAssembly  . instantiate  (source, importObject);
};
}


const   go =  new    Go  ();
( result  ) =>   {
go. run  (result. instance  );
}
);
}  else   {
console  . log  ( "WebAssembly is not supported in your browser"  );
}
</ script  >


```


Note the` fetch` call used to retrieve the binary data itself: this means that[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) must be enabled or the same web server used.


It is also **very important** to enable caching and compression of the WASM binary being loaded, as these WASM binaries can be quite large (ours is 25MB uncompressed); fortunately, our frontend performance tests caught this issue


## Calling SpiceDB from JavaScript


With SpiceDB loaded into the JavaScript context, the next step was determining a way that the two different environments could be connected.


Fortunately for us, the Go team also covered this use case via the[syscall/js](https://pkg.go.dev/syscall/js) library, which provides methods for easily accessing the JavaScript environment, converting objects between the two environments, and error handling.


For SpiceDB, we chose to use a callback-based approach for invoking the development methods: the[SpiceDB development package main](https://github.com/authzed/spicedb/blob/main/pkg/development/wasm/main.go) registers a single callback function` runSpiceDBDevelopmentOperations` , which is invoked with the set of operations to run, and invokes callbacks with its results.


Registering the callback via` syscall/js` proved to be very simple:


go


1


2


3


4


```text
c :=  make  ( chan    struct  {},  0  )
js.Global().Set( "runSpiceDBDevelopmentOperations"  , js.FuncOf(runDevelopmentOperations))
fmt.Println( "Development interface initialized"  )
<-c


```


go


1


2


3


4


```text
c :=  make  ( chan    struct  {},  0  )
fmt.Println( "Development interface initialized"  )
<-c


```


Note the use of` js.FuncOf` in the call, which converts the Go function into a JavaScript function: such conversion is necessary throughout the package, whenever a function or object needs to be serialized across the boundary of the environments:


go


1


2


3


```text
operation := op.Get( "operation"  ).String()
parameters := op.Get( "parameters"  ).String()
callback := op.Get( "callback"  )


```


go


1


2


3


```text
operation := op.Get( "operation"  ).String()
parameters := op.Get( "parameters"  ).String()
callback := op.Get( "callback"  )


```


With the ability to define arbitrary functions to be run inside the SpiceDB development package, we now had a complete toolset to use in the Playground:


js


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


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


```text
const   [devErrs, err] =  runSpiceDBDevelopmentOperations  (
JSON  . stringify  ({
'schema'  :  `definition user {}


definition document {
relation viewer: user
permission view = viewer
}`  ,
'relationships'  : [
{
resource_and_relation  : {
namespace  :  'document'  ,
object_id  :  'somedoc'  ,
relation  :  'viewer'  ,
},
subject  : {
namespace  :  'user'  ,
object_id  :  'foo'  ,
relation  :  '...'
}
}
],
}),
[
{
operation  :  'check'  ,
parameters  :  JSON  . stringify  ({
resource  : {
namespace  :  'document'  ,
object_id  :  'somedoc'  ,
relation  :  'view'  ,
},
subject  : {
namespace  :  'user'  ,
object_id  :  'foo'  ,
relation  :  '...'  ,
}
}),
callback  :  ( result, err  ) =>   {
console  . log  (result, err)
}
},
...
],
)


```


js


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


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


```text
const   [devErrs, err] =  runSpiceDBDevelopmentOperations  (
JSON  . stringify  ({
'schema'  :  `definition user {}


definition document {
relation viewer: user
permission view = viewer
}`  ,
'relationships'  : [
{
resource_and_relation  : {
namespace  :  'document'  ,
object_id  :  'somedoc'  ,
relation  :  'viewer'  ,
},
subject  : {
namespace  :  'user'  ,
object_id  :  'foo'  ,
relation  :  '...'
}
}
],
}),
[
{
operation  :  'check'  ,
parameters  :  JSON  . stringify  ({
resource  : {
namespace  :  'document'  ,
object_id  :  'somedoc'  ,
relation  :  'view'  ,
},
subject  : {
namespace  :  'user'  ,
object_id  :  'foo'  ,
relation  :  '...'  ,
}
}),
callback  :  ( result, err  ) =>   {
console  . log  (result, err)
}
},
...
],
)


```


A complete example of invoking the package can be found in[the example](https://github.com/authzed/spicedb/blob/main/pkg/development/wasm/example/wasm.html)


## Benefits


Conversion of the Playground to use these functions (instead of the previous grpc-web equivalents) proved very straightforward and immediately provided a number of benefits, such as the ability to use the playground's tooling offline and for a good deal of code reduction and cleanup. A bonus was that we were able to remove some supporting back end services and simplify our architecture.


However, there was a major side benefit that was unexpected when we began this project: **performance** .


Previous calls from the playground were grpc-web requests made over the network to the development API1 that was running within GCP. These calls could take over 50ms. Not necessarily slow but long enough to notice a delay, especially for large requests (schemas with many definitions and/or large test relationship data).


Currently with in-browser processing, the same requests often return in under 5ms, a 90% reduction in latency! But perhaps more importantly, latencies at this rate are perceived to be instant, which in the context of developing and iterating in the playground, can make the development experience more delightful by eliminating friction in the feedback loop. If Amazon famously found that an[extra 100ms of latency](https://news.ycombinator.com/item?id=273900) had negative impacts, hopefully gaining 45ms will have the opposite, positive impact for our users.


## Final thoughts


When we initially started the project, I expected far more rough edges than actually encountered, but the Go team has done an excellent job in making the use of Go via WASM straightforward, both in terms of compilation and invocation.


I recommend visiting[https://play.authzed.com](https://play.authzed.com/) now to give the new, faster playground a try!


Have questions about SpiceDB or the WASM integration? Visit the[Discord](https://authzed.com/discord)


## Notes


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


## Footnotes


1.


Another benefit of moving to WASM is that we can formally deprecate the majority of the Development API. See[the issue](https://github.com/authzed/spicedb/issues/764) for details.↩


On this page


- TL;DR:
- Background
- Some Assembly Required
- An early roadblock
- Adjusting SpiceDB to compile under WebAssembly
- Loading SpiceDB in JavaScript
- Calling SpiceDB from JavaScript
- Benefits
- Final thoughts
- Notes
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
