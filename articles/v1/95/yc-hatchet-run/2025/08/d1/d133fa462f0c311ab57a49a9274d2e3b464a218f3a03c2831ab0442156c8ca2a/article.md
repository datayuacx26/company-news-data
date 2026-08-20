---
schema_version: "1.0.0"
document_id: "d133fa462f0c311ab57a49a9274d2e3b464a218f3a03c2831ab0442156c8ca2a"
company_key: "yc-hatchet-run"
company: "Hatchet"
source_id: "yc-hatchet-run-rss-3f16f06bd764"
canonical_url: "https://hatchet.run/blog/automated-documentation"
published_at: "2025-08-26T00:00:00+00:00"
first_seen_at: "2026-08-09T22:57:06.374584+00:00"
fetched_at: "2026-08-09T22:57:07.291413+00:00"
content_hash: "sha256:fd085d4974d9cada37bb4b2461ccdfd4e208a5372d66257ffc3c4f3526310b4f"
---

# How We Solved Multi-Language SDK Documentation Chaos without LLMs

Gabriel Ruttner


Founder


Hatchet


Towards the beginning of this year, our documentation was 💩


.


Not intentionally, of course. But when you’re shipping SDKs for Python, TypeScript, and Go that change daily, keeping documentation examples accurate becomes an impossible game of whack-a-mole. We’d fix the Python example, break the TypeScript one, update the Go snippet, and suddenly the Python version was using deprecated APIs.


At Hatchet, we face a challenge that many dev tool platform companies know all too well: keeping documentation in sync with rapidly evolving SDKs across multiple programming languages. But our problem was uniquely complex. Since our sdks are responsible for running code, we don’t just build simple API wrappers so generating from an OpenAPI spec wasn’t an option. They are native libraries complete with type safety, advanced workflows, and sophisticated error handling.


Like many engineering teams in 2025, our first instinct was to throw LLMs at the problem. The promise was compelling: AI could automatically update documentation when code changes, translate examples between languages, and even generate new tutorials. But as we experimented with various approaches, we kept hitting the same fundamental issues. LLMs would generate code that looked correct but wouldn’t run, introduce subtle bugs in complex workflows, or make incorrect assumptions about our SDKs’ behavior. When your users are copying code directly into production systems, “mostly correct” isn’t good enough—you need examples that are guaranteed to work.


Here’s how we solved it with a system that keeps our documentation examples as living code that’s actually tested and maintained as part of our SDK development workflow.


### The Problem: Native SDKs Don’t Play Nice with Static Docs


Most API companies can get away with simple code snippets in their documentation, or generating from doc strings or OpenAPI specs.


*Note: We still generate traditional API reference documentation from doc strings for comprehensive method and class documentation—this system is supplemental to that, focusing specifically on user guide examples and tutorials where context and real-world usage patterns matter most.*


Loading syntax highlighting...


But our SDKs aren’t thin HTTP clients with simple CRUD operations, instead they’re comprehensive workflow orchestration libraries with features like:


- **Typed workflow definitions** with full generic support
- **Durable execution patterns** with automatic retries and error recovery
- **Complex conditional logic** with branching and parallel execution
- **Event-driven architectures** with filtering and scoping
- **Worker management** with concurrency controls and resource allocation


A simple example from our Python SDK demonstrates the complexity:


Loading syntax highlighting...


The developer needs to understand how we recommend using the SDK to avoid common pitfalls….And this is one of our *simple* examples… its possible to mix-and-match features to build complex orchestration.


Multiply this complexity across Python’s multiple concurrency patterns, TypeScript’s structural types, and Go’s idiomatic nuances, and you start to see the problem. Each language has its own quirks and features for the same underlying concepts. A retry policy in Python uses decorators, in TypeScript it’s a configuration object, and in Go it’s a functional option. Every time we add a feature or fix a bug, we need to update examples in three (and soon to be more) different languages while ensuring they all demonstrate the same concepts correctly.


### The Traditional Approach (And Why It Failed Us)


Like most teams, we started with the obvious approach: write example code directly in our documentation files. When we needed a Python example, we’d write some Python. TypeScript example? More TypeScript. Each example lived in a markdown file, carefully crafted to be clear and concise.


This worked… for about two weeks.


The first problem emerged when we refactored our client initialization. Suddenly, every single example in our docs was broken because they all used the old constructor. So we spent an hour of searching and updating each example by hand.


The second problem was more subtle but equally destructive: drift. When we added new features, we’d update the examples for one language but forget the others. Or we’d fix a bug in the TypeScript example but not notice that the Python version had the same issue. Worse yet, we’d sometimes update examples with code that *looked* right but had never actually been run.


We needed a fundamentally different approach. The examples had to be real, runnable code that evolved with our SDKs automatically. They needed to be linted, type-checked, and tested as part of our development workflow. And they had to work seamlessly across three different programming languages without requiring our team to become experts in documentation tooling.


### The Solution: Living Code with Comment Annotation


The idea that changed everything was simple: what if documentation examples weren’t separate files at all, but lived directly in our SDK repositories as real, working code?


Instead of maintaining separate documentation examples, our examples live directly in the SDK repositories where they belong:


Loading syntax highlighting...


These are fully functional code examples that get linted, type-checked, and tested as part of our CI pipeline.


But that created a new challenge: how do you extract clean, focused snippets from complex, runnable applications for documentation? A complete example file might be 200 lines long, but you only want to show the 10 lines that demonstrate task declaration.


#### Simple Comment Markup for Extraction


We developed a lightweight comment-based markup system that lets us annotate code blocks for extraction:


Loading syntax highlighting...


The markup uses each language’s native comment syntax, so it feels natural to developers. A block starts with` > Block Title` and ends with` !!` .


The magic happens during our documentation build process. A Python script scans every example file across all three SDK repositories, parses the comment markup, and extracts the annotated code blocks. Each extracted snippet gets packaged with metadata: the original source location, a GitHub URL for context, proper language tagging, and normalized indentation.


Loading syntax highlighting...


This means engineers get IDE support when working with code examples. Instead of copying and pasting strings, they import typed objects with autocompletion and compile-time verification that the examples they’re referencing actually exist.


#### Simple Code Parsing


While the developer experience is simple, there’s some interesting technical implementation details under the hood. The core challenge is parsing three different programming languages reliably while handling edge cases like nested comments, complex string literals, and varying indentation styles.


Our parser starts with a simple but flexible design:


Loading syntax highlighting...


The heavy lifting happens in the parsing logic, where regex patterns extract comment blocks while preserving the original code structure:


Loading syntax highlighting...


> *Note: This is simplified pseudo-code for clarity. The actual implementation includes additional logic for file reading, path handling, and fallback cases when no annotated blocks are found.*


The trickiest part is handling indentation correctly. When you extract a code block from the middle of a function, it might be indented 8 spaces, but you want it to display starting at the left margin in documentation. Our` dedent_code` function automatically finds the minimum indentation level and normalizes everything:


Loading syntax highlighting...


The whole system integrates seamlessly into our development workflow. When a developer commits changes to any SDK example, GitHub Actions triggers the generation script. The parser runs, extracts all the annotated snippets, and commits the updated JSON back to our documentation repository. Our docs site rebuilds automatically, and changes are live within minutes.


#### A Real Example: From Code to Docs


To make this concrete, let’s trace how a single workflow example file would written and then used in our documentation. For context, this is a trivial task that accepts a number as input and multiplies it by ten.


**Source (Python):**


Loading syntax highlighting...


**Generated Snippet Object:**


Loading syntax highlighting...


> *Note: the code in this example is truncated for brevity.*


**Usage in Documentation:**


Loading syntax highlighting...


> *Note: This demonstrates real MDX usage where snippets are embedded directly into documentation content with explanatory text, exactly as they appear in our live documentation.*


### The Results: Documentation That Actually Works


This system has transformed how we handle documentation at Hatchet:


**✅ Our examples always work.** When a user copies code from our documentation and pastes it into their project, it compiles and runs correctly. This isn’t wishful thinking—it’s guaranteed by our CI pipeline. If an example is broken, our builds fail, and someone fixes it before it ever reaches users.


**✅ Consistency across languages is automatic.** When we add a new feature like distributed tracing, we implement it in all three SDKs and annotate examples in each. The documentation automatically reflects the idiomatic patterns for Python decorators, TypeScript configuration objects, and Go functional options, without any manual coordination.


**✅ Developer velocity has increased dramatically for SDK changes.** Our team no longer spends time hunting through markdown files to update examples. When they write new SDK features, they just annotate the natural examples they’d write anyway, and those examples flow into documentation automatically.


**✅ User onboarding is smooth.** We haven’t had a single support ticket about broken documentation examples since launching this system. Users can trust that if they follow our guides step-by-step, everything will work.


**✅ Context is always available.** Every code snippet automatically includes a GitHub link to the complete, runnable example. Users can see the full context, explore related code, and understand how snippets fit into larger applications without hunting through repositories or guessing about missing imports and setup code.


### What We Learned Along the Way


Building this system taught us several important lessons about sustainable documentation practices.


Simple comment-based markup was the right choice. We experimented with an emoji markdown language to avoid collisions with existing comments but that turned out to be a pain to use and unnecessary. Using each language’s native comment syntax and simple uncommon characters meant zero learning curve and zero additional tooling requirements.


Strong typing catches documentation bugs. Having TypeScript definitions for our snippet objects caught dozens of cases where documentation was referencing examples that didn’t exist or had been renamed.


Performance matters for developer experience. Our initial parser took 2 minutes to run, which made the development feedback loop painfully slow. Optimizing it down to 30 seconds made the whole system much more pleasant to use.


### The Future of Hatchet’s Documentation


While our system keeps code examples accurate, explanatory text can still drift when APIs change. Our next step: use LLMs to suggest prose updates when our parser detects significant snippet changes.


The approach is simple—when code changes, an LLM suggests corresponding text updates for human review. Unlike our earlier experiments, the AI never touches actual code, only the prose explaining it.


Goal: zero-drift guarantee for explanatory content, just like our code examples.


---


*Want to see this system in action? Check out[Hatchet’s documentation](https://docs.hatchet.run/) to see how these living examples integrate into our developer experience, or explore the[open-source implementation](https://github.com/hatchet-dev/hatchet/blob/main/frontend/snippets/generate.py) to adapt this approach for your own projects.*


---


**Does building killer developer tools sound interesting? Check out our[careers](https://www.ycombinator.com/companies/hatchet-run/jobs) page to see if Hatchet is a good fit for you. We’re always looking for talented engineers to join our team.**
