---
schema_version: "1.0.0"
document_id: "12100dab632a18a9f5f02afe7a3824889a43271e76bd68681c0a07713667e71f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollos-new-api-ip-allowlist-policy"
published_at: "2026-07-15T12:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:79178c9ffa19711ae1a806a6a548426fe4f5ec7d99570037cbce2a741199873a"
---

# Apollo’s New API IP Allowlist Policy

If you run[GraphOS Router](https://www.apollographql.com/graphos-router) or Apollo Gateway behind a corporate proxy or firewall, you may need to configure outbound network access to[Apollo’s domain names](https://www.apollographql.com/trust/graphos-data-privacy-and-compliance) such as uplink.api.apollographql.com, which Router uses to fetch its schema and configuration from GraphOS. Apollo recommends configuring your network access rules based on domain name. However, some network environments require IP-based rules and, for those deployments, you need advance notice when IP addresses change.


## Introducing the Apollo API IP Allowlist Policy


Apollo has published an[API IP Allowlist Policy](http://apollographql.com/trust/api-ip-allowlist-policy) , available now in our Trust Center. The policy documents:


- **Which services are covered,** starting with the GCP endpoints for Apollo Uplink (uplink.api.apollographql.com and persisted-queries.api.apollographql.com).
- **What IP addresses those services currently resolve to,** so you have a single authoritative source of truth.
- **How changes will be communicated,** with at least 60 calendar days’ advance notice before any new IP is added, announced here on the Apollo blog in the Announcements category. See the[policy](http://apollographql.com/trust/api-ip-allowlist-policy) for full details.


This post is itself the first announcement under that policy.


## What’s changing: a second IP per service, effective no earlier than September 16, 2026


Along with the policy, we’re announcing our first planned change: we will be adding a second IP address to each covered service, effective no earlier than September 16, 2026.


Domain Current IP Additional IP


uplink.api.apollographql.com 34.117.186.194 34.149.219.4


persisted-queries.api.apollographql.com 34.36.202.125 8.233.57.214


If you already have configured your network policy to allow your GraphOS Router (or Apollo Gateway) to access the current IP addresses, you should add the second IP address as well before September 16th; otherwise, your installation may be unable to fetch its configuration from Apollo’s Uplink servers after that date.


The reason for maintaining two IPs per service is operational: it gives Apollo the infrastructure flexibility to make future changes with minimal disruption, which means fewer situations requiring customer action down the road.


## What you need to do


## **If you restrict outbound network access by IP address:**


Before September 16, 2026, add the additional IP addresses listed above to your allowlist for the corresponding domains. If you need help or have questions about your specific setup, contact Apollo Technical Support.


## **If you restrict outbound access by domain name, or don’t restrict it at all:**


No action required. This change does not affect you.


## **If you’re not sure how your network is configured:**


Check with your network or infrastructure team. If Router is fetching its schema successfully today, domain-based allowlisting is likely already in place.


## Stay informed


The policy page is the authoritative source for current and upcoming IP changes. All future changes will be announced on the Apollo blog in the Announcements category –[subscribe](https://www.apollographql.com/blog/rss-announcement.xml) to our blog’s announcement RSS feed or bookmark the[policy page](http://apollographql.com/trust/api-ip-allowlist-policy) if IP allowlisting is relevant to your deployment.


Questions? Reach out to Apollo Technical Support or emailsupport@apollographql.com .


Written by


David Glasser


[Read more by David Glasser](https://www.apollographql.com/blog/author/glasser)
