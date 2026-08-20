---
schema_version: "1.0.0"
document_id: "a315eb2a640d0278380a6a8dd2ed74c3d7c4565e627e7f6b5f61a57713e8bee8"
company_key: "yc-emi-labs"
company: "Emi Labs"
source_id: "yc-emi-labs-rss-87232385bc09"
canonical_url: "https://medium.com/@EmiLabsTech/tuning-up-text-pattern-matching-queries-in-sql-82d1bfa46d6c"
published_at: "2023-06-23T17:55:12+00:00"
first_seen_at: "2026-07-27T09:02:34.241036+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:833709752693c52e643ee1cfbe6f53d54e9beb0efa31d20567830f24b41ce89e"
---

# Tuning up text pattern-matching queries in SQL

Sql


Postgres


Optimization


Performance


Database Design


# Tuning up text pattern-matching queries in SQL


[Emi Labs Tech - Ravens](https://medium.com/@EmiLabsTech?source=post_page---byline--82d1bfa46d6c---------------------------------------)


9 min read


·


Jun 23, 2023


--


*Learn how you can optimize text pattern matching queries in SQL using the LIKE operator*


by[Lucas Pereyra](https://medium.com/@lukaspereyra8)


Text-based searching is one of the most well-known headaches software developers often come across. Still, it is a highly relevant capability that almost any software provides as it enables users to do their job efficiently. Finding products by their name or description; looking for users whose email addresses start with a specific value; searching for recipes containing a specific ingredient; are just a few scenarios well covered by text-pattern matching solutions. However, as the patterns and techniques being used get more complex, efficiency tends to degrade, often leading to overlooked performance issues in regard to query execution times.


Several issues might pop up when tackling these challenges, especially when working with SQL databases. In this article, we explore some common text search scenarios and the recommended indexing strategies to address them. A Postgres database instance was used to set up the examples, but they can be seamlessly taken to almost any other SQL engine.


## Initial setup


As previously mentioned, the examples presented were run in Docker using the latest Postgres image. For trying out a different stack, you might need to adjust them accordingly.


To start with, we set up a *product* table to hold all our dummy products:


```text
CREATE TABLE product (      id integer NOT NULL,      name character varying(255),      description text,      quantity integer DEFAULT 0 NOT NULL,      popularity double precision DEFAULT '0'::double precision NOT NULL,      "createdAt" timestamp with time zone NOT NULL,      "updatedAt" timestamp with time zone NOT NULL  );
```


This is a very basic structure, yet it’ll be helpful for explaining the contents of this article. After the table is successfully created, we’ll need to add some fake data to work with. For this example, 1.3M rows were created using the[Faker](https://fakerjs.dev/guide/) package.


## Default indexing behavior


When working with relational databases, if a *primary key* column is specified for a table, the database engine automatically creates an index for it. When searching for a row using such column, the query runs quite smoothly:


```text
postgres=# explain analyze select * from product where id=55;                                                         QUERY PLAN  ------------------------------------------------------------------------------------------------------------------------   Index Scan using product_pkey on product  (cost=0.43..8.45 rows=1 width=169) (actual time=0.034..0.035 rows=1 loops=1)     Index Cond: (id = 55)   Planning Time: 0.105 ms   Execution Time: 0.064 ms
```


Here we can see that the execution plan opts for the *Index Scan* using the automatically generated *product_pkey* index. An **Index Scan** strategy makes use of the binary tree default index to directly look up rows without having to traverse the full table for a row-by-row comparison.


Now, something different happens when searching for another column. Non-primary key columns aren’t indexed by default. This is the case when using the *name* column:


```text
postgres=# explain analyze select * from product where name='Benchmark Test';                                                         QUERY PLAN  -------------------------------------------------------------------------------------------------------------------------   Gather  (cost=1000.00..40687.91 rows=190 width=169) (actual time=94.741..96.488 rows=1 loops=1)     Workers Planned: 2     Workers Launched: 2     ->  Parallel Seq Scan on product  (cost=0.00..39668.91 rows=79 width=169) (actual time=90.270..90.271 rows=0 loops=3)           Filter: ((name)::text = 'Benchmark Test'::text)           Rows Removed by Filter: 440001   Planning Time: 0.077 ms   Execution Time: 96.519 ms
```


The execution plan shows a *Seq Scan* over the whole table, looking for rows that meet the specified criteria. This is a rather slow approach and thus, should be avoided.


## Indexing the ‘name’ column


Let’s try adding an index for the *name* column. We’ll use the default *btree* index, which is highly useful for comparisons using *=, <, >*


```text
postgres=# CREATE INDEX name_btree ON product USING btree (name);
```


The newly added index shows a noticeable improvement in the query execution plan:


```text
postgres=# explain analyze select * from product where name='Benchmark Test';                                                       QUERY PLAN  ---------------------------------------------------------------------------------------------------------------------   Bitmap Heap Scan on product  (cost=5.90..724.89 rows=190 width=169) (actual time=0.053..0.055 rows=1 loops=1)     Recheck Cond: ((name)::text = 'Benchmark Test'::text)     Heap Blocks: exact=1     ->  Bitmap Index Scan on name_btree  (cost=0.00..5.85 rows=190 width=0) (actual time=0.046..0.047 rows=1 loops=1)           Index Cond: ((name)::text = 'Benchmark Test'::text)   Planning Time: 0.371 ms   Execution Time: 0.081 ms
```


Besides the *Execution Time* metric, we should also pay attention to the *cost* metric, which is much lower in this second run. This is a measure of how much effort (in terms of resource utilization, like CPU) it takes to gather the matching rows.


The *Bitmap Index Scan* shows the strategy in use for the query, which replaces a whole table look up by direct index access.


## Issues with pattern-matching queries


The` LIKE` SQL operator is usually the first choice for doing pattern-matching queries. For example, let’s try to find some products matching the *name* column. First, notice that when comparing the column with a constant value, we get the very same behavior as when using the ‘=’ operator:


```text
postgres=# explain analyze select * from product where name like 'Benchmark Test';                                                       QUERY PLAN  ---------------------------------------------------------------------------------------------------------------------   Bitmap Heap Scan on product  (cost=5.90..724.89 rows=190 width=169) (actual time=0.031..0.037 rows=1 loops=1)     Filter: ((name)::text ~~ 'Benchmark Test'::text)     Heap Blocks: exact=1     ->  Bitmap Index Scan on name_btree  (cost=0.00..5.85 rows=190 width=0) (actual time=0.018..0.019 rows=1 loops=1)           Index Cond: ((name)::text = 'Benchmark Test'::text)   Planning Time: 0.189 ms   Execution Time: 0.104 ms
```


But, as soon as we append a` %` to the search value, the index becomes useless. Consequently, Postgres chooses a *Seq Scan* again, ignoring the previously added index.


```text
postgres=# explain analyze select * from product where name like 'Benchmark%';                                                         QUERY PLAN  -------------------------------------------------------------------------------------------------------------------------   Gather  (cost=1000.00..40682.12 rows=131 width=169) (actual time=92.153..93.566 rows=1 loops=1)     Workers Planned: 2     Workers Launched: 2     ->  Parallel Seq Scan on product  (cost=0.00..39669.02 rows=55 width=169) (actual time=89.198..89.199 rows=0 loops=3)           Filter: ((name)::text ~~ 'Benchmark%'::text)           Rows Removed by Filter: 440001   Planning Time: 0.134 ms   Execution Time: 93.585 ms
```


## Using the varchar_pattern_ops to improve query resolution


From the Postgres documentation:


> *The optimizer can also use a B-tree index for queries involving the pattern-matching operators*` LIKE` *and*` ~` ** if *the pattern is a constant and is anchored to the beginning of the string — for example,*` col LIKE 'foo%'` *or*` col ~ '^foo'` *, but not*` col LIKE '%bar'` *. However, if your database does not use the C locale you will need to create the index with a special operator class to support indexing of pattern-matching queries. The optimizer can also use a B-tree index for queries involving the pattern-matching operators*


The *CREATE INDEX* statement allows to specify an *operator class* for the column being indexed. This is often useful when we know the type of comparison operations we’ll perform over the column, and we want Postgres to be aware of them.


From the Postgres documentation:


> *The operator classes*` *text_pattern_ops*` *,*` *varchar_pattern_ops*` *, and*` *bpchar_pattern_ops*` *support B-tree indexes on the types*` *text*` *,*` *varchar*` *, and*` *char*` *respectively.*


For this experiment, let’s create a custom index by specifying the` varchar_pattern_ops` as our operator class:


```text
postgres=# CREATE INDEX name_btree_pattern ON product USING btree (name varchar_pattern_ops);
```


Now, let’s run the same query again to see how it goes:


```text
postgres=# explain analyze select * from product where name like 'Benchmark%';                                                             QUERY PLAN  --------------------------------------------------------------------------------------------------------------------------------   Index Scan using name_btree_pattern on product  (cost=0.43..8.45 rows=131 width=169) (actual time=0.046..0.048 rows=1 loops=1)     Index Cond: (((name)::text ~>=~ 'Benchmark'::text) AND ((name)::text ~<~ 'Benchmarl'::text))     Filter: ((name)::text ~~ 'Benchmark%'::text)   Planning Time: 0.664 ms   Execution Time: 0.071 ms
```


As you can see, it is now faster as the index we’ve just added is used.


The binary tree structure used by the index stores the different *name* values sorting them either in ASC or DESC order. The values stored in a binary tree follow certain sorting criteria that make it easier to resolve queries involving the =, <, > operators. For the` LIKE ‘text%’` queries, this binary tree structure is also useful as it eases gathering results that *start with* a certain constant value. The following picture attempts to show how the binary tree traversing algorithm looks for queries like these:


Moving away from this pattern for *like* queries, the index becomes useless as it has no clues on how to traverse the binary tree. For example, adding a leading` %` to the search value will change the query plan to a full sequential scan, as it otherwise should have to traverse the entire binary tree, as the following image suggests:


Another common example involves using the` ILIKE` expression for case-insensitive searches:


```text
postgres=# explain analyze select * from product where name ilike 'Benchmark%';                                                          QUERY PLAN  ---------------------------------------------------------------------------------------------------------------------------   Gather  (cost=1000.00..40682.12 rows=131 width=169) (actual time=497.313..500.259 rows=1 loops=1)     Workers Planned: 2     Workers Launched: 2     ->  Parallel Seq Scan on product  (cost=0.00..39669.02 rows=55 width=169) (actual time=493.147..493.149 rows=0 loops=3)           Filter: ((name)::text ~~* 'Benchmark%'::text)           Rows Removed by Filter: 440001   Planning Time: 0.604 ms   Execution Time: 500.288 ms
```


The *ilike* statement commands the database not to do a direct comparison anymore. Instead, the values are now compared taking into account the upper/lower case letters.


There’s a special case in which the *ilike* statement seems to work well, though:


```text
postgres=# update product set name ='... Surprise!' where id=44;
```


Running the query we get:


```text
postgres=# explain analyze select * from product where name ilike '.%';                                                             QUERY PLAN  --------------------------------------------------------------------------------------------------------------------------------   Index Scan using name_btree_pattern on product  (cost=0.43..8.45 rows=131 width=169) (actual time=0.040..0.042 rows=1 loops=1)     Index Cond: (((name)::text ~>=~ '.'::text) AND ((name)::text ~<~ '/'::text))     Filter: ((name)::text ~~* '.%'::text)   Planning Time: 0.423 ms   Execution Time: 0.069 ms
```


From the Postgres documentation:


> *It is also possible to use B-tree indexes for*` *ILIKE*` *and*` *~**` *, but only if the pattern starts with non-alphabetic characters, i.e., characters that are not affected by upper/lower case conversion.*


## Underperforming pattern-matching queries


As we’ve seen before, queries with a leading` %` cannot be efficiently resolved using the binary tree index. Hence, the database triggers a *Seq Scan* operation to look for matching results. Let’s confirm it with an example:


```text
postgres=# explain analyze select * from product where name like '%Test';                                                         QUERY PLAN  -------------------------------------------------------------------------------------------------------------------------   Gather  (cost=1000.00..40682.12 rows=131 width=169) (actual time=102.291..103.891 rows=1 loops=1)     Workers Planned: 2     Workers Launched: 2     ->  Parallel Seq Scan on product  (cost=0.00..39669.02 rows=55 width=169) (actual time=99.521..99.523 rows=0 loops=3)           Filter: ((name)::text ~~ '%Test'::text)           Rows Removed by Filter: 440001   Planning Time: 0.207 ms   Execution Time: 103.925 ms
```


As you might have guessed, the very same behavior applies for` LIKE ‘%text%’` queries:


```text
postgres=# explain analyze select * from product where name like '%Benchmark%';                                                          QUERY PLAN  ---------------------------------------------------------------------------------------------------------------------------   Gather  (cost=1000.00..40682.12 rows=131 width=169) (actual time=131.009..132.523 rows=1 loops=1)     Workers Planned: 2     Workers Launched: 2     ->  Parallel Seq Scan on product  (cost=0.00..39669.02 rows=55 width=169) (actual time=126.693..126.695 rows=0 loops=3)           Filter: ((name)::text ~~ '%Benchmark%'::text)           Rows Removed by Filter: 440001   Planning Time: 0.119 ms   Execution Time: 132.554 ms
```


Finally, queries with the` ILIKE` operator also fall into this category, as they hinder direct text comparisons so that no binary tree traversing strategies can be used. This is true even if we avoid using leading` %` in the search value:


```text
postgres=# explain analyze select * from product where name ilike 'Benchmark%';                                                          QUERY PLAN  ---------------------------------------------------------------------------------------------------------------------------   Gather  (cost=1000.00..40682.12 rows=131 width=169) (actual time=477.563..479.202 rows=1 loops=1)     Workers Planned: 2     Workers Launched: 2     ->  Parallel Seq Scan on product  (cost=0.00..39669.02 rows=55 width=169) (actual time=473.911..473.911 rows=0 loops=3)           Filter: ((name)::text ~~* 'Benchmark%'::text)           Rows Removed by Filter: 440001   Planning Time: 0.294 ms   Execution Time: 479.247 ms
```


## Main takeaways


- Default binary tree indexes are useful when comparing text columns with the operators =, <, >. However, these indexes quickly become useless for pattern-matching queries, like those using` LIKE` expressions.
- In such cases, the special` varchar_pattern_ops` operator class allows us to define a special index type, which optimizes pattern-matching expressions. Search values with a leading` %` hinder using this index as the database chooses a *Seq Scan* instead. This also applies to` ILIKE` queries.
- The less we vary the leading part of the search value, the faster the query is resolved. Prefer queries with` LIKE ‘text%'` patterns, as they exploit the binary tree index by using the leading text as a hint to traverse it more efficiently.


## The end


Press enter or click to view image in full size


“The End” (scrabble lettering)


Text-pattern matching queries are a powerful mechanism that allows us to easily locate the data we need using human-friendly inputs. Yet, they should not be overlooked when tackling performance issues. Most relational databases provide a` LIKE` (or similar) statement, along with some indexing strategies or plugins that can be used to make pattern-matching queries smoother. It is our responsibility, however, to encourage the use of best practices and recommended approaches when making the final implementation decisions.


At Emi, we’re always learning how to get the best out of our tools and techniques. Recognizing there’s always room to continue learning and improving keeps us strongly motivated to try, fail and grow as we move on. If you enjoyed reading this article, please consider giving thumbs-up and sharing it! Also, stay connected for more interesting content coming up soon!
