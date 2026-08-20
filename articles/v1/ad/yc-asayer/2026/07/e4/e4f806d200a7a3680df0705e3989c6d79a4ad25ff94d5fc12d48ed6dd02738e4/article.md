---
schema_version: "1.0.0"
document_id: "e4f806d200a7a3680df0705e3989c6d79a4ad25ff94d5fc12d48ed6dd02738e4"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/tanstack-ecosystem-guide/"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-25T17:38:53.548823+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:cf12d2bfc68e9d107ca547ab8d390e7539b84dbcd9c2fe134479a4aeffdf6e93"
---

# A Field Guide to the TanStack Ecosystem

TanStack is a collection of headless, type-safe, composable libraries that handle data fetching, routing, tables, forms, virtualization, and client state. The ecosystem grew out of[React Query, since renamed TanStack Query](https://tanstack.com/query/latest) . Its data and UI libraries ship adapters for[React, Vue, Solid, Svelte, and Angular](https://tanstack.com/) , while its router and full-stack framework target React and Solid. TanStack is not itself a framework; the project describes its pieces as headless, composable utilities, and only one member — TanStack Start — is a full-stack framework.


If you already use TanStack Query and keep seeing Router, Start, Table, Form, Store, DB, and AI mentioned, the practical problem is figuring out what each one does, how mature it is, and when to reach for it instead of the React-default stack (Next.js, React Router, Redux/Zustand, React Hook Form, Apollo, AG Grid). This guide maps the ecosystem library by library, gives each a minimal code signature, marks each one’s maturity with a version number, and shows how the pieces connect. It is a map, not a tutorial — setup steps live in the official docs, which are linked throughout.


## Key Takeaways


- TanStack is a family of headless, type-safe libraries — not a framework — with one full-stack framework member, TanStack Start.
- As of June 2026, Query, Router, Table, Form, and Virtual are stable; Start is at the Release Candidate stage; DB and AI are beta; Store is alpha — maturity varies widely across the ecosystem.
- Every TanStack library separates logic from rendering: the library owns the state machine, your component owns the markup, so there is no TanStack CSS to override or component to extend.
- Use TanStack Start for public, SEO-facing pages and TanStack Router alone for authenticated dashboards where SEO is irrelevant.
- TanStack’s headless model is the wrong fit when your team needs pre-styled components, when your hiring pool knows Redux and React Router but not TanStack, or when you need stable React Server Components in production today.


## What Is TanStack, and Why “Headless” Matters


TanStack is a set of independent libraries that share one design rule: the library owns the logic, and you own the rendering. Every TanStack library separates state from markup — the library runs the state machine; your component decides what the interface looks like. This is what “headless” means in practice: there is no TanStack CSS to override, no TanStack component to extend, and no vendor opinion about how your UI should appear. TanStack Query is the origin point of the brand — it was renamed from React Query when the ecosystem grew — though TanStack Table predates the rebrand, so Query is the *popularizing* origin, not literally the first library.


The shared philosophy is often summarized as “own your code, not your framework.” Three properties follow from it:


- **Headless** : logic without UI. You wire the library’s state to whatever markup, design system, or accessibility layer you already use.
- **Framework-agnostic core** : the data and UI libraries —[Query](https://tanstack.com/query/latest) ,[Table](https://tanstack.com/table/latest) ,[Form](https://tanstack.com/form/latest) ,[Virtual](https://tanstack.com/virtual/latest) , and[Store](https://tanstack.com/store/latest) — ship official adapters for React, Vue, Solid, Svelte, and Angular. Routing is the exception:[TanStack Router](https://tanstack.com/router/latest) and[TanStack Start](https://tanstack.com/start/latest) target React and Solid only.
- **Composability over monoliths** : each library works alone or together. You adopt Query without adopting Router, or Table without adopting Start.


## The Library-by-Library Map


Each library below covers what it does, what it replaces, its maturity as of June 2026, and a minimal code signature. The signatures are the API surface, not a setup tutorial — follow the linked docs to install and configure.


### TanStack Query — server state


[TanStack Query](https://tanstack.com/query/latest) manages asynchronous server state: fetching, caching, background refetching, deduplication, and optimistic updates. It replaces the manual` fetch` -plus-` useEffect` pattern most React apps started with. It is stable.


```text
// @tanstack/react-query v5.101.0
const   { data:   users  ,   isLoading  ,   error   }   =   useQuery  ({
queryKey  :   [  '  users  '  ],
queryFn  :   ()   =>   fetch  (  '  /api/users  '  ).  then  ((  res  )   =>   res  .  json  ()),
});
```


Query is the most widely adopted entry point to the ecosystem and the highest-ROI change to an existing React codebase, because it removes hand-rolled loading and error states without touching your routing or UI layers.


### TanStack Router — type-safe routing


[TanStack Router](https://tanstack.com/router/latest) is a fully type-safe client-side router for React and Solid. Route paths, URL parameters, search params, route context, and loader data are all typed end to end, so the compiler — not the user at runtime — catches a link with the wrong parameter shape. It replaces React Router for SPAs and is stable.


```text
// @tanstack/react-router v1.170.16
const   Route   =   createFileRoute  (  '  /products  '  )({
validateSearch  :   (  search  )   =>   ({
category  :   (  search  .  category   as   string  )   ??   '  all  '  ,
page  :   Number  (  search  .  page   ??   1  ),
}),
});


const   {   category  ,   page   }   =   Route  .  useSearch  ();
```


Router treats URL search params as typed, validated state, which makes filters, sort order, and pagination shareable via URL and restorable on refresh. This solves a specific, reproducible bug class: session replays of SPA dashboards routinely surface users losing filter, sort, and pagination state on refresh or back-navigation when that state lived in component memory rather than the URL — exactly the failure mode typed search params eliminate.


### TanStack Table — headless data grids


[TanStack Table](https://tanstack.com/table/latest) handles the logic of data grids — column definitions, sorting, filtering, pagination, row selection, grouping, and column visibility — while leaving every pixel of the UI to you. It established the headless pattern in the React ecosystem and replaces opinionated grid libraries like AG Grid and MUI DataGrid for teams that want full control of markup. It is stable, with a V9 line in development that is not yet the published` latest` .


```text
// @tanstack/react-table v8.21.3
const   table   =   useReactTable  ({
data  ,
columns  ,
getCoreRowModel  :   getCoreRowModel  (),
});
```


The Table instance manages state; your markup renders whatever that state describes. Combined with Query for server-side pagination and filtering, it handles large datasets without loading everything into the client.


### TanStack Form — typed forms


[TanStack Form](https://tanstack.com/form/latest) manages form state and validation — field-level and form-level errors, synchronous and asynchronous validation, submission state — without rendering anything itself. It is the headless successor to React Hook Form, and it is stable (v1).


```text
// @tanstack/react-form v1.33.0
const   form   =   useForm  ({
defaultValues  :   {   email  :   ''  ,   password  :   ''   },
onSubmit  :   async   ({   value   })   =>   {   await   loginUser  (  value  );   },
});
```


Field values, validation schemas, and submission payloads are typed end to end, and async validation (checking username availability, for example) is a first-class feature with debouncing and cancellation handled by the library.


### TanStack Virtual — list and grid virtualization


[TanStack Virtual](https://tanstack.com/virtual/latest) renders only the rows or columns currently in the viewport for large scrollable lists and grids, keeping the DOM small while the data set stays huge. It gives you the measurements and offsets; you render the markup. It replaces hand-rolled windowing and libraries like react-window and react-virtualized. It is stable.


```text
// @tanstack/react-virtual v3.14.3
const   virtualizer   =   useVirtualizer  ({
count  :   items  .  length  ,
getScrollElement  :   ()   =>   parentRef  .  current  ,
estimateSize  :   ()   =>   48  ,
});


const   rows   =   virtualizer  .  getVirtualItems  ();
```


` useVirtualizer` returns the virtual items in view plus the total size; you absolutely-position each item by its offset. It pairs naturally with Table for large data grids — Table owns the row model, Virtual keeps only visible rows in the DOM — so a five-thousand-row list scrolls at full speed.


### TanStack Store — client state


[TanStack Store](https://tanstack.com/store/latest) is a fine-grained reactive store that powers the internals of several other TanStack libraries and is exposed as a standalone package. It offers an alternative to Zustand and Redux for client (non-server) state. As a standalone package it is **alpha** (v0.11.0), so treat any “replaces Zustand/Redux” framing with that maturity in mind.


```text
// @tanstack/store v0.11.0 + @tanstack/react-store
import   {   Store   }   from   '  @tanstack/store  '  ;
import   {   useStore   }   from   '  @tanstack/react-store  '  ;


const   countStore   =   new   Store  (  0  );
// in a component:
const   count   =   useStore  (  countStore  );
```


The division of labor is the useful mental model: Query owns server state, Store owns client state. For most apps, Query plus a small amount of Store (or your existing client-state tool) covers the full state surface.


### TanStack Start — full-stack framework


[TanStack Start](https://tanstack.com/start/latest) is a full-stack React framework built on top of TanStack Router. It brings file-based routing, server functions, streaming SSR, and end-to-end TypeScript while keeping the underlying mechanics visible. It positions against Next.js and Remix. Start is at the **Release Candidate stage** (current release line 1.168.x), not yet declared stable, and its[React Server Components support is available experimentally](https://tanstack.com/start/latest/docs/framework/react/guide/server-components) rather than as a production default.


```text
// @tanstack/react-start v1.168.26
import   {   createServerFn   }   from   '  @tanstack/react-start  '  ;


const   getUser   =   createServerFn  ({   method  :   '  GET  '   })
.  validator  ((  userId  :   string  )   =>   userId  )
.  handler  (  async   ({ data:   userId   })   =>   db  .  users  .  findById  (  userId  ));
```


Server functions are typed, co-located functions that execute on the server but call from client components like any async function — the types flow from the data source to the component without a manual annotation, and there is no separate API layer to maintain for simple fetching. Note the import path is` @tanstack/react-start` ; older guides showing` @tanstack/start` are outdated.


### TanStack DB — reactive client store


[TanStack DB](https://tanstack.com/db/latest) is a reactive, client-first store with collections, live queries, and optimistic mutations. You define collections, write queries against them, and any component observing a query re-renders when the underlying data changes — from a server sync, a local mutation, or an optimistic update. It sits in front of your own backend or API; it is a client-side store/sync layer, **not** a Firebase or Supabase backend-as-a-service replacement. It is **beta** (v0.6.9).


DB addresses the gap past fetch-and-cache: data-intensive apps with real-time updates, complex client-side filtering, and multi-user state. The “headless Firebase” comparison is an analogy for the reactive query model, not a substitution claim about hosted infrastructure.


### TanStack AI — provider-agnostic AI primitives


[TanStack AI](https://tanstack.com/ai) provides headless primitives for AI-powered features — streaming response handling, conversation state, and a unified interface across model providers — without locking you into a single SDK or provider. It is **beta** , as[announced on the TanStack blog](https://tanstack.com/blog) , not alpha or experimental.


Streaming responses (tokens arriving incrementally) are handled natively, which is what makes AI chat and generation interfaces feel responsive. The value is architectural: a provider-agnostic layer keeps you free to switch model providers without rewriting your UI state.


## The Maturity Table


Maturity varies significantly across the ecosystem, and the single most common error in ecosystem overviews is presenting experimental libraries as settled. Five of the nine libraries covered here — Query, Router, Table, Form, and Virtual — are stable as of June 2026; Start is at the Release Candidate stage with React Server Components available experimentally; DB and AI are beta; and Store is alpha. The table below records each library’s published version and status.


Library What it does Replaces Maturity (June 2026) Version Docs


Query Server state / data fetching` fetch` +` useEffect` Stable 5.101.0[query](https://tanstack.com/query/latest)


Router Type-safe routing React Router Stable 1.170.16[router](https://tanstack.com/router/latest)


Table Headless data grids AG Grid, MUI DataGrid Stable 8.21.3[table](https://tanstack.com/table/latest)


Form Form state + validation React Hook Form Stable 1.33.0[form](https://tanstack.com/form/latest)


Virtual List/grid virtualization hand-rolled windowing Stable 3.14.3[virtual](https://tanstack.com/virtual/latest)


Store Client state Zustand, Redux Alpha 0.11.0[store](https://tanstack.com/store/latest)


Start Full-stack framework Next.js, Remix Release Candidate 1.168.x[start](https://tanstack.com/start/latest)


DB Reactive client store/sync client-side data layer Beta 0.6.9[db](https://tanstack.com/db/latest)


AI Provider-agnostic AI primitives per-provider AI SDKs Beta 0.x[ai](https://tanstack.com/ai)


*Check the linked docs for current release status.*


## How the Pieces Fit Together


The libraries are designed to compose, and the central integration is Router loaders pre-fetching Query data so a component renders against a warm cache. A route loader can call` queryClient.ensureQueryData` before the component mounts; by the time it renders, the data is already cached. When Start adds SSR to that pipeline, the data is server-preloaded and dehydrated, arriving as fully rendered HTML on the first request.


```text
// @tanstack/react-router v1.170.16 + @tanstack/react-query v5.101.0
const   Route   =   createFileRoute  (  '  /users/$userId  '  )({
loader  :   ({   params   })   =>
queryClient  .  ensureQueryData  ({
queryKey  :   [  '  user  '  ,   params  .  userId  ],
queryFn  :   ()   =>   fetchUser  (  params  .  userId  ),
}),
component  :   UserPage  ,
});
```


Three concrete connections are worth naming:


1. **Router → Query** : route loaders pre-fetch Query data, eliminating the loading flash on first render.
2. **Start → Router** : Start is built on Router, so the typed routing and loader model carries straight into the full-stack framework, with SSR and dehydration added.
3. **Table → Query** : Table consumes paginated, filtered data from Query for server-side pagination, so the grid handles large datasets without pulling everything client-side.


## Choosing TanStack vs the Incumbents


Choose TanStack Start for public-facing pages that need to be indexed and shared, and TanStack Router alone for authenticated dashboards where SEO is irrelevant and client-side performance is the priority. This rendering-strategy decision is the first architectural choice, and it precedes any library choice — a product decision, not a technical preference.


- **Public-facing sites** (marketing pages, blogs, content products) need SSR. Crawlers do not reliably execute JavaScript;[Core Web Vitals like LCP, CLS, and INP are Google ranking signals](https://developers.google.com/search/docs/appearance/core-web-vitals) ; and Open Graph metadata is read from the server response. Start delivers fully rendered HTML on the first request.
- **Authenticated apps** (dashboards, internal tools, customer portals) cannot be crawled anyway, are session-based and stateful, and prioritize time-to-interactive. A SPA on Router alone — without Start’s SSR machinery — is often leaner.


For the library-versus-incumbent question, the ecosystem maps cleanly onto the React defaults:


You currently use TanStack alternative Note


Next.js / Remix Start Start is RC; RSC is experimental


React Router Router Stable; full type safety


Redux / Zustand Store Alpha; for client state


Redux Toolkit Query / Apollo Query Query is API-agnostic; Apollo targets GraphQL


React Hook Form Form Stable v1


AG Grid / MUI DataGrid Table Headless; you build the UI


Firebase / Supabase client DB DB sits over *your* backend, not a BaaS


## When Not to Use TanStack


TanStack’s headless, type-safe model is a strong default but a real liability in three situations, and an honest map names them. The model is the wrong fit when your team needs pre-styled components out of the box, when your hiring pool knows Redux and React Router but not TanStack, or when you need stable React Server Components in production today — because RSC support in Start is still experimental, Next.js and React Hook Form remain the more pragmatic choices in those cases.


Specifically:


- **Pre-styled component needs** : headless means you build the UI. If you want a grid or form that looks finished on install, AG Grid, MUI, or a component kit will be faster.
- **Team familiarity and hiring** : more developers know Redux, React Router, and Next.js. The learning curve and the smaller hiring pool are real costs for team scaling.
- **Production RSC today** : Next.js has years of production battle-testing for React Server Components; Start’s RSC support is experimental as of June 2026.
- **Early-stage libraries** : Store (alpha), DB (beta), and AI (beta) are not yet production-default. Adopt them with that maturity in mind, and watch the version and status on the linked docs.


Beyond the libraries covered here, TanStack maintains a utility tier — including[Virtual](https://tanstack.com/virtual/latest) , Pacer, Ranger, and the DevTools — that follows the same headless, type-safe pattern at smaller scope.


## Where to Start


Adopt Query first if you haven’t — it is the highest-ROI change to a React codebase and commits you to nothing else. Reach for Router on a greenfield SPA rather than migrating an existing app, and treat Start as the choice for new public-facing sites that need SSR while watching its RC status. For the early-stage libraries — Store, DB, and AI — let the dated maturity table and the linked official docs, not ecosystem hype, decide whether a library is ready for the role you have in mind.


## FAQs


What is the difference between TanStack Query and TanStack DB?


TanStack Query manages asynchronous server state through fetching, caching, background refetching, and deduplication, treating the server as the source of truth that the client mirrors. TanStack DB is a reactive, client-first store with collections, live queries, and optimistic mutations that sits in front of your own backend for data-intensive apps with real-time updates and complex client-side filtering. Query is stable; DB is beta as of June 2026.


Can I use TanStack libraries with Vue, Svelte, or Angular instead of React?


Yes for the data and UI libraries. Query, Table, Form, Virtual, and Store ship official adapters for React, Vue, Solid, Svelte, and Angular. Routing is the exception: TanStack Router and the TanStack Start full-stack framework target React and Solid only. So a Vue or Angular team can adopt Query and Table but cannot use TanStack Router for type-safe routing.


Is TanStack Start production-ready as a Next.js replacement?


Not fully, as of June 2026. TanStack Start is at the Release Candidate stage on the 1.168.x release line, not yet declared stable, and its React Server Components support is available only experimentally rather than as a production default. If you need stable RSC in production today, Next.js has years of battle-testing for that feature. Start is a reasonable choice for new public-facing SSR sites where experimental RSC is acceptable.


Do I have to adopt the whole TanStack ecosystem to use one library?


No. Each TanStack library is independent and composable, so you can adopt Query without Router, or Table without Start. The libraries integrate when used together (route loaders can pre-fetch Query data, for example), but none require the others. Query is the most common entry point because it is the highest-ROI change to an existing React codebase and commits you to nothing else in the ecosystem.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
