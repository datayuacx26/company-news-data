---
schema_version: "1.0.0"
document_id: "1cc11ecd24d9c98264a6284e872366c3acf9709f64276a829ef81a8e3f132f79"
company_key: "nu-holdings-ltd-class-a-ordinary-shares"
company: "Nu Holdings Ltd."
source_id: "nu-holdings-ltd-class-a-ordinary-shares-rss-32a3c94cba04"
canonical_url: "https://building.nubank.com/12-years-of-component-a-decade-of-interactive-development/"
published_at: "2025-12-22T18:35:22+00:00"
first_seen_at: "2026-07-20T04:35:52.604812+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:049503fc7c9a88b96e5e1b499a32b2a86d7ebd057fc57a0ceb771670d652030b"
---

# 12 years of Component: A decade of interactive development

A little over a month ago, Nubank’s office in Vila Leopoldina, São Paulo became the meeting point for the Clojure community across South America and beyond. The second edition of Clojure South brought together more than 200 developers, researchers, and language enthusiasts for two days of knowledge sharing, connection, and celebration of functional programming.


The event reinforced Brazil’s role as one of the most vibrant hubs for the Clojure community, and Nubank’s role as an active focal point for technology communities.


It was in this atmosphere of shared enthusiasm that Alessandra Sierra, Principal Software Engineer at Nubank, opened the conference with her talk **“12 Years of Component”** , reflecting not only on the history of one of Clojure’s most influential libraries, but on her personal experience helping shape how thousands of developers work today.


## Where the journey began


Alessandra’s story dates back to 2007, when she attended a meetup in New York City where Rich Hickey publicly introduced Clojure for the first time. She walked out of that session impressed as Clojure brought the power of an interactive Read-Eval-Print Loop (REPL) to the JVM ecosystem, enabling software developers to inspect and modify running programs and receive immediate feedback.


Sierra became one of the earliest adopters of Clojure for professional work and made significant early contributions to its standard library. Within a few years, this led to her joining Relevance, later renamed Cognitect, which was[acquired by Nubank in 2020](https://building.nubank.com/nubank-acquires-cognitect-press-release/) .


[Check our job opportunities](https://bit.ly/jobs-at-nu)


## The power of the REPL and the frustration of interruptions


As a consultant at Cognitect, working with teams adopting Clojure for real-world applications, Sierra noticed a recurring pattern: the REPL gave developers enormous advantages in feedback and velocity, but many application structures made that experience harder than it needed to be.


In the early 2010s, web development in Clojure often centered around Ring: simple and elegant, but with trade-offs. Running a Jetty server from the REPL could block the main thread, and reloading code often left stale definitions in memory. Restarting the REPL became routine, and frustrating.


Sierra wanted developers to keep the flow of interactive development even in systems with state and complexity. The core question was simple but fundamental: How can a system keep running while the developer continues evolving it?


## The birth of Component


Instead of accepting the friction caused by restarting the REPL every time code changed, Sierra spent the following years designing a new discipline — a workflow that would eventually be known as the **Reloaded Workflow** , named after her widely read blog post “[My Clojure Workflow, Reloaded](https://www.cognitect.com/blog/2013/06/04/clojure-workflow-reloaded) .”


The goal was straightforward but transformative: developers should be able to evolve a running system safely, consistently, and without losing context.


This approach combined the[tools.namespace](https://github.com/clojure/tools.namespace) library with[Component](https://github.com/stuartsierra/component) , along with design practices that encouraged explicit dependencies, minimized global state, and separated pure logic from stateful boundaries. The result was a development experience centered on the REPL, where applications could be started, stopped, refreshed, and inspected in real time, without breaking flow.


Component offered a lightweight way to model systems as independent parts with clear lifecycles, without sacrificing functional design. Its small API surface and long-term stability were intentional: changes were introduced slowly and deliberately, helping the library remain simple, approachable, and durable.


## A lasting impact


Twelve years after its introduction, Component remains one of the most influential libraries in the Clojure ecosystem. It has been referenced more than **13,000 times in Nubank’s production source code** , continues to shape extensions and forks, and has even inspired ports to other languages — a level of longevity rarely achieved by tooling with such a minimal footprint.


Sierra closed her talk by acknowledging how fortunate she was to be at the right place, at the right time, with the right job, as Clojure was emerging. That kind of timing can’t be engineered.


But her advice to people beginning with Clojure or open-source was universal:


> *“Work on whatever you find interesting. Find a pattern that’s useful, build tools that help others benefit from it. But mostly, just have fun, because that’s the only thing you can choose.”*
>
>
> Alessandra Sierra, Principal Software Engineer at Nubank
