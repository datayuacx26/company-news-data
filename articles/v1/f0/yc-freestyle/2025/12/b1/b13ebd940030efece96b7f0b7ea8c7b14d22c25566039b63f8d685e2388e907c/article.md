---
schema_version: "1.0.0"
document_id: "b13ebd940030efece96b7f0b7ea8c7b14d22c25566039b63f8d685e2388e907c"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/rust-crates-2025"
published_at: "2025-12-15T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T22:24:58.378758+00:00"
content_hash: "sha256:e4c2790ebb30ed82824b00c04d7ed29855802bb93bbe57ff47efdc5940d1978c"
---

# Our teams' favorite Rust crates 2025

People who know us personally have heard us say before “You should switch to Rust” — and finally some of our friends are taking that advice. As a relatively young language the Rust ecosystem evolves quickly. New libraries appear every week, and many solve real problems without ever breaking into the mainstream conversation. This is a list of our favorite rust crates that we think all rustacians should be aware of.


## Codebase Management


### [TestContainers](https://crates.io/crates/testcontainers)


I wish I had found this sooner. TestContainers makes it easy to run Docker containers for integration testing. It supports a wide range of databases, message brokers, and other services out of the box, and you can easily define your own custom containers as well. We use this extensively at Freestyle for running integration tests with real database setups easily. You can finally define your test environment as code instead of writing complex setup scripts for CI and local dev.


### [Bon](https://crates.io/crates/bon)


Bon builds builders. It provides a derive macro for creating builders for your structs, functions, and anything else. It uses powerful type-level programming to ensure that all required fields are set at compile time, and it supports optional fields, default values, and more. The generated builders have a clean and ergonomic API, making it easy to construct complex objects.


### [Strum](https://crates.io/crates/strum)


Strum is useful for everything you've ever wanted to do with enums. Specifically EnumIter to allow you to iterate over every item in your enum, and EnumString to easily go from Enum to String and back.


### [DotEnvy](https://crates.io/crates/dotenvy)


This is one of the weirdest things in rust — the dotenv crate is unmaintained and seemingly abandoned. DotEnvy is a maintained fork of it for loading your environment variables. We used dotenv for a while because we didn't know better, it caused problems.


## Parsing


### [Chumsky](https://crates.io/crates/chumsky)


Chumsky is a parser combinator library with a focus on error reporting and recovery. It is a bit less performant than some other parser combinator libraries (it's still fast, don't worry!), but it makes up for this with its excellent error messages and its ability to recover from errors and continue parsing. This makes it a great choice for building user-facing parsers for compilers, shells, etc. where good error messages are important.


### [Hexplay](https://crates.io/crates/hexplay)


Ever have to work with binary data? Debugging can be rough. Hexplay is a simple crate for displaying binary data in a hex editor style with configurable highlighting. It's great for debugging binary protocols, file formats, and such.


### [Bytemuck](https://crates.io/crates/bytemuck)


Bytemuck is a serialization-friendly crate that provides zero-copy conversion between types. It allows you to safely cast between types as long as they have the same size and layout, making it useful for working with raw bytes and ensuring memory safety. It's particularly helpful when implementing performance-critical applications that require dealing with binary data, such as GPU workloads.


## Macros


### [Darling](https://crates.io/crates/darling)


Ever wanted to write your own derive macros? Darling makes doing that about as easy as it can be to parse attributes and meta items in your proc macros. Doing this manually is a pain, and Darling abstracts away much of the boilerplate and complexity involved.


### [Inventory](https://crates.io/crates/inventory)


Inventory is a crate for keeping track of things at compile time via macros. This is extremely useful for when you want to track sets of variables, strings or anything else you can think of in your codebase for codegen at, or after compile time.


## RPC


### [Utoipa](https://crates.io/crates/utoipa)


Utoipa is a framework for declaratively documenting APIs in Rust via proc macros. It provides a simple way to generate OpenAPI documentation and supports various protocols and formats for API development. It integrates well with popular web frameworks like Axum, making it easy to add documentation to your existing projects. It also supports several documentation frontends, allowing you to serve your API docs easily from your application.


### [Tarpc](https://crates.io/crates/tarpc)


Tarpc is a simple RPC framework targeted entirely at Rust. It's not as fully-featured as something like gRPC, but it's much simpler to use with Rust and is easy to integrate with your projects. Its transport system is generic, allowing you to use it over TCP, Unix sockets, or even in-memory channels for message passing within a program.


### [Schemars](https://crates.io/crates/schemars)


Schemars is a set of macros for generating JSON Schemas for Rust structs. The schemas are generated at compile time, which makes build scripts with codegen exports extremely doable.


## Data Structures


### [Lock_api](https://crates.io/crates/lock_api)


Lock API is a really cool and unique crate — I haven't seen many projects like Lock API outside of the Rust community. It is a crate that purely defines traits which other lock implementations can implement, which allows rust lock systems to have a standard interface.


### [Papaya](https://crates.io/crates/papaya)


Everyone knows DashMap. DashMap is fast and looks great on benchmarks, but it uses sharded locking which can lead to contention under certain workloads. Papaya is a lock-free concurrent map that uses atomic operations and deferred memory reclamation to achieve high throughput without contention. The main caveat of this is mild eventual consistency: references to a value can be held after the value has been removed from the map (though the value will still be inaccessible through the map after deletion). This is rarely a problem though in practice.


It also supports incremental resizing which is great for latency-sensitive workloads where a spike due to blocking resize isn't acceptable.


## Other Languages


### [MLua](https://crates.io/crates/mlua)


Ever wanted to add scripting to your Rust application? MLua is a safe high-level binding to Lua, a simple but powerful scripting language. It supports all major 5.x Lua versions, and even supports LuaJit and Roblox's Luau.


Mlua makes it very easy to call Rust functions from Lua and vice versa. You can even share Rust data structures with Lua, allowing for seamless integration between the two languages. With LuaJIT this is even more powerful, as you can leverage its FFI capabilities.


It also has built-in support for async/await, allowing you to write non-blocking scripts that can interact with Rust's async ecosystem.


### [V8](https://crates.io/crates/v8)


The V8 crate allows you to run JavaScript isolates from Rust, you have full control over the underlying V8 platform, and control over the event loop of the isolates. Deno is also built in Rust, and built on this package.


### [WGPU](https://crates.io/crates/wgpu)


WGPU is a weirdly well developed graphics crate for Rust. It's relatively young compared to most things in graphics, but its been stable and production ready since before WebGPU hit most browsers. Writing Compute Shaders in Rust is generally very challenging as the ecosystem is not as well developed as C++, the best experience we've had writing them in Rust has by far been with this crate.


## Other


### [Hickory](https://crates.io/crates/hickory-dns)


Hickory is a set of crates for working with DNS. The crates include a fully built out dns server, along with the building blocks to make your own DNS server, or interact with other DNS servers.


### [Embassy](https://crates.io/crates/embassy-executor)


Embassy is a framework and async runtime for Rust. It is made to run on anything, I've run it on ESPs and Raspberry Pis without any operating system under it. Because of its library of HALs its extremely nice for working on embedded systems.
