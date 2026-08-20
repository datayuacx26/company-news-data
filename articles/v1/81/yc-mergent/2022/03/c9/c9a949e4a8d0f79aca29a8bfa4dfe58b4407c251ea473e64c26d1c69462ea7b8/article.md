---
schema_version: "1.0.0"
document_id: "c9a949e4a8d0f79aca29a8bfa4dfe58b4407c251ea473e64c26d1c69462ea7b8"
company_key: "yc-mergent"
company: "Mergent"
source_id: "yc-mergent-rss-c4a67b378e23"
canonical_url: "https://blog.mergent.co/announcing-the-mergent-v2-api"
published_at: "2022-03-18T17:38:09+00:00"
first_seen_at: "2026-07-25T13:51:58.649676+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:310767bbc074c1a27781734048efd3f0d91a9e4907aee843d0bc25245ef863cc"
---

# Announcing the Mergent V2 API

We're happy to announce the launch of Mergent's new V2 API, which provides improvements to working with Mergent and a *ton* of behind-the-scenes changes.


There are a few notable changes to the API between V1 and V2.


*As always, feel free to visit our[docs](https://docs.mergent.co/) or[API reference](https://api.mergent.co/docs) directly. We're also available atsupport@mergent.co to answer any questions.*


**There are new library versions.**


If you're using one of the official Mergent libraries, update to the most recent version to use the new V2 API.


**The API prefix is now` /v2` instead of` /v1` .**


If you're using the API directly, change the API prefix to` /v2` . API endpoints prefixed with` /v1` will become inaccessible on May 31st, 2022.


**The rate limit has been changed to 500 requests per second.**


This is a soft limit, and with increases per project available by emailingsupport@mergent.co . We plan on continuing to increase the rate limit over time.


**All resources now require a` queue` attribute when being created.**


By default, the Mergent libraries will set this to` default` . Soon, we will be providing another update with ways to control Tasks within a queue. Stay tuned.


**All resources now return an` id` attribute upon creation.**


Previously, Mergent's APIs used an optional` name` as a primary identifier. Now, Mergent returns an` id` attribute. Please use this for Task/Schedule identification.


**Tasks no longer have a` description` attribute.**


This was a holdover from before Mergent Schedules were separated from Mergent Tasks several months ago. Schedules continue to have a` description` attribute.


**` name` is now strictly used for the deduplication of** ***queued*** **and** ***working*** **tasks.**


Before, Mergent used` name` both as a deduplication key and primary identifier for all resources. Due to the way deduplication was handled behind the scenes,` name` s could not be reused once a Task was completed.


With this change,` name` only deduplicates tasks that are either *queued* or *working* .


## Want to Learn More?


Check out our[docs](https://docs.mergent.co/) , our new[API reference](https://api.mergent.co/docs) , or send us a note atsupport@mergent.co . We're here to help!
