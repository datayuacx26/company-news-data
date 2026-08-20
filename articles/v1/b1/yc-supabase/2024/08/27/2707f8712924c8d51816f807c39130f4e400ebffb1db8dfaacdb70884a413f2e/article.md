---
schema_version: "1.0.0"
document_id: "2707f8712924c8d51816f807c39130f4e400ebffb1db8dfaacdb70884a413f2e"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/postgres-foreign-data-wrappers-with-wasm"
published_at: "2024-08-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:076e40b894b3556996aa628528ad973939a9d330a2e37df8360aa667cd0ee31f"
---

# Postgres Foreign Data Wrappers with Wasm

Foreign Data Wrappers (FDWs) allow Postgres to interact with externally hosted data. To operate a FDW, the user creates a foreign table. When queried, the foreign table reaches out to the 3rd party service, collects the requested data, and returns it to the query in the shape defined by the foreign table. This allows seamless querying and data manipulation across different tools as if they were local tables from within Postgres.


[Wrappers](https://github.com/supabase/wrappers) is a Rust framework for creating Postgres Foreign Data Wrappers. Today we're releasing support for[Wasm (WebAssembly)](https://webassembly.org/) wrappers.


With this feature, anyone can create a Wasm wrapper to an external service and run it directly from e.g. GitHub:


`
_14


-- An Example Google Sheets Wasm Wrapper:


_14


_14


create server google_sheets


_14


foreign data wrapper wasm_wrapper


_14


options (


_14


-- Install from GitHub


_14


fdw_package_url 'https://github.com/<ORG>/<REPO>/releases/download/v0.2.0/google_sheets_fdw.wasm',


_14


fdw_package_name 'my-company:google-sheets-fdw',


_14


fdw_package_version '0.2.0',


_14


fdw_package_checksum '338674c4c983aa6dbc2b6e63659076fe86d847ca0da6d57a61372b44e0fe4ac9',


_14


_14


-- Provide custom options


_14


base_url 'https://docs.google.com/spreadsheets/d'


_14


);


`


This feature is available today in public alpha for all new projects.


## What are Foreign Data Wrappers?#


[Foreign Data Wrappers (FDW)](https://wiki.postgresql.org/wiki/Foreign_data_wrappers) are a powerful feature of Postgres that allows you to connect to and query external data sources as if they were regular tables.


[Wrappers](https://github.com/supabase/wrappers) is an open source project that simplifies the creation of Postgres Foreign Data Wrappers using[Rust](https://www.rust-lang.org/) .


## Why WebAssembly?#


[WebAssembly (Wasm)](https://webassembly.org/) is a binary instruction format that enables secure and high-performance execution of code on the web. It is originally designed for web browsers, but now can also be used in server-side environments like Postgres.


Here's how the Wasm FDW benefits us:


- **Improved Security:** Wasm's sandboxed execution runtime with minimum interfaces enhances the security of FDW.
- **Simplified Development:** Developers can use[Rust](https://www.rust-lang.org/) to create complex FDWs without diving deep into Postgres internal API.
- **Simplified Distribution:** Easily distribute your Wasm FDW through any URL-accessible storage (such as GitHub or S3).
- **Enhanced Performance:** Wasm's near-native speed ensures FDWs operate with minimal overhead.
- **Increased Modularity:** Each Wasm FDW is an isolated package which is dynamically loaded and executed by Wrappers individually.


## Architecture#


To better understand how the Wasm FDW works, let's take a look at the architecture:


The above diagram illustrates the key components and how they interact:


1. **Supabase Wrappers Extension (Host):** This is the core component that runs within Postgres. It includes below modules:


- **Wasm Runtime:** Provides runtime environment to executes the Wasm FDW package.
- **HTTP Interface:** Manages communication with external data sources through HTTP.
- **Utilities:** Helper tools and functions to support FDW operations.
- Other modules providing specific functionalities, such like JWT, stats and etc.


2. **Wasm FDWs (Guests):** Isolated, dynamically-loaded Wasm packages that perform data fetching and processing. They execute in a sandboxed environment to ensure security and performance. For example:


- **Snowflake Wasm FDW:** A foreign data wrapper specifically designed to interact with[Snowflake](https://www.snowflake.com/) .
- **Paddle Wasm FDW:** Another FDW example, tailored for[Paddle](https://www.paddle.com/) integration.


3. **Web Storage:** Represents external storage services like[GitHub](https://github.com/) or[S3](https://aws.amazon.com/s3/) , where Wasm packages can be publicly stored and downloaded from.
4. **External Data Source:** Various external systems which data is fetched from or pushed to, such as[Snowflake](https://www.snowflake.com/) and[Paddle](https://www.paddle.com/) . Data is accessed using RESTful APIs.


## Data fetching#


Wasm FDWs are loaded dynamically when the first request is made. The interaction flow is:


1. **Wasm download:** The Wasm FDWs are dynamically downloaded from web storage services, like GitHub or S3, and cached locally. This happens the first time the` SELECT` statement is initiated.
2. **Initialization and Execution:** Once downloaded, the Wasm FDWs are initialized and executed within the embedded Wasm runtime environment. This provides a secure, sandboxed execution environment that isolates the packages from the main Postgres system.
3. **Data Fetching via RESTful API:** The Wasm FDWs interact with their respective external data sources via RESTful APIs.
4. **Query Handling and Data Integration:** When a query is executed against a foreign table in Postgres, the Supabase Wrappers extension invokes the appropriate Wasm FDW, fetches data from the external source, processes it, and returns it to the Supabase Wrappers, which integrates it back into the Postgres query execution pipeline.


The Wasm FDW currently only supports data sources which have HTTP(s) based JSON API, other sources such like TCP/IP based DBMS or local files are not supported yet.


## Developing your own Wasm FDW#


A major benefit of Wasm FDW is that you can build your own FDW and use it on Supabase. To get started, clone the[Postgres Wasm FDW \[Template\]](https://github.com/supabase-community/postgres-wasm-fdw) . Building your own Wasm FDWs opens up a world of possibilities for integrating diverse data sources into Postgres.


Visit[Wrappers docs and guides](https://fdw.dev/guides/create-wasm-wrapper/) to learn more about how to develop a Wasm FDW.


As the Wasm FDW can access external data sources, you should never install Wasm Wrappers from untrusted source. Always use official Supabase FDWs, or use sources which you have full visibility and control.


## Try it now on Supabase#


The Wasm FDW feature is available today on the Supabase platform. We have 2 new built-in Wasm FDWs:[Snowflake](https://supabase.com/docs/guides/database/extensions/wrappers/snowflake) and[Paddle](https://supabase.com/docs/guides/database/extensions/wrappers/paddle) .


To get started, follow below steps:


1. Create a new Supabase project:[database.new](http://database.new/)
2. Navigate to the[Database -> Wrappers](https://supabase.com/dashboard/project/_/database/wrappers) section and enable Wrappers.
3. Add` Snowflake` or` Paddle` wrapper, follow the instructions and create foreign tables.


We can also use SQL. Let's try, using the Paddle FDW as an example.


### Enable Wasm Wrappers#


Inside the[SQL editor](https://supabase.com/dashboard/project/_/sql/new) , enable the Wasm Wrapper feature:


`
_10


-- install Wrappers extension


_10


create extension if not exists wrappers with schema extensions;


_10


_10


-- create Wasm foreign data wrapper


_10


create foreign data wrapper wasm_wrapper


_10


handler wasm_fdw_handler


_10


validator wasm_fdw_validator;


`


### Get your Paddle credentials#


Sign up for[a sandbox account](https://developer.paddle.com/api-reference/overview#base-url) and[get API key](https://sandbox-vendors.paddle.com/authentication-v2) with Paddle.


### Save your Paddle credentials#


Create a Paddle server in Postgres using the Wasm FDW created above:


`
_15


-- create Paddle foreign server


_15


create server paddle_server


_15


foreign data wrapper wasm_wrapper


_15


options (


_15


-- check all available versions at


_15


-- https://fdw.dev/catalog/paddle/#available-versions


_15


fdw_package_url 'https://github.com/supabase/wrappers/releases/download/wasm_paddle_fdw_v0.1.1/paddle_fdw.wasm',


_15


fdw_package_name 'supabase:paddle-fdw',


_15


fdw_package_version '0.1.1',


_15


fdw_package_checksum 'c5ac70bb2eef33693787b7d4efce9a83cde8d4fa40889d2037403a51263ba657',


_15


_15


-- save your Paddle credentials


_15


api_url 'https://sandbox-api.paddle.com',


_15


api_key '<your Paddle sandbox API key>'


_15


);


`


### Set up your Foreign Tables#


Create a table for Paddle data:


`
_19


-- create dedicated schema for Paddle foreign tables


_19


create schema if not exists paddle;


_19


_19


-- create foreign table


_19


create foreign table paddle.customers (


_19


id text,


_19


name text,


_19


email text,


_19


status text,


_19


custom_data jsonb,


_19


created_at timestamp,


_19


updated_at timestamp,


_19


attrs jsonb


_19


)


_19


server paddle_server


_19


options (


_19


object 'customers',


_19


rowid_column 'id'


_19


);


`


### Query Paddle from Postgres#


Now let's query the foreign table and check the result:


`
_10


select id, name, email, status


_10


from paddle.customers;


`


That's it. Head over to the[Supabase Wrappers documentation](https://fdw.dev/) to find more detailed guides on setting up and using Wasm FDWs.


## Thanks to our community contributors#


None of this innovation would have been possible without the relentless efforts and contributions of our vibrant community. We'd like to thank all the following developers for their contributions:


[Aayushya Vajpayee](https://github.com/AayushyaVajpayee) ,[Romain Graux](https://github.com/romaingrx)


Want to join the Supabase Wrappers community contributors?[Check out our contribution docs](https://fdw.dev/contributing/core/) .
