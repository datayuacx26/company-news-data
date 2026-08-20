---
schema_version: "1.0.0"
document_id: "d2254afbc12155fb7b948f6e4179dbe7b34b6ea0afd3e881fd6d58d718fe38ca"
company_key: "yc-pgdog"
company: "PgDog"
source_id: "yc-pgdog-rss-662afb283f74"
canonical_url: "https://pgdog.dev/blog/writing-nothing-but-docs"
published_at: "2025-09-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:49.478310+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:9986a8d9ac415175131f25b655627f32cb43b34d68787b5d2a6ca520362550bb"
---

# Writing nothing but docs for a week

I spent all of last week writing documentation. Literally. You can check my[commit history](https://github.com/pgdogdev/pgdog/commits/main/?before=3b5c98c7830fb1edd65049ea5c095ca05d96b921+35) . You’d think it was a pretty light week, but no. I spent every waking hour in my markdown editor, trying to explain to the world what we have built.


I discovered entire sections that I had no recollection of, which you may think is concerning for the founder of a database company, but actually is a sign of huge progress we’ve made over the last few months.


PgDog is[built](https://github.com/pgdogdev/pgdog)[in](https://github.com/pgdogdev/helm) the[open](https://github.com/pgdogdev/docs) , so you can see everything. That includes the[docs](https://docs.pgdog.dev/) , which are now 3k lines of pretty technical stuff, and it’s getting to a point where you can’t read it in one sitting.


### Why write docs


Documentation for code is kinda useless. It can’t be trusted because we don’t update it when the code changes. I don’t think we are to blame here. Our text editors render it in that light shade of grey, as if it didn’t really matter. Whoever designed the color theme certainly thought so. We know how our code works.


Documentation for a software project is *the only thing* that matters. Literally not a single user (except the rare and precious ones) would ever read your code to understand how it works. And software we don’t understand, we would never use.


Consider this hypothetical. You finally find that library for some obscure, 20 year old, RFC spec for some OAuth flow you desperately need but would never implement yourself. You feel an immense sense of relief: you would download that library in a second and slap it straight into the critical path with no remorse or afterthought.


But then you notice that it’s basically undocumented, and your heart sinks to the floor. No way you can convince your team (or yourself, really) that this can be used in prod. There is just something really off about projects that do something but don’t tell you how. It’s like they have something to hide.


More often than not, the author just didn’t have the time. Writing docs is harder than writing code, and for an open source library, the ROI1 , beyond the glory and admiration that comes from doing a good job, isn’t there. But for databases, trains, planes and other critical pieces of infrastructure that we rely on every day, documentation is critical. Donald Douglas said it right:


> *When the weight of the paper equals the weight of the plane, only then you can go flying.*


We don’t print stuff anymore, but our documentation is getting pretty good.


### How to get it right


Writing docs is really hard. It’s harder than coding, for sure. You can’t really use LLMs, because they don’t really understand *why* your code does what it does. They are probability machines, spitting out the next word in a sentence. They certainly don’t know the edge cases your users need to be aware of.


So at the end of the day, it’s really up to us, the humans, to tell the world *why* we did it.


Accurate documentation is pretty important, so we have taken steps to make it easier. First, we’re using[MkDocs](https://squidfunk.github.io/mkdocs-material/) , so all of our docs are just Markdown. Spell-checking with Claude has been pretty good: I just need to tell it three times to check again, until it fixes all the comma splices. My prompt is:


> *Spell check, grammar check and punctuation check file X. Repeat until no more errors are found.*


Funny enough, if I don’t mention all three kinds of errors, it’ll ignore some pretty obvious ones. I think it takes everything I say *very* literally and doesn’t like to do “extra” work unless explicitly asked. Talk about micromanagement.


Next, we have examples. Since PgDog is config-driven, that’s mostly a bunch of TOML code blocks. This is where I got myself into some trouble, because I kept fat-fingering setting names. It’s pretty frustrating, as a user, to copy/paste an example config only to have the app yell at you about missing or nonexistent fields.


So, I wrote a Python script to extract them (using just a regex), and pass them to the newly added[configcheck](https://github.com/pgdogdev/pgdog/pull/490) command in PgDog. This command isn’t documented yet (oh, sweet irony), but it allows you to validate that a config file is correctly written, without starting the proxy process. Just like` nginx -t` , if you’re familiar.


As soon as that shipped, the number of typos in the examples dropped to 0.


Next, it was the SQL. Since we are sharding Postgres, we have a lot of examples that show how queries work. A typo there doesn’t look very professional, so I added` pglast` into my test script, which is a Python binding on top of our favorite library:` pg_query` .


Just like PgDog parses every SQL command it receives,` pglast` does the same for our documentation. If a code snippet isn’t valid Postgres SQL, we shouldn’t merge the PR. Now that all of this runs in CI, those typos are gone, too.


### No substitute for elbow grease


Documentation is hard work, and writing it takes time. It’s never finished, because the software isn’t either, so you have to constantly refactor it. Delete old and embarrassing stuff that no longer works, add new and awesome stuff that does.


If you find an issue in our[documentation](https://github.com/pgdogdev/docs) , don’t hesitate to file an issue or submit a PR. You don’t have to know how to shard Postgres to contribute to the project.


1.


Radio on the Internet.↩
