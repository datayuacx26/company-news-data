---
schema_version: "1.0.0"
document_id: "e771b5536e414bc2fa8894bd6c03726dda914a10f2695c9d53763bb838639480"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/build-rest-api-fastify/"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-25T18:02:25.733496+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:43f8d5fd7955b12641fb9c5f10f3cead124ded7b63a79a7cabea421f5d02010a"
---

# How to Build a REST API with Fastify

Fastify is a high-performance, low-overhead Node.js web framework whose defining feature is JSON Schema-driven validation and serialization: you declare the shape of each request and response, and Fastify compiles those schemas into fast functions at startup. As far as the maintainers know, Fastify is[one of the fastest web frameworks in town](https://www.npmjs.com/package/fastify) , depending on the code complexity, serving up to 76+ thousand requests per second. This tutorial builds a complete CRUD REST API for a` posts` resource on **Fastify v5** with **Prisma** as the data layer, and it focuses on the four primitives that separate Fastify from Express — encapsulated plugins, decorators, schema-based validation/serialization, and lifecycle hooks.


## Key Takeaways


- Fastify v5 requires Node.js v20 or newer; v4 reached end-of-life on June 30, 2025, and v3 and earlier are unmaintained, so new APIs should start on the v5.x line.
- A Fastify` response` schema does double duty — it makes serialization faster by compiling a dedicated stringifier, and it strips any property not declared in the schema, so internal fields never leak into responses.
- As of v5 the schema shorthand is gone: every` querystring` ,` params` ,` body` , and` response` schema must be a full JSON Schema that includes a` type` property.
- Share a database client across routes by wrapping the plugin in` fastify-plugin` to deliberately break encapsulation, then attaching the client with` fastify.decorate('prisma', client)` .
- With the TypeBox type provider you declare each schema once and Fastify infers request and response types from it, eliminating drift between validation rules and TypeScript types.


## Why Fastify: the four primitives Express doesn’t have


You can write the same CRUD endpoints in Express. What you can’t easily reproduce is Fastify’s model, which is built on four primitives: encapsulated plugins, decorators, schema-based validation and serialization, and lifecycle hooks. The maintainers note that Fastify is[fully extensible via its hooks, plugins, and decorators](https://www.npmjs.com/package/fastify) . These are not conveniences bolted onto a router; they’re the architecture.


Concern Express (typical) Fastify (idiomatic)


Input validation Manual or middleware (` express-validator` ) JSON Schema declared on the route


Response serialization` JSON.stringify` Compiled` fast-json-stringify` from a response schema


Sharing resources Global middleware on one shared` req` Encapsulated plugins + decorators


Request lifecycle Linear middleware chain Typed hooks (` onRequest` ,` preHandler` ,` onError` , …)


The performance claim is mechanical, not marketing. Even though it is not mandatory, Fastify recommends using JSON Schema to validate routes and serialize outputs; internally Fastify compiles the schema into a highly performant function. A response schema is turned into a specialized serializer at boot, so producing JSON skips the generic` JSON.stringify` path. Treat the official figures as what they are: these benchmarks are taken using a synthetic “hello world” benchmark that aims to evaluate the framework overhead — benchmark your own app before quoting numbers.


## Project setup and a minimal v5 server


Start with ESM, because both Fastify and Prisma v7 are ESM-first.[Fastify v5 will only support Node.js v20+](https://fastify.dev/docs/latest/Guides/Migration-Guide-V5/) ; if you are using an older version of Node.js, you will need to upgrade to a newer version to use Fastify v5.


```text
mkdir   fastify-posts-api   &&   cd   fastify-posts-api
npm   init   -y
npm   pkg   set   type=  "  module  "
npm   i   fastify@5
```


The current stable release is[5.9.0, published June 28, 2026](https://github.com/fastify/fastify/releases) . Create` server.js` :


```text
import   Fastify   from   '  fastify  '


const   app   =   Fastify  ({   logger  :   true   })


app  .  get  (  '  /health  '  ,   async   ()   =>   ({   status  :   '  ok  '   }))


try   {
await   app  .  listen  ({   port  :   3000   })
}   catch   (  err  ) {
app  .  log  .  error  (  err  )
process  .  exit  (  1  )
}
```


Note the object-form listen call. The variadic argument signature of the` .listen()` method has been removed in v5, so you can no longer call` .listen()` with a variable number of arguments — always pass an options object like` { port: 3000 }` . Run it with` node --watch server.js` and hit` http://localhost:3000/health` .


## Routing the Fastify way: plugins and encapsulation


In Fastify, routes live inside plugins, and every plugin runs in its own encapsulation context. A decorator, hook, or route registered inside a plugin is visible to that plugin and its children, but invisible to its siblings — the opposite of Express middleware, where everything shares one global request object. The[Plugins reference](https://fastify.dev/docs/latest/Reference/Plugins/) documents this scoping model.


A plugin is just an async function that receives the encapsulated instance:


```text
// routes/posts.js
export default async   function   postsRoutes  (  app  ,   opts  )   {
app  .  get  (  '  /  '  ,   async   (  request  ,   reply  )   =>   {
return   []   // list handler comes later
})
}
```


Register it under a prefix in` server.js` :


```text
import   postsRoutes   from   '  ./routes/posts.js  '


app  .  register  (  postsRoutes  , {   prefix  :   '  /api/posts  '   })
```


Keep plugin functions consistently async. The[v5 migration guide](https://fastify.dev/docs/latest/Guides/Migration-Guide-V5/) is explicit that all v4 deprecations have been removed and will no longer work after upgrading, and mixing the callback and promise styles inside one plugin — tolerated in v4 — is no longer allowed. Pick` async` /` await` and stay there.


## Schema validation and serialization


Attach a` schema` object to any route and Fastify validates the incoming` body` ,` params` ,` querystring` , and` headers` , then serializes the outgoing payload against the` response` schema. This is the framework’s signature feature, documented under[Validation and Serialization](https://fastify.dev/docs/latest/Reference/Validation-and-Serialization/) .


One v5 rule governs every schema you write: the shorthand is gone. As of v5 you must supply a full JSON Schema for` querystring` ,` params` ,` body` , and` response` , each including the type property, or the route won’t validate — the[v5 migration guide](https://fastify.dev/docs/latest/Guides/Migration-Guide-V5/) records the removal of the` jsonShortHand` option.


Define the schemas for the` posts` resource:


```text
// schemas/posts.js
export   const   postResponse   =   {
type  :   '  object  '  ,
properties  :   {
id  :   {   type  :   '  string  '   },
title  :   {   type  :   '  string  '   },
content  :   {   type  :   '  string  '   },
published  :   {   type  :   '  boolean  '   },
createdAt  :   {   type  :   '  string  '   }
}
}


export   const   createPostBody   =   {
type  :   '  object  '  ,
required  :   [  '  title  '  ,   '  content  '  ],
properties  :   {
title  :   {   type  :   '  string  '  ,   minLength  :   1   },
content  :   {   type  :   '  string  '  ,   minLength  :   1   },
published  :   {   type  :   '  boolean  '  ,   default  :   false   }
}
}


export   const   idParams   =   {
type  :   '  object  '  ,
required  :   [  '  id  '  ],
properties  :   {   id  :   {   type  :   '  string  '   }   }
}
```


The` response` schema is the part most tutorials skip, and it earns its place twice over. It compiles to a dedicated serializer that’s faster than generic stringification, **and** it acts as an allowlist: any property your handler returns that isn’t declared in the schema is dropped before it reaches the client. A` passwordHash` or internal` userId` that sneaks into a query result never leaves the server, because the serializer simply doesn’t know how to emit it.


### The TypeScript path with TypeBox


If you’re in TypeScript, the friction is keeping validation schemas and type definitions in sync. The[TypeBox type provider](https://github.com/fastify/fastify-type-provider-typebox) collapses them into one declaration. Install` typebox` as the required peer dependency and` @fastify/type-provider-typebox` :


```text
npm   i   typebox   @fastify/type-provider-typebox
```


Register the provider and declare each schema once with` Type` :


```text
import   Fastify   from   '  fastify  '
import   {   Type  ,   TypeBoxTypeProvider   }   from   '  @fastify/type-provider-typebox  '


const   app   =   Fastify  ().  withTypeProvider  <  TypeBoxTypeProvider  >()


app  .  post  (  '  /api/posts  '  , {
schema  :   {
body  :   Type  .  Object  ({
title  :   Type  .  String  ({   minLength  :   1   }),
content  :   Type  .  String  ({   minLength  :   1   })
})
}
},   async   (  request  )   =>   {
// request.body is typed { title: string; content: string }
const   {   title  ,   content   }   =   request  .  body
return   {   title  ,   content   }
})
```


You import` Type` and` TypeBoxTypeProvider` from` @fastify/type-provider-typebox` and call` Fastify().withTypeProvider<TypeBoxTypeProvider>()` ; the body’s field types are then automatically inferred. Use the scoped package — the legacy` @sinclair/typebox` import shown in older guides is the pre-v5 name. One v5 subtlety to know: the migration guide notes that the type providers have been split into two separate types: ValidatorSchema and SerializerSchema, so upgrade the provider package alongside Fastify. Note also that[the provider types don’t propagate globally](https://fastify.dev/docs/latest/Reference/Type-Providers/) ; in encapsulated usage, one can remap the context to use one or more providers, which means you re-declare the provider in each plugin that needs it.


## The database layer as a decorator plugin


Wire the database onto the Fastify instance with a decorator, exposed through a plugin that deliberately breaks encapsulation. To share one resource — a database client — across every route, wrap the plugin in[fastify-plugin](https://github.com/fastify/fastify-plugin) so the decoration escapes its own scope, then attach it with` fastify.decorate('prisma', client)` . The[Decorators reference](https://fastify.dev/docs/latest/Reference/Decorators/) covers the pattern.


This tutorial uses[Prisma](https://www.prisma.io/docs) (v7, currently 7.8.x), which is Rust-free and ESM-only — a clean fit for the ESM setup above. Install it, define the schema, and generate the client. Prisma 7 also requires a driver adapter to create the client, so install the SQLite adapter alongside the core packages:


```text
npm   i   @prisma/client@7   @prisma/adapter-better-sqlite3
npm   i   -D   prisma@7
```


```text
// prisma/schema.prisma
generator   client   {
provider   =   "prisma-client"
output     =   "../generated/prisma"
}


datasource   db   {
provider   =   "sqlite"
url        =   "file:./dev.db"
}


model   Post   {
id          String     @id   @default  (  uuid  ())
title       String
content     String
published   Boolean    @default  (  false  )
createdAt   DateTime   @default  (  now  ())
}
```


Prisma 7 configures its CLI through a` prisma.config.ts` file and generates the client to the` output` path you set above; run` npx prisma migrate dev` to create the database and client. Then expose it as a decorator. Note that in Prisma 7 the bare` new PrismaClient()` call has been removed — you must construct the client with a driver adapter:


```text
// plugins/prisma.js
import   fp   from   '  fastify-plugin  '
import   {   PrismaBetterSqlite3   }   from   '  @prisma/adapter-better-sqlite3  '
import   {   PrismaClient   }   from   '  ../generated/prisma/client.js  '


export default   fp  (  async   (  app  )   =>   {
const   adapter   =   new   PrismaBetterSqlite3  ({   url  :   '  file:./dev.db  '   })
const   prisma   =   new   PrismaClient  ({   adapter   })
await   prisma  .  $connect  ()


app  .  decorate  (  '  prisma  '  ,   prisma  )
app  .  addHook  (  '  onClose  '  ,   async   (  instance  )   =>   {
await   instance  .  prisma  .  $disconnect  ()
})
})
```


Because the plugin is wrapped in` fp` ,` app.prisma` is now reachable from every route in the application.


Prefer Postgres with TypeORM? The decorator pattern is identical: connect the` DataSource` , then` app.decorate('db', dataSource)` . Only the schema layer changes.


## CRUD handlers with correct status codes


Implement the five operations as routes inside the posts plugin, each carrying its schema. Return the right status code for each verb:


- ` 201` with the created resource on POST;
- ` 200` on reads;
- ` 204` with no body on DELETE; and
- ` 404` when a lookup misses.


Fastify sets` Content-Type` and serializes against your response schema automatically — see the[Reply API](https://fastify.dev/docs/latest/Reference/Reply/) .


```text
// routes/posts.js
import   {   postResponse  ,   createPostBody  ,   idParams   }   from   '  ../schemas/posts.js  '


export default async   function   postsRoutes  (  app  )   {
// CREATE
app  .  post  (  '  /  '  ,   {
schema  :   {   body  :   createPostBody  ,   response  :   {   201  :   postResponse   }   }
},   async   (  request  ,   reply  )   =>   {
const   post   =   await   app  .  prisma  .  post  .  create  ({   data  :   request  .  body   })
return   reply  .  code  (  201  ).  send  (  post  )
})


// READ all
app  .  get  (  '  /  '  ,   {
schema  :   {   response  :   {   200  :   {   type  :   '  array  '  ,   items  :   postResponse   }   }   }
},   async   ()   =>   {
return   app  .  prisma  .  post  .  findMany  ({   orderBy  :   {   createdAt  :   '  desc  '   }   })
})


// READ one
app  .  get  (  '  /:id  '  ,   {
schema  :   {   params  :   idParams  ,   response  :   {   200  :   postResponse   }   }
},   async   (  request  ,   reply  )   =>   {
const   post   =   await   app  .  prisma  .  post  .  findUnique  ({   where  :   {   id  :   request  .  params  .  id   }   })
if   (  !  post  )   return   reply  .  code  (  404  ).  send  ({   message  :   '  Post not found  '   })
return   post
})


// UPDATE
app  .  put  (  '  /:id  '  ,   {
schema  :   {   params  :   idParams  ,   body  :   createPostBody  ,   response  :   {   200  :   postResponse   }   }
},   async   (  request  ,   reply  )   =>   {
const   exists   =   await   app  .  prisma  .  post  .  findUnique  ({   where  :   {   id  :   request  .  params  .  id   }   })
if   (  !  exists  )   return   reply  .  code  (  404  ).  send  ({   message  :   '  Post not found  '   })
return   app  .  prisma  .  post  .  update  ({   where  :   {   id  :   request  .  params  .  id   },   data  :   request  .  body   })
})


// DELETE
app  .  delete  (  '  /:id  '  ,   {
schema  :   {   params  :   idParams   }
},   async   (  request  ,   reply  )   =>   {
await   app  .  prisma  .  post  .  delete  ({   where  :   {   id  :   request  .  params  .  id   }   })
return   reply  .  code  (  204  ).  send  ()
})
}
```


One v5 gotcha worth flagging on DELETE: in v4, Fastify allowed DELETE requests with a Content-Type: application/json header and an empty body;[this is no longer allowed in v5](https://fastify.dev/docs/latest/Guides/Migration-Guide-V5/) . Send no` Content-Type` header when there’s no payload.


## Error handling, hooks, and production hardening


Centralize error handling with` setErrorHandler` and intercept the request lifecycle with hooks — the two extension points that replace Express’s middleware chain. The[Hooks reference](https://fastify.dev/docs/latest/Reference/Hooks/) lists the request flow in order:` onRequest` →` preParsing` →` preValidation` →` preHandler` → handler →` preSerialization` →` onSend` →` onResponse` , with` onError` firing when something throws.


```text
class   NotFoundError   extends   Error   {
constructor   (  message  ) {   super  (  message  );   this  .  statusCode   =   404   }
}


app  .  setErrorHandler  ((  error  ,   request  ,   reply  )   =>   {
request  .  log  .  error  (  error  )
const   status   =   error  .  statusCode   ??   500
reply  .  code  (  status  ).  send  ({
error  :   error  .  name  ,
message  :   status   ===   500   ?   '  Internal Server Error  '   :   error  .  message
})
})


// preHandler hook example: gate a route before the handler runs
app  .  addHook  (  '  preHandler  '  ,   async   (  request  )   =>   {
request  .  log  .  info  ({   url  :   request  .  url   },   '  incoming request  '  )
})
```


A validation failure short-circuits before your handler ever runs, returning a` 400` automatically — one reason schemas reduce handler code. For timing inside a hook, use` reply.elapsedTime` ;[the reply.getResponseTime() method has been removed in v5](https://github.com/fastify/fastify/blob/main/docs/Guides/Migration-Guide-V5.md) , and you should use` reply.elapsedTime` instead.


From here, the production checklist is plugin installation, each a scoped` @fastify/*` package that registers the same way as your routes plugin:


- [@fastify/jwt](https://github.com/fastify/fastify-jwt) for authentication;
- [@fastify/swagger](https://github.com/fastify/fastify-swagger) to generate OpenAPI docs directly from your route schemas;
- [@fastify/rate-limit](https://github.com/fastify/fastify-rate-limit) for throttling; and
- [@fastify/cors](https://github.com/fastify/fastify-cors) for cross-origin access.


For deeper auth flows and deployment, link out to dedicated guides — those are separate builds. If you want to see how these endpoints behave under real traffic, pair the API with[Node.js error monitoring](https://blog.openreplay.com/) on the front end consuming it.


You now have a runnable Fastify v5 CRUD API where schemas validate input, serialize output, and document themselves, and where the database is shared through a decorator instead of a global import. The next concrete step: add` @fastify/swagger` , point it at the schemas you already wrote, and watch the OpenAPI spec generate itself — proof that in Fastify the schema is the source of truth, not an afterthought.


## FAQs


What is the difference between fastify.register and fastify.decorate?


The register method mounts a plugin in its own encapsulation context, so routes, hooks, and decorators added inside it stay scoped to that plugin and its children. The decorate method attaches a reusable property or method directly onto the Fastify instance, request, or reply. You typically use decorate to expose shared resources like a database client, and wrap that plugin in fastify-plugin so the decoration escapes encapsulation and becomes available application-wide.


Is Fastify faster than Express, and by how much?


Fastify's own benchmark reports serving up to 76 thousand or more requests per second, but the maintainers caveat that this is a synthetic 'hello world' test measuring framework overhead, not real-world throughput. The mechanical advantage is concrete: Fastify compiles JSON Schemas into specialized validation and serialization functions at startup, so producing a response skips generic JSON.stringify overhead. Always benchmark your own application before quoting any speed multiplier.


Why does my Fastify v5 route schema fail validation when it worked in v4?


As of v5 the schema shorthand and the jsonShortHand option have been removed, so every querystring, params, body, and response schema must be a full JSON Schema that explicitly includes a type property. A schema that only declares properties without a top-level type will no longer validate. Add type 'object' to each schema object. The v5 migration guide documents this as a deliberate breaking change from the v4 shorthand behavior.


Can I use Fastify with TypeScript without writing types twice?


Yes. Install typebox as the peer dependency plus the scoped @fastify/type-provider-typebox package, then call Fastify().withTypeProvider with the TypeBoxTypeProvider generic. You declare each schema once using Type, and Fastify infers the request and response types from it, eliminating drift between validation rules and TypeScript interfaces. Import Type from @fastify/type-provider-typebox, not the legacy @sinclair/typebox name shown in older guides, and re-declare the provider in each plugin that needs it.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
