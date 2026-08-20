---
schema_version: "1.0.0"
document_id: "d56e91f75819655027840a9b4204c9860f697cd65e671fdfe1a927c2d1dac0db"
company_key: "yc-etleap"
company: "Etleap"
source_id: "yc-etleap-news-import-75d796be5177"
canonical_url: "https://etleap.com/blog/new-feature-user-defined-api-connections"
published_at: null
first_seen_at: "2026-07-21T18:50:49.183689+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:ba465d54e26353bded94fd945c09fc678b0c9f9143910d2c2d4b6f2d7f15ba24"
---

# New feature: User-Defined API Connections

Today, I am proud to announce the launch of User-Defined API Connections in Etleap. We developed this feature in response to one of the most frequent user requests: how do I ingest data from APIs that isn’t a[pre-built integration](https://etleap.com/integrations/) ? With User-Defined API Connections, users can quickly and seamlessly ingest data from any API that meets[these criteria](https://etleap.stoplight.io/docs/documentation/ycl5d932ve9h6-user-defined-ap-is#api-requirements) .


Until now, integrating data from unsupported APIs required custom coding, taking up time and engineering resources. User-Defined API Connections removes this barrier, providing users a configuration-based approach that perfectly aligns with Etleap's core mission: streamlining ETL processes and minimizing engineering overhead.


## Removing Coding Roadblocks by Leveraging Experience


Our team has built a large number of API integrations over the years, which has deepened our understanding of the unique complexities of various APIs, including authentication methods, pagination strategies, and data change capture mechanisms. By identifying patterns across APIs we have arrived at a minimal set of configuration options and parameters that cater to a breadth of API scenarios (there are some current limitations - more on that below).


Additionally, through many iterations of design and user research we have crafted a step-by-step wizard that guides users through the configuration process. For example, after inputting a URL and authentication parameters, users can see sampled data from the API responses, enabling them to pinpoint what to extract from the response and confirm the correct pagination strategy. Providing feedback and propagating error messages from the API along the way helps get parameters correct. This keeps the setup intuitive, even for those less familiar with API intricacies.


## How does it work?


User-Defined API Connections support two[pipeline modes](https://docs.etleap.com/docs/documentation/2f8f4a5cd7bec-pipeline-modes) to start:


1.


[Replace Mode](https://docs.etleap.com/docs/documentation/ZG9jOjIyMjE5NDgz-replace-mode) : Ideal for situations where frequent updates are not necessary. This mode retrieves the entire dataset on each pipeline run. Replace-mode pipelines ensure a complete and consistent snapshot of the data at the time of ingestion. It is the most straightforward to configure: provide a URL, and set the authentication parameters and pagination strategy (optional).


2.


[Update Mode](https://docs.etleap.com/docs/documentation/c05fb4de380ba-update-mode) : Optimized for large datasets or APIs with call quotas, this mode minimizes data[pipeline latency](https://etleap.stoplight.io/docs/documentation/e876f3e80b4c2-latency#end-to-end-latency) and API call volume. The connection-setup wizard guides users through a few more steps that tell Etleap how to fetch new and updated records from the API based on date ranges.


## Will it work for my API?


It’s hard to create a one-size-fits-all API connector that works in all scenarios. Even seemingly standardized APIs can exhibit idiosyncrasies. What we are launching today is a minimally-viable feature, with some current limitations that we’re aware of. One of them is the ability to handle what we refer to as *recursive extraction* , which is when data from one API call is used to fetch data in another API call. We will listen attentively to user feedback in order to evolve this feature and make it work for more scenarios over time.


We hope you’ll take it for a spin and let us know what you think! To get started, go to the[New Connection](https://app.etleap.com/#/connections/new) page and pick[User-Defined API](https://app.etleap.com/#/connections/new/USER_DEFINED_API) . You can read more about capabilities and requirements in[our documentation](https://docs.etleap.com/docs/documentation/ycl5d932ve9h6-user-defined-ap-is) .


If you’re not yet using Etleap, contact us[here](https://etleap.com/demo/) or start an Etleap trial from your Snowflake and Redshift dashboards.
