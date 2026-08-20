---
schema_version: "1.0.0"
document_id: "cdbc207827013d4b69e31bb870173e747417032ba9c7851a3b8bf909466d831d"
company_key: "cars-com-inc-common-stock"
company: "Cars.com Inc."
source_id: "cars-com-inc-common-stock-rss-49a4db916ec1"
canonical_url: "https://tech.cars.com/cars-com-journey-to-elasticsearch-e86713e1ece3"
published_at: "2019-11-08T21:20:01+00:00"
first_seen_at: "2026-07-20T23:17:02.597310+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:18fd299162b73ba3e7e8ede69ac451ea5a8f0927025fdd8e5af07bf8d37d4fb9"
---

# Cars.com Journey to Elasticsearch

Elasticsearch


Cars Com Inc


Endeca


Aws Elasticsearch


Search Engines


# **Cars.com Journey to Elasticsearch**


[Danish Jawa](https://medium.com/@danishjawa?source=post_page---byline--e86713e1ece3---------------------------------------)


7 min read


·


Nov 8, 2019


--


Imagine you are searching for a car on[Cars.com](https://www.cars.com/for-sale/searchresults.action/?dealerType=localOnly&page=1&perPage=20&rd=99999&searchSource=GN_REFINEMENT&sort=relevance&zc=60606) . You select a make and model and click search and wait for several seconds to see the results. Every time you change the filter option, you wait for several seconds to see the change reflected on your result set. Imagine as a dealer you make a change to your listed vehicle and have to wait for a day to see the change reflected on the web site. This is the situation that Cars.com found itself in and decided to look into Elasticsearch to improve user search experience. Before I walk you through the Cars.com decision to transition to Elasticsearch, here are a few facts about the Cars.com search engine. Cars.com serves an average peak traffic of 18–20K requests per minute. The search is geographically aware and supports multiple facet options with single or multi-selects. The facet options include vehicle features, machine generated data such as price badges, hot car flags, and user generated data such as vehicle or dealer reviews.


At the time Cars.com was using Endeca for its search needs. The objective here was to consider transitioning the Cars.com search engine from Endeca to Elasticsearch to improve (1) search response time, (2) the listings speed to glass times and (3) the ease of feature development. I was tasked to evaluate Elasticsearch, keeping our current and future search needs in mind. I had two challenges to tackle at once. The first challenge was to learn Elasticsearch and the second was to make sure the Cars.com business needs are met. I had a high-level introduction to Elasticsearch but no practical experience. As the first step, I spun up an AWS Elasticsearch instance and created an index with most of the default values. It was surprisingly simple and easy to populate the index and run some basic queries.


Although I was able to get up and running with Elasticsearch quickly, I realized it required a deeper understanding to scale and optimize the search. I read “[Elasticsearch: Definitive Guide](https://www.amazon.com/Elasticsearch-Definitive-Distributed-Real-Time-Analytics-dp-1449358543/dp/1449358543/ref=mt_paperback?_encoding=UTF8&me=&qid=) ” cover-to-cover in order to have a better understanding of how Elasticsearch works. As compared to Endeca, the biggest difference is that Elasticsearch has a distributed dataset while Endeca uses a[proprietary data structure](https://docs.oracle.com/cd/E29584_01/webhelp/mdex_basicDev/src/cqs_MDEX_package_overview.html) for its search needs.


> **Distributed Data and Elasticsearch Main components:**
>
>
> As the name suggests, the complete dataset does not reside in one place. Elasticsearch has the concepts of clusters, nodes, and shards in order to organize the dataset and to run search queries efficiently. Here are the high-level components defined in Elasticsearch:
>
>
> **Cluster** is one or more Elasticsearch instances with the same cluster name.
>
>
> **Node** is a single running instance of Elasticsearch. A node can have a specific role within a cluster. A node can be a master node, a data node or both.
>
>
> **Master Node** does not hold any data. It is only involved in cluster wide changes, for example, adding a node to a cluster, create/delete an index. A master Node is not involved in document level changes.
>
>
> **Data Node** performs document level operations like index, search, aggregation, deletion, and/or additions.
>
>
> **Shard** is a subset of the data and is stored on a node. A set of shards (defined in configuration at the time of index creation) will have the complete dataset. There can be two types of Shards i.e. primary or replica. Primary shard holds the master data and the replica shard is a copy of the master data.
>
>
> Helpful facts about Elasticsearch operations:
>
>
> · Each shard is an[Apache Lucene](https://lucene.apache.org/core/) Index and the shard independently is a searchable index.
>
>
> · Any node within a cluster can receive a search or indexing request. However, it may reroute the request to the appropriate node.
>
>
> · The node with the primary shard will orchestrate any write operation. Although configurable, primary shard writes to the index and will not give acknowledgement to the caller until it ensures the write operation on each replica shard is completed.
>
>
> · Any node (with primary or replica shard) can orchestrate the search operation. Every search request gets executed by each shard primary or replica to make the complete dataset. At the end, the orchestrating node will summarize the results.


In order to create the Elasticsearch index, the first question to answer is: “what’s the optimal number of shards”? Like any real-world problem, there is no one perfect answer for it. It really depends upon what’s your expected search response time and your expected indexing time. If you have two few or too many shards, then your search/indexing performance can suffer. Maybe you have to configure your index with a few options and A/B test your configuration to have a satisfactory result. In the case of Cars.com we decided to go with 6 shards index for approximately 4 million cars listings. The next step was to benchmark response times and indexing time in comparison to Endeca as the baseline.


Let’s look into some of the underlying factors that could impact the comparison of the two technologies, factors like dataset size, un-cached vs cached data, and network infrastructure. In case of this POC (proof of concept), Endeca was running on the on-prem hardware (8 CPU and 32G memory) 2 instances, whereas Elasticsearch was running on AWS r4.xlarge.elasticsearch instance (i.e. 4 vCPU and 30G memory), 6 data nodes (6 primary shards only).


Cached results can artificially improve the response time. Elasticsearch and Endeca both support query caching. However, the architectures of the two technologies make caching behave differently. Let’s say we have an Endeca cluster with 6 instances. If you run a search twice, then there is 1/6 chance of hitting the same node in order to get the cached result. On the other hand, let’s say we have 6 shards in the Elasticsearch index with one replica for each shard; if you run a search twice then there is ½ chance of hitting the same shard in order to get the cached result. In the multi instance cluster scenario, Elasticsearch has a greater chance of returning a cached result given the query is cached. On a side note, Cars.com experiences wide variation of searches, so caching for performance boost was an important but not a critical aspect.


## Get Danish Jawa’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


The variation among underlying factors made it very difficult to compare the two technologies in an identical environment. It was a critical decision to come up with a measure that we can relate between the two technologies. After some debate and discussion, we decided to look at the processing time that a query takes on each search engine rather than measuring on the client side where network latency can have an impact. We came up with the following criteria to measure the performance:


1) Processing times for our top 6 most commonly performed queries in an isolated environment both un-cached and cached. It included:


> a) Ford/F-150 Nationwide
>
>
> b) Ford/All Models Nationwide
>
>
> c) All Makes/All Models Nationwide
>
>
> d) Ford/F-150 within 30 miles radius of any location (we picked Chicago)
>
>
> e) Ford/All Models within 30 miles of Chicago
>
>
> f) All Makes/All Models within 30 miles of Chicago


2) Time required to load all inventory and make search engine ready for search. We call this the baseline process.


3) Some non-mission critical measures:


> a) Ease of use for certain business use cases
>
>
> b) Flexibility for future business enhancement
>
>
> c) Vendor agnostic


In the Cars.com use case, Elasticsearch outperformed Endeca substantially. See the processing times below for the above-mentioned queries categorized into un-cached and cached.


Press enter or click to view image in full size


Press enter or click to view image in full size


The Elasticsearch processing times had been revisited since the first POC. Originally, Elasticsearch queries calculated[aggregation](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/search-aggregations.html) on the filtered result set. In the case of the model filter, the business requirement was to calculate[aggregation on pre filtered result](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/search-request-post-filter.html) . Elasticsearch showed better processing time even after the revision.


As for loading the complete inventory, Elasticsearch took ~ 40 minutes vs 6 hours for Endeca. A faster load time meant Cars.com could process a higher volume of listing changes (add, update, and deletes) per day. A dealer can see his changes reflected on the website sooner as well. Additionally, the baseline downstream applications could start early in the day. The applications like daily email price drop and newly listed alerts could run in a performant and timely manner.


As for non-mission critical points. Elasticsearch is an open source project with a strong development community behind it. Moreover, there are a few vendor options available like AWS, Elastic.co being the most popular. You can also decide to manage your own Elasticsearch cluster. Endeca now owned by Oracle doesn’t have a strong web community. A simple google search for Endeca yields 814,000 results vs 23,800,000 for Elasticsearch. Elasticsearch supports REST API and its DSL (domain specific language) JSON syntax is easy to understand and adopt. Elasticsearch’s growing popularity gives it advantage over Endeca.


An “all makes and models nationwide” search is by far the most expensive query in the Cars.com experience. Elasticsearch improved the response time by ~ 70% on this important query. Overall, the search performance and improved listing ingestion process made easy Cars.com’s decision to migrate to Elasticsearch. Since the POC, Cars.com has successfully been using AWS Elasticsearch for over a year. It’s[P90](http://www.quotium.com/performance/90-percentile-response-time/) performance has consistently hovered around 125ms with sustained continuous traffic growth.
