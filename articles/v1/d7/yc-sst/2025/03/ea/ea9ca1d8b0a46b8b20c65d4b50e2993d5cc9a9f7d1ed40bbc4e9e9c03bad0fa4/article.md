---
schema_version: "1.0.0"
document_id: "ea9ca1d8b0a46b8b20c65d4b50e2993d5cc9a9f7d1ed40bbc4e9e9c03bad0fa4"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/issues-container-support/"
published_at: "2025-03-04T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:22baf8601f4ef1afaa062a1d34016e52b06dcb401ecfe37c541eae477d73f269"
---

# Issues container support

[Blog](https://sst.dev/blog)


# Issues container support


[Jay](https://x.com/jayair) March 4, 2025


[Issues](https://sst.dev/docs/console#issues) now reports errors from Node.js container applications. Previously, only errors from Lambda functions were reported.


---


### Reporting errors


For the Console to automatically report errors, you need to` console.error` an error object.


src/index.ts


```text
console  .  error  (  new     Error  (  "  my-error  "  ));
```


In container applications, your code needs to also import the[SST JS SDK](https://sst.dev/docs/reference/sdk/) .


src/index.ts


```text
import     "  sst  "  ;
console  .  error  (  new     Error  (  "  my-error  "  ));
```


This applies a polyfill to the` console` object to prepend the log lines with a marker that allows Issues to detect errors.


If you are already importing the SDK, you won’t need to add an additional import.


---


### How it works


Issues works by adding a log subscriber to the CloudWatch Log groups in your SST apps. This has a filter that matches anything that looks like an error.


In the case of Lambda functions, the Lambda runtime automatically adds a marker to the logs that the filter matches for. For containers, the SST SDK polyfills the` console` object to add the marker.


[Learn more about Issues](https://sst.dev/docs/console#issues) .
