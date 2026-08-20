---
schema_version: "1.0.0"
document_id: "7d0447b994e2f185aa5270a446c1adb30fd016614f2d652c3763567352eb84b1"
company_key: "yc-ledger-investing"
company: "Ledger Investing"
source_id: "yc-ledger-investing-news-import-76567589263d"
canonical_url: "https://www.ledgerinvesting.com/news/bermuda-package-release"
published_at: "2025-02-25T00:00:00+00:00"
first_seen_at: "2026-07-22T02:06:53.692026+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:905d33f8b18e85fb26b326313d1d2b4d66c87cf88c223fad84f3fbb721a80c72"
---

# Bermuda: A New Open-Source Package for Working with Casualty Loss Triangles

We're very excited to announce the release of a new open-source package for working with casualty loss triangles: Bermuda!


When I started working at Ledger, I was frustrated by the lack of great options for working with triangles in Python. I could represent the triangles in Pandas, but general-purpose data frames are annoying for some common actuarial operations on triangles.


On the other hand, existing packages tailored for loss triangles often made assumptions like "every experience period in a triangle is the same length" or "every evaluation date in a triangle will be at month-end" or "you'll never have to deal with a collection of triangles that are an ugly mixture of quarterly and annual, accident-year and policy-year, and four different currencies". These assumptions are violated more often than you'd hope in practice, especially when you don't have control over the data being generated.


So we built our own solution! Bermuda solves both of the problems described above. It treats triangles as immutable data structures that you can manipulate through a rich set of chainable methods. (Actuaries: this means the syntax is easy to pick up and the effects of a chunk of code are easy to predict.) It can handle "normal-looking" triangles without any problems, but it also can deal with some of the ugliest, weirdest, most cobbled-together, assumption-breaking-est triangles you've ever seen. It's got utilities to convert triangles to or from a host of other formats, including data frames, CSV, JSON, and a high-performance custom binary format we've developed.


(By the way, the package got its name because Bermuda is well-known for two things: insurance and bizarre triangles.)


We've been using this package internally at Ledger for four years, so it's seen a lot of use already. We have a solid library of functions for manipulating and processing triangles, and an extensive set of documentation and tutorials for new users looking to get up to speed. We're excited to share this tool with the rest of the actuarial community. All are welcome to use and/or contribute! You can check out the package here:


-


Github:[github.com/LedgerInvesting/bermuda-ledger](https://www.ledgerinvesting.com/news/github.com/LedgerInvesting/bermuda-ledger)


-


Documentation:[ledger-investing-bermuda-ledger.readthedocs-hosted.com](https://ledger-investing-bermuda-ledger.readthedocs-hosted.com/en/latest/)
