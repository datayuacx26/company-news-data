---
schema_version: "1.0.0"
document_id: "58a252994eaff491bd18fd35f88164e505af84ef85a0e0635648399a6d36deea"
company_key: "zoominfo-technologies-inc-common-stock"
company: "ZoomInfo Technologies Inc"
source_id: "zoominfo-technologies-inc-common-stock-rss-ef9b8d7bcd4f"
canonical_url: "https://engineering.zoominfo.com/enhancing-search-migrating-from-traditional-solr-faceting-to-the-json-faceting-api"
published_at: "2024-12-18T15:03:25+00:00"
first_seen_at: "2026-07-20T23:24:42.992125+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:a6be13307eec60c5c10c2ab59810ad7809ba75dd3256842c7aef1f32bd489ec1"
---

# Enhancing Search: Migrating From Traditional Solr Faceting to the JSON Faceting API

Before the transition to Apache Solr 9, it is critical to modernize our codebase by embracing newer approaches supported by future Solr releases. Part of this transition is migrating away from legacy faceting code in favor of the JSON Facet API, which provides an alternative and more sleek solution to sending facets to Solr. This project encapsulates the efforts taken to successfully migrate to this newer API, and insights into future improvements we could potentially make.


## Faceted Search Using ZoomInfo


Faceted search is a powerful tool for users of ZoomInfo’s products, as it allows the user to navigate and filter the large sets of data that ZoomInfo offers its clients. The way it does this is by providing the option to drill down the data by filtering by attributes in the data.


As shown above, the ZoomInfo interface has key facets such as Location, Management Levels, and Job Departments. After the initial search, analyzing the importance of these facets can help break down all the data a client has. These charts can help visualize to clients what facets are prevalent in their initial search, and then can help clients refine their searches.


Diving deeper into ZoomInfo Marketing, we can see how selecting multiple filters allows for a smarter faceted search. Users can combine various facets to create highly targeted searches. The power of faceted search lies in its ability to provide a dynamic search experience with visuals that represent search results obtained through faceting. This helps users not only navigate through data but also discover patterns and insights within the data that might not be immediately apparent.


## Why JSON Facet API?


Before the introduction of the JSON Facet API, Apache Solr users relied on traditional faceting methods to enhance search experiences. To understand the importance of the JSON Facet API, let’s delve into the nuances which separate the legacy methodologies from the newer implementations.


### Limitations of Traditional Faceting


Traditional faceting methods in Apache Solr, while effective, have several drawbacks that the JSON Facet API aims to fix:


1. **Verbose Code** : Constructing complex facet requests often resulted in lengthy and difficult-to-read code, reducing knowledge transferring capabilities due to the nature of the codebase.
2. **Limited Flexibility** : Traditional facets come with a very rigid structure, making it difficult to create intricate, nested facets.
3. **Performance** **Bottlenecks** : Legacy facets come with increased request latency and drastically inefficient garbage collection procedures compared to the JSON Facet API.
4. **Readability** **Concerns** : Responses are difficult to parse and understand, especially for hierarchical facets. The nesting and structure of JSON objects is far easier to understand as opposed to the flat namespace of the legacy faceting techniques.


### Advantages of JSON Facet API


JSON Facet API addresses these limitations and offers several advantages, making it far more favorable to adopt:


1. **Intuitive** **Structure** : JSON is a format many are familiar with, and even for those unfamiliar with it, JSON’s inherent nested structure allows for a very natural representation of complex facet relationships.
2. **Improved** **Flexibility** : Facet requests are easily constructed with several Java classes which provide support for constructing complicated facets with several parameters in a structured manner.
3. **Better** **Readability:** Both the requests and responses are readable by the untrained eye, which helps improve debugging amongst developers and data interpretation.


### Under the Hood Analysis


As mentioned above, how a SolrQuery looks under the hood varies greatly between the JSON Facet API and legacy faceting code. When a developer is debugging and viewing the state of all variables, how the SolrQuery is structured is important, and as seen below, there are clear advantages to the JSON Facet API.


- JSON structure provides a clean, hierarchical representation of facets. Each facet is a self-contained object with its own parameters, making it easier to understand the facet’s configuration at a glance.
- Easier to debug and maintain complex faceting logic, as changes and additions to facets are more straightforward.
- Designed with extensibility in mind, as additions in future Solr releases will be easier to integrate due to its flexible structure.


This JSON structure replaces traditional parameters like:


- Traditional parameters are flat and become difficult for complex faceting scenarios. Query string becomes harder and harder to read with more additions.
- Debugging is especially challenging due to the long-winded string that SolrQuery can become.
- Less flexible for future enhancements due to its parameter-based approach.


## Solution: Embracing the JSON Facet API


The JSON Facet API offers a robust solution for the issues presented by the deprecated legacy faceting solution. Providing a more intuitive, flexible, and powerful way of constructing facet queries, this API requires extensive modifications of current faceting code, as well as parsing the responses and building the appropriate responses. Let’s explore these steps in greater detail:


1. Understanding Existing Implementation to Send Requests


Legacy faceting code has a fundamentally different approach to setting the parameters of facets. Rather than working to create several instances of facets to be sent to Solr, the SolrQuery is modified itself to accommodate for facet usage, as defined by the user who made the request. Below, we see a chart which delineates the way in which legacy facets are curated and sent to Solr.


1. Understanding Existing Response Handling


After the response is received from Solr, the response must be handled accordingly. Legacy facet responses from Solr are currently equipped with functions which perform much of the heavy lifting when it comes to logic and obtaining the relevant information needed to build a valuable client response. The QueryResponse class provides methods to conveniently obtain facet fields and facet queries to prepare the final facet data for client consumption. The chart below illustrates the processes in which legacy faceting responses are handled, starting from extracting the facet results, applying logic to prepare these responses for post-processing, and building a cohesive and useful result:


1. Reworking to follow JSON Facet API Design Pattern


The JSON Facet API leverages several useful and practical classes to compartmentalize all the different types of facets used to make a developer’s life far simpler. Rather than directly manipulating the SolrQuery, the facet requests are defined through a readable Map which forms each facet in a JSON-like manner. Under the hood, we can easily read the JSON facet which will be sent to Solr, allowing for more flexibility when debugging and modifying facets. The key differences are further discussed and shown below:


- The SolrQuery is not modified until the JSON facet is set manually, allowing for the ability to build deeper and more complex facets to Solr.
- Flexibility enhancements allowed by defining multiple facet types.
- Clear visualization of how different facet types are incorporated into a single JSON facet map, along with their state.


1. Handling JSON Facet Responses


Contrary to the legacy code, the JSON Facet API requires a more manual approach to processing all the facet data. The flexibility of the JSON Facet API allows for more complex and varied structure, which is why the API provides lower-level access to the facet data. This gives developers more control but does require more manual processing compared to the “simpler” traditional faceting. The chart demonstrates this process, which involves extracting the JSON faceting response and drilling down on the bucket-based facets and the query facets using methods introduced exclusively for the JSON Facet API. Highlighted below is a chart showing the process and methods utilized to streamline the process:


1. Abstraction


A valuable thing to note is the idea of abstraction. When migrating from traditional faceting to the JSON Facet API in Solr, abstraction is crucial in maintaining consistency for client applications. By meticulously implementing the response processing methodologies, a developer can shield clients from the underlying changes in the faceting mechanism, ensuring that the response structure remains unchanged despite the shift in the backend implementation.


## Future Scope


1. Transition to JsonQueryRequest from SolrQuery


Currently, SolrQuery is used everywhere when building queries to be sent to Solr. Switching to using only JsonQueryRequest as the container holding query parameters allows greater use of the JSON Facet API, as this class contains many methods to further streamline the process of defining facets. JsonQueryRequest provides great support for the JSON Facet API, as any JsonFacetMap can be configured to be set as the facet for each request directly, removing much of the overhead associated with constructing a facet request.


Old SolrQuery Approach New JsonQueryRequest Approach


SolrQuery solrQuery = new SolrQuery();solrQuery.setQuery(“*:*”);solrQuery.addFacetField(“category”);solrQuery.setFacetMinCount(1);solrQuery.setRows(0); TermsFacetMap categoryFacet = new TermsFacetMap(“category”).setMinCount(1);TermsFacetMap companyFacet = new TermsFacetMap(“company”);JsonQueryRequest jsonRequest = new JsonQueryRequest() .setQuery(“*:*”) .setLimit(0) .setMinCount(5) .withFacet(“category”, categoryFacet);
