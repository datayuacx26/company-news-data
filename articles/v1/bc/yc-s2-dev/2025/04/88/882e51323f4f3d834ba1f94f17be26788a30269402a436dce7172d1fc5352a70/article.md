---
schema_version: "1.0.0"
document_id: "882e51323f4f3d834ba1f94f17be26788a30269402a436dce7172d1fc5352a70"
company_key: "yc-s2-dev"
company: "s2.dev"
source_id: "yc-s2-dev-news-import-d1415bf25083"
canonical_url: "https://s2.dev/blog/dst"
published_at: "2025-04-02T00:00:00+00:00"
first_seen_at: "2026-07-22T12:29:40.978296+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:884eaac381c09fcb97b5d1db3d0fe70f0e1f8f9d99868680d271602364c86358"
---

# Deterministic simulation testing for async Rust

Deterministic simulation testing (DST) is a really powerful technique to gain confidence in a system by shaking out edge cases and reliably reproducing the bugs that turn up — and you thank the DST gods when they do, because that’s a bug a user did not have to encounter!


We knew DST was the way to go in building[S2](https://s2.dev/blog/intro) , inspired by the prior art of[FoundationDB](https://www.youtube.com/watch?v=fFSPwJFXVlw) and[TigerBeetle](https://www.youtube.com/watch?v=sC1B3d9C_sI) . But how could we practically implement it for our Rust codebase that uses Tokio as a runtime? My goal in this post is to share some of what we learned along the way.


Unit tests can be made plenty deterministic. What’s different here is we are trying to exercise *a lot* of different scenarios (like a property test) and with a whole-of-system mindset (like an end-to-end integration test).


The construction should be such that with each run, you *randomly* chart a path through the huge state space. Randomness and determinism may sound contradictory, but each choice flows from a single point – the random number generator (RNG) seed.


## The test subject


DST works best when you not only check invariants as a client, but also more granularly in the mainline code – a practice in defense-in-depth. Databases in particular should be liberal with assertions; far better to loudly crash when[ACID](https://en.wikipedia.org/wiki/ACID) ic properties are involved.


Incidentally, while some languages may treat assertions as something to be optimized away for production, I think Rust picked the right defaults here – it is a rare expensive check that we allow getting compiled away with` debug_assert!` .


But of course, we don’t *want* panics in production! To supercharge the long journey of building a robust distributed data system, the core of the actual test is synthesizing chaotic interactions over an extended period of simulated time. We run our DSTs on every PR, commit, and in thousands of nightly trials.


Beyond internal checks, the system must also maintain externally observable invariants that are best verified from a client's perspective as the simulation progresses, such as durability after a crash and API semantics around concurrent operations.


## Making it deterministic


What must be tamed to create a truly deterministic simulation?


- **Execution** – Single-threaded to eliminate scheduler noise
- **Entropy** – All RNGs should have a known seed
- **Time** – No physical clocks
- **I/O** – No dependency on any external IO with components not part of the simulation, and subject to all the same constraints


That’s a lot of variables to control for. Happily, we were able to leverage a lot of existing open source work done by the community!


Tokio does have first-class support for running with a single-threaded scheduler. Internally, its clock is abstracted, and can run “paused” for testing, where time only advances on calls to` sleep()` . By using` tokio::time::Instant` instead of` std::time::Instant` , you can ensure any measurement of elapsed time is aligned with this clock. The runtime also has an internal RNG used in making scheduling decisions such as picking a branch for` tokio::select!` – but this can be seeded.


Where does this leave us with regard to IO? Here we adopted the[Turmoil](https://github.com/tokio-rs/turmoil) project, which presumes Tokio as a runtime.


> Turmoil is a framework for testing distributed systems. It provides deterministic execution by running multiple concurrent hosts within a single thread. It introduces “hardship” into the system via changes in the simulated network. The network can be controlled manually or with a seeded rng.


Simulated networking is precisely what we needed, between logical 'hosts' in the same physical process. The ability to inject issues like latency and crashed processes is immensely useful for teasing out behavior under the unhappy paths.


Each of our networked services runs as one or more hosts. Turmoil provides counterparts to Tokio’s` TcpListener` /` TcpStream` which we use when in DST mode, behind a compile-time feature.


There are also external dependencies to consider, like metadata and object storage. The practice of coding to minimal interfaces allowed us to easily substitute the implementation. We have in-memory emulators that can be accessed over the simulated turmoil network, and also run as hosts in the simulation.


We managed to get simulations running – but as we dug deeper, they were not completely deterministic. We experienced failures in CI that we could not reproduce on our Macs, and in some cases even between runs on the same platform.


What were we failing to control for? Looking at` TRACE` -level logs, all kinds of differences stood out. Stuff like timestamps in HTTP packets 🤦.


Part of what makes Rust a productive language is the ecosystem of high-quality libraries. But every single one of them represents a possible source of multi-threading, reliance on an RNG, current system time, or external IO. Controlling the world is hard – and hence the space for a startup like[Antithesis](https://antithesis.com/) to make this easy. However, it felt like we were closing in, and we weren’t ready to fold.


It was easy enough to tell no threads were being spun up, all work happened on the main thread. We knew there were no unexpected network calls, all communications were strictly over the simulated network. But for time and randomness – turmoil’s approach was not comprehensive enough to address what arbitrary dependencies may be doing!


We also realized subtleties like Rust’s` HashMap` s being[randomized for DOS prevention](https://github.com/rust-fuzz/book/issues/35) . We could perhaps make sure our own app used a seeded` RandomState` for each hash map, but our dependencies..?


## Blending Turmoil and MadSim


This is where we started poking around[MadSim](https://github.com/madsim-rs/madsim) – the` Magical Deterministic Simulator for distributed systems in Rust` –[used in RisingWave](https://www.risingwave.com/blog/deterministic-simulation-a-new-era-of-distributed-system-testing/) . *What’s the magic?*


We liked the overall ergonomics of a turmoil-based DST, but a bit of madness seemed like the missing ingredient –` libc` symbol overrides to control time and entropy. You can checkout the MadSim-derived crate we just pushed to Github,[mad-turmoil](https://github.com/s2-streamstore/mad-turmoil?tab=readme-ov-file#setup) .


- The` rand` module overrides` getrandom` ,` getentropy` , and (Mac-only)` CCRandomGenerateBytes` . The new implementations utilize an RNG we statically initialize with` set_rng()` .
- The` time` module overrides` clock_gettime` using turmoil’s clock. This is scoped using a` SimClocksGuard` to avoid issues when the process is tearing down.


## Takeaways


**So, are we deterministic yet?** YES! To avoid repeating the scars of non-determinism, we also added a “meta test” in CI that reruns the same seed, and compares` TRACE` -level logs. Down to the last bytes on the wire, we have conformity. We can take a failing seed from CI, and easily reproduce it on our Macs.


**Was the effort worth it?** It goes without saying, distributed data systems are complex and production usage will always bring surprises. But there is a tremendous sense of relief in knowing we can mature our system in simulated time, and catch a lot of problems early.


We have a running doc of all kinds of gnarly issues our DSTs have helped us find, and the tally stands at 17 that were notable. Everything from nuances of our external APIs and internal protocols, to plain old deadlocks. Many of these would make for interesting stories, so watch out for future posts!
