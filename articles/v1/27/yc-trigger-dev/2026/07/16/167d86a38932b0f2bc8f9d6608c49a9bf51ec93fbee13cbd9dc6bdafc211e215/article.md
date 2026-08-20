---
schema_version: "1.0.0"
document_id: "167d86a38932b0f2bc8f9d6608c49a9bf51ec93fbee13cbd9dc6bdafc211e215"
company_key: "yc-trigger-dev"
company: "Trigger.dev"
source_id: "yc-trigger-dev-news-import-c59968a07222"
canonical_url: "https://trigger.dev/blog/v3-announcement"
published_at: "2026-07-23T12:00:00+00:00"
first_seen_at: "2026-07-22T17:13:09.827889+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:f2942eb69433bf81b8f2ed973e475767bac03dca313e89a84c12fa22b09ac42f"
---

# Trigger.dev v3: Durable Serverless functions. No timeouts.

UPDATE: Trigger.dev v3 is now open access.[Sign up for a free account](https://cloud.trigger.dev/) .


---


Trigger.dev v2 allows you to create durable long-running code that successfully avoids serverless timeouts. We achieve this by using a simple trick: caching completed chunks and replaying the function repeatedly until everything is finished. But there are some key downsides, some of which we can't fix while your code is executing inside your serverless functions.


To solve this problem completely, and make it easy for you, we need to run your code and use a pretty amazing piece of technology: CRIU. More on that later.


## Durable long-running tasks


Achieving long-running code isn't hard – you just need to have a long-running server and get the code onto it. Before "serverless" this was how everything worked. Localhost is a long-running server, and you can run code on it for as long as you want (or until your cat sits on the power button).


There are a couple of issues though that need to be dealt with:


1. Sometimes you want to wait for something to happen before continuing to the next line of code. That could be waiting until a specific point in time, for a specific event to happen, or for an HTTP request.
2. Servers go down. Mostly this is caused by deploying new code. Servers do also (very rarely) fail. You don't want to start your task from scratch when this happens especially if mutations have happened that aren't[idempotent](https://en.wikipedia.org/wiki/Idempotence) .


## Writing regular async code with no timeouts and durability


Our ultimate goal is to enable you to write normal async code, without timeouts and inherently durable, without resorting to awkward or error-prone syntax.


This is how a task for purchasing movie theatre tickets will look in v3:


trigger/


purchase.ts


`
// Purchase flow for movie theatre tickets


export const purchaseTicket = task({


id: "purchase-ticket",


run: async ({


payload,


}: {


payload: { ticketId: string; userId: string };


}) => {


// First we need to reserve the ticket


const reservedTicket = await reserveTicket(


payload.ticketId,


payload.userId


);


// Logs show up in the Trigger.dev UI


console.log("Reserved ticket", { reservedTicket });


// Release the ticket if there are errors after this point


rollback(async () => releaseTicket(reservedTicket.id));


// Give the user a 5 minute window to checkout.


// Will throw an error if the user doesn't checkout in time


const event = await events.waitForEvent({


event: "cart/checked-out",


filter: {


items: \[payload.ticketId\],


userId: \[payload.userId\],


},


timeout: { minutes: 5 },


});


console.log("Checked out", { checkout: event.payload });


// Now lookup the cart


const cart = await db.findCart(event.payload.cartId);


// and charge the user


const charge = await stripe.charges.create({


amount: cart.total,


currency: "usd",


source: cart.paymentSource,


description: \`Ticket purchase for ${cart.items\[0\].name}\`,


});


console.log("Charged user", { charge });


// Refund the charge if there are any errors after this point


rollback(async () =>


stripe.refunds.create({


charge: charge.id,


})


);


// Finalize the ticket


await finalizeTicket(reservedTicket.id, payload.userId);


console.log("Finalized ticket");


//send confirmation email to the user


const emailResult = await resend.emails.send({


from: "[\[email protected\]](https://trigger.dev/cdn-cgi/l/email-protection) ",


to: cart.email,


subject: "Ticket Purchase Confirmation",


text: \`Thanks for purchasing a ticket to ${cart.items\[0\].name}!\`,


});


if (emailResult.error || emailResult.data === null) {


//this will cause the rollbacks to run


throw new Error("Failed to send email");


}


//send a Slack message to our team


try {


//this uses the official Slack SDK


await slack.chat.postMessage({


channel: "C1234567890",


text: \`Someone just purchased a ticket to ${cart.items\[0\].name}!\`,


});


} catch (e) {


// Don't throw an error here, since it's not critical


console.error("Failed to send Slack notification", e);


}


},


});


`


Notice that many things from v2 are no longer needed. You don't need to use` io.runTask` to "cache" things for replays, and you can just use regular SDKs. In fact, you don't need to think about timeouts at all, since they don't exist.


This is how you would trigger this purchase ticket task from your code:


app/api/reserve/


route.ts


`
import { purchaseTicket } from "~/trigger/purchase";


//you'd call this somewhere in your backend


const taskHandle = await purchaseTicket.trigger({


payload: {


ticketId: "tkt_12345",


userId: "usr_12345",


},


});


`


Note that this function returns a` TaskHandle` from the API, it does not wait until the task has completed. You can use the handle to look up the status of the task, cancel, retry and more.


In the middle of the task there's this interesting piece of code:


`
// Give the user a 5 minute window to checkout.


// Will throw an error if the user doesn't checkout in time


const event = await events.waitForEvent({


event: "cart/checked-out",


filter: {


items: \[payload.ticketId\],


userId: \[payload.userId\],


},


timeout: { minutes: 5 },


});


console.log("this will only be executed if a matching event is received");


`


When this code runs execution will pause and the server will get spun down. You could set a very long timeout here if you wanted, although it doesn't make sense for this example. You won't pay for compute time while it's waiting because the code is no longer executing. More on how this is achieved in a moment.


For execution to continue you need to send a matching event when the user has actually pressed the checkout button:


app/api/checkout/complete/


route.ts


`
import { events } from "~/trigger/cart";


//somewhere in your backend code


const sentEvent = await events.trigger({


event: "cart/checked-out",


payload: {


userId: "usr_12345",


cartId: "cart_12345",


},


});


`


## How does this work?


### Checkpoints and Restoring


When deployed, the code will run in a container that will be paused and resumed using[Checkpoint/Restore In Userspace (CRIU)](https://criu.org/Main_Page) .


CRIU is a Linux tool that allows you to freeze a running container and checkpoint it to disk. You can then restore the application from the checkpoint at a later time on a different machine. This is similar to how you can hibernate your computer and then resume it later. Google have been[using this at scale internally since 2017](https://lpc.events/event/2/contributions/69/attachments/205/374/Task_Migration_at_Scale_Using_CRIU_-_LPC_2018.pdf) to pause low priority tasks and then continue them later on different machines.


We will automatically checkpoint your task when:


Function What it does


` wait.for()` Waits for a specific period of time, e.g. 1 day.


` wait.until()` Waits until the provided` Date` .


` wait.forRequest()` Waits until a matching HTTP request is received, and gives you the data to continue with.


` event.waitForEvent()` Waits for a matching event, like in the example above.


` task.triggerAndWait()` Triggers a task and then waits until it's complete. You get the result data to continue with.


` task.batchTriggerAndWait()` Triggers a task multiple times in parallel and then waits until they're all complete. You get the resulting data to continue with.


In all of those situations the code will stop executing and will be resumed at a later date. You won't pay for compute time while it's waiting because the code is no longer executing.


### Where does this run?


Your code will run in containers that support CRIU and workloads will scale up and down automatically. This is a major change from how it works in v2 where your code runs in your own serverless functions.


This is required for zero-timeout durable code that is easy to write.


It also has the benefit of simplifying costs. With v2 you pay us for orchestrating runs and you pay your cloud provider separately for compute time of your serverless functions. With v3 we provide durable compute and orchestration. We continue to be committed to open-source and self-hosting, more details on that later.


### No timeouts


Most "serverless" platforms have timeouts. Some are very limiting like 10s on the Vercel free plan, others are higher like 15 minutes on AWS Lambda. Even 15 minutes is a problem for lots of tasks.


Version 3 has no timeouts. You can run code for as long as you want and since execution can be paused it will be efficient.


### Versioning and immutable deploys


Every deploy will create a new version of your tasks (e.g.` 2024-01-19-1` ). When a run starts it is locked to that version and deployed versions aren't deleted or modified. This means that if you deploy a new version of your code after a task has started executing it will continue to run uninterrupted on the older version.


This means:


- New deploys don't impact started tasks.
- You don't have to worry about breaking changes impacting running tasks.
- You can "migrate" running tasks to different versions, like re-running failed tasks on a new version of your code.


### Server hardware


Most of the time you don't need beefy hardware or have unusual requirements. But sometimes you do. For example, you might be doing something CPU or RAM intense, or you might need to use FFmpeg or Puppeteer.


You can specify machine specs on a task:


trigger/


encode-video.ts


`
export const encodeVideo = task({


id: "encodeVideo",


machine: {


image: "ffmpeg",


cpu: 1,


memory: 512,


},


run: async ({ payload }: { payload: string }) => {


//do stuff


},


});


`


## The DX for running locally and deploying


### Local development


In your project you'll add your tasks inside` trigger` folders. We'll also have a` trigger.config.js` (or` .mjs` ) file with some settings.


To run locally you'll use our new CLI dev command to run your tasks and simulate checkpointing. The behaviour will be the same as when deployed, except that it will run in a non-containerized Node.js process.


### Bundling and deployment


There will be multiple ways to deploy:


1. Use the CLI deploy command.
2. Use GitHub Actions, or other CI/CD tools.
3. A GitHub app on Trigger.dev. This will allow you to select a repo and we'll automatically deploy on every` main` and PR push. This is how Vercel works.


### Environment variables


As we'll be running your code we will need Environment Variables for things like API keys. These will be securely stored in the same way we currently do for integration credentials.


To save you having to add these in two places we will build integrations to sync them. First will probably be a Vercel integration that will sync overlapping secrets from Vercel to Trigger.dev.


## Integrations and webhooks


In v2 integrations allow you to easily trigger jobs using webhooks and perform actions inside your` run` functions. For example, you can easily[subscribe to new GitHub stars and send a Slack message with details about it](https://trigger.dev/apis/github) . We support using API Keys and OAuth to authenticate with these services.


Here's an example of a v3 task that sends Stripe subscription change notifications:


trigger/


stripe.ts


`
import { stripeWebhooks } from "@trigger.dev/sdk";


import { WebClient } from "@slack/web-api";


import { Resend } from "resend";


//this is similar to v2, but it is just for webhooks


const stripe = stripeWebhooks({


id: "stripe",


});


//these are the official Slack and Resend SDKS, NOT integrations


const slack = new WebClient(process.env.SLACK_TOKEN!);


const resend = new Resend(process.env.RESEND_API_KEY!);


//this is how you'll subscribe to webhooks


export const stripePlanChanged = stripe.task({


//the official webhook event names


on: "customer.subscription.updated",


id: "subscription-plan-changed",


//payloads will be nicely typed as they are in v2


run: async ({ payload, context }) => {


const user = await db.users.find({ stripeId: payload.customer });


const planId = getNewPlanId(payload);


if (user.planId !== planId) {


await db.users.update(user.id, { planId });


//this is using the official Resend SDK


await resend.emails.send({


to: user.email,


from: "[\[email protected\]](https://trigger.dev/cdn-cgi/l/email-protection) ",


subject: "Your plan has changed",


html: planEmail(payload),


});


if (isPlanUpgraded(user.planId, planId)) {


//this is using the official Slack SDK


await slack.chat.postMessage({


text: \`Plan upgraded for ${user.email} to ${planId}\`,


channel: "subscriptions",


});


}


}


},


});


`


There are some important changes highlighted by this code:


1. Webhooks work the same although the syntax is a bit nicer.
2. Integrations aren't needed for performing actions inside run functions. As mentioned before, there is no need to wrap code in` io.runTask` to avoid timeouts. So you can just use SDKs like you normally would, use HTTP requests, or do anything that would normally work in a Node.js process.


### OAuth, credentials and Trigger.dev Connect


In v2 we supported OAuth for integrations like Slack and Supabase. We will add support for OAuth in v3 that will work with webhooks and be available to use from our SDK.


From the Trigger.dev app you can do an OAuth flow and we will securely store and refresh the tokens. From anywhere inside your code (including outside the` trigger` folder) you will be able to retrieve them using our SDK – so you can authenticate with APIs.


Trigger.dev Connect will make it easy for you to collect OAuth and API keys from your users. You can then use them to subscribe to webhooks and use SDKs with your users' credentials.


## Open-source and self-hosting


We continue to be 100% committed to open-source.


We're figuring out how to make self-hosting v3 as easy as possible. It will be harder to self-host than v2 because it will no longer be possible to use a single Docker container and checkpointing will require CRIU-compatible system. CRIU is pretty widely supported across cloud providers.


## What about Trigger.dev v2?


Trigger.dev v2 and v3 will live side-by-side. When creating a new project you will be able to choose which version you want to use.


## Feedback and the developer preview


The continuous conversations and feedback we get from all of you has had a huge impact on how Trigger.dev works, and made us realize that we needed to make these changes.


Please let us know your honest thoughts and concerns on[Discord](https://trigger.dev/discord) ,[Twitter](https://twitter.com/triggerdotdev) , or[via email](https://trigger.dev/contact) .
