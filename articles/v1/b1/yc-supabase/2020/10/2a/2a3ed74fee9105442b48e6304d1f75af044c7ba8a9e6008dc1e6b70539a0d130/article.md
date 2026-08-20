---
schema_version: "1.0.0"
document_id: "2a3ed74fee9105442b48e6304d1f75af044c7ba8a9e6008dc1e6b70539a0d130"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/improved-dx"
published_at: "2020-10-30T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:5b17906362095dc8e1d69bd6314ee13e3a67de3960db59d7bac782385c7053bd"
---

# Supabase.js 1.0

**UPDATE 16/08/2022** : supabase-js v2 is out and focuses on “quality-of-life” improvements for developers. V2 includes Type support, new auth methods, realtime multiplayer sneak peek, and more:[Read the blog post](https://supabase.com/blog/supabase-js-v2)


### New Docs#


Before digging into the improvements, we're excited to point out our new[developer docs](https://supabase.com/docs/reference/javascript/installing) . While they're still a work in progress, here are some things we think you'll like:


- The[Reference Docs](https://supabase.com/docs/reference/javascript/installing) are auto-generated from our TypeScript definitions and then enriched with examples. This forces us to document our code and makes it easier to keep everything in sync.
- We added placeholders for the other languages that the community is developing. They have already started with Python, C#, Dart, Rust, and Swift. Expect to see the docs filling up soon!
- We've added sections for all of the open source tools we use, including[Postgres](https://supabase.com/docs/postgres/server/about) , PostgREST1 ,[GoTrue](https://supabase.com/docs/guides/auth/architecture#auth-service) , and Realtime2 . We'll be filling these with lots of valuable information including self-hosting, benchmarks, and simple guides.


### Errors are returned, not thrown#


We attribute this improvement to community feedback. This has significantly improved the developer experience.


Previously we would throw errors:


`
_10


try {


_10


const { body } = supabase.from('todos').select('*')


_10


} catch (error) {


_10


console.log(error)


_10


}


`


And now we simply return them:


`
_10


const { data, error } = supabase.from('todos').select('*')


_10


if (error) console.log(error)\\n


_10


// else, carry on ..


`


After testing this for a while we're very happy with this pattern. Errors are handled next to the offending function. Of course you can always rethrow the error if that's your preference.


### We created` gotrue-js`#


Our goal for` supabase-js` is to tie together many sub-libraries. Each sub-library is a standalone implementation for a single external system. This is one of the ways we support existing open source tools.


To maintain this philosophy, we created[gotrue-js](https://github.com/supabase/gotrue-js) , a library for Netlify's GoTrue auth server. This library includes a number of new additions, including third-party logins.


Previously:


`
_10


const {


_10


body: { user },


_10


} = await supabase.auth.signup('someone@email.com', 'password')


`


Now:


`
_10


const { user, error } = await supabase.auth.signUp({


_10


email: 'someone@email.com',


_10


password: 'password',


_10


})


`


### Enhancements and fixes#


- Native TypeScript. All of our libraries are now natively built with TypeScript:[supabase-js](https://github.com/supabase/supabase-js) ,[postgrest-js](https://github.com/supabase/postgrest-js) ,[gotrue-js](https://github.com/supabase/gotrue-js) , and[realtime-js](https://github.com/supabase/realtime-js) .
- Better realtime scalability: we only generate one socket connection per Supabase client. Previously we would create a connection for every subscription.
- We've added support for OAuth providers.
- 60% of minor bugs outstanding for` supabase-js` have been[solved](https://github.com/supabase/supabase-js/pull/50) .
- You can use` select()` instead of` select(*)`


### Breaking changes#


We've bumped the major version because there are a number of breaking changes. We've detailed these in the[release notes](https://github.com/supabase/supabase-js/releases/tag/v1.0.1) , but here are a few to be aware of:


- ` signup()` is now` signUp()` and` email` /` password` is passed as an object
- ` logout()` is now` signOut()`
- ` login()` is now` signIn()`
- ` ova()` and` ovr()` are now just` ov()`
- ` body` is now` data`


Previously:


`
_10


const { body } = supabase.from('todos').select('*')


`


Now:


`
_10


const { data } = supabase.from('todos').select()


`


### Upgrading#


We have documented all of the changes in the[release notes](https://github.com/supabase/supabase-js/releases/tag/v1.0.1) .


To summarise the steps:


1. Install the new version:` npm install @supabase/supabase-js@latest`
2. Update all your` body` constants to` data`
3. Update all your` supabase.auth` functions with the new[Auth interface](https://supabase.com/docs/reference/javascript/auth-signup)


### Get started#


- Start using Supabase today:[supabase.com/dashboard](https://supabase.com/dashboard/)
- Make sure to[star us on GitHub](https://github.com/supabase/supabase)
- Follow us[on Twitter](https://twitter.com/supabase)
- Become a[sponsor](https://github.com/sponsors/supabase)


## Footnotes#


1.


Removed link on June 14 2022 for search optimization: page currently does not exist↩


2.
