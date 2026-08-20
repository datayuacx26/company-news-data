---
schema_version: "1.0.0"
document_id: "6c5b977cced511ceed464e6161c7312a220141f8a13e2689a996ba7cea9acbb6"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-custom-to-json/"
published_at: "2025-09-10T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T20:56:00.447075+00:00"
content_hash: "sha256:dcd06a2890a44f1be7a8d0e61b0ef3a4f5ee5b328e8a25ffa028180de58840d0"
---

# Controlling What Your Models Send Back with customToJSON()

By default, Sails is generous: whenever you send a model instance back in a response, it serializes **every attribute** into JSON. That means if your` User` model has` id` ,` email` ,` password` , and` createdAt` , all of it will end up in the response.


That’s convenient when you’re just starting out. You don’t have to think about serialization at all — you get the full record every time.


But as soon as your app grows, this default can become a liability. Some fields should stay private (like passwords or reset tokens). Other fields are only meant for internal use and shouldn’t leak to the client.


That’s where` customToJSON()` comes in.


## What is` customToJSON()` ?


` customToJSON()` is a method you can define in your[model settings](https://sailsjs.com/documentation/concepts/models-and-orm/model-settings) . Whenever Sails needs to turn a record into JSON, it calls this method if defined. Whatever object you return is what gets sent to the client.


Here’s a simple example:


```text
// api/models/User.js
module  .  exports   =   {
attributes: {
id: { type:   'number'  , autoIncrement:   true   },
email: { type:   'string'  , required:   true   },
password: { type:   'string'  , required:   true   },
},


customToJSON  :   function   () {
// Exclude sensitive fields
return   _.  omit  (  this  , [  'password'  ]);
}
};
```


Now, no matter what endpoint you’re hitting, the` password` field is never exposed.


## Two Strategies


There are two common ways to use` customToJSON()` :


### 1. **Omission (blacklist sensitive fields)**


```text
customToJSON  :   function   () {
return   _.  omit  (  this  , [  'password'  ,   'resetToken'  ]);
}
```


#### Pros


- New fields automatically show up
- Less to maintain during prototyping


#### Cons


- Risky if you add sensitive fields later and forget to omit them


### 2. **Selection (whitelist intended fields)**


```text
customToJSON  :   function   () {
return   {
id:   this  .id,
email:   this  .email,
};
}
```


#### Pros


- Explicit and safe — nothing leaks by accident
- Clear contract between backend and frontend


#### Cons


- Needs updating whenever you add new fields to expose.


## Personal Take


When I’m prototyping, omission is convenient. I can add fields quickly without worrying about updating` customToJSON()` .


But in production? I switch to selection. It forces me to be explicit about what I’m exposing, and I don’t have to worry about leaking something I didn’t mean to.


Yes, I’ve run into the “why isn’t this new field showing up?” bug more than once when building full-stack apps with[The Boring JavaScript Stack](https://docs.sailscasts.com/boring-stack) . But that small friction is worth it for the clarity and safety you get in return.


## Best Practices


- **Don’t mutate` this`** — always return a new object (` _.omit` ,` _.pick` , or manual return).
- **Treat it like a contract** — if a field should go to the client, it belongs here.
- **Write a test** that serializes a record and checks for the right fields.


## Wrapping Up


By default, Sails will happily serialize every field on your model. That’s great for small projects, but once you care about security and clear API boundaries,` customToJSON()` is your friend.


Whether you omit the sensitive stuff or explicitly select the fields you want, defining` customToJSON()` gives you control over what data leaves your app.


If you ever see data in your logs but not in your frontend, and you’re using the selection approach, double-check your` customToJSON()` . Nine times out of ten, that’s the reason.
