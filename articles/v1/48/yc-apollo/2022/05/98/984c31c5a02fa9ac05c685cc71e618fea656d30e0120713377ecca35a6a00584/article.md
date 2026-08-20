---
schema_version: "1.0.0"
document_id: "984c31c5a02fa9ac05c685cc71e618fea656d30e0120713377ecca35a6a00584"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-multi-cloud-support-for-apollo-uplink"
published_at: "2022-05-04T09:50:24+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:95a7587e96708f1dcaa00391531a3281dc1c6846b3fac3673d4661bbc71eb100"
---

# Introducing multi-cloud support for Apollo Uplink

If you’re running services in the cloud, what do you do when your cloud provider goes down? Even if you don’t follow the news, you know that this is something that happens a little too often. The way to protect your services is to make sure you are not reliant on only one cloud provider. You can work to make your own services multi-cloud, and you should, but of course you also need to make sure that the services you rely on are also multi-cloud. That’s why we are so happy to introduce multi-cloud support for Apollo Uplink.


Apollo Uplink is a free service that provides federated Apollo Gateways with a Supergraph Schema, which is just plain GraphQL. If you are using Apollo Managed Federation, then you publish your subgraph schemas into the Apollo Schema registry. Apollo Studio composes the subgraph schemas into a Supergraph schema and makes those schemas available via the Uplink service. Every Apollo Gateway polls Apollo Uplink to see when a new schema is available for download. Now, Apollo Uplink is more reliable because it is multi-cloud.


The diagram below, from the[Apollo Studio documentation about Uplink](https://www.apollographql.com/docs/federation/managed-federation/uplink/) , illustrates how Uplink works.


**How do you use multi-cloud Uplink?**


To start using multi-cloud Uplink, all you need to do is upgrade the` @apollo/gateway` package to the latest version, multi-cloud was introduced in v0.36.0. If you’re running Federation one, then you would do this with:


` npm install @apollo/gateway@latest-1`


And of if you’re running Federation 2 then you’ve been running with multi-cloud Uplink already since Gateway version v2.0.0-alpha.3. To upgrade to the latest Federation use the latest-2 tag like so:


` npm install @apollo/gateway@latest-2`


**How does multi-cloud Uplink work?**


Uplink is now implemented as two independent end-points, one running on Google Cloud (GCP) and one on Amazon Web Services (AWS). When you publish a schema to Apollo Studio, that schema will be made available on both cloud providers. When Apollo Gateway requests schemas from Apollo Uplink, it will alternate between those two providers. If a schema request fails, Apollo Gateway will try the other end-point. If both end-points are down, Gateway will fall back to the previously fetched schema. That means that your Apollo Gateway will not be impacted if one of our two cloud providers is down.


**You can customize Apollo Uplink, but you probably won’t need to**


Most users will be happy with Apollo Gateway’s default Apollo Uplink setup, but Uplink is configurable. You may want to configure the Apollo Gateway to use only one Apollo Uplink endpoint or maybe add your own Uplink end-point. Adding your own Uplink end-point is beyond the scope of this blog post.


**What’s next?**


Apollo Uplink was already a very reliable service providing 99.97% uptime and we’re excited to provide even more stability with our Multi-cloud support. If you’re using Uplink, your next step is to upgrade to the latest Gateway release and enjoy increased reliability just by taking the defaults! And if you’re not using Uplink, maybe you should be. It’s a free service you can rely on to deploy changes to your Apollo Gateways 🚀


Written by


Dave Johnson


[Read more by Dave Johnson](https://www.apollographql.com/blog/author/djohnson)
