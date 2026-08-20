---
schema_version: "1.0.0"
document_id: "84b9ee70c0aa3a8bef473c8d2810758569b6351aa3840ce51746d8dbb43e494b"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/v3-open-access"
published_at: "2026-07-23T12:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:f7f18207594d9b90d77b5f3c918e31d79a532a8b0f5ddc810bc49415a146e625"
---

# Trigger.dev v3 is now open to everyone

Today, I'm excited to announce that the Trigger.dev v3 cloud is now open to everyone. No more waitlist!


Over the past few months we've been[hard at work](https://trigger.dev/changelog) building new features, improving our SDK and making the platform as reliable as possible to reach this milestone.


## Trigger.dev v3 in 40 seconds


## Why we built v3 (and why it's better than v2)


Before diving into the details, let's briefly revisit how we got here. The answer is straightforward; after speaking to hundreds of v2 users, we identified some key issues that needed addressing:


- **Timeouts** . Version 2 ran your code inside your serverless functions, often suffering from timeouts. We provided ways to divide jobs into smaller chunks, but the code was hard to write and confusing.
- **Very long-running processes.** Some use cases need to run for extended periods, which v2 couldn't handle. Tasks like video transcoding or certain AI operations often exceed even the most generous serverless timeouts, such as those in Lambda.
- **Double billing** . Users had to pay for both our platform and their hosting platform costs to run their jobs.


To truly eliminate timeouts, we knew we needed to build our own infrastructure to host, manage, and scale tasks.[This new architecture](https://trigger.dev/blog/v3-developer-preview-launch) led to Trigger.dev version 3, and finally to today's announcement that we're now open access.


## Introducing Trigger.dev v3 open access with three new plans: Free, Hobby and Pro


You can now choose from three new plans: Free, Hobby and Pro, designed to help you evaluate the product and unlock features as you go. Check out the full details on the[pricing page](https://trigger.dev/pricing) and[sign up for a free account](https://cloud.trigger.dev/) to explore Trigger.dev v3.


Each plan includes monthly usage, with the Free plan offering $5 of monthly usage for free. The machines executing runs on the platform are configurable, and prices for each can be seen on our[pricing page](https://trigger.dev/pricing#computePricing) . We also charge a small["run invocation" cost](https://trigger.dev/pricing#runPricing) .


Existing v3 users have been put on the Free plan, **meaning you'll need to upgrade to exceed the free $5 of usage per month** . Once the $5 of free usage is exhausted, existing runs will be queued, and new ones won't be created until you upgrade.


For v2 users, simply[log in](https://cloud.trigger.dev/) and create a new project – v3 is now the default. To upgrade an existing v2 job to a v3 task, check out our[migration guide](https://trigger.dev/docs/guides/use-cases/upgrading-from-v2) . You can migrate v2 jobs incrementally, and if you need help, reach out via[email](https://trigger.dev/contact) or[Discord](https://trigger.dev/discord) .


> I need to work more with trigger, but I'm very impressed by what you guys are doing. versioned deploys, multiple environments, easy local development, long running tasks with no shutdowns, very fair and scaling pricing, tenant scoped concurrency limits, open source, waitFor with events or requests, awaiting/polling results. its crazy


## Self hosting


If you prefer to self-host, we've made a[handy guide](https://trigger.dev/docs/open-source-self-hosting) . We're Apache 2.0 licensed and have made it as easy as possible to self-host using Docker. As always you can view[the source code](https://github.com/triggerdotdev/trigger.dev) on GitHub.


## Price transparency


Figuring out how much you're spending with cloud providers is hard. We've kept it simple and transparent.


You can easily see how much each task costs to run in Production or Staging by visiting the new Usage page. We also provide a monthly estimate and a daily summary.


As developers, it makes sense to access this data using the SDK. From inside a run, you can get cost and duration data like this:


`
export const heavyTask = task({


id: "heavy-task",


machine: {


preset: "medium-2x",


},


run: async (payload, { ctx }) => {


// Do some compute


const result = await convertVideo(payload.videoUrl);


// Get the current cost and duration up until this line of code


// This includes the compute time of the previous lines


let currentUsage = usage.getCurrent();


/* currentUsage = {


compute: {


attempt: {


costInCents: 0.01700,


durationMs: 1000,


},


total: {


costInCents: 0.0255,


durationMs: 1500,


},


},


baseCostInCents: 0.0025,


totalCostInCents: 0.028,


}


*/


},


});


`


Hosting and managing users code on our platform also solved the double billing problem. Users are no longer charged for both their background jobs provider *and* their hosting provider. It's still an issue our competitors face.


> With v3 everything just works


*Max – Waitless*


## What's next?


With v3 now open to everyone, we're just getting started. Here's 3 things we're currently working on:


- **Realtime Notifications:** send data from your tasks to your front end in realtime
- **Better bundling:** Make deploying far easier
- **Firecracker** : For blazing fast run starts


To request a feature, or vote on others, check out our[Feedback page](https://feedback.trigger.dev/) .
