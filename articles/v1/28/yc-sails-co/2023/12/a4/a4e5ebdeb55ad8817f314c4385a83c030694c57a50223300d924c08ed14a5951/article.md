---
schema_version: "1.0.0"
document_id: "a4e5ebdeb55ad8817f314c4385a83c030694c57a50223300d924c08ed14a5951"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-mongo-v2-1-0/"
published_at: "2023-12-07T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:509f8abe316f6d09e873512cb6b20ac7947fa2588e6c4855c174ef391a6a92fd"
---

# sails-mongo 2.1.0

In this` sails-mongo` release, a much requested update was merged via this[PR](https://github.com/balderdashy/sails-mongo/pull/499) , upgrading the MongoDB driver version from` 3.7.3` to` 6.3.0` .


Before this update, Sails developers using MongoDB in their applications were unable to leverage the latest and greatest of MongoDB features.


This release brings about a noteworthy change by replacing callbacks with Promises in the internals of` sails-mongo` when interacting with the APIs provided by the MongoDB driver.


Also see the[full changelog](https://github.com/balderdashy/sails-mongo/releases/tag/v2.1.0) on GitHub.


## ✅ Upgrading


To upgrade to **sails-mongo 2.1.0** , run:


```text
npm   i   sails-mongo@latest
```


### Updating your code


When interacting with the MongoDB client provided by` sails-mongo` in your Sails applications, it is essential to transition from callbacks to promises. This can be achieved through either` .then()` calls or using the` async/await` syntax.


To minimize alterations to your existing codebase, we suggest employing an Immediately Invoked Function Expression ([IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) ) to encapsulate the Promise’s` .then()` and` .catch()` methods. This way, your callbacks can stay unaltered. The following example is extracted from the test in` sails-mongo` .


### Before ❌


```text
it  (  'should find a record w/ a numeric ID'  ,   function  (  done  ) {
models.user._adapter.datastores.test.manager.  collection  (  'user'  ).  insertOne  ({_id:   123  , name:   'bob'  },   function  (  err  ) {
if   (err) {  return   done  (err);}
models.user.  findOne  ({id:   123  }).  exec  (  function  (  err  ,   record  ) {
if   (err) {  return   done  (err);}
assert.  equal  (record.id,   123  );
assert.  equal  (record.name,   'bob'  );
return   done  ();
});
});
});
```


### After ✅


```text
it  (  'should find a record w/ a numeric ID'  ,   function  (  done  ) {
(  function  (  iifeDone  ) {models.user._adapter.datastores.test.manager.  collection  (  'user'  ).  insertOne  ({_id:   123  , name:   'bob'  }).  then  (  function  (){   iifeDone  ();}).  catch  (  function  (  err  ) {   iifeDone  (err);});})(  function  (  err  ) {
if   (err) {  return   done  (err);}
if   (err) {  return   done  (err);}
assert.  equal  (record.id,   123  );
assert.  equal  (record.name,   'bob'  );
return   done  ();
});
});
});
```
