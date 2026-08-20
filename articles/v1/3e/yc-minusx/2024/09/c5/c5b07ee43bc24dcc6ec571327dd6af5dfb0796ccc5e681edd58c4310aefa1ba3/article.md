---
schema_version: "1.0.0"
document_id: "c5b07ee43bc24dcc6ec571327dd6af5dfb0796ccc5e681edd58c4310aefa1ba3"
company_key: "yc-minusx"
company: "MinusX"
source_id: "yc-minusx-rss-04cd28ab33fb"
canonical_url: "https://minusx.ai/blog/foss"
published_at: "2024-09-01T00:00:00+00:00"
first_seen_at: "2026-07-25T14:25:41.183756+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:a6dcd2834e23c613eeada45ff61c622bcc8eb8a86af895557098aade7613c2d2"
---

# MinusX is Now Free Software

> "When we call software “free,” we mean that it respects the users' essential freedoms: the freedom to run it, to study and change it, and to redistribute copies with or without changes. This is a matter of freedom, not price, so think of “free speech,” not “free beer.” —[RMS](https://www.gnu.org/philosophy/open-source-misses-the-point.en.html)


Today, we're releasing MinusX under the MIT license. You can find our code on our[GitHub page](https://www.github.com/minusxai/minusx-metabase) . In this post, I want to share our motivations.


## Trust


In the process of building a browser extension, we were made acutely aware of the power that extensions have. Especially when it comes to software that deals with your data. We want to make sure that you could trust us. But we know that trust is earned, not given.


We hope that releasing the source code publicly, being[transparent](https://minusx.ai/privacy-simplified) about how we use your data, and enabling you to run your own models is a good first step in that direction.


## Extensibility


From the very beginning, we've felt that MinusX could be extended to far more use cases than we could ever build ourselves. The MinusX architecture has a clear separation between the core extension and the apps it supports. This makes it easy to add support for a new app by adding a single file.


For example, if you use an app other than Jupyter or Metabase, you can add support for it without waiting for us. Or, if you use a custom internal tool in your company, you can extend MinusX to operate that as well.


Our approach of retrofitting apps by adding deep integrations allows the model to consider not only the current UI but runtime information, context from across the app, and even API calls. In many ways, this mirrors the approach[comma](https://www.comma.ai/) takes to retrofitting cars with self-driving capabilities.


## Incentives & Alignment


We believe the best products align their incentives with their users. Making MinusX free software and enabling users to run their own models aligns us with our users. We're not in the business of selling your data or locking you into our platform. We're in the business of building the best AI data scientist for you.


We're betting that you trust us and find enough value in MinusX to continue with us. Personally, I believe that managing user trust at scale will help us unlock a lot of value to build a great product and that it's the *right* way of doing business.
