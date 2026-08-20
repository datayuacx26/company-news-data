---
schema_version: "1.0.0"
document_id: "cc7af76516d96ff9c4c04ec5ebd004e376c665f205b4605ab7e39b3d8070ae01"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/orm-or-raw-sql/"
published_at: "2026-07-15T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:289fd2ea034f74016d8ee7eb63d62d8105d98200aef2a036b275d9d1fc192102"
---

# ORM vs Raw SQL: Which Should You Choose?

For most applications the honest default is an ORM or query builder for the 90% of CRUD that’s routine, and hand-written SQL for the small set of reporting, analytics, and hot-path queries that measurably earn it — it’s not an either/or decision. The question that actually matters isn’t “which one is better in the abstract” but “which one for *this* query, on *this* team, given *these* constraints.” This article gives you a decision rule, a comparison table, and the one failure mode — the N+1 query — that bites teams far more often than raw abstraction overhead ever does. Examples are anchored in the JS/TS stack (Prisma, Drizzle, Kysely, Knex), since that’s where most of these decisions are being made today.


## Key Takeaways


- Default to an ORM or query builder for routine CRUD and reach for raw SQL on the specific reporting, analytics, and hot-path queries that measurably need it — most production apps end up running both.
- The real performance tax of an ORM is almost never the abstraction itself; it’s the N+1 query pattern, where loading a list and then its relations one row at a time turns one page load into hundreds of round-trips.
- You fix N+1 by eager-loading the relation in a single query (Prisma’s` include` or a` JOIN` ) and selecting only the columns you render — not by abandoning the ORM.
- Query builders like Drizzle, Kysely, and Knex are the missing third option: SQL-shaped, type-checked queries with full control of the generated SQL and no object-mapping layer.
- ORMs parameterize queries by default, which closes off the most common SQL-injection vector; raw SQL is only as safe as your discipline with parameterized statements.


## What an ORM is, and what “raw SQL” means


An **ORM (Object-Relational Mapper)** is a library that maps database tables to objects in your programming language, so you query data through method calls and typed models instead of writing SQL strings by hand. It generates the SQL, sends it to the database, and hydrates the rows back into objects, handling relations, parameterization, and type mapping along the way. In the JS/TS world,[Prisma](https://www.prisma.io/docs) and[TypeORM](https://typeorm.io/) are the full-ORM examples. A typical Prisma read looks like this:


```text
// Users plus their posts, in one call
const   users   =   await   prisma  .  user  .  findMany  ({
where  :   {   posts  :   {   some  :   {   title  :   {   contains  :   '  test  '   }   }   }   },
include  :   {   posts  :   true   },
});
// users[0].posts is already a typed array — no manual reshaping
```


**Raw SQL** means writing the query text yourself and executing it through a database driver, then reshaping the flat result rows into the structure your application needs. You get full control of the exact query, but you also own the mapping. The same “users with their posts” read against` pg` requires a join and a manual regroup:


```text
const   {   rows   }   =   await   pool  .  query  (
`  SELECT u.id, u.name, p.id AS post_id, p.title
FROM users u
LEFT JOIN posts p ON p.author_id = u.id
WHERE p.title LIKE $1  `  ,
[  '  %test%  '  ],
);


// Flat rows -> nested users, done by hand
const   users   =   Object  .  values  (
rows  .  reduce  ((  acc  ,   r  )   =>   {
acc  [  r  .  id  ]   ??=   {   id  :   r  .  id  ,   name  :   r  .  name  ,   posts  :   []   };
if (  r  .  post_id  )   acc  [  r  .  id  ].  posts  .  push  ({   id  :   r  .  post_id  ,   title  :   r  .  title   });
return   acc  ;
}, {}   as   Record  <  number  , { id  :   number  ; name  :   string  ; posts  :   any  [] }>),
);
```


That` reduce` is the real cost of raw SQL for relational reads: the database returns rows, not object graphs, and you write the join-and-regroup logic every time. The` $1` placeholder is also doing important work — see safety, below.


## ORM vs raw SQL: the comparison that actually decides it


The decision comes down to five criteria. Here’s how the three approaches — full ORM, query builder, and raw SQL — compare across them:


Criterion Full ORM (Prisma, TypeORM) Query builder (Drizzle, Kysely, Knex) Raw SQL


**Injection safety** Parameterized by default Parameterized by default Safe only with parameterized statements


**Dev speed (CRUD)** Fastest — relations, hydration handled Fast — SQL-shaped, you write joins Slowest — manual mapping


**Migrations / schema** First-class (Prisma Migrate, TypeORM) Built-in or paired tool (Drizzle Kit, Knex migrations) Hand-written DDL scripts


**Portability across DBs** High — swap providers with little change Medium — dialect-aware Low — dialect-specific SQL


**Control / DB-specific features** Limited; escape hatch needed High — close to SQL Total — every feature, every hint


**Type safety** End-to-end from schema Strong (Kysely/Drizzle); best-effort (Knex) None without extra tooling


On **safety** , the practical truth is simple: ORMs and query builders parameterize values by default, which closes off the classic injection vector. Raw SQL is only as safe as your discipline — Prisma’s own docs warn that[if you use the unsafe method with user inputs you open up the possibility for SQL injection attacks](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql/raw-queries) , which can expose your data to modification or deletion. Use placeholders (` $1` ,` ?` ) and never interpolate user input into the query string, and raw SQL is safe; skip that discipline and it isn’t.


On **migrations and portability** , ORMs win the routine case. A schema change becomes a version-controlled, repeatable migration that runs identically across environments, and switching database providers is mostly a config change. Raw SQL gives you neither for free — you manage DDL scripts yourself and your queries are tied to one dialect’s syntax. On **control** , raw SQL wins decisively: window functions, recursive CTEs, database-specific index hints, and` EXPLAIN ANALYZE` tuning are all available without fighting an abstraction.


## The performance truth: it’s N+1, not the abstraction


The most repeated criticism of ORMs — “they generate slow queries” — is mostly wrong for CRUD and mostly right for one specific pattern. Modern ORMs generate competent SQL for everyday reads and writes. The real performance tax is the **N+1 query problem** : you load a list of N rows, then trigger one more query per row to fetch a relation, turning a single page load into N+1 round-trips to the database.


Here’s the failure, which usually hides inside an innocent-looking loop:


```text
// 1 query for the users...
const   users   =   await   prisma  .  user  .  findMany  ();


// ...then N more, one per user — this is the N+1 trap
for (  const   user   of   users  ) {
user  .  posts   =   await   prisma  .  post  .  findMany  ({   where  :   {   authorId  :   user  .  id   }   });
}
```


The fix is not to abandon the ORM. You eager-load the relation in a single query and select only the columns you actually render:


```text
const   users   =   await   prisma  .  user  .  findMany  ({
include  :   {   posts  :   {   select  :   {   id  :   true  ,   title  :   true   }   }   },
});
```


That collapses 1 + N queries into one. The user-facing symptom of an unfixed N+1 is a list or dashboard page that’s snappy on seed data and slow under production volume — exactly the kind of front-end latency a session replay surfaces from the user’s side, pointing you back at the query layer to count queries and add eager-loading.


### Inspect the generated SQL before you blame the tool


Before you conclude an ORM is slow, look at what it actually emits. Prisma logs every query when you instantiate the client with` log: \['query'\]` ;[Kysely](https://kysely.dev/) exposes` .compile()` to return the SQL and parameters; and Drizzle exposes a` .toSQL()` method on a query for the same purpose. Then run[EXPLAIN ANALYZE](https://www.postgresql.org/docs/current/sql-explain.html) on the output to see the plan the database actually executes.


```text
const   prisma   =   new   PrismaClient  ({   log  :   [  '  query  '  ]   });
// every query Prisma runs is now printed — count them, copy them into EXPLAIN ANALYZE
```


This converts “the ORM is slow” from a vibe into an inspectable workflow: read the SQL, count the round-trips, check the plan.


A note on the engine itself: one of the most significant changes in Prisma 7 is[the complete removal of the Rust-based query engine in favor of a TypeScript implementation](https://www.infoq.com/news/2026/01/prisma-7-performance/) . The new Query Compiler runs in TypeScript and WebAssembly, which removes the need for the cross-language serialization step and[results in faster query execution](https://www.prisma.io/blog/from-rust-to-typescript-a-new-chapter-for-prisma-orm) , and because the engine no longer depends on a native binary, you can now use Prisma in environments that support JavaScript or WASM, such as Cloudflare Workers, Bun, and Deno. Prisma reports the rewrite is[often significantly faster where it matters most](https://www.prisma.io/blog/prisma-orm-without-rust-latest-performance-benchmarks) — on large and complex queries — while remaining on par for simpler ones. The point for this comparison: the engine debate has shifted shape rather than vanished, and it’s still secondary to query count. For the overwhelming majority of slow pages, N+1 and over-fetching are the cause, not the abstraction layer.


## Query builders: the third option the binary framing misses


The “ORM vs raw SQL” framing hides a strong middle ground. Query builders like[Drizzle](https://orm.drizzle.team/) , Kysely, and[Knex](https://knexjs.org/) let you write SQL-shaped, type-checked queries and keep full control of the generated SQL, without an ORM’s object-mapping layer. Kysely is the most powerful type-safe SQL query builder for TypeScript, used in production by Deno, Maersk, and Cal.com, with modern TypeScript and zero runtime overhead. It’s a thin abstraction layer over SQL, focusing on familiarity through naming and structure, and predictability through 1:1 compilation.


```text
const   users   =   await   db
.  selectFrom  (  '  users  '  )
.  innerJoin  (  '  posts  '  ,   '  posts.author_id  '  ,   '  users.id  '  )
.  select  ([  '  users.id  '  ,   '  users.name  '  ,   '  posts.title  '  ])
.  where  (  '  posts.title  '  ,   '  like  '  ,   '  %test%  '  )
.  execute  ();
```


The shape mirrors SQL, but column and table names are type-checked against your schema. As Kysely’s docs put it, a query builder[gives you full control over your SQL while ensuring TypeScript catches mistakes early](https://betterstack.com/community/guides/scaling-nodejs/kysely-query-builder/) , before your code even runs. Knex is the long-standing option and works across Postgres, MySQL, SQLite, and more, but be aware of its typing limits: per its own guide,[TypeScript support is best-effort](https://knexjs.org/guide/) , not all usage patterns can be type-checked, and lack of type errors doesn’t currently guarantee the generated queries will be correct, so writing tests is recommended even with TypeScript. For a new TS project that wants type safety without ORM heft, Kysely or Drizzle is the stronger pick.


## A decision rule by scenario


Match the tool to the workload rather than picking one for the whole codebase:


1. **Simple-to-moderate CRUD, a team, and evolving schema → ORM.** You get parameterized queries, version-controlled migrations, relation loading, and end-to-end types. This covers most application code.
2. **Reporting, analytics, bulk operations, or a measured hot path → raw SQL, views, or stored procedures.** When you need window functions, complex aggregation, or hand-tuned plans verified with` EXPLAIN ANALYZE` , write the SQL directly.
3. **You want type safety and SQL control without an ORM → query builder.** Drizzle or Kysely give you SQL-shaped, typed queries with predictable output and no hidden N+1 behavior.


## The hybrid most production apps land on


In practice, mature codebases don’t pick a side — they run an ORM for the bulk of the work and keep a raw-SQL escape hatch for the rest. Prisma makes this explicit with **[TypedSQL](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql/typedsql)** : a feature of Prisma ORM which[lets you write your raw SQL queries in a fully type-safe manner](https://github.com/prisma/prisma/discussions/25106) . You write SQL in a` .sql` file and call it through a generated, typed function:


```text
import   {   getUsersByAge   }   from   '  ./generated/prisma/sql  '  ;
const   users   =   await   prisma  .  $queryRawTyped  (  getUsersByAge  (  18  ,   30  ));
```


It’s a preview feature: you enable it with` previewFeatures = \["typedSql"\]` in the generator block, and because it’s statically typed,[it may not handle certain scenarios such as dynamically generated WHERE clauses](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql) — those still need the untyped raw methods. Prisma frames the workflow exactly as this article does:[a higher-level abstraction that serves the majority of queries, plus a type-safe escape hatch](https://www.prisma.io/blog/announcing-typedsql-make-your-raw-sql-queries-type-safe-with-prisma-orm) for when you need to craft SQL directly. That’s why the modern answer to “ORM or raw SQL” is usually “an ORM (or query builder) that lets you escape to raw SQL when you need to.”


## Conclusion


Pick the ORM or query builder as your default for everyday CRUD, reach for raw SQL on the queries that measurably earn it, and treat the two as layers in one data access strategy rather than rival camps. The concrete next step on any existing project: turn on query logging, find the list or dashboard endpoints, count the queries per request, and fix the N+1s with eager-loading and tighter column selection — that single pass usually recovers more performance than any ORM-versus-SQL rewrite ever would.


## FAQs


What is the difference between an ORM and a query builder?


An ORM maps database tables to objects and hydrates query results into typed models, handling relations and object mapping for you, while a query builder generates type-checked SQL but returns plain rows without an object-mapping layer. Prisma and TypeORM are ORMs; Drizzle, Kysely, and Knex are query builders. Query builders sit closer to SQL, giving more control over the generated query with less abstraction and no hidden relation-loading behavior.


Does using an ORM prevent SQL injection?


Yes, for normal usage. ORMs and query builders parameterize values by default, sending user input as bound parameters rather than interpolating it into the query string, which closes off the classic injection vector. The exception is unsafe raw-query methods: Prisma's docs warn that using the unsafe method with user input opens the possibility for SQL injection. Raw SQL is only as safe as your discipline with placeholders like dollar-one or question-mark.


Is raw SQL faster than an ORM?


Not in most real cases. Modern ORMs generate competent SQL for everyday CRUD, and the usual performance gap comes from query count, not the abstraction. The dominant cause of slow pages is the N+1 pattern, where loading a list and then its relations one row at a time creates hundreds of round-trips. Raw SQL is worth reaching for on reporting, analytics, and measured hot-path queries that genuinely need hand-tuned plans.


How do I see the actual SQL an ORM generates?


Each tool exposes the generated SQL directly. Prisma logs every query when you instantiate the client with the query log option enabled. Kysely exposes a compile method that returns the SQL string and its parameters. Drizzle exposes a toSQL method on a query for the same purpose. Once you have the SQL, run EXPLAIN ANALYZE in PostgreSQL on it to inspect the execution plan and confirm whether indexes are being used as expected.


Can I write raw SQL inside an ORM project?


Yes, and it is the common production pattern. Most ORMs keep a raw-SQL escape hatch alongside their generated queries. Prisma offers TypedSQL, a preview feature enabled with the typedSql preview flag, letting you write SQL in a dot-sql file and call it through a generated, fully typed function. Because it is statically typed, it does not handle dynamically generated WHERE clauses, which still require the untyped raw query methods.


## Understand every bug


Uncover frustrations, understand bugs and fix slowdowns like never before with **OpenReplay** — self-hosted, with full data ownership.


[Star on GitHub](https://github.com/openreplay/openreplay)
