---
schema_version: "1.0.0"
document_id: "6ae5dc908e17d199a9f60a3f25f397a31b39403fa00f3374053614cb5a4ef6f9"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/greenlet-support-python-wasm"
published_at: "2026-05-20T13:33:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:a73c0693863b16ef831678ddaa4adc46d9c51055f27cff0c42a1cd1e7ef6da9f"
---

# Introducing Greenlet support for Python in WebAssembly

Today we are incredibly happy to announce full greenlet support for Python in WebAssembly using Wasmer.


This unlocks running real-world Python backends (using SQLAlchemy, etc.) in WebAssembly (on the edge and in the browser) without any rewrites or async refactors.


Greenlets allow cooperative multitasking without blocking, similar to` async` coroutines but without explicit` await` points.


However, until today, there was no Wasm runtime able to run Python greenlets natively neither on the server nor in the browser via Pyodide, since they require switching stack contexts. The Greenlet library has over **250 million downloads** per month, and the lack of support meant many applications could not run on Wasmer or Wasmer Edge.


We added support for greenlets by **enabling context switching APIs** without relying on the Wasm Stack Switching proposal (which is not yet supported in browsers), by using a new workaround inspired by Wasm JSPI proposal (enables stack switching on Wasm via JS).


To support greenlets, we had to:


- Add support for async functions into the Wasmer API
- Create three new WASIX system calls for working with continuations


## What is Greenlet?


A greenlet is a Python-level implementation of a green thread: a lightweight, stackful execution context scheduled in user space.


If you are unfamiliar with green threads, they are conceptually similar to coroutines (Python, JavaScript) or fibers (PHP, Lua, Ruby).


```text
from greenlet import greenlet


def func1():
print("A")
g2.switch()     # jump into func2
print("C")      # resume after func2 returns


def func2():
print("B")
g1.switch()     # go back into func1


g1 = greenlet(func1)
g2 = greenlet(func2)


g1.switch()         # start execution


```


Concept Scheduling Stack? Preemptive? Typical Use


**Green thread** User-space, thread-like abstraction Own stack Cooperative or preemptive Concurrency without OS threads


**Coroutine** Cooperative; explicit yield *No separate stack* (stackless) Never preemptive Asynchronous flows, generators


**Fiber** User-space, manual context switching Own stack Cooperative Lightweight thread with explicit control


*Note: many languages such as Lua and Ruby call them coroutines even though they are fibers concept-wise (they require their own stack). Fibers and green-threads are operationally equivalent (they require their own stack), while coroutines are stackless.*


## Why is it challenging to run green threads / fibers in WebAssembly?


While coroutines can be fully implemented inside the WebAssembly emitted code (you just need to include the scheduler implementation inside the WebAssembly generated module), Fibers and green threads currently can’t without native stack switching support in WebAssembly.


The Stack Switching proposal introduces a small set of new Wasm instructions:


- ` cont.new` : creates a new continuation that can yield execution to others
- ` cont.bind` : is a monad for continuations (it returns a new function that calls the continuation with extra arguments)
- ` suspend` : it suspends the current running continuation
- ` resume*` ,` switch` , : it invokes a suspended continuation


With these operations, you can implement any coroutine, continuation or green thread in almost any language.


> If you want to read more about it, please check the reference slides for the proposal presented in WasmCG in February 2025:[https://effect-handlers.org/talks/stack-switching-wasmcg-feb2025.pdf](https://effect-handlers.org/talks/stack-switching-wasmcg-feb2025.pdf)


Unfortunately, the Wasm Stack Switching proposal is an experimental proposal that is not yet supported or enabled in any of the popular WebAssembly runtimes (V8, SpiderMonkey, JavascriptCore, Wasmer, Wasmi, …).


## How we made it possible


About a year ago (June 2024) Google Chrome launched JSPI:[https://v8.dev/blog/jspi](https://v8.dev/blog/jspi) .


JSPI allows WebAssembly to import and call functions that are asynchronous. JSPI is equivalent to the Wasm Stack Switching proposal, but it adds the APIs on the JS layer rather than as new WebAssembly operations, by introducing two new APIs:


- ` WebAssembly.Suspending` allows importing async functions
- ` WebAssembly.promising` allows calling wasm functions that can yield (that is, calling suspended imported functions under the hood)


> JSPI enables async suspension across the Wasm boundary, but does not provide stackful execution by itself.


An example might make this easier to understand:


```text
const module = `(module
(import "env" "sum" (func $sum (param i32 i32) (result i32)))
(func (export "run") (result i32)
i32.const 40
i32.const 2
call $sum
)
)`;


async function sum(a, b) {
// Do something async
return a + b;
}


const { instance } = await WebAssembly.instantiate(wat2wasm(module), {env: {
sum: new WebAssembly.Suspending(sum)
}});


result = await WebAssembly.promising(instance.exports.run)();
console.log(result); // 42


```


> Read more about the JSPI integration proposal on their Spec overview page:[https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md](https://github.com/WebAssembly/js-promise-integration/blob/main/proposals/js-promise-integration/Overview.md)


JSPI is now available in stable Chrome and[Node.js 25](https://github.com/nodejs/node/pull/59941) . Firefox and WebKit are also actively implementing support \[[1](https://bugzilla.mozilla.org/show_bug.cgi?id=1897981) \]\[[2](https://github.com/WebKit/standards-positions/issues/422#issuecomment-3321209076) \].


So we decided to enable similar functionality in Wasmer with a new Async Function API.


## New Async Function API in Wasmer 7.0


We mirrored the JS Promise Integration proposal in the Wasmer Rust API, using native async syntax and without explicitly tying execution to any scheduler (tokio or others).


> You will need to use the feature flag` experimental_async` if you use the crate directly, to be able to access the Rust APIs directly. The API is highly likely to change.


Here’s the table mapping how the JSPI maps into Wasmer:


JSPI (WebAssembly JS API) Wasmer async API


` WebAssembly.Suspending`` Function::new_async` ,` Function::new_typed_async`


` WebAssembly.promising`` call_async`


So, the previous example in JavaScript, is equivalent to the following:


```text
let mut store = Store::default();
let module = Module::new(&store, r#"(module
(import "env" "sum" (func $sum (param i32 i32) (result i32)))
(func (export "run") (result i32)
i32.const 40
i32.const 2
call $sum
)
)"#)?;


// Async host function (JSPI equivalent of `WebAssembly.Suspending`)
let sum = Function::new_typed_async(&mut store, async |a: i32, b: i32| {
a + b
});


let import_object = imports! { "env" => {
"sum" => sum,
}};


let instance = Instance::new(&mut store, &module, &import_object)?;
let run = instance.exports.get_function(&mut store, "run")?;


// Async boundary (JSPI equivalent of `WebAssembly.promising`)
let result = run.call_async(&store.into_async()).await?;


println!("{result}"); // 42


```


## Context-switching API


With the new Wasmer async API for functions, we only have half of the picture solved.


To complete the design, we exposed explicit context-switch operations so WASIX programs can manage continuations directly.


For now, we added the following functions into WASIX:


```text
// Identifier of the main context of the current context-switching
extern wasix_context_id_t wasix_context_main;


// Creates a new context in the suspended state
int wasix_context_create(wasix_context_id_t *context_id,
void (*entrypoint)(void));


// Invokes a suspended continuation
int wasix_context_switch(wasix_context_id_t target_context_id);


// Destroys the context
int wasix_context_destroy(wasix_context_id_t target_context_id);


// [wasix/context.h](https://github.com/wasix-org/wasix-libc/blob/main/libc-bottom-half/headers/public/wasix/context.h)


```


And now, we can let` greenlet` use these new API functions instead of the custom assembly it uses on other platforms for doing the stack switching.


---


## Try it now!


The latest version of Wasmer (` 7.0` ) has full support for the new stack switching api, install it so you can start using it locally!


We have prepared a template in Wasmer, so you can deploy a Todo application made with FastAPI + SQLAlchemy easily:


[https://wasmer.io/templates/fastapi-sqlalchemy](https://wasmer.io/templates/fastapi-sqlalchemy)


Or you can also use the CLI!


```text
$ git clone https://github.com/wasmerio/examples.git
$ cd examples/python-fastapi-sqlalchemy
$ wasmer deploy


```
