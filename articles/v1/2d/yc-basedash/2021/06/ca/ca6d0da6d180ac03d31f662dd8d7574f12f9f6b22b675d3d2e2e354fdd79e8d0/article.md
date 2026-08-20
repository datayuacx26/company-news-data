---
schema_version: "1.0.0"
document_id: "ca6d0da6d180ac03d31f662dd8d7574f12f9f6b22b675d3d2e2e354fdd79e8d0"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/optimizing-rest-api-calls/"
published_at: "2021-06-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:1fafb7200a30a4cb938ef1dc11e525a0399bc69dfdfba1c078551b7440836e3a"
---

# Optimizing REST API calls

Recently, we refactored our codebase at Basedash to fetch our server data with React Query and optimize our REST API calls in the process. The transition to React Query allowed for better code readability, and the optimization of our API calls resulted in half the number of data-fetching API calls and a 3x reduction in the amount of data loaded on initial page load.


This post describes what prompted the move to React Query and the optimizations that were made to our REST API calls and routes in the process.


## Problems with the current data-fetching logic


Our data-fetching logic was hard to follow and we had a lot of bugs because of this. We were using Redux and Redux thunks to coordinate data fetching and storing data in our Redux store. The following pattern was commonly used to fetch data:


- On component load, dispatch a thunk action in a` useEffect` to trigger data fetching.
- The thunk dispatches an action indicating that the request has been initiated.
- A reducer updates the store with a loading state in response to that action.
- Components that should show loading indicators use` useSelector` and render a spinner accordingly.
- The thunk makes the API call.
- The thunk dispatches an action for success or failure.
- A reducer updates the store with API data (or error data) and resets loading to false.
- Components that depend on the API data use` useSelector` and update accordingly.


💡 If you follow the above pattern, check out` createAsyncThunk` from Redux Toolkit. It dispatches` pending` ,` fulfilled` , and` rejected` actions for you. You only need to write the data-fetching and reject logic.


Things got more complicated if some API calls needed to happen before others. We were also making a lot of API calls on initial page load to get all the data a page needed, but in many cases it did not make sense to keep those calls split up.


We decided to take a shot at using React Query for our data-fetching needs since it seemed to have a nice API for data fetching, and we had been hearing good things about how React Query makes it easy to keep server data in sync on the client side.


While migrating to React Query, we also decided to optimize the number of API calls we made and prevent sending unnecessary data from the server when it was not needed for the UI.


## Analyzing current state of affairs


Refactoring to React Query started by analyzing the API calls for existing pages and defining what the optimal data-fetching flow should be.


For one Basedash page, we had 23 data-fetching API calls. Some of those calls requested data that the page UI did not require (for example, billing information and user activities used elsewhere). Some of this data was saved in our normalized Redux store, which we could leverage to save API calls later when that data was needed.


One table view was built from four separate API calls:


- ` columns` :` GET` request to fetch all columns for the table
- ` foreign-keys` :` GET` request to fetch all foreign keys for the table
- ` enum-values` :` GET` request to fetch enum values for enum-type columns
- ` records` :` POST` request to fetch table records


We found that the first three calls could be combined. We reworked the API so the table data was fetched through:


- ` table` :` GET` request to fetch columns, foreign keys, and enum values
- ` records` :` POST` request to fetch table records


Following this same process, we combined routes that could logically live together. We also mapped which API calls were needed on which pages so we could avoid fetching data that was not necessary for the current page.


ℹ It is not always a bad idea to fetch data not used on the current page if you plan to cache it and avoid API calls later. This is especially true for data users are highly likely to request during their session. You can use` React.lazy` or` react-loadable` to preload pages and components.


Also look out for the` prefers-reduced-data` media query, which can help you decide whether to preload data while still respecting a user’s preference for reduced data usage.


## React Query structure


Most of our React Query code uses the` useQuery` and` useMutation` hooks. Since those hooks are reused across many components, we created custom hooks that wrap` useQuery` and` useMutation` and strongly type the params, options, errors, and data.


Here is an example custom hook:


```text
export   const   useApiTable   =   (
params  :   FetchTableParams  ,
options  ?:   UseQueryOptions  <
FetchTableResponse  ,
ApiError  ,
FetchTableResponse  ,
[  string  ,   FetchTableParams  ]
>
)   =>
useQuery  <
FetchTableResponse,
ApiError,
FetchTableResponse,
[string, FetchTableParams]
>  (
[  "table"  , params],
async   ({   queryKey   })   =>   {
const   [  _key  ,   params  ]   =   queryKey;
const   response   =   await   fetchTable  (params);
if   (  !  response.ok) {
if   (response.status   ===   400  ) {
throw   new   ApiError  (  await   response.  json  ());
}
throw   new   ApiError  (  "Network response was not ok"  );
}
return   response.  json  ();
},
options
);
```


In some cases, we also use` queryClient.fetchQuery` to fetch queries. For those cases, we sometimes extract the query function so it can be reused:


```text
export   const   useApiTableQueryFunction  :   QueryFunction  <
FetchTableResponse  ,
[  string  ,   FetchTableParams  ]
>   =   async   ({   queryKey   })   =>   {
const   [  _key  ,   params  ]   =   queryKey;
const   response   =   await   fetchTable  (params);
if   (  !  response.ok) {
if   (response.status   ===   400  ) {
throw   new   ApiError  (  await   response.  json  ());
}
throw   new   ApiError  (  "Network response was not ok"  );
}
return   response.  json  ();
};


export   const   useApiTable   =   (
params  :   FetchTableParams  ,
options  ?:   UseQueryOptions  <
FetchTableResponse  ,
ApiError  ,
FetchTableResponse  ,
[  string  ,   FetchTableParams  ]
>
)   =>
useQuery  <
FetchTableResponse,
ApiError,
FetchTableResponse,
[string, FetchTableParams]
>  ([  "table"  , params], useApiTableQueryFunction, options);
```


The data-fetching flow with React Query looks like this:


- Create a custom hook using` useQuery` with a query key and query function.
- Use the custom hook in components.
- Read` loading` while the request is in progress.
- Read` data` after success, and` error` if the request fails.
- Use` onSuccess` and` onError` callbacks for side effects after queries and mutations.


React Query also makes it easy to do useful things like retrying failed calls, refetching when users refocus the window, query cancellation, and more.


When updating API data with mutations, we often perform optimistic updates via` queryClient.setQueryData` in` onMutate` , so the UI updates instantly before the request completes. If the API call fails, we revert in` onError` .


In other cases, we do not patch query data manually and instead invalidate queries for refetching. A practical rule of thumb for us is:


- Patch query data when it is currently visible in the UI.
- Invalidate queries for data used on pages that are not currently visible.


That gives us instant UI updates on the current page while avoiding lots of manual cache patching for off-screen data.


## Beware of combining too much data into one API route


We hit an issue with an API call we had refactored to return a large amount of data needed on initial page load (for example, all sidebar items). A bug in a subset of that server logic caused the call to take 40+ seconds because of a timeout/retry mechanism.


That meant users saw a loading screen for 40+ seconds because one combined API response could not fully resolve.


The more data you move into one API call, the more points of failure you introduce for that call, which is risky when a large part of your UI depends on it.


Error handling is also less clear: it becomes harder to tell which part of a large response caused failure, and the client may need to parse complex error structures to render useful UI errors.


When calls are split more thoughtfully, you are in a better position to show partial UI and specific error messages for only the sections that fail.


Another benefit (especially with React Query) is more precise invalidation. Smaller, focused routes make it easier to invalidate the right query and reduce overfetching.


ℹ GraphQL APIs are also useful in this context because they allow more precise field selection in a single request.


## Normalized cache and overfetching data


With Redux, you can keep a normalized cache where entities are stored without duplication and referenced from one source of truth.


For example, in a Twitter-like app that shows tweet lists and tweet detail pages, a normalized cache might look like:


```text
const   store   =   {
tweets: {
ids: [  1  ,   2  ,   3  ],
entities: {
1  : { message:   "Hello world"  , replyCount:   8  , likes:   30   },
2  : { message:   "Goodbye world"  , replyCount:   12  , likes:   28   },
3  : { message:   "YOLO"  , replyCount:   32  , likes:   1003   },
},
},
};
```


If a user opens tweet` 1` , likes it, and increments from` 30` to` 31` , the same entity reference updates across views:


```text
const   store   =   {
tweets: {
ids: [  1  ,   2  ,   3  ],
entities: {
1  : { message:   "Hello world"  , replyCount:   8  , likes:   31   },
2  : { message:   "Goodbye world"  , replyCount:   12  , likes:   28   },
3  : { message:   "YOLO"  , replyCount:   32  , likes:   1003   },
},
},
};
```


Because the data has one canonical entity reference, the like count updates instantly in all relevant UI without refetching.


With React Query, you do not get normalized caching by default. So you either:


- Invalidate all related queries after updates, or
- Manually patch all references to that entity across query results.


Invalidating many queries can cause overfetching, but it also guarantees client data stays in sync with server data and avoids reimplementing complex server-side rules in the client cache layer.
