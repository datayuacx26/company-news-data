---
schema_version: "1.0.0"
document_id: "1293084310706feb0f8374a6ef828d5ab3b87ca290b1dae713c076e71c4a817c"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/understanding-waterline-projections/"
published_at: "2021-03-18T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:402b2a8c6072b0888f711b64b4d887e4f62559043fb7de16445dde6e11662da5"
---

# Understanding Waterline projections

Returning all the fields in a record may be suitable some of the time but most of the time, you may want to be intentional with what’s being returned from a record. Here are some reasons you may want to do this:


- Remove sensitive informations like **password** and **SSNs**
- Make faster and more performant Waterline queries by returning a subset of the fields of a record.
- Making sure only the fields you intended get sent to the client.


In other to achieve this behavior of selecting or omitting fields in a Waterline query, Waterline provides us with **Projections** .


## What are projections


Projections are mechanisms for controlling what fields gets returned from a Waterline query. Under the hood, Waterline is always adding the default values of these projections to your query. Let’s look at what these projections are:


### Select


The select projection allows you to select what fields to be returned in a Waterline query. By default when you do something like:


```text
await   User.  find  ()
```


Waterline expands the above query to the below form:


```text
await   User.  find  ({select: [  '*'  ]})
```


Note:` \['*'\]` is the default value for the` select` projection.


If you are familiar with SQL statements you will notice that the` select` projection is similar to writing the following SQL statement:


```text
SELECT   *   FROM   users
```


### select usage


To use the` select` projection, you either pass it as a property in your Waterline query criteria or you call the` .select` method on your Waterline query.


```text
await   User.  find  ({select: [] })
```


Or


```text
await   User.  find  ().  select  ([])
```


The select projection takes an array of strings where each string is the name of the field you want to include in the query. So let’s say we want the users’ email, first name and age, we will write:


```text
await   User.  find  ({ select: [  'email'  ,   'firstName'  ,   'age'  ] })
```


Or


```text
await   User.  find  ().  select  ([  'email'  ,   'firstName'  ,   'age'  ])
```


### Omit


The omit projection is the reverse of the select projection. Instead of specifying the fields you want to return using the select projection, with the omit projection you specify the fields you don’t want to return.


This is useful as you might want to return 20 fields and omit 3, it’s much more efficient in this scenario to use omit to specify the 3 fields you don’t want to return as opposed to typing out the 20 fields you want to return.


### omit usage


Waterline defaults the omit projection to an empy array - this means the query won’t omit any fields. So when you do:


```text
await   User.  find  ()
```


Waterline expands to


```text
await   User.  find  ({omit: []})
```


Note:` \[\]` is the default value for the` omit` projection.


Like its` select` , counterpart you can either pass` omit` as a property to your query or as a Waterline method. So you can do:


```text
await   User.  find  ({ omit: [] })
```


Or


```text
await   User.  find  ().  omit  ([])
```


So if you want to omit the` password` and` ssn` fields from your users records when returning them to a client, you can do:


```text
await   User.  find  ({ omit: [  'password'  ,   'ssn'  ] })
```


Or


```text
await   User.  find  ().  omit  ([  'password'  ,   'ssn'  ])
```


### Waterline assumptions


Waterline holds some assumptions about the usage of` omit/select` . let’s look at them.


### Always return the primary key


When using projections, you should expect the primary key for the records to always be included. Trying to omit the primary key in a Waterline query is invalid and Waterline will complain. So the below is invalid:


```text
// invalid usage of omit - can't omit the primary key
await   User.  find  ({ omit: [  'id'  ] })
```


Also note that the primary key will always be included in your` select` projections so you don’t need to specify it.


### Use select or omit but never both


You can’t use omit and select in the same query. Waterline assumes you must either use` select` or` omit` but never both so it will throw an error if you try to do so. So the below is invalid in Waterline:


```text
// wrong usage of select/omit
await   User.  find  ({ select: [  'email'  ], omit: [  'password'  ] })
```


#### Usage with where clause shorthand


Waterline allows you to do a query using this shorthand syntax:


```text
await   User.  find  ({ gender:   'female'   })
```


This will expand to the` where` clause like so:


```text
await   User.  find  ({ where: { gender:   'female'   } })
```


The shorthand method is really convenient for fewer key strokes however if you want to use any of the projections as a query criteria property you will always need to use the` where` clause for specifying query criteria. So the below will be invalid in Waterline:


```text
// invalid syntax
await   User.  find  ({ gender:   'female'  , select: [  'age'  ] })
```


Instead you should write:


```text
// correct syntax
await   User.  find  ({ where: { gender:   'female'   }, select: [  'age'  ] })
```


However you can opt in on using the methods version of the projections in order to keep using the query shorthand like so:


```text
await   User.  find  ({ gender:   'female'   }).  select  ([  'age'  ])
```


## Conclusion


Projections are handy for having the flexibility of defining what fields in your database records get sent back to the client. This article explains the two projections in Waterline and their usage in a Sails application.
