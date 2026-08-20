---
schema_version: "1.0.0"
document_id: "401a1e8e7113fdb6365a30f12c916672f78a13541fe6afcbbc7ba79cc735ae7c"
company_key: "yc-ashby"
company: "Ashby"
source_id: "yc-ashby-news-import-72f22fd66301"
canonical_url: "https://www.ashbyhq.com/blog/engineering/array-at"
published_at: "2022-04-04T00:00:00+00:00"
first_seen_at: "2026-07-21T07:59:34.035471+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:339e67c1ba9181d6cd3b75f02fd1c4783a9152adfec8370fecb9cea92402d14e"
---

# Type Safety with ES2022's Array.prototype.at

ES2022 introduces` Array.prototype.at


` and we've come to love using it.` Array.prototype.at


` can replace using the standard index operator` \[\]


` :


```text
1  const   arr:   number  [] = [];
2
3  arr[  0  ].toString();
4
5   // Same as above!
6  arr.at(  0  ).toString();
```


In the example above, even though index 0 may not have anything there, Typescript is happy to allow us to to call` toString()


` . With` .at()


` , we get a compiler error:


Typescript's compiler error.


` Array.prototype.at


` is not officially supported in all versions of Node. Check[node.green](https://node.green/) to see if you can use it!
