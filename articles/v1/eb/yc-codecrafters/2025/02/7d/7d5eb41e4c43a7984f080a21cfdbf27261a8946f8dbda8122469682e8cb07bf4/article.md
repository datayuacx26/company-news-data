---
schema_version: "1.0.0"
document_id: "7d5eb41e4c43a7984f080a21cfdbf27261a8946f8dbda8122469682e8cb07bf4"
company_key: "yc-codecrafters"
company: "CodeCrafters"
source_id: "yc-codecrafters-news-import-01a1b304c75c"
canonical_url: "https://codecrafters.io/blog/rust-projects"
published_at: "2025-02-18T00:00:00+00:00"
first_seen_at: "2026-07-21T14:04:10.550467+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:78b99cd2444a9ff3ba402e153ee432df8c397147474268f2b7ebd93635c9c5a4"
---

# 15 Rust Projects To Sharpen Your Skills

You have read through[Rust basics](https://fasterthanli.me/articles/a-half-hour-to-learn-rust) and done some[exercises](https://github.com/rust-lang/rustlings) .


Now you want to build some actual projects and get hands-on experience.


Ferris the crab is the unofficial mascot of the Rust community.


Here's a list of 15 curated projects, ordered by difficulty, to help you:


- apply Rust's core concepts in real-world scenarios
- get comfortable with ownership, borrowing and lifetimes
- optimize performance with zero-cost abstractions and concurrency
- build everything from auth servers to browser engines


Pick a project, start building, and deepen your Rust expertise.


## Beginner Rust Projects


### Project 1: **CLI To-Do List App**


A terminal app to track tasks without cmd/alt-tabbing is perfect for developers who live in the terminal.


- Add, delete, and track tasks
- Mark them as done or TBD
- Save them to a file so they persist


Taskwarrior is a popular open source project doing this. Check it out at[taskwarrior.org](https://taskwarrior.org/)


Add features to make it more useful.


- Create tasks with a deadline
- Tasks that repeat on a set interval


You'll find that Rust forces you to handle potential issues like missing files or invalid input, ensuring reliable code.


Take a look at this[app](https://github.com/DashikiBulbasaur/chartodo) .


Skills you'll pick up - File I/O, error handling, testing, iterators


### Project 2: Web Scraper for E-Commerce


Web scrapers can monitor price changes, tickets and limited drops so you get notified before everyone else.


Try to extract product data from this[dummy e-commerce site](https://www.scrapingcourse.com/ecommerce/) .


- Crawl all pages
- Parse product details
- Save data


Amazon has a dedicated team (Amazon CMT) tracking competitor prices and availability.


On real sites, you'll face problems like client-side rendering and cookie walls.


You can improve your app to scrape Airbnb using a webdriver with[thirtyfour](https://docs.rs/thirtyfour/latest/thirtyfour/) .


While scraping speed is independent of programming language, Rust's iterators and functional paradigm simplify data searching and make it intuitive.


Check out this[source code](https://github.com/itehax/rust-scraping?tab=readme-ov-file) .


Skills you'll pick up - HTML parsing, web scraping, functional programming


### Project 3: File Compression


Compression is everywhere from email storage to video streaming. A good way to understand the fundamentals is by compressing a text file.


Create a program to -


- compress
- extract
- benchmark performance


You can use any effective compression algorithm like[Huffman Coding](https://www.javatpoint.com/huffman-coding-algorithm) .


You can also use Lempel-Ziv-Welch, Arithmetic coding, or Burrows-Wheeler Transform.


Rust's bit-level control and parallelism will let you optimize beyond traditional tools. Use a[large .txt file](https://www.kaggle.com/datasets/mikeortman/wikipedia-sentences) and measure how your program performs.


This Rust[code](https://github.com/Lakret/huffman-rs) outperforms zip in both compression rate and speed.


Skills you'll pick up – Lossless compression, bit control, parallel computation


### Project 4: Real-Time Chat Room


Create a chat room to hide your conversations from big tech servers and the powers that be. (For legal purposes, this is a joke.)


- multiple users per room
- real-time message updates
- simple frontend with HTML & JavaScript


Discord moved parts of its chat backend from Go to Rust for better efficiency.


You can now add security.


- authentication
- end-to-end encryption


Rust's async model and lightweight concurrency make it great for building chat apps.


Check out this neat[chat app built in Rust](https://github.com/LoipesMas/accord) .


Skills you'll pick up – Networking, websockets, async programming, concurrency


### Project 5: Search Engine


Apps today let you search through collections of text documents like chats, news articles, and emails. Build a search engine to understand the basics.


You can start with a program that ranks text documents against a query.


- inverted index : tokenization with stemming, lemmatization, etc.
- ranking algorithm : relevance metrics like BM25, window score, etc.


Add spelling correction and predictive search to improve search experience.


Tantivy is a full-text search engine library inspired by Apache Lucene and written in Rust


Here's a[sample dataset](https://www.kaggle.com/datasets/ffatty/plain-text-wikipedia-simpleenglish) of Wikipedia articles you can work with.


Add a regex mode with[Rust's regex engine](https://github.com/rust-lang/regex) which uses finite automata and SIMD to make it blazing fast.[ripgrep](https://github.com/BurntSushi/ripgrep?tab=readme-ov-file) uses it to outperform grep.


Check out this[implementation](https://github.com/tomfran/search-rs) of a search engine in Rust.


Skills you'll pick up – Text processing, search algorithms, query optimization


## Intermediate Rust Projects


### Project 6: API Server for Dog Walking Bookings


Building an API is a rite of passage in any language. Time to do it in Rust.


For example, you can build a REST API to manage dog walking bookings.


- Add owners and their dogs
- Schedule and manage bookings
- List all upcoming bookings
- Assign a walker to a booking
- Cancel a booking


Use a NoSQL database like MongoDB to store the data.


GraphQL is better at fetching data, but the simplicity and ease of caching keep REST relevant.


Make it production-ready -


- stress-test your API and optimize database queries
- use a constraint-solving algorithm (e.g., Z3) to auto-assign walkers
- implement surge pricing based on demand.


Rust's async runtime and type safety make it ideal for building fast, reliable API servers.


With Tokio's async runtime, you can handle many requests concurrently without blocking threads.


Check out this[guide](https://www.bretcameron.com/blog/how-to-build-an-api-server-with-rust) .


Skills you'll pick up – API dev, async programming, MongoDB, web frameworks


### Project 7: Authentication System with Sessions


Build your own user authentication and see what it entails.


Here's what you can implement -


- User signup and login
- Secure password storage
- Session management with cookies
- Authentication middleware for protected routes
- Simple frontend


JWTs will be broken by quantum computers, but passwords and session tokens will hold up.


You can also try -


- authentication with JSON Web Tokens (JWT) instead of sessions
- adding multi-factor authentication


The debate between sessions vs JWTs for auth has been going on for years -[this video](https://www.youtube.com/watch?v=fyTxwIa-1U0) breaks it down.


Rust's ownership model eliminates some common security flaws like use-after-free and buffer overflows.


Its lifetimes and borrow checker enable you to manage sessions without worrying about memory leaks.


Check out this[guide](https://www.shuttle.dev/blog/2022/08/11/authentication-tutorial) with[source code](https://github.com/kaleidawave/axum-shuttle-postgres-authentication-demo) . For JWT auth,[check this](https://blog.logrocket.com/jwt-authentication-in-rust/) .


Skills you'll pick up – Auth, password hashing, sessions, middleware


### Project 8: Version Control System (Git)


Reinvent the wheel and build a lightweight Git clone that tracks changes in source code. (because understanding Git internals is worth it)


To do this, you must read about[.git directories](https://blog.meain.io/2023/what-is-in-dot-git/) ,[blobs, trees and commits](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) .


Few features you can start with-


- initialize a .git directory
- read and write blob objects
- create tree structures
- commit changes


[app.codecrafters.io](https://app.codecrafters.io/) gives you everything you need to start building this project yourself.


You can then implement[git clone](https://stefan.saasen.me/articles/git-clone-in-haskell-from-the-bottom-up/#format_of_the_delta_representation) , which is more complex.


Rust's zero-cost abstractions let you manage Git's object storage efficiently, with no runtime overhead.


Take a look at this[source code](https://github.com/jonhoo/codecrafters-git-rust) .


Skills you'll pick up – Git internals, object storage, hashing, data structures


### Project 9: BitTorrent Client


Decentralized file sharing powers everything from Linux ISOs to, well… you know. Build a BitTorrent client to see how it all works.


You first need to understand the[BitTorrent protocol](https://www.bittorrent.org/beps/bep_0003.html) .


It might be a good idea to start with single-file torrents.


You'll need to implement -


- .torrent parsing
- peer discovery and connection
- file download and verification


BitTorrent's tit-for-tat algorithm rewards peers who upload more with faster downloads.


You can use this[sample single-file torrent](https://github.com/codecrafters-io/build-your-own-bittorrent/blob/main/compiled_starters/rust/sample.torrent) for a txt file.


Once you can download it, add support for -


- magnet links
- multi-file torrents
- seeding


Rust ensures memory safety without a garbage collector, enabling low latency and reliable performance while handling peers concurrently.


[Watch Jon Hoo build a client in Rust](https://www.youtube.com/watch?v=jf_ddGnum_4) . Here's the[source code](https://github.com/jonhoo/codecrafters-bittorrent-rust) .


Skills you'll pick up – P2P networking, hashing, low-level data transmission


### Project 10: Video Call App


Build an app for video calls and learn how to leverage WASM.


Here's what you can implement -


- Web-based peer-to-peer calls
- Signaling server using WebSockets
- Rust compiled to WASM for browser compatibility


You can use WebRTC to handle video and audio streaming.


WebRTC enables p2p connections, but TURN servers are often needed to bypass NATs and firewalls.


The frontend can be a simple HTML page with some buttons, and video elements.


You can also add features like -


- Group calls
- Screen sharing
- End-to-end encryption


Rust-to-WASM helps integrate Rust with projects written in other languages. Rust can then replace slow parts with faster, memory-efficient code.


Check out this[source code](https://github.com/Charles-Schleich/WebRTC-in-Rust) .


Skills you'll pick up – WebRTC, Rust-to-WASM, async networking, peer connections


## Advanced Rust Projects


### Project 11: Wordle Solver


Build a program that solves Wordle, with the help of information theory.


This challenge comes from the[Jon Gjengset YouTube channel](https://www.youtube.com/@jonhoo) .


To get started - watch this[video](https://www.youtube.com/watch?v=v68zYyaEmEA) to understand the algorithm. get the Wordle dictionary and list of Wordle answers. download the[Google 1-grams dataset](https://storage.googleapis.com/books/ngrams/books/20200217/eng/eng-1-ngrams_exports.html) to get word occurrences.


Josh Wardle, creator of Wordle, also built the viral r/thebutton social experiment.


Once you are ready,


- create the solver
- benchmark speed
- optimize the solver


Benchmark performance using[hyperfine](https://github.com/sharkdp/hyperfine) and improve it as much as possible.


Rust excels at high-performance tasks, and this project gives you hands-on experience optimizing computations over a large search space.


Watch[Jon's video](https://youtu.be/doFowk4xj7Q?t=1734) as he optimizes the solver, and try to outperform[his code](https://github.com/jonhoo/roget) .


Skills you'll pick up – Performance optimization, probability, benchmarking


### Project 12: SQL database Engine


Writing SQL is easy. But writing the engine that runs it is not.


Build a simple SQL engine that reads .db files and executes queries.


You can start here -


- Read and parse SQLite database files
- Execute basic SELECT query with WHERE clause
- Use indexes for faster data retrieval


SQL has been around for 50 years, showing how great abstractions can stand the test of time.


You need to understand[B-trees](https://medium.com/basecs/busying-oneself-with-b-trees-78bbf10522e7) and[how SQLite stores data on disk](https://www.sqlite.org/fileformat.html#b_tree_pages) .


Try running this query on[companies.db](https://github.com/codecrafters-io/sample-sqlite-databases/blob/master/companies.db) with your engine in less than 4 seconds.


```text
$ ./your_program.sh companies.db "SELECT id, name FROM companies WHERE country = 'eritrea'"


121311|unilink s.c.
2102438|orange asmara it solutions
5729848|zara mining share company
6634629|asmara rental


```


You can also extend support for more clauses and query optimization.


Rust's memmap2 enables efficient SQLite page access with direct RAM-speed B-tree traversal instead of reading the entire file into memory.


Check out[Brooks Patton's implementation](https://www.youtube.com/watch?v=gNZvSrvnPl0) .


Skills you'll pick up – B-trees, SQL engines, SQLite internals, indexing


### Project 13: Lox Interpreter


Build an interpreter for Lox, a simple scripting language, and understand how languages come to life.


The project comes from[Crafting Interpreters](https://craftinginterpreters.com/) by Robert Nystrom.


Before starting, read the welcome section of the book here -


- [Introduction](https://craftinginterpreters.com/introduction.html) (chapter 1)
- [A Map of the Territory](https://craftinginterpreters.com/a-map-of-the-territory.html) (chapter 2)
- [The Lox Language](https://craftinginterpreters.com/the-lox-language.html) (chapter 3)


Building an interpreter involves complex engineering and some novel concepts.


Here is an overview of the steps -


- [Scan tokens](https://craftinginterpreters.com/scanning.html)
- [Represent code](https://craftinginterpreters.com/representing-code.html)
- [Parse expressions with an AST](https://craftinginterpreters.com/parsing-expressions.html)
- [Evaluate expressions](https://craftinginterpreters.com/evaluating-expressions.html)
- [Handle statements and state](https://craftinginterpreters.com/statements-and-state.html)
- [Support implicit declarations](https://craftinginterpreters.com/statements-and-state.html#design-note)
- [Add control flow](https://craftinginterpreters.com/control-flow.html)
- [Support user-defined functions and execution](https://craftinginterpreters.com/functions.html)


Once it is running, try optimizing it or adding new features-


- Bytecode compilation for faster execution
- Automatic memory management
- Built-in functions for usability


Rust's enum + match approach makes it easier to define and process syntax tree nodes efficiently.


It manages interpreter state without GC, making you consider lifetimes, references, and memory safety in ways uncommon to dynamic languages.


Jon Hoo built this over an 8-hour[YouTube stream](https://www.youtube.com/watch?v=mNOLaw-_Buc) . Here's the[code](https://github.com/jonhoo/lox) .


Skills you'll pick up – Tokenization, ASTs, tree-walk interpreters


### Project 14: Browser Engine


Build a very basic web browser from scratch. No Chromium, Gecko, or WebKit.


- Parse and render simple HTML & CSS
- No JavaScript, animations, or advanced layouts
- Single-threaded sync page loading. Okay to be slow.


Building an engine like Chromium yourself is nearly impossible due to the complexity involved.


As the first goal, you can match how a real browser renders this[HTML file](https://github.com/jonhoo/codecrafters-bittorrent-rust) .


You can then push further -


- Support multiple pages
- Add a caching layer
- Load web pages over HTTP instead of just local files
- Offload rendering to a separate thread for performance


Rust's memory safety and performance make it a good choice for browser engines. Servo, Mozilla's research browser, is written in Rust.


Josh On Design's[blog series](https://joshondesign.com/tags/browser) walks through building a Rust browser engine.


Skills you'll pick up – Parsing, rendering, layout algorithms, systems programming


### Project 15: NES Emulator


Run the first Super Mario Bros on it's original hardware that you emulate in software.


To do this, you'll have to create an NES emulator -


- Implement the 6502 CPU with instruction handling
- Support iNES format ROMs and mapper 0 (NROM)
- Render graphics with the PPU (without scrolling first)
- Add scrolling for full game support


[This NES ebook](https://bugzmanov.github.io/nes_ebook/chapter_1.html) can help you implement all the steps.


Today emulators like ShadPS4 for PS4 and Xenia for Xbox 360 can run select modern games.


Use these[ROMs](https://www.nesdev.org/wiki/Emulator_tests) to test and debug your code.


Start by running Donkey Kong as your first goal.


Then add support for Super Mario Bros and any other NES games you want.


You can complete your emulator by adding -


- APU (audio processing unit)
- more mappers to support other games


Rust lets you write high-level, expressive code (like pattern matching for CPU opcodes or iterators for memory access) without sacrificing performance.


The compiler optimizes them, making your emulator as fast as one written in C.


At the same time, Rust's borrow checker ensures safe memory access, preventing common emulator bugs like use-after-free or buffer overflows that are common in C/C++.


Check out this[NES emulator written in Rust](https://github.com/starrhorne/nes-rust) .


Skills you'll pick up – CPU emulation, memory management, binary parsing, graphics rendering, low-level systems programming.


## Conclusion


[Rust is the most admired programming language among developers today.](https://survey.stackoverflow.co/2024/technology/#admired-and-desired)


Stack Overflow's survey on most admired languages


Tech leaders are choosing Rust to build and maintain critical systems:


- [Google's shift to Rust has cut Android memory vulnerabilities by 68%](https://thehackernews.com/2024/09/googles-shift-to-rust-programming-cuts.html) .
- [Discord switched from Go to Rust for better performance and security](https://discord.com/blog/why-discord-is-switching-from-go-to-rust) .
- [Cloudflare chose Rust for 1.1.1.1 due to its elegance & lightweight design](https://blog.cloudflare.com/big-pineapple-intro/) .


At the same time, the Rust community uses it for projects like :


- operating systems
- game development engines
- embedded systems


If you are a programmer, mastering Rust now makes a lot of sense.


By completing these projects, you'll gain the skills to build everything from high-performance APIs to game engines, compilers, and creative tools.


What's next?


1. Join the Rust community -[User Forum](https://users.rust-lang.org/) ,[Reddit](https://www.reddit.com/r/rust/) ,[Discord](https://discord.com/invite/rust-lang)
2. Contribute to open source to collaborate with experienced developers.
3. Start using Rust for work, open-source or personal projects.


Keep building, keep exploring, and most importantly, enjoy the journey!
