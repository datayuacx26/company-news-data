---
schema_version: "1.0.0"
document_id: "3a0d3ca4b5bcd83c0c4beee3845d2b7b1c953a7391dcaa27eb48ef7f81eceee9"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/beta-to-latest-announcement"
published_at: "2024-09-17T12:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:74e17295c6cffed0f0eefe3d14164a8069dad503bd73eb4fb8e66a4a3bbd6f16"
---

# From beta to 3.0: Trigger.dev v3 reaches GA

Trigger.dev v3, the open-source background job platform with no timeouts, is now generally available and out of beta. Get started today by running` npx trigger.dev@latest init` and deploying your first job in minutes.


Before we dive into the details of this release, a quick recap of what Trigger.dev v3 is and how it can help you (or skip right to theWhat's new in 3.0.0 section):


## Trigger.dev v3


Trigger.dev v3 is not just a rewrite—it's a complete reimagining of the platform. You still write your typesafe tasks right next to your application code. However, instead of merely orchestrating your tasks, Trigger.dev v3 now builds and deploys them to an elastic, serverless runtime. This also means you aren't double billed, once for your orchestrator and once for your serverless runtime.


### A new runtime built for long-running tasks


Unlike existing runtimes like AWS Lambda or Google Cloud Functions, we've built a new runtime from the ground up specifically for writing and executing long-running tasks, featuring a checkpoint-restore system that allows for tasks to wait indefinitely without wasting resources or forcing a clunky programming model.


`
import { task, wait } from "@trigger.dev/sdk";


export const parentTask = task({


id: "parent-task",


run: async () => {


console.log("Starting parent task");


// This will cause the parent task to be checkpointed and suspended


const result = await childTask.triggerAndWait({ data: "some data" });


console.log("Child task result:", result);


// This will also cause the task to be checkpointed and suspended


await wait.for({ seconds: 30 });


console.log("Resumed after 30 seconds");


return "Parent task completed";


},


});


export const childTask = task({


id: "child-task",


run: async (payload: { data: string }) => {


console.log("Starting child task with data:", payload.data);


// Simulate some work


await sleep(5);


return "Child task result";


},


});


`


This comes with a built-in way to build and deploy your tasks using our CLI:


`
npx trigger.dev@latest deploy


`


And a way to make sure your tasks run locally, before they're deployed:


`
npx trigger.dev@latest dev


`


To learn more about how the runtime works, check out our[How it works](https://trigger.dev/docs/how-it-works) guide.


### Queueing and scheduling


On top of this runtime, we've added a queueing and scheduling system. This allows you to queue up work to be done later, schedule tasks to run at specific times, and even dynamically create queues on the fly.


#### Multi-tenant ready


Trigger.dev v3 is built from the ground up to be multi-tenant ready, without having to manually manage multiple accounts or workspaces. For example, you can easily create on-the-fly tenant concurrency limits when triggering tasks:


`
import { task } from "@trigger.dev/sdk";


export const tenantTask = task({


id: "tenant-task",


run: async () => {


console.log("Starting tenant task");


return "Tenant task completed";


},


});


// Trigger the task with a tenant concurrency limit of 5, but just for this tenant


await tenantTask.trigger({


concurrencyKey: "my-tenant-id",


queue: { concurrencyLimit: 5 },


});


`


We also support scheduling tasks to be triggered at a specific time for a specific tenant:


`
import { schedules } from "@trigger.dev/sdk";


export const firstScheduledTask = schedules.task({


id: "first-scheduled-task",


run: async (payload) => {


// payload.externalId will be "my-tenant-id"


},


});


// Create a schedule for the task to run at 8am every day for this tenant


const createdSchedule = await schedules.create({


task: reminderTask.id,


//8am every day


cron: "0 8 * * *",


//the user's timezone


timezone: "America/New_York",


//the user id


externalId: "my-tenant-id",


//this makes it impossible to have two reminder schedules for the same tenant


deduplicationKey: "my-tenant-id/reminder",


});


`


To learn more, check out our[Concurrency & Queues guide](https://trigger.dev/docs/queue-concurrency) , and our[Scheduling guide](https://trigger.dev/docs/tasks/scheduled) .


### Extreme visibility with OpenTelemetry


Our dashboard is powered by[OpenTelemetry](https://opentelemetry.io/) , which means you can see exactly what your tasks are doing in real-time. This includes logs and traces auto-correlated across your tasks. And we make it easy to add custom instrumentation to your tasks, so you can see exactly what you need to see.


Learn more over at our[Logging & Tracing guide](https://trigger.dev/docs/logging) .


### Key Features


- **No Timeouts** : tasks can run indefinitely without being interrupted.
- **Open-source** : The entire platform is open-source, allowing you to[run it on your own infrastructure](https://trigger.dev/docs/open-source-self-hosting) if you prefer.
- **Dedicated Execution** : Each job runs in its own isolated environment, eliminating interference between jobs.
- **Deploy and Scale** : Trigger.dev handles the execution and scaling of your jobs, letting you focus on writing code.


They say a picture is worth a thousand words, so here are about 1200 pictures stitched together (that's 1.2m words) to show you what Trigger.dev v3 is all about:


To dig even deeper, check out our[How it works](https://trigger.dev/docs/how-it-works) guide or our[Quick Start](https://trigger.dev/docs/quick-start) guide.


## What's new in 3.0.0


We released the beta version of Trigger.dev v3 in early April of this year. Since then, we've been working on various improvements and bug fixes based on the feedback we received from our beta users.


One major area of focus has been on the build system, which has been completely rewritten as part of the v3 latest release. The main features of the new build system are:


- **Bundling by default** : All dependencies are bundled by default, so you no longer need to specify which dependencies to bundle. This solves a whole bunch of issues related to monorepos.
- **Build extensions** : A new way to extend the build process with custom logic. This is a more flexible and powerful way to extend the build process compared to the old system (including custom esbuild plugin support).
- **Multi-runtime support** : We now support multiple runtimes, including Bun, which is a new runtime for Trigger.dev.
- **Improved configuration** : We've migrated to using[c12](https://github.com/unjs/c12) to power our configuration system.
- **Better deployment error reporting** : We now do a much better job of reporting any errors that happen during the deployment process.


## Bundling by default


Our new build system is powered by esbuild, and we've moved to a model where all dependencies are bundled by default. This means you no longer need to specify which dependencies to bundle, and you can just import them as you normally would in your code. This should make it much easier to work with monorepos and other complex project structures. The biggest change is that you will now need to specify any dependencies that you don't want to bundle, and instead load externally. This can be done like this:


`
import { defineConfig } from "@trigger.dev/sdk";


export default defineConfig({


build: {


external: \["sharp"\],


},


});


`


We also auto-detect dependencies that will need to be external to enable OpenTelemetry instrumentation, so you don't need to worry about adding them manually:


`
import { defineConfig } from "@trigger.dev/sdk";


import { OpenAIInstrumentation } from "@traceloop/instrumentation-openai";


export default defineConfig({


// \`openai\` will be automatically added to the external list


instrumentations: \[new OpenAIInstrumentation()\],


});


`


## Build extensions


Build extensions are a powerful new feature in Trigger.dev v3 that allow you to extend the build process with custom logic. You can now fully customize the bundled code through adding esbuild plugins, and you can also add and modify the deployed image to add custom system dependencies and more.


One example of a build extension is the` prismaExtension` , which will prepare your deployment to be able to use Prisma. It can be used like this:


`
import { defineConfig } from "@trigger.dev/sdk";


import { prismaExtension } from "@trigger.dev/build/extensions/prisma";


export default defineConfig({


build: {


extensions: \[


prismaExtension({


version: "5.19.0", // optional, we'll automatically detect the version if not provided


schema: "prisma/schema.prisma",


}),


\],


},


});


`


The extension handles generating the Prisma client and optionally allows you to run migrations on deployment. It also has built-in support for multi-schema Prisma projects and the newly released[TypedSQL](https://www.prisma.io/typedsql) feature.


Another example is the` ffmpeg` extension, which allows you to add the` ffmpeg` binary to your deployment. It can be used like this:


`
import { defineConfig } from "@trigger.dev/sdk";


import { ffmpeg } from "@trigger.dev/build/extensions/core";


export default defineConfig({


build: {


extensions: \[ffmpeg()\],


},


});


`


Our syncEnvVars extension allows you to easily sync secrets when you deploy. An example of how you can use it with[Infisical](https://infisical.com/) :


`
import { InfisicalClient } from "@infisical/sdk";


import { syncEnvVars } from "@trigger.dev/build/extensions/core";


import { defineConfig } from "@trigger.dev/sdk";


export default defineConfig({


build: {


extensions: \[


syncEnvVars(async (ctx) => {


if (


!process.env.INFISICAL_CLIENT_ID ||


!process.env.INFISICAL_CLIENT_SECRET ||


!process.env.INFISICAL_PROJECT_ID


) {


return;


}


const client = new InfisicalClient({


clientId: process.env.INFISICAL_CLIENT_ID,


clientSecret: process.env.INFISICAL_CLIENT_SECRET,


});


const secrets = await client.listSecrets({


environment: ctx.environment,


projectId: process.env.INFISICAL_PROJECT_ID,


});


return secrets.map((secret) => ({


name: secret.secretKey,


value: secret.secretValue,


}));


}),


\],


},


});


`


You can easily add esbuild plugins to your build process using the` esbuildPlugin` extension. Here's an example of how you can use the` @sentry/esbuild-plugin` with Trigger.dev for automatically uploading sourcemaps to Sentry on deploy:


`
import { sentryEsbuildPlugin } from "@sentry/esbuild-plugin";


import { esbuildPlugin } from "@trigger.dev/build";


import { defineConfig } from "@trigger.dev/sdk";


export default defineConfig({


build: {


extensions: \[


esbuildPlugin(


sentryEsbuildPlugin({


org: process.env.SENTRY_ORG,


project: process.env.SENTRY_PROJECT,


authToken: process.env.SENTRY_AUTH_TOKEN,


}),


{ placement: "last", target: "deploy" }


),


\],


},


});


`


You can also write your own custom extensions to add any custom logic you need to the build process. Extensions are written in TypeScript and can be as simple or complex as you need them to be:


trigger.config.ts


`
import { defineConfig } from "@trigger.dev/sdk";


export default defineConfig({


//..other stuff


build: {


extensions: \[


{


name: "aptGet",


onBuildComplete(context) {


if (context.target === "dev") {


return;


}


context.logger.debug("Adding apt-get layer", {


pkgs: \["ffmpeg"\],


});


context.addLayer({


id: "apt-get",


image: {


pkgs: \["ffmpeg"\],


},


});


},


},


\],


},


});


`


See our[docs](https://trigger.dev/docs/config/config-file) for more on our built-in extensions and how to write your own.


## Bun runtime support


We've added support for the[Bun](https://bun.sh/) runtime in Trigger.dev v3:


`
import { defineConfig } from "@trigger.dev/sdk";


export default defineConfig({


runtime: "bun",


});


`


Allowing you to use Bun APIs and features in your Trigger.dev tasks:


`
import { Database } from "bun:sqlite";


import { task } from "@trigger.dev/sdk";


export const bunTask = task({


id: "bun-task",


run: async (payload: { query: string }) => {


const db = new Database(":memory:");


const query = db.query("select 'Hello world' as message;");


console.log(query.get()); // => { message: "Hello world" }


return {


message: "Query executed",


};


},


});


`


To get started using Bun, check out our[Bun quickstart guide](https://trigger.dev/docs/guides/frameworks/bun) .


## Other improvements


We've completely rewritten our` dev` and` deploy` CLI commands as well, including a much simpler architecture and more shared code. During the beta these commands did not share much code when it came to building and running, which led to regressions and inconsistencies. We've now unified the codebase and made it much easier to maintain and extend. We've also added an end-to-end testing suite for the build system to ensure that we don't introduce regressions in the future and make it easier to refactor and improve the build system. Additionally:


- **Faster cold start times** - We no longer load all your trigger task files when executing a task, instead we load them dynamically as needed. This should lead to faster cold start times.
- **Faster deploys** - We've improved the deploy process to be much faster by optimizing the way we build and push images.
- **ESM builds** - We now output builds as ESM instead of CommonJS, improving the ability to tree-shake out unused code and reducing the size of the final build. This also means we're prepared for the future of JavaScript & TypeScript.
- **Improved configuration** - Our configuration loading is now powered by[c12](https://github.com/unjs/c12) , which is used by Nuxt, Nitro, and other large projects. This should mean fewer bugs as we're using a well-tested library, instead of rolling our own.


## Self-hosting updates


-


**Full support for our latest CLI** - We updated our repo and[self-hosting guide](https://trigger.dev/docs/open-source-self-hosting) to add full support for our latest tag packages. Goodbye` @beta` .


-


**Easy version locking** - You can now more easily lock your CLI and self-hosted instance to the same version tag to ensure compatibility and prevent nasty bugs:


`
TRIGGER_IMAGE_TAG=v3.0.4


`


- **Better self-hosted deploys** - We've improved the` deploy` command for self-hosters of Trigger.dev.` deploy` now supports pushing to a custom registry:


`
npx trigger.dev@latest deploy \\


--self-hosted \\


--load-image \\


--registry docker.io \\


--namespace mydockerhubusername


`


- **Deploy dry runs** - You can now preview what your deployment will look like before actually deploying it:


`
npx trigger.dev@latest deploy --dry-run


`


## Upgrading from the beta


If you are currently running on the beta version of Trigger.dev v3 packages, you can upgrade to the latest version by running` npx trigger.dev@latest update` . Make sure to check out the[upgrade guide](https://trigger.dev/docs/upgrading-beta) for more information on what has changed and how to update your config. If you have any questions or need help with the upgrade, feel free to reach out to us on[Discord](https://trigger.dev/discord) .


NOTE


If you are self-hosting v3 and want to upgrade from the beta, checkout our[self-hosting docs](https://trigger.dev/docs/open-source-self-hosting#updating) for instructions on how to upgrade.


## Upgrading from v2


We announced the end-of-life for Trigger.dev v2 last month, which is coming up on Jan 31st, 2025. So you still have some time to upgrade to v3. We have a[migration guide](https://trigger.dev/docs/guides/use-cases/upgrading-from-v2) to help you with the upgrade process. You can still use the` @trigger.dev/sdk` package and the` @trigger.dev/cli` , and both have matching 3.0.x releases. The same goes for all our v2 integration and framework packages, like` @trigger.dev/nextjs` ,` @trigger.dev/openai` , etc. All the code for these v2 packages can be found in our new[v2 Legacy repo](https://github.com/triggerdotdev/v2-legacy.trigger.dev) .


If you have any questions or need help with the migration, feel free to reach out to us on[Discord](https://trigger.dev/discord) .


## Moving forward


Going forward, our` @trigger.dev/sdk` package and` trigger.dev` CLI will follow semantic versioning, which you can stay up to date on in our[changelog](https://trigger.dev/changelog) and[GitHub Releases](https://github.com/triggerdotdev/trigger.dev/releases) . We're also working on some big new features, like[Realtime Notifications](https://feedback.trigger.dev/p/realtime-notifications) , to bridge the gap between your tasks and your users. Stay tuned for more updates on that and other new features in the coming months.
