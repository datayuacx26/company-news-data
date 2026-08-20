---
schema_version: "1.0.0"
document_id: "3d738c72586b47cd498772f864ca144232a7e0c8a9b0532ab5402ecb5df91cc2"
company_key: "yc-pgdog"
company: "PgDog"
source_id: "yc-pgdog-rss-662afb283f74"
canonical_url: "https://pgdog.dev/blog/plugins-are-back"
published_at: "2025-08-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:49.478310+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:b225b59cac0e9fe626c2c7e2b354b68f5dff3e173e220acd946392dd4530b795"
---

# PgDog adds support for Rust plugins

Plugins are back! You can now inject your own logic directly into PgDog’s query router, which allows you to change its sharding and load balancing algorithms.


For context: PgDog is a Postgres sharding proxy written in Rust. We use FFI to call the Postgres SQL parser directly, allowing us to understand queries. Since we became comfortable with FFI basics, we thought why not try passing complex data structures like Abstract Syntax Trees across library boundaries?


At first, this seemed very difficult. C’s interop story isn’t great for complex data, and safely sharing memory between a host process and dynamically loaded plugins felt dangerous. Since we set out to solve problems like these in the first place, I ran a mental` sed -i s/unsafe/probably fine/g` on my Rust knowledge and got to work.


What follows is our journey building a safe, efficient plugin interface that gives plugins direct access to parsed SQL without serialization overhead. If you’re looking to build your own plugin, check out our docs[here](https://docs.rs/pgdog-plugin) and[here](https://docs.pgdog.dev/features/plugins/) .


#### The FFI challenge


FFI (Foreign Function Interface) is essentially about managing references to memory owned by another program. You can’t modify that program, but you can call its functions to do useful work. To make this work, we needed to know 2 things:


1. **Memory layout** : how much space the data occupies
2. **Data types** : what structures you’re working with


These are typically encoded in data structures, with types either passed as generics or agreed upon through documentation and compiler enforcement.


The catch? FFI in Rust means embracing` unsafe` code, where the compiler’s safety guarantees no longer apply.


To understand why this matters, consider Rust’s most popular data structure: the standard` Vec` . Despite its advanced interface, it’s fundamentally just three integers:


```text
struct    Vec    {
len  :    usize  ,         // Number of elements
ptr  :    *  mut    u8  ,       // Pointer to the data
capacity  :    usize  ,    // Allocated memory size
}


```


The actual implementation hides these inside` RawVec` , using pointer arithmetic for most operations. This is` unsafe` Rust at its core: code the compiler can’t verify, leaving that job to the programmer.


Here’s the thing: you’ve been using` unsafe` code throughout your entire Rust career. Much of the standard library is essentially “C written in Rust,” carefully wrapped in safe methods:


```text
pub    fn    safe_function  ()    {
// SAFETY: Extensive testing and documentation ensure correctness
unsafe    {
// Decades-old patterns, now with Rust's type system
}
}


```


` unsafe` doesn’t mean dangerous: it just means “programmer verified” instead of “compiler verified.”


#### Enter plugins


For plugins to be genuinely useful, they need dynamic loading: no recompilation of PgDog or the plugin required. This means compiling plugins as shared libraries and communicating through the C ABI.


Shared libraries are essentially programs without a` main` function. They’re loaded into another process at runtime using` dlopen(3)` , with FFI serving as the contract defining how the two interact.


Rust’s` libloading` crate provides an simple wrapper around this syscall, handling the loading mechanics and capability discovery. But first, we needed to define what our plugins could actually do.


#### The core challenge


Meaningful plugins need query access. Since PgDog uses` pg_query` for parsing SQL, we needed to pass[ParseResult](https://docsrs.pgdog.dev/pg_query/protobuf/struct.ParseResult.html) structures to plugins without expensive serialization or memory corruption.


` ParseResult` , underneath, is just a` Vec` of parsed statements:


```text
pub    struct    ParseResult    {
stmts  :    Vec  <  RawStmt  >  ,     // The Abstract Syntax Tree
}


```


Since` Vec` is just three 64-bit integers, we can decompose it into a Plain Old Data (POD) struct for the FFI transfer.


Decomposing a` Vec` requires three simple function calls:


```text
let    ptr    =    result  .stmts  .as_ptr  ();          // Pointer to the data
let    len    =    result  .stmts  .len  ();             // Number of elements
let    capacity    =    result  .stmts  .capacity  ();    // Allocated memory size


```


These three values contain everything needed to reconstruct the` Vec` on the other side of the FFI boundary.


##### Crossing the C ABI boundary


When working with C ABI, defining the interface in C first is good practice, even for Rust-to-Rust communication. Our` Vec` decomposition maps to a simple C struct:


```text
typedef    struct    Query    {
void    *  ptr  ;       // Pointer to the data
size_t    len  ;      // Number of elements
size_t    cap  ;      // Allocated capacity
}    Query  ;


```


Using` bindgen` , this becomes idiomatic Rust:


```text
pub    struct    Query    {
ptr  :    *  mut    c_void  ,
len  :    u64  ,
cap  :    u64  ,
}


```


Note: All this complexity is abstracted away in our[pgdog-plugin](https://docs.rs/pgdog-plugin) crate. Plugin authors work with high-level APIs while we handle the unsafe internals.


#### Making unsafe, safe


Surprisingly, everything so far has been completely safe Rust.` Vec::as_ptr()` might look dangerous, but the pointer remains owned by the` Vec` . No memory management issues yet.


The real challenge begins on the plugin side: converting that raw pointer back into something ergonomic. Plugin authors shouldn’t need pointer arithmetic to iterate over query statements.


This is where we enter the real` unsafe` territory.


Rust’s standard library comes with` Vec::from_raw_parts()` for exactly this situation. It’s marked` unsafe` because the programmer must guarantee several invariants:


1. **Allocator compatibility** : the` Vec` must use the standard global allocator
2. **Memory alignment** : the pointer alignment must match` Vec` ’s expectations
3. **Size immutability** : no resizing the reconstructed` Vec`


Rather than burden plugin authors with these requirements, we enforce them automatically within our crate.


PgDog uses jemalloc on Linux, which replaces the global allocator program-wide. Since all allocations go through the same allocator, this invariant holds automatically.


All our` Vec` s are allocated by Rust (including within` pg_query` ), guaranteeing proper alignment. We require (and automatically enforce) plugins use the same Rust compiler version as PgDog to prevent any theoretical alignment changes between compiler versions.


` ParseResult` is read-only after creation: we never mutate query structures post-parsing.


With these guarantees in place, we can safely reconstruct the` Vec` :


```text
let    stmts    =    unsafe    {
Vec  ::  from_raw_parts  (
stmt  .ptr    as    *  mut    RawStmt  ,
stmt  .len    as    usize  ,
stmt  .cap    as    usize
)
};
let    query    =    ParseResult    {    stmts    };


```


Now plugins have direct access to the complete Abstract Syntax Tree with zero serialization overhead. They can perform the same sophisticated query analysis as PgDog itself.


But there’s one more crucial piece left.


#### The ownership dance


Rust’s safety comes from clear ownership rules: when variables go out of scope, their destructors run and memory gets freed (RAII). This creates a problem for our reconstructed` ParseResult` .


By calling` Vec::from_raw_parts()` , we’ve claimed ownership of memory that actually belongs to PgDog. When the plugin finishes executing, Rust will try to free this “borrowed” memory, causing PgDog to crash when it later accesses what it thinks is still its data.


Fortunately, the standard library provides a solution for this common pattern:


```text
std  ::  mem  ::  forget  (  stmts  );


```


This disables the destructor, preventing the plugin from freeing memory it doesn’t own. Normally this would leak memory, but since we’re just returning ownership to PgDog, everything works just right.


*The complete plugin data flow, from query parsing to safe reconstruction.*


#### Plugin interface


With the memory management solved, plugins can now influence PgDog’s core routing decisions. Our interface is deliberately simple:


```text
use    pgdog_plugin  ::  prelude  ::  *  ;


#[macros::route]
fn    route  (  context  :    Context  )    ->    Route    {
// Plugin receives parsed query + parameters
// Returns shard number + read/write designation
todo!  ()
}


```


Plugins receive the full query context and return a` Route` specifying the target shard and whether the query can use a replica. All the complexity of FFI, memory management, and safety invariants is hidden behind the scenes.


### The bigger picture


What started as a weekend experiment became a great plugin system that gives third-party code direct access to our query parser’s output. Zero serialization overhead and complete memory safety.


The key insights:


- ` unsafe` doesn’t mean dangerous, it just means “programmer responsibility”
- Complex data can cross FFI boundaries safely with careful invariant management
- Good abstractions hide complexity from end users while maintaining performance


For a proxy like PgDog, where microseconds matter and query parsing is already done, this approach delivers the best of both worlds: the performance of direct memory access with the safety of Rust’s ownership model.


If you like what you see, consider writing your own plugin for PgDog. Examples of this are in our[repo](https://github.com/pgdogdev/pgdog/tree/main/plugins) ; and if you really like what we’ve been doing, give us a star on[GitHub](https://github.com/pgdogdev/pgdog) .
