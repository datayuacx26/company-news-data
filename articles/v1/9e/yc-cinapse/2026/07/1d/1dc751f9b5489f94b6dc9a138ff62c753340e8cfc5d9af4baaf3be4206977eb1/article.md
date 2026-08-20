---
schema_version: "1.0.0"
document_id: "1dc751f9b5489f94b6dc9a138ff62c753340e8cfc5d9af4baaf3be4206977eb1"
company_key: "yc-cinapse"
company: "Cinapse"
source_id: "yc-cinapse-news-import-fffd8c17df07"
canonical_url: "https://www.cinapse.io/resources/whats-new-in-cinapse----july-2026"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-24T23:08:49.299142+00:00"
fetched_at: "2026-07-28T21:21:05.434568+00:00"
content_hash: "sha256:d9906c1b057df1b4090699f6e19dcd85d86ba45784ef6d14908e7f83d4a1fa5f"
---

# What's New in Cinapse — July 2026

## Control the Weather


You can’t control the weather (even though every AD wishes they could). You can, however, control exactly how it displays in Cinapse.


Weather drives real scheduling decisions, so this release gives you full control over how it appears. Display temperatures in Celsius or Fahrenheit, and pick exactly which parts of the forecast show on the breakdown sheet, so you see what matters and skip what doesn't. Sun times extend to your banners as well: use **@sunrise** and **@sunset** directly in text banners, or drop the **{Sunrise}** and **{Sunset}** tokens into your daybreak and start-of-day banners through the Layout Manager.


## Merge Duplicate Elements


Breaking down a script generates duplicates. The same prop, the same vehicle, the same background element shows up across scenes, and cleaning it up by hand is tedious and error-prone. You can now merge duplicate elements directly on the breakdown sheet: select the ones that belong together, merge them, and you're left with a single element that consolidates the details. No re-import and no manual reconciliation. It's on the breakdown sheet, and accessible via the Element Manager.


## Reports for Any Category


List reports and category-by-day reports were previously constrained in the categories they could work with, which meant workarounds when you needed something specific. Both report types now work with *any* category. Build a location report, a set report, or a Scenes by Day vs Night report and generate it directly, no restructuring required. Run them from the reports menu.


## More Features. More Improvements.


**Terms acceptance.** Terms are confirmed at signup and again whenever they're updated.


**Bot protection at signup.** A captcha now guards the signup flow against automated accounts.


**Stability and fixes**


- Hand-entered breakdown quantities survive re-import
- Breakdown summaries resolve script day from the element, not the shoot day
- Tier limits copy correctly on Starter to Professional upgrades, and hold through Professional pause cycles
- Banners handle missing sun times gracefully
- More reliable sync and error reporting
- Security patch for sign-in redirects
- A round of QA polish
