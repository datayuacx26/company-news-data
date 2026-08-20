---
schema_version: "1.0.0"
document_id: "160cedeb23af2dc64e80fb248090157579ac1512f2f670652e5db3180f68ecee"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgresql-views"
published_at: "2020-11-18T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:7ea32b55405c67f15a7d6386aecc65118a96507ef9759b87d558f1951d086439"
---

# Postgres Views

A quick summary of Postgres views, materialized views, and why you should use them.


## What is a view?#


A[view](https://www.postgresql.org/docs/12/sql-createview.html) is a convenient shortcut to a query. Creating a view does not involve new tables or data. When run, an underlying query is executed, returning its results to the user.


### Basic example#


Say we have the following tables from a database of a university:


**students**


id name type


1 Arun undergraduate


2 Zack graduate


3 Joy graduate


**courses**


id title code


1 Introduction to Postgres PG101


2 Authentication Theories AUTH205


3 Fundamentals of Supabase SUP412


**grades**


id student_id course_id result


1 1 1 B+


2 1 3 A+


3 2 2 A


4 3 1 A-


5 3 2 A


6 3 3 B-


Creating a view consisting of all the three tables will look like this:


`
_11


create view transcripts as


_11


select


_11


students.name,


_11


students.type,


_11


courses.title,


_11


courses.code,


_11


grades.result


_11


from


_11


grades


_11


left join students on grades.student_id = students.id


_11


left join courses on grades.course_id = courses.id;


`


Once done, we can now access the underlying query with:


`
_10


select * from transcripts;


`


For additional parameters or options, refer[here](https://www.postgresql.org/docs/12/sql-createview.html#:~:text=parameters) .


## Why should we use views?#


Views provide the several benefits:


- Simplicity
- Consistency
- Logical Organization
- Security


### Simplicity#


As a query becomes complex it becomes a hassle to call it. Especially when we run it at regularly. In the example above, instead of repeatedly running:


`
_10


select


_10


students.name,


_10


students.type,


_10


courses.title,


_10


courses.code,


_10


grades.result


_10


from


_10


grades


_10


left join students on grades.student_id = students.id


_10


left join courses on grades.course_id = courses.id;


`


We can run this instead:


`
_10


select * from transcripts;


`


Additionally, a view behaves like a typical table. We can safely use it in table` JOIN` s or even create new views using existing views.


### Consistency#


Views ensure that the likelihood of mistakes decreases when repeatedly executing a query. In our example above, we may decide that we want to exclude the course *Introduction to Postgres* . The query would become:


`
_11


select


_11


students.name,


_11


students.type,


_11


courses.title,


_11


courses.code,


_11


grades.result


_11


from


_11


grades


_11


left join students on grades.student_id = students.id


_11


left join courses on grades.course_id = courses.id


_11


where courses.code != 'PG101';


`


Without a view, we would need to go into every dependent query to add the new rule. This would increase in the likelihood of errors and inconsistencies, as well as introducing a lot of effort for a developer. With views, we can alter just the underlying query in the view **transcripts** . The change will be applied to all applications using this view.


### Logical organization#


With views, we can give our query a name. This is extremely useful for teams working with the same database. Instead of guessing what a query is supposed to do, a well-named view can easily explain it. For example, by looking at the name of the view **transcripts** , we can infer that the underlying query might involve the **students** , **courses** , and **grades** tables.


### Security#


Views can restrict the amount and type of data presented to a user. Instead of allowing a user direct access to a set of tables, we provide them a view instead. We can prevent them from reading sensitive columns by excluding them from the underlying query.


## What is a materialized view?#


A[materialized view](https://www.postgresql.org/docs/12/rules-materializedviews.html) is a form of view but with the added feature of physically storing the results. In subsequent reads of a materialized view, the time taken to return its results would be much faster than a conventional view. This is because the data is readily available for a materialized view while the conventional view executes the underlying query each time it is called.


### Basic example#


Using our example above, a materialized view can be created like this:


`
_11


create materialized view transcripts as


_11


select


_11


students.name,


_11


students.type,


_11


courses.title,


_11


courses.code,


_11


grades.result


_11


from


_11


grades


_11


left join students on grades.student_id = students.id


_11


left join courses on grades.course_id = courses.id;


`


Reading from the materialized view is the same as a conventional view:


`
_10


select * from transcripts;


`


For additional parameters or options, refer[here](https://www.postgresql.org/docs/12/sql-creatematerializedview.html#:~:text=parameters) .


### Refreshing#


Unfortunately, there is a trade-off - data in materialized views are not always up to date. We need to refresh it regularly to prevent the data from becoming too stale. To do so:


`
_10


refresh materialized view transcripts;


`


It's up to you how regularly refresh your materialized views, and it's probably different for each view depending on its use-case.


## Materialized views vs conventional views#


Materialized views are useful when execution times for queries or views become unbearable or exceed the service level agreements of a business. These could likely occur in views or queries involving multiple tables and millions of rows. When using such a view, however, there should be tolerance towards data being outdated. Some use-cases for materialized views are internal dashboards and analytics.


Creating a materialized view is not a solution to inefficient queries. You should always seek to optimize a slow running query even if you are implementing a materialized view.


## Conclusion#


Postgres views and materialized views are a great way to organize and view results from commonly used queries. Although similar to one another, each has its purpose. Views simplify the process of running queries. Materialized views are usually faster at returning results, with the trade-off of having stale data.


## More Postgres resources#


- [Implementing "seen by" functionality with Postgres](https://supabase.com/blog/seen-by-in-postgresql)
- [Partial data dumps using Postgres Row Level Security](https://supabase.com/blog/partial-postgresql-data-dumps-with-rls)
- [Postgres Auditing in 150 lines of SQL](https://supabase.com/blog/audit)
- [Cracking PostgreSQL Interview Questions](https://supabase.com/blog/cracking-postgres-interview)
- [What are PostgreSQL Templates?](https://supabase.com/blog/postgresql-templates)
- [Realtime Postgres RLS on Supabase](https://supabase.com/blog/realtime-row-level-security-in-postgresql)
