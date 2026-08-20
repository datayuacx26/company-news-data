---
schema_version: "1.0.0"
document_id: "ead426b1ff9bb7c3210f6c66a2e7f53a10f1a482ebf75aed5d66ad6f6a0da0fc"
company_key: "yc-evidence"
company: "Evidence"
source_id: "yc-evidence-news-import-47bf0dc75044"
canonical_url: "https://evidence.dev/blog/what-is-a-flat-file"
published_at: "2025-01-30T00:00:00+00:00"
first_seen_at: "2026-07-21T19:04:43.661863+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:619bb81b38fb378bfff0f01aa36913f88770260cbd6ebe01728874da743210cc"
---

# What Is a Flat File?

# What Is a Flat File?


*The ultimate guide to flat files, their use cases, format specifics, and how they stack up against non-flat file databases and relational DBMSs*


[Alex Prokudin - Guest Contributor January 30, 2025 · 3 min read](https://www.linkedin.com/in/aleksandr-prokudin-828b631b9/)


Alex is a tech writer with a background in information security, identity management, and SaaS.


Flat files are an incredibly common format in data analytics. This guide explains what they are, what types of flat files exist, and when you would pick one over the other.


Jump to file:


- CSV (Comma-Separated Values)
- TSV (Tab-Separated Values)
- PSV (Pipe-Separated Values)
- Fixed-Width Files
- TXT Files
- JSON (JavaScript Object Notation)
- XML (eXtensible Markup Language)
- YAML (YAML Ain’t Markup Language)
- ENV Files


## The Definition Challenge


Most of the time, you deal with flat files when you’re extracting data from a database and feeding it to a processing pipeline. Chances are, you’ll be using the simplest representation of that data like a CSV file—a text file where each row represents a database record, and fields are separated with a comma. That’s where the “text database” and “flat file database” monikers come from.


However, the term “flat file” itself is used so loosely by companies that there is simply no single definition that would satisfy everybody. Here are some generally agreed-upon characteristics:


- **Text-only** : flat files have no blobs and store text and numbers only.
- **Tabular format** : one table per file, row-based organization of records.
- **Portable** : it’s usually very easy to output, edit and ingest flat files, as they don’t require specialist software.
- **Unstructured** : *generally* no hierarchy or relationships between records or fields.
- **No built-in compression** : flat files don’t have smart compression algorithms by design, although tools like DuckDB and Spark will accept zip-compressed files.
- **No index** : flat files have no built-in method to find a specific record.
- **No documentation** : *generally* , they have no embedded descriptive metadata or schema information.


Even this attempt at a definition starts falling apart when considering JSON and YAML - two file formats commonly referred to as flat - which absolutely *can* have both hierarchy and schema.


It doesn’t really help that flat files can serve vastly different use cases, either:


- CSV is typically used as a data storage and exchange format.
- JSON is everywhere: exchanging large amounts of data, managing configurations, plus modern APIs mostly return JSON.
- YAML is primarily a configuration management and pipeline description file format.


Let’s try to make sense of that.


## Types of Flat Files


There’s more than one way to slice flat files into types. Much of that is about how data is organized and stored.


- **Field and record organization** . Unstructured flat files (e.g. CSV) contain a single database table where each row represents a record. Structured flat files (JSON, YAML, XML) mostly have a hierarchy.
- **Field delimiter type** . When one row represents one database record, a special character can be used to separate parts of that row into fields. Typically, it’s a comma, a tab, or a pipe (”|”).
- **Fixed-width vs non-fixed-width formats** . With fixed-width formats, every field has a predefined width, which makes it cheaper to parse data. Writing and storing fixed-width data, however, may have some overhead as compared to some non-fixed-width formats.
- **Data delimiters** . File formats can dictate whether you can save tabs and newlines inside a field. For example, CSV uses escape syntax to allow that, while TSV disallows tabs and newlines.
- **Human readability** . It is usually assumed that flat files are human-readable because they only contain plain text. However, some platform vendors refer to Excel files as flat files, even though they are zipped XML files with an XML schema.
- **Metadata** . Most flat file formats, like CSV, don’t have any metadata and need external documentation that explains what data is considered valid in each field and how it should be processed. Others, like JSON, can contain embedded metadata or schema and, therefore, are self-descriptive.


## How Flat Files Are Used


Flat files don’t all have the same applications. Some work great for porting data across systems, others aren’t even designed for storing large amounts of data and serve entirely different purposes.


### Storage And Exchange


- **Data exchange** . Flat files like CSV and JSON are commonly used to exchange data between applications or platforms. Many platforms already support them out of the box, and you can write your own data source connector for a platform that doesn’t support them.
- **Data integration in ETL** . Flat files often act as a source or destination for data in ETL pipelines, so you can transform them and load them into data warehouses.
- **Archiving and backup** . Flat files are a common choice to store historical or archived data. Because they are text-only, the data remains accessible without dependency on proprietary software.


### Utility Use


- **Configuration management** . Flat files like YAML, JSON, and INI are commonly used to define configuration settings for applications and systems: environment variables, database connections, feature toggles, and more.
- **Data pipeline definitions** . Flat files like JSON and YAML are often used to define the structure and flow of data pipelines to automate data processing tasks.
- **Metadata for datasets** . JSON is sometimes used to describe how to validate and transform CSV files.


## Examples of Flat Files


Let’s look at the most common flat file formats: what they look like, where they excel, and where they are not the best choice.


### CSV (Comma-Separated Values)


- **File extension** :` .csv`
- **Delimiter** : Comma (` ,` )
- **Field Organization** : Flat
- **Human Readable** : Yes


CSV files are typically used in data exchange, ETL pipelines, and lightweight tabular data analysis. It’s also a standard export option for data tools such as Excel, PowerBI, Looker. CSVs are often compressed with gzip or zip, in which case the extension may be` .csv.gz` or` .csv.zip` .


Example:


```text
name, country, age
Alice, USA, 22
Bob, Canada, 34
Charlie, UK, 28
```


*Choose if* :


- You need a widely supported format for structured tabular data.
- Your data is coming from BI systems or Excel/Google Sheets.


*Avoid if* :


- You need to represent complex hierarchies.
- Your data contains many commas that require parsing escape sequences.


> **Tip** : You can easily add CSVs as a data source in Evidence.[Learn how](https://docs.evidence.dev/core-concepts/data-sources/csv/)


### TSV (Tab-Separated Values)


- **File extension** :` .tsv`
- **Delimiter** : Tab (` \\t` )
- **Field Organization** : Flat
- **Human Readable** : Yes


TSV is typically used in scientific computing (e.g. in[The Cancer Genome Atlas](https://www.cancer.gov/ccg/research/genome-sequencing/tcga/using-tcga-data/types) project) and workflows where commas might conflict with data values (e.g. address fields).


Example:


```text
name	country	age
Alice	USA	22
Bob	Canada	34
Charlie	UK	28
```


*Choose if* :


- Your data contains commas and you need an alternative delimiter that minimizes conflicts.
- You need to use simple Unix CLI tools to parse the data.


*Avoid if:*


- Your data contains tabs that require escaping.
- You need to store hierarchical data.


### PSV (Pipe-Separated Values)


- **File extension** :` .psv`
- **Delimiter** : Pipe (` |` )
- **Field Organization** : Flat
- **Human Readable** : Yes


PSV is typically used in financial systems (e.g.[Bank of America](https://healthaccounts.bankofamerica.com/assets/pdf/CDEX_Integration_Guide.pdf) ), industry-specific workflows, and proprietary data exchange where unique delimiters are required.


Example:


```text
name|country|age
Alice|USA|22
Bob|Canada|34
Charlie|UK|28
```


*Choose if* :


- You need a delimiter that is less likely to appear in data fields.


*Avoid if:*


- Your tools lack native support for parsing pipe-separated files.
- You need to represent nested data.


### Fixed-Width Files


- **File extension** : Varies
- **Delimiter** : None (fixed-width fields)
- **Field Organization** : Flat
- **Human Readable** : Limited


Fixed-width files are typically used in legacy systems, financial reporting, and mainframe data processing.


Example:


```text
Name       Country    Age
Alice      USA        22
Bob        Canada     34
Charlie    UK         28
```


*Choose if:*


- You need speed and low resource consumption when reading files.
- All your systems that read these files know how long each “column” is.


*Avoid if:*


- You need flexible field length.
- The overhead of writing fixed-width files may outweigh the benefits of high performance when reading them.


### TXT Files


- **File extension** :` .txt`
- **Delimiter** : None or arbitrary
- **Field Organization** : Flat or hierarchical
- **Human Readable** : Yes


TXT files are typically used for simple text storage, writing log files, and unstructured data exchange.


Example:


```text
2025-01-24 12:00:00 INFO  Data pipeline started: Extracting data from source 'sales_db'.
2025-01-24 12:01:15 WARN  Missing values detected in column 'Region'. Proceeding with default value 'Unknown'.
2025-01-24 12:02:45 ERROR Failed to connect to API 'weather_service'. Retrying in 30 seconds.
2025-01-24 12:03:15 INFO  Data transformation completed: 10,000 rows processed.
```


*Choose if:*


- You need a lightweight, generic format for storing plain text data.


*Avoid if:*


- Your data needs a hierarchy.


### JSON (JavaScript Object Notation)


- **File extension** :` .json`
- **Delimiter** : None (uses key-value pairs and nested objects)
- **Field Organization** : Hierarchical
- **Human Readable** : Yes


JSON is typically used in web APIs, data exchange, and nested or semi-structured data processing.


Example:


```text
[
{
"name"  :    "Alice"  ,
"country"  :    "USA"  ,
"age"  :    22
}  ,
{
"name"  :    "Bob"  ,
"country"  :    "Canada"  ,
"age"  :    34
}  ,
{
"name"  :    "Charlie"  ,
"country"  :    "UK"  ,
"age"  :    28
}
]
```


*Choose if:*


- Your data includes hierarchies or nested relationships.
- Your data needs to be machine-readable as well as human-readable.


*Avoid if:*


- Data processing speed is a priority.
- You have data types not supported by JSON.


### XML (eXtensible Markup Language)


- **File extension** :` .xml`
- **Delimiter** : None (uses tags)
- **Field Organization** : Hierarchical
- **Human Readable** : Yes (but verbose)


This format was used in enterprise data exchange, web services (e.g., SOAP), and document storage with metadata, but it has largely been superseded by JSON for most use cases.


Example:


```text
<  people  >
<  person  >
<  name  >   Alice </  name  >
<  country  >   USA </  country  >
<  age  >   22 </  age  >
</  person  >
<  person  >
<  name  >   Bob </  name  >
<  country  >   Canada </  country  >
<  age  >   34 </  age  >
</  person  >
<  person  >
<  name  >   Charlie </  name  >
<  country  >   UK </  country  >
<  age  >   28 </  age  >
</  person  >
</  people  >
```


*Choose if:*


- You need a highly structured format with metadata and validation capabilities.
- You need compatibility with older systems that require XML.


*Avoid if:*


- You prioritize file size efficiency or readability because XML is verbose.


### YAML (YAML Ain’t Markup Language)


- **File extension** :` .yaml` or` .yml`
- **Delimiter** : None (uses indentation for structure)
- **Field Organization** : Hierarchical
- **Human Readable** : Yes


YAML is typically used in configuration files, GitHub workflows, and data serialization.


Example:


```text
restart-strategy  :
type  :   failure -  rate
failure-rate  :
delay  :   1 s
failure-rate-interval  :   1 min
max-failures-per-interval  :    1
```


*Choose if:*


- You need a human-readable format for configuring pipeline jobs and orchestrating workflows.


*Avoid if:*


- You need to store a large dataset.


### ENV Files


- **File extension** :` .env`
- **Delimiter** : None (uses` =` for key-value pairs)
- **Field Organization** : Flat
- **Human Readable** : Yes


ENV files are typically used in storing environment variables and application configurations.


Example:


```text
APP_NAME=MyAnalyticsApp
ENVIRONMENT=production
DATABASE_URL=postgres://user:password@localhost:5432/mydb
API_KEY=abcdef123456
DEBUG=False
```


*Choose if:*


- You need a lightweight and straightforward way to manage configurations for deployment or local environments


*Avoid if:*


- You need advanced data structures or are storing large amounts of data.


## Flat Files vs Non-Flat Files vs DBMS


Flat files are commonly compared against relational databases, although there is a middle-ground: file formats like Avro, Parquet, and ORC. Let’s compare the most popular options by the features we discussed in the beginning.


**Feature** **CSV** **JSON** **Parquet** **Relational DBMS**


**Records organization** Row-based Key-value pairs Columnar Row-based


**Human readability** Text Text Binary Binary


**Portability** High High High Low


**Hierarchy** None Available Nested structures Multiple tables


**Scalability** Low Low High High


**Index** No No Yes* Yes


**Schema** None Possible Enforced Enforced


* Parquet doesn’t have B-trees or hash indexes. Instead, it has file-level, row group-level, and column-level metadata that helps decide which data to read and which to skip instead of scanning the entire dataset.


## How to Select The Right Flat File Format


With this many options, you can use a simple rule of thumb:


- Use flat files with a delimiter, such as **CSV** and **TSV** , if you need a simple file format to archive data or move it between platforms or spreadsheet software like Excel and Google Sheets.
- Use **JSON** if you want a self-descriptive file format with a hierarchy, for either data storage/exchange or configurations.
- Use other flat file formats when you need to integrate with tools that need them.
- Consider using file formats like Parquet if you need smaller file sizes, faster queries, and support for complex data types.


## Start Analyzing Flat Files Data with Evidence


Evidence allows you to analyse and display data from flat files. It accepts JSON, CSV, and Parquet files. It also supports most popular analytics databases as data sources. To get started with Evidence and create your first data product, install the[VSCode extension](https://docs.evidence.dev/install-evidence/) .
