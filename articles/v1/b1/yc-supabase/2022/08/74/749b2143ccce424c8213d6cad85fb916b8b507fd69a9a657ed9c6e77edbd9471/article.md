---
schema_version: "1.0.0"
document_id: "749b2143ccce424c8213d6cad85fb916b8b507fd69a9a657ed9c6e77edbd9471"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgrest-v10"
published_at: "2022-08-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:a506095f497bb4e09d92f39bc5197bc21ce8f82cffef7e4f96449e4e6b4028b2"
---

# PostgREST v10: EXPLAIN and Improved Relationship Detection

PostgREST turns your PostgreSQL database automatically into a RESTful API. Today, PostgREST v10 was[released](https://github.com/PostgREST/postgrest/releases/tag/v10.0.0) . v10 is not available on the Supabase Platform yet, but it is available for[self-hosting](https://supabase.com/docs/guides/hosting/overview) or as a executable binary from the GitHub release page.


Let's take a look at some of its new features that go hand in hand with[supabase-js v2](https://supabase.com/blog/supabase-js-v2) .


## EXPLAIN#


Akin to the[PostgreSQL EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html) command, you can now get the execution plan of a request through HTTP or` supabase-js` . This works transparently for reads, writes and RPC because every request to PostgREST generates a single SQL query.


This is only suitable for development environments and is only enabled when the` db-plan-enabled` config is set to` true` .


### Getting the execution plan through HTTP#


Using` curl` , you can obtain the execution plan by specifying a vendor media type on the` Accept` header.


`
_10


$ curl -H 'Accept: application/vnd.pgrst.plan' \\


_10


'https://<project>.supabase.co/rest/v1/clients?select=*&id=eq.1'


_10


_10


Aggregate (cost=8.18..8.20 rows=1 width=112)


_10


-> Index Scan using clients_pkey on clients (cost=0.15..8.17 rows=1 width=36)


_10


Index Cond: (id = 1)


`


The` text` format is used by default, which gives you the same output you’d get in other SQL clients like` psql` . You can change the format to JSON by using a media type suffix` application/vnd.pgrst.plan+json` .


### Explaining supabase-js queries#


For` supabase-js` , you can get the execution plan using the` explain()` transform. This works for every` supabase-js` method, including` rpc()` . Here’s an example for` select()` .


`
_10


const { data, error } = await supabase


_10


.from('projects')


_10


.select('*')


_10


.eq('id', 1)


_10


.explain()


_10


_10


console.log(data)


`


hideCopy


`
_10


Aggregate (cost=8.18..8.20 rows=1 width=112)


_10


-> Index Scan using projects_pkey on projects (cost=0.15..8.17 rows=1 width=40)


_10


Index Cond: (id = 1)


`


Explaining the plan output in detail is beyond the scope of this blog post but basically it’s a tree of operations that PostgreSQL will follow for executing a query. Here we see the “Aggregate” node which corresponds to the` json_agg` function used by the PostgREST generated query(more details on how to find these later) and the “Index Scan” node which means an index on “id” was used for a fast search.


### Explaining RLS policies#


The raison d'etre of` explain()` is to provide quicker feedback on the performance of queries, especially in the presence of RLS policies. For instance, let’s say we have this basic policy:


`
_10


create policy "anon can't read"


_10


on projects for select to anon


_10


using (


_10


false


_10


);


`


And we use` explain()` again, this time with the` analyze` option(executes the query, same as the EXPLAIN ANALYZE counterpart in SQL) so we can see the execution time.


`
_10


const { data, error } = await supabase


_10


.from('projects')


_10


.select('*')


_10


.eq('id', 1)


_10


.explain({ analyze: true })


_10


_10


console.log(data)


`


hideCopy


`
_10


Aggregate (cost=8.18..8.20 rows=1 width=112) (actual time=0.017..0.018 rows=1 loops=1)


_10


-> Index Scan using projects_pkey on projects (cost=0.15..8.17 rows=1 width=40) (actual time=0.012..0.012 rows=0 loops=1)


_10


Index Cond: (id = 1)


_10


Filter: false


_10


Rows Removed by Filter: 1


_10


Planning Time: 0.092 ms


_10


Execution Time: 0.046 ms


`


Here you can see the “Filter” node, which is a simple` false` as defined by the above policy, this proves that the RLS policy is getting applied. Also the actual “Execution Time” is shown, which is a fraction of a millisecond. Note that this is only the query execution time, it doesn’t account for the latency for transferring the data from the database to the frontend.


### Getting the Query Identifier#


` explain()` is also useful for getting the[pg_stat_statements](https://www.postgresql.org/docs/current/pgstatstatements.html) query identifier, which you can use in the Supabase logs to obtain the generated SQL queries. Here we use the json` format` and the` verbose` option to get it.


`
_10


const { data, error } = await supabase


_10


.from('projects')


_10


.select('*')


_10


.eq('id', 1)


_10


.explain({ format: 'json', verbose: true })


_10


_10


console.log(data\[0\]\['Query Identifier'\])


_10


// 2811722635570756600


`


For getting more detailed information, you can also use the` settings` ,` buffers` ,` wal` options with` explain()` .


## Improved Relationship Detection#


### One-to-one relationships#


To avoid unnecessary JSON arrays in a query result, one-to-one relationships are now automatically detected. For this you can use a:


`
_24


-- A unique constraint on a foreign key


_24


create table country (


_24


id serial primary key,


_24


name text


_24


);


_24


_24


create table capital (


_24


id serial primary key,


_24


name text,


_24


country_id int unique,


_24


foreign key (country_id) references country (id)


_24


);


_24


_24


-- or a primary key on a foreign key


_24


create table country (


_24


id serial primary key,


_24


name text


_24


);


_24


_24


create table capital (


_24


id serial primary key,


_24


name text,


_24


foreign key (id) references country (id)


_24


);


`


Both options should give you a json object when embedding one table with the other.


`
_10


const { data, error } = await supabase


_10


.from('country')


_10


.select('name,capital(name)')


_10


.in('id', \[1, 2\])


_10


_10


console.log(data)


`


noCopy


`
_10


\[


_10


{ "name": "Afghanistan", "capital": { "name": "Kabul" } },


_10


{ "name": "Algeria", "capital": { "name": "Algiers" } }


_10


\]


`


### Computed relationships#


PostgREST uses foreign keys to detect relationships. This poses a problem on database objects that cannot have foreign keys, like views. Though PostgREST tries to infer relationships based on the views’ source tables foreign keys([docs](https://postgrest.org/en/latest/api.html#embedding-views) ), this is not infallible - in particular, it fails when views have a complex definition (e.g. multiple UNIONs). For this you can use “computed relationships”, which are “inlinable” SQL functions similar to computed columns.


Let’s assume we have a` players` view, a` scores` materialized view and we want to define a one-to-many relationship on them.


`
_16


create view players as


_16


select id, name from players_a


_16


union


_16


select id, name from players_b;


_16


_16


create materialized view scores as


_16


select


_16


name as lvl_name,


_16


compute_score(stats) as total, player_id


_16


from level_1;


_16


union


_16


select


_16


name as lvl_name,


_16


compute_score(stats) as total,


_16


player_id


_16


from level_2;


`


For this we can define a couple of computed relationships.


`
_19


-- many-to-one relationship on scores -> players


_19


create function player(scores)


_19


returns setof players rows 1 -- rows 1 defines a "one" end


_19


language sql stable


_19


as $$


_19


select *


_19


from players


_19


where id = $1.player_id;


_19


$$;


_19


_19


-- one-to-many relationship on players -> scores


_19


create function scores(players)


_19


returns setof scores -- there's an implicit rows 1000 here, which is assumed to be "many"


_19


language sql stable


_19


as $$


_19


select *


_19


from scores


_19


where player_id = $1.id;


_19


$$;


`


And now we can embed both views from one end to the other. Note that the function names are arbitrary, here we named them similar to the views for convenience.


`
_28


const { data, error } = await supabase


_28


.from('scores')


_28


.select('lvl_name, player(name)')


_28


.eq('lvl_name', "Grand Prix 1")


_28


.single()


_28


_28


console.log(data)


_28


_28


{


_28


"lvl_name": "Grand Prix 1",


_28


"player": { "name": "Ben Richards"}


_28


}


_28


_28


const { data, error } = await supabase


_28


.from('players')


_28


.select('name,scores(lvl_name, total)')


_28


.eq('id', 1)


_28


.single()


_28


_28


console.log(data)


_28


_28


{


_28


"name":"Ben Richards",


_28


"scores":\[


_28


{"lvl_name": "Grand Prix 1", "total": 48761.24},


_28


{"lvl_name": "Grand Prix 2", "total": -40.25}


_28


\]


_28


}


`


Computed relationships follow the rules of[Inlining of SQL Functions](https://wiki.postgresql.org/wiki/Inlining_of_SQL_functions) , which basically allows them to be injected into PostgREST generated queries, making them efficient to use. You can also use computed relationships to override detected relationships.


### Breaking change on many-to-many relationships#


Detecting join tables for many-to-many relationships has been working for many releases. However on complex schemas join tables can be incorrectly detected, causing errors when used in resource embedding. For this the following BREAKING CHANGE had to be made:


`
_19


-- for "books_authors" to be detected as a join table,


_19


-- the primary key must include the foreign key columns


_19


-- of the many-to-many ends


_19


_19


create table books (


_19


id int primary key,


_19


name text


_19


);


_19


_19


create table books_authors (


_19


book_id int references books (id),


_19


author_id int references authors (id),


_19


primary key (book_id, author_id) -- this is now necessary


_19


);


_19


_19


create table authors (


_19


id int primary key,


_19


name text


_19


);


`


If changing the PK is not feasible for a particular case, the alternative would be using computed relationships to define the many-to-many.


One-to-many and many-to-one relationships keep working as always, no change in their detection.


## Closing up#


Computed relationships are the first step towards PostgREST extensibility, customizing the aggregate used for the response and custom operators are planned for next releases.


We’ll release PostgREST 10 on the Supabase platform over the next month.
