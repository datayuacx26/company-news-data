---
schema_version: "1.0.0"
document_id: "fe06273b9e8677edc140d80115fdc1dc25a4a469779b5755fee65d48754838fe"
company_key: "yc-minusx"
company: "MinusX"
source_id: "yc-minusx-rss-04cd28ab33fb"
canonical_url: "https://minusx.ai/blog/memory"
published_at: "2025-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T14:25:41.183756+00:00"
fetched_at: "2026-07-28T22:25:51.205850+00:00"
content_hash: "sha256:39d431856644bc41c22c68636a6e3395a94dba410e0570fa295b84330d79dc3f"
---

# Introducing Memory: minusx.md

MinusX works by determining the "right context" for the surface you're in - be it a SQL query page, question builder page or a dashboard page. This works well if the raw data in your Metabase is clean and unambiguous. Unfortunately this is a rarer than spotting a unicorn on your street corner (but can be spotted in certain places, or so I've heard). When you say profit, do you want MinusX to use gross profit, or net profit? What about commonly used abbreviations in your company - CPI? ARPU? ARR? Should MinusX give you all time data, or do you always want it broken down month-wise? Don't you wish you could just tell this to MinusX and that it could just remember?


#### Introducing minusx.md \[[Docs](https://docs.minusx.ai/en/articles/11675800-memory-minusx-md) \]


Inspired by the dev-ex of[Cursor Rules](https://docs.cursor.com/context/rules) or[Claude.md](https://docs.anthropic.com/en/docs/claude-code/overview) , we're introducing minusx.md! This is a single file that houses all your custom preferences. Do you always prefer line plots over bar plots? Do you always want to use that one specific table? Just go ahead and write it in minusx.md!


Additionally, whenever you give a custom definition, MinusX will prompt you to clarify if you want it remembered. You can even just ask MinusX to remember something, and it will go ahead and store the preference for you, as a memory!


You can add MinusX to your company's Metabase, and experience this for yourself in ~5 mins![Schedule a demo](https://minusx.ai/demo) to find out how.
