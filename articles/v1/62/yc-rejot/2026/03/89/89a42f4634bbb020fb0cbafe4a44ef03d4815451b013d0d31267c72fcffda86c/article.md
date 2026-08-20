---
schema_version: "1.0.0"
document_id: "89a42f4634bbb020fb0cbafe4a44ef03d4815451b013d0d31267c72fcffda86c"
company_key: "yc-rejot"
company: "ReJot"
source_id: "yc-rejot-news-import-01598ccac029"
canonical_url: "https://rejot.dev/blog/fragno-home-page/"
published_at: "2026-03-23T00:00:00+00:00"
first_seen_at: "2026-07-31T19:47:01.544738+00:00"
fetched_at: "2026-07-31T19:47:01.910446+00:00"
content_hash: "sha256:d30f301955cb6960f297d75563c421b70691c06aec00417c77fc667133e3ed65"
---

# Fragno: full-stack encapsulation

**Ensloppification** is what happens when you let AI generate code without strong abstractions. By designing vertical slices of functionality, agents can work on many modules concurrently.


Libraries are the best form of abstraction we have because they provide **encapsulation** . But libraries are limited. They’re only one layer of the slice: frontend or backend. Not to mention the data layer. *Fragno* is changing that by providing the primitives to build **full-stack libraries** .


## What is a full-stack library made of?


- Server-side API routes — Handle secure operations, API keys, and network orchestration close to the backend.


- Client-side reactive hooks — Expose stateful UI primitives that keep the integration ergonomic in the app shell.


- A database schema — Persist durable feature data so behavior survives sessions and deployments.


These three components make it so that a library can do much more: it can now dictate the entire user experience, a real vertical slice. This gives the author the opportunity to embed **taste** into the library.


Compare this to traditional libraries that only provide backend functions. The integrator still needs to create routes, define hooks, wire them up, and store data if needed.


Take Vercel’s` aisdk` , a clear **example** of a library that has to span both the frontend and the backend. The user interaction on the frontend is the heart of the experience, while the backend holds API keys and handles function calling. Contrast that to the` openai` library, which only provides the backend functionality and leaves the user to do the rest.


But even the` aisdk` library is limited. It provides backend and frontend, but does not include the data layer. As such, it doesn’t provide a way to store conversations, or have persistent LLM memory.


Fragno proposes a different unit of composition: the **Fragment** . A Fragment is a portable full-stack library slice that carries its own transport surface, optional schema, and client integration. Instead of teaching the developer to rebuild the same feature boundary in three places, it treats that boundary as one authored object.


## What can be built with Fragno?


Well, anything. But these are some examples from the Fragno ecosystem.


### Pi Agents


Pi is the minimal agent runtime. With our fragment, you get durable agent sessions with easy tool calling and session management from the frontend.


[View fragment →](https://fragno.dev/fragments/pi)


### Forms


Create forms using the form builder, and track submissions from your own backoffice or database.


[View fragment →](https://fragno.dev/fragments/forms)


### Workflows


Package long-running orchestration with routes, durable state, and client controls together, so workflow behavior stays consistent across frameworks.


[View fragment →](https://fragno.dev/fragments/workflows)


But that is not all. We have fragments for[Stripe Billing](https://fragno.dev/fragments/stripe) ,[Telegram Bots](https://fragno.dev/fragments/telegram) ,[Resend Email](https://fragno.dev/fragments/resend) ,[GitHub Apps](https://fragno.dev/fragments/github) , and[S3 Uploads](https://fragno.dev/fragments/upload) .


[View all fragments →](https://fragno.dev/fragments)


## How does it work?


Fragno is a full set of primitives: it contains handlers for all popular full-stack frameworks, as well as database integrations for popular ORMs and SQL databases.


Below you’ll find some examples of how fragments are constructed and used in production.


AI agents with durable sessions


This is from our backoffice. A Pi agent is defined with a system prompt, a model, and tools. The fragment creates durable workflow-backed sessions, so agent state survives restarts and tool calls are executed through normal workflow turns. The React client gets typed hooks for sessions and messages with zero glue code.


```text
import   { createPiHarness, createPiWorkflows }   from   "@fragno-dev/pi-harness/factory"  ;
import   { createInteractiveChatWorkflow }   from   "@fragno-dev/pi-harness/workflows/interactive-chat-workflow"  ;
import   { createWorkflowsFragment }   from   "@fragno-dev/workflows"  ;


const   interactiveChat   =   createInteractiveChatWorkflow  ({
harnesses: {
support: {
env,
model,
systemPrompt:   "You are a helpful support agent."  ,
tools: [bashTool],
},
},
});


const   piConfig   =   { workflows: [interactiveChat] };
const   workflowsFragment   =   createWorkflowsFragment  (
{ workflows:   createPiWorkflows  (piConfig), runtime: defaultFragnoRuntime },
{ databaseAdapter: adapter, mountRoute:   "/api/workflows"   },
);


export   const   piFragment   =   createPiHarness  (
piConfig,
{ databaseAdapter: adapter, mountRoute:   "/api/pi"   },
{ workflows: workflowsFragment.services },
);
```


```text
import   { createPiFragmentClient }   from   "@fragno-dev/pi-harness/react"  ;


const   pi   =   createPiFragmentClient  ({ mountRoute:   "/api/pi"   });
const   createSession   =   pi.  useCreateSession  ();


const   created   =   await   createSession.  mutateQuery  ({
path: { workflowName:   "interactive-chat-workflow"   },
body: { name:   "Customer issue"  , input: { harnessName:   "support"   } },
});


const   session   =   pi.  useSession  ({
path: { workflowName: created.workflowName, sessionId: created.id },
});


await   session.  sendCommand  ({
kind:   "prompt"  ,
input: { text:   "Summarize the bug report and propose next steps."   },
});
```


Forms on a Durable Object with embedded SQLite


This is how the Fragno docs site runs its own Forms fragment. The fragment carries its own database schema, service layer, and HTTP handler. You create it, wrap it in a Cloudflare Durable Object, call migrate() in the constructor, and delegate fetch().


```text
import   { createFormsFragment }   from   "@fragno-dev/forms"  ;
import   { SqlAdapter }   from   "@fragno-dev/db/adapters/sql"  ;
import   { DurableObjectDialect }   from   "@fragno-dev/db/dialects/durable-object"  ;
import   { CloudflareDurableObjectsDriverConfig }   from   "@fragno-dev/db/drivers"  ;


export   function   createFormsServer  (init  :   FormsInit  ) {
const   adapter   =   new   SqlAdapter  ({
dialect:   new   DurableObjectDialect  ({ ctx: init.state }),
driverConfig:   new   CloudflareDurableObjectsDriverConfig  (),
});


return   createFormsFragment  ({}, { databaseAdapter: adapter });
}
```


```text
import   { DurableObject }   from   "cloudflare:workers"  ;
import   { migrate }   from   "@fragno-dev/db"  ;
import   { createFormsServer }   from   "./fragno/forms"  ;


export   class   Forms   extends   DurableObject<  CloudflareEnv  > {
#fragment  :   ReturnType  <  typeof   createFormsServer>;


constructor  (state  :   DurableObjectState  , env  :   CloudflareEnv  ) {
super(state, env);


this.#fragment   =   createFormsServer  ({ env, state, type:   "live"   });


// Schema is ready before the first request
state.  blockConcurrencyWhile  (()   =>   migrate  (this.#fragment));
}


async   fetch  (request  :   Request  )  :   Promise  <  Response  > {
return   this.#fragment.  handler  (request);
}
}
```


```text
import   { Hono }   from   "hono"  ;
export   { Forms }   from   "./forms.do"  ;


const   app   =   new   Hono  <{ Bindings  :   CloudflareEnv   }>()
.  all  (  "/api/forms/*"  , (c)   =>   {
const   stub   =   c.env.FORMS.  get  (c.env.FORMS.  idFromName  (  "default"  ));
return   stub.  fetch  (c.req.raw);
});


export   default   app;
```


Telegram bots with typed commands and replies


This is the Telegram integration pattern we use in Fragno apps. You define commands with scopes and typed handlers, then let the fragment process incoming webhooks and persist chats/messages. Handlers can reply through the Telegram API wrapper without building custom routing glue.


```text
import   {
createTelegram,
createTelegramFragment,
defineCommand,
}   from   "@fragno-dev/telegram-fragment"  ;


const   telegramConfig   =   createTelegram  ({
botToken: process.env.TELEGRAM_BOT_TOKEN  !  ,
webhookSecretToken: process.env.TELEGRAM_WEBHOOK_SECRET  !  ,
botUsername:   "my_bot"  ,
})
.  command  (
defineCommand  (  "ping"  , {
description:   "Health check command"  ,
scopes: [  "private"  ,   "group"  ,   "supergroup"  ],
handler  :   async   ({ api, chat })   =>   {
await   api.  sendMessage  ({ chat_id: chat.id, text:   "pong"   });
},
}),
)
.  command  (
defineCommand  (  "status"  , {
description:   "Show service status"  ,
scopes: [  "private"  ],
handler  :   async   ({ api, chat })   =>   {
await   api.  sendMessage  ({ chat_id: chat.id, text:   "All systems operational."   });
},
}),
)
.  build  ();


export   const   telegramFragment   =   createTelegramFragment  (telegramConfig, {
databaseAdapter: adapter,
mountRoute:   "/api/telegram"  ,
});
```


Fig. Production Fragno fragments in the wild: Pi agent sessions, Forms on Durable Objects, and Telegram bots.


For users, integration should be as frictionless as possible. For a library that spans the backend and frontend, the following is enough:


```text
import   type   { Route }   from   "./+types/example-fragment"  ;
import   { createExampleFragmentInstance }   from   "@/lib/example-fragment-server"  ;


export   async   function   loader  ({ request }  :   Route  .  LoaderArgs  ) {
return   await   createExampleFragmentInstance  ().  handler  (request);
}


export   async   function   action  ({ request }  :   Route  .  ActionArgs  ) {
return   await   createExampleFragmentInstance  ().  handler  (request);
}
```


Fig. Mounting a fragment, the combination of loader and action makes sure all HTTP verbs are covered.


Fragments that use the optional database layer require slightly more boilerplate. By default, Fragno will use a local SQLite file to store the data. However, you can use any SQL database by providing a **database adapter** .


Users decide how they want to integrate. They can use Fragno directly to migrate their database, or use the Fragno CLI to generate a schema in their preferred ORM.


## Framework support


Mount the same fragment across modern full-stack frameworks with shared server and client contracts.


- React
- Vue
- Svelte
- SolidJS
- Astro
- Next.js
- Nuxt
- Node.js
- [More frameworks](https://fragno.dev/docs/fragno/reference/frameworks)


## Authoring libraries on top of Fragno


Fragno takes inspiration from industry-leading frameworks such as Hono to provide features typically expected from a backend router framework.


### End-to-end type safety


Keep input, output, client hooks, and database schema typed from route handlers to UI usage.


### Frontend state management


Compose reactive stores and invalidation behavior as part of your library so users get ergonomic state. Based on Nano Stores.


### Streaming support


Model long-running and incremental responses with NDJSON streams while preserving typed client-side consumption.


### Middleware support


Let integrators add auth and cross-cutting behavior while preserving your fragment contract and runtime semantics.


## What does the data layer look like?


Letting third-party libraries write to an app’s database is a **delicate thing** . Fragno’s data layer is very opinionated. This makes it slightly more complicated for authors but gives users the safety they need.


Things that are **not supported** :


- **Interactive transactions** — Long-lived, interactive transactions can hold locks unpredictably in user-owned environments, so Fragno has two-phased transactions with optimistic concurrency control instead.
- **Arbitrary joins** — Arbitrary joins make query performance characteristics unpredictable, so Fragno only supports simple left joins.


In some way, Fragno is the most opinionated ORM. This is necessary to provide safety, consistency, and compatibility with several ORMs and databases. These are some features that Fragno *does* support:


- **Atomicity** — Reads and writes run as one retryable unit using optimistic concurrency checks instead of lock-heavy interactive transactions.
- **Durable Hooks** — Side effects are persisted in-transaction and dispatched after commit with retries and scheduled execution support. This makes interactions with third-party services and webhook ingestion from external systems reliable.
- **Cursor-based pagination** — List endpoints page through stable index cursors, keeping large datasets efficient without offset drift. This works great with client-side state management. Pages are kept in memory to make pagination feel seamless.
- **Testing with a real database** — Fragment tests run against real adapters so schema behavior, migrations, and hooks are validated end-to-end.


Two-phase optimistic concurrency control


Instead of interactive transactions that hold locks, Fragno uses a retrieve → mutate pattern. Services read first, then schedule mutations. The .check() call pins version numbers — on conflict the entire transaction retries automatically with exponential backoff.


```text
markFileDeleted  ({ provider, fileKey }: FileByKeyInput) {
return   this.  serviceTx  (uploadSchema)
.  retrieve  ((uow)   =>
uow.  findFirst  (  "file"  , (b)   =>
b.  whereIndex  (  "idx_file_provider_key"  , (eb)   =>
eb.  and  (
eb  (  "provider"  ,   "="  , provider),
eb  (  "key"  ,   "="  , fileKey),
)),
),
)
.  mutate  (({ uow, retrieveResult: [file] })   =>   {
if   (  !  file)   throw   new   Error  (  "FILE_NOT_FOUND"  );
if   (file.status   ===   "deleted"  )   return   file;


// .check() pins the row version — retries on conflict
uow.  update  (  "file"  , file.id, (b)   =>
b.  set  ({ status:   "deleted"  , updatedAt: uow.  now  (), deletedAt: uow.  now  () }).  check  (),
);
// Persisted in the same transaction. See Durable Hooks tab
uow.  triggerHook  (  "onFileDeleted"  , {   ...  buildFileHookPayload  (file) });


return   {   ...  file, status:   "deleted"   };
})
.  build  ();
}
```


```text
handler:   async   function   ({ input }, { json }) {
const   data   =   await   input.  valid  ();
await   this.  handlerTx  ()
.  withServiceCalls  (()   =>   [
services.  markFileDeleted  ({ provider: data.provider, fileKey: data.key }),
])
.  execute  ();
return   json  ({ ok:   true   });
}
```


Transactional side effects with at-least-once delivery


Hook triggers are written to the database in the same transaction as your mutations. After commit, a background dispatcher executes them with retry and backoff. Hooks can schedule future execution with processAt, and can themselves run full OCC transactions via handlerTx.


```text
.  provideHooks  <  OtpHooksMap  >(({ defineHook, config })   =>   ({
onOtpIssued:   defineHook  (  async   function   (payload) {
await   config.hooks?.  onOtpIssued  ?.(payload, this.idempotencyKey);
}),
expireOtp:   defineHook  (  async   function   ({ otpId }) {
// Hooks can run their own OCC transactions
await   this.  handlerTx  ()
.  retrieve  (({ forSchema })   =>
forSchema  (otpSchema).  findFirst  (  "otp"  , (b)   =>
b.  whereIndex  (  "idx_otp_id_status_expiresAt"  , (eb)   =>
eb.  and  (
eb  (  "id"  ,   "="  , otpId),   eb  (  "status"  ,   "="  ,   "pending"  ),
eb  (  "expiresAt"  ,   "<="  , eb.  now  ()),
)),
),
)
.  mutate  (({ forSchema, retrieveResult: [otp] })   =>   {
if   (  !  otp)   return  ;
const   uow   =   forSchema  (otpSchema);
uow.  update  (  "otp"  , otp.id, (b)   =>
b.  set  ({ status:   "expired"  , expiredAt: uow.  now  () }).  check  (),
);
uow.  triggerHook  (  "onOtpExpired"  , {   ...  otp, expiredAt: uow.  now  () });
})
.  execute  ();
}),
}))
```


```text
.  mutate  (({ uow })   =>   {
const   expiresAt   =   uow.  now  ().  plus  ({ minutes: expiryMinutes });
const   otpId   =   uow.  create  (  "otp"  , {
externalId, type, code, status:   "pending"  , expiresAt,
});


// Fires after the transaction commits
uow.  triggerHook  (  "onOtpIssued"  , { id: otpId.  valueOf  (), code, expiresAt });


// Scheduled: runs when the OTP expires
uow.  triggerHook  (  "expireOtp"  , { otpId: otpId.  valueOf  () }, { processAt: expiresAt });


return   { id: otpId.  valueOf  (), code };
})
```


Versioned, dialect-agnostic table definitions


Fragment authors declare their data model in a single fluent builder. Schemas are versioned so migrations are deterministic across environments.


```text
import   { column, idColumn, referenceColumn, schema }   from   "@fragno-dev/db/schema"  ;


export   const   commentSchema   =   schema  (  "comment"  , (s)   =>   {
return   s
.  addTable  (  "comment"  , (t)   =>   {
return   t
.  addColumn  (  "id"  ,   idColumn  ())
.  addColumn  (  "title"  ,   column  (  "string"  ))
.  addColumn  (  "content"  ,   column  (  "string"  ))
.  addColumn  (  "createdAt"  ,   column  (  "timestamp"  ).  defaultTo  ((b)   =>   b.  now  ()))
.  addColumn  (  "postReference"  ,   column  (  "string"  ))
.  addColumn  (  "parentId"  ,   referenceColumn  ({ table:   "comment"   }).  nullable  ())
.  createIndex  (  "idx_comment_post"  , [  "postReference"  ]);
})
.  addTable  (  "upvote_total"  , (t)   =>   {
return   t
.  addColumn  (  "id"  ,   idColumn  ())
.  addColumn  (  "reference"  ,   column  (  "string"  ))
.  addColumn  (  "total"  ,   column  (  "integer"  ).  defaultTo  (  0  ))
.  createIndex  (  "idx_upvote_total_reference"  , [  "reference"  ], { unique:   true   });
});
});
```


Fig. Fragno's data layer: schema definitions, OCC transactions, and durable hooks for reliable side effects.


### Database & ORM support


Fragments that use the data layer work with the ORMs and databases your app already relies on.


K


Kysely


D


Drizzle


P


Prisma


PG


PostgreSQL


S


SQLite


MY


MySQL


Fig. Database portability: fragments declare a schema once; the user picks which ORM and engine runs it.


## Why all of this matters


Before Fragno, libraries did the bare minimum.


Now, developers of platforms such as Stripe, Telegram, and Resend can build opinionated, tasteful **integration libraries** that are not just wrappers around the API. The developers that know the platform best can ship libraries that **own the entire integration surface** : webhook ingestion, state persistence, and surfacing information to the end user.


## Start building using agent skills


Fragno ships Agent Skills that teach your AI coding assistant how to integrate or author fragments. Install a skill once and your agent knows the conventions, APIs, and best practices.


For users


Integrate existing fragments into your app. The skill includes a list of first-party fragments to make installing them easy.


```text
npx   skills   add   https://github.com/rejot-dev/fragno   --skill   fragno
```


For authors


Build your own fragments. The skill teaches your agent how to scaffold a package, define routes and hooks, set up code-splitting, and export framework clients.


```text
npx   skills   add   https://github.com/rejot-dev/fragno   --skill   fragno-author
```


## Our Fragno Claw-like agent


The Fragno website and our internal Claw-like agent tooling run on the same fragment primitives outlined in this article. We believe that the only way to build truly good software is by dogfooding it every day.


Stay tuned for more updates on our “Claw”.


Fig. First-party fragments (pictured: Resend) in our own Claw backoffice.


## Further reading


- [Fragno documentation for library authors](https://fragno.dev/docs/fragno/for-library-authors/getting-started)
- [Fragno user quick start](https://fragno.dev/docs/fragno/user-quick-start)
- [The Resend fragment essay](https://fragno.dev/fragments/resend/essay)
- [Fragno on GitHub](https://github.com/rejot-dev/fragno)
- [Join the Fragno Discord](https://discord.gg/jdXZxyGCnC)
