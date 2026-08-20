---
schema_version: "1.0.0"
document_id: "85462f7620e691935754ccf0e62d7383be42c800f977f1dc19ca9bdb8679c7eb"
company_key: "yc-precip"
company: "Precip"
source_id: "yc-precip-rss-f59c0f39e1b1"
canonical_url: "https://precip.ai/blog/verdi-weather/"
published_at: "2025-12-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:06.643563+00:00"
fetched_at: "2026-07-28T22:25:00.065453+00:00"
content_hash: "sha256:b9fbb1fe6b9f3af3c9f2ddca8c879458b1d81cd747c2fa72bc77d87b8e7e25f7"
---

# How Verdi shipped hyperlocal weather fast with Precip’s embed UI

# Verdi Weather powered by Precip


Verdi recently launched[Verdi Weather](https://www.verdi.ag/blog/verdi-weather) , bringing hyperlocal weather history and insights directly into their dashboard where growers make irrigation decisions.


By choosing Precip, Verdi was able to deliver the highest-quality weather data to their customers with minimal engineering effort.


## Why this partnership matters to growers


Irrigation is one of the most important variables growers can control, but it’s difficult to get right without accurate, field-level weather context.


With Verdi Weather, growers can see:


- **Current conditions** (temperature, wind, and more)
- **Recent rainfall and snowfall totals**
- **The most recent significant precipitation event**
- **Forecasts, trends, and historical context**


This gives growers the “weather station experience” across an entire operation without needing to install, maintain, or manage physical hardware.


As Verdi’s team put it:


> “Knowing how much rain fell yesterday, last week, or across the whole season changes everything.”


## The technical unlock: ship weather UX without building a weather app


Verdi didn’t want generic weather data, and they didn’t have time to build a full weather platform from scratch. They needed a weather experience that felt native inside their product and fit naturally into existing workflows without spending months building charts, tables, and visualizations.


That’s exactly why we built our hosted embed UI: a complete, self-contained weather page designed to drop into any web product.


Available through the[Precip API](https://api-docs.precip.ai/) , the embed reads coordinates and UI preferences from URL query parameters and renders the requested widgets for a specific location.


```text
<  iframe  >
src="https://api.precip.ai/embed/location?lat=42.378792&lon=-83.7935"
</  iframe  >


```


The result is a beautifully styled, responsive weather page with hyperlocal data that can be embedded into any product in minutes.


## What made the embed work for a real product team


The embed UI is a fully interactive page that powers a complete “location weather” experience. It includes:


- **A composable widget system** , so teams can show exactly what they need (current conditions, recent events, hourly precipitation, cumulative totals, tables, wind, temperature, soil data, snow, and more)
- **Unit control** (` units=metric|imperial` ) so the same embed works across regions and customer preferences
- **Theme control** to match light or dark dashboard styling
- **Responsive layout** that adapts seamlessly to web, tablet, and mobile viewports


Under the hood, the embed page reads query parameters (coordinates, units, widget list, theme preferences) and renders the appropriate widgets for each location.


That’s what allowed Verdi to integrate so quickly: their team could focus on *where* the weather experience should live inside their product, rather than *how* to build a weather UI from scratch.


## Want to build something similar?


If you want to embed Precip data in your own product, our developer portal includes[working examples](https://api-docs.precip.ai/examples) to help you get started quickly.


We’d also love to hear what you’re building. Schedule time with us here:
[https://savvycal.com/precip/demo](https://savvycal.com/precip/demo)
