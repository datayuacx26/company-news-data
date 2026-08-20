---
schema_version: "1.0.0"
document_id: "97e5a58c9eac3ff326afe0404dbbc8fa33e4eaf400ae3b826f6e6c41536b4fba"
company_key: "yc-cleartax"
company: "Clear"
source_id: "yc-cleartax-rss-3da8b6d91e7d"
canonical_url: "https://medium.com/cleartax-engineering/sqlcli-or-creating-a-side-project-in-rust-without-knowing-the-language-4364d641faca"
published_at: "2021-02-24T03:48:50+00:00"
first_seen_at: "2026-07-27T13:12:04.161420+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:3e3ee13c37c318b2e970bd9ba84c75c23c0a07c6366b7a366802959b93ea4529"
---

# sqlcli, or: Creating a side-project in rust without knowing the language

Open Source


Rust


Sql


Data Platforms


Cleartax


# sqlcli, or: Creating a side-project in rust without knowing the language


[Ankit Solanki](https://medium.com/@_anks?source=post_page---byline--4364d641faca---------------------------------------)


4 min read


·


Feb 24, 2021


--


We just open-sourced a small command-line tool called[sqlcli](http://github.com/cleartax/sqlcli) . This blog post is the story of how this tool came into existence.


## Motivation


ClearTax’s data platform has a lot of small parts. One of the tools we have built is a[DIY ETL layer](https://medium.com/cleartax-engineering/diy-etl-for-analysts-on-redshift-4dfe424931e3) that lets you quickly do transformations with a bunch of SQL queries. Basically, this ETL layer allows you to:


- Write an arbitrarily complex SQL query
- Save the output of executing this SQL query into a table (basically, do a` CREATE TABLE AS` )
- Finally, schedule execution for these queries so the output tables can be repopulated on a given frequency


This worked for a while, but as the number of such jobs that were scheduled started going up, we started to see issues:


- Jobs were dependent on each other: if a query two reads from the output of query one, query two should be executed after query one.
- Evolving schema of the data warehouse resulted in some of the saved queries becoming invalid without clear visibility.
- Since it is easy to create a specific ‘view’, we ended up doing too many such views (leading to inefficiencies, cost increases, etc)


We started to look for tools that could help with these issues. We were looking for something that can give us *intelligence* on user-entered SQL queries, so we could lint / validate them before execution.


One of our end goals was to also automate the scheduling — so that the jobs would run in order of dependencies without manual scheduling / prioritisation. (We’re still not 100% there yet, but this tool is a step in that direction.)


## Search


I tried to find and evaluate existing solutions first.


Uber’s[queryparser](https://github.com/uber/queryparser) seemed like it would exactly fit our needs, but we ran into some problems with it. Since this was a side-project, I didn’t really keep great notes so I don’t remember the exact issues. Suffice to say that we had ruled this out.


After ruling out queryparser, we looked for any libraries that generically read SQL statements and respond with an AST. I tried a couple of python / nodejs libraries, but ended up looking at[sqlparser-rs](https://github.com/ballista-compute/sqlparser-rs) — a rust library.


It was extremely easy to run the[examples](https://github.com/ballista-compute/sqlparser-rs#example) provided in the project’s readme. So in a super-unscientific manner, I chose to proceed with this library even though I have no rust experience — sideprojects are a place to have fun after all :)


## Implementation


I don’t know rust, and all I’ve read about the language led me to believe that it’s a very verbose and low level language. I was wrong.


I still wouldn’t say that I *know* the language in-depth, but rust just has excellent developer tooling and ergonomics. Using[cargo](https://doc.rust-lang.org/cargo/getting-started/first-steps.html) , I was quickly able to set up a new project and get started.


I opened the new project in VS Code, installed the extensions that were recommended to me, and within minutes I had an editor with full syntax checking, type hints and compiler integration.


Rust’s documentation is stellar. And it’s surprisingly easy to build[small scripts](https://v4.chriskrycho.com/2016/using-rust-for-scripting.html) very quickly — the compiler gets out of the way in most scenarios.


And rust has[pattern matching](https://medium.com/cleartax-engineering/safer-programming-with-f-pattern-matching-a2512532a2cd) , my favourite language feature. Pattern matching + recursive descent makes AST Traversal very easy.


And for quick scripts, you don’t really need to understand the borrow checker or understand memory management, liberal usage of` .clone` and` .unwrap` will get you far! My aim was to just get the compiler to[stop complaining](http://xion.io/post/code/rust-borrowchk-tricks.html) .


In fact, without knowing the language at all, I had a working prototype for the` tables` command of sqlcli within 90 minutes!


And since rust makes it dead easy to[do testing](https://doc.rust-lang.org/rust-by-example/testing/unit_testing.html) , I was actually pretty sure that the prototype was actually working!


After this initial proof of concept, the project lay idle for a few months since we hadn’t yet picked up the changes on the infra platform side. I recently decided to open-source it, it actually took me longer to pretty it up than write the initial implementation.


I am sure that any rust programmer who looks at the source of sqlcli would probably have a lot of feedback — I’m not sure it’s idiomatic or if it follows all the best practices. But I’m reasonably certain that it works reliably and it has been able to handle the complex test cases I’ve thrown at it. Not bad for a weekend hack :)


## Conclusion


Working with rust was surprisingly pleasant. I don’t think it will be a major part of our tech stack at ClearTax right now, but I am looking forward to personally hacking with this language. It’s now on my personal list of languages to learn / master. My next side project may probably be in rust again.


Any language community that makes onboarding so simple deserves to succeed.
