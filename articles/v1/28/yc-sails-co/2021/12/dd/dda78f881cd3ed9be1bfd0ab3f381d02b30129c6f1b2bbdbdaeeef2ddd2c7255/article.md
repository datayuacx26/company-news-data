---
schema_version: "1.0.0"
document_id: "dda78f881cd3ed9be1bfd0ab3f381d02b30129c6f1b2bbdbdaeeef2ddd2c7255"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/simulate-latency/"
published_at: "2021-12-02T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:f9835457d28d9db09fa58d62736d2af82271d2b044813b21e40e375a5bee8a4e"
---

# How to simulate latency in a Sails action

I was going through my timeline on Twitter and came across this[tweet](https://twitter.com/themsaid/status/1466019633488609283) by Mohamed Said on how he simulates latency in his local APIs.


> Local frontends fetches data very quickly from local APIs. Which makes you miss the loading state. - Mohamed Said


Seeing the above tweet I recalled that Sails actually made provision for such a scenario and provides us with a property in our[actions2](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2/) actions to do just that.


## The simulateLatency property


Sails made it super easy to simulate latency in your actions2 actions. To do so pass in the number of milliseconds you want to simulate the latency for i.e how long you want the latency on that action to be, to a top-level` simulateLatency` property in your actions options.


```text
// api/controllers/user/signin.js
module  .  exports   =   {
friendlyName:   'Signin'  ,
description:   'Signs in a user'  ,


// Simulate latency for 1000 milliseconds
simulateLatency:   1000


inputs: {},


exits: {},


fn:   async   () {}
}
```


In the above example action, I omitted the rest of the action to focus on the` simulateLatency` property.


## Conclusion


Alright! So now you have seen how not to miss loading state in your local frontend when making a request to a local Sails API. Happy deploying 🚀
