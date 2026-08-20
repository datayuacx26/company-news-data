---
schema_version: "1.0.0"
document_id: "7f5fcc8fa2961ccfdd3704289fef52a57a267d8cd6a94347fdc62333416f33d9"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/edge-runtime-self-hosted-deno-functions"
published_at: "2023-04-11T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:1736fd95ac26d2c7543c7fdc90a374ea61f2f2e487fe304f477a3767963fdc25"
---

# Supabase Edge Runtime: Self-hosted Deno Functions

🆕✨ Edge Functions now natively supports npm modules and Node built-in APIs.[Learn more](https://supabase.com/blog/edge-functions-node-npm)


Today we’re open-sourcing[Supabase Edge Runtime](https://github.com/supabase/edge-runtime) for self-hosting Deno Edge Functions.


Edge Runtime is MIT licensed, written in Rust, and based on the latest Deno Runtime (1.32+). If you’ve been using the Supabase CLI to serve functions then you’re already one of our Beta testers (thanks!).


## Host your Edge Functions anywhere#


We[launched](https://supabase.com/blog/supabase-edge-functions) Supabase Edge Functions a little more than a year ago. We use[Deno Deploy](https://deno.com/deploy) to host your edge functions globally across 30+ data centers, so your users get super fast responses. This setup works great for us! We didn’t have an easy solution for self-hosting Edge Functions. We’re releasing Edge Runtime to address this.


One of our core principles is “[Everything is portable](https://supabase.com/docs/guides/getting-started/architecture#everything-is-portable) ”, meaning you should be able to take any part of the Supabase stack and host it yourself.


Supabase Edge Runtime is a web server written in Rust that uses a custom Deno runtime. It can serve TypeScript, JavaScript, and WASM functions. All your existing Edge Functions run on Edge Runtime without changing a single line of code.


## Better local development experience#


Self-hosting is not the only benefit of Edge Runtime. It will improve the local development experience of Edge Functions.


### Serve all functions#


Supabase CLI can now serve all local Edge Functions by running` supabase functions serve` . Previously, you could only serve a single Edge Function at a time. This was not a great experience for local development. Some of you even devised[clever hacks](https://github.com/orgs/supabase/discussions/6786) to get around this limitation.


When you run` supabase functions serve` , the CLI uses Edge Runtime to serve all functions. It supports JWT verification, import maps, and passing custom environment variables. It hot-reloads local changes, giving you a seamless development experience.


### Dev/Prod parity#


Edge Runtime improves Dev/Prod parity for Edge Functions. You may have encountered issues where an Edge Function works locally but fails when deployed. The main cause for this is Deno Deploy Runtime is more restrictive and[only supports](https://deno.com/deploy/docs/runtime-api) a subset of Deno APIs. Edge Runtime exposes the same APIs available in the Deno Deploy Runtime. This will help you spot issues faster while developing and avoid surprises when deploying.


### Enforcing memory/duration limits#


Another neat feature we built into Edge Runtime is the option to enforce limits on memory and wall-clock durations. Currently, we are setting them to sensible defaults (memory set to 150 MB and execution duration set to 60s). This will allow you to simulate your functions’ resource usage and handle the behavior if they run into the limits. Soon we will allow configuring these limits via CLI config so that you can match them with the real limits of the deployment platform.


## How to self-host Edge Functions#


We have put together a demo on how to self-host edge functions on[Fly.io](http://fly.io/) (you can also use other providers like Digital Ocean or AWS).


To try it yourself:


1.


Sign up for an[Fly.io](http://fly.io/) account and install[flyctl](https://fly.io/docs/hands-on/install-flyctl/)


2.


Clone the[demo repository](https://github.com/supabase/self-hosted-edge-functions-demo) to your machine


3.


Copy your Edge Function into the` ./functions` directory in the demo repo.


4.


Update the Dockerfile to pull the latest edge-runtime image (check[releases](https://github.com/supabase/edge-runtime/pkgs/container/edge-runtime) )


5.


Optionally edit` ./functions/main/index.ts` , adding any other request preprocessing logic (for example, you can enable JWT validation, handle CORS requests)


6.


Run` fly launch` to create a new app to serve your Edge Functions


7.


Access your Edge Function by visiting:


` https://{your-app-name}.fly.dev/{your-function-name}`


View the logs for the Edge Runtime by visiting Fly.io’s Dashboard > Your App > Metrics. You can serve Edge Runtime from multiple regions by running` fly regions add \[REGION\]` .


## Standing on the shoulders of Deno#


You may wonder why we cannot use Deno Runtime to self-host functions. Isn’t it open-source and available as a Docker container?


[Deno Runtime](https://deno.com/runtime) , by default, includes a wide array of built-in APIs, making it easy to use for multiple use cases out of the box. However, this makes it difficult to use for serving web requests. You need the runtime embedded within a web server that can boot fast and, for security, has a more restricted API.


However, Deno’s architecture makes it easy to extend its core capabilities and create a customized runtime to match our needs. Deno provides a Rust crate called` deno_core` , which abstracts the interactions with V8 JavaScript engine. Using` deno_core` we can create a JS context (known as a V8 Isolate). A V8 isolate has minimal overhead to boot up and a single process can host multiple V8 isolates. When you load a web page that contains scripts from multiple domains in a browser, each of them runs in a separate v8 isolate.


Deno team has a detailed 2-part blog post on[how to create a custom runtime](https://deno.com/blog/roll-your-own-javascript-runtime) .


Edge Runtime implements an HTTP server (using[hyper](https://github.com/hyperium/hyper) ) that listens to incoming requests. When Edge Runtime is booted, it spins up a JS context (V8 isolate), which we call the` Main Worker` . Main Worker runs in a separate thread, executing the provided main module. When a new HTTP request is received, the Rust runtime will forward it to the Main Worker.


You can write a main module to handle all incoming requests. This would look like a typical Deno Edge Function. The main difference is that it has access to a global object called “` EdgeRuntime` ”.


` EdgeRuntime` global provides methods to create and access` UserWorkers` .` Main Worker` can optionally delegate a request to a UserWorker to handle and respond.


User Workers are separate JS contexts (V8 isolates) that can run a given Edge Function. They have a restricted API (for example, they don’t get access to the host machine’s environment variables). You can also control the memory and duration a User Worker can run.


Here’s a simple implementation of a Main Worker that receives a request, then creates a User Worker and passes the handling of request to the worker.


`
_28


serve(async (req: Request) => {


_28


const memoryLimitMb = 150


_28


const workerTimeoutMs = 1 * 60 * 1000


_28


const noModuleCache = false


_28


const importMapPath = null


_28


const envVars = \[


_28


\['USER', 'foo'\],


_28


\['PASSWORD', 'BAR'\],


_28


\]


_28


_28


try {


_28


const worker = await EdgeRuntime.userWorkers.create({


_28


servicePath,


_28


memoryLimitMb,


_28


workerTimeoutMs,


_28


noModuleCache,


_28


importMapPath,


_28


envVars,


_28


})


_28


return await worker.fetch(req)


_28


} catch (e) {


_28


const error = { msg: e.toString() }


_28


return new Response(JSON.stringify(error), {


_28


status: 500,


_28


headers: { 'Content-Type': 'application/json' },


_28


})


_28


}


_28


})


`


## What’s Next?#


Open-sourcing Edge Runtime is the first step of an exciting roadmap we have planned for Edge Functions. In the coming months, you will see tighter integrations with the rest of the Supabase ecosystem. Here are some sneak peeks at what is to come next.


### API Gateway to other Supabase services#


We plan to use Edge Runtime as a replacement for Kong, acting as an API gateway to other Supabase services. This will not only simplify the self-hosting setup but also give you the option to do Request pre/post-processing using JavaScript.


Here’s a simple example of re-routing a request to a different endpoint using Edge Runtime.


`
_17


serve(async (req) => {


_17


try {


_17


if (req.url.endsWith('/rest/v1/old_table')) {


_17


return await fetch('http://rest:3000/rest/v1/new_table', {


_17


headers: req.headers,


_17


method: req.method,


_17


body: req.body,


_17


})


_17


}


_17


} catch (e) {


_17


const error = { msg: e.toString() }


_17


return new Response(JSON.stringify(error), {


_17


status: 500,


_17


headers: { 'Content-Type': 'application/json' },


_17


})


_17


}


_17


})


`


### Scheduled Functions#


Since Edge Runtime’s Main Worker runs in the background as long as the server is running, we can utilize it to run periodic tasks.


For example, here’s a naive implementation of how it can be used to trigger a function every 2 minutes. In production, you need to account for server restarts and timer resetting.


`
_15


const interval = 2 * 60 * 1000 // 2 minutes


_15


try {


_15


const worker = await EdgeRuntime.userWorkers.create({


_15


servicePath,


_15


memoryLimitMb,


_15


workerTimeoutMs,


_15


noModuleCache,


_15


importMapPath,


_15


envVars,


_15


})


_15


const req = new Request('http://localhost/scheduled-job')


_15


setInterval(() => worker.fetch(req), interval)


_15


} catch (e) {


_15


console.error(e)


_15


}


`


### Custom Global Objects#


Another exciting thing about shipping a custom JavaScript runtime is that we can control the available global objects in the runtime. In previous examples, you may noticed we used` EdgeRuntime` without importing a specific module to our function, this was possible because we exposed it as a global object in the runtime.


We can introduce a` Supabase` global object that can provide platform specific features. For example, similar to` Deno.writeTextFile` , we can expose a` Supabase.writeTextFile` which can directly write a file to Supabase Storage.


## We 💚 Contributions#


We are excited to build Edge Runtime in public and involve the Supabase community in the process. As an initial beta release, there are still bugs and performance quirks to be ironed out. Don’t shy away from trying it though.


You can report any issues you encounter in repo’s[GitHub issues](https://github.com/supabase/edge-runtime/issues) . If you have ideas on how to make edge-runtime better, reach out via[Twitter](https://twitter.com/supabase) or[Discord](http://discord.supabase.com/) .
