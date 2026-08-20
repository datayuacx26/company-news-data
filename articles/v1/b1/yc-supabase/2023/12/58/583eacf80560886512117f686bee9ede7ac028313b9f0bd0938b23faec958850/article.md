---
schema_version: "1.0.0"
document_id: "583eacf80560886512117f686bee9ede7ac028313b9f0bd0938b23faec958850"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/launch-week-x-best-launches"
published_at: "2023-12-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:20.635269+00:00"
content_hash: "sha256:595ea3df4fddda013887c5be8939ae48a7df216e0836af80ed8c9425931bd093"
---

# Top 10 Launches of LWX

This launch week was unique. We had so much content that we decided to do a “main stage”, with five major features, and a “build stage” with additional content. It's a lot to digest, so here are 10 of my favorites.


## Top 10#


### #1: We teamed up with Fly#


We're launching Fly Postgres, a managed Postgres offering by Supabase and[Fly.io](http://fly.io/) . Fly's current Postgres offering is unmanaged, so we're working with them to bring the same delightful Postgres experience to Fly.


[Read more](https://supabase.com/blog/postgres-on-fly-by-supabase)


### #2: We launched Supabase Grafana#


We shipped an open source observability suite for your Supabase project, using Prometheus and Grafana. It collects around 200 metrics and can be deployed to any server.


[Read more](https://github.com/supabase/supabase-grafana)


### #3: pg_graphql now supports Postgres Functions#


Supabase GraphQL (pg_graphql) 1.4+ supports the most requested feature: Postgres functions a.k.a. User Defined Functions. Execute custom SQL logic within GraphQL queries to support complex server-side operations.


[Read more](https://supabase.com/blog/pg-graphql-postgres-functions)


### #4: Python libs are now stable#


Supabase Python is now stable and ready to use in your Python applications. We've created a few guides and examples to show how easy it is to use Python libraries with existing frameworks like Flask.


[Read more](https://supabase.com/blog/client-libraries-v2)


### #5: Aggregate Functions in PostgREST#


Support for aggregate functions has been much requested feature that went through multiple iterations of design and review. PostgREST 12 was just released and it now supports` avg` ,` count` ,` max` ,` min` ,` sum` .


[Read more](https://supabase.com/blog/postgrest-12)


### #6: Supavisor 1.0#


Supavisor is a cloud-native connection pooler for Postgres, built with Elixir. We've migrated all projects on the platform from pgbouncer to Supavisor. Every new Supabase project launched now gets a Supavisor connection string to use for connection pooling.


[Read more](https://supabase.com/blog/supavisor-postgres-connection-pooler)


### #7: Edge Functions now support Node & NPM#


Edge Functions now natively supports npm modules and Node built-in APIs. You can directly import millions of popular, commonly used npm modules into your Edge Functions.


[Read more](https://supabase.com/blog/edge-functions-node-npm)


### #8: Leaked Password Protection with Have I Been Pwned#


we have integrated the[HaveIBeenPwned.org](https://haveibeenpwned.com/) *Pwned Passwords API* in Supabase Auth to prevent users from using leaked passwords. This will prevent your users from using a password that has previously been leaked.


[Read more](https://supabase.com/blog/supabase-auth-identity-linking-hooks)


### #9: Supabase Branching#


Branching gives you a Postgres database for every Pull Request. You can run experimental changes on your branch database, and then merge your changes into production when you're happy with the changes. We're rolling out branching in batches.


[Read more](https://supabase.com/blog/supabase-branching)


### #10: Postgres Read Replicas#


Read replicas continuously, well, *replicate* data from a primary database. It contains a constantly-updated copy of the data in your Primary. These are great for distributing data closer to your users to reduce application latency, and for reducing the load on your Primary database.


[Read more](https://supabase.com/blog/introducing-read-replicas)


### Bonus: we dropped an Album#


I made a[bet](https://twitter.com/kiwicopple/status/1664118998169014273) with Sam that we would make an album. Luckily Jon has a side hobby. Check out[supabase.productions](https://supabase.productions/) .


## Cool things from the community#


I also wanted to highlight a few things that happened in the community over the past few weeks:


### Local AI Stack with Yoko from a16z#


I caught up with[Yoko](https://twitter.com/stuffyokodraws) from Andreessen Horowitz to chat about the[Local AI stack](https://github.com/ykhli/local-ai-stack) that she developed with Supabase, Ollama, Langchain, and Next.js.


### **Finance integrations with OpenBB**#


Didier explained the origins of[OpenBB Terminal](https://github.com/OpenBB-finance/OpenBBTerminal) , the first financial terminal that is free and fully open source. He shared some details about the integration that he's building with Supabase.


### Offline sync with ElectricSQL#


Offline sync is one of the most requested features for Supabase. I caught up with the team at ElectricSQL to learn more about their[integration](https://supabase.com/partners/integrations/electricsql) .


### Basejump is like “shadcn for Supabase”#


In the past few months,[Tiniscule](https://twitter.com/tiniscule) has been quietly upgrading the[Basejump](https://usebasejump.com/) starter kit for Supabase. I think of it a bit like[shadcn](https://ui.shadcn.com/) for Supabase - super easy to template out a secure, scalable enterprise app. Along the way, he's been developing periphery tooling like[test helpers](https://github.com/usebasejump/supabase-test-helpers) , and pushing us to provide better primitives. One of our most common questions is “accounts and permissions?”.[Basejump](https://usebasejump.com/) solves that and more.


### Mockup madness from tldraw#


Oh, and the[tldraw](https://www.tldraw.com/) team have been on an[absolute tear this week](https://twitter.com/tldraw/status/1734624421623521719) :


### In case you missed it …#


- [Supabase + OpenAI cookbook](https://github.com/openai/openai-cookbook/pull/913)
- [Self-hosting Supabase on Linode](https://twitter.com/linode/status/1735068344410108170)
- [k8s is coming](https://github.com/supabase-community/supabase-kubernetes/pull/48)
- [Bun supports Supabase](https://twitter.com/bunjavascript/status/1734470860755566815)
- [Svix Webhooks, from Postgres](https://www.svix.com/blog/use-svix-from-supabase/)
- [Offline support with Powersync](https://www.powersync.com/blog/flutter-tutorial-building-an-offline-first-chat-app-with-supabase-and-powersync)
- [Lowcode Supabase with Buildship](https://docs.buildship.com/supabase)
- [Visual programming with Comnoco and Supabase](https://docs.comnoco.com/usecases/Supabase/)


### And finally…#


We wouldn't be anything[without Postgres](https://twitter.com/supabase/status/1735377750788042824?s=20) .
