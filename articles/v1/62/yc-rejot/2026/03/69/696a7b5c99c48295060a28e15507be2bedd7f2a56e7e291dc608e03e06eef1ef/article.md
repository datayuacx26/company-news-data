---
schema_version: "1.0.0"
document_id: "696a7b5c99c48295060a28e15507be2bedd7f2a56e7e291dc608e03e06eef1ef"
company_key: "yc-rejot"
company: "ReJot"
source_id: "yc-rejot-news-import-01598ccac029"
canonical_url: "https://rejot.dev/blog/resend-essay/"
published_at: "2026-03-27T00:00:00+00:00"
first_seen_at: "2026-07-31T19:47:01.544738+00:00"
fetched_at: "2026-07-31T19:47:01.910446+00:00"
content_hash: "sha256:038389a4d0ea8cae28d848cee847fe11ea1d51a0fc552242c532fb2e8e890ff4"
---

# Receiving Resend inbound email without webhooks

Sending an email using the Resend SDK takes only a couple of lines of code. This is to be expected, as the SDK doesn’t do much more than wrap a fetch to the` POST /emails` endpoint.


Last November, Resend shipped a long-requested feature: **inbound email**\[1\] . How does the SDK help the user handle inbound email? Same thing as with outbound email: wrap a fetch to the` GET /emails/receiving` endpoint.


```text
import   { Resend }   from   'resend'  ;


const   resend   =   new   Resend  (  're_xxxx...xxxxxx'  );


const   { data }   =   await   resend.emails.receiving.  get  (emailId);


console.  log  (  `Email ${  data.id  } has been received`  );
```


Fig. Retrieving an inbound email with the Resend Node.js SDK.


Of course, that doesn’t really help. Resend’s users want to know when an email has been received. The actual integration point for inbound email is a webhook. The SDK doesn’t help with that, at all.


Libraries are inherently limited. We can make them do more if we get rid of the assumption that libraries should only integrate on one side of the stack (usually the backend).


## How could an SDK help with inbound email?


The SDK could help with inbound email by providing a way to register a callback function that will be called when a webhook event is received. Presumably this code path would then also verify the webhook signature and return a 200 status code.


However, this does not actually solve any of the usual friction points of handling incoming webhooks.


To actually be useful, the SDK should be able to define endpoints as part of the user’s application. This way, the SDK can handle idempotency and deduplication. To do this, the SDK needs to be able to write to the user’s database as well.


Since we’re already traversing the stack, let’s also bring the frontend in scope. Eventually, the email will have to be shown in the user’s application. So it should be possible to retrieve the email from the frontend. This is usually directed through the backend, which then queries the database. If the SDK could also provide the user with frontend reactive hooks, that would truly be useful.


Fig. Typical integration points for a third-party service connected through webhooks.


## To fix this, we built a Resend “full-stack library”


But first, we had to invent the primitives for that: **Fragno** . Fragno is a toolkit that makes it possible to build full-stack libraries. These libraries are made out of:


- Server-side API routes
- Client-side reactive hooks
- A database schema


Precisely the ingredients needed to receive and store inbound email events durably. The figure below shows what such an implementation in Fragno might look like.


01_WEBHOOK_ROUTE


The` /resend/webhook` route is defined as part of the Resend library. This route will be mounted in the user's application.


As is best practice for handling webhooks, the route handler only verifies the signature, stores the event payload, and then immediately returns 200. An async operation is queued to call the` onResendWebhook` callback, shown on the next tab.


```text
export   const   registerWebhookRoutes   =   ({ defineRoute, config, deps })   =>   [
defineRoute  ({
method:   "POST"  ,
path:   "/resend/webhook"  ,
outputSchema: z.  object  ({ success: z.  boolean  () }),
errorCodes: [  "MISSING_SIGNATURE"  ,   "WEBHOOK_SIGNATURE_INVALID"  ,   "WEBHOOK_ERROR"  ]   as   const  ,
handler  :   async   function   ({ headers, rawBody }, { json, error }) {
if   (  !  config.webhookSecret) {
return   error  ({ message:   "Missing webhook secret in config"  , code:   "WEBHOOK_ERROR"   },   400  );
}


let   event;
try   {
event   =   deps.resend.webhooks.  verify  ({ payload: rawBody, headers, webhookSecret: config.webhookSecret });
}   catch   (err) {
return   error  ({ message:   formatErrorMessage  (err), code:   "WEBHOOK_SIGNATURE_INVALID"   },   400  );
}


await   this.  handlerTx  ()
.  mutate  (({ forSchema })   =>   {
const   uow   =   forSchema  (resendSchema);
uow.  triggerHook  (  "onResendWebhook"  , { event });
})
.  execute  ();


return   json  ({ success:   true   });
},
}),
];
```


02_CALLBACK


A background processor picks up the operation and executes the callback. This hook first retrieves the email content from Resend's API and then persists it to the database.


Since handling webhooks is a common pattern for Fragno, we included the concept of durable hooks


\[2\] . The handler you see here is automatically retried on failure, such as when a call to Resend fails or the user-defined` onEmailReceived` callback throws.


```text
onResendWebhook:   defineHook  (  async   function   ({ event }) {
if   (  !  isReceivedEmailWebhookEvent  (event))   return  ;


// Retrieve the actual email content from Resend
const   { data: receivedDetail, error }   =
await   deps.resend.emails.receiving.  get  (event.data.email_id);


if   (error)   throw   new   Error  (error.message);


await   this.  handlerTx  ()
.  mutate  (({ forSchema })   =>   {
const   uow   =   forSchema  (resendSchema);
uow.  create  (  "emailMessage"  , {
direction:   "inbound"  ,
status:   "received"  ,
providerEmailId: receivedDetail.id,
from: receivedDetail.from,
to: receivedDetail.to,
subject: receivedDetail.subject,
headers: receivedDetail.headers,
html: receivedDetail.html,
text: receivedDetail.text,
occurredAt: receivedAt,
lastEventType: event.type,
lastEventAt: webhookReceivedAt,
});


if   (config.onEmailReceived) {
await   config.  onEmailReceived  ({
// ..
});
};
})
.  execute  ();
}),
```


03_SCHEMA


A minimal schema slice for inbound persistence.


From this schema, Fragno is able to generate a schema in the user's ORM of choice (Drizzle, Prisma, Kysely). In the case of Cloudflare Durable Objects, the schema can also be migrated directly, skipping the need for an ORM.


```text
export   const   resendSchema   =   schema  (  "resend"  , (s)   =>
s.  addTable  (  "emailMessage"  , (t)   =>
t
.  addColumn  (  "direction"  ,   column  (  "string"  ))
.  addColumn  (  "status"  ,   column  (  "string"  ))
.  addColumn  (  "providerEmailId"  ,   column  (  "string"  ).  nullable  ())
.  addColumn  (  "from"  ,   column  (  "string"  ).  nullable  ())
.  addColumn  (  "to"  ,   column  (  "json"  ))
.  addColumn  (  "subject"  ,   column  (  "string"  ).  nullable  ())
.  addColumn  (  "headers"  ,   column  (  "json"  ).  nullable  ())
.  addColumn  (  "html"  ,   column  (  "string"  ).  nullable  ())
.  addColumn  (  "text"  ,   column  (  "string"  ).  nullable  ())
.  addColumn  (  "occurredAt"  ,   column  (  "timestamp"  ))
.  addColumn  (  "lastEventType"  ,   column  (  "string"  ).  nullable  ())
.  addColumn  (  "lastEventAt"  ,   column  (  "timestamp"  ).  nullable  ()),
),
);
```


Fig. The key pieces of the Resend full-stack library.


A full-stack library can do much more than just wrap API endpoints like a traditional library. All logic, from ingesting events, handling retries, persisting data, and showing the data in the UI, can be implemented in the library.


But there are even more opportunities for full-stack libraries.


## Full-stack libraries also help ship *taste*


Threads in email clients are not an inherent concept of email. They’re emergent behavior based on the headers and metadata of the email. As such, I don’t think it makes sense to have threads as a first-class feature of the Resend platform. It’s something users can build themselves if need be.


But it might make sense to ship a library that **adds an opinionated layer** for threading, reply routing, deduplication, and conversation history as first-class concerns.


That is what we did in the implementation of the Resend full-stack library. On the client, the user gets typed hooks:` useThreads` ,` useThreadMessages` , and the` useReplyToThread` mutator. These send requests to the user’s own backend as needed. The only thing left then is to build the UI on top of these hooks.


```text
import   { createResendFragmentClient }   from   "@fragno-dev/resend-fragment/react"  ;


const   resend   =   createResendFragmentClient  ({ mountRoute:   "/api/resend"   });


function   SupportInbox  () {
const   { data: threads }   =   resend.  useThreads  ();
const   { data: messages }   =   resend.  useThreadMessages  (threadId);
const   reply   =   resend.  useReplyToThread  ();


return   (
<  ThreadList   threads  =  {threads}  >
<  MessageTimeline   messages  =  {messages}   />
<  ReplyBox   onSend  =  {(text)   =>   reply.  mutate  ({
path: { threadId },
body: { text },
})}   />
</  ThreadList  >
);
}
```


Fig. A support inbox built on three hooks. Thread history, message timeline, and replies.


As emails are sent and received, email headers are matched and the resolved threads are stored in the database. The end-user develops at a higher abstraction layer, not having to be bothered with the details of the email protocol.


## Bonus: asynchronous email sending


One overlooked aspect of calling third-party APIs is the fact that no one should be calling an API such as Resend synchronously. If we want to send a transactional email to a user that just signed up, we shouldn’t be waiting for the email to be sent before we return a success response to the user. Instead, we need to store the intent of sending an email to the database, in the same transaction as processing the user’s sign-up.


This is also something traditional libraries don’t provide any help with.


The same` triggerHook(...)` pattern we saw earlier for handling webhooks can be reused to trigger the email sending process in the background. This provides resilience against the provider being down, as well as configuration errors such as a missing API key.


## User-facing integration


Full-stack libraries are a bit harder to integrate than traditional libraries. Instead of only installing and importing the package, it also needs to be mounted and hooked up to the database. When durable hooks are used, a background process is also required.


This example shows how to integrate the Resend library into an application.


01_CREATE_FRAGMENT


First, a single server instance has to be created. This is where credentials and defaults live, and where the database adapter is configured.


The` onEmailReceived` callback is the user-facing counterpart of the internal hook we saw earlier.


```text
import   { createResendFragment }   from   "@fragno-dev/resend-fragment"  ;
import   { databaseAdapter }   from   "./db"  ;


export   const   resendFragment   =   createResendFragment  (
{
apiKey: process.env.RESEND_API_KEY  !  ,
webhookSecret: process.env.RESEND_WEBHOOK_SECRET  !  ,
defaultFrom:   "Support <support@example.com>"  ,
onEmailReceived  :   async   ({ threadId })   =>   {
console.  log  (  "Inbound →"  , threadId);
},
},
{ databaseAdapter, mountRoute:   "/api/resend"   },
);
```


02_MOUNT_ROUTES


Then the library's routes are mounted by routing requests to its handler from both` loader` and` action` . The combination covers all HTTP verbs in React Router.


```text
import   type   { Route }   from   "./+types/api.resend"  ;
import   { resendFragment }   from   "@/lib/resend-fragment-server"  ;


export   async   function   loader  ({ request }  :   Route  .  LoaderArgs  ) {
return   await   resendFragment.  handler  (request);
}


export   async   function   action  ({ request }  :   Route  .  ActionArgs  ) {
return   await   resendFragment.  handler  (request);
}
```


03_GENERATE_DRIZZLE


Finally, generate a Drizzle schema file from the fragment schema so it can be owned and migrated alongside the rest of your app's database.


This can also be adapted for other ORMs, or no ORM at all.


```text
# Generate a Drizzle schema from the fragment's schema
# (so you can integrate it into your app's migrations workflow)


npx   fragno-cli   db   generate   lib/resend-fragment-server.ts   --format   drizzle   -o   app/db/resend.schema.ts
```


Fig. Integrating the Resend library: create the server instance, mount it in your framework, and generate a Drizzle schema for your migrations.


The entire process may be handled with a coding agent using the Fragno integration skill.


Agent skill


This skill includes a list of first-party Fragno libraries to make installing them easy. Just ask your agent to integrate the Resend fragment.


```text
npx   skills   add   https://github.com/rejot-dev/fragno   --skill   fragno
```


Install


Or install the package and integrate it manually.


```text
npm   install   @fragno-dev/resend-fragment   @fragno-dev/db
```


## A model for every API provider.


The Resend library is a proof of concept for how any API provider can ship opinionated, full-stack integration libraries.


There are now several “webhookless” Stripe providers/wrappers, presumably because the Stripe API/event surface is now complicated enough to warrant it. The title of this article is a nod to that. Of course, we actually do use webhooks, but this is hidden from the user. We also built a proof of concept for webhookless Stripe:[see here](https://fragno.dev/fragments/stripe) .


We did the same for some other API providers, for example:[Telegram Bots](https://fragno.dev/fragments/telegram) , and[GitHub Apps](https://fragno.dev/fragments/github) . More can be found on the[“Fragments” page](https://fragno.dev/fragments) . I’ll write more on those later.


In an ideal world, these libraries would be created by the API providers themselves, as they are the foremost experts on their own APIs. They could ship full-stack libraries instead of leaving integrators to figure it out themselves.


## Our Fragno Claw-like agent


We’re building full-stack libraries to be integrated into our own Claw-like agent. Vertical slices make the maintainability of the entire application easier.


Fig. Resend library as integrated into our “Claw-like”.


## Footnotes


1.


[Resend: Inbound Emails](https://resend.com/blog/inbound-emails)↩
2.


Durable hooks are persisted background hook executions that automatically retry on failure. They keep webhook-driven work reliable by decoupling intake from processing.↩


## Further reading


The Resend library lives in the broader Fragno ecosystem, so start with the fragment overview, read the main essay, or jump straight into the docs.


- [The Fragno full-stack encapsulation essay](https://rejot.dev/blog/2026-03-23_fragno-home-page)
- [The Resend fragment overview](https://fragno.dev/fragments/resend)
- [Fragno user quick start](https://fragno.dev/docs/fragno/user-quick-start)
- [Fragno on GitHub](https://github.com/rejot-dev/fragno)
- [Join the Fragno Discord](https://discord.gg/jdXZxyGCnC)
