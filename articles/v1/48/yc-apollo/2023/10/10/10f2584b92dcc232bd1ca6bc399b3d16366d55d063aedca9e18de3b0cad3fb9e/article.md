---
schema_version: "1.0.0"
document_id: "10f2584b92dcc232bd1ca6bc399b3d16366d55d063aedca9e18de3b0cad3fb9e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollos-response-to-cve-2023-38545"
published_at: "2023-10-09T14:59:26+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:cac8d0fefd7431289dce0b2a18ebfd75bc0df8d46129bdfefae545cf14fb207f"
---

# Apollo’s Response to CVE-2023-38545

## October 12, 2023 Update


Yesterday, the curl project released[details regarding CVE-2023-38545](https://curl.se/docs/CVE-2023-38545.html) . We want to provide an update on Apollo’s impact from this vulnerability.


As mentioned in our original post, Apollo Router, Apollo Client, Apollo Server, Apollo Kotlin, Apollo iOS, and Rover do not rely on curl and are not affected by this vulnerability. The build processes for these projects do utilize curl, but exclusively communicate with trusted domains and are therefore not impacted.


Additionally, as mentioned previously, Apollo provides installation instructions and installers that leverage curl. These installers exclusively communicate with trusted domains and are therefore not impacted. Nevertheless, we strongly encourage customers to consider applying the curl patch to their environments.


In our original post we also discussed GraphOS, which consists of many containerized microservices. Some of the containers that comprise GraphOS do include affected versions of curl. However, Apollo does not use SOCKS5 proxies to manage traffic within the GraphOS environment. As curl’s maintainers detail, usage of SOCKS5 proxies is a key requirement for exploitation of this CVE. This effectively safeguards GraphOS from exposure to the vulnerability. We will update affected containers as part of our routine vulnerability management processes.


This update contains the latest information we have regarding Apollo’s impact from this vulnerability and we anticipate that this is our final update. Should any new developments arise, we will promptly communicate them to keep you informed and provide any necessary actions to ensure the security of your systems. Thank you for your continued trust in Apollo.


## Original Post


On October 4, 2023, the curl project released a[pre-announcement for CVE-2023-38545](https://github.com/curl/curl/discussions/12026) , a high-severity vulnerability found in curl and libcurl. However, the specific details of the vulnerability will not be disclosed until October 11. The curl project maintainers mentioned in their announcement that while this vulnerability will not affect all curl users, it will impact many.


Due to the lack of public details, it is not currently possible to determine who will be impacted by this vulnerability. To provide transparency for our customers, Apollo has conducted an initial review of our projects to identify those that rely on curl. Our goal is to help you understand if any updates for your applications might be necessary if Apollo’s products are affected.


We have not found usage of curl in any of the following projects:


- Apollo Router. Apollo Router containers prior to version 1.20.0 included curl but did not utilize it at runtime. Starting from version 1.20.0 of the container, curl was removed entirely. Apollo Router binaries also do not rely on curl.
- Apollo Client
- Apollo Server
- Apollo Kotlin
- Apollo iOS
- Rover


Regarding GraphOS Studio:


- GraphOS Studio consists of many microservices. Apollo is currently conducting a review of these microservices to identify any dependencies on curl. Appropriate updates will be applied to the services behind GraphOS Studio to address any impact from this vulnerability.


Throughout the build processes for the mentioned projects, Apollo does utilize curl. However, our usage of curl in these processes involves requesting resources from trusted URLs. We do not pass externally-supplied URLs to curl. Depending on the nature of the vulnerability, this approach may help mitigate impact on these systems. Should any impact be discovered, Apollo will update our build processes accordingly.


In our documentation, we frequently provide installation instructions, installers, and code samples that involve invoking curl. These examples directly use curl, but they do not indicate a dependency on curl in our projects. We strongly advise our customers to apply any available updates to curl as they become available.


Once the specific details of the vulnerability are made public, Apollo will conduct additional internal reviews and update this blog post to provide further information and clarity regarding impacts we find. We understand the importance of keeping our customers informed, and we will promptly communicate any relevant updates or actions that need to be taken to address the vulnerability.


Written by


Matt Peake


[Read more by Matt Peake](https://www.apollographql.com/blog/author/matt-peake)
