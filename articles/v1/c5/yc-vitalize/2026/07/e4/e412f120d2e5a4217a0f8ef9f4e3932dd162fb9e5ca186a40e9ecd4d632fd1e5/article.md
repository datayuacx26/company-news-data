---
schema_version: "1.0.0"
document_id: "e412f120d2e5a4217a0f8ef9f4e3932dd162fb9e5ca186a40e9ecd4d632fd1e5"
company_key: "yc-vitalize"
company: "Vitalize"
source_id: "yc-vitalize-news-import-b1320fea2000"
canonical_url: "https://vitalize.care/blog/how-vitalize-made-javascript-dates-10x-faster"
published_at: null
first_seen_at: "2026-07-25T07:03:38.833452+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:9e14bd17e7fd7bc83d60c62441d0e762bfe63fd806db732255578751a9f0a690"
---

# How Vitalize Made Javascript Dates 10x Faster - Vitalize

Working with dates in Javascript appears simple on the surface, but real world scheduling exposes its limits quickly. At[Vitalize](https://vitalize.care/) , we manage staffing across entire hospitals, and hospital calendars diverge from standard ISO rules in significant ways.


A hospital day runs from 07:00 to 31:00. A shift from 1 a.m. to 10 a.m. on November 4th is considered part of November 3rd. Each hospital defines its own week start, weekend boundaries, lunch break rules, and scheduling period length. And that’s before you even get into date operations: subtracting lunch breaks, generating shifts from recurring patterns, or classifying day and night shifts becomes very complex.


When we first built Vitalize, we used[Dayjs](https://day.js.org/en/) . Beyond the[countless timezone bugs](https://github.com/iamkun/dayjs/issues/1260) that is has when doing math, Dayjs applies timezones as a[global patch](https://day.js.org/docs/en/timezone/set-default-timezone) . That causes all sorts of issues on the server; if it’s not scoped correctly, multiple requests from different hospitals can easily end up with incorrect calculations.


Temporal offered a way out of that complexity. The ZonedDateTime (ZDT) API provides precise timezone logic, predictable math, and a standards driven design. Unfortunately,[only Firefox](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Temporal#browser_compatibility) has implemented it to date. Luckily, there is a package[temporal-polyfill](https://www.npmjs.com/package/temporal-polyfill) which does a great job of following the spec with a small bundle size (20kb).


To understand the impact, we built benchmarks comparing Day.js and Temporal side by side. The pattern was clear right away: Temporal ran dramatically faster and produced correct values across daylight savings transitions, while Day.js struggled at scale.


Moving an existing system to Temporal is not a drop in replacement. ZDT was a lot more verbose, and we’d gotten used to the DX of Day.js (which, honestly, is pretty fantastic).


To handle all the scheduling rules a hospital might have, like when the weekend starts and ends or what day the week begins on, we had a pattern called DateFactory. It took in a hospital and returned a class where you could initialize Day.js instances with various formatted strings and perform all the math correctly according to that hospital’s settings.


To bridge the difference, we extended ZDT instances with familiar Day.js style helpers like` .isAfter()` ,` .isBefore()` , and` .toISOString()` . The goal was simple: keep the performance of Temporal without giving up the DX our engineering team relied on.


Formatting got the same treatment.


Rolling it out from there was easy - we migrated parts of our app one by one. Every area that switched to Temporal saw 10x+ performance gains.


We still appreciate the simplicity of Day.js formatting, but Temporal has proven to be a better long term foundation. After 6 months of production use, we’d recommend using Temporal over JavaScript Dates any day.
