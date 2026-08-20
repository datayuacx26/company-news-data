---
schema_version: "1.0.0"
document_id: "c02c348442b6adc4cd9302247b33444519bfd77eac76493b355687f571d027d6"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/persist-state-local-storage-react/"
published_at: "2026-08-14T00:00:00+00:00"
first_seen_at: "2026-08-14T12:44:22.742634+00:00"
fetched_at: "2026-08-14T12:44:24.576878+00:00"
content_hash: "sha256:c776f867c56bf1cc38524585ee492db5b783b593c709792cda2f528d118c2cd5"
---

# How to Persist State in Local Storage with React

To persist React state in` localStorage` , initialize` useState` from storage inside its initializer function and write the value back whenever it changes — then wrap the JSON in` try/catch` and guard against server-side rendering.


Every React app eventually grows one of these, usually for a theme toggle or a sidebar that should stay collapsed. The three-line version takes five minutes to write and then quietly costs you an afternoon later. That naive version works for a counter on a single tab, but it breaks in three predictable ways: it crashes on corrupt data, throws` window is not defined` in Next.js, and goes stale across tabs. This article builds a` useLocalStorage` hook up the correctness ladder, fixing each failure mode in turn, and ends with a drop-in hook you can paste into a React 18 or 19 project.


` localStorage` is a synchronous, same-origin, string-only key/value store of roughly 5MB per origin, documented in the[MDN Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) . One rule before any code: never store auth tokens or PII in it. It’s readable by any JavaScript on the page and unencrypted.


## Key Takeaways


- Read` localStorage` inside` useState` ’s initializer so the lookup runs once on mount instead of flashing the default value first through a` useEffect` .
- Because` localStorage` stores strings only, persist with` JSON.stringify` on write and` JSON.parse` on read, wrapped in` try/catch` so one corrupt value can’t crash the component.
- On the server there is no` window` , so reading storage during first render throws` window is not defined` in Next.js and Remix. Render the default on the server and sync to the persisted value after mount.
- The browser’s` storage` event fires only in *other* tabs, never the one that wrote the value, so same-tab listeners need a manually dispatched event.
- ` useSyncExternalStore` , added in React 18, is the officially supported way to subscribe a component to an external mutable store like` localStorage` .


## The naive React localStorage pattern


The starting point is a lazy-initialized` useState` paired with a write effect. In React, read` localStorage` inside` useState` ’s initializer function so the lookup runs once on mount, instead of reading it in a` useEffect` that would flash the default value first.


```text
import   {   useState  ,   useEffect   }   from   '  react  '  ;


function   ThemeToggle  () {
const   [  theme  ,   setTheme  ]   =   useState  (()   =>   {
return   localStorage  .  getItem  (  '  theme  '  )   ??   '  light  '  ;
});


useEffect  (()   =>   {
localStorage  .  setItem  (  '  theme  '  ,   theme  );
}, [  theme  ]);


return   (
<  button   onClick  =  {  ()   =>   setTheme  (  t   =>   (  t   ===   '  light  '   ?   '  dark  '   :   '  light  '  ))  }>
Theme: {theme}
</  button  >
);
}
```


Passing a *function* to` useState` (not` useState(localStorage.getItem(...))` ) matters: the[lazy initializer runs only on the first render](https://react.dev/reference/react/useState) , so you avoid hitting` localStorage` on every re-render. Reading in the initializer rather than a separate` useEffect` also means the correct value is present on the very first paint, with no default-then-persisted flash.


## Serialize safely with JSON and try/catch


The naive version only handles strings. Because` localStorage` only stores strings, persist non-string state with` JSON.stringify` on write and` JSON.parse` on read, and wrap the parse in` try/catch` so a single corrupted or legacy value can’t crash the component. A common production failure mode is a schema change or a half-written value leaving invalid JSON under a key; without the guard,` JSON.parse` throws on mount and takes the component down.


```text
function   readJSON  <  T  >(  key  :   string  ,   fallback  :   T  )  :   T   {
try   {
const   raw   =   localStorage  .  getItem  (  key  );
return   raw   ?   (  JSON  .  parse  (  raw  )   as   T  )   :   fallback  ;
}   catch   {
return   fallback  ;   // corrupt or legacy value → fall back to default
}
}
```


The` catch` branch returns the default instead of propagating, which is the difference between a bad key resetting one preference and a bad key blanking the page.


## How do you build a reusable useLocalStorage hook?


Wrap the pattern into a hook that mirrors` useState` so it’s a drop-in replacement. To keep parity with` useState` , your` useLocalStorage` setter must accept a functional update, so` setValue(prev => prev + 1)` works the same way it does with built-in state. It’s the ergonomic gap most hand-rolled versions miss.


```text
function   useLocalStorage  <  T  >(  key  :   string  ,   initialValue  :   T  ) {
const   [  value  ,   setValue  ]   =   useState  <  T  >(()   =>   readJSON  (  key  ,   initialValue  ));


const   set   =   useCallback  (
(  next  :   T   |   ((  prev  :   T  )   =>   T  ))   =>   {
setValue  (  prev   =>   {
const   resolved   =   next   instanceof   Function   ?   next  (  prev  )   :   next  ;
localStorage  .  setItem  (  key  ,   JSON  .  stringify  (  resolved  ));
return   resolved  ;
});
},
[  key  ],
);


return   [  value  ,   set  ]   as   const  ;
}
```


The` next instanceof Function` check is what preserves` useState` ergonomics. This version is correct on the client, but it still reads` localStorage` during render, which breaks the moment you server-render it.


## The SSR gotcha: “window is not defined” and hydration mismatch


On the server there is no` window` or` localStorage` , so reading storage during the first render throws` window is not defined` in Next.js and Remix. Guard with` typeof window === 'undefined'` and read the persisted value after mount instead.


There’s a second, subtler bug even after you stop the crash. A hydration mismatch happens because the server renders your default state while the client already has the stored value; React’s first client render must match the server HTML, so if you read` localStorage` in the initializer during hydration, the markup diverges. The fix is to render the default on the server, then sync to the persisted value in an effect after hydration.


```text
const   IS_SERVER   =   typeof   window   ===   '  undefined  '  ;


function   useLocalStorage  <  T  >(  key  :   string  ,   initialValue  :   T  ,   initializeWithValue   =   true  ) {
const   readValue   =   ()   =>   (  IS_SERVER   ?   initialValue   :   readJSON  (  key  ,   initialValue  ));


const   [  value  ,   setValue  ]   =   useState  <  T  >(()   =>
initializeWithValue   ?   readValue  ()   :   initialValue  ,
);


useEffect  (()   =>   {
setValue  (  readValue  ());   // sync from storage after mount
}, [  key  ]);
// ...setter as before
}
```


The` initializeWithValue` flag mirrors the switch in the[usehooks-ts useLocalStorage](https://usehooks-ts.com/react-hook/use-local-storage) : set it to` false` for SSR so the hook returns the default on the server and syncs after hydration. This class of bug is nearly invisible in a clean` localhost` load. Replaying a real production session is often how the hydration flash (the default theme painting for one frame before the persisted value takes over) actually becomes visible, since it’s timing- and environment-dependent rather than reproducible on demand.


## Cross-tab sync and the modern useSyncExternalStore approach


Persisted state should stay consistent when a user has two tabs open. The browser’s` storage` event, described in[MDN’s Window: storage event](https://developer.mozilla.org/en-US/docs/Web/API/Window/storage_event) , fires only in *other* tabs and documents, never the tab that wrote the value. Cross-tab sync therefore needs a` storage` listener, and same-tab listeners need a manually dispatched custom event.


For new code, there’s a cleaner primitive than` useState` + effects.` useSyncExternalStore` was[introduced in React 18](https://www.epicreact.dev/use-sync-external-store-demystified-for-practical-react-development-w5ac0) as the official way to subscribe a component to an external mutable store. Components normally read from props, state, and context, but occasionally one has to read a value that lives outside React and changes over time, including browser APIs that hold a mutable value and emit events when it changes.[React’s own reference for the hook](https://react.dev/reference/react/useSyncExternalStore) recommends built-in state when possible and reserves it mostly for integrating with existing non-React code.` localStorage` qualifies, and it’s why maintained libraries adopted it for concurrent-safe, cross-tab-correct reads.


```text
function   useLocalStorageValue  (  key  :   string  ,   initial  :   string  ) {
const   subscribe   =   (  cb  :   ()   =>   void  )   =>   {
window  .  addEventListener  (  '  storage  '  ,   cb  );
return   ()   =>   window  .  removeEventListener  (  '  storage  '  ,   cb  );
};
return   useSyncExternalStore  (
subscribe  ,
()   =>   localStorage  .  getItem  (  key  )   ??   initial  ,
()   =>   initial  ,   // server snapshot
);
}
```


## Should you hand-roll useLocalStorage or use a library?


Hand-roll when you need a single primitive value on the client. Reach for a maintained library when you need serialization edge cases, SSR, and cross-tab sync handled together. Both options below work on React 18 and 19. The current release line is[React 19.2](https://react.dev/blog/2025/10/01/react-19-2) , which shipped on October 1, 2025, with the 19.2.x patch releases since then listed in the[React changelog](https://github.com/react/react/blob/main/CHANGELOG.md) .


Option Best for SSR handling Notes


Hand-rolled hook One-off primitives, full control` typeof window` guard + post-mount effect You own the edge cases


usehooks-ts Drop-in hook with` removeValue`` initializeWithValue: false` Built on` useState` + events, not` useSyncExternalStore`


[use-local-storage-state](https://www.npmjs.com/package/use-local-storage-state) Cross-tab + concurrent correctness Built on` useSyncExternalStore` Widely used; maintainer notes hydrating components may[render twice](https://github.com/astoilkov/use-local-storage-state)


Here’s the full hand-rolled hook, correct on React 18 and 19, with lazy init,` try/catch` JSON, an SSR guard, functional updates,` removeValue` , and cross-tab plus same-tab events:


```text
import   {   useCallback  ,   useEffect  ,   useState   }   from   '  react  '  ;


const   IS_SERVER   =   typeof   window   ===   '  undefined  '  ;


type   Options<  T  >   =   {
serializer  ?:   (  value  :   T  )   =>   string  ;
deserializer  ?:   (  value  :   string  )   =>   T  ;
initializeWithValue  ?:   boolean  ;   // set false for SSR
};


export   function   useLocalStorage  <  T  >(
key  :   string  ,
initialValue  :   T  ,
options  :   Options  <  T  >   =   {},
)  :   [  T  , (  value  :   T   |   ((  prev  :   T  )   =>   T  ))   =>   void  , ()   =>   void  ] {
const   {   initializeWithValue   =   true   }   =   options  ;
const   serialize   =   options  .  serializer   ??   JSON  .  stringify  ;
const   deserialize   =   options  .  deserializer   ??   ((  v  :   string  )   =>   JSON  .  parse  (  v  )   as   T  );


const   readValue   =   useCallback  (()  :   T   =>   {
if (  IS_SERVER  )   return   initialValue  ;
try   {
const   raw   =   window  .  localStorage  .  getItem  (  key  );
return   raw   ?   deserialize  (  raw  )   :   initialValue  ;
}   catch   {
return   initialValue  ;
}
}, [  key  ,   initialValue  ,   deserialize  ]);


const   [  storedValue  ,   setStoredValue  ]   =   useState  <  T  >(()   =>
initializeWithValue   ?   readValue  ()   :   initialValue  ,
);


const   setValue   =   useCallback  (
(  value  :   T   |   ((  prev  :   T  )   =>   T  ))   =>   {
try   {
const   next   =   value   instanceof   Function   ?   value  (  readValue  ())   :   value  ;
window  .  localStorage  .  setItem  (  key  ,   serialize  (  next  ));
setStoredValue  (  next  );
window  .  dispatchEvent  (  new   StorageEvent  (  '  local-storage  '  , {   key   }));
}   catch   {
/* quota exceeded or private mode — ignore */
}
},
[  key  ,   readValue  ,   serialize  ],
);


const   removeValue   =   useCallback  (()   =>   {
window  .  localStorage  .  removeItem  (  key  );
setStoredValue  (  initialValue  );
}, [  key  ,   initialValue  ]);


// Sync from storage after mount (fixes SSR hydration) and on key change.
useEffect  (()   =>   {
setStoredValue  (  readValue  ());
}, [  key  ]);   // eslint-disable-line react-hooks/exhaustive-deps


// Cross-tab ('storage') + same-tab ('local-storage') listeners.
useEffect  (()   =>   {
const   onChange   =   (  event  :   Event  )   =>   {
const   e   =   event   as   StorageEvent  ;
if (  e  .  key   &&   e  .  key   !==   key  )   return  ;
setStoredValue  (  readValue  ());
};
window  .  addEventListener  (  '  storage  '  ,   onChange  );
window  .  addEventListener  (  '  local-storage  '  ,   onChange  );
return   ()   =>   {
window  .  removeEventListener  (  '  storage  '  ,   onChange  );
window  .  removeEventListener  (  '  local-storage  '  ,   onChange  );
};
}, [  key  ,   readValue  ]);


return   [  storedValue  ,   setValue  ,   removeValue  ];
}
```


Pass a stable` initialValue` (a primitive or a memoized object) so the effect dependencies don’t churn on every render.


Persisting React state is a ladder, not a one-liner: start with a lazy-initialized` useState` and a write effect, add JSON` try/catch` , guard for SSR, then wire up cross-tab events. Drop the hook above into a shared` hooks/` file, replace` useState` with it for the piece of state that needs to survive a refresh, and reach for` useSyncExternalStore` or a maintained library the moment concurrent correctness across tabs starts to matter.


## FAQs


What is the difference between localStorage and sessionStorage for persisting React state?


Both are synchronous, same-origin, string-only key/value stores of roughly 5MB, but they differ in lifetime. localStorage persists indefinitely until it is explicitly cleared, so state survives a refresh, tab close, and browser restart. sessionStorage is scoped to a single tab session and is wiped when that tab closes, and it is not shared between tabs. Use localStorage for preferences that should outlive the session and sessionStorage for per-tab transient state.


Why should I not use Redux Persist or a global store to persist a single piece of state?


Reaching for a global store like Redux Persist to save one value adds a store, middleware, and serialization config for state that a local hook already handles. A useLocalStorage hook keeps the value colocated with the component that owns it and mirrors useState ergonomics, including functional updates. Redux Persist earns its weight when you already run a Redux store and need whole-slice rehydration, not for a theme toggle or a single form field.


What happens when localStorage is full or disabled in private browsing mode?


Writing to localStorage throws a QuotaExceededError when the roughly 5MB origin quota is exceeded, and some browsers throw on any write in private or incognito mode because the quota is set to zero. An unguarded setItem crashes the component, which is why the setter in a robust hook wraps writes in try/catch. Reads should also fall back to the default so a blocked or full store degrades to in-memory state instead of breaking the render.


Does useSyncExternalStore replace the useState plus useEffect localStorage pattern entirely?


Not for every case. useSyncExternalStore, added in React 18, is the concurrency-safe way to subscribe a component to an external mutable store and is the right choice when cross-tab correctness and concurrent rendering matter. React's own docs recommend built-in state when possible and reserve the hook for integrating non-React stores. For a single client-only primitive, a lazy-initialized useState with a write effect remains simpler and correct; adopt useSyncExternalStore when tabs must stay in sync.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
