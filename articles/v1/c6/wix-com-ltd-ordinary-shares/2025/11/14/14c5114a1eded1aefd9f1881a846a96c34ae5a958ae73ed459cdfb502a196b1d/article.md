---
schema_version: "1.0.0"
document_id: "14c5114a1eded1aefd9f1881a846a96c34ae5a958ae73ed459cdfb502a196b1d"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/40-faster-interaction-how-wix-solved-react-s-hydration-problem-with-selective-hydration-and-suspen"
published_at: "2025-11-09T12:58:45+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:462226bba12b320cffd56e27c2247bf1083b64a4d3f340cd1b2642b499827f46"
---

# 40% Faster Interaction: How Wix Solved React's Hydration Problem with Selective Hydration and Suspense

###


**Introduction**


For years, one of React’s most persistent performance pain points has been hydration.


At Wix, where we render hundreds of millions of websites using React and SSR, we’ve experienced this problem at scale. SSR initially gave us everything we wanted - instant paint, great SEO, and full-site CDN caching. Our pages loaded faster than ever.


But as sites grew richer and heavier with content, hydration became a tax we could no longer ignore. The more components a page had, the longer it took to become interactive. Time-to-Interactive kept climbing, especially on mobile devices with slow networks and weak CPUs. We didn’t want to give up SSR - but the hydration cost was holding us back.


And it wasn’t just theoretical: our users felt it. Our Interaction to Next Paint (INP) score - a Core Web Vitals metric from Google that measures responsiveness in the real world - was the lowest among all our competitors. Less than 40% of Wix sites achieved a “good” INP score.


As the Performance Architect at Wix, improving hydration performance became one of my top priorities. We needed a way to keep all the benefits of SSR - without paying the hydration tax.


Then React 18 arrived with a quiet but powerful change: the new Suspense API. It opened the door to something we’d been dreaming about for years - Selective Hydration - a technique that would later help us dramatically improve our INP scores.


###


**The Challenge of Hydration**


To understand why hydration matters - and why it can be so painful - let’s go back to the basics.


Server-Side Rendering (SSR) means React renders HTML on the server and sends it to the browser. It’s great: users see meaningful content instantly, crawlers love it, and entire HTML responses can be cached at the CDN edge. The result is a fast first paint and excellent SEO.


Our Largest Contentful Paint (LCP) - the metric that measures how quickly meaningful content appears - was (and still is) among the best in the industry.


But then comes hydration. On the client side, the browser must download all the JavaScript bundles, then hydrate every React component that was pre-rendered on the server - reconnecting the static HTML to the React tree and attaching all the event listeners. Hydration is what turns a server-rendered DOM into an interactive React app - but it comes at a cost: JavaScript - both download and execution.


On small pages, this cost is negligible. But on large, content-heavy pages - with dozens or hundreds of components - the browser struggles under the weight of JavaScript. Even with the concurrent rendering that was introduced in React 18, too much JS still leads to sluggish interactions and poor responsiveness.


That’s the hidden cost of SSR and hydration: it gives you a fast paint, but delays interactivity.


###


**Could We Keep SSR Without Paying the Hydration Tax?**


That was the question that drove our team for years.


If users only interact with the visible portion of a page - why hydrate everything below the fold? Why spend CPU cycles and network bandwidth on components the user hasn’t even scrolled to yet, and can’t interact with?


Until React 18, React didn’t let you do that. If you skipped hydration for even a single component, React would throw a hydration mismatch. The result was often unexpected - React could remount the entire component on the client, causing extra work and poor performance.


This happened because during the hydration process React expected the server-rendered markup and client tree to match exactly, so any missing node broke the whole process. Hydration was all or nothing.


Example: the classic “Hydration failed because the initial UI does not match what was rendered on the server” error developers know all too well.


This limitation was finally lifted with the introduction of the new Suspense API in React 18.


###


**Enter Suspense: From Lazy Loading to Selective Hydration**


When Suspense first appeared in React 16, it was a simple idea - a wrapper that lets you show a fallback while a component loads asynchronously.


It was mainly used for code-splitting with React.lazy(), allowing components to load on demand and reduce the size of the main bundle. Useful, yes - but limited.


```text
// React 16 – Suspense for lazy loading
const     Profile = React.lazy(() =>    import    (   './Profile'    ));


export default function     App() {
return     (
<Suspense fallback={<Spinner />}>
<Profile />
</Suspense>
);
}
```


SSR support was minimal - the server would always render the fallback instead of the actual component, which made it impractical for real-world pages.


Then came React 18, and everything changed.


Suspense evolved into a promise-driven API that works seamlessly across client and server. It can pause rendering until data is ready - or until any asynchronous operation completes.


And that was the key insight: If Suspense can pause rendering for async data, why not use the same mechanism to pause hydration itself?


Instead of hydrating every component right away, we could hydrate them only when needed - when they actually enter the viewport or when the user is likely to interact with them.


Although it was still an experimental API in React 18, we had to try it - and it worked perfectly.


###


**Selective Hydration in Practice**


Here's how it works inside Wix’s rendering engine:


1.


**Server-side render** : Every component is rendered on the server - the full page is visible instantly thanks to SSR.


2.


**Suspense wrapping** : Each component is wrapped in a Suspense boundary and lazy-loaded on the client with React.Lazy (meaning the component is bundled separately from the main bundle).


3.


**Paused hydration (client only)** : When the app boots on the client, hydration for these components is intentionally paused. Their wrapper


*throws* a Promise inside the Suspense boundary - and Suspense catches it, effectively deferring hydration.


4.


**Viewport detection** : An Intersection Observer tracks when each component enters the viewport.


5.


**Selective Hydration** : When a component becomes visible, the Promise resolves - React (The Suspense) resumes rendering, downloads the component, hydrates the component, and it becomes interactive.


In other words, components below the fold cost zero JavaScript and zero CPU until the user scrolls to them.


And the best part: React no longer throws hydration mismatches - because we render the same Suspense boundaries on both server and client, only changing when they resolve.


The React tree remains identical across environments, so hydration works flawlessly. You might be wondering - do we really need to


**throw** a Promise?


It’s strange, and you’re right to think so. Throwing anything other than errors isn’t normal JavaScript behavior. In this case, It’s a React-specific mechanism: when a component throws a Promise inside a Suspense boundary, the Suspense catches it and treats it as a signal to pause rendering until the Promise resolves.


This pattern - used internally by frameworks like Next.js and Remix - is unofficial and undocumented in React but has been the only way to suspend rendering (and hydration) up until React 19, which we’ll discuss later.


This code example shows a wrapper that use Suspense to render children only when they enter the viewport (very similar to what we use in production):


```text
import     { Suspense, useRef, useMemo } from    "react"    ;
import     { useEffect } from    "react"    ;
import     { createPromise } from    "./createPromise"    ;
import     { createIntersectionObserver } from    "./createIntersectionObserver"    ;


const     isServer =    typeof     window ===    "undefined"    ;


function     SuspenseInner({ children, promise }) {
// if the promise is not fulfilled, suspend the component (only on the client side)
if     (!isServer && !promise.fulfilled) {
throw     promise;    // this will be caugth by the suspense and pause the rendering until its resolved
}
return     children;    // this will render the children
}


export function     SuspenseWrapper({ children }) {
// create a ref to the div that will be used to observe the intersection
const     ref = useRef(null);
// create a promise (from a utility) that will be used to suspend the component
const     { promise, resolve } = useMemo(() => createPromise(), [])


useEffect(() => {
// create the intersection observer (from a utility) and resolve the promise when the component is in the viewport
createIntersectionObserver(ref.current, () => resolve())
}, []);


// return the div with the ref and the suspense component
return     (
<div ref={ref}>
<Suspense>
{   /* SuspenseInner is a component that will be used to suspend the children inside the Susepense Boundry */    }
<SuspenseInner promise={promise}>
{children}
</SuspenseInner>
</Suspense>
</div>
);
}
```


Notice that we created the


**SuspenseInner** component because we must


*throw* the promise inside the suspense boundary, and we wanted to keep the children clean and unaware of the wrapper, so any component will work inside this generic wrapper.


You can see the a full code example here that demonstrates how the selective hydration works in this repo:


[https://github.com/gileck/react-selective-hydration-demo](https://github.com/gileck/react-selective-hydration-demo)


###


**The Impact: Less JS, Faster Interaction**


Rolling this out across Wix sites produced a massive impact in real-world metrics. We reduced JavaScript payloads by


**20%** and improved


[INP](https://web.dev/articles/inp) **(Interaction to Next Paint)** - by roughly


**40%** .


While Selective Hydration was a major driver of these gains, we also improved INP through


**main-thread optimizations** - reducing long tasks, yielding during heavy work, and scheduling non-critical scripts more intelligently.


These techniques complement selective hydration and together contributed to a significant improvement in responsiveness - as shown in the graph below. For a deeper dive into INP optimization strategies, see


[Optimize Interaction to Next Paint](https://web.dev/articles/optimize-inp) on


[web.dev](http://web.dev/) .


*


**Figure:** Data from Google’s


[Core Web Vitals Technology Report](https://lookerstudio.google.com/u/0/reporting/55bc8fad-44c2-4280-aa0b-5f3f0cd3d2be/page/M6ZPC) , showing the share of origins with


*good*


**INP (Interaction to Next Paint)** scores over time.


And these aren’t synthetic lab numbers. This comes from field data across hundreds of millions of real user sessions.


The result: Wix sites now not only load faster, but also respond to user interactions faster than ever before.


###


**Why Not Just Use Server Components?**


A fair question we get a lot:


*If Server Components can reduce JavaScript, why not just use those?*


React Server Components (RSC) are great in theory - but in practice, they’re restrictive. Server Components must be static, can’t use hooks or context, which make them difficult to integrate into a large codebase.


Suspense, on the other hand, gives us dynamic control. We can defer hydration at runtime, while keeping components fully interactive when needed. And the best part - the components themselves don’t need to know they’re wrapped in Suspense. That isolation gives us enormous flexibility.


To adopt selective hydration, we didn’t have to migrate our components to a new model. Aside from a few edge cases, it was completely seamless. With RSC, many of those same components would need to be rewritten to meet RSC constraints - an impractical effort for a platform with hundreds of interactive components like Wix.


###


**Looking Ahead: React 19 and Beyond**


Everything described above was built on React 18. But React 19 (that was just released in a stable version) makes this pattern even cleaner, thanks to the new use() API.


Instead of manually throwing Promises to pause hydration, React can now natively await asynchronous values - whether that’s fetching data, an Intersection Observer trigger, or any asynchronous operation.


The idea remains the same: hydrate only when needed - but now it’s officially part of React’s core.


```text
function     SuspenseInner({ children, promise }) {
// instead of throwing the promise, we can use the use hook to pause the rendering
use(!isServer ? promise : Promise.resolve())
return     children;
}
```


The same code that we saw before will work in React 19, but now we can also use the


***use*** API instead of throwing the promise.


One important detail: the use() API must be called inside a Suspense boundary. If it’s called outside, React will throw an error, since Suspense is the mechanism that handles the asynchronous pause and resume cycle.


###


**Key Considerations for Effective Selective Hydration**


Implementing Selective Hydration in a large-scale React application comes with a few important details that can significantly impact performance and reliability.


####


**1. Prefetching Visible Components**


Above-the-fold components (like headers, hero sections, or top navigation) should have their client bundles prefetched directly from the HTML.


This is crucial because the Intersection Observer doesn’t start observing until React, the main app script, and the initial render are all loaded - a startup gap that can easily be hundreds of milliseconds, especially on mobile.


By prefetching, we ensure these critical components hydrate immediately once the app boots, avoiding visible interactivity delays for top-of-page elements.


If possible, drive this from your SSR pipeline or CDN edge, using a per-route manifest to decide which components are above the fold, and consider HTTP 103 Early Hints to start fetching even before the HTML response.


####


**2. Avoid Observer Overload**


When using many components with Intersection Observers, prefer attaching observers to


**container elements** rather than every small component. This reduces observer setup cost and improves page-load performance by preventing the browser from creating and managing hundreds of observation targets at once.


However, this comes with a tradeoff: More observers generally give the best scroll responsiveness (since hydration triggers at the exact component level), while fewer, broader observers improve initial page-load performance but may slightly delay hydration as the user scrolls.


The sweet spot is usually somewhere in between - splitting the page into logical containers that divide the layout evenly.


This keeps the observer overhead low while maintaining smooth, predictable hydration timing during scroll. And if you’re testing for INP in production, you can always experiment with different combinations and find the right balance - just like we did.


####


**3. Match Suspense Boundaries on Server and Client**


Hydration mismatches can occur if the Suspense tree isn’t identical on both sides. Ensure that the


**same Suspense boundaries** are rendered in both environments - even if the client defers their resolution.


###


**What’s Next: Taking Selective Hydration Even Further**


Our work on Selective Hydration didn’t stop with the initial rollout.


We’re continuing to explore ways to make hydration even faster, smarter, and more adaptive to real-world user behavior.


####


**1. Zero-JavaScript Components**


Some UI (e.g., Text, Image, other static visuals) never needs hydration. We’re expanding pure-HTML rendering with zero client JS for these cases. It’s philosophically similar to Server Components, but without their constraints - a better fit for a large, mature codebase.


The implementation is nearly identical to the earlier pattern (even simpler) - instead of resolving the promise when the element enters the viewport, we simply never resolve it, so the component never downloads or hydrates, behaving just like a Server Component.


####


**2. Priority and Interaction-Driven Hydration**


Viewport entry is just one way to trigger hydration. We can extend the same mechanism to other user interactions - like hover, focus, or click - or even to priority-based scheduling, where critical components hydrate first and low-priority ones wait for idle time.


[It](http://time.it/) ’s the same implementation pattern: each component suspends behind a promise, but that promise is resolved based on user intent or priority, giving us fine-grained, context-aware control over when hydration happens.


Looking ahead, this approach could evolve even further - with AI models predicting user behavior based on interaction patterns, dynamically deciding which components to hydrate next to optimize responsiveness in real time.


####


**3. Tuning Observer Thresholds**


We’re experimenting with different Intersection Observer thresholds to fine-tune when hydration starts as the user scrolls. With a small threshold, page-load interactivity is optimal - but components that enter the viewport may take slightly longer to download and hydrate, sometimes feeling laggy.


With a larger threshold, hydration starts earlier, improving perceived responsiveness at the cost of more upfront JavaScript work. Finding the right threshold is about balancing load performance and scroll responsiveness for each layout type.


###


**The Takeaway**


Hydration has always been React’s hidden bottleneck - especially at the scale we operate at Wix.


By combining Suspense with runtime hydration control, we turned hydration from an “all-at-once” process into an on-demand one.


Components now hydrate only when needed, reducing JavaScript execution, improving responsiveness, and


**making interactions much faster for millions of Wix sites** .


The best part? We achieved it without giving up SSR, without refactoring any React component, and without changing how our developers build React apps. Selective Hydration proves that sometimes the biggest performance wins come not from new frameworks - but from using existing tools in smarter ways.


This post was written by


**Gil Eckstein**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) or


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA)
