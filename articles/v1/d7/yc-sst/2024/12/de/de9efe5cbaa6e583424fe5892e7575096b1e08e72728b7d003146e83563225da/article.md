---
schema_version: "1.0.0"
document_id: "de9efe5cbaa6e583424fe5892e7575096b1e08e72728b7d003146e83563225da"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/go-runtime-support/"
published_at: "2024-12-20T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:5cdf49cb4a59f6ae9b1ca94a28cd57ba9a59945d97ed4111e91f9b81457dc5c6"
---

# Go runtime support

SST v3 now supports the[go](https://sst.dev/docs/component/aws/function#runtime) runtime for your Lambda functions. Golang is a great option for tasks that are more compute intensive and it also has faster cold starts. We talk about it here:


[Play](https://youtube.com/watch?v=WTAeW0wyTzA)


You can use Go in SST by setting the` runtime` prop and pointing the` handler` to the directory with your Go function.


sst.config.ts


```text
new   sst  .  aws  .  Function  (  "  MyFunction  "  , {         runtime:   "  go  "  ,         link: [bucket],         handler:   "  ./src  "    });
```


For more details, check out our[Go example](https://sst.dev/docs/examples/#aws-lambda-go) .


---


Also in this update:


1.


The[sst dev](https://sst.dev/docs/reference/cli/#dev) CLI supports running your Go functions[Live](https://sst.dev/docs/live/) .


2.


You can use our new[Go SDK](https://sst.dev/docs/reference/sdk/#golang) to access linked resources in your Go functions or container applications.


src/main.go


```text
import   (        "  github.com/sst/sst/v3/sdk/golang/resource  "    )
resource  .  Get  (  "  MyBucket  "  ,   "  name  "  )
```


The Go SDK does not support the[clients](https://sst.dev/docs/reference/sdk/#clients) that are available in the JS SDK.


3.


You can view Go function logs in the[Console](https://sst.dev/docs/console/) . Support for[Issues](https://sst.dev/docs/console/#issues) is coming soon.
