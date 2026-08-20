---
schema_version: "1.0.0"
document_id: "1a625a0245b93f67c34fb2cfd7f689183b3e8ffb4971dd664ee19665c2c0b921"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/database-drivers-naughty-or-nice/"
published_at: "2022-12-12T00:00:00+00:00"
first_seen_at: "2026-07-22T10:08:50.167024+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:a367f18621508267b0f0f597ddafa7f412b6e99c70d949f3f0656dc17233de6d"
---

# Database Drivers: Naughty or Nice?

We spent a lot of time writing code against databases, data warehouses, and their assorted drivers this year. Like, a lot a lot. Through that process, we’ve uncovered some… unexpected behaviors. In some cases, those behaviors go directly against the tool’s documentation. In other cases, they just go against common sense (to the extent that our sense is common anyway). In any case, we figured it would be interesting to compile those and share them, both for entertainment and for posterity.


Before we dive in, a quick note. We feel extremely fortunate to get to work with such a breadth of tools, and to be able to build on the work of others to continue to deliver better products. We write software for a living, and we’re the first to know that bugs come with the territory — unless you’re superhuman, or you’re not writing any meaningful amount of code, bugs are pretty much inevitable.


Our goal here is very much not to deride specific projects or companies for having them. Instead, we figured it was interesting enough knowledge to share. Wherever and whenever possible, we do our best to be upstanding citizens and report those bugs with their respective owners. Regardless, to prevent this post from serving as a “name and shame” opportunity, we thought it best to anonymize the databases and drivers here. If you work at a db company and you’d like to get our feedback on a specific driver or open a line of communication, drop us a note at hello (at) prequel.co!


Now, without further ado, here’s our top hits of “uhm, WTF?!” of database and driver behavior of the year.


## Some first party drivers don’t implement advertised functionality


The first party GoLang driver for a major database/dwh doesn’t implement COPY file functionality, despite clearly stating in its documentation that it does. We’re not sure either, but we know from first-hand experience that it’s hard to keep documentation perfectly up to date. So we’ll give them a pass.


In order to unlock full functionality (and get around other issues), we started using more ODBC-based drivers. This brings us to our next hit.


## Some ODBC drivers leak memory


The ODBC interface is designed for use with the C programming language. In other words, ODBC drivers, by nature, have to run some C code. Turns out, C programs may leak memory, and ODBC drivers are no exception. Some leak a little bit with every call, while some others have specific calls or usage patterns that cause them to leak a lot of memory.


Here’s a fun graph of the memory usage of our pods, over a period of a few days when we were investigating a memory leak in our dev environment. Thankfully, we caught it before it made it to prod. The root cause turned out to be a certain call pattern within an ODBC driver.


## Some drivers leak credentials


Some drivers log credentials if there is an error in a query containing a credential like an API key or an AWS secret key. Other, more upstanding citizens will replace known credential fields with a safe` REDACTED` string when logging to stdout or stderr.


## Some drivers squash key errors and just return an empty result set instead


We learned this one the hard way when trying to ship a new integration. Our code looked right, and worked on tiny datasets. But when we tried to run it on larger datasets, it would behave as though there was no data in the db table and return an empty result set.


We found out that some drivers have implicit query timeouts and that when those timeouts are exceeded, the drivers don’t raise or return any kind of exception or error. Instead, they simply return an empty result set.


## Some drivers don’t agree on how epochs should be represented


It’s no secret that timestamps, timezones, and assorted objects frequently cause confusion among developers. What’s perhaps less well-known is that even epochs (Unix seconds representation of a timestamp), despite their appeal as a timezone free representation, can be tricky to work with.


As it turns out, databases frequently disagree on what exactly an epoch should look like. Some follow the strict definition, treating them explicitly as the number of *seconds* elapsed since midnight UTC on Jan 1 1970. In those cases, they are stored and represented as int32. Others however choose to include millisecond or nanosecond precision. These are represented as int64 or as various kinds of floats, with the whole number being seconds and the fraction the added precision.


This can cause all kinds of data integrity issues when moving data. In the best cases, no precision is meaningfully lost. In more insidious cases, the load works but introduces deltas between the timestamps which can only be caught by carefully diff’ing the loaded data.


In other cases still, different drivers for the same warehouse might treat them differently, one treating them as an int and another as a float. We ran into this when switching from a native driver to an ODBC driver for a well known warehouse. All this to say: just because you move your timestamps as epochs doesn’t mean that you’re out of the woods when it comes to time-handling complexity.


## Some database vendors are not careful about breaking changes


A well known data warehouse updated the connection string format expected by its driver, leading to an obscure connection error. The corresponding version (Semver) change indicated a` MINOR` change, even though this was in fact not a backwards compatible change. Through this, we learned 1) that not all vendors treat their drivers as first class product offerings, and 2) to always check the level of support a vendor provides for their driver before committing to supporting a given database or data warehouse.


## Some drivers are opinionated about strings… like really opinionated


Some drivers believe strings have to be UTF-8, others believe all strings are just bytes and will always treat them as byte arrays. Beyond that, some drivers have weird exceptions about the characters that their driver can support (most of which aren’t actually documented).


## Some data warehouses are just really inconsistent when it comes to types


We have so much to say on the topic that this deserves to be its own blog post. But overall, data warehouses differ wildly in how leniently they handle types. Some will happily load an instance of a` DECIMAL(38,9)` into a` DECIMAL(38,18)` column, because they don’t enforce size, precision, or scale limits on their numeric types. Others will complain loudly and call you an idiot for even thinking about doing such a thing.


Some don’t even have full type support within their own driver. Most of them don’t have great support when it comes to JSON columns, and treat it all kinds of mean ways.


## Some databases don’t quite follow common standards


We recently encountered an error when trying to load Parquet files created by DuckDB into Databricks. What’s funny is that the same files could load just fine in other data warehouses, such as Snowflake or BigQuery. The specific error was the following:


> Only one of num_children and type should be set in SchemaElement.


Upon digging deeper, we realized this was the byproduct of two things:


- Databricks implements the Parquet spec more literally than others, at least when it comes to metadata. It’s not entirely surprising, given that Parquet has deep support in the Spark ecosystem.
- DuckDB incorrectly set both metadata fields when creating a Parquet file.


We filed a[bug report](https://github.com/duckdb/duckdb/issues/5575) with the DuckDB OSS project to let them know, and they fixed it in just 48 hours. We’re massive fans of the DuckDB project to begin with, but this went way beyond anything we expected. Kudos to the team.


Aaand that’s it. We’re planning on spending just as much time with databases and drivers in 2023, so here’s to writing more software, uncovering more odd behaviors, filing more bugs, and doing more with data!


If there are any specific bugs that you’ve uncovered in your own work, we’d love to hear about them. Feel free to drop us a line and we’ll document those as well!
