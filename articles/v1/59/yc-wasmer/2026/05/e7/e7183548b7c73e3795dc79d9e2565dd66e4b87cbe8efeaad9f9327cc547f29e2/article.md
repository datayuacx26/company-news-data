---
schema_version: "1.0.0"
document_id: "e7183548b7c73e3795dc79d9e2565dd66e4b87cbe8efeaad9f9327cc547f29e2"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/wasmer-7"
published_at: "2026-05-20T13:33:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T22:12:59.555603+00:00"
content_hash: "sha256:75f7153337a5fdec46bc14bd4da4e9cd67defd17c36f5854f46187dde150aa10"
---

# Wasmer 7

Today we are incredibly excited to launch Wasmer 7.0.


To sum up the updates we have:


- New (experimental) **Async API**
- Support for **Exceptions in Cranelift**
- **RISC-V and multi-value** Support in Singlepass
- Full support for **Dynamic Linking** in WASIX
- Many **bugfixes and improvements**
- Quality of life improvements


Install the latest version of Wasmer with:


```text
curl https://get.wasmer.io -sSfL | sh


```


## New (experimental) Async API


Wasmer 7.0 introduces first-class support for async functions. The async API is currently available across the` singlepass` ,` cranelift` and` llvm` backends.


This capability enables full async support in Python on Wasmer, unlocking powerful libraries such as SQLAlchemy and many other ecosystem packages that previously could not run.


Read more about it here:[https://wasmer.io/posts/greenlet-support-python-wasm](https://wasmer.io/posts/greenlet-support-python-wasm)


## Support for Exceptions in Cranelift


We’ve upgraded Cranelift to the latest release and added full support for WebAssembly exceptions using its new exception-handling APIs. To make this work end-to-end, we integrated with the system’s standard` libunwind` library, filling in the missing pieces as the Cranelift compiler relies on its own unwinding implementation. By doing that, we use the same unwinding mechanism for all compilers using Exceptions.


## RISC-V and Multi-value Support in Singlepass


RISC-V continues to gain momentum across the industry, now even Intel is betting on it.


While previous Wasmer releases already supported RISC-V through LLVM and Cranelift, Singlepass was still missing support. With Wasmer 7.0, we close that gap by adding RISC-V support to Singlepass, and we go a step further by introducing the LLVM` RV32gc` target, significantly expanding our RISC-V coverage. We made sure the platforms are fully tested by our integration test suite.


## Support for Dynamic linking in WASIX


Historically, Python support in Wasmer was limited to the core interpreter, with many native libraries remaining unsupported (like numpy or pydantic). Wasmer 7.0 removes this limitation by introducing proper dynamic linking support in WASIX.


This unlocks a much broader ecosystem of Python packages and native modules. We have written a detailed blogpost about this here:[https://wasmer.io/posts/dynamic-linking-in-wasm-wasix](https://wasmer.io/posts/dynamic-linking-in-wasm-wasix)


## Many bug fixes and improvements


During the development of Wasmer 7.0, we merged more than 200 pull requests, 80 of which addressed bugs or longstanding limitations. We believe this makes Wasmer 7.0 the most stable release we’ve shipped to date.


Our project depends heavily on third-party crates, and we updated the vast majority of them to their latest major versions, including LLVM 21, our most critical dependency.


## Quality of life improvements


When building a module for the first time using the more heavy-duty LLVM compiler, we now display a compilation progress bar. In addition, when building large packages such as Python or PHP, we selectively disable optimizations for extremely large functions, resulting in significantly faster compile times (for example, Python builds drop from ~90s to ~10s).


# Getting Started with Wasmer 7.0


Ready to dive in? Here's how you can start exploring the new features:


- **Download Wasmer 7.0** : Get the latest version from our[official website](https://wasmer.io/) .
- **Update Your Projects** : Upgrade your existing Wasmer projects to leverage the new capabilities.
- **Explore the Documentation** : Visit our updated docs for detailed guides and tutorials.
- **Join the Community** : Connect with other developers on our Discord server and share your experiences.


## Looking Ahead


Wasmer 7.0 is a significant step forward in our mission to empower developers thanks to the exciting possibilities that WebAssembly brings to the table. We can't wait to see what you'll build next with Wasmer.


---


**Stay Updated**


- **Website** :[wasmer.io](https://wasmer.io/)
- **GitHub** :[github.com/wasmerio/wasmer](https://github.com/wasmerio/wasmer)
- **Twitter** :[@wasmerio](https://twitter.com/wasmerio)
- **Discord** : Join our community


Thank you everyone for being part of the Wasmer journey!
