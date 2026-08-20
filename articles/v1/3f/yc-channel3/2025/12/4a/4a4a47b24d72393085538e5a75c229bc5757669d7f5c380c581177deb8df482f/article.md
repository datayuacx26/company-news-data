---
schema_version: "1.0.0"
document_id: "4a4a47b24d72393085538e5a75c229bc5757669d7f5c380c581177deb8df482f"
company_key: "yc-channel3"
company: "Channel3"
source_id: "yc-channel3-news-import-a892b65a43cc"
canonical_url: "https://trychannel3.com/blog/add-product-recommendations-to-your-app"
published_at: "2025-12-18T00:00:00+00:00"
first_seen_at: "2026-07-21T13:12:57.086694+00:00"
fetched_at: "2026-07-28T22:24:55.411240+00:00"
content_hash: "sha256:952126b0d0b39f1778c9e31b03c54db17dffc90295e890323013653c731e4873"
---

# How to add product recommendations to your app

Product recommendations can boost engagement and revenue — whether you're building a style app, a gift finder, or an AI assistant. This guide shows how to add recommendations using a single API: query by text or image, apply filters, and get results you can display or pass to an LLM.


## Choose your entry point


**Text search:** User types or says something like "running shoes under $100." Call the search API with that query, optional filters (brand, price range, category), and a limit. You get back products with titles, images, prices, and links.


**Image search:** User uploads a photo (e.g. "find something like this"). Send the image URL or base64 to the API and get visually or semantically similar products. Great for "find similar" or style-matching features.


## Filters and options


Narrow results by brand, gender, price min/max, and availability. You can also pass context (e.g. "gift for new parent") to improve relevance. Use the SDK's typed options or the REST API — both support the same parameters.


## From API to UI


Once you have results, render them in a grid or list. Each product typically includes an image, title, price, and link. Use the affiliate link from the response so clicks and purchases are attributed to you. With Channel3, you get a single integration that works for web, mobile, and AI backends.
