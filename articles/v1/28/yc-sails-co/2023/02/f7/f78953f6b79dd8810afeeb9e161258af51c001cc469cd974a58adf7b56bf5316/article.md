---
schema_version: "1.0.0"
document_id: "f78953f6b79dd8810afeeb9e161258af51c001cc469cd974a58adf7b56bf5316"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/rate-limiting-in-sails/"
published_at: "2023-02-02T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:c9b468341cd7bb708fc3d56135291d0a685f4b16b11ad8761d58a71390ea2343"
---

# Rate limiting in Sails

I recently had to implement rate limiting on the[Conference API](https://github.com/sailscasts/conference-api) - a[Sailscasts community](https://sailscasts.com/chat) project.


The whole idea of rate limiting is to limit the number of requests coming to a particular API in a certain time window.


The Conference API is written with Sails.js and when researching on how to implement rate limiting, I stumbled on the` express-rate-limit` package which handles rate limiting for Express and since Sails is based on Express, I saw an opportunity to wrap this package in a Sails hook to fine-tune the experience of using the package for Sails developers.


## sails-hook-rate-limit


The result of that fine-tuning is[sails-hook-rate-limit](https://github.com/sailscastshq/sails-hook-rate-limit) . This hook simplifies setting up rate limiting in a Sails application by wrapping the` express-rate-limit` package thereby providing a nicer DX for setting up rate limiting in Sails.


## Installation


To use this hook in your Sails project, run the below command in your terminal


```text
npm   i   sails-hook-rate-limit   --save
```


And that’s all you need to setup rate limiting in your Sails API!


## Setup


By default,` sails-hook-rate-limit` set some basic configs that you may or may not want to override.


For example, it sets the` windowMs` to` 10 minutes` , the max request per window to` 100` , set` express-rate-limit` to use standard headers i.e` RateLimit-*` and disables legacy headers i.e` X-RateLimit-*`


To override these values and more config options, create` config/rate-limit.js` and export a` rateLimit` object like so:


```text
module  .  exports  .rateLimit   =   {
// config goes here
}
```


In this config object you can pass in any configuration property that` express-rate-limit` expects. See the[configuration docs](https://github.com/express-rate-limit/express-rate-limit#configuration) of` express-rate-limit` for those properties.


For example let’s say I want to override the default` 10 minutes` window to` 20 minutes` , I will pass the following to` config/rate-limit.js` object.


```text
module  .  exports  .rateLimit   =   {
windowMs:   20   *   60   *   1000   // 20 minutes
}
```


## Conclusion


Rate limiting is frequently used to control the frequency of requests to an API and in this article we covered how to set up rate limiting in your Sails APIs by using the` sails-hook-rate-limit` Sails hook.
