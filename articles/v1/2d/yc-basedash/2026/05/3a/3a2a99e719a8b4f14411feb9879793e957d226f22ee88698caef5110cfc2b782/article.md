---
schema_version: "1.0.0"
document_id: "3a2a99e719a8b4f14411feb9879793e957d226f22ee88698caef5110cfc2b782"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/paginating-user-written-sql-across-every-dialect-we-support/"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:cbfbd897d60d24f7b88be2c1674d20ddcc206327a28500da3b70f7ded85d2253"
---

# Paginating user-written SQL across every dialect we support

A lot of Basedash boils down to running a SQL query the user (or our chat agent) wrote, and showing the rows. The query runs against whichever database the customer connected: Postgres, MySQL, BigQuery, Snowflake, Redshift, ClickHouse, Databricks, DuckDB, Athena, SQL Server, Spanner, plus a few flavors on top of those (Supabase, PlanetScale, PostHog).


Every one of those queries gets paginated, because we can’t reasonably stream a 12M row result set to the browser. So we slap a` LIMIT 100 OFFSET 200` on the query and ship the page. That sounds like a one-liner, and it was, until it wasn’t.


This is the story of how our pagination worked, why it kept breaking on the more annoying dialects, and what we replaced it with.


## The original approach: wrap and pray


The first version was simple, and it lasted for a long time. Take whatever the user wrote, wrap it in a subquery, and append pagination:


```text
SELECT   *   FROM   (
-- whatever the user wrote
)   AS   t
LIMIT   100
OFFSET   200  ;
```


This worked for the obvious dialects. Postgres, MySQL, Snowflake, Redshift, BigQuery, DuckDB, ClickHouse, Databricks. They all accept` LIMIT/OFFSET` on the outside and don’t care that the inner query already has its own` ORDER BY` .


It also has 2 nice properties we didn’t want to give up. The user’s query is opaque to us in the wrapper, so we don’t have to understand its grammar to paginate it. And if the user writes` SELECT id, name FROM users` , we don’t need to know what columns came back to project them again.


Most days, this is fine. The exceptions are where it gets interesting.


## The dialects where wrapping falls apart


**SQL Server.** T-SQL doesn’t have` LIMIT` . It has` OFFSET ... ROWS FETCH NEXT ... ROWS ONLY` , which requires an` ORDER BY` on the same query. You can’t put` ORDER BY` inside a derived table without also adding` TOP` or` OFFSET` , so the wrapper can’t just borrow the inner query’s order. Our code, having looked at this and given up, used to literally do:


```text
protected   wrapQueryWithPagination  ({ sqlQuery }: { sqlQuery: string }): string {
// SQL Server doesn't handle pagination well, so we'll just avoid pagination
// for now.
return   sqlQuery;
}
```


SQL Server users got no pagination at all. The whole result set came back, and the browser dealt with it. Not great.


**Spanner.** Spanner does support` LIMIT/OFFSET` , but its subquery semantics don’t preserve ordering from inner to outer. So` SELECT * FROM (SELECT ... ORDER BY x) AS t LIMIT 100` will give you 100 rows in some order, just not necessarily the one the user asked for. We took the same shortcut: skipped pagination entirely and shipped the full result.


**Athena.** Athena (built on Trino/Presto) puts` OFFSET` before` LIMIT` , the opposite of MySQL and Postgres. So we had a separate Athena-specific wrapper that emitted:


```text
SELECT   *   FROM   (
-- ...
)   AS   t
OFFSET   200
LIMIT   100  ;
```


Same shape, different keyword order, separate code path.


**Already-paginated queries.** This one we’d been ignoring. If a user writes` SELECT * FROM events ORDER BY ts DESC LIMIT 10` , our wrapper turns it into:


```text
SELECT   *   FROM   (
SELECT   *   FROM   events   ORDER BY   ts   DESC   LIMIT   10
)   AS   t
LIMIT   100   OFFSET   0  ;
```


Which technically returns the right thing on page 1 (10 rows, the inner LIMIT wins). But on page 2, you get an empty result set, because the inner LIMIT already capped it at 10 rows and the outer OFFSET 100 lands past the end. The user wrote a query with an explicit limit, and our pagination broke their intent.


So the wrapper was a leaky abstraction with provider-specific overrides taped onto it. Time to do something different.


## What we wanted instead


The thing we actually want is to take the user’s` SELECT` (or` WITH ... SELECT` , or` UNION` , or whatever) and add a` LIMIT/OFFSET` clause directly to the root statement. Like this:


```text
-- input
SELECT   id,   name   FROM   users   ORDER BY   id


-- output
SELECT   id,   name   FROM   users   ORDER BY   id   LIMIT   100   OFFSET   200
```


No subquery, no` t` alias polluting error messages, no provider-specific overrides for syntax order. The inner query keeps its` ORDER BY` , and the pagination clause gets attached to the same scope.


To do that without writing 10 different SQL parsers, we needed an AST.


## Enter` @polyglot-sql/sdk`


We pulled in[@polyglot-sql/sdk](https://www.npmjs.com/package/@polyglot-sql/sdk) (a TypeScript wrapper around a multi-dialect SQL parser) for the rewrite. The 3 functions we care about:


- ` parse(sql, dialect)` returns an AST or an error.
- ` ast.removeLimitOffset` ,` ast.setLimit` ,` ast.setOffset` mutate the AST.
- ` generate(statements, dialect)` serializes back to SQL for that dialect.


The parsing is surprisingly forgiving across dialects. We picked one` Dialect` per provider and it mostly does the right thing:


```text
protected   getPaginationDialect  (): Dialect {
const   dialectBinding   =   this  .logger.  bindings  ?.().dialect;
switch   (dialectBinding?.  toUpperCase  ()) {
case   'ATHENA'  :         return   Dialect.Presto;         // Athena is Presto syntax
case   'BIGQUERY'  :       return   Dialect.BigQuery;
case   'CLICKHOUSE'  :
case   'POSTHOG'  :        return   Dialect.ClickHouse;
case   'DATABRICKS'  :     return   Dialect.Databricks;
case   'DUCKDB'  :         return   Dialect.DuckDB;
case   'MYSQL'  :
case   'PLANETSCALE'  :    return   Dialect.MySQL;
case   'POSTGRES'  :
case   'SUPABASE'  :       return   Dialect.PostgreSQL;
case   'REDSHIFT'  :       return   Dialect.Redshift;
case   'SNOWFLAKE'  :      return   Dialect.Snowflake;
case   'SQL_SERVER'  :     return   Dialect.  TSQL  ;
case   'SPANNER'  :        return   Dialect.Generic;
default  :               return   Dialect.Generic;
}
}
```


Spanner gets` Generic` because the SDK doesn’t ship a Spanner profile yet, but Spanner’s pagination grammar is close enough to ANSI that the generic generator produces something Spanner accepts.


## The happy path


For most queries on most dialects, the rewrite is 4 steps:


1. Parse the query.
2. Get the root query node (the outermost` SELECT` , or the top of a` UNION` /` INTERSECT` /` EXCEPT` ).
3. Set` LIMIT` and` OFFSET` on that node.
4. Re-emit the SQL.


```text
const   parsed   =   parse  (trimmedSqlQuery, dialect);
const   statement   =   parsed.ast?.[  0  ];


let   paginatedStatement   =   ast.  removeLimitOffset  (statement);
paginatedStatement   =   ast.  setLimit  (paginatedStatement, limit);
if   (offset   >   0  ) {
paginatedStatement   =   ast.  setOffset  (paginatedStatement, offset);
}


const   generated   =   generate  ([paginatedStatement], dialect);
return   generated.sql.  join  (  ';  \n  '  );
```


The` removeLimitOffset` step is paranoia, in case some downstream code somehow added pagination already. After that, what comes out the other side is exactly what the user wrote, with a` LIMIT/OFFSET` (or` OFFSET/FETCH` for SQL Server) on the outermost query. CTEs, window functions,` ORDER BY` clauses, all preserved.


It’s a much smaller change than it sounds, because the AST library does almost all of the work. The interesting part is the cases where we deliberately don’t take the happy path.


## When we do still wrap


Two cases.


**The user already paginated.** If the parsed query has` LIMIT` ,` OFFSET` ,` FETCH` , or` TOP` on the root node, we don’t touch their clauses. We fall back to the old subquery wrapper, so our pagination runs as a second layer on top of whatever they wrote.


```text
if   (  hasPaginationClause  (rootQueryNode)) {
return   this  .  wrapQueryWithSubqueryPagination  ({ sqlQuery, limit, page });
}
```


This is the only way to honor the user’s intent without trying to be clever. If they wrote` LIMIT 10` , page 1 should be those 10 rows, page 2 should be empty. The wrapper gives them that. Editing the AST and replacing their` LIMIT` with ours would silently change the answer.


**The parser couldn’t make sense of it.** Vendor extensions, weird DDL, dialect quirks the parser hasn’t picked up yet. If` parse` fails, we wrap. The wrapper is a worse pagination, but a worse pagination is a lot better than a 500.


```text
if   (  !  parsed.success   ||   !  statement) {
this  .logger.  debug  ({ dialect, parserError: parsed.error },   'Skipped SQL pagination rewrite'  );
return   this  .  wrapQueryWithSubqueryPagination  ({ sqlQuery, limit, page });
}
```


We log every fallback, so we can keep an eye on the parser’s coverage and chase down whatever weird shape the user is feeding us.


## The SQL Server special case


SQL Server pagination requires` ORDER BY` . So if the user’s query has none, the AST rewrite would emit invalid T-SQL. Our fix is to inject a no-op order:


```text
ORDER BY   (  SELECT   NULL  )
```


That’s the conventional T-SQL incantation for “I don’t care, but I have to put something here”. We parse a tiny throwaway statement once at module load to grab a real` ORDER BY` AST node, then clone it for any SQL Server query that needs one:


```text
function   createSqlServerNoOpOrderBy  ()  :   PolyglotOrderBy   |   null   {
const   parsed   =   parse  (  'SELECT 1 ORDER BY (SELECT NULL)'  , Dialect.  TSQL  );
// ...pull the order_by node off the parsed AST
return   queryNode.order_by;
}


const   SQL_SERVER_NO_OP_ORDER_BY   =   createSqlServerNoOpOrderBy  ();
```


Then, when paginating a SQL Server query without an existing order:


```text
if   (dialect   ===   Dialect.  TSQL   &&   !  hasOrderByClause  (paginatedRootQueryNode)) {
paginatedRootQueryNode.order_by   =   structuredClone  (  SQL_SERVER_NO_OP_ORDER_BY  );
}
```


We also strip` TOP n` and any pre-existing` FETCH` if we’re rewriting, because both conflict with the` OFFSET ... FETCH NEXT` we’re about to add. The result is a query that pages cleanly even when the user didn’t think about ordering.


The output for a basic` SELECT * FROM users` on page 2 looks like:


```text
SELECT   *   FROM   users   ORDER BY   (  SELECT   NULL  ) OFFSET   10   ROWS   FETCH   NEXT   10   ROWS   ONLY
```


It’s not the prettiest, but the SQL Server users we tested it against are real customers paginating real tables for the first time.


## What this got us


A handful of things that are worth the extra dependency.


SQL Server and Spanner now actually paginate. Before this change we returned the entire result set on those 2 backends, which was a problem we’d been quietly punting. The customers running on SQL Server got the bigger end of the deal, since their result sets were the biggest.


We deleted 4 provider-specific` wrapQueryWithPagination` overrides (Athena, SQL Server, Spanner, plus an inherited variant on the base` Connection` class). The single shared implementation handles all of them through the dialect map. New providers (we shipped Oracle the same week) get correct pagination by adding 1 case to the dialect switch and writing tests.


Error messages and query plans got noticeably nicer. The wrapped query had an extra projection through a derived table called` t` , which showed up in EXPLAIN output and exception traces. That’s gone for the 90% of queries that the parser handles.


Already-paginated queries finally do the right thing on page 2 and beyond. We got 2 support pings about this in the year before the change, and 0 since.


## What we took away


Some things we’ll keep in mind the next time we touch user-supplied SQL.


1. **A wrapper is a tempting first move and a bad long-term home.** Wrapping a user query in a subquery is the easiest way to add a clause without parsing anything. It works until the dialect or the user’s intent disagrees with you, and then your only knobs are provider-specific overrides.
2. **Parse the SQL when you need to be precise about what changed.** We avoided this for years because writing a SQL parser is a project on its own. Once a good multi-dialect parser is a dependency you can pull in, the calculus changes.
3. **Always have a safe fallback.** Parsing fails sometimes. The right answer when it does is to fall back to the dumber thing that mostly works, log loudly, and fix the parser later. Crashing on an unparseable but valid query is a bug we’d rather not ship.
4. **` ORDER BY (SELECT NULL)` is a T-SQL idiom you should know exists.** When the dialect insists on an ordering clause and you genuinely don’t have one, this is the canonical “no order, please” expression. Use it on purpose, not by accident.
5. **Treat already-paginated user queries as a separate case from un-paginated ones.** It’s tempting to assume the user “didn’t mean it” and override their` LIMIT` . They did mean it, and it’s easier to wrap their query than to argue with it.


If you want to see the surrounding stack, we have writeups on[virtualizing our table to render 100x more rows](https://www.basedash.com/blog/how-virtualization-increased-our-table-performance-by-500) ,[reordering pages with floating-point indices](https://www.basedash.com/blog/implementing-re-ordering-at-the-database-level-our-experience) , and[the Kubernetes default that was killing our Node.js pods](https://www.basedash.com/blog/what-was-killing-our-healthy-kubernetes-pods) .


If you want to put a customer’s SQL through a pipeline like this without writing it yourself,[Basedash](https://www.basedash.com/) is the AI-native BI platform we’re building on top of it.
