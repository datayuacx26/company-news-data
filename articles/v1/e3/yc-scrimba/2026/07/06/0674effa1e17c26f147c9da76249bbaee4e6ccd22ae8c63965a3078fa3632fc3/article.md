---
schema_version: "1.0.0"
document_id: "0674effa1e17c26f147c9da76249bbaee4e6ccd22ae8c63965a3078fa3632fc3"
company_key: "yc-scrimba"
company: "Scrimba"
source_id: "yc-scrimba-news-import-2d86273fd3f5"
canonical_url: "https://scrimba.com/articles/react-19-whats-new-for-developers/"
published_at: "2026-07-25T10:28:45+00:00"
first_seen_at: "2026-07-25T14:29:37.926735+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:756e4310120010519fde2b0b1833842b523b627ef9fd4830c0fc51db7f38ed10"
---

# React 19: What's New for Developers [2026]

React 19 shipped on December 5, 2024 and is now the baseline for modern React work, not the new thing. It introduced **Actions** and the hooks built on them, the` use` API for reading promises and context during render,` ref` as an ordinary prop, native document metadata, and stable Server Components. Since then the line has reached 19.2, and the React Compiler has reached 1.0.


Most articles about React 19 were written the week it landed and never updated, which matters: the compiler is no longer experimental, and 19.2 added stable APIs the release notes never mentioned. This guide covers what shipped, what is genuinely stable, and what breaks when you upgrade.


## Where React 19 stands in 2026


React 19 is the current major version. According to[React's versions page](https://react.dev/versions?ref=scrimba.com) , the latest patch is 19.2.7, released June 1, 2026. No React 19.3 or React 20 has been announced.


Release Date Best known for


React 19.0 December 5, 2024 Actions,` use` , ref as a prop, document metadata, Server Components


React 19.1 March 28, 2025 Owner Stacks and` captureOwnerStack` , a development-only debugging API


React 19.2 October 1, 2025` <Activity>` ,` useEffectEvent` , Performance Tracks, Partial Pre-rendering


React 19.2.7 June 1, 2026 Latest patch release


React also moved under an independent[React Foundation](https://react.dev/blog/2025/10/07/introducing-the-react-foundation?ref=scrimba.com) hosted by the Linux Foundation, announced at React Conf in October 2025. That changes who stewards releases, not the API. If you are still weighing whether to invest in React,[React vs Angular](https://scrimba.com/articles/react-vs-angular-which-framework-should-you-learn-in-2026/) answers that separate question.


## React 19 features at a glance


Here is the surface area, with what each thing replaces and whether it is stable as of July 2026.


Feature What it replaces Stable?


Actions (async functions in transitions) Hand-rolled pending and error state Yes, 19.0


` useActionState` The canary` useFormState` , manual submit handlers Yes, 19.0


` useFormStatus` Prop-drilling` isSubmitting` into buttons Yes, 19.0


` useOptimistic` Manual optimistic state and rollback Yes, 19.0


` use`` useEffect` plus` useState` to read a promise Yes, 19.0


` ref` as a prop, plus ref cleanup` forwardRef` Yes, 19.0


Document metadata,` precedence` , preloading` react-helmet` , manual` <head>` work Yes, 19.0


Server Components and Server Functions Framework-specific data loading Yes, 19.0, with a caveat below


React Compiler Most` useMemo` ,` useCallback` ,` memo` Yes, 1.0 in October 2025


` <Activity>` Unmounting and remounting hidden subtrees Yes, 19.2


` useEffectEvent` The` useRef` workaround for stale Effect values Yes, 19.2


Partial Pre-rendering Nothing, it is new Yes, 19.2


` <ViewTransition>` and Fragment Refs Animation libraries, wrapper divs **No** , Canary only


*Everything above the last row ships in a version you can install today.* View Transitions and Fragment Refs are real and demoed, but they live in React's Canary and Experimental channels, not in 19.2.


## Actions: the idea the rest of React 19 is built on


An **Action** is an async function passed to a transition. React takes over the pending state, error handling, optimistic updates and form resets every team used to write by hand. The three hooks below are not separate features; they are three views onto the same mechanism.


### useActionState


In React 18 that meant juggling three pieces of state:


```text
// React 18: pending, error and ordering all handled manually
function UpdateName() {
const [name, setName] = useState("");
const [error, setError] = useState(null);
const [isPending, setIsPending] = useState(false);


const handleSubmit = async () => {
setIsPending(true);
const error = await updateName(name);
setIsPending(false);
if (error) {
setError(error);
return;
}
redirect("/profile");
};


return (
<div>
<input value={name} onChange={(e) => setName(e.target.value)} />
<button onClick={handleSubmit} disabled={isPending}>Update</button>
{error && <p>{error}</p>}
</div>
);
}


```


React 19 collapses that into one hook:


```text
// React 19: useActionState owns pending, error and sequencing
function UpdateName() {
const [error, submitAction, isPending] = useActionState(
async (previousState, formData) => {
const error = await updateName(formData.get("name"));
if (error) return error;
redirect("/profile");
return null;
},
null
);


return (
<form action={submitAction}>
<input type="text" name="name" />
<button type="submit" disabled={isPending}>Update</button>
{error && <p>{error}</p>}
</form>
);
}


```


Three details[the release notes](https://react.dev/blog/2024/12/05/react-19?ref=scrimba.com) bury. The hook was called` useFormState` in canary builds, so older tutorials use the wrong name. The dispatch function must run inside a Transition, which a form` action` prop handles automatically. And the optional third argument,` permalink` , enables progressive enhancement for forms submitted before the JavaScript bundle loads, but only with React Server Components.


### Form Actions and useFormStatus


` <form>` ,` <input>` and` <button>` now accept a function for their` action` and` formAction` props. React calls it with the` FormData` , manages the pending state, and resets the form on success.


` useFormStatus` then lets any descendant read the parent form's state without prop drilling. It returns` pending` ,` data` ,` method` and` action` . One gotcha catches almost everyone on first use:


```text
// Wrong: useFormStatus cannot see a form rendered by the same component
function Form() {
const { pending } = useFormStatus(); // pending is always false
return (
<form action={submit}>
<button disabled={pending}>Send</button>
</form>
);
}


// Right: read the status from a child of the form
function SubmitButton() {
const { pending } = useFormStatus();
return <button disabled={pending}>Send</button>;
}


function Form() {
return (
<form action={submit}>
<SubmitButton />
</form>
);
}


```


### useOptimistic


` useOptimistic` shows the expected result immediately and reverts automatically if the request fails. No manual rollback, no snapshot.


```text
function ChangeName({ currentName, onUpdateName }) {
const [optimisticName, setOptimisticName] = useOptimistic(currentName);


const submitAction = async (formData) => {
const newName = formData.get("name");
setOptimisticName(newName);
onUpdateName(await updateName(newName));
};


return (
<form action={submitAction}>
<p>Your name is: {optimisticName}</p>
<input type="text" name="name" disabled={currentName !== optimisticName} />
</form>
);
}


```


## The use API: reading promises and context in render


` use` is a React API that reads the value of a resource, either a promise or a context, during render. Unlike hooks, it can be called inside conditionals and loops.


*It is the first React API that legitimately breaks the shape the rules of hooks enforce.* It also comes with two hard constraints tutorials skip.


First, promises passed to` use` must be cached. A promise created during render is recreated on every render, which makes React show the Suspense fallback forever.


```text
// Wrong: a new promise every render, so the fallback never goes away
function Albums() {
const albums = use(fetch("/api/albums").then((r) => r.json()));
}


// Right: cache outside render, then unwrap
const cache = new Map();
function fetchAlbums(url) {
if (!cache.has(url)) cache.set(url, fetch(url).then((r) => r.json()));
return cache.get(url);
}


function Albums({ url, show }) {
const albums = use(fetchAlbums(url));   // suspends until resolved
if (show) {
const theme = use(ThemeContext);      // legal inside an if
return <List items={albums} theme={theme} />;
}
return <List items={albums} />;
}


```


Second,` use` cannot sit inside a` try/catch` ; rejections are caught by the nearest Error Boundary instead. Reading context with` use` is also unsupported in Server Components.


The clean mental model: Server Components` await` a promise during render, Client Components cannot, so they unwrap it with` use` . Both suspend where the value is read.


## ref as a prop, and the slow end of forwardRef


Function components now receive` ref` as a normal prop.` forwardRef` still works, but the[React docs](https://react.dev/reference/react/forwardRef?ref=scrimba.com) state it "will be deprecated in a future release".


```text
// React 18
const Input = forwardRef(function Input(props, ref) {
return <input {...props} ref={ref} />;
});


// React 19
function Input({ ref, ...props }) {
return <input {...props} ref={ref} />;
}


```


Ref callbacks can also return a cleanup function now, which runs when the element unmounts:


```text
<input ref={(node) => {
observer.observe(node);
return () => observer.unobserve(node);
}} />


```


This is why TypeScript in React 19 rejects implicit returns from ref callbacks:` ref={el => (inputRef.current = el)}` returns a value React now reads as a cleanup function. Wrap the body in braces.


## Document metadata, stylesheets, and scripts


React 19 hoists` <title>` ,` <meta>` and` <link>` tags to the document` <head>` wherever in the tree you render them. Stylesheets gained a` precedence` attribute that controls insertion order and holds back content until they load. Async scripts are deduplicated automatically.


```text
function BlogPost({ post }) {
return (
<article>
<h1>{post.title}</h1>
<title>{post.title}</title>
<meta name="description" content={post.excerpt} />
<link rel="stylesheet" href="/blog.css" precedence="default" />
<p>{post.body}</p>
</article>
);
}


```


Four resource-loading functions came with it:` preload` ,` preinit` ,` preconnect` and` prefetchDNS` . One caveat: your framework's metadata API has not gone away. Next.js still has its own, and mixing the two in one route ships duplicate tags.


## Suspense: pre-warming and batched reveals


React 19 changed the order of operations when a component suspends. The nearest fallback commits immediately, then React renders the suspended siblings, pre-warming lazy requests further down the tree.


The trade-off was[debated loudly](https://github.com/facebook/react/issues/29898?ref=scrimba.com) during the RC period. Because siblings are no longer pre-rendered before the fallback commits, some patterns that fired requests in parallel now fire them as a *waterfall* . If you rely on siblings kicking off fetches, measure before and after.


React 19.2 added a related server-side change: Suspense boundaries batch their reveals during streaming SSR, so the server reveals content in the same rhythm as the client.


## The React Compiler in 2026


The React Compiler reached[version 1.0 on October 7, 2025](https://react.dev/blog/2025/10/07/react-compiler-1?ref=scrimba.com) . It is stable, production-tested, and the single biggest reason to distrust a React 19 article written in early 2025.


The compiler is a build-time tool that memoizes automatically. It makes` useMemo` ,` useCallback` and` React.memo` largely unnecessary, and it memoizes where manual techniques cannot, such as after an early return. Those three APIs remain as escape hatches.


```text
npm install --save-dev --save-exact babel-plugin-react-compiler@latest
npm install --save-dev eslint-plugin-react-hooks@latest


```


What you should know before adopting it:


- It works with **React 17 and up** . On 17 or 18, add` react-compiler-runtime` and set a minimum target in the compiler config.
- New apps created with Vite, Next.js or Expo SDK 54 and later have it enabled by default.
- The ESLint rules ship inside` eslint-plugin-react-hooks` , so linting and compiling share one source of truth.
- Meta reports up to 12% faster initial loads and navigations on the Meta Quest Store, and roughly 2.5x faster performance on some interactions.
- Pin an exact version if your test coverage is thin, so memoization behavior does not shift on a patch bump.


## Server Components: stable, with one asterisk


React Server Components and Server Functions are stable in React 19. Components render ahead of bundling, at build time or per request, and Server Functions marked` "use server"` are callable from Client Components.


> **The asterisk:** React's[release notes](https://react.dev/blog/2024/12/05/react-19?ref=scrimba.com) say Server Components are "stable and will not break between minor versions," but the APIs a bundler or framework uses to implement them "do not follow semver" and may break between 19.x minors. Pin your React version if you maintain that layer.


A security note belongs in any 2026 upgrade conversation. Denial-of-service and source-code-exposure[advisories](https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components?ref=scrimba.com) hit the` react-server-dom-*` packages through January 2026; the patched releases are **19.0.4, 19.1.5 and 19.2.4** . Apps without RSC are unaffected, but if you run Next.js, React Router, Waku, or a Parcel or Vite RSC setup, check your lockfile.


To work with server rendering in practice rather than theory, a framework is the better teacher. Scrimba's free[Learn Next.js](https://scrimba.com/learn-nextjs-c02moisq6a?ref=scrimba.com) course (8.5 hours, Bob Ziroll) builds a database-backed app across server and client rendering, and this[Next.js course comparison](https://scrimba.com/articles/best-next-js-courses-and-tutorials-2026/) covers alternatives.


## What React 19.2 added on top


Four stable additions from[React 19.2](https://react.dev/blog/2025/10/01/react-19-2?ref=scrimba.com) that most React 19 explainers never covered:


1. ` **<Activity>**` hides or shows a subtree. In` hidden` mode it unmounts effects and defers updates while keeping state, so you can pre-render a route the user is likely to visit, or park a page they left.
2. ` **useEffectEvent**` extracts non-reactive logic from an Effect so a value like` theme` can be read without becoming a dependency. Effect Events must never go in the dependency array.
3. **Performance Tracks** add React-specific Scheduler and Components lanes to Chrome DevTools profiles.
4. **Partial Pre-rendering** prerenders the static shell and resumes the dynamic parts later, through` resume` ,` resumeToPipeableStream` and` resumeAndPrerender` .


One small breaking detail hides in 19.2: the default` useId` prefix changed from` :r:` to` _r_` so IDs work in CSS selectors and XML names. Snapshot tests containing` :r0:` will fail.


## Upgrading to React 19: what breaks and what to do first


React 19 removes a decade of deprecated APIs at once. The upgrade is mechanical for most apps and painful for a few. Do it in this order:


1. **Upgrade to the latest patch** , not` 19.0.0` . Given the RSC advisories, land on 19.2.x.
2. **Run**[the migration recipe](https://react.dev/blog/2024/04/25/react-19-upgrade-guide?ref=scrimba.com) , which bundles the render, string-ref,` act` -import and` useFormState` codemods.
3. **Run the TypeScript codemod** for the ref-cleanup,` useRef` and JSX namespace changes.
4. **Switch to the new JSX transform** , which React 19 requires.
5. **Re-wire error reporting** , for the reason in the callout below.


```text
npx codemod@latest react/19/migration-recipe
npx types-react-codemod@latest preset-19 ./src


```


What is gone for good:


- ` propTypes` and` defaultProps` on **function** components. Class components keep` defaultProps` ; function components use default parameters.
- Legacy context (` contextTypes` ,` getChildContext` ), string refs,` React.createFactory` , module pattern factories.
- ` ReactDOM.render` ,` ReactDOM.hydrate` ,` unmountComponentAtNode` ,` findDOMNode` .
- ` react-dom/test-utils` , with` act` now imported from` react` , and` react-test-renderer/shallow` .
- UMD builds,` javascript:` URLs in` src` and` href` , and several` unstable_*` APIs.


> **The change that catches teams out:** errors thrown during render are no longer re-thrown. Uncaught errors go to` window.reportError` , boundary-caught errors to` console.error` . Monitoring hooked to the global` error` event quietly stops reporting React render errors until you pass` onUncaughtError` and` onCaughtError` to your root.


```text
const root = createRoot(container, {
onUncaughtError: (error, errorInfo) => reportToMonitoring(error, errorInfo),
onCaughtError: (error, errorInfo) => log(error, errorInfo),
});


```


**Who should wait.** If a critical dependency has not published React 19 peer support, wait for the library rather than forcing resolutions. If your test suite leans on` react-test-renderer` , budget the migration to Testing Library first, because that deprecation is the real work. And if you maintain a framework or bundler integration against the RSC APIs, plan for churn between minors.


## How to learn React 19 hands-on


Reading about Actions is not the same as debugging one. Scrimba's scrim format lets you pause the lesson and edit the instructor's code in the browser, the fastest way to get these APIs into your fingers.


Scrimba's[What's New in React 19?](https://scrimba.com/whats-new-in-react-19-c03d?ref=scrimba.com) is free, runs 69 minutes, and is taught by Bob Ziroll. It covers a` useTransition` recap and the React Compiler, form actions with error and loading state,` useActionState` ,` useOptimistic` and` useFormStatus` , refs as props, the` use()` API, and the meta-tag improvements. It assumes solid React, so it is an *upgrade* course, not an introduction.


If the fundamentals are shaky, start with[Learn React](https://scrimba.com/learn-react-c0e?ref=scrimba.com) , also free and taught by Bob Ziroll across 15.1 hours, or follow the sequence in[how to learn React](https://scrimba.com/articles/how-to-learn-react/) .[Advanced React](https://scrimba.com/advanced-react-c02h?ref=scrimba.com) (Pro, 13.2 hours) goes deeper on reusability, routing, performance and TypeScript, though not on React 19-specific APIs. To practise the new hooks, pick from these[React project ideas](https://scrimba.com/articles/best-react-projects-for-beginners/) or weigh options in this[React course roundup](https://scrimba.com/articles/best-react-courses-and-tutorials-compared-2026/) .


Scrimba's free courses include completion certificates. Pro access is $24.50 per month on the annual plan ($294 per year), with location-based, student and promotional discounts on top.


## Frequently Asked Questions


### Is React 19 stable and production ready?


Yes. React 19.0 was released as stable on December 5, 2024, and the line has reached 19.2, with 19.2.7 as the latest patch in June 2026. Server Components are stable too. Only` <ViewTransition>` and Fragment Refs remain Canary-only.


### Do I need to use the React Compiler with React 19?


No. The React Compiler is optional and separate from React 19. It reached version 1.0 in October 2025 and works with React 17 and later. New Vite, Next.js and Expo apps enable it by default, but existing apps can adopt it incrementally or skip it.


### Does React 19 remove forwardRef?


No.` forwardRef` still works in React 19. Function components now accept` ref` as a normal prop, so the wrapper is unnecessary, and the React docs state that` forwardRef` will be deprecated in a future release. A codemod migrates existing components.


### What breaks when upgrading from React 18 to React 19?


The main removals are` propTypes` and` defaultProps` on function components, legacy context, string refs,` ReactDOM.render` ,` ReactDOM.hydrate` ,` findDOMNode` ,` react-dom/test-utils` and UMD builds. The new JSX transform is required, and render errors are reported rather than re-thrown.


### What is the difference between React 19 and React 19.2?


React 19.0 introduced Actions, the` use` API, ref as a prop, document metadata and stable Server Components. React 19.2, released October 1, 2025, added` <Activity>` ,` useEffectEvent` ,` cacheSignal` , Performance Tracks, Partial Pre-rendering and batched Suspense reveals in server rendering.


## Key Takeaways


- React 19 launched December 5, 2024 and is the current major version. 19.2.7 from June 2026 is the latest patch; no React 20 is announced.
- Actions are the organizing idea of the release;` useActionState` ,` useFormStatus` and` useOptimistic` are three views onto the same mechanism.
- The` use` API can be called conditionally, but promises passed to it must be cached outside render or the Suspense fallback never clears.
- ` ref` is now an ordinary prop.` forwardRef` still works and is documented as headed for deprecation, not removed.
- The React Compiler is stable at 1.0 since October 2025, works back to React 17, and replaces most manual memoization.
- Server Components are stable, but their bundler-facing APIs sit outside semver, and RSC users should be on 19.0.4, 19.1.5 or 19.2.4 at minimum.
- ` <ViewTransition>` and Fragment Refs are still Canary-only. Treat any article that calls them shipped as out of date.


## Sources


- React. "React v19." 2024.[https://react.dev/blog/2024/12/05/react-19](https://react.dev/blog/2024/12/05/react-19?ref=scrimba.com)
- React. "React 19 Upgrade Guide." 2024.[https://react.dev/blog/2024/04/25/react-19-upgrade-guide](https://react.dev/blog/2024/04/25/react-19-upgrade-guide?ref=scrimba.com)
- React. "React 19.2." 2025.[https://react.dev/blog/2025/10/01/react-19-2](https://react.dev/blog/2025/10/01/react-19-2?ref=scrimba.com)
- React. "React Compiler v1.0." 2025.[https://react.dev/blog/2025/10/07/react-compiler-1](https://react.dev/blog/2025/10/07/react-compiler-1?ref=scrimba.com)
- React. "React Versions."[https://react.dev/versions](https://react.dev/versions?ref=scrimba.com)
- React. "React Server Components security advisories." 2025.[https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components](https://react.dev/blog/2025/12/11/denial-of-service-and-source-code-exposure-in-react-server-components?ref=scrimba.com)
- facebook/react. "Release 19.1.0." 2025.[https://github.com/facebook/react/releases/tag/v19.1.0](https://github.com/facebook/react/releases/tag/v19.1.0?ref=scrimba.com)
- facebook/react. Issue #29898, "Disabling prerendering siblings of suspended components."[https://github.com/facebook/react/issues/29898](https://github.com/facebook/react/issues/29898?ref=scrimba.com)
- Scrimba. Course data, self-reported, accessed July 2026.
