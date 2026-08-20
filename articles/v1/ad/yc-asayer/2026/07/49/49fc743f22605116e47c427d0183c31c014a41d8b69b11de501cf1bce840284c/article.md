---
schema_version: "1.0.0"
document_id: "49fc743f22605116e47c427d0183c31c014a41d8b69b11de501cf1bce840284c"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/react-app-libraries/"
published_at: "2026-07-02T00:00:00+00:00"
first_seen_at: "2026-07-25T17:38:53.548823+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:865663df524cffc35dc8a393dc605bd19c099e14d641e687703951a2c83ff46d"
---

# The Libraries Behind Modern React Apps

A modern React app is mostly React plus a small, predictable set of libraries: one for server-state data fetching, one for client state, one for routing, one for styling, one for UI primitives, and one for forms. React itself — currently the[19.2 line](https://react.dev/blog) — handles components, hooks, and rendering. Everything else is a choice, and the ecosystem’s churn makes those choices feel harder than they are. The reality is that for each job there’s now a clear default and one or two situational alternatives.


This article maps that stack by job-to-be-done: foundation, state, data fetching, routing, styling, UI, forms, and animation. For each category you get the 2026 default, when to reach for an alternative, and how the choice interacts with two forces reshaping the ecosystem — React Server Components (RSC) and the React Compiler.


## Key Takeaways


- Modern React apps split state into four buckets — server state (TanStack Query), client UI state (useState or Zustand), URL state (nuqs), and global app state (Zustand) — and choosing the wrong tool for the bucket is the most common source of unnecessary complexity.
- [TanStack Query’s staleTime defaults to 0](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults) , so every component mount triggers a background refetch; setting a non-zero value for stable data is the highest-leverage configuration most apps skip.
- [React Compiler reached 1.0 on October 7, 2025](https://react.dev/blog/2025/10/07/react-compiler-1.0) , auto-memoizing components and removing most manual` useMemo` /` useCallback` from library integration code.
- Runtime CSS-in-JS conflicts with RSC; in a Next.js app,[Tailwind CSS v4](https://tailwindcss.com/blog/tailwindcss-v4) or CSS Modules are the compatible styling defaults.
- [shadcn/ui](https://ui.shadcn.com/) defaults to Radix primitives and added Base UI as an official alternative in early 2026; you own the component code outright rather than depending on a versioned package.


## React Project Foundations: Choosing Your Framework


Before any library choice, you pick the foundation your app builds on — and in 2026 that decision is shaped largely by whether you want React Server Components. The job-to-be-done here is project scaffolding, build tooling, routing, and rendering strategy bundled into a single starting point.


For most new apps,[Next.js](https://nextjs.org/) is the default: it’s the most complete full-stack React framework, with stable RSC, server actions, and file-based routing. Choose **[Vite](https://vite.dev/)** when you’re building a client-rendered single-page app and want a fast, unopinionated build without a full framework — you assemble routing and data fetching yourself. Choose **[Astro](https://astro.build/)** when the project is content-led — marketing pages, docs, blogs — and you want to ship mostly static HTML with React islands only where interactivity is needed. Choose **[TanStack Start](https://tanstack.com/start/latest)** when type-safe routing and server functions are the priority and you can adopt early: it’s at a v1 Release Candidate, approaching stable, with RSC support still in development.


The two forces worth weighing across all of these are[React Server Components](https://react.dev/reference/rsc/server-components) , which shift data fetching to the server, and the[React Compiler](https://react.dev/learn/react-compiler) , which auto-memoizes your components regardless of which foundation you pick.


## What the React Compiler Changes About Your Library Choices


The React Compiler is now stable and changes which optimizations you write by hand.[React Compiler 1.0 shipped on October 7, 2025](https://react.dev/blog/2025/10/07/react-compiler-1.0) , announced at React Conf, and[automatically memoizes components and hook values at build time](https://react.dev/learn/react-compiler) , removing most manual` useMemo` and` useCallback` calls. For library selection, this matters because a class of boilerplate that once cluttered integration code — wrapping selectors, memoizing callbacks passed to form fields, stabilizing derived values — is now handled by the compiler.


The practical effect on library choice: that auto-memoization narrows the performance argument for atomic state libraries and removes a category of boilerplate from form integrations, so you choose libraries on their data model and ergonomics, not on how much hand-tuning they demand to avoid re-render storms. The compiler is supported across Vite, Next.js, and Expo per the[React Compiler installation docs](https://react.dev/learn/react-compiler/installation) .


## React State Management Libraries: The Four-Bucket Taxonomy


Modern React apps have four distinct state buckets — server state managed by[TanStack Query](https://tanstack.com/query/latest) , client UI state handled by` useState` or[Zustand](https://zustand.docs.pmnd.rs/) , URL state owned by[nuqs](https://nuqs.dev/) , and global app state in Zustand — and choosing the wrong tool for the bucket is the most common source of unnecessary complexity. Most “state management is hard” pain comes from putting server data in a client store, or modeling shared UI state that belongs in the URL as global state.


State bucket What it holds 2026 default Choose instead when


Server state Remote data, caches, mutations TanStack Query You use GraphQL at scale → Apollo Client; you own both ends in TypeScript → tRPC


Client UI state Local component state, toggles, drafts` useState` /` useReducer` State is shared across distant components → Zustand


URL state Filters, tabs, pagination nuqs —


Global app state Cross-app session, theme, cart Zustand You have an existing Redux codebase → Redux Toolkit


Between TanStack Query for server state, built-in hooks for local state, and the URL for shared UI state, the surface needing a dedicated global store has shrunk. When you do need one,[Zustand](https://zustand.docs.pmnd.rs/getting-started/introduction) is the default — a minimal` create()` store with selector subscriptions and no provider:


```text
import   {   create   }   from   '  zustand  '


const   useCartStore   =   create  ((  set  )   =>   ({
items  :   [],
addItem  :   (  item  )   =>   set  ((  state  )   =>   ({   items  :   [  ...  state  .  items  ,   item  ]   })),
}))
```


Choose **Redux Toolkit** when you’re maintaining or extending an existing Redux codebase and want its DevTools time-travel and structured action tracking. Choose **Jotai** when you genuinely need fine-grained atomic subscriptions — though with the React Compiler auto-memoizing, the re-render argument for the atomic model is weaker than it was. For URL state specifically, nuqs gives you typed search params so filters and pagination survive refresh and share cleanly via link.


### Server state and the staleTime default


[TanStack Query’s staleTime defaults to 0](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults) , meaning every component mount marks data stale and triggers a background refetch; for stable data like navigation menus or user profiles, setting` staleTime` to a non-zero value is the single highest-leverage configuration change most apps never make. The important defaults documentation spells this out:


```text
import   {   useQuery   }   from   '  @tanstack/react-query  '


// staleTime defaults to 0 → refetches on every mount.
// Set a non-zero value for data that doesn't change per-view.
const   {   data   }   =   useQuery  ({
queryKey  :   [  '  profile  '  ],
queryFn  :   fetchProfile  ,
staleTime  :   5   *   60   *   1000  ,   // 5 minutes
})
```


A common production failure mode is leaving this default in place: session replays of data-heavy apps frequently surface waterfalls of background refetches firing on every route change — a pattern that’s hard to attribute to a single line of config until you watch the network sequence replay against a real user session.


## React Data Fetching and Routing Libraries


For data fetching, the default depends on where you fetch. For client-side fetching — REST or GraphQL caching, optimistic updates, background sync —[TanStack Query](https://tanstack.com/query/latest) is the default; in an RSC framework like Next.js 16, you fetch on the server inside Server Components and pass data down as props, avoiding a client-side fetching library entirely. Choose **Apollo Client** when you’re GraphQL-first and want normalized caching; choose **tRPC** when you control both client and server in TypeScript and want end-to-end type inference without a schema layer.


Routing follows the same server/client split. If you use a meta-framework, it handles routing. Outside one,[React Router](https://reactrouter.com/) is the most-used library; it ships both a classic client-side library mode and a full framework mode with loaders, actions, and server rendering. Choose[TanStack Router](https://tanstack.com/router/latest) when type-safe route inference is a priority — it’s stable at v1 with best-in-class TypeScript inference for params and search.


[TanStack Start](https://tanstack.com/start/latest) — which offers type-safe file-based routing, server functions, and SSR — is approaching stable as a v1 Release Candidate; per the[TanStack Start documentation](https://tanstack.com/start/latest/docs/framework/react/overview) , RSC support is still in development, which makes Next.js 16 the more complete full-stack choice for teams that need RSC today. Track it as the newcomer to watch rather than the current default.


## React Styling Libraries and RSC Compatibility


RSC compatibility is now a first-class filter for styling choices. Runtime CSS-in-JS libraries like styled-components and Emotion execute style injection in the browser, which conflicts with the[React Server Components model](https://react.dev/reference/rsc/server-components) of rendering on the server — so in a Next.js 16 app using RSC,[Tailwind CSS v4](https://tailwindcss.com/blog/tailwindcss-v4) or CSS Modules are the compatible defaults, not runtime CSS-in-JS.


[Tailwind CSS](https://tailwindcss.com/) is the utility-first default. The v4 line moved configuration into CSS via the` @theme` directive and ships a faster engine, per the Tailwind v4 announcement. A single className carries the styling:


```text
<  h1   className  =  "  text-blue-700  "  >{title}</  h1  >
```


Choose **CSS Modules** when you want co-located, scoped CSS with no utility-class vocabulary to learn — they leak nothing and work cleanly in RSC. Choose a build-time CSS-in-JS option like[vanilla-extract](https://vanilla-extract.style/) when you want type-safe styles authored in TypeScript without a runtime cost.


styled-components is in[maintenance mode as of March 2025](https://styled-components.com/blog/celebrating-a-decade-of-styled-components/) and not recommended for new projects, but it is not abandonware — it still works in RSC behind a` 'use client'` boundary and shipped RSC-compatible CSS-variable theming via` createTheme` in its v6.4 release.


The broader point stands: runtime style injection fights the server-render model, so utility-first or build-time approaches are the safer choice in new RSC apps.


## React UI Libraries: Dependency Model vs. Ownership Model


The biggest decision in the UI layer is architectural: install a versioned component library, or own the component code outright. Styled component libraries —[MUI](https://mui.com/) ,[Mantine](https://mantine.dev/) ,[Chakra UI](https://chakra-ui.com/) ,[Ant Design](https://ant.design/) — give you a polished, themeable design system as a dependency you upgrade over time. The ownership model, popularized by shadcn/ui, copies component source into your project instead.


[shadcn/ui](https://ui.shadcn.com/) ’s copy-paste model means you own the component code outright — there’s no versioned dependency to upgrade, no breaking-change migration, and no library author’s design decisions you can’t override — which is architecturally different from installing MUI or Mantine and explains why it has become a default for teams building custom design systems on Tailwind. It has[grown past 110k GitHub stars](https://github.com/shadcn-ui/ui) .


```text
// Ownership model: Button lives in your repo, you edit it directly.
import   {   Button   }   from   '  @/components/ui/button  '


// Dependency model: Button comes from a versioned package.
import   {   Button   }   from   '  @mui/material  '
```


shadcn/ui builds its components on headless primitives, and the default primitive is[Radix](https://www.radix-ui.com/) . Per the[shadcn/ui documentation](https://ui.shadcn.com/docs) , Base UI was added as an official alternative primitive in early 2026, but Radix remains the recommended default.


Radix has been maintained by[WorkOS since its 2022 acquisition of Modulz](https://workos.com/blog/series-b) , and some developers perceive its release cadence as having slowed — context for why[Base UI](https://base-ui.com/) (from the MUI team) appeared as an alternative — though shadcn/ui still defaults to Radix, which remains actively maintained and open source under MIT. Choose **Base UI** when you want primitives from the MUI/Floating UI lineage and styling-engine flexibility; choose a fully styled library like **Ant Design** when you’re building enterprise data-heavy interfaces and want batteries-included tables, forms, and date pickers rather than assembling them.


## React Forms Libraries


[React Hook Form](https://react-hook-form.com/) is the default for forms in modern React. Its design is uncontrolled: fields register via refs rather than holding each keystroke in React state, which minimizes re-renders by construction. With validation handled through schema resolvers, the typical pairing is React Hook Form plus[Zod](https://zod.dev/) for runtime validation and inferred types.


```text
import   {   useForm   }   from   '  react-hook-form  '


const   {   register  ,   handleSubmit   }   =   useForm  ()
```


Per the[React Hook Form documentation](https://react-hook-form.com/get-started) ,` register` wires a field to the form without making it controlled. With the React Compiler auto-memoizing, the manual callback memoization that integration code once needed around field handlers is removed, simplifying the wiring further. Choose[TanStack Form](https://tanstack.com/form/latest) when you want fully type-safe handling of complex, deeply nested form state; choose **Conform** when you’re building progressively enhanced forms around server actions. Formik and React Final Form are effectively unmaintained — avoid them for new projects.


A common production failure mode in forms is excessive re-rendering from controlled inputs that update state on every keystroke. The uncontrolled model produces fewer intermediate state events, which shows up in session replay as cleaner interaction traces — fewer redundant renders between a user typing and the form submitting.


## React Animation Libraries


[Motion](https://motion.dev/) is the de facto animation default for React. Motion — the library formerly known as Framer Motion — uses the package name` motion` and the import path` motion/react` , reached its v12 major with full React 19 support, and[pulls roughly 30 million npm downloads per month](https://www.npmjs.com/package/motion) .


The declarative API drives most of what apps need with` initial` and` animate` props:


```text
import   {   motion   }   from   '  motion/react  '


<  motion.div   initial  =  {  {   opacity  :   0   }  }   animate  =  {  {   opacity  :   1   }  } />
```


Per the[Motion documentation](https://motion.dev/docs/react) , it covers gestures, layout transitions, and` AnimatePresence` for exit animations. One accessibility note: respect the[prefers-reduced-motion media query](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion) , which MDN documents as the standard signal that a user wants minimized motion — Motion lets you read it and gate animations accordingly. Choose **react-spring** when you want physics-based spring animation as the core model; choose **GSAP** when you need fine-grained timeline orchestration across many elements.


## Supporting Cast: Auth, Testing, Charts, and i18n


Beyond the core stack, a few categories round out most apps — one sensible default each:


- **Authentication:**[Auth.js](https://authjs.dev/)
- **Testing:**[Vitest](https://vitest.dev/) for unit tests,[Playwright](https://playwright.dev/) for end-to-end
- **Charts:**[Recharts](https://recharts.org/)
- **Internationalization:**[i18next](https://www.i18next.com/)


Each is a deep topic in its own right; treat these as starting points and follow the docs from there.


## The 2026 React Stack at a Glance


For a Next.js 16 app today, the compatible defaults line up by job:


Category 2026 default Choose instead when RSC-compatible


Server state TanStack Query GraphQL at scale → Apollo; full-stack TS → tRPC Yes (client islands)


Client/global state Zustand Existing Redux → Redux Toolkit Yes (` 'use client'` )


URL state nuqs — Yes


Routing Next.js / React Router Type-safe inference → TanStack Router Yes


Styling Tailwind v4 Scoped CSS → CSS Modules Yes


UI primitives shadcn/ui + Radix MUI lineage → Base UI Yes


Animation Motion (` motion/react` ) Physics → react-spring; timelines → GSAP` 'use client'`


Forms React Hook Form + Zod Complex nested state → TanStack Form` 'use client'`


## Conclusion


The churn is real, but the decision space is small: pick the right bucket for your state, fetch on the server when your framework allows it, default to Tailwind and shadcn/ui for the UI layer, and reach for Motion and React Hook Form for animation and forms. The two changes worth internalizing now are that the React Compiler removes most manual memoization from your integration code, and that RSC compatibility is a hard filter — runtime CSS-in-JS and not-yet-stable RSC support narrow what fits a Next.js 16 app. Start from these defaults, then swap in an alternative only when a specific requirement — GraphQL scale, type-safe routing, physics animation — actually demands it.


## FAQs


Can I use styled-components with React Server Components?


styled-components works with React Server Components only behind a 'use client' boundary, because it injects styles at runtime in the browser, which conflicts with the server-render model of RSC. The library entered maintenance mode in March 2025 and is not recommended for new projects, though it still ships releases and added RSC-compatible CSS-variable theming. For new RSC apps, Tailwind CSS v4 or CSS Modules are the compatible defaults, since both produce styles at build time rather than runtime.


What is the difference between shadcn/ui's ownership model and installing a library like MUI?


shadcn/ui copies component source code directly into your project, so you own and edit the files outright with no versioned dependency to upgrade and no breaking-change migration. MUI and Mantine install as versioned packages you import and upgrade over time, trading customization control for managed updates. The ownership model suits teams building custom design systems who want full override control; the dependency model suits teams who want a polished, maintained design system without owning the code.


When should I use Zustand instead of useState for client state?


Use Zustand when state must be shared across distant components that do not have a direct parent-child relationship, such as a shopping cart, theme, or session data accessed from many places. Use useState or useReducer for state local to one component or a small subtree, like form drafts and toggles. Reaching for Zustand on purely local state adds indirection without benefit, while keeping genuinely global state in prop drilling or context creates unnecessary re-render and coupling problems.


Does the React Compiler replace TanStack Query or a state management library?


No. React Compiler 1.0, stable since October 7, 2025, auto-memoizes components and hook values at build time, removing most manual useMemo and useCallback calls, but it does not manage server-state caching, background refetching, or cross-component state sharing. You still need TanStack Query for server state and Zustand or useState for client state. The compiler changes how much hand-tuning these integrations require, not whether you need the libraries themselves.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
