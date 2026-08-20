---
schema_version: "1.0.0"
document_id: "8088713a58f36cc886629470b6ca6feee6abe224d3799b9e0aeede17f2130705"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-js-v2"
published_at: "2022-08-16T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:03:19.340687+00:00"
content_hash: "sha256:39acdcc04dcf8a6cbbbbe4cf405647e046a45ac16945abfdb2fdf444aaf23ebf"
---

# supabase-js v2

⚠️ UPDATED 20/10: supabase-js v2 is fully released 🥳


[Check the updated docs](https://supabase.com/docs/reference/javascript) and[migration guide](https://supabase.com/docs/reference/javascript/v1/upgrade-guide) .


Today we're publishing a release candidate for[supabase-js v2](https://github.com/supabase/supabase-js) , which focuses on “quality-of-life” improvements for developers.


Try it out by running` npm i @supabase/supabase-js@rc`


Nearly 2 years ago we[released supabase-js v1](https://supabase.com/blog/improved-dx) . Since then it has been used in over[17K repositories](https://github.com/supabase/supabase-js/network/dependents?package_id=UGFja2FnZS04MjM3OTUyMDU%3D) and has grown to[50K weekly downloads](https://www.npmjs.com/package/@supabase/supabase-js) . Supabase users give a lot of great feedback and we've learned some of the largest pain-points as a result.


## Major Updates#


This release focuses on solving these pain-points. At the same time we want to make the upgrade path for supabase-js as easy as possible, so we've been strategic about the changes we're making. We plan to follow this model going forward: incremental changes over huge rewrites.


### Type support#


If you followed yesterday's announcement, you will have noticed that we added[type generators](https://supabase.com/docs/reference/cli/usage#supabase-gen-types) to the CLI.


`
_10


supabase start


_10


supabase gen types typescript --local > DatabaseDefinitions.ts


`


These types can now be used to enrich your development experience:


`
_10


import type { Database } from './DatabaseDefinitions'


_10


_10


const supabase = createClient<Database>(SUPABASE_URL, ANON_KEY)


_10


_10


const { data } = await supabase.from('messages').select().match({ id: 1 })


`


ℹ️ Differences from v1


v1 also supported types, but the types were generated from the API rather than the database, so it lost a lot of detailed information. The library also required you to specify the definition in every method call, rather than at the client level.


`
_10


supabase.from<Database\['Message'\]>('messages').select('*')


`


---


### New Auth methods#


We're removing the` signIn()` method in favor of more explicit method signatures:` signInWithPassword()` , and` signInWithOtp()` .


`
_11


// v2


_11


const { data } = await supabase.auth.signInWithPassword({


_11


email: 'hello@example',


_11


password: 'pass',


_11


})


_11


_11


// v1


_11


const { data } = await supabase.auth.signIn({


_11


email: 'hello@example',


_11


password: 'pass',


_11


})


`


This helps with type hinting. Previously it was difficult for developers to know what they were missing. A lot of developers didn't even realize they could use passwordless magic links.


---


### Data methods return minimal by default#


The` insert()` ,` update()` , and` upsert()` methods now require you to explicitly append` select()` if you want the data to be returned.


`
_10


// v2


_10


const { data } = await supabase.from('messages').insert({ id: 1, message: 'Hello world' }).select() // select is now explicitly required


_10


_10


// v1


_10


const { data } = await supabase.from('messages').insert({ id: 1, message: 'Hello world' }) // insert would also "select()"


`


This was another common question in our GitHub Discussions. While the idea of automatically returning data is great, developers often turn on Row Level Security (which is great), and then they forget to add a` select` Policy. It is a bit surprising that you need to add a` select` policy to do an` insert` , so we opted for the “principle of least surprise”. If you don't append` select()` , the` data` value will be an empty object:` {}` .


ℹ️ Differences from v1


Previously you could pass a` returning: 'minimal'` option to the` insert()` ,` update()` , and` upsert()` statements. We've now made this the default behaviour.


---


### Auth Admin methods#


We've move all server-side Auth methods from` supabase.auth.api` to` supabase.auth.admin` :


`
_10


// v2


_10


const { data: user, error } = await supabase.auth.admin.listUsers()


_10


_10


// v1


_10


const { data: user, error } = await supabase.auth.api.listUsers()


`


All` admin` methods expect a` SERVICE_ROLE` key. This change makes it clear that any methods under the` admin` namespace should only be used on a trusted server-side environment.


---


### Async Auth overhaul#


We've rebuilt the Auth library, making it async for almost all methods.


`
_10


// v2


_10


const { data } = await supabase.auth.getSession()


_10


_10


// v1


_10


const { data } = supabase.auth.session()


`


This solves the “getting logged out” issue, which has been a recurring challenge in our GitHub Discussions.


ℹ️ Improvements from v1


The previous implementation had a race condition when refreshing the auth token across multiple tabs. The async re-write forces the library to wait for a valid/invalid session before taking any action.


---


### Scoped constructor config#


We're being much more explicit about the modular approach that` supabase-js` uses:


`
_18


const supabase = createClient(apiURL, apiKey, {


_18


db: {


_18


schema: 'public',


_18


},


_18


auth: {


_18


autoRefreshToken: true,


_18


persistSession: true,


_18


},


_18


realtime: {


_18


channels,


_18


endpoint,


_18


},


_18


// common across all libraries


_18


global: {


_18


fetch: customFetch,


_18


headers: DEFAULT_HEADERS,


_18


},


_18


})


`


This is clearer for developers - if you're only using some parts of Supabase, you only receive the hints for those parts.


ℹ️ Improvements from v1


We noticed a lot of confusion for the variable naming between each library. For example, Auth has a config parameter called "storageKey", which was was often confused with the` storage-js` library bundled in the` supabase-js` library.


---


### Better Errors#


We've created error types for all of the sub-libraries in` supabase-js` . Here's a example for Edge Functions:


`
_11


import { FunctionsHttpError, FunctionsRelayError, FunctionsFetchError } from '@supabase/supabase-js'


_11


_11


const { data: user, error } = await supabase.functions.invoke('hello')


_11


_11


if (error instanceof FunctionsHttpError) {


_11


console.log('Function returned an error', error.message)


_11


} else if (error instanceof FunctionsRelayError) {


_11


console.log('Relay error:', error.message)


_11


} else if (error instanceof FunctionsFetchError) {


_11


console.log('Fetch error:', error.message)


_11


}


`


---


### Improvements for Edge Functions#


` supabase-js` now automatically detects the content type for the request/response bodies for all Edge Function invocations:


`
_10


// v2


_10


const { data: user, error } = await supabase.functions.invoke('hello', {


_10


body: { foo: 'bar' },


_10


})


_10


_10


// v1


_10


const { data: user, error } = await supabase.functions.invoke('hello', {


_10


headers: { 'Content-Type': 'application/json' }


_10


body: JSON.stringify({ foo: 'bar' }),


_10


})


`


This improvement inspired by a Supabase community member. Thanks[@vejja](https://github.com/supabase/functions-js/pull/23) !


---


### Multiplayer Sneak Peek#


There is a new` channel()` interface which are releasing in v2. This is a "preparatory" release for our upcoming[multiplayer](https://supabase.com/blog/supabase-realtime-with-multiplayer-features) features.


`
_10


supabase


_10


.channel('any_string_you_want')


_10


.on('presence', { event: 'track' }, (payload) => {


_10


console.log(payload)


_10


})


_10


.subscribe()


`


As part of this change, the old` from().on().subscribe()` method for listening to database changes will be changing:


`
_23


// v2


_23


supabase


_23


.channel('any_string_you_want')


_23


.on(


_23


'postgres_changes',


_23


{


_23


event: 'INSERT',


_23


schema: 'public',


_23


table: 'movies',


_23


},


_23


(payload) => {


_23


console.log(payload)


_23


}


_23


)


_23


.subscribe()


_23


_23


// v1


_23


supabase


_23


.from('movies')


_23


.on('INSERT', (payload) => {


_23


console.log(payload)


_23


})


_23


.subscribe()


`


You can listen to PostgreSQL database changes on any channel you want by subscribing to the` 'postgres_changes'` event. For now, we will continue to support` from().on().subscribe()` , but in the future we will deprecate this in favor of the` channel().on().subscribe()` method.


---


## Community#


Version 2.0 is the result of the combined work of over 100 contributors to our libraries, and over 450 contributors to our docs and websites. If you're one of those contributors, thank you.


- [functions-js](https://github.com/supabase/functions-js/graphs/contributors) (4)
- [gotrue-js](https://github.com/supabase/gotrue-js/graphs/contributors) (47)
- [postgrest-js](https://github.com/supabase/postgrest-js/graphs/contributors) (30)
- [realtime-js](https://github.com/supabase/realtime-js/graphs/contributors) (16)
- [storage-js](https://github.com/supabase/storage-js/graphs/contributors) (17)
- [supabase-js](https://github.com/supabase/supabase-js/graphs/contributors) (39)


Special shout outs to:[@vejja](https://github.com/vejja) ,[@pixtron](https://github.com/pixtron) ,[@bnjmnt4n](https://github.com/pixtron) , and[@karlseguin](https://github.com/karlseguin) .


## Migrating to v2#


Update today by running:


`
_10


npm i @supabase/supabase-js@2


`


[Migration guide](https://supabase.com/docs/reference/javascript/v1/upgrade-guide)


We'll continuing merging security fixes to v1, with maintenance patches for the next three months.


## Announcement video and discussion#


## supabase-js v2 resources#


- [v2 Documentation](https://supabase.com/docs/reference/javascript)
- [Migration guide](https://supabase.com/docs/reference/javascript/v1/upgrade-guide)
- [Next.js quickstart guide](https://supabase.com/docs/guides/with-nextjs)
- [Examples](https://github.com/supabase/supabase/tree/master/examples)
