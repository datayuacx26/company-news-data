---
schema_version: "1.0.0"
document_id: "cddd0a496da6ffe28f71d5a9bb8fd169a5ad92e381060a9aaa27da6c2fa081da"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/aws-network-firewall-forward-proxy-preview/"
published_at: "2026-08-04T22:24:00+00:00"
first_seen_at: "2026-08-05T01:26:20.491141+00:00"
fetched_at: "2026-08-05T01:38:03.459815+00:00"
content_hash: "sha256:3705b58628dd7e2015f075faac5599bca26a9c29cf2711fe837033895c7971df"
---

# [Preview Announcement] Re-introducing Forward Proxy as AWS Network Firewall Functionality

You can now use your Network Firewall with all its existing filtering capabilities and features as an explicit forward proxy.


On Nov 25, 2025, AWS introduced Network Firewall *proxy* in public preview to help customers exert centralized security controls against data exfiltration and malware injection. At the time, the Network Firewall *proxy* was introduced as a standalone product, separate from Network Firewall *transparent firewall* and used its own separate *proxy* security policy. Customers who tested it in preview shared that they want the Network Firewall *proxy* to maintain parity with Network Firewall’s existing set of capabilities and use the same security policy across the two functionalities *.* In keeping with customer feedback, we are reintroducing explicit *proxy* as a functionality of Network Firewall. With this launch, you can configure Network Firewall with your existing Firewall policy in a new *no-source-preservation* deployment where it can be used as an explicit *proxy* with all its existing features including managed rule groups, active threat defense, Geo-IP filtering, URL and domain category filtering, container attribute-based rules for Amazon EKS and Amazon ECS, etc. You can create a single security policy and use it for both explicit *proxy* and *transparent firewall* functionalities.


Try out AWS Network Firewall in *no-source-preservation* deployment with *proxy* functionality in your test environment today in US East (Ohio) region. *no-source-preservation* Network Firewall is available for free during public preview. For more information, check[no-source-preservation Network Firewall documentation](https://docs.aws.amazon.com/network-firewall/latest/developerguide/nfw-no-source-preservation.html) .
