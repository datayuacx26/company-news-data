---
schema_version: "1.0.0"
document_id: "0554f0310b6ded27109ad500db1a62e84fc29dc6b7681a0047fd818094970f55"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/cracking-postgres-interview"
published_at: "2021-02-27T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:9fa39ad9e5bf81d6e02a1f0034c5d6b55c69f54437effa8a7b2e77eccc6b43ee"
---

# Cracking PostgreSQL Interview Questions

There are plenty of resources out there for preparing for PostgreSQL interview questions. Most posts are for technical interviews with a focus on PostgreSQL, however many just cover the basics and the advanced resources often conflate transactional SQL with analytical SQL (WINDOW/RANK functions, aggregates etc.).


Here, we're going to focus on PostgreSQL interview questions that are aimed to understand the transactional side of PostgreSQL, and offer some areas that you may want to go a little deeper on in order to really impress your interviewer (and more importantly, become a kick-ass software engineer).


## 1. Modeling#


### Know how to model 1-M, M-M, 1-1 relationships. And know how to use foreign keys.#


A review of[database normalization](https://ocw.mit.edu/courses/civil-and-environmental-engineering/1-264j-database-internet-and-systems-integration-technologies-fall-2013/lecture-notes-exercises/MIT1_264JF13_lect_10.pdf) is a great place to start when thinking about how to correctly model relationships. However if you don't have the time to read through lecture notes, head over to[DBDesigner](https://app.dbdesigner.net/designer/schema/guest_template) and inspect their example schema. The table StudentCourses is a great example of how to model a Many-to-Many relationship, by using a join table. (side note: you can export these visual schemas to SQL using Ctrl+E). Modelling your data correctly is arguably the most important part of any software project, writing applications becomes a breeze if you can get the data layer right.


### Know how to use pg rich type system:[arrays](https://www.postgresql.org/docs/current/arrays.html) ,[domains](https://www.postgresql.org/docs/current/domains.html) ,[JSONB](https://www.postgresql.org/docs/13/datatype-json.html) ,[timestamptz](https://www.postgresql.org/docs/current/functions-datetime.html) ,[enums](https://www.postgresql.org/docs/13/datatype-enum.html)#


Postgres has tons of useful types beyond the basics, knowing how to use them will show you can leverage the true power of Postgres. JSONB for example can be incredibly useful for storing non-structured data, which you can query using syntax like:


`
_10


-- grades = {'geography': 'A', 'history': 'B', 'postgres': 'A++'}


_10


select * from students where grades->>'geography' = 'A';


`


### Know about namespacing with SCHEMAs#


In Supabase for example we keep system schemas such as` extensions` and` auth` in separate schemas so that we don't pollute the default` public` schema.


## 2. INDEXes#


### Know how speed up queries with indexes.#


The art of indexing in Postgres could fill an entire book. In some circumstances it can happen that a bad index is worse for performance than no index, so it's worth spending a little time to learn some of the common strategies.


An index can be simple, for example, if your students table is most frequently queried on surname alone, you create an index:


`
_10


create index idx_students_surname on students (surname);


`


The default index type used here is` btree` (you could have specified this as` USING btree` ), but there are other types of indexes, such as` BRIN` ,` GiST` ,` GIN` ,` hash` , and more. Readers wanting to go deeper may also want to explore[Partial](https://www.postgresql.org/docs/current/indexes-partial.html) or[Multicolumn](https://www.postgresql.org/docs/13/indexes-multicolumn.html) Indexes.


### Know how to analyze with EXPLAIN ANALYZE#


Running


`
_10


EXPLAIN (ANALYZE) SELECT *


_10


FROM students


_10


WHERE surname = 'Krobb';


`


Before and after adding your index will show you the difference in approach the query planner took to finding your data. Note that using EXPLAIN alone will give us estimated plan costs. When used together with ANALYZE like:` EXPLAIN (ANALYZE)` you will receive both estimated and actual costs.


## 3. VIEWs#


### Know how to create different representations of data with[VIEWs](https://supabase.com/blog/postgresql-views) .#


We might create a VIEW` transcripts` which pulls out data from` students` ,` courses` , and` grades` . It's useful for security, and logical abstractions. Check out our longer post on VIEWs here:[/blog/postgresql-views](https://supabase.com/blog/postgresql-views) . Some purists may argue that you should always query VIEWs and never TABLEs.


### Know about Autoupdatable views.#


If a VIEW is named as the target relation in an INSERT, UPDATE, or DELETE and only SELECTs from a single base relation, then the underlying subquery is automatically rewritten to update the underlying base relation instead.


### Limitations on VIEWs#


One example of a limitation is when a VIEW is not Autoupdatable. This happens when the VIEW does not SELECT from a single base relation. If the user does not specify an INSTEAD OF trigger that upgrades the underlying query, then an error will be thrown, since the executor cannot update a view as such.


## 4. ROLEs#


### Know how to secure their database. Permissions at the table, column, row level.#


All databases have different user types, your client for example doesn't usually need the ability to create and drop schemas, but your DB admin does. You should play around with[creating roles](https://www.postgresql.org/docs/current/database-roles.html) , and[granting](https://www.postgresql.org/docs/current/role-membership.html) various permissions.


### Know about[ROLEs](https://www.postgresql.org/docs/13/sql-createrole.html) , application ROLEs, the PUBLIC role, and[GRANTs](https://www.postgresql.org/docs/current/sql-grant.html)#


In Postgres, the special “role” name PUBLIC can be used to grant a privilege to every role on the system. For example, if you want to grant insert access to all users on table students:


`
_10


GRANT INSERT ON students TO PUBLIC;


`


### Know how to do RLS - Policies#


We use Row Level Security in Supabase as a way to grant/restrict access on a row level basis. For example if you're writing a Discord clone, perhaps only a given user should be able to write their own messages:


`
_10


CREATE POLICY "Individuals can only write their own messages." ON messages FOR


_10


INSERT WITH CHECK ((select auth.uid()) = user_id);


_10


_10


-- auth.uid() is a function provided by Supabase which plucks the uid out


_10


-- of the JWT sent along with an API request more on this here:


_10


-- https://www.youtube.com/watch?v=0LvCOlELs5U


`


## 5. FUNCTIONs#


### Know how to do business logic on SQL/[PLPGSQL](https://www.postgresql.org/docs/current/plpgsql-overview.html#PLPGSQL-ADVANTAGES)#


PL/pgSQL is a procedural programming language that can be used to write functions inside of your database. It can be useful for making[remote procedure calls](https://supabase.com/docs/reference/javascript/rpc) from an API. You can go as deep as you want here, since it's an entire programming language, but understanding the basics will really go a long way, and give you super powers when working with your data.


You can use FUNCTIONs in combination with TRIGGERs to do cool stuff like have auto-updating` updated_at` columns on your data:


`
_14


-- a function that sets the updated_at value to now()


_14


CREATE FUNCTION set_updated_at()


_14


RETURNS TRIGGER AS $$


_14


BEGIN


_14


new.updated_at = now();


_14


RETURN NEW;


_14


END;


_14


$$ LANGUAGE plpgsql;


_14


_14


-- a trigger that fires when students table is updated


_14


CREATE TRIGGER handle_updated_at


_14


BEFORE UPDATE ON students


_14


FOR EACH ROW


_14


EXECUTE PROCEDURE set_updated_at();


`


### Thinks in SETs when doing logic#


Whilst PL/pgSQL does have[loops](https://www.postgresql.org/docs/current/plpgsql-control-structures.html#PLPGSQL-CONTROL-STRUCTURES-LOOPS) and[cursors](https://www.postgresql.org/docs/current/plpgsql-cursors.html) , there is usually a faster and more legible pure SQL based solution available using JOIN/UNION etc. So it's important to become well acquainted with thinking in these terms.


### Know how to use[CTEs](https://www.postgresqltutorial.com/postgresql-cte/)#


Common table expressions are temporary or intermediate result sets. They can make your queries more readable and even enable recursion. The typical form is:


`
_10


with ten_strumpers as (


_10


select id, first_name


_10


from students


_10


where surname = 'Strumper'


_10


order by first_name


_10


limit 10


_10


)


_10


select id


_10


from ten_strumpers


_10


where first_name like "S%";


`


If you can reason about most of the topics in this post then you'll be in a very strong position to impress with your answers to PostgreSQL interview questions. As with all programming topics however, the real learning starts when you put these things into practice. At Supabase we offer a very very fast (the fastest?) way to spin up a PostgreSQL database and start querying it, and our browser based SQL editor is getting more powerful every day.


[Get started on Supabase for free here](https://supabase.com/dashboard)


Thanks Steve Chavez for providing all the good bits of this post :)


## More Postgres resources#


- [Implementing "seen by" functionality with Postgres](https://supabase.com/blog/seen-by-in-postgresql)
- [Partial data dumps using Postgres Row Level Security](https://supabase.com/blog/partial-postgresql-data-dumps-with-rls)
- [Postgres Views](https://supabase.com/blog/postgresql-views)
- [Postgres Auditing in 150 lines of SQL](https://supabase.com/blog/audit)
- [What are PostgreSQL Templates?](https://supabase.com/blog/postgresql-templates)
- [Realtime Postgres RLS on Supabase](https://supabase.com/blog/realtime-row-level-security-in-postgresql)
