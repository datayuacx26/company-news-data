---
schema_version: "1.0.0"
document_id: "b08bb5302281ed1f4e378f628b643a616b2012125f315d656c6395b21a320190"
company_key: "cargurus-inc-class-a-common-stock"
company: "CarGurus Inc."
source_id: "cargurus-inc-class-a-common-stock-rss-ea326c83890f"
canonical_url: "https://cargurus.dev/2024/02/22/data-loading-patterns-in-remix-applications/"
published_at: "2024-02-22T17:00:09+00:00"
first_seen_at: "2026-07-20T03:33:13.583792+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:b537b456188739b6376ac036d4d34c1635d241257e790b23dec217041de6e30a"
---

# Data Loading Patterns in Remix Applications

## Introduction: Understanding the Value of Server-Side Rendering (SSR)


Server-side rendering (SSR) applications like Next.js and Remix play a pivotal role in delivering substantial value to both CarGurus customers and the company itself. These frameworks offer the ability to present web pages seamlessly on low-performance devices and in scenarios where JavaScript is disabled. Notably, SSR also contributes to improved search engine rankings, as crawlers can efficiently process server-generated pages containing all content—a notable distinction from client-side rendering (CSR). Frameworks such as Remix and Next.js offer several advantages. The initial rendering occurs on the server, providing the browser with a fully-loaded HTML page. Subsequently, as users engage with the content, the browser receives and executes the JavaScript to hydrate the page, enabling client-side rendering. This approach offers developers the flexibility to implement progressive enhancement, thereby providing an effective fallback to SSR if JavaScript is unavailable. While SSR offers substantial benefits, challenges arise when rendering a fully-formed HTML page becomes time-consuming. For instance, a server may need to make intricate requests to third-party services, leading to a delay in response time and a less-than-ideal user experience, characterized by an empty browser tab accompanied by a loading spinner. In this article, we will delve into several techniques employed at CarGurus to address and mitigate the challenges posed by time-consuming requests during server-side rendering. Our focus is on enhancing the overall user experience by optimizing the performance of SSR applications.


## Exploring SSR Frameworks: Next.js and Remix


We commence our exploration with a fundamental Remix application. Following the creation of the project and the addition of minimal functionality for page navigation, we have our Application, Home, and About pages which all respond promptly. However, a notable delay is observed when loading the Profile page, prompting us to explore strategies for improving its performance. By implementing these optimizations, we aim to ensure that CarGurus continues to deliver a seamless and efficient user experience, even in scenarios where SSR may face challenges in instantaneously rendering fully-formed HTML pages.


[Initial branch](https://github.com/MMilovanovCG/remix-data-loading-paterns/tree/initial_state_long_profile_request) and[Profile page](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/initial_state_long_profile_request/app/routes/_.profile.tsx)


Let’s begin by simulating a lengthy backend request to get user details:


```text
import { useLoaderData } from "@remix-run/react";


export async function loader() {
return new Promise((resolve) => {
setTimeout(() => resolve("Profile"), 3000); // represents long request
});
}


export default function Profile() {
const data = useLoaderData();
return data;
}
```


While a three-second delay may not seem significant on its own, particularly considering that API calls can often take longer, it can nonetheless have a notable impact on the overall user experience. In the absence of immediate feedback following the user’s selection of the Profile page, there is a potential for frustration or confusion to arise. To illustrate this point, please refer to the screen recording provided below:


At this point we all understand the problem, let’s talk about possible solutions.


The initial solution focuses on the moment a user initiates navigation from the Home page to the Profile. At this point we can provide the user some feedback like “We are processing your request, and the page will be available shortly” can reassure the user. Let’s examine the code of the Home page to implement this feature:[Leaving home page example](https://github.com/MMilovanovCG/remix-data-loading-paterns/tree/ssr_loading_state) and[Home page changes](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/ssr_loading_state/app/routes/_._index.tsx)


```text
import { useNavigation } from "@remix-run/react";


export default function Home() {
const nav = useNavigation();
const isLoading = nav.state === 'loading';
const content = isLoading ? 'Loading...' : "Home page";
return content;
}
```


We utilize the` useNavigation` hook to manage navigation states. When the application state is loading we return` Loading...` as page content. In this example, a user can see something happening while they are waiting for their profile to load. The only problem with this approach is that we will have to add this code snippet to every page where we expect a user can go to a Profile page and it won’t help us when we navigate directly to a Profile page:


Here we can see noticeable delay if we go straight to profile view:


Another option is to switch to CSR. This involves initially returning a partially rendered page with quickly available data, followed by fetching additional data that may require more time to load. This approach combines the advantages of both rendering methods. Initially, we return the page with data necessary for rendering everything above the fold, keeping the user engaged. Then, we fetch additional data that will appear below the fold, significantly improving our Largest Contentful Paint (LCP). Below is an example illustrating this concept. Please note that while the example isn’t presented below the fold, it provides insight into its functionality and potential usefulness. Let’s examine the code implementation:


[CSR with resource route](https://github.com/MMilovanovCG/remix-data-loading-paterns/tree/csr_resource_loading) ,[Profile page](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/csr_resource_loading/app/routes/_.profile.tsx) and[Profile resource endpoint](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/csr_resource_loading/app/routes/api.profile.tsx)


```text
// profile resource
const getUserDetails = () => new Promise((resolve) => {
setTimeout(() => resolve("User details"), 3000);
});


export async function loader() {
return await getUserDetails();
}
```


```text
// profile page
import { useFetcher, useLoaderData } from "@remix-run/react";
import { ReactNode, useEffect } from "react";


export async function loader() {
return {
mainPageContent: "Profile",
};
}


export default function Profile() {
const { mainPageContent } = useLoaderData<typeof loader>();
const fetcher = useFetcher();


useEffect(() => {
fetcher.load('/api/profile')
}, []);


return <div>
<div>
{mainPageContent}
</div>
<div>
{fetcher.data ? fetcher.data as ReactNode : <div>Loading...</div>}
</div>
</div >;
}
```


Here we return the page with a profile header and request all the data for the user profile after the page gets rendered in the browser.` useEffect` won’t work on the server side, it will be triggered only when React runs in the browser, then it will send the request to the resource route to get actual user data. When the data is available, React will render it on the client side, opening a Profile page from the blank. Navigating from the Home page will work the same:


A third option closely resembles the second one, but without a separate resource route:[Async and defer branch](https://github.com/MMilovanovCG/remix-data-loading-paterns/tree/async_data_loading) and[Profile page](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/async_data_loading/app/routes/_.profile.tsx)


```text
import { Await, useAsyncValue, useLoaderData } from "@remix-run/react";
import { defer } from "@remix-run/node";
import { Suspense } from "react";


const getUserDetails = () => new Promise((resolve) => {
setTimeout(() => resolve("User details"), 3000);
});


export async function loader() {
return defer({
deferedData: getUserDetails(),
mainPageContent: "Profile",
});
}


const UnderTheFoldContent = () => {
const resolvedValue = useAsyncValue();
return <>{resolvedValue}</>;
};


export default function Profile() {
const { deferedData, mainPageContent } = useLoaderData<typeof loader>();


return <div>
<div>
{mainPageContent}
</div>
<div>
<Suspense fallback={<div>Loading...</div>}>
<Await resolve={deferedData}>
<UnderTheFoldContent />
</Await>
</Suspense>
</div>
</div >;
}
```


Several changes have been implemented. First, our loading function now returns a deferred object instead of a resolved promise. This object includes one property containing already resolved content, which we can render immediately, and a second property representing an unresolved promise.


Next, we introduce a` Suspense` section on the page, wrapping the` Await` component. While our promise remains unresolved, we render a fallback state. Once the promise is resolved, we render the` UnderTheFoldContent` component, which accesses the resolved value using the` useAsyncValue` hook.


The resulting application will function as follows:


## Addressing Slow API Responses: Workarounds and Solutions


As observed, the initial portion of the page loads immediately. Subsequently, as the remaining content becomes available, we update only the loading section. Both the second and third options will work only when Javascript is enabled in the browser In our last demo there’s a notable issue: when we navigate to the Profile page, load data (which takes 3 seconds), then return to the Home page and attempt to reopen the Profile page, the user is forced to wait an additional 3 seconds to retrieve profile details. This redundancy in loading time is undesirable. However, if our data doesn’t change frequently, we can explore caching solutions. We have various options at our disposal, including HTTP caching, localStorage, sessionStorage, indexedDB, and memory cache. Let’s begin by exploring HTTP caching:[Http cache branch](https://github.com/MMilovanovCG/remix-data-loading-paterns/tree/http_cache) and[Profile page](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/http_cache/app/routes/_.profile.tsx)


the only change on profile page was to add http headers:


```text
export async function loader() {
return defer({
deferedData: getUserDetails(),
mainPageContent: "Profile",
}, {
headers: {
"Cache-Control": "public, max-age=3600",
},
});
}
```


Enabling HTTP caching instructs the browser to cache the response from this page for an hour. Consequently, the next time the user visits the page, the browser won’t need to make a request to our server at all. Let’s observe a demonstration with both disabled and enabled browser cache to understand the impact:


Another option is to utilize memory or any other storage mechanism as a cache. Let’s explore an example of this approach:[Memory cache](https://github.com/MMilovanovCG/remix-data-loading-paterns/tree/memory_cache) and our[Profile page](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/memory_cache/app/routes/_.profile.tsx)


Here, we utilize the` clientLoader` and a local variable as our cache:


```text
let response: null | typeof loader | {} = null;
export async function clientLoader({serverLoader}: ClientLoaderFunctionArgs) {
if (!response) {
response = await serverLoader();
}
return response;
}


clientLoader.hydrate = true;
```


If the router file contains a` clientLoader` function, Remix will invoke it instead of the` loader` . One of the arguments provided to this function is` serverLoader` . Here, we check if we already have a response from the server. If so, we don’t need to wait and can simply return the response. Otherwise, we must wait for the backend first.


Setting` clientLoader.hydrate = true;` means that the` clientLoader` will be called on the first page load. If not set, the` clientLoader` will be invoked only on the second page load.


As a result of this change we have the following demo: even with the browser cache disabled, we can retrieve our Profile page pretty quickly.


## Enhancing User Experience with Prefetching and Client-Side Rendering (CSR)


In our last example, we discussed utilizing local storage, session storage, or IndexedDB as alternatives to the local variable for caching purposes. To simplify this process, we can leverage the capabilities of the` localForage` library, which provides a unified interface to work with all these storage APIs:[localforage](https://github.com/localForage/localForage)


There are instances where we can anticipate a user’s actions. If we know that our user will likely visit the Profile page, we can use a React Router feature to prefetch and cache content in advance. React Router provides us following strategies for prefetching:


```text
* - "intent": Fetched when the user focuses or hovers the link
* - "render": Fetched when the link is rendered
* - "viewport": Fetched when the link is in the viewport
```


For our demo app, there is no difference between viewport and render, but we reasonably anticipate that a user will navigate there. Therefore, for simplicity we’ll use` render` , and see how it works:[Prefetch branch](https://github.com/MMilovanovCG/remix-data-loading-paterns/tree/prefetch) changes in our[NavBar](https://github.com/MMilovanovCG/remix-data-loading-paterns/blob/prefetch/app/components/Navbar.tsx#L18) component:


```text
<NavLink to="/profile" className={linkStyle} prefetch="render">
Profile
</NavLink>
```


We just added` prefetch` attribute, let’s see the demo:


## Conclusion: Harnessing the Flexibility of Remix for Better SSR Performance


As evident from the demonstration, upon page refresh, the browser promptly sends a profile request. Although it still takes some time to retrieve the data, the user is occupied with other activities in the meantime. Consequently, by the time the user navigates to the Profile page, all the necessary data is readily available in the prefetch cache.


In summary, we explored various strategies to optimize server-side rendering (SSR) applications, focusing on enhancing the user experience. We discussed the benefits of SSR frameworks like Next.js and Remix, which enable rendering both on the server and client-side, improving page load times and search engine rankings. We investigated techniques such as prefetching and caching, utilizing options like HTTP caching, local storage, and libraries like localForage. Additionally, we explored React Router’s prefetching strategies to anticipate user actions and improve performance.


It’s important to note that there’s no one-size-fits-all solution to address slow API responses. However, with Remix’s flexibility, we have a range of options available to work around such challenges and optimize the user experience. Here’s to creating faster, more efficient, and user-friendly web experiences. Happy coding!
