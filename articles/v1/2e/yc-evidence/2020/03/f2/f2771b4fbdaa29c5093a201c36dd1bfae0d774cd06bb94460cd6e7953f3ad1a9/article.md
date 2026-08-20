---
schema_version: "1.0.0"
document_id: "f2771b4fbdaa29c5093a201c36dd1bfae0d774cd06bb94460cd6e7953f3ad1a9"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/why-we-built-usql"
published_at: null
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:e305413efe7ff03ebef84708ad1f126c48701add1ed9280ba0daaa7e78872a31"
---

# Introducing Universal SQL

# Introducing Universal SQL


*Our next step towards our ambition to be the universal front-end for data: Interactivity via SQL and connections to simultaneous data sources, all with extraordinary performance*


[Archie Wood Feb 2nd, 2024 · 3 min read](https://www.linkedin.com/in/archiesarrewood/)


## The Universal Front-End for Data


Our vision for Evidence is for it to be the universal front-end for data. A powerful, batteries-included framework combining the best of web frameworks, BI tools, and notebooks into a package that feels approachable to modern data professionals.


By this, we mean:


- **Open source software to build the front-end for a full spectrum of data apps;** from reports and dashboards to decision support tools.
- **Offering a modern developer experience,** which works with version control and is managed in CI/CD.
- **Enabling data professionals to deliver a superlative user experience,** and build things that feel as good as any consumer web product, with fast page loads and great mobile support.


When we started Evidence, we focused on reports. The humble report: With headers and text, tables and charts, and a well-worn export to pdf button. The thing that all of your users already know how to read, but is so poorly executed in most modern BI tools. If you wanted to generate that type of thing from data in your data warehouse, Evidence was, and is, the best choice for the job.


That focus has worked well for us. Usage of the Evidence framework has been growing steadily by about[5% per week](https://evidence-stats.netlify.app/) since we started.


But the *universal front-end for data* can’t just be static reports on data in your data warehouse.


## Introducing Universal SQL


That’s why we’re so excited to share Universal SQL, the new query engine built into Evidence core, powered by DuckDB’s WebAssembly distribution. This is a foundational release that will enable many new features in the coming weeks and months.


Universal SQL introduces three major improvements to the Evidence open-source framework:


1. **Interactives** like inputs and filters, in pure SQL
2. **Support for multiple simultaneous data sources** , and an open standard for building data source adapters
3. **Extraordinary performance** , running calculations in the browser


## 1. Interactives in Pure SQL


Interactive features have been, by far, the most requested feature from our community. However, the way most web frameworks offer interactivity is by dropping you into JavaScript.


And most of our users are data professionals, not web developers. They want to write SQL.


And this is what Universal SQL delivers: Interactivity via SQL.


## 2. Support for Multiple Data Sources


There are *precisely zero* organizations where all their data lives in a data warehouse. Targets, ad-hoc costs, and custom lookups are frequent culprits and often live in spreadsheets, people’s heads or CSV files.


As a result, analytics tools need to be able to pull in data from all the places it lives. With Universal SQL, Evidence brings support for multiple data sources, so you can merge in data, wherever it lives.


On top of that, to our set of supported data sources in Evidence, we have also added the ability to extend Evidence with data source plugins: You can write your own data source adapters to use with Evidence.


## 3. Extraordinary Performance


The audience for data apps is *regular people* , and those people’s expectations have moved substantially over the last five years. Five seconds to load now feels like an eternity.


Modern BI tools store data on a server, and when you interact with them, send a query to the database to get results, so 2 seconds is a best-case scenario for response times, and 5+ seconds is common.


I’ve sat over the shoulder of enough people trying to access reports to know that this is beyond frustrating. It’s prohibitive to achieving work.


Consumer web apps now achieve response times measured in milliseconds. They achieve this in two ways:


- By pre-rendering content at build time, so that it’s exceptionally fast
- By responding to user input in the client (on the user’s computer or phone)


With Universal SQL, we’re bringing the performance of web apps to data tools.


This simply wasn’t possible before now, as you could not do the data re-aggregation required to change a filter on the user’s device


But now, the computers that people use at work and carry around in their pockets are very powerful, which allows an increasing amount of data processing to be done on their device, which previously would have been done on a server. This trend continues with each new smartphone and laptop release.


A recent release from the DuckDB project is DuckDB[WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly#in_a_nutshell) (WASM), which brings the ability to run a SQL database in the browser. It’s an ideal fit.


## How Universal SQL Works: Cached Data, Queried on your Device


With Universal SQL, we bring together a set of data sources, a client SQL runtime, and a set of interactive components.


Universal SQL Architecture: Data -> Storage -> DuckDB WASM -> Components


1. **Every source is stored in a cache layer:** The cache layer consists of a set of[Parquet](https://parquet.apache.org/) (highly compressed columnar data storage format) files.
2. **An in-browser instance of DuckDB executes queries live against the cache layer:** Each parquet file is accessible as a table in the SQL runtime.
3. **Input components allow you to take input parameters from the user:** These can be used as parameters in queries or components can issue their own.


## Zero JavaScript, Low Latency, Seriously Large Data


As a result of this new architecture, Evidence has a slew of new capabilities.


Filtering in Universal SQL is just SQL where clauses


- **User inputs can be templated into SQL queries** with our new[input components](https://docs.evidence.dev/core-concepts/filters/)
- **You can write filter logic in pure SQL** to make components responsive to inputs
- **Filtering 1M+ rows in the browser is practically instant** - this[demo ecommerce analytics site](https://ecommerce.evidence.app/) includes a 500k row dataset
- **You can pull in new data sources** , including from outside of the warehouse, *and* still run them through a CI/CD process so that they don’t break your reports if someone accidentally changes them


```text
select
count  (  *  )    as   num_orders
from   orders
where   category =  '${inputs.category_picker}'
```


Templating a user input into a SQL query


## What’s Coming Next?


This is just the start of what we can offer with Universal SQL. Stay tuned for:


- **Many more input components:** Sliders, Date pickers, Multi-selects and more. If there are components you need, now is the perfect time to hop into GitHub and[submit a feature request](https://github.com/evidence-dev/evidence/issues/new?assignees=&labels=components&projects=&template=feature_request.md&title=Input%20Component:)
- **Many data sources:** Anyone, including you, can now maintain their own source plugin and use it with Evidence


- We’ve put together plugins for[Google Sheets](https://github.com/evidence-dev/datasources/tree/main/gsheets) ,[NPM downloads](https://github.com/archiewood/npm-stats) and[You Need a Budget](https://github.com/itsmebriand/evidence-connector-ynab) as examples of a 3rd Party adapters
- There’s a[template adapter](https://github.com/evidence-dev/datasource-template) which you can fork as a starting point
- We’ve also published docs on how tocreate an adapter


- **Self-serve data exploration** : We’re working on allowing end users to explore all the data loaded in your cache layer with a pivot-table style database explorer, and a SQL console for more advanced users


An Evidence SQL Console


We’re grateful to our community and supporters who’ve helped us get this far, and so excited for this next chapter as we build the best front-end for data.
