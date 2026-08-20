---
schema_version: "1.0.0"
document_id: "10c1dd3e6690fec9dbd972ac9967d19f95aa66a231087c1c1925ed509d3e9d47"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/no-code-request-routing/"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:23.235793+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:70eb02438e5da612f913240789a4cb3191f9e1d1ed35a1d578074b34df66ac38"
---

# No Code Request Routing

Modern applications don't live at a single origin. You've got API servers, static assets, auth services, media pipelines, each needing traffic to route correctly. Getting there meant complex configuration: multiple domains, custom VCL, compute code sitting in front of your origin, or the use of service chaining.


**Request Routing** can simplify this for you.


## One Domain, Multiple Services


With Request Routing, you define path-based rules on any domain in our Domain Management system, and we send each request to the right service automatically, no custom code required.


Host your marketing pages on a static CDN, your API on a containerized backend, and your media files on object storage, all under one domain. Route` /api/*` to your app server,` /assets/*` to S3,` /auth/*` to your identity provider, clean, fast, and entirely declarative.


### Use Cases


-


**Microservices Orchestration** : Route different API versions to specific legacy or modern services.


-


**Geographic Localization** : Direct users from specific countries to localized origin servers to reduce latency.


-


**A/B Testing & Canary Deploys** : Use header-based matching to route a percentage of traffic to staging or beta services.


## How It Works


Route configurations live under the **Domains** top-level navigation in your dashboard. You can create sophisticated rules using three primary condition types. Our first release will only support **headers** , but the other condition types are coming soon.


-


**Header** : Match based on HTTP headers like API versions or User-Agents.


-


**Geo** : Route traffic based on the user's geographic location, such as country codes.


-


**IP** : Filter and route based on specific IP addresses or ranges.


For this release, you can check whether a header value` starts_with` ,` ends_with` ,` equals` , or` contains` a given string. Every path can define multiple rules, and each rule has an associated action. The action determines what happens when/if a condition is matched. For the initial release we're providing a 'service' action that takes a Service ID as its value (we have plans for other action types, coming soon).Every path requires a default action, a fallback route for when no rules match.


Here's a simple example of routing traffic based on the User-Agent request header:


Copied!


```text
Route configuration:
Path: /example
Rule:
Condition: header User-Agent starts_with "Mozilla/5.0"
Action: route to service-A
Rule:
Condition: header User-Agent starts_with "Mozilla/4.0"
Action: route to service-B
Default: route to service-C
```


The system evaluates requests to /example top-to-bottom. The first matching rule wins; anything that does not match any condition falls through to the default.


## The Draft-to-Deploy Workflow


Request Routing ensures safety and increases developer velocity by following a streamlined "Draft-to-Deploy" workflow:


1.


**Draft** : When you start an edit, we automatically create a draft by cloning your active configuration.


2.


**Configure** : Add paths and define rules. Each route configuration supports up to 10,000 paths, 20 rules per path, and 5 conditions per rule.


3.


**Preview** : Use the Structured Diff API endpoint or UI to review exactly what changes you made before going live.


4.


**Activate** : Deploy your changes to the edge with a single call. Strict validation ensures every path has a fallback rule.


5.


**Rollback** : We store the last five active versions, allowing for near-instant rollbacks to a known-good state.


## Get Started


Request Routing is available now on all plans. Head to your dashboard to create your first configuration, or read[the documentation](https://www.fastly.com/documentation/reference/api/domain-management/routing-configs/) for rule syntax, wildcard matching, and advanced options.


Building something interesting with it? Share it on the[Fastly community](https://community.fastly.com/) or drop us a line at[Fastly Support](https://support.fastly.com/s/) .
