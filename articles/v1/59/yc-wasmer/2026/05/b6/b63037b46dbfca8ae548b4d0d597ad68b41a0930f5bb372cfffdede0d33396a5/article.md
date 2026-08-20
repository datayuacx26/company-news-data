---
schema_version: "1.0.0"
document_id: "b63037b46dbfca8ae548b4d0d597ad68b41a0930f5bb372cfffdede0d33396a5"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/edgejs-safe-nodejs-using-wasm-sandbox"
published_at: "2026-05-20T13:33:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T21:44:39.747323+00:00"
content_hash: "sha256:eff536fadaac5aedb18be23985681aa22d2caa0242a9a94ecf3260d2313efd78"
---

# Edge.js: Running Node apps inside a WebAssembly Sandbox

Today we are incredibly excited to open source[Edge.js](https://edgejs.org/) , a JavaScript runtime specifically designed to safely run Node.js workloads for AI and Edge computing.


The goal is simple: run your existing Node.js applications safely and with[density and startup times that are impossible to get with containers](https://wasmer.io/posts/wasm-clouds-the-world-after-containers) .


Edge.js takes a different approach from existing JS edge runtimes like Deno or Cloudflare Workers. Instead of introducing new APIs Edge.js preserves full Node compatibility and isolates the unsafe parts of execution using **WebAssembly** .


This allows existing Node.js applications and native modules to run unmodified while system calls and native modules are sandboxed through[WASIX](https://wasix.org/) .


Edge.js is like Node.js: same architecture, same dependencies, and fully matching semantics.


- **Fully compatible with Node.js**
- **Pluggable JS engine** (V8, JavascriptCore or QuickJS)
- Securely **sandboxed** when in` --safe` mode


*TL;DR: all this means that you can now run your JS-based apps, MCPs, and agents safely sandboxed without Docker!*


## Edge.js story


In recent at Wasmer, we have been aiming to run JS and Node.js sandboxed via WebAssembly while also aiming to scale to millions serverlessly.


We don’t just **need full Node.js compatibility** , we also need to run it with **great startup and runtime speeds** comparable to other awesome serverless providers like Cloudflare Workers or Deno Deploy.


In the past we launched our own JS server for running WinterCG applications ([WinterJS](https://winterjs.org/) ), matching Cloudflare’s and Deno’s offering. However, over time we realized this approach had two issues that prevented broad usage: **speed** , and **compatibility** of apps (very few frameworks support WinterCG).


This pushed us to start thinking of alternatives that could mix the sandboxing capabilities of WebAssembly and WASIX with the already hardened, product-ready JS runtimes.


The architecture was obvious in retrospect, but it took many months and different trials to discover the optimal path.


We made Edge.js possible by splitting the sandbox into two different silos:


- **JS engine** : exposed via custom` napi` APIs. We already know JS is sandboxed by default, so no more hardening is needed for having JS fully sandboxed.
- **OS system calls** and native application code: isolation via[WASIX](https://wasix.org/) (read a file, create a thread, doing network operations, …)


The great thing about this design is that **we sandbox only what’s unsafe** : the OS system calls and native logic. The rest of things will still go through a JS runtime natively, which is sandboxed by default.


This allowed Edge.js to provide full Node compatibility (v24). We also decided to use the same dependencies as Node is using (libuv as the event loop, simdutf for fast utf8, ada for URL parsing, llhttp for http parsing, ncrypto for encryption, ares for dns…), so behavior could be easily preserved.


> Did you know that even[Deno is working to make its tokio event loop to behave like Node’s uv](https://github.com/denoland/deno_core/pull/1309) to unlock further Node compatibility?


Edge.js currently runs within ~5–30% of Node.js native speed JS when run using the` --safe` flag for sandboxing with significant improvements on the horizon.


Our goal is to make Edge.js the most friction-less and efficient runtime for Node workloads in sandboxed and serverless environments.


## Why not use Node.js


Node.js is a great piece of software, it runs millions of servers, its fast and incredibly robust.


However, Node.js has two architectural issues: is closely **tied to a specific Javascript engine** (V8: same engine that powers the Google Chrome, Brave and Edge browsers), and unlike V8 it **can’t run workloads safely** without Containerization or hardware virtualization.


Tying the Node.js workloads to slow-to-startup containers was a no-go for our hosting offering, since it would decimate our fast startup and high-server-density requirements.


> We have showcased Wasmer’s architecture in depth in our article:[WebAssembly Clouds: The world after containers](https://wasmer.io/posts/wasm-clouds-the-world-after-containers) .


We needed an engine that allows for **extremely high-density of applications** running in the same server, that has **fast startup times** and that provides all this while also being **fully sandboxed** .


Wasmer is not the first provider creating a JavaScript runtime, although we are the first one allowing **fully sandboxed execution of workloads without Docker containers** . Here’s the list of Node.js-like compatible runtimes:


- Bun (using JavascriptCore)
- Deno (using V8)
- LLRT (using QuickJS)
- WinterJS (using SpiderMonkey)


## Why not compile Node.js to WASIX


Projects like[Node-WASIX](https://github.com/Multi-V-VM/node-wasix32) succeeded in compiling and running V8 inside Wasm with WASIX and Wasmer (including compiling[Torque sources to Wasm](https://v8.dev/docs/torque) … an incredible feat!)


While this worked to some extent, it also introduced significant performance cost because V8 is running in interpreter mode inside of Wasm.


Edge.js instead runs the **JavaScript engine natively** and isolates only the unsafe parts of execution: system calls, threading, and native code. This reduces overhead while still providing strong sandboxing.


The coolest thing of all, is that the current Edge.js architecture model also allows embedding the JS engine inside of Wasm without any extra effort. So we can enable the V8-interpreter-inside-Wasm model very easily in case it is useful in the future!


## Why not use WinterCG


WinterCG was created by Edge providers (Cloudflare and Deno) as an alternative specification run JS applications on the Edge safely, and serverlessly.


Node.js was hard to run sandboxed, and thus they created a small subset to allow real JS applications on their offerings with a custom API.


While this solved their problem, it created a bigger issue: fragmentation of servers in JS and incompatibility with existing frameworks and servers. Now you are bound to a specification that your hosting company controls.


This strategy is not only fragmented, it’s also bound to **create a lot of overhead** as well. Cloudflare[reimplemented Next.js with Vite](https://github.com/cloudflare/vinext) to allow running those workloads in Cloudflare Workers.


While we applaud their initiative fully, we believe there’s a better way that allows not only Next.js but any framework to work at the Edge without having to reimplement it for a new hosting platform.


# How Edge.js works


Node did something really good architecture-wise:[napi](https://nodejs.org/api/n-api.html) .


NAPI allows native Node dependencies to target Node without actually depending on a specific version of Node or V8. NAPI abstracted the V8 JS engine away, by providing JS-like APIs for: creating an object, declaring a property, etc.


NAPI is the contract that all modern native Node modules use to interact with Javascript.


So we decided to bet on` napi` as the base abstraction layer of Edge.js (plus WASIX) to create the JS engine abstraction, while betting on WASIX for system call sandboxing.


This was not our first trial of running Node.js workloads though:


- We tried creating a[V8 shim on top of SpiderMonkey](https://github.com/wasmerio/spidershim) (that we compiled to Wasm) → Problem: it was too slow and very brittle on compatibility
- We tried[exposing the V8 C++ API as imports](https://github.com/wasmerio/v8-wasm-bridge) to the Wasm module → Problem: it could be used in an unsafe way, putting the sandbox at risk. It also would be hard to maintain over time if we wanted to keep updating V8 with new versions (it’s likely their API will evolve)


All this different trials and projects helped us realize that` napi` was the way to go, and thus: Edge.js was born!


## What Edge.js can run


Edge.js runs everything Node.js can run (we target Node.js v24 compatibility). No exceptions. It runs Next.js, Astro, and any other framework without any modifications whatsoever. If it doesn’t, it’s a bug.


> At Wasmer, we commit to solve any bugs on Edge.js within a week (we will need a reproducible example that works in Node but not in Edge.js)


Edge.js fully supports running Native modules as well, since all the modern native modules already target NAPI.


This is essential to provide fast JavaScript access in the Edge, while serving a fully-sandboxed Node.js serverless provider.


Here’s the compatibility table with Node.js modules.


**✅ All tests pass 🟢 > 90% tests pass. 🟡 60-90% pass. 🟠 30-60% pass 🔴 < 30% pass**


Modules / Tests passing EdgeJS Bun Deno


node:assert **✅ 24/24** **🔴** 3/24 **🟡** 18/24


node:buffer **✅ 63/63** **🟠** 33/63 **🟢** 57/63


node:console **✅ 24/24** **🟠** 9/24 **🟠** 12/24


node:dgram **✅ 70/70** **🟡** 51/70 **🟡** 49/70


node:diagnostics_channel **✅ 55/55** **🟠** 23/55 **🔴** 11/55


node:dns **✅ 18/18** **🟠** 9/18 **🔴** 4/18


node:events **✅ 34/34** **🟡** 25/34 **🟡** 29/34


node:fs **✅ 342/342** **🟠** 176/342 **🟠** 179/342


node:http **✅ 411/411** **🟠** 171/411 **🟠** 141/411


node:https **✅ 89/89** **🔴** 15/89 **🔴** 4/89


node:os **✅ 7/7** **🟡** 4/7 **🟡** 6/7


node:path **✅ 17/17** **🟡** 11/17 **🟢** 16/17


node:punycode **✅ 1/1** **✅ 1/1** **✅ 1/1**


node:querystring **✅ 4/4** **✅ 4/4** **✅ 4/4**


node:readline **✅ 19/19** **🟡** 13/19 **🟡** 14/19


node:stream **✅ 225/225** **🟡** 154/225 **🟡** 199/225


node:string_decoder **✅ 4/4** **🟡** 3/4 **🟡** 3/4


node:timers **✅ 63/63** **🟠** 37/63 **🟡** 43/63


node:tty **✅ 15/15** **🟡** 10/15 **🟡** 10/15


node:url **✅ 43/43** **🟠** 13/43 **🟠** 19/43


node:zlib **✅ 58/58** **🟡** 43/58 **🟡** 54/58


node:child_process **✅ 148/148** **🟡** 88/148 🟡 79/148


node:cluster **✅ 85/85** **🟡** 55/85 **🔴** 4/85


node:crypto **✅ 150/150** **🟡** 70/150 **🟡** 77/150


node:domain **✅ 48/48** **🔴** 2/48 **🟠** 15/48


node:http2 **✅ 248/248** **🟡** 105/248 **🟡** 138/248


node:module **🟢** 327/341 **🔴** 86/341 **🔴** 62/341


node:net **✅ 169/169** **🟡** 84/169 🟡 105/169


node:process **✅ 156/156** **🟡** 60/156 **🟡** 69/156


node:sys **✅ 1/1** **✅ 1/1** **✅ 1/1**


node:tls **✅ 195/195** **🟡** 76/195 **🔴** 35/195


node:util **✅ 98** / **98** **🔴** 21/98 **🟡** 39/98


node:worker_threads **✅ 145/145** **🔴** 46/145 **🟡** 69/145


node:repl **✅ 100/100** **🔴** 5/100 **🔴** 9/100


node:test **🟢** 154/161 **🔴** 6/161 **🔴** 27/161


**TOTAL PASSING** **🟢 3592/3626** **🟠** 1513**/**3626 **🟠** 1607/3626


Tests skipped from this suite:


- Tests that use modules highly tied to VM internals:` node:diagnostics_channel` ,` node:inspector` ,` node:v8` ,` node:addon` (new addons are fully supported, only legacy tests using v8 internals were skipped) and some` node:async_hooks` tests
- Tests for experimental modules not stable in Node:` node:wasi` ,` node:trace_events` ,` node:sqlite` ,` node:sea`
- Tests using custom flags: permission flags (we deprecated them in favor of` --safe` ),` --snapshot` flags (not yet supported) or using node’s` v8` flags (` --expose-gc` ,` ...` )


> Tests done in a macOS M3 Max laptop, using the[github.com/wasmerio/node-test](https://github.com/wasmerio/node-test) repository


## Performance


Edge.js is currently about 5-20% slower than current Node.js when run natively, and 30% when ran fully sandboxed with Wasmer. In some cases, when Native<>Wasm work is intense (when doing HTTP benchmarks), there could be a bigger gap, although we will focus on closing that gap for Edge.js 1.0 and next releases of Wasmer.


Due to missing js-snapshotting, the startup times for a given program are currently worse than Node.js, although we already have a plan in mind to solve this in an universal way for any JS engine that supports JS bytecode compilation for snapshots (V8 or QuickJS).


> During the first 0.x releases we made the conscious decision of **focusing on correctness over speed** .


We are extremely confident we can improve speeds drastically (both natively and when run with Wasmer). Check our[public roadmap to help us make it happen](https://github.com/wasmerio/ubi/issues/8) . Stay tuned for exciting updates!


## Why now?


The efforts of doing something of this scale have been drastically reduce thanks to AI. For implementing Edge.js we relied mainly on` gpt-5.4` . Without it, this project would have been unfeasible to achieve for a startup of our scale, since its development would have taken us at least a year or two instead of just a few weeks.


Even some developers from our team with no expertise in C++ or Node internals were able to collaborate fixing bugs thanks to Codex.


I’d like to personally thank[Ash](https://ashvardanian.com/) ,[Francisco](https://github.com/francisco-perez-sorrosal) , and[Jon](https://blog.jonabrams.com/) for the feedback they provided through the process of starting, developing and evolving Edge.js. And the Wasmer team, of course, for allowing Edge.js work with Wasmer and polishing the product to where is today.


Without OpenAI, their awesome Codex App and our beloved WebAssembly, Edge.js would not have been possible ❤️.


Welcome to the golden age of software, where everything is possible!


Next:


- Star the project on GitHub:[github.com/wasmerio/edgejs](https://github.com/wasmerio/edgejs?tab=readme-ov-file)
- Visit Edge.js website:[edgejs.org](https://edgejs.org/)


Install Edge.js now and let us know your thoughts!


```text
curl -fsSL https://edgejs.org/install | bash


```


And then, run it easily with:


```text
edge myscript.js
# Or wrap your existing node/npm commands so node is aliased to edge
edge pnpm run dev


```
