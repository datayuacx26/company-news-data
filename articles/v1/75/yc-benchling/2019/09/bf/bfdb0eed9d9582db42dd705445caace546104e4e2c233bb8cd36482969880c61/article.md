---
schema_version: "1.0.0"
document_id: "bfdb0eed9d9582db42dd705445caace546104e4e2c233bb8cd36482969880c61"
company_key: "yc-benchling"
company: "Benchling"
source_id: "yc-benchling-rss-dcbca8149e4b"
canonical_url: "https://www.benchling.com/blog/benchling-product-release-newsletter-august-2019"
published_at: "2019-09-04T14:47:41+00:00"
first_seen_at: "2026-07-20T03:30:03.260200+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:f940d10ed446fa4050b2f9802899c4b7f3fad3d4aa90b169c60aa310513f2e8b"
---

# Benchling Product Release Newsletter: August 2019

Welcome to the August edition of our monthly product release newsletter! Here are some of the major new features and updates that were released over the month.


### Computed fields to display key results in Registry


Computed fields over results is a new feature that allows users to create schema fields based off of results associated with an entity. This allows the most relevant results or measurements to be displayed directly in the Registry entry. The formulas supported currently for auto-computation include:


-


-


Most recent result value


-


Sum of field in results


-


Mean of field in results


-


Standard deviation of field from results


Users can now view, filter, and export results directly within the Registry without the need to query the Data Warehouse. Users can also search for entities based on recorded results.


### Worklists for containers, entities, and batches


Worklists is a brand new feature of the Inventory application which allows users to move containers, entities, and batches between Benchling applications. This feature also facilitates bulk actions on a large number of Inventory samples, saving Scientists time and effort. As a result, the Inventory application is now easier to use and is more seamlessly integrated with actions performed in other Benchling applications. A few of the use cases of the Worklists feature are listed below.


#### 1) Barcode scanning


Dramatically improved barcode scanning experience allowing users to scan multiple container barcodes into a single Worklist. This Worklist can then be used to perform the same action on all the Worklist samples at once such as sample transfer or expend. Read this[help article](https://help.benchling.com/hc/en-us/articles/9684235659277-Use-worklists-to-scan-barcodes) to learn more.


#### 2) Adding containers, entities, or batches to Transfer table


Create Worklists of relevant containers, entities, or batches and easily add all the samples from the Worklist to a Transfer table with a few clicks without the need for laborious copy pasting or spreadsheet uploads.[Learn more.](https://help.benchling.com/hc/en-us/articles/9684227134221-Create-a-new-worklist)


#### 3) Adding entities to Results table


Generate Worklists of relevant entities or batches and easily add all of them to a Results table with a few clicks. This allows Scientists to quickly set-up Results tables and record data against them.


From the bench to your inbox


Our monthly newsletter features science insights, industry best practices, and stories from teams pushing biotech forward.


### Improved Inventory search


Inventory search on Benchling received a major overhaul this past month. Users can now perform advanced searches to find batches, entities, or containers within Inventory search directly. The search feature offers new filters and options to search based on entity type, container attributes, container contents, batch-entity relationships, and volume / quantity of contents. Additionally, the container attributes and container contents can be viewed directly in the search results without the need for additional clicks. This feature more tightly integrates Registry with Inventory, allowing you to search for containers based on all the fields you could search for in the Registry. To learn more read[this help article](https://help.benchling.com/hc/en-us/articles/9684227178637-Searching-your-inventory-and-managing-samples) .


### Dedicated Warehouse tables for every schema type


Writing custom queries against Benchling’s Data Warehouse is now easier than ever. With the new update, every entity, batch, container, box, location, and entry will have its own dedicated table in the Data Warehouse. This allows the data in Benchling to be organized similar to traditional relational databases, thus making it easier to query the data and allowing the schema tables directly connectable and usable in BI tools like Spotfire and Tableau. Please note that your existing tables and queries will remain unaffected by this change.


### Multiple rows in Registration table template


Registration tables can now have multiple rows in both Workflow and Entry templates. This feature is useful for cases where several samples are generated together (e.g. samples in 96-well plate) or when a pre-set number of derivative samples are generated (e.g. replicate preps of same sample sets). Additionally, more advanced development templates such as Registration tables to look up equipment metadata are now possible. This feature should help reduce busy work and improve compliance in data entry.


### Codon Optimization


In August, we also added a Codon Optimization tool to our Molecular Biology application. In Benchling, Codon Optimization streamlines the process of designing a DNA sequence to optimize protein expression in a target organism. Our new tool considers not only codon frequencies, but also GC content and other advanced parameters. In addition, this intuitive tool can perform optimized back translations of amino acid sequences. To learn more about our Codon Optimization tool, check out this[product tutorial](http://help.benchling.com/en/articles/3276224-codon-optimize-dna-sequences) . To see the feature in action and get your questions answered, don’t forget to register for our upcoming webinar, *Codon Optimization with Benchling.*
