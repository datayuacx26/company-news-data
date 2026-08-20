---
schema_version: "1.0.0"
document_id: "22ae4f0d78d76330cc5926b497e3f13c0e928b5f80ab413b743bf8d7a11ec561"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/2024/03/11/sql-select-statement-best-practices/"
published_at: "2024-03-11T00:00:00+00:00"
first_seen_at: "2026-07-23T21:47:49.914954+00:00"
fetched_at: "2026-07-28T22:26:08.375343+00:00"
content_hash: "sha256:934a4f0e69794b8162c611ce8911b3d6b5d65dc903110de59205a07f82baca3c"
---

# Maximize your SQL efficiency: SELECT best practices

QuestDB is the open-source time-series database for demanding workloads—from trading floors to mission control. It delivers ultra-low latency, high ingestion throughput, and a multi-tier storage engine. Native support for Parquet and SQL keeps your data portable, AI-ready—no vendor lock-in.


---


SQL is wildly popular. Within it, the most common command is the SELECT statement. After all, what good is a database full of valuable information if you're unable to read it with flexibility and efficiency?


This guide walks you through best SELECT practices. Afterwards, you'll write more efficient and powerful SQL statements. This can save both processing time and resources, leading to faster queries and cheaper databases.


While many of the optimization tips would make sense universally across any SQL database, the statements we show are specific for QuestDB and many include its robust time-series extensions.


If you don't have a database of your own running, consider launching these queries into the[QuestDB demo instance](https://demo.questdb.io/) . The example queries leverage the built in data sets when possible.


## Apply columnar specificity


> **Read only the columns you need and avoid doing select *.**


The first one may seem like common sense, but searching GitHub for` SELECT * FROM` returns 5.7 million results. Surely a great number of these are only looking for a column or two. Specifying a column vs. all columns narrows down the result set. This reduces data transfer and lightens the load on memory.


Specify - or "scope" - specific columns, if all you need are specific columns:


```text
SELECT fare_amount from trips;
```


Scoping your columns is always a wise idea. But where it shines most is within databases that support the[columnar database format](https://questdb.com/glossary/columnar-database/) , such as QuestDB.


## Always filter by designated timestamp


Designated timestamps are a very powerful feature. With timestamps, the database quickly locates the initial data point that is relevant to the query.


QuestDB optimizes query execution by physically organizing data in ascending order based on a designated timestamp and partitioning this data according to a predefined resolution.


This minimizes random access operations, instead favouring sequential access. Sequential access is preferred by both the operating system and the storage drive, leading to faster and more efficient data retrieval.


> **If possible, always filter by the designated timestamp.**


Instead of returning "all time", we return only the time we need. Since data is stored in chronological order, a database like QuestDB will know when to stop reading as soon it sees a value later than the timestamp boundary. As a result, it can immediately return results:


```text
SELECT price, timestamp FROM trades   WHERE timestamp BETWEEN '2022-03-08T08:59:59' AND '2022-03-08T18:59:59';
```


## Optimize partitions


Partitions split databases into smaller chunks. Time is one convenient way to partition a database. But consider that your partitions are very large: a YEAR, for example.


> Learn more about[database partitioning](https://questdb.com/glossary/database-partitioning/) .


Your queries may often apply a short scope, a day, or hours. In this common case, you are accessing a large chunk of un-needed data. This is inefficient.


Consider the following query to against a table partitioned by YEAR:


```text
SELECT prices, timestamp   FROM trades   WHERE timestamp >= '2022-03-08T00:00:00' AND timestamp < '2022-03-09T00:00:00';
```


Looks OK. But because of the underlying partitioning, we need to read - and discard - many rows until we find the start of the time range, which is in August. If the table was instead partioned by MONTH, then we would read/discard a much smaller amount of data, even if we end up opening more files as a result of spanning two partitions.


Try to find the sweet spot based on your most typical queries.


## Divide tables to match query patterns


Building off of partition precision, consider matching your tables to your query types. Apart from matching the timestamps up with an appropriately broad partition, it is strong practice to match tables up with query types.


> **Divide large single tables into multiple tables that match query types.**


Consider dividing a large table into multiple tables if you have different query patterns for different columns in the query. Maybe some columns you typically need to query daily and some monthly. Table makeup can reflect this.


## JOINs are expensive


While many tables are appropriate in some cases, a singular table may be the right choice if you often find yourself applying JOIN to your queries. A database query that JOINs together many tables is typically more expensive than a similar query that reads from a single table. If different columns are frequently queried together, then a single table is preferred.


Let's use an example query:


```text
SELECT       pickup_datetime,       fare_amount,       tempF,       windDir   FROM       trips   ASOF JOIN weather   WHERE       trips.pickup_datetime >= '2018-06-01T00:00:00'   AND       trips.pickup_datetime < '2018-06-02T00:00:00';
```


The above query is a two-table join. Not bad. Efficient and clean.


However, what if we added four additional tables: drivers, vehicles, and payment_info. In this query, t, w, d, v, p, tc are table aliases used for clarity and brevity.


```text
SELECT       t.pickup_datetime,       t.fare_amount,       w.tempF,       w.windDir,       d.driver_name,       d.driver_rating,       v.vehicle_make,       v.vehicle_model,       p.payment_method,       p.amount_paid,   FROM       trips t   ASOF JOIN weather w   JOIN drivers d ON (driver_id)   JOIN vehicles v ON(vehicle_id)   LEFT OUTER JOIN payment_info p ON (transaction_id)   WHERE       t.pickup_datetime >= '2022-03-08T00:00:00' AND t.pickup_datetime < '2022-03-09T00:00:00';
```


This query is much more difficult to process.


If these were all part of a singular trip_details table, it would be that much more efficient.


## Match tables up with symbols


A symbol is a data structure used to store repetitive strings. Within the database itself, symbol types are stored as a table of integers and their corresponding string values.


Using symbols can improve query performance by a large margin, as string operations compare and write int types instead of string. As an additional benefit, less storage space is consumed. They're also unobtrusive to the query writer (you!) and offer reduced overall complexity.


If you often query by timestamp plus a symbol, like type, country, model and so on, you may be able to find additional performance through the creation of multiple tables. Typically, one table per different symbol value would provide a boost.


Creating a table with a column named symbol


```text
CREATE TABLE symbol_table     (symbol SYMBOL CAPACITY 128 NOCACHE, timestamp TIMESTAMP)   timestamp(timestamp);
```


However do note that if you needed to query two different symbol values, you would need to apply a` UNION` , so make sure the trade off is right for you. Applying` UNION` merges the results of 2 different queries:


Union to merge two query results


```text
symbol1_table   UNION   symbol2_table;
```


Also, if the number of symbols is in the thousands and you create a different table for each, there will be extra memory & disk consumption. In that case, you may find that you need to manually tune configuration, or perhaps just keep using a single table.


## EXPLAIN your queries first


Use EXPLAIN to see how queries will execute.


Depending on which column types you are filtering by, or which aggregation functions you are using, queries might parallelise or not.


To demonstrate, let's deconstruct the trades table in the[demo instance](https://demo.questdb.io/) .


It is created like so:


```text
CREATE TABLE trades (     symbol SYMBOL CAPACITY 256 CACHE,     side SYMBOL CAPACITY 256 CACHE,     price DOUBLE,     amount DOUBLE,     timestamp TIMESTAMP   ) TIMESTAMP (timestamp) PARTITION BY DAY
```


Let's run one of our ill-advised open` *` queries:


```text
EXPLAIN SELECT * FROM trades ORDER BY timestamp DESC;
```


It returns:


```text
QUERY PLAN   DataFrame       Row backward scan       Frame backward scan on: trades
```


Noteable is that the entire DataFrame was processed backward.


Now let's try another query, with a filtering parameter.


```text
EXPLAIN SELECT * FROM trades WHERE amount > 100.0;
```


It returns:


```text
Async JIT Filter     filter: 100.0<amount     workers: 1       DataFrame           Row forward scan           Frame forward scan on: trades
```


Ah, now it's a forward scan and we can see the filter.


Let's try one more example:


```text
EXPLAIN SELECT avg(price), var_pop(price)   FROM trades WHERE amount > 100.0;
```


This returns:


```text
GroupBy vectorized: false     values: [avg(price),var_pop(price)]       Async JIT Filter workers: 24         filter: 100.0<amount           DataFrame               Row forward scan               Frame forward scan on: trades
```


Note` GroupBy vectorized: false` . Vectorization means that QuestDB will parallelize some computations and aggregations. It leads to better performance. The` avg` aggregation does support vectoration, but` var_pop` does not.


> Read more about[vectorization and SIMD operations](https://questdb.com/blog/2020/04/02/using-simd-to-aggregate-billions-of-rows-per-second/) .


We can remove` var_pop` and try again:


```text
EXPLAIN SELECT avg(price)   FROM trades WHERE amount > 100.0;
```


We no longer see the notice that the query as a whole is not applying vectorization:


```text
Async JIT Group By workers: 24     values: [avg(price)]     filter: 100.0<amount       DataFrame           Row forward scan           Frame forward scan on: trades
```


Learning the nitty-gritty of how your queries are *actually* being processed will pay future dividends. You'll soon get a sense of just how your queries are running, and become more familiar with the conditions which lead to high query performance.


## Are you up-to-date?


OK, this one is a bit of a soft ball. But database innovation often moves at a staggering pace. Newer versions tend to improve execution, so as a general rule: Always ensure you are on the latest available version.


For example,[QuestDB 7.3.10](https://github.com/questdb/questdb/releases/tag/7.3.10) , brought with it a 5x-10x speed-up in ASOF and LT JOIN queries, as well as a number of optimizations in parallel filters and GROUP BY, among other improvements.


Simply bumping the version offers strong value.


You'll remain current, and - naturally - very hip.


## Know your queries


Imagine you found a query you love.


Like, LATEST ON running on the trades table to find the most recent pair of rows:


```text
SELECT price, symbol, timestamp FROM trades   LATEST ON timestamp PARTITION BY side;
```


It's neat to see different trades fly into the database.


But let's say we apply *many* symbols:


```text
SELECT price, symbol, timestamp FROM trades   WHERE symbol IN ('SYM1', 'SYM2', 'SYM3', 'SYM4')   LATEST ON timestamp PARTITION BY side;
```


This will be a fairly taxing query, as LATEST ON performs better when only a single symbol is used. How would we know? Well, the[docs tell us](https://questdb.com/docs/query/sql/latest-on/#performance-considerations) .


If you're using a database like QuestDB that has native time-series extensions, or any database that offers less common extensions, they may have tradeoffs and caveats.


As such, it's always wise to read the docs when using novel extensions.


## Apply early inner-filtering


In SQL, the "innermost" query represents the deepest level of nesting. It is often the first part of the query to be executed in a sequence of nested queries, also known as a subquery.


When using subqueries or Common Table Expressions (CTEs), apply filters as early as possible in your query structure. Filtering early reduces the volume of data processed in subsequent steps. Irrelevant rows are removed and not passed down the query chain, thus preventing unnecessary computations on data not included in the final result.


Let's look at an example.


We want to calculate the average price of ETH-USD trades bewteen '2022-03-15' and '2022-03-19', but only for 'buy' side trades.


An inefficient example might look as such:


```text
WITH DailyTrades AS (     SELECT       symbol,       side,       AVG(price) AS avg_price     FROM trades     WHERE timestamp BETWEEN '2022-03-15 00:00:00' AND '2022-03-19 23:59:59'     GROUP BY symbol, side   )   SELECT * FROM DailyTrades   WHERE symbol = 'ETH-USD'   AND side = 'buy';
```


Our DailyTrades CTE calculates the average price for all symbols and sides for the entire day first. Only after aggregation does it filter for 'ETH-USD' and 'buy' trades. This means the aggregation operation processes more data than is needed. As per the demo instance,` ~230ms` is our total computation time.


Can we do better?


```text
SELECT     symbol,     side,     AVG(price) AS avg_price   FROM trades   WHERE     timestamp BETWEEN '2022-03-15 00:00:00' AND '2022-03-19 23:59:59'     AND symbol = 'ETH-USD'     AND side = 'buy'   GROUP BY symbol, side
```


In this second cut, we've applied filters on timestamp, symbol, and side in the main FROM clause. This ensures that only relevant data is considered from the start. And as a result, our second query finishes around` ~190ms` , a cool 18% improvement.


How come? The GROUP BY clause - an already well optimized extension - now has less data to process and thus operates more efficiently. Avoiding an aggregation across the entire dataset is, indeed, bound to be helpful.


## ORDER BY at the end


Apply ORDER BY at the end of your statements. Or, if you expect a large result set, avoid ORDER BY. Also by default the natural order of results will be ascending designated timestamp. If that’s good enough for you, then great! No need for ORDER BY. But assuming you do, consider the following example.


Let's say we want 10 'buy' side trades for 'ETH-USD' between '2022-03-15' and '2022-03-19', including the symbol, side, price, and timestamp.


```text
SELECT *   FROM (       SELECT symbol, side, price, timestamp       FROM trades       ORDER BY timestamp DESC   ) AS OrderedTrades   WHERE symbol = 'ETH-USD'   AND side = 'buy'   AND timestamp BETWEEN '2022-03-15 00:00:00' AND '2022-03-19 23:59:59'   LIMIT 10;
```


This structure is logically correct, however note that ORDER BY is applied within a subquery. The query completed at around ~` 200ms` .


Can we do better? We can!


Let's apply ORDER BY at the end:


```text
SELECT symbol, side, price, timestamp   FROM trades   WHERE symbol = 'ETH-USD'   AND side = 'buy'   AND timestamp BETWEEN '2022-03-15 00:00:00' AND '2022-03-19 23:59:59'   ORDER BY timestamp DESC   LIMIT 10;
```


This re-ordering of our statement averages around` ~180ms` . The goal, as in the prior best practice, is to present a minimized dataset to ORDER BY. The opimization becomes more stark the larger your dataset.


It should be noted that, in the case of a database like QuestDB, SQL extensions can also help. For example, if we were to apply a LIMIT clause, the whole` ORDER BY timestamp DESC LIMIT 10` can be replaced by` 'LIMIT -10'` . The natural sorting of QuestDB once again saves the day (or the microsecond):


```text
SELECT symbol, side, price, timestamp   FROM trades   WHERE symbol = 'ETH-USD'   AND side = 'buy'   AND timestamp BETWEEN '2022-03-15 00:00:00' AND '2022-03-19 23:59:59'   LIMIT -10;
```


## Summary


Hopefully these 10 SELECT tips will help you refine your queries. Great improvements can often be found through fairly minor adjustments. Applying these tips will help you write efficient and flexible queries.


If you have any other recommendations, we'd love to hear about them. Tweet or x-eet or wave or email or smoke signal us (whatever your flavour), or visit us in our[Community Forum](https://community.questdb.com/) to share.


Thanks for reading!
