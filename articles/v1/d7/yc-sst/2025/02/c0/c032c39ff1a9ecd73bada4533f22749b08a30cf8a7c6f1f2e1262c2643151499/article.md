---
schema_version: "1.0.0"
document_id: "c032c39ff1a9ecd73bada4533f22749b08a30cf8a7c6f1f2e1262c2643151499"
company_key: "yc-sst"
company: "SST"
source_id: "yc-sst-news-import-9375d5a7fa7b"
canonical_url: "https://sst.dev/blog/new-console-logs-ui/"
published_at: "2025-02-18T00:00:00+00:00"
first_seen_at: "2026-07-26T01:49:09.857782+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:fe9354c41a92b58feaebf0c814037f837892b840176dbd57fb43bb28893f3518"
---

# New Console Logs UI

[Blog](https://sst.dev/blog)


# New Console Logs UI


[Jay](https://x.com/jayair) February 18, 2025


We recently updated the logs UI in the SST Console. We’ve added the ability to **search** , view **log streams** , and just made it **faster** .


In the Console, you can view logs from your functions, containers, or any other CloudWatch log groups. We added the ability to:


- **Search these logs** for any string. You can also set a search filter and tail the logs. This helps you debug any issues by triggering your app and watching for the generated logs
- **View log streams** . From the search results, you can jump to the log stream that the log message is coming from. Typically, for very active log groups, you’ll see logs from multiple log streams interleaved together. When you view a specific log stream, you can drill down and just view the logs from that specific invocation or request.
- **Faster logs** . We’ve also used one of the newer CloudWatch APIs that makes fetching these logs from your account a lot faster.


---


Check out the[new logs](https://sst.dev/docs/console#logs) in the Console.
