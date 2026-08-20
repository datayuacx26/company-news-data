---
schema_version: "1.0.0"
document_id: "54c981c524cdec5b8f14739435029c68bc3913d3c6b6857c58a423955a592e56"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/react-router-complete-guide-2026/"
published_at: "2026-07-27T08:51:53+00:00"
first_seen_at: "2026-07-27T18:21:18.752332+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:694393d50dba5d6b09513a80938644a8f344a4056c7062e6fd3ac977ba132606"
---

# React Router: A Complete Guide [2026]

React Router is the standard routing library for React, and it maps URLs to components so a single-page app can have real, shareable URLs. **React Router v8.2.0 is the current release as of July 2026** , following v8.0.0 on June 17, 2026 ([changelog](https://reactrouter.com/changelog?ref=scrimba.com) ). It ships in three modes: declarative, data, and framework.


Start with a warning, because it will save you an hour. Most React Router tutorials still open with` npm install react-router-dom` . That package no longer exists in v8. Copy a tutorial from 2023 into a new project today and the very first import fails.


This guide covers setup through protected routes and migration. Every version claim below is dated and sourced.


## What Does React Router Do, and Why Does React Need a Router?


React Router maps URLs to components, so a single-page React app can have real, bookmarkable URLs, working back-button behavior, and nested layouts.


React ships no router of its own. Build a React app without one and the entire application lives at a single URL. The back button breaks, users cannot bookmark a view, and sharing a link to "that one page" is impossible.


You could reach for` history.pushState` directly, but then you own the URL matcher, the parameter parser, and scroll restoration. That is a router, and writing one yourself is rarely a good use of a week.


> Routes are a tree. The URL selects a branch, and the branch renders as a stack of nested layouts. Almost everything else in React Router follows from that one idea.


A router gives you URL matching, nested layouts, route parameters, natural boundaries for code splitting, and, in the data modes, a place to fetch data *before* a component renders. React was used by **44.7% of developers** in the[2025 Stack Overflow Developer Survey](https://survey.stackoverflow.co/2025/technology?ref=scrimba.com) , which makes routing a near-universal React problem rather than a niche one.


## Which React Router Version Should You Use in 2026?


Use v8, the current major. React Router v7 still receives security updates, while v6 and Remix v2 are formally End of Life as of June 2026.


The version history is genuinely confusing, so here it is in order:


1. **v5** used` <Switch>` and a` component` prop. No data APIs.
2. **v6** (2021) replaced` Switch` with` <Routes>` and` component` with` element` . Then **v6.4 added the data APIs** :` createBrowserRouter` , loaders, and actions.
3. **v7** (November 2024) absorbed Remix. Remix's bundler and server runtime moved into React Router and became *framework mode* ([Remix blog](https://remix.run/blog/merging-remix-and-react-router?ref=scrimba.com) ).
4. **v8** shipped June 17, 2026, with v8.2.0 following on July 8 ([changelog](https://reactrouter.com/changelog?ref=scrimba.com) ). Middleware is on by default,` react-router-dom` is gone, and the package is ESM-only.


The most actionable fact here: the[official v8 announcement](https://remix.run/blog/react-router-v8?ref=scrimba.com) marks **React Router v6 and Remix v2 as End of Life** , so neither receives security updates. If you run v6 in production, that is now a security question rather than a preference.


The team also adopted a **yearly major release cadence** , timed to Node LTS end-of-life, so budget for one major a year instead of treating each as a crisis.


Version floors for v8 are strict: Node 22.22 or later, React 19.2.7 or later, and Vite 7 or later for framework mode ([upgrade guide](https://reactrouter.com/upgrading/v7?ref=scrimba.com) ).


## How Do You Install and Set Up React Router?


Install React Router with` npm i react-router` , then wrap your app in` BrowserRouter` for declarative mode or pass a router to` RouterProvider` for data mode.


The package is` react-router` . Not` react-router-dom` , which v8 removed ([declarative installation](https://reactrouter.com/start/declarative/installation?ref=scrimba.com) ).


```text
npm i react-router


```


Declarative mode wraps your app:


```text
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router";
import App from "./app";


ReactDOM.createRoot(document.getElementById("root")).render(
<BrowserRouter>
<App />
</BrowserRouter>,
);


```


Data mode has one detail that trips people up constantly.` createBrowserRouter` comes from` react-router` , but` RouterProvider` comes from` react-router/dom` ([data installation](https://reactrouter.com/start/data/installation?ref=scrimba.com) ). Two imports, two paths:


```text
import { createBrowserRouter } from "react-router";
import { RouterProvider } from "react-router/dom";


const router = createBrowserRouter([
{ path: "/", element: <div>Hello World</div> },
]);


ReactDOM.createRoot(document.getElementById("root")).render(
<RouterProvider router={router} />,
);


```


Framework mode gets its own scaffold ([framework installation](https://reactrouter.com/start/framework/installation?ref=scrimba.com) ):


```text
npx create-react-router@latest my-app


```


## The Three Modes: Declarative, Data, and Framework


React Router has three modes. Declarative handles URL matching only, data adds loaders and actions, and framework adds server rendering, bundling, and typed routes.


Mode Setup Key APIs Use it when


Declarative` <BrowserRouter>`` Link` ,` NavLink` ,` useNavigate` ,` useLocation` You want routing only, or already have a data layer


Data` createBrowserRouter` plus` RouterProvider`` loader` ,` action` ,` useLoaderData` ,` useFetcher` ,` useNavigation` You want data loading and pending states without a full framework


Framework` routes.ts` plus the Vite plugin` route` ,` index` ,` layout` ,` prefix` , Route Module API, typed` href` You are starting fresh, or weighing Next.js and want SSR plus type safety


The critical framing most comparisons miss: **the modes are additive, not alternatives** ([picking a mode](https://reactrouter.com/start/modes?ref=scrimba.com) ). Framework mode is data mode plus a build pipeline. Data mode is declarative mode plus loaders. You are not choosing between three libraries, you are choosing how far up the stack to go.


The decision rule is short. Adding routing to an existing Vite SPA that already fetches with React Query? Declarative. Fighting` useEffect` waterfalls and hand-writing loading spinners? Data. Starting greenfield and seriously comparing against Next.js? Framework.


Framework mode configures routes in a` routes.ts` file rather than JSX ([framework routing](https://reactrouter.com/start/framework/routing?ref=scrimba.com) ):


```text
import { type RouteConfig, route, index, layout, prefix } from "@react-router/dev/routes";


export default [
index("./home.tsx"),
route("about", "./about.tsx"),
layout("./auth/layout.tsx", [
route("login", "./auth/login.tsx"),
route("register", "./auth/register.tsx"),
]),
...prefix("concerts", [
index("./concerts/home.tsx"),
route(":city", "./concerts/city.tsx"),
]),
] satisfies RouteConfig;


```


## How Do You Define Routes, Nested Layouts, and Dynamic Segments?


Routes nest as a tree. A parent route renders shared layout plus an` <Outlet />` placeholder, and dynamic` :param` segments are read with the` useParams` hook.


The simplest configuration pairs a path with an element:


```text
import { BrowserRouter, Routes, Route } from "react-router";


<BrowserRouter>
<Routes>
<Route path="/" element={<App />} />
</Routes>
</BrowserRouter>


```


Nesting is where routing stops being a lookup table and starts being useful. A parent renders its own UI and drops` <Outlet />` wherever the matched child belongs:


```text
import { Outlet } from "react-router";


export default function Dashboard() {
return (
<div>
<h1>Dashboard</h1>
<Outlet />
</div>
);
}


```


```text
<Routes>
<Route path="dashboard" element={<Dashboard />}>
<Route index element={<Home />} />
<Route path="settings" element={<Settings />} />
</Route>
</Routes>


```


Two concepts there cause most beginner confusion:


- An **index route** has no path and renders into the parent's outlet at the parent's own URL. Above,` /dashboard` renders` <Home />` inside` <Dashboard />` .
- A **layout route** is a` <Route>` with an` element` but no` path` . It nests without adding a URL segment, which is how you share a sidebar across pages living at unrelated paths.


Dynamic segments start with a colon and are read with` useParams` ([declarative routing](https://reactrouter.com/start/declarative/routing?ref=scrimba.com) ):


```text
<Route path="teams/:teamId" element={<Team />} />


```


```text
import { useParams } from "react-router";


export default function Team() {
const { teamId } = useParams();
return <h1>Team {teamId}</h1>;
}


```


If components, props, and hooks are still shaky, routing will feel harder than it is, and Scrimba's[guide to learning React](https://scrimba.com/articles/how-to-learn-react/) covers the order to tackle things in. For routing specifically, Scrimba's[Learn React Router](https://scrimba.com/learn-react-router-c06?ref=scrimba.com) runs 9.7 hours with Bob Ziroll and is built around one application called VanLife, so routes, layouts, and params accumulate in a single real codebase. It teaches React Router v6, where the declarative and data-mode concepts here originate, and it does not cover framework mode. Routing also clicks fastest once you build something multi-page with it, which is where Scrimba's list of[React projects for beginners](https://scrimba.com/articles/best-react-projects-for-beginners/) finishes up.


## Link, NavLink, and Programmatic Navigation


Use` <Link>` for normal navigation,` <NavLink>` when a link needs an active state, and` useNavigate` only when navigation happens without a user click.


```text
import { Link } from "react-router";


<Link to="/login">Login again</Link>


```


` <NavLink>` knows whether it points at the current URL. It applies an` .active` class automatically and accepts callbacks for` className` ,` style` , and children:


```text
import { NavLink } from "react-router";


<NavLink to="/" end>Home</NavLink>


<NavLink
to="/messages"
className={({ isActive }) => (isActive ? "text-red-500" : "text-black")}
>
Messages
</NavLink>


```


The` end` prop matters more than it looks. Without it, a` NavLink` to` /` counts as active on every page, because every path starts with a slash.


For navigation with no click behind it, use the hook:


```text
import { useNavigate } from "react-router";


const navigate = useNavigate();
navigate("/dashboard");


```


Reach for each one deliberately:


- ` <Link>` for anything a user clicks. It renders a real anchor, so keyboard access and open-in-new-tab work.
- ` <NavLink>` for navigation menus, breadcrumbs, and tabs where the current location needs styling.
- ` useNavigate` for redirects after a submit, logout on inactivity, or a timed quiz advancing itself.


The docs are direct: prefer` <Link>` and` <NavLink>` over` useNavigate` for user-initiated navigation ([navigating](https://reactrouter.com/start/declarative/navigating?ref=scrimba.com) ). A` div` with an` onClick` that calls` navigate()` looks identical and is *quietly inaccessible* .


## Loading Data with Loaders and Actions


A` loader` runs before its route renders and its result is read with` useLoaderData` . An` action` handles mutations and revalidates loader data automatically.


This is the reason to move from declarative to data mode. Fetching inside` useEffect` means the component renders, then requests data, then renders again. Nest three of those and you get a waterfall.


Loaders move fetching onto the route, so the request starts when navigation starts:


```text
import { useLoaderData, createBrowserRouter } from "react-router";


async function loader({ params }) {
return { project: await db.getProject(params.projectId) };
}


function Project() {
const { project } = useLoaderData();
return <h1>{project.title}</h1>;
}


createBrowserRouter([
{ path: "/projects/:projectId", loader, Component: Project },
]);


```


Actions handle writes. Point a` <Form>` at a route and its` action` receives the request ([actions](https://reactrouter.com/start/data/actions?ref=scrimba.com) ):


```text
import { Form, useActionData } from "react-router";


async function action({ request }) {
const formData = await request.formData();
return await db.updateProject({ title: formData.get("title") });
}


function Project() {
const actionData = useActionData();
return (
<Form method="post">
<input type="text" name="title" />
<button type="submit">Save</button>
</Form>
);
}


```


The mutation loop is the payoff, and it runs on its own:


1. The form submits and the route's` action` runs.
2. The action returns, and every loader on the matched routes revalidates.
3. Components re-render with fresh data.


No manual cache invalidation, no` refetch()` threaded through props. Two related hooks fill the gaps:` useFetcher` submits without navigating, and` useNavigation` exposes global pending state for progress bars.


One honest limitation. Loaders are not a cache. They give you fetch-on-navigate with revalidation, not deduplication or background refetching. If you need those, keep a data library and let the router handle URLs.


## Error Boundaries and Protected Routes


### Handling errors


Attach an` ErrorBoundary` to a route and read the thrown value with` useRouteError` . The` isRouteErrorResponse` helper separates thrown HTTP responses from ordinary JavaScript errors:


```text
import { useRouteError, isRouteErrorResponse } from "react-router";


function RootErrorBoundary() {
const error = useRouteError();
if (isRouteErrorResponse(error)) {
return <h1>{error.status} {error.statusText}</h1>;
}
if (error instanceof Error) {
return <p>{error.message}</p>;
}
return <h1>Unknown Error</h1>;
}


createBrowserRouter([
{ path: "/", Component: Root, ErrorBoundary: RootErrorBoundary },
]);


```


Loaders can throw deliberate HTTP errors, turning a missing record into a real 404 instead of a crash:


```text
import { data } from "react-router";


export async function loader({ params }) {
const record = await db.getRecord(params.id);
if (!record) throw data("Record Not Found", { status: 404 });
return record;
}


```


Errors bubble to the **closest** boundary up the route tree ([error boundaries](https://reactrouter.com/how-to/error-boundary?ref=scrimba.com) ). Put one at the root always, then add specific ones where a partial failure should not blank the whole page.


### Protecting routes with auth


Here v8 changed the recommended answer, for a correctness reason rather than style. Loaders run in **parallel** , so a` redirect` thrown from a parent loader does not stop child loaders from running. Unauthenticated requests still reach your data functions.


Middleware runs as a sequential chain from parent to child, so a redirect thrown in a parent genuinely short-circuits everything below it ([middleware](https://reactrouter.com/how-to/middleware?ref=scrimba.com) ):


```text
import { createContext, redirect } from "react-router";


export const userContext = createContext(null);


async function authMiddleware({ request, context }) {
const user = await getUserFromSession(request);
if (!user) throw redirect("/login");
context.set(userContext, user);
}


const routes = [
{
path: "/dashboard",
middleware: [authMiddleware],
loader: ({ context }) => ({ user: context.get(userContext) }),
Component: Dashboard,
},
];


```


Middleware is **enabled by default in v8** . On v7 it sits behind the` future.v8_middleware` flag ([upgrade guide](https://reactrouter.com/upgrading/v7?ref=scrimba.com) ). It works in framework and data modes, not declarative.


## Code Splitting Routes with Lazy Loading


In data mode, the` lazy` route property loads a route's component and loader on demand, keeping them out of the initial bundle.


```text
createBrowserRouter([
{
path: "/app",
lazy: async () => {
const [Component, loader] = await Promise.all([
import("./app"),
import("./app-loader"),
]);
return { Component, loader };
},
},
]);


```


Route-matching fields cannot be lazy.` path` ,` index` , and` children` must be known upfront, because matching happens before the lazy function is ever called ([route object](https://reactrouter.com/start/data/route-object?ref=scrimba.com) ). Framework mode handles code splitting for you, so` lazy` is a data-mode concern.


## React Router vs Next.js App Router vs TanStack Router


Pick React Router for SPAs and incremental adoption, Next.js App Router for server-first React, and TanStack Router when end-to-end type safety is the priority.


**React Router** is the ecosystem default with the largest installed base, and the only one with a genuine incremental path. You can add declarative routing to an existing SPA this afternoon and adopt loaders next quarter. Framework mode inherits the Remix lineage, so server rendering is available rather than assumed.


**Next.js App Router** is not really a router choice, it is a framework choice. File-system routing arrives bundled with React Server Components, a caching model, and an opinionated deployment story. That is the point if your app is server-first, and overhead if you are shipping a dashboard behind a login. Scrimba's roundup of[Next.js courses](https://scrimba.com/articles/best-next-js-courses-and-tutorials-2026/) is the next stop if that is your branch.


**TanStack Router** has the strongest type-safety story. Params, loader data, and *search params* are all typed, with validated schemas and defaults. React Router still hands you search params as strings to parse yourself, which is a real gap. The trade-offs are a smaller ecosystem and a codegen step. Scrimba's[TypeScript guide](https://scrimba.com/articles/how-to-learn-typescript-a-guide-for-javascript-developers-2026/) covers the prerequisites if typed routing appeals.


One line each. Existing React SPA that needs URLs: React Router. Server-rendered content site or commerce: Next.js. TypeScript-first app where wrong URL state is a recurring bug: TanStack Router.


## Migrating from v5, v6, and v7


Migration is incremental rather than a rewrite. The pattern repeats for every major: enable future flags on your current version, fix what breaks, then bump.


**v5 to v6.** Replace` <Switch>` with` <Routes>` , swap` component={X}` for` element={<X />}` , and replace` useHistory` with` useNavigate` . Matching became ranked rather than first-match, so` exact` is gone.


**v6 to v7.** Turn on the future flags in v6 first, one at a time:` v7_relativeSplatPath` ,` v7_startTransition` ,` v7_fetcherPersist` ,` v7_normalizeFormMethod` ,` v7_partialHydration` , and` v7_skipActionErrorRevalidation` ([v6 future flags](https://reactrouter.com/6.30.4/upgrading/future?ref=scrimba.com) ). With all six green, the v7 bump is close to a version number change.


**v7 to v8.** Same playbook. Enable` v8_middleware` ,` v8_splitRouteModules` ,` v8_viteEnvironmentApi` ,` v8_passThroughRequests` , and` v8_trailingSlashAwareDataRequests` on v7, then upgrade. Two code changes are unavoidable ([upgrade guide](https://reactrouter.com/upgrading/v7?ref=scrimba.com) ). Every` react-router-dom` import moves, and` data` becomes` loaderData` in` meta` functions and` useMatches()` :


```text
// Before (v7)
import { Link, RouterProvider } from "react-router-dom";


// After (v8)
import { Link } from "react-router";
import { RouterProvider } from "react-router/dom";


```


One more floor to check before starting: v8 is published ESM-only, which will surprise any project still running CommonJS tooling.


*Do the flags on the old version, where rollback is one line.* Doing them during the major bump means debugging five changes at once with no way back.


## Frequently Asked Questions


### What is the latest version of React Router?


React Router v8.2.0, released July 8, 2026, is the current version. Version 8.0.0 shipped on June 17, 2026. The team has adopted a yearly major release cadence timed to Node LTS end-of-life, so a new major arrives each June.


### Is react-router-dom still needed?


No. React Router v8 removed the` react-router-dom` package entirely. Install` react-router` instead. Most APIs import from` react-router` , while a few DOM-specific ones, including` RouterProvider` , import from` react-router/dom` . Older tutorials that install` react-router-dom` will fail on v8.


### What is the difference between declarative, data, and framework mode?


The three modes are additive. Declarative handles URL matching and navigation only. Data adds loaders, actions, and pending states by moving route config outside React rendering. Framework adds server rendering, code splitting, and typed routes on top of data mode.


### Do I need React Router if I use Next.js?


No. Next.js includes its own file-system router, and the App Router handles layouts, dynamic segments, and data loading natively. Mixing the two causes conflicting navigation state. Choose one router per application.


### How long does it take to learn React Router?


Basic routing, links, and params take an afternoon if you already know React. Loaders, actions, error boundaries, and protected routes take longer. Scrimba's dedicated React Router course runs 9.7 hours and builds one application across all of those concepts.


## Key Takeaways


- React Router v8.2.0 is the current release as of July 2026, and v8.0.0 shipped June 17, 2026.
- React Router v6 and Remix v2 are End of Life and no longer receive security updates. Version 7 still does.
- The package is` react-router` . The` react-router-dom` package was removed in v8, which breaks the first line of most older tutorials.
- In data mode,` createBrowserRouter` imports from` react-router` but` RouterProvider` imports from` react-router/dom` .
- The three modes are additive: framework builds on data, which builds on declarative.
- Loaders start fetching when navigation starts, and actions revalidate loaders automatically after a mutation.
- Protect routes with middleware, not parent loaders, because loaders run in parallel and a parent redirect does not stop child loaders.
- Migrate by enabling future flags on your current version first, then bumping the major.
- Scrimba's Learn React Router is a 9.7-hour deep dive on one application, and the free[Learn React](https://scrimba.com/learn-react-c0e?ref=scrimba.com) course covers the fundamentals it assumes, certificate included. Pro runs $24.50/mo annually, with student and location-based discounts.


## Sources


Official React Router documentation, accessed July 2026:


- "CHANGELOG."[https://reactrouter.com/changelog](https://reactrouter.com/changelog?ref=scrimba.com)
- "Picking a Mode."[https://reactrouter.com/start/modes](https://reactrouter.com/start/modes?ref=scrimba.com)
- "Declarative Installation."[https://reactrouter.com/start/declarative/installation](https://reactrouter.com/start/declarative/installation?ref=scrimba.com)
- "Declarative Routing."[https://reactrouter.com/start/declarative/routing](https://reactrouter.com/start/declarative/routing?ref=scrimba.com)
- "Declarative Navigating."[https://reactrouter.com/start/declarative/navigating](https://reactrouter.com/start/declarative/navigating?ref=scrimba.com)
- "Data Mode Installation."[https://reactrouter.com/start/data/installation](https://reactrouter.com/start/data/installation?ref=scrimba.com)
- "Route Object."[https://reactrouter.com/start/data/route-object](https://reactrouter.com/start/data/route-object?ref=scrimba.com)
- "Actions."[https://reactrouter.com/start/data/actions](https://reactrouter.com/start/data/actions?ref=scrimba.com)
- "Framework Routing."[https://reactrouter.com/start/framework/routing](https://reactrouter.com/start/framework/routing?ref=scrimba.com)
- "Framework Installation."[https://reactrouter.com/start/framework/installation](https://reactrouter.com/start/framework/installation?ref=scrimba.com)
- "Error Boundaries."[https://reactrouter.com/how-to/error-boundary](https://reactrouter.com/how-to/error-boundary?ref=scrimba.com)
- "Middleware."[https://reactrouter.com/how-to/middleware](https://reactrouter.com/how-to/middleware?ref=scrimba.com)
- "Upgrading to v8."[https://reactrouter.com/upgrading/v7](https://reactrouter.com/upgrading/v7?ref=scrimba.com)
- "Current Future Flags."[https://reactrouter.com/6.30.4/upgrading/future](https://reactrouter.com/6.30.4/upgrading/future?ref=scrimba.com)


Other sources:


- Remix. "React Router v8." 2026.[https://remix.run/blog/react-router-v8](https://remix.run/blog/react-router-v8?ref=scrimba.com)
- Remix. "Merging Remix and React Router." 2024.[https://remix.run/blog/merging-remix-and-react-router](https://remix.run/blog/merging-remix-and-react-router?ref=scrimba.com)
- Stack Overflow. "2025 Developer Survey: Technology." 2025.[https://survey.stackoverflow.co/2025/technology](https://survey.stackoverflow.co/2025/technology?ref=scrimba.com)
