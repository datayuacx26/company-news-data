---
schema_version: "1.0.0"
document_id: "743bb69846f3c50e73e56b521412859dfc61f5808c92a438c436432a91919b49"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/edge-functions-node-npm"
published_at: "2023-12-12T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:9c53750a1f6603dcf2b0fd7209897edce4ca37598ee1124d1094425e54d7c279"
---

# Edge Functions: Node and native npm compatibility

We are excited to announce that[Edge Functions](https://supabase.com/docs/guides/functions) now natively supports npm modules and Node built-in APIs. You can directly import millions of popular, commonly used npm modules into your Edge Functions.


`
_10


import { drizzle } from 'npm:drizzle-orm/node-postgres'


`


## Migrate existing Node apps to Edge Functions#


You can migrate your existing Node apps to Supabase Edge Functions with minimal changes.


We created a demo to show how to migrate a Node app that uses Express, Node Postgres, and Drizzle. For more information on using npm modules and Node built-ins within your Edge Functions, see the[Managing dependencies guide](https://supabase.com/docs/guides/functions/dependencies) .


## How npm modules work under the hood#


We run an open source Deno server for hosting Edge Functions called[Supabase Edge Runtime](https://supabase.com/blog/edge-runtime-self-hosted-deno-functions) . This custom version helps us keep Edge Functions working the same way no matter where it is deployed - on our hosted platform, in local development, or in your self-hosted environment.


The biggest challenge when adding npm support was finding an approach that would work across all environments. We wanted to keep the workflow close to the Deno CLI experience. It should be possible to import npm modules directly in your source code without an extra build step.


When deploying a Function, we serialize its module graph into a single file format (an[eszip](https://github.com/denoland/eszip) ). In the hosted environment, all module references are then loaded from the eszip. This prevents any extra latency in fetching modules and potential conflicts between module dependencies.


We used the eszip module loader in the local and self-hosted environments too, so we only need to implement one module-loading strategy for all environments. As an additional benefit for local development, this approach avoids potential conflicts with npm modules installed in the user's system since the Edge Function's npm modules are self-contained within the eszip.


[Refactoring the module loader](https://github.com/supabase/edge-runtime/pull/223) fixes a few other bugs, such[edge functions erroring out](https://github.com/supabase/cli/issues/1584#issuecomment-1848799355) when an` deno.lock` file is already present in the project.


## A few other things you've asked for…#


### Regional Invocations#


You now have the option to specify a region when running an Edge Function (perhaps we should change the name in the future). Usually, Edge Functions run in the region closest to the user invoking the Function. However, sometimes you want to run it closer to your Postgres database or another 3rd party API for optimal performance.


Functions are still deployed to all regions. However, during invocation, you can provide the` x-region` header to restrict the execution to a specific region.


cURL


JavaScript


`
_10


# https://supabase.com/docs/guides/functions/deploy#invoking-remote-functions


_10


curl --request POST 'https://<project_ref>.supabase.co/functions/v1/hello-world' \\


_10


--header 'Authorization: Bearer ANON_KEY' \\


_10


--header 'Content-Type: application/json' \\


_10


--header 'x-region: eu-west-3' \\


_10


--data '{ "name":"Functions" }'


`


ℹ️ Check out the[Regional Invocation guide](https://supabase.com/docs/guides/functions/regional-invocation) for more details.


### Better Metrics#


We've added more metrics in the Edge Functions section of the[Supabase Dashboard](https://supabase.com/dashboard/project/_/functions) : it now shows CPU time and memory used. We've also broken down invocations by HTTP status codes.


These changes help you spot any issues with your Edge Functions and act on them.


ℹ️ See the[logging and metrics guide](https://supabase.com/docs/guides/functions/debugging) for Edge Functions to learn more.


### Track errors with Sentry#


Our friends at Sentry recently shipped an official[Sentry SDK for Deno](https://deno.land/x/sentry@7.76.0) . With this, it's now easy to track errors and exceptions in your edge functions in Sentry.


Here is a simple example of how to handle exceptions within your function and send them to Sentry.


`
_31


import * as Sentry from 'https://deno.land/x/sentry/index.mjs'


_31


_31


Sentry.init({


_31


dsn: _DSN_,


_31


integrations: \[\],


_31


// Performance Monitoring


_31


tracesSampleRate: 1.0,


_31


// Set sampling rate for profiling - this is relative to tracesSampleRate


_31


profilesSampleRate: 1.0,


_31


})


_31


_31


// Set region and execution_id as custom tags


_31


Sentry.setTag('region', Deno.env.get('SB_REGION'))


_31


Sentry.setTag('execution_id', Deno.env.get('SB_EXECUTION_ID'))


_31


_31


Deno.serve(async (req) => {


_31


try {


_31


const { name } = await req.json()


_31


const data = {


_31


message: \`Hello ${name}!\`,


_31


}


_31


_31


return new Response(JSON.stringify(data), { headers: { 'Content-Type': 'application/json' } })


_31


} catch (e) {


_31


Sentry.captureException(e)


_31


return new Response(JSON.stringify({ msg: 'error' }), {


_31


status: 500,


_31


headers: { 'Content-Type': 'application/json' },


_31


})


_31


}


_31


})


`


## What's next#


NPM support was one of the most requested features for Edge Functions. If you couldn't use Edge Functions previously because of the lack of support, we hope this update will entice you to[try it again](https://supabase.com/dashboard/project/_/functions) . If you run into any issues, we are just[one support request away](https://supabase.help/) .


For existing Edge Functions users, regional invocations, better metrics, and error handling are just a glimpse of what will come next. We continue to iterate on platform stability and setting custom limits on resources Edge Functions can use. Watch out for another blog post in the new year.
