---
schema_version: "1.0.0"
document_id: "ffd084ba8061719671d2b1f8f2d98b615a8a6416527caf5f1ad8c0ece0a5b5f8"
company_key: "yc-prequel"
company: "Prequel"
source_id: "yc-prequel-news-import-43b0021e02b0"
canonical_url: "https://www.prequel.co/blog/could-open-table-formats-end-the-reign-of-snowflake-and-databricks/"
published_at: "2025-06-25T00:00:00+00:00"
first_seen_at: "2026-07-24T10:10:51.593417+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:8fbc4d8420cb66230e8bddefbed484eef4c2644cd431237797069ea70a820584"
---

# Could Open Table Formats End the Reign of Snowflake and Databricks?

## From Buildings to Buckets


For most of its history, the library was a simple idea: it was a building. A grand, often granite, building that smelled of old paper and wood polish. That building did everything. It held the books on its shelves, it hosted the librarians who would fetch things for you, and it kept the definitive record of its own contents. The building was the beginning and the end of the story. If you wanted a book, you went to the building. Everything was bundled together.


But eventually, the collection simply grew too large for the Corinthian columns and marble floors. Keeping every book in prime downtown real estate became impossibly expensive and impractical. So a new model emerged. The bulk of the collection was moved to a vast, inexpensive warehouse in a less glamorous part of town. The original, beautiful building became more of a reading room and a request desk.


To get you a book, the library now dispatches couriers. You’d fill out a slip at the main desk, and these couriers would travel to the warehouse, find requested books, and bring them back to the libraries. The place where the books lived was now entirely separate from the people and processes that fetched them.


This created a new, rather profound problem. The courier would arrive at this colossal warehouse, faced with millions of books, many of them in identical-looking boxes. Where is “Moby Dick”? Which one of the seventeen editions of “Hamlet” was requested? If two patrons request the last remaining copy of “Harry Potter”, who gets it? The courier has no way of knowing.


A simple shelving system like Dewey Decimal was fine for browsing, but it couldn’t manage a million-volume warehouse. A worker couldn’t possibly know where everything was, which version of a text was the most recent, or who was allowed to see what. The system needed a master directory. It needed the card catalog. Libraries adopted increasingly digital and powerful cataloging systems, leading to the status quo we all know today.


Once upon a time, data warehouses were just like the libraries of old. A data warehouse was a big place that did everything; it stored your data, it ran your queries, and it managed who was allowed to see what. Then the cloud came along and blew that model apart. Data warehouses went on the same journey libraries did, decoupling storage from other functions. The new way was to dump all your raw data files into a vast, cheap cloud bucket like Amazon S3, and then point a separate, ephemeral query engine at it only when you needed to ask a question.


But it creates the same challenges that libraries faced. A pile of files is not a data warehouse. A data warehouse gives you lovely, civilized guarantees. It gives you transactions, the ability to make a change without worrying that someone else is changing the same thing at the same time. It gives you snapshots, so you can ask what your data looked like on a specific day, like last Tuesday. A pile of files in a bucket gives you none of this. It’s just a pile of files. Just like a pile of books isn’t a library.


## Why the Catalog Matters


To bring order to this chaos, we invented a new layer for the stack, the open table format. Think of it as the card catalog on top of your data. This layer tracks every file that belongs to a table, it manages changes over time, and it generally provides the database-like guarantees that the raw storage bucket lacks.


The most popular of these systems is called Apache Iceberg. Its approach is very straightforward. It unbundles the data warehouse into 4 parts:


- Data Storage
- Metadata Storage (Manifest files)
- Catalog (Iceberg REST)
- Query Engine


The DuckDB team has recently released their own Open Table Format, DuckLake. They looked at Iceberg, Delta, and other formats, and basically said, “This is silly.” Their project, DuckLake, is built on a simple observation: the components that enforce table state, the metadata and catalog, need to be really good at database-y things like transactions, locking, and fast lookups. So, if these need to act exactly like a database, *why not just use a database?*


The DuckLake approach is to throw out the metadata and catalog layers entirely and replace them with a relational database. It’s a solution of profound, almost aggressive, common sense. So why would anyone not do this? Why keep the extra layers? Why don’t we all switch to DuckLake or push Iceberg to dump manifests and REST catalogs?


Well, the first and most boring answer is simply sunk cost. There is an entire ecosystem built around Iceberg with products, integrations, and bloggers relying on the current protocol. An immense amount of time, money, and reputation has been invested in selling Iceberg as the future of data warehouses. Nobody gets a promotion for announcing that the last three years of their team’s effort were, in retrospect, a charming but superfluous exercise in over-engineering.


But the more interesting answer is about incentives. Follow the incentives. Data warehouse unbundling is a potentially terrifying prospect for the biggest players, Snowflake and Databricks. Their entire multi-billion-dollar business model is a strategic re-bundling. They masterfully re-bundle the unbundled pieces—the storage, the query engine, the user interface, and the security into one seamless, elegant, and reassuringly expensive package.


The Iceberg REST catalog is the perfect tool for this re-bundling. It’s a chokepoint. It’s a clean, official place where they can add immense value and, more importantly, own a critical part of the process. Both are careful to design their catalogs so that they keep you in their ecosystem.


## The Real Threat: Commoditization


A world where even a catalog is just another component you can spin up yourself is a world where the whole stack becomes dangerously commoditized. If your data is on S3 and your catalog is just a simple Postgres database, what’s stopping you from pointing any number of cheap, interchangeable query engines at it? The query engine becomes a commodity.


This is Snowflake’s nightmare. It is, however, an absolute dream for the cloud providers. Amazon, Google, and Microsoft are more than happy to sell you commoditized compute and storage all day long. A race to the bottom is fine when you own the pavement the race is run on.


And this is where the DuckLake thesis gets interesting. The CIO of a Fortune 500 company isn’t going to rip and replace her existing infrastructure tomorrow because of sunk costs and the very real need for enterprise support. They pay for the bundle precisely because they want the single point of accountability.


But who might take advantage of this? Who loves to take a high-margin cloud product and offer a radically cheaper, good enough version? You could imagine a company like Cloudflare raising its hand. They already have R2, their cost-conscious S3 competitor. They have D1, their edge relational database. What if they bolt DuckLake on top of that? Suddenly, you have a ridiculously inexpensive, serverless analytical stack for the 80% of use cases that aren’t truly “big data” at a really enticing price.


It wouldn’t kill the enterprise data warehouse overnight. But it is one of many examples of what could be a constant, nagging threat from below. The DuckDB team isn’t just offering a different opinion on how to run the library; it’s quietly suggesting the whole building might be overpriced.
