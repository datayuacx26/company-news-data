---
schema_version: "1.0.0"
document_id: "7320bb718738ebca1851aa012a43743d9259bf269a4c987ed5a224d12221ba3e"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/observability-shouldnt-be-private"
published_at: "2021-08-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:1792101d0b01071c06319d7acea774d95ab92b3defb5bf4d4125e3161baad88c"
---

# Observability shouldn't be private

Today we're announcing the availability of API latency metrics in the Authzed dashboard. Now Authzed users can see the performance of their API usage and more easily correlate performance of Authzed with their own applications.


Authzed is a database and service that stores, computes, and validates your application's permissions.


Developers create a schema that models their permissions requirements and use client libraries to apply the schema to the database, insert data into the database, and query the data to efficiently check permissions in their applications.


You can learn more by following our **[Protect Your First App guide](https://docs.authzed.com/guides/first-app/)** or watching the video of us **[modeling GitHub's permission system](https://www.youtube.com/watch?v=x3-B9-ICj0w)** .


## Playing buzzword bingo


Observability is the focus of collecting telemetry such as metrics, logs, and execution traces in order to reduce the friction in understanding the internals of running software. In the past few years, there have been major strides to improve the observability of software in the cloud native ecosystem. The systems powering Authzed are no exception and follow these best practices. Observability data helps us run a stable and performant service for our users. This is where most SaaS products stop, but we think that the SaaS ecosystem can do better.


## What about the users?


While following best practices internally is great for *our team* , it leaves *our users* in a position where they are forced to treat Authzed as an opaque, black-box system. To improve upon this situtation, communicating outages and maintenance via[Status Pages](https://status.authzed.com/) and[Twitter](https://twitter.com/authzed/status/1414684338747166721) has now become commonplace in the industry of SaaS products. Many services, including Authzed, also have service-wide[metrics that are made public](https://status.authzed.com/#system-metrics) on their status pages. But if some of this data is being made public, this begs the question: why keep observability data private at all?


## Removing the veil


Moving forward, our plan is maintain **a single** observability stack for both internal and external usage. The only difference between these two use cases is that users will be only able to query for observability data for their own systems. Observability data will first make its way into the Authzed dashboard, but always with a design requirement that our users will eventually be able to directly query the data sources that power the dashboard.


Because Authzed depends on having low latency communication with customer applications, it was obvious that we start by exposing performance metrics first. Users will now find a "Metrics" tab on their Permissions Systems that contains line graphs with the[95th percentile](https://en.wikipedia.org/wiki/Percentile) latency for the Check, Expand, Read, and Write API requests.


After sufficient testing, we plan to document the process for configuring your own observability stack to use our[Prometheus](https://prometheus.io/) data source for your own visualizations and alerting rules. For those looking for all the awesome technical details as to how we are securing and scaling out our observability stack: we will be covering that in a future blog post.


If tackling problems like these sounds exciting to you, consider checking out our[careers page](https://authzed.com/careers) , as we're currently hiring for the full spectrum of engineering roles.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Playing buzzword bingo
- What about the users?
- Removing the veil
- Additional Reading


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)
