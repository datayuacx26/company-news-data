---
schema_version: "1.0.0"
document_id: "aca2421be06508c809d2602de2c8afa172f440c69ebb5d348a1afc1ba7fa4510"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/dynamic-linking-in-wasm-wasix"
published_at: "2026-05-20T13:33:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:41b322c0591b43205f9be8dd54f749003abaeab0e1cc7d4176db1922828f2cc8"
---

# Dynamic linking in WebAssembly with WASIX

At Wasmer, we've had a[WASIX](https://wasix.org/) build of Python for a long time now. It works too; you can run scripts, get a REPL, and even pull in pip packages. However, there's a catch: with[the Python 3.12 build](https://wasmer.io/wasmer/python) , you could only use pure-Python packages. With[the new Python 3.13 build](https://wasmer.io/python/python) , this limitation no longer exists.


This limitation was due (in part) to a rather big limitation in WASIX. When a Python module includes native code, that code is built into a[shared library](https://en.wikipedia.org/wiki/Shared_library) . The Python interpreter then loads the shared library at runtime, making the functions contained in it available to the rest of the scripts. WASIX did not support loading shared libraries.


## What is Dynamic Linking?


Dynamic Linking happens when some binary only has a part of the code it needs to function, while the rest of the code lives in a different binary (a shared library). You've almost certainly used it at some point, even if you didn't know it was happening. After all, when you run almost any executable on a Linux machine, one of the first things it does is to dynamically link against glibc (or musl, if you're using a more minimal distro like Alpine).


There are two flavors of Dynamic Linking. ELF binaries can statically specify that they require functionality from another shared library in their` .dynamic` section. When loading such a binary, the OS dynamic linker will look for the requested library and load it automatically. These are known as` needed` modules.


The second way is for Dynamic Linking to happen at runtime. A binary can use the` dlopen` function to load in a shared library, and subsequently use` dlsym` to load symbols from it. This is the approach taken by the Python interpreter when loading modules with native code.


## How does Dynamic linking work with WASM binaries?


While` dlopen` and` dlsym` are just normal functions that can be provided to a WASM module, needed modules require support in the WASM format itself, as well as from the compilers and toolchains generating the WASM modules. Luckily, there's already a[spec for Dynamic Linking](https://github.com/WebAssembly/tool-conventions/blob/main/DynamicLinking.md) and it's supported by LLVM (and, by extension,` clang` ).


A Dynamically-Linked WASM module contains, as its first section, a` dylink.0` section that contains information about needed modules, as well as memory requirements and metadata about imported and exported symbols. WASM already has the concept of imports and exports, and those are used to support Dynamic Linking as well.


In the context of Dynamic Linking, a module can export two things:


- Functions are exported in[the usual way](https://www.w3.org/TR/wasm-core-2/#exports%E2%91%A0) .
- Data is exported as globals pointing to the location of the data in the module's memory.


- Additionally, exported data can be thread-local. More on this later.


Imports can be one of 3 things:


- Functions, imported directly; these will show up as normal WASM function imports in the` env` namespace.
- Functions, imported as a function pointer; these will show up as globals in the` GOT.func` namespace. The global is expected to contain the index of the function in the indirect function table, which is the same table used by[the call_indirect operation](https://www.w3.org/TR/wasm-core-2/#syntax-instr-control) . Note that function pointers in WASM are just indices into the indirect function table.
- Data, which is imported as a global in the` GOT.mem` namespace, and expected to contain a pointer to the data in the module's linear memory.


In comparison, runtime Dynamic Linking is rather simple.` dlopen` calls can be handled by the WASM runtime, locating and instantiating the requested module.` dlsym` calls will then search through all instantiated modules looking for the requested symbol, and return one of two things:


- If a function was found, it is appended to the indirect function table and the new index returned.
- If a data export is found, its address is returned.


## How do you fit multiple modules into the same linear memory?


Good question! This is where the concept of[Position-Independent Code](https://en.wikipedia.org/wiki/Position-independent_code) (PIC for short) comes into play.


Normally, WASM modules can just assume they own all of their memory. A module will either import or export a memory instance, and that memory instance will belong only to that module over its entire life.


However, with Dynamic Linking in the picture, all modules must share the same memory; after all, how else can they pass data between themselves? Things as simple as passing some string data into another module's function will be essentially impossible without a shared memory.


This is why all modules that wish to participate in Dynamic Linking must be compiled with PIC enabled. When a module is compiled with PIC, it will have a new global named` __memory_base` , which will be the offset from which it can find its static data. If some data used to exist in memory address` 42` , it now lives at` __memory_base + 42` .


Also, all DL modules must import their memory, and can make no assumptions about the size of the memory; what happens instead is that, in the` dylink.0` section, each module specifies how much memory it requires for its static data. It is the linker's job to allocate memory for each module as it's being loaded in, and put the address in` __memory_base` .


The linker can just allocate new memory pages whenever it can't fulfill a module's memory requirements. Luckily for us, WASM memory is allocated in pages, with no guarantees about address continuity between successive` memory.grow` instructions, so the memory allocations made by the linker and the module's own memory allocator end up playing nicely together.


The same is true of the indirect function table; each module specifies how many entries it needs, and the linker allocates entries as needed. There's similarly a new` __table_base` global as well, which gets initialized to the start of the area allocated for the module.


## What about threads?


So far, I've used the word "module" loosely to refer to WASM code that wishes to run within a runtime. To discuss threads, we must use the correct terminology from the WASM spec:


- A "[module](https://www.w3.org/TR/wasm-core-2/#modules) " corresponds to a WASM binary, with everything it defines: functions, imports, exports, globals, etc.
- An "[instance](https://www.w3.org/TR/wasm-core-2/#module-instances) " is the runtime representation of a module. It owns the actual memory, data, execution state, etc. that have to exist for the execution of a WASM module.
- A "[store](https://www.w3.org/TR/wasm-core-2/#store) ” contains all the state belonging to an executing WASM program.


Importantly, the same module can be *instantiated* multiple times to create multiple instances. Each instance lives in exactly one store, but the same store can contain multiple instances.


To support threads, WASM programs use these concepts rather creatively. In the case of non-dynamically-linked WASM modules, an executable is a single WASM file, corresponding to a single module. To create new threads, a new store is created, and a new instance of the module is created in the new store. The new instance gets its own copy of everything (globals, tables, imports, etc.) *except* the linear memory; the memory is the only WASM entity that can be shared among multiple stores.


In a dynamically-linked application, the executable and each shared library is its own WASM module. The linker instantiates each module once into the same store, since they need to share (via imports and exports) not just their memory, but their functions, globals, and tables as well.


Thus, in order to spawn a new thread, *each* module needs to be instantiated into the new thread's store again. This means that, given a dynamically-linked application with M modules and N threads, you end up with N stores, each containing M instances, for a total of M*N instances.


## Runtime dynamic linking with threads


I mentioned each thread gets its own store. This means that each one also gets its own indirect function table. Consider this scenario:


- Thread A loads a new shared library,` libcool.wasm` , and` dlsym` s a new function pointer out of it.
- The linker places that function into the thread's indirect function table and returns the index.
- Thread A then passes this function pointer to thread B.
- Thread B calls the function pointer.


Since thread B has its own indirect function table, the function pointer points to an invalid index that doesn't even exist in B's table. What's worse, thread B doesn't even have an instance for` libcool.wasm` at all!


All of this happens because thread A never communicated to B that it was loading a new shared library. On native platforms, a function pointer is just that: a pointer to some code somewhere in the system's memory, so no special care needs to be taken. In WASM, that is not the case.


To correctly implement Dynamic Linking with threads in the picture, we implemented a rendezvous mechanism. When thread A performs a DL operation (such as a` dlopen` or` dlsym` call), before control is returned to guest code, it goes through these steps:


- First, it retrieves the number of active threads.
- A barrier is created with the same size.
- The barrier is then broadcast to all other threads.
- An atomic bool is flipped to true, signalling that a new DL operation is in progress.
- All other threads receive a wake-up signal, so they can process the operation and go back to sleep in case they are sleeping or otherwise waiting for async operations to finish.
- Thread A then starts waiting at the barrier.
- Other threads eventually see the atomic bool.
- Each one receives the barrier and starts waiting on it.


- Note that threads can only look at the atomic bool once they make a syscall; if one thread does a DL operation when another is busy doing some CPU-heavy work, the entire application will be blocked until the thread doing the CPU work finally makes a syscall.


- Thread A then resets the atomic bool back to false, and broadcasts the operation.
- Thread A then waits on the barrier a second time.
- Other threads receive the operation and perform it for themselves; this involves instantiating newly loaded modules and placing functions in their indirect function table.
- Once each thread is done processing the operation, they also start waiting on the barrier.
- Once all threads hit the barrier a second time, we know the operation has been performed by every thread in the application; we can now safely return from the original call in thread A.


## Then, there's TLS


We're not done yet. Remember I mentioned talking about TLS later on? Now's the time to do it!


WASM modules generally have more than one data section; if you look at the disassembly of a module, you'll notice` .data` and` .rodata` (which contain mutable and read-only static data, respectively), as well as` .tdata` , which contains thread-local data. Static data is put into memory once, and all threads share the same data. Thread-local data is copied into memory once per thread, or, in other words, once per WASM instance.


In non-dynamically-linked applications, the logic is rather simple.` wasm-ld` takes care of reserving space in the module's static memory for the main thread's TLS area. New threads get their TLS area[allocated for them in](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/libc-top-half/musl/src/thread/pthread_create.c#L494)[pthread_create](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/libc-top-half/musl/src/thread/pthread_create.c#L494) . Everyone's happy.


In a DL scenario, this gets more complicated. There are now 4 separate cases to consider:


- Main thread, main executable: same as main thread in non-DL applications, the space is already reserved by wasm-ld.
- Main thread, shared libs: each shared library has its own static memory area, and TLS space for its main thread is reserved in that area.
- Worker threads, main executable: the same logic as non-DL applies here as well;` pthread_create` can allocate and copy over the TLS data.
- Worker threads, shared libs: this is where it gets... hairy.


When spawning a new thread and instantiating every shared lib, we need to give each one its own TLS area. Problem is, the linker has no way of knowing how much space is required (unlike the static memory, this information does not exist in` dylink.0` ) and also has no way to initialize it either. This is where[scrt1.o](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/libc-bottom-half/crt/scrt1.c#L18)[comes in](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/libc-bottom-half/crt/scrt1.c#L18) . Each shared lib gets an exported function named` __wasix_init_tls` , that allocates a new TLS area using the guest-side memory allocator and uses the` __wasm_init_tls` function generated by` wasm-ld` to initialize it. The linker knows to call this function when re-instantiating shared libs.


To let TLS work across shared libraries, you also need to switch the TLS model to` global-dynamic` . By default, WASM binaries are built with the` local-exec` TLS model, which assumes all TLS symbols will exist within the same module.` global-dynamic` alleviates that by doing a proper lookup at runtime.


Interestingly enough,` clang`[forcefully sets the TLS model back to local-exec](https://github.com/llvm/llvm-project/blob/dca088023e2b126e67989070d8d47aeea44b414f/llvm/lib/Target/WebAssembly/WebAssemblyISelLowering.cpp#L2014) for all non-Emscripten WASM targets. To fix this (and some more TLS-related issues), we had to create[our own fork of LLVM](https://github.com/wasix-org/llvm-project/tree/wasix-21.1.2) .


## Wait, where do the libc functions go?


Again, good question. On native platforms, the OS provides its own glibc (or musl) shared library, and applications will link against that at startup. However, a WASM application has no underlying OS to provide that library.


You might argue that one can just link libc into every shared lib and let dead code elimination take care of the rest, and you'd be partially right; however, libc has[lots](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/dlmalloc/src/malloc.c#L2640)[of](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/libc-bottom-half/sources/__wasilibc_initialize_environ.c#L14)[static](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/libc-bottom-half/sources/getcwd.c#L7)[state](https://github.com/wasix-org/wasix-libc/blob/a55bb39dc678cda844f42d24d7235ffe2707d349/libc-bottom-half/sources/preopens.c#L28) that needs to exist exactly once per process. Statically linking libc into shared libs would give each shared library its own copy of the static state, and that's no good; for example, if you called` setenv` in one shared lib, you wouldn't get the new env back in another.


What happens instead is that all libc functions are linked into and exported from the main executable. This means executables that expect to perform DL operations will be slightly larger than normal (by ~600KB for just libc, ~1.6MB if you also include libc++). Shared libs will then import the libc functions; the dynamic linker takes care of setting that up.


This bloat from including all of libc, as well as the inherent slowdowns from enabling PIC, made us turn DL-enabled WASIX applications into their own configuration; this is the` -ehpic` (exception handling + PIC) variant in[wasix-libc releases](https://github.com/wasix-org/wasix-libc/releases) , as well as the new` wasm32-wasmer-wasi-dl` target in the[WASIX Rust toolchain](https://github.com/wasix-org/rust/releases) (yes, Rust supports all of this as well!)


## Circular dependencies


Putting libc functions in the executable is all well and good until you realize that the executable itself can also import functions from its needed shared libs. Imagine the executable importing a function` foo` from some shared lib, and` foo` calls` printf` , which exists in the executable. We have a circular dependency.


Circular dependencies are bad for business because you need to build your imports object before you instantiate your WASM module. This means that, if a module imports some function` foo` from another module, the second module will need to be instantiated first, so we can get the` foo` function out of it and put it in the imports object for the first module.


Circular dependencies can be fixed up rather easily for pointer symbols (` GOT.mem` and` GOT.func` ). You can just give the module an uninitialized pointer at first, and once you're done instantiating every module, you just fix up the values of the pointer.


With functions, however, you need an actual function you can put in the imports object. To fix this, we have to build stub functions.


Going back to the original example, the dynamic linker will start looking at the executable. It'll then run into the` needed` entry for the shared lib, and try to load the shared lib in. When instantiating the shared lib, we don't have access to the executable yet, because we're not done instantiating it; instead, the shared lib will get a stub for` printf` .


Later, at runtime, when the stub is called the first time, it'll go and look at all available instances (including the executable), and find the` printf` function. It'll cache the function so that later calls are fast.


However, this does create the rather unique case of stubs failing to resolve something when the WASM code is already running. If the stub fails to locate the` printf` function, it'll trap.


## RUNPATH


By default, the dynamic linker only looks at certain default paths, as well as paths provided by the user in` LD_LIBRARY_PATH` . This is not enough for self-contained applications that ship some of their own shared libs; you'd want the linker to also look next to your executable, or maybe in your` libraries` folder.


To fix this, native binaries can have an RPATH or RUNPATH subsection in their` .dynamic` section. This is a list of paths that the system's dynamic linker will take into account when resolving libraries specified in the` needed` subsection.


The corresponding subsection in WASM is` runtime-path` in` dylink.0` . Back when we first started developing the WASIX dynamic linker, LLVM didn't support this. Luckily for us, LLVM 21 added support, and we submitted[our own patch](https://github.com/bytecodealliance/wasm-tools/pull/2283) to` wasmparser` . With this, the usual` -Wl,-rpath` argument to` clang` now works as usual for dynamically-linked WASM modules.


## Demo time!


Let's put all of this together. If you wish to follow along, you'll need a[sufficiently recent version of Wasmer](https://github.com/wasmerio/wasmer/releases) (6.1.0 and upwards will support Dynamic Linking), as well as[a working installation of](https://github.com/wasix-org/wasixcc)[wasixcc](https://github.com/wasix-org/wasixcc) .` wasixcc` will download the correct wasix-libc artifacts, as well as the artifacts from the LLVM fork mentioned earlier, giving you a working WASIX environment.


Let's create 3 separate binaries. One will be the main executable, one will be a needed shared library that's linked in automatically at instantiation time, and the other we'll manually link against at runtime.


First, let's create the needed library. Put this code into` libneeded.c` :


```text
#include <stdio.h>


void needed_say_hello() {
printf("Hello from the needed library!\\n");
}


```


and compile it:


```text
$ wasixcc -fwasm-exceptions -fPIC -Wl,-shared libneeded.c -o libneeded.so


```


Let's take that invocation apart.` -fwasm-exception` enables WASM exception handling, which is needed for DL modules to work; if you don't enable it,` wasixcc` will complain:


```text
$ wasixcc -fPIC -Wl,-shared libneeded.c -o libneeded.so
Error: PIC without wasm exceptions is not a valid build configuration


```


` -fPIC` enables PIC. While PIC and Dynamic Linking are disjoint features, there is very little reason why you'd need PIC in a non-DL WASM module, so` wasixcc` just takes that to mean "enable PIC, but also build a DL module for me". Lastly,` Wl,-shared` tells` wasixcc` to generate a shared library rather than an executable.


Just for fun, let's look at the text representation of the newly compiled module:


```text
$ wasm-tools print libneeded.so
(module
(@dylink.0
(mem-info (memory 36 0))
)
...
(import "env" "printf" (func (;0;) (type 1)))
...
(export "needed_say_hello" (func 5))
...
(func (;5;) (type 0)
i32.const 0
global.get 0
i32.add
i32.const 0
call 0
drop
return
)
...
)


```


The first thing you'll see is the` dylink.0` section, with the memory requirements for this module. You can then see the` printf` function being imported at index 0. The` needed_say_hello` function we created is being exported, and you can see how it calls` printf` (the` call 0` instruction in the function body.)


Let's create the next library. Put this into` libdlopened.c` :


```text
#include <stdio.h>


void dlopened_say_hello(char* message) {
printf("Hello from the dlopened library, the main executable says: %s\\n", message);
}


```


And compile it:


```text
$ wasixcc -fwasm-exceptions -fPIC -Wl,-shared libdlopened.c -o libdlopened.so


```


Now, let's create the main executable. Put this into` main.c` :


```text
#include <stdio.h>
#include <dlopen.h>


extern void needed_say_hello();


int main() {
printf("Hello from the main program!\\n");


// Call into the needed side module first
needed_say_hello();


// Now let's do our dlopen magic. First, load the
// actual library in:
void* dl_handle = dlopen("./libdlopened.so");
if (!dl_handle) {
printf("Failed to load library: %s\\n", dlerror());
}


// Now, we can look up the symbol
void (*dlopened_say_hello)(char*) = (void(*)(char*))dlsym(dl_handle, "dlopened_say_hello");
if (!dlopened_say_hello) {
printf("Failed to locate symbol: %s\\n", dlerror());
}


// Finally, we can call it
dlopened_say_hello("Dynamic Linking is cool!");


printf("All done!\\n");
}


```


And compile that as well:


```text
$ wasixcc -fwasm-exceptions -fPIC -Wl,-rpath,\$ORIGIN main.c libneeded.so -o main.wasm


```


There are a couple of interesting things here. One is` -Wl,-rpath` . As discussed above, the dynamic linker won't automatically search next to the binary requesting the library. We have two options:


- Mount the library into the WASM process's` /lib` folder.` /lib` is automatically searched by the dynamic linker.
- Alternatively, we can give the module a` RUNPATH` of` $ORIGIN` , which tells the linker also to look right next to the binary itself.


Both approaches work equally well. We used` RUNPATH` here mainly for demonstration purposes.


The second interesting thing is the` libneeded.so` argument; this tells` wasixcc` to dynamically link against the library. This will prevent the compilation from failing with an unresolved symbol and turn` extern void needed_say_hello();` into a DL import.


Let's look at the text representation of` main.wasm` :


```text
$ wasm-tools print main.wasm
(module
(@dylink.0
(mem-info (memory 266840 4) (table 41 0))
(needed "libneeded.so")
...
(runtime-path "$ORIGIN")
)
...
(import "env" "needed_say_hello" (func (;138;) (type 7)))
...
(import "env" "__memory_base" (global (;1;) i32))
(import "env" "__table_base" (global (;2;) i32))
...
(import "env" "memory" (memory (;0;) 5 65536 shared))
(import "env" "__indirect_function_table" (table (;0;) 41 funcref))
...
)


```


You can see some really interesting stuff here. First, there's the` needed` entry for` libneeded.so` , as well as the` RUNPATH` we specified when compiling. Then, there's an import for` needed_say_hello` . Looking further, you can see imports for all the stuff we discussed earlier, including` __memory_base` ,` __table_base` , the linear memory itself, and the indirect function table.


Finally, let's put it all together!


```text
$ wasmer run main.wasm --dir .
Hello from the main program!
Hello from the needed library!
Hello from the dlopened library, the main executable says: Dynamic Linking is cool!
All done!


```


Note the` --dir .` argument. This mounts the current directory into the WASIX application's file system and also changes` cwd` to point to it. This in turn allows the dynamic linker to locate and load both libraries successfully.


## What's next?


You can already try Python with native modules today. To do that, you'll need WASIX builds of the native dependencies you're using; a number of these can be found already in[the WASIX Python index](https://pythonindex.wasix.org/) , and[shipit](https://pypi.org/project/shipit-cli/)[can grab them for you automatically](https://pypi.org/project/shipit-cli/) .


```text
$ uvx shipit-cli ./my-python-app --wasmer --start


```


Another thing we're interested in exploring is the possibility of using Dynamic Linking to support JIT runtimes. Currently, WASIX versions of interpreted languages (WinterJS, Python, and PHP) operate in interpreter mode. With Dynamic Linking in the picture, we now have the ability to generate and load code at runtime. This should provide very nice performance benefits.
