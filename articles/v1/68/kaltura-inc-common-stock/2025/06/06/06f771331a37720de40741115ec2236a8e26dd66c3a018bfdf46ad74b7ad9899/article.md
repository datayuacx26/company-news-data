---
schema_version: "1.0.0"
document_id: "06f771331a37720de40741115ec2236a8e26dd66c3a018bfdf46ad74b7ad9899"
company_key: "kaltura-inc-common-stock"
company: "Kaltura Inc."
source_id: "kaltura-inc-common-stock-rss-8a80d100aa25"
canonical_url: "https://medium.com/kaltura-tech/istio-rate-limit-hands-on-lab-part-2-1e8b5fdad217"
published_at: "2025-06-09T07:02:04+00:00"
first_seen_at: "2026-07-20T23:18:51.911067+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:5cfe1c9a16abdd0080e18cf449979058c3d96453c4ccca4ea72dc7d42df48ca6"
---

# Istio rate limit — Hands on Lab -Part 2

# Istio rate limit — Hands on Lab -Part 2


[Tomer Shaiman](https://medium.com/@tomershaiman?source=post_page---byline--1e8b5fdad217---------------------------------------)


7 min read


·


Jun 4, 2025


--


Press enter or click to view image in full size


Welcome to the second part of our two-part series on advanced Istio rate limiting, featuring a hands-on approach. In Part 1, we covered the basics — setting up and configuring simple rate limits. In this part, we will delve into a more advanced scenario, combining multiple rules across several domains.


[Part 1 : Setup and simple Rate Limit](https://medium.com/p/4dccb9885e64/edit)
Part 2 : Advance Rate limit scenarios <- **You are here**


The code for the lab can be found[here](https://github.com/tshaiman/istio-ratelimit-blog)


Press enter or click to view image in full size


Photo by[Naufal Giffari](https://unsplash.com/@naufalcreed?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on[Unsplash](https://unsplash.com/photos/cars-on-road-near-station-during-night-gPT8reQIxDU?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)


## Example 2 : use more advanced ruling scenarios


Let’s explore a more complex rate-limiting scenario. First, navigate to the` 03-advance-rate-limit` folder and open the` filter-ratelimit-svc.yaml` file:


```text
operation: MERGE        value:          rate_limits:            - actions:                - request_headers:                    header_name: 'Partner'                    descriptor_key: 'PARTNER'                - request_headers:                    header_name: ':path'                    descriptor_key: 'PATH'
```


Then, review the` config.yaml` file located in the same folder:


```text
config.yaml: |      domain: global      descriptors:        - key: PARTNER          descriptors:          - key: PATH            value: "/status/200"            rate_limit:              unit: minute              requests_per_unit: 4
```


In this configuration, we introduced a new descriptor called` PARTNER` , mapped to the` Partner` request header. The` MERGE` operation updates or adds fields without removing existing configurations.


This rate limit configuration includes two actions:


1. It maps the` Partner` header to the descriptor key` PARTNER` .
2. It maps the request URL path (` :path` ) to the descriptor key` PATH` .


Thus, when a request is sent to` httpbin.example.com:80` , the following combined rule applies:


- Requests containing both the` Partner` header and the request path` /status/200` are limited to **4 requests per minute** per unique partner ID.


Since we haven’t specified a particular value for the partner, this rule applies to all unique partner IDs by default.


> **Note:** This configuration uses an **AND** combination — meaning the limit specifically applies to the unique combination of these headers. Exceeding this limit results in a` 429 Too Many Requests` response.


## Custom Rules for Specific Partners


What if we want dedicated rules for specific partners? For instance, let’s say we want Partner` 222` to have a higher rate limit of **6 calls per minute** . Here's how the configuration would look:


```text
domain: global      descriptors:        - key: PARTNER          value: "222"          descriptors:          - key: PATH            value: "/status/200"            rate_limit:              unit: minute              requests_per_unit: 6        - key: PARTNER          descriptors:          - key: PATH            value: "/status/200"            rate_limit:              unit: minute              requests_per_unit: 4
```


Now, Partner` 222` has a customized rate limit, distinct from the default rule.


## **Time For Some Smoke Test**


Press enter or click to view image in full size


Photo by[Pascal Meier](https://unsplash.com/@zhpix?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on[Unsplash](https://unsplash.com/photos/white-smoke-1uVCTVSn-2o?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)


**Partner 222 — First call:**


```text
curl -s -I -H "Host: httpbin.example.com" -H "Partner: 222" "http://$INGRESS_HOST:$INGRESS_PORT/status/200"
```


Press enter or click to view image in full size


For Partner` 222` , the limit is **6 calls per minute** . After the first call, you still have 5 remaining calls within this 60-second window.


**Partner 223 — First call:**


```text
curl -s -I -H "Host: httpbin.example.com" -H "Partner: 223" "http://$INGRESS_HOST:$INGRESS_PORT/status/200"
```


Since Partner` 223` has no specific entry, the default rule of **4 calls per minute** applies.


Press enter or click to view image in full size


**Partner 222 — Second call:**


```text
```


Now, Partner` 222` has **4 calls remaining** within the current time window, as expected.


Press enter or click to view image in full size


## **Understanding Redis State Store**


Let’s deepen our understanding by examining how these rate limits are stored internally. In Part 1, we set up a Redis server as part of our environment. Now, we’ll investigate its contents directly.


We’ll use the` kubectl exec` command to access the Redis pod and interact with it using` redis-cli` :


```text
redis_pod=$(kubectl get pod -l app=redis -n istio-system -o jsonpath='{.items[*].metadata.name}')  kubectl exec -it $redis_pod -n istio-system -- redis-cli
```


First, run the` keys` command to list the stored keys:


```text
keys *
```


Each Partner/Path combination has its own key, along with a timestamp indicating the start of the current time window.


By examining each key individually, we can see the actual count of requests made within each window:


```text
get <key_name>
```


Press enter or click to view image in full size


Press enter or click to view image in full size


This confirms our earlier test results — for instance, Partner` 222` received 2 requests, while Partner` 223` received 1 request.


Pretty neat, right?


## **Domains**


In our final scenario, we explore using multiple domains for even more granular control. Up to now, we’ve been using a single, global domain configured in our Envoy filter as` domain: global` . Domains in Istio's rate limiting allow us to logically separate rules and independently manage rate limits for different parts of our applications.


**Real life scenario**


Let’s consider a practical example where we want to apply rate limits based on combinations of User and Path, in addition to the previously defined rules. Assume we now have two headers:` PARTNER` and` USER_ID` . For instance, let's say user` user123` belongs to partner` 222` .


Initially, our configuration might look like this:


```text
domain: "global"  descriptors:    - key: PARTNER      value: "222"      descriptors:        - key: PATH          value: "/status/200"          rate_limit:            unit: MINUTE            requests_per_unit: 20    # More specific rule    - key: "USER_ID"      value: "user123"      descriptors:        - key: "PATH"          value: "/status/200"          rate_limit:            unit: MINUTE            requests_per_unit: 10
```


Our intention is clear:


- Partner` 222` should be limited to a maximum of 20 calls per minute, regardless of the user ID.
- User` user123` specifically should have a stricter limit of 10 calls per minute.


However, the actual behavior would be different:


- Suppose partner` 222` has made 19 calls without any` USER_ID` header. According to rule #1, they would have just one call remaining.
- However, if the next 10 calls are specifically for` user123` , rule #2 would apply separately, granting an additional 10 calls.


This means Partner` 222` could potentially exceed their intended limit by reaching 29 calls.


To resolve this, we need separate sets of rules (domains) to enforce both constraints independently.


## Key Differences: Single Domain vs. Separate Domains


**Single Domain:**


- Rules are evaluated hierarchically.
- Only the most specific matching rule is applied.
- Cannot enforce multiple rules simultaneously (either user or partner limits, but not both).


**Separate Domains:**


- Rules in different domains are evaluated independently.
- All matching domain rules are applied simultaneously (AND logic).
- Allows enforcement of both partner-level and user-level limits simultaneously.
- Offers greater flexibility for varied rate-limiting policies.


In summary, using a single domain might lead to unintended consequences when evaluating nested rules. Using separate domains ensures that multiple independent constraints can be enforced simultaneously without conflict.


## **Corrected Version:**


The corrected configuration using multiple domains can be found under the` 4-Domains` folder:


```text
apiVersion: v1  kind: ConfigMap  metadata:    name: ratelimit-config    namespace: istio-system    labels:      app: ratelimit  data:    global-rate-limit.yaml: |      domain: global      descriptors:        - key: PARTNER          value: "222"          descriptors:          - key: PATH            value: "/status/200"            rate_limit:              unit: minute              requests_per_unit: 6        - key: PARTNER          descriptors:          - key: PATH            value: "/status/200"            rate_limit:              unit: minute              requests_per_unit: 4    user-rate-limit.yaml: |      domain: user      descriptors:        - key: USER_ID          value: "user123"          descriptors:          - key: PATH            value: "/status/200"            rate_limit:              unit: minute              requests_per_unit: 2
```


> Notice we now have two separate domain keys in the ConfigMap.


In the` filter-rate-limit-svc.yaml` file, we added another actions section:


```text
operation: MERGE        value:          rate_limits:          - actions:            - request_headers:                header_name: 'Partner'                descriptor_key: 'PARTNER'            - request_headers:                header_name: ':path'                descriptor_key: 'PATH'          - actions:            - request_headers:                header_name: 'User'                descriptor_key: 'USER_ID'            - request_headers:                header_name: ':path'                descriptor_key: 'PATH'
```


Additionally, in the` filter-ratelimit.yaml` file, we duplicated the` HTTP_FILTER` section for another domain named "user."


### Final Result


With these adjustments, we can independently enforce rate limits at both user and partner levels:


- For Partner` 222` with any user, the limit is 6 calls per minute (from the "global" domain).
- Specifically, for Partner` 222` with user` user123` , the limit is 2 calls per minute (from the "user" domain).


This provides precise and flexible control over traffic management and rate-limiting rules.


Press enter or click to view image in full size


Partner 222 and user 124


and for Partner 222 with user 123 we are only limited to 2 calls per minute, defined by the “user” domain.


Press enter or click to view image in full size


Partner 222 and user 123


## Conclussion


That concludes our 2-part series on Istio rate limiting. In this series, we learned how to set up the rate limit infrastructure, apply simple rules using headers, and gradually moved into more advanced scenarios — like combining descriptors, exploring Redis state, and using multiple domains for layered enforcement. With these tools, you’re now equipped to build flexible and powerful rate-limiting strategies tailored to your application’s needs.


That’s a wrap !


Press enter or click to view image in full size


Photo by[The Maker Jess](https://unsplash.com/@themakerjess?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on[Unsplash](https://unsplash.com/photos/a-black-and-white-movie-clapper-on-a-white-background-33LxhLSWQx0?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)
