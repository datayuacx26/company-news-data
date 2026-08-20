---
schema_version: "1.0.0"
document_id: "df101fa7b3f159c6519f51c03df803b1e64c08d4165d41287c797d316e4f25e6"
company_key: "stmicroelectronics-n-v-common-stock"
company: "STMicroelectronics N.V."
source_id: "stmicroelectronics-n-v-common-stock-rss-811b8a53447b"
canonical_url: "https://blog.st.com/rust-mems-drivers/"
published_at: "2026-07-14T13:00:00+00:00"
first_seen_at: "2026-07-24T02:26:40.894294+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:ff684ee118bc8d5288264a8f4963fb79c2958865f98029d6d21d5aef2c9e18f7"
---

# Rust MEMS drivers: 3 reasons to try and adopt our new sensor drivers written in Rust

**ST is introducing an initiative to provide[Rust drivers for many of our sensors](https://github.com/STMicroelectronics/st-mems-rust-drivers?ecmp=tt50175_gl_link_jul2026) , opening a new avenue for those looking to adopt this programming language in their embedded systems development.** Obviously, we will continue to ship and maintain our[C-based ecosystem](https://github.com/STMicroelectronics/STMems_Standard_C_drivers?ecmp=tt50175_gl_link_jul2026) . However, we are also aware of the growing popularity of the Rust programming language, the benefits it offers for many projects, and that building a robust, platform-agnostic alternative in another programming language takes significant time and investment. It’s for those reasons that we have started porting drivers piecemeal and are now advertising our presence on[crates.io](https://crates.io/users/avisconti) and showcasing projects like[Embassy STM32, which provides a hardware abstraction layer in Rust for our MCUs](https://crates.io/crates/embassy-stm32?ecmp=tt50175_gl_link_jul2026) .


## Reason #1: Safety


The logo of the Rust programming language


One of the most popular aspects of Rust is its inherent code safety compared to other languages, including C and C++. In essence, that means the Rust compiler will forbid certain operations that could jeopardize memory safety. **Being memory-safe means that the Rust compiler will block any memory access if it hasn’t been explicitly allocated or has already been deallocated.** This is a significant departure from C and C++, where developers have much more freedom in memory management. However, it also means that bad code can lead to issues such as buffer overflows, memory leaks, and “use-after-free” or dangling pointers. All of these can result in data corruption or even security vulnerabilities.


The reason Rust is safer is that it implements Resource acquisition is initialization (RAII), a technique that automatically and immediately allocates resources when an object is created and releases them when the object is deleted. The compiler allocates these resources to an owner and can then transfer ownership or allow another owner to borrow it if needed. As a result, Rust handles resource management and does not need a garbage collector to manage memory allocations, unlike C++, another language that implements RAII but still requires developers to manage memory themselves. In practice, the absence of a garbage collector reduces overhead, but it also makes Rust code much less lenient.


While this blog post can’t begin to scratch the surface of all the ways Rust differs from C and C++ (we won’t even go into type safety), the point above illustrates an important reason why developers are moving to Rust and why ST has already tried to provide alternatives to its sensor drivers. The Rust learning curve is high. Writing safe code that compiles is challenging because there are so many guardrails to ensure code safety. **However, it also means that once the code compiles, developers have far greater assurance of safety and security.** And for developers new to Rust, having open source drivers like the ones ST provides means they can study our implementations and learn from our experience.


## Reason #2: Practicality, portability, and performance (sort of)


LSM6DSV320X is one of our sensors that comes with Rust drivers


Another reason developers are adopting Rust for their embedded projects is the way it handles interfaces and bus operations. **Unlike C, which traditionally uses a pointer-based bus abstraction, Rust uses “Traits”, which define interfaces as types and then provide a flexible abstraction over them.** Through ecosystem standards such as embedded-HAL, interfaces for buses like I2C and SPI can be implemented consistently across platforms, helping developers gain expertise. In fact, our code already uses APIs aligned with our original C drivers to ease the learning curve, and we adopted consistent naming to smooth the transition to our Rust alternative.


One misconception we often encounter is that Rust is inherently “faster” than C or C++. The truth is far more nuanced.[Depending on the application](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/rust.html?ecmp=tt50175_gl_link_jul2026) , Rust may be slower than, on par with, or faster than C. **However, in many instances, Rust forces teams to rewrite their application, which naturally leads to refactoring, dropping legacy code, or creating new, optimized processes.** The absence of a garbage collector in Rust, or its modern compiler enhancements, certainly helps, but embedded system developers should not rush in thinking it will magically provide significant runtime improvements. However, it will offer significantly safer, often more flexible code, which is why we are providing Rust alternatives for our C sensor drivers.


## Reason #3: The Rust community


The NUCLEO-F401RE


The[discussions in our GitHub repository](https://github.com/STMicroelectronics/st-mems-rust-drivers/issues/1?ecmp=tt50175_gl_link_jul2026) for this project show a thriving community, which is an important factor for many developers. It’s not just key industry players like Meta, Google, or Microsoft that have publicly pledged to adopt the new programming language. **Many smaller teams are investing in the new language because acquiring fluency takes time, and falling behind could lead to significant technical debt that would benefit a competitor.** We know that adoption is still in the early phase. This is why we are providing Rust sensor drivers now. ST wants to support those who want to be first in this transition, which seems to be shaping the future of embedded systems development.


The best way to get started is to install Rust using[rustup](https://rust-lang.org/tools/install/) , then, as developers build their project, grab one of our[packages on Crates](https://crates.io/users/avisconti?ecmp=tt50175_gl_link_jul2026) . **Our GitHub Page also walks users through the download and integration process.** We offer examples in Cargo to help teams get results faster. And while the drivers themselves allow engineers to be MCU-agnostic, the examples we use are written for STM32 microcontrollers and primarily use the[NUCLEO-F401RE](https://www.st.com/en/evaluation-tools/nucleo-f401re.html?ecmp=tt50175_gl_link_jul2026) development board. Current Rust drivers focus on I2C and SPI interfaces, as these are by far the most commonly used, but additional support, including I3C, is underway, and we will update this blog post as we release major updates.


- [Check out the ST sensor drivers in Rust](https://github.com/STMicroelectronics/st-mems-rust-drivers?ecmp=tt50175_gl_link_jul2026)
