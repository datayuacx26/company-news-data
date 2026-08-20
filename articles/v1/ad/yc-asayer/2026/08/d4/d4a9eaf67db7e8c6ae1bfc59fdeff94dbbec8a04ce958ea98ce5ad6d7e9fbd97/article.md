---
schema_version: "1.0.0"
document_id: "d4a9eaf67db7e8c6ae1bfc59fdeff94dbbec8a04ce958ea98ce5ad6d7e9fbd97"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/test-api-driven-components-storybook/"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T15:29:32.412738+00:00"
fetched_at: "2026-08-11T15:29:34.394512+00:00"
content_hash: "sha256:f460333f75f509ccfb9990711baa088c223513df020d93ff4bd710cfd7322632"
---

# Testing API-Driven Components in Storybook

A component that fetches data will hang on “Loading…” or throw in Storybook because there is no backend to answer its request — the fix is to intercept that request at the network layer with[Mock Service Worker](https://mswjs.io/) , not to stub the hook.


If you’ve ever watched a component sit on “Loading…” in Storybook with nothing in the console to explain it, this is the cause: no server is there to answer the request it fires on mount. Set the mock up once and every story you write gets the same treatment for free.


This guide sets up[msw-storybook-addon](https://github.com/mswjs/msw-storybook-addon) (which requires MSW 2.x) on Storybook 10, then builds a` UserList` component with four stories (success, loading, error, and empty) and closes the loop by turning those mocked states into automated interaction tests.


## Key Takeaways


- Mock at the network layer so one set of MSW handlers works unchanged in Storybook, in Node-based unit tests, and in Chromatic visual regression.
- In MSW v2, a success handler is` http.get(url, () => HttpResponse.json(data))` ; there is no more` rest.get` or the` (req, res, ctx) => res(ctx.json())` resolver signature.
- Model a permanent loading state by awaiting` delay('infinite')` , an error with` HttpResponse.json(null, { status: 500 })` , and an empty result by returning` HttpResponse.json(\[\])` .
- Wire the addon once by adding` mswLoader` to the` loaders` array in` .storybook/preview` , and serve the worker by setting` staticDirs` in your main config.
- Pair each mocked state with a` play` function so the state is asserted automatically. A story with a play function becomes a component test.


## Why do data-fetching components break in Storybook?


Storybook renders components in isolation, with no application shell and no server. An “app component” that calls` fetch` ,` useQuery` , or Apollo on mount fires a request that nothing answers, so it either sits on its loading branch forever or throws when the promise rejects. The instinct is to stub the hook: swap` useQuery` for a mock that returns canned data. Don’t. Stubbing the hook couples your story to one data library and to internal component structure, and the stub is useless the moment you want to run the same scenario in a Node test.


Mock at the network layer instead. MSW registers a service worker that intercepts outgoing requests in the browser and returns responses you define, so your component runs its real fetching code path unchanged. The same handlers run under Node via` setupServer` , which is what makes them portable across Storybook, Vitest, and CI. You author the scenario once; it works everywhere the component runs.


## How do you set up the Storybook mock API stack?


Install both packages, generate the service worker, register the loader, and point Storybook at the worker file. This one-time setup follows[Storybook’s guide to mocking network requests](https://storybook.js.org/docs/writing-stories/mocking-data-and-modules/mocking-network-requests) .


```text
npm   install   msw   msw-storybook-addon   --save-dev
npx   msw   init   public/
```


` npx msw init public/` writes` mockServiceWorker.js` into your static directory. Register the addon globally by adding` mswLoader` to` loaders` in` .storybook/preview.ts` . Loaders run *before* a story renders, which is why the addon uses a loader rather than a decorator:


```text
import   type   {   Preview   }   from   '  @storybook/react-vite  '  ;
import   {   initialize  ,   mswLoader   }   from   '  msw-storybook-addon  '  ;


initialize  ();


const   preview  :   Preview   =   {
loaders  :   [  mswLoader  ],
};


export default   preview  ;
```


Then serve the generated worker by listing your public folder in[staticDirs](https://storybook.js.org/docs/api/main-config/main-config-static-dirs) inside` .storybook/main.ts` . The old` start-storybook -s public` flag no longer exists in Storybook 10:


```text
import   type   {   StorybookConfig   }   from   '  @storybook/react-vite  '  ;


const   config  :   StorybookConfig   =   {
framework  :   '  @storybook/react-vite  '  ,
stories  :   [  '  ../src/**/*.stories.@(js|jsx|ts|tsx)  '  ],
staticDirs  :   [  '  ../public  '  ],
};


export default   config  ;
```


## The success story


Here is the component under test, a` UserList` that fetches an array and renders one of four UI branches. Note the accessible` role="status"` and` role="alert"` , which the tests later query against.


```text
// UserList.tsx
import   {   useEffect  ,   useState   }   from   '  react  '  ;


type   User   =   {   id  :   number  ;   name  :   string   };
const   endpoint   =   '  https://api.example.com/users  '  ;


export   function   UserList  () {
const   [  status  ,   setStatus  ]   =   useState  <  '  loading  '   |   '  success  '   |   '  error  '  >(  '  loading  '  );
const   [  users  ,   setUsers  ]   =   useState  <  User  []>([]);


useEffect  (()   =>   {
fetch  (  endpoint  )
.  then  ((  res  )   =>   {
if (  !  res  .  ok  )   throw   new   Error  (  res  .  statusText  );
return   res  .  json  ();
})
.  then  ((  data  )   =>   {
setUsers  (  data  );
setStatus  (  '  success  '  );
})
.  catch  (()   =>   setStatus  (  '  error  '  ));
}, []);


if (  status   ===   '  loading  '  )   return   <  p   role  =  "  status  "  >Loading…</  p  >  ;
if (  status   ===   '  error  '  )   return   <  p   role  =  "  alert  "  >Something went wrong.</  p  >  ;
if (  users  .  length   ===   0  )   return   <  p  >No users yet.</  p  >  ;


return   (
<  ul  >
{users  .  map  ((  u  )   =>   (
<  li   key  =  {u  .  id}>{u  .  name}</  li  >
))  }
</  ul  >
);
}
```


Set the handlers per story through` parameters.msw.handlers` . In MSW v2,` http.get` replaces` rest.get` , and the[HttpResponse](https://mswjs.io/docs/api/http-response) class replaces the` ctx` utilities:


```text
// UserList.stories.tsx
import   type   {   Meta  ,   StoryObj   }   from   '  @storybook/react-vite  '  ;
import   {   http  ,   HttpResponse  ,   delay   }   from   '  msw  '  ;
import   {   UserList   }   from   '  ./UserList  '  ;


const   endpoint   =   '  https://api.example.com/users  '  ;


const   meta   =   {   component  :   UserList   } satisfies   Meta  <  typeof   UserList  >;
export default   meta  ;
type   Story   =   StoryObj  <  typeof   meta  >;


export   const   Success  :   Story   =   {
parameters  :   {
msw  :   {
handlers  :   [
http  .  get  (  endpoint  ,   ()   =>
HttpResponse  .  json  ([
{   id  :   1  ,   name  :   '  Ada Lovelace  '   },
{   id  :   2  ,   name  :   '  Alan Turing  '   },
]),
),
],
},
},
};
```


## Modeling every state


The happy path is where most tutorials stop, and it’s the least interesting story. The value of network-layer mocking is that one handler swap produces every state your component can enter. Session replays of API-driven UIs routinely surface states developers never storyboarded: a spinner that never resolves because a request hung, or an empty response that renders as a broken layout instead of an empty state. Storybook plus MSW is where you author and assert exactly those states before they ship.


State Handler What it proves


Loading` await delay('infinite')` before responding The pending UI renders and doesn’t flash


Error` HttpResponse.json(null, { status: 500 })` The error branch handles a 5xx


Empty` HttpResponse.json(\[\])` The zero-results layout is designed, not broken


The[delay function](https://mswjs.io/docs/api/delay/) accepts an` 'infinite'` mode that holds the request pending forever, the reliable way to freeze a component on its loading branch. Import` delay` from` msw` and` await` it inside an async resolver:


```text
export   const   Loading  :   Story   =   {
parameters  :   {
msw  :   {
handlers  :   [
http  .  get  (  endpoint  ,   async   ()   =>   {
await   delay  (  '  infinite  '  );
return   HttpResponse  .  json  ([]);
}),
],
},
},
};


export   const   Error  :   Story   =   {
parameters  :   {
msw  :   {   handlers  :   [  http  .  get  (  endpoint  ,   ()   =>   HttpResponse  .  json  (  null  ,   {   status  :   500   }))]   },
},
};


export   const   Empty  :   Story   =   {
parameters  :   {
msw  :   {   handlers  :   [  http  .  get  (  endpoint  ,   ()   =>   HttpResponse  .  json  ([]))]   },
},
};
```


MSW mocks GraphQL the same way (` graphql.query('AllUsers', () => HttpResponse.json({ data }))` ), so the pattern carries to Apollo, urql, and React Query without change.


## From viewing to testing


A mocked story you only look at is documentation; add a[play function](https://storybook.js.org/docs/writing-tests/interaction-testing) and it becomes an automatable component test. Import` expect` from` storybook/test` (the current module, which replaced Storybook 8’s` @storybook/test` ) and assert on the rendered result:


```text
import   {   expect   }   from   '  storybook/test  '  ;


// on Success:
play  :   async   ({   canvas   })   =>   {
await   expect  (  await   canvas  .  findByText  (  '  Ada Lovelace  '  )).  toBeInTheDocument  ();
},


// on Loading:
play  :   async   ({   canvas   })   =>   {
await   expect  (  canvas  .  getByRole  (  '  status  '  )).  toBeInTheDocument  ();
},
```


For the infinite-delay` Loading` story, assert that the spinner is present. Don’t await resolution, since the request pends forever by design. These stories run through the[Vitest addon](https://storybook.js.org/docs/writing-tests/integrations/vitest-addon) , which executes them as component tests in Playwright’s Chromium browser from the Storybook UI, the terminal, or CI. Because MSW handlers are environment-agnostic, the same success/error/empty handlers back a standalone Vitest test via` setupServer` , and Chromatic snapshots each story for visual regression.


Mock at the network layer, model all four states, and attach a` play` function to each: that turns a folder of stories into a live test suite that catches the loading-forever and broken-empty-state bugs before they reach production. Start by adding the loading, error, and empty stories to one existing app component today. The handlers you write there are the same ones your Vitest tests will reuse.


## FAQs


Why is my component stuck on 'Loading…' in Storybook even after installing msw-storybook-addon?


The component is stuck because either no MSW handler matches its request or the addon's loader is not wired. Confirm you added mswLoader to the loaders array in .storybook/preview and called initialize(), that public was generated with npx msw init and listed in staticDirs, and that a handler in parameters.msw.handlers matches the exact request URL and method. A URL mismatch leaves the request unhandled and the component pending forever.


What is the difference between mocking at the network layer with MSW and stubbing the fetch hook?


Network-layer mocking with MSW intercepts the actual outgoing request and returns a response, so the component runs its real fetching code path unchanged and the same handlers work in the browser, in Node via setupServer, and in Chromatic. Stubbing the hook replaces useQuery or fetch with canned data, which couples the story to one data library and to internal component structure, and cannot be reused in a Node test.


Does msw-storybook-addon work with MSW v1 handlers like rest.get and res(ctx.json())?


No. Since version 2.0.0 the addon requires MSW 2.0.0 or higher, and MSW v2 removed the rest namespace and the res(ctx.json()) resolver signature. Rewrite handlers using http.get and the HttpResponse class, for example http.get(url, () => HttpResponse.json(data)). MSW v1 code will not run against the current addon and must be migrated using MSW's official 1.x to 2.x migration guide.


Why does my infinite-delay loading story hang when run as a test?


A story using await delay('infinite') pends its request forever by design, so a play function that awaits the resolved UI never completes. Assert only that the pending UI is present, for example that the spinner with role status renders, rather than awaiting resolution. If a Node or Vitest test must complete cleanly, use a finite delay such as delay(1000) instead of the infinite mode for that scenario.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
