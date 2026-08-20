---
schema_version: "1.0.0"
document_id: "cef9db4b5c402a52310aba738d06d2351dde88be3208634203a7303c1a974b26"
company_key: "amazon-com-inc-common-stock"
company: "Amazon.com Inc."
source_id: "amazon-com-inc-common-stock-rss-4d9f015bc7ad"
canonical_url: "https://aws.amazon.com/about-aws/whats-new/2026/08/Amazon-GameLift-Streams-Shader-Caching/"
published_at: "2026-08-10T21:15:00+00:00"
first_seen_at: "2026-08-10T23:53:59.020385+00:00"
fetched_at: "2026-08-10T23:54:00.848081+00:00"
content_hash: "sha256:327da37299108c4784a9a4a663f6cc2e97619c1e2a1c973811fa3d05dba4e27e"
---

# Amazon GameLift Streams Now Offers Service-managed Shader Caching

Amazon GameLift Streams now manages shader cache capture and distribution for your applications. You capture a shader cache from a stream session, and the service automatically makes it available for future sessions across your streaming locations. No application changes are required.


Capturing shader caches can help reduce loading times and visual stuttering, during the session. With service-managed shader caching, you designate a stream session for capture and run your application to generate the cache. Amazon GameLift Streams then replicates the cache to compatible stream groups and locations, and loads it automatically in future sessions.


You can monitor shader cache status and storage size using the[ListApplicationShaderCaches API](https://docs.aws.amazon.com/gameliftstreams/latest/apireference/API_ListApplicationShaderCaches.html) or the Amazon GameLift Streams console. The feature supports Linux (Ubuntu 22.04), Proton, and Windows Server 2022 runtimes.


You are charged for storage of the latest version of each shader cache. For pricing details, visit the[Amazon GameLift Streams pricing page](https://aws.amazon.com/gamelift/streams/pricing/) . For supported Regions, see the[AWS Region table](https://docs.aws.amazon.com/gameliftstreams/latest/developerguide/regions-quotas-rande.html) .
