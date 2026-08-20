---
schema_version: "1.0.0"
document_id: "58c0efb11df82db8512c1d1f21f190115b6e5e419ebd84c55a0350b80efcb1dd"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/database-table-joins-with-and-without-foreign-key-constraints/"
published_at: "2023-01-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:ceb8e6a417b35373dd6893224597aae7073d8e2f7a61d7a68bc5b7a351c8b969"
---

# Database table joins with and without foreign-key constraints

In a well-structured relational database, different kinds of items are kept clearly separated from each other, as records in different database tables. Conceptually, each table forms a different compartment, and it’s important to keep things strictly compartmentalized. At the same time though, items of different types are not necessarily independent, they often relate to one another, and related records can be connected by one record storing the identifier of another.


It’s all about finding the balance between keeping items separated according to their type, while allowing for joining separate items together according to their relationships.


In this post we’ll look in detail at the joining part, and what’s going on with the famous` JOIN` operator in SQL.


## The simplest example


Say you work with books, and you work with authors. Let’s look at a classic way to structure a relational database.


### Conceptually


- We want to cleanly separate information about books (year published, title, price, size, page count, etc.) from information about authors (name, date of birth, place of residence, etc.). These things don’t really have anything to do with one another.
- But, at the same time there is a key relationship to define: that is, a book has a specific author. (For the sake of simplicity, assume that any book has just one author).


### Concretely


- We setup two separate tables, called` authors` and` books` , each with columns specific to the makeup of each.
- We define a unique identifier for records on each table (an` id` column which stores an integer). We add a` PRIMARY KEY` constraint to this` id` column to tell the database that it will be what uniquely identifies a record on each table.


- This primary-key constraint ensures that multiple records can’t have the same ID. If you try and create a record and give it an ID that’s already in use, the database won’t let you.


- We add an important column on the` books` table, say` books.author_id` which will be for storing the identifier of a specific record on the` authors` table. We can say “a book has one author”.
- We add a` FOREIGN KEY` constraint to the` books.author_id` column, which tells the database that this column should contain the identifier of a record on the` authors` table.


- This foreign-key constraint ensures that the ID stored in the` books.author_id` column, actually points to a specific record on the` authors` table. For a given book, if you try and put an ID in the` books.author_id` column that doesn’t correspond to any author in the` authors` table, the database won’t let you.
- Think of the term “foreign key” as saying, it’s the key (the unique identifier) for getting a certain record from a foreign table (another table that’s not the table in question).


Here’s what all this looks like in SQL:


```text
CREATE   TABLE   authors   (
id   int   not null  ,
PRIMARY KEY   (id),
name   varchar  (  255  ),
city   varchar  (  255  )
);


CREATE   TABLE   books   (
id   int   not null  ,
PRIMARY KEY   (id),
title   varchar  (  255  ),
author_id   int  ,
FOREIGN KEY   (author_id)   REFERENCES   authors(id)
);
```


Now that we’ve created the` authors` and` books` table, we create some records for each with the following SQL and we’ll have our database going with records for our two tables.


```text
INSERT INTO   authors (id,   name  , city)   VALUES
(  1  ,   'Ernest Hemingway'  ,   'Havana, Cuba'  ),
(  2  ,   'George Orwell'  ,   'Greenwich, England'  ),
(  3  ,   'Ken Kesey'  ,   'Pleasant Hill, Oregon'  );


INSERT INTO   books (id, title, author_id)   VALUES
(  1  ,   'Nineteen Eighty-Four'  ,   2  ),
(  2  ,   'Sometimes a Great Notion'  ,   3  ),
(  3  ,   'For Whom the Bell Tolls'  ,   1  );
```


### What it looks like in Basedash


With the above database going, let’s see what we would see if we connect it to Basedash.


First, if you’re an admin in Basedash, you could browse to the` authors` table under your SQL connection:


You can see the records on the table, and from this interface you can also create, delete, and update records. Note the column names in the table header, next to the name of each column is also an icon indicating the data type (int for` id` , text for` name` , and also text for` city` ).


Then, if you browse to the` books` table, here’s what you’d see:


Same as before, but now look at the` author_id` column. It has an icon indicating that it contains a foreign-key (the identifier of a specific record on another table, in this case the` books` table). Also notice that in this column we don’t see ID numbers in the cells. Instead, for convenience, Basedash renders a label representing the specific record which is pointed-to. (If need be, when you hover over the label you can see the ID number.)


Next, if you’re an admin in Basedash, you could create a query on the` authors` and` books` table with a` JOIN` clause like so:


```text
SELECT
books  .  title  ,
authors  .  name
FROM   books
JOIN   authors
WHERE   books  .  author_id   =   authors  .  id
```


And what you get would be authors and books matched up, allowing you to see values from each table brought together. We’re selecting values from “books joined with authors”. Note the` WHERE` clause which specifies that the join be done with the columns on which we earlier defined the foreign-key relationship.


And here’s what you’d see when you run that query in Basedash:


And that’s the magic moment, leveraging the relations between tables to see a complete picture. Classic relational database setup and usage.


### But wait, there’s more with Basedash


Since we setup the` books.author_id` column with a foreign-key constraint pointing to the` authors.id` column, if you’re an admin in Basedash you could create a view of the` books` table, with the` authors` table joined, like so:


In the view builder interface, there’s a section for adding joined tables, and you can select any available tables to join. Basedash detects that there’s a foreign-key constraint on the` author_id` column pointing to the` authors` table, and so it presents the option to join the` authors` table onto the view.


Then with the join in place, you would be able to build the view and make use of columns from the base table and any joined tables as well.


In the above screenshot, we select the` books.title` column and the` authors.name` column to appear in the view. Note how the Basedash UI presents the table name next to the column name once you’re working in a view with one or more joins.


## The same example, with a notable omission


Imagine that we do the same as above, but when creating our` authors` and` books` table we neglect to specify the` FOREIGN KEY` constraint like so:


```text
CREATE   TABLE   authors   (
id   int   not null  ,
PRIMARY KEY   (id),
name   varchar  (  255  ),
city   varchar  (  255  )
);


CREATE   TABLE   books   (
id   int   not null  ,
PRIMARY KEY   (id),
title   varchar  (  255  ),
author_id   int  ,
//   No   FOREIGN KEY   constraint   added here
);
```


And then imagine, we proceeded to Basedash. We would have pretty much the same thing. The` authors` table is exactly the same as before, but the` books` table as a subtle difference. The` author_id` column wouldn’t have the foreign-key icon, and its contents would be just the plain numbers stored on that table. And there’d be no label showing a representation of a specific foreign record:


### Joining without a foreign-key constraint


But perhaps we proceed nonetheless. We might write the same SQL query as before. In fact, the same SQL query would work, we’d get the same result:


Note the query and the` WHERE` clause are exactly the same. In SQL you can perform` JOIN` s whether or not there’s a foreign-key constraint. All that matters is that the data types match up (in this case integers). So be careful, just because` WHERE books.author_id = authors.id` reads well, it doesn’t mean necessarily that there’s an explicit relationship, backed by an in-tact foreign-key constraint on the database.


If we proceed to create a view based on the` books` table, we would not have the option to join the` authors` table:


In the above screenshot, note that the joined tables section is then missing from the view builder. Basedash detects foreign-key constraints on your database, and the view builder only supports joining tables via columns that are backed by foreign-key constraints. (In the future we plan to support to the view builder for “manual” or arbitrary joins on columns not necessarily having foreign-key constraints, giving you more of the power that you get from writing SQL queries).


### Just because you can join, doesn’t mean you should


Ok, so seeing that in SQL there’s nothing stopping you from writing a` JOIN` that’s not backed by a foreign-key constraint, consider this wacky example. Imagine that in our database we also setup a table called` grocery_list_items` . We could use this table as our shopping list. Here’s how we could create and populate it in SQL:


```text
CREATE   TABLE   grocery_list_items   (
id   int   not null   auto_increment,
PRIMARY KEY   (id),
product   varchar  (  255  ),
quantity   int
);


INSERT INTO   grocery_list_items (product, quantity)   VALUES
(  'Coffee'  ,   1  ),
(  'Bananas'  ,   3  ),
(  'Grapefruit'  ,   2  ),
(  'Milk'  ,   1  ),
(  'Eggs'  ,   12  ),
(  'Broccoli'  ,   4  );
```


And then here’s what our newly-created` grocery_list_items` table would look like when we browse to it in Basedash:


Wouldn’t you know? The` quantity` column contains integers. Does this give you any ideas? What other columns in other tables in our database contain integers? How about the` author_id` column on the` books` table? Why not write a query like so:


```text
SELECT
books  .  title  ,
grocery_list_items  .  product
FROM   books
JOIN   grocery_list_items
WHERE   books  .  author_id   =   grocery_list_items  .  quantity
```


And here’s what we’d see in Basedash:


We see for example, that we get “For Whom the Bell Tolls” paired with coffee, and “Sometimes a Great Notion” paired with bananas. Wild! Also note that “For Whom the Bell Tolls” appears twice. The second time its paired with milk. This should give you an idea of the wackiness you can get when joining arbitrary tables without being thoughtful.


Imagine that we enrich our query a bit like so, so that we can see the matching of integer values that is taking place:


```text
SELECT
books  .  title   as   'Book title'  ,
grocery_list_items  .  product   as   'Grocery list item'  ,
books  .  author_id   as   'Author ID'  ,
grocery_list_items  .  quantity   as   'Grocery item quantity'
FROM   books
JOIN   grocery_list_items
WHERE   books  .  author_id   =   grocery_list_items  .  quantity
```


When we run the query in Basedash we would indeed see the integer values match up:


Even though this particular example is nonsensical, do note, however, that there are many cases where performing` JOIN` s (of all different kinds) on arbitrary tables is legitimate, assuming the relationship is a meaningful one. Joins in SQL are not limited to foreign-key constrained columns for good reason, there’s many cases where the full power of arbitrary joins is essential for working with databases. This example is meant to show though, that not all joins make sense.


## Bonus: checking on foreign-key constraints via SQL


Let’s say you aren’t the database admin, and you weren’t around when the tables in question were set up, nor do you have access to internal documents on the database structure, nor are you setup with a tool for administering your database and seeing its structure that way. But let’s say you do have the ability to write queries against the database using SQL. If you know the right recipe for the database dialect in question, tou can find out whether there is a foreign-key constraint present on a given column with a plain SQL query.


Here, for example is what that query would look like for our example database, assuming that we’re working with the MySQL dialect:


```text
SELECT
TABLE_NAME,COLUMN_NAME,CONSTRAINT_NAME, REFERENCED_TABLE_NAME,REFERENCED_COLUMN_NAME
FROM
INFORMATION_SCHEMA  .  KEY_COLUMN_USAGE
WHERE
REFERENCED_TABLE_SCHEMA   =   'basedash'   AND
REFERENCED_TABLE_NAME   =   'authors'  ;
```


> From:[https://stackoverflow.com/a/201678/15487978](https://stackoverflow.com/a/201678/15487978)


And if you run that query in Basedash here’s what you’d see:


In this way you can verify the presence or absence of a foreign-key constraint with a quick query. Recipes for other database dialects, apart from MySQL can readily be found online too.
