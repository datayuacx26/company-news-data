---
schema_version: "1.0.0"
document_id: "2bf6d0712d592bb83aff4148870515a29422a497c25743a22ed7e7c92fbec9b5"
company_key: "yc-channel3"
company: "Channel3"
source_id: "yc-channel3-news-import-a892b65a43cc"
canonical_url: "https://trychannel3.com/blog/search-products-by-color"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-24T01:15:47.748960+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:101c0000daa1091c6fe6c1bf014c3c9df31d9341324b016cdb63698fb20acf8e"
---

# How to Search Products by Color with the Channel3 API (Color Filters, Now in Beta)

*Last updated: June 8, 2026*


**The Channel3 API now supports color filtering on product search: add one or more colors — as hex values — to your search request and you get back products that contain them. It is live in beta today.**


Color is one of the most common ways people describe what they want to buy — and now your shopping agent can filter for it in a single call.


### Key takeaways


- **What's new:** Product search now accepts a` colors` filter — one or more hex colors per request, returning products that contain them. Available in beta now.
- **Why it matters:** Color is a primary dimension of product discovery — a sage-green linen blazer, a matte-black phone case — and matching it precisely makes a shopping agent feel like it understood the request.
- **How:** Add a` colors.palette` array of hex values to the` filters` object on` POST /v1/search` . List multiple colors to require a full palette — they are combined as an AND.


▶ Watch the demo (~27s) ↓


## Why color matters for shopping agents


Color is rarely a nice-to-have in product discovery — it is often the whole intent. Shoppers think in terms of "burgundy," "sage," "off-white," and a result set that ignores color feels like it ignored the request. For agents building on the Channel3 API, filtering by color turns a vague visual intent into a precise, structured query, so the products that come back actually look like what the shopper pictured.


## Inspired by Cosmos


We added color filters after seeing the color search built by[Cosmos](https://cosmos.so/) — a visual discovery app for creatives that lets you search and filter inspiration by color, keyword, or image. Their color search is genuinely one of the best implementations we have seen, and it is what made us want to bring the same precision to product search in the Channel3 API.


## How to use color filters


Color filtering lives in the` filters` object on` POST /v1/search` . Add a` colors.palette` array, where each entry is a color given as an sRGB hex value:


```text
POST /v1/search
{
"query": "leather jacket",
"filters": {
"colors": {
"palette": [
{ "hex": "#000000" }
]
}
}
}
```


Pass more than one color to match a multi-color product. The colors are combined with **AND** , so the API returns only products that contain *every* color you list — not products that contain just one of them. A palette of red and blue, for example, returns products that are red *and* blue (think a red-and-blue varsity jacket), not products that are only red or only blue:


```text
POST /v1/search
{
"query": "leather jacket",
"filters": {
"colors": {
"palette": [
{ "hex": "#000000" },
{ "hex": "#8B4513", "percentage": 0.2 }
]
}
}
}
```


That second call finds leather jackets containing both black and brown, with brown expected at roughly 20% of the product. The optional` percentage` lets you say how much of each color you expect; leave it off and Channel3 applies a sensible default based on how many colors you have listed.


Two things worth knowing:


- **Hex in, perceptual match out.** You pass a hex code, and matching is done by perceptual color distance — so you get products that are visually close to your color, not only exact hex matches.
- **It composes with everything else.** The color filter is just another entry in` filters` , so it stacks with` query` , brand, price, category, and the rest in a single call.


## See it in action


## What you can build


- **Color-aware shopping agents** that honor "find me this, but in forest green."
- **Shop-the-look and palette matching** — require a multi-color palette so results fit a moodboard or a room's color scheme.
- **Better visual-to-product flows** — extract a hex (or a palette) from an image upstream, then pass it straight into a color-filtered product search.


## Try it


Color filters are live in beta now. Add a` colors` filter to your next` POST /v1/search` call and see what comes back.


[Start building →](https://trychannel3.com/)


## FAQ


### How do I filter Channel3 search results by color?


Add a` colors.palette` array to the` filters` object in your` POST /v1/search` request, with each color given as an sRGB hex value. The API returns products that contain those colors, alongside your other query terms and filters.


### Do I pass color names or hex codes?


Hex codes (sRGB, e.g.` #a1b2c3` ). Channel3 matches them perceptually, so a hex finds visually similar products rather than requiring an exact match.


### Can I filter by more than one color?


Yes. List multiple colors in the palette and they are treated as an AND — every color must be present in the product. A red-and-blue palette returns products that are both red and blue, not products that are only one of them, so you can match a whole palette rather than a single hue. You can optionally weight each color with a` percentage` .


### Is color filtering available now?


Yes, it is live in beta in the Channel3 API today. As a beta feature, the exact shape may still change.
