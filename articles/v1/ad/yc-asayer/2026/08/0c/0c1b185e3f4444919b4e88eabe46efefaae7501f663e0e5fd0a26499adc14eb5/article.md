---
schema_version: "1.0.0"
document_id: "0c1b185e3f4444919b4e88eabe46efefaae7501f663e0e5fd0a26499adc14eb5"
company_key: "yc-asayer"
company: "OpenReplay"
source_id: "yc-asayer-news-import-d07b882e81c8"
canonical_url: "https://blog.openreplay.com/test-svelte-5-components-vitest/"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-07T14:49:13.247278+00:00"
fetched_at: "2026-08-07T14:49:14.251890+00:00"
content_hash: "sha256:23152290febc14fd221bd0b6986194aab213da1aeb8b8b6dbacef57639fd1dc4"
---

# Testing Svelte 5 Components with Vitest

Svelte 5 changed how you set up component tests: you no longer instantiate a component with` new Component({ target })` — that constructor API was removed. Instead, mount components with[mount() from svelte](https://svelte.dev/docs/svelte/testing) ,` render()` from[@testing-library/svelte](https://github.com/testing-library/svelte-testing-library) , or` render()` from[vitest-browser-svelte](https://vitest.dev/api/browser/svelte) . Runes (` $state` ,` $derived` ,` $effect` ,` $props` ) only run once the Svelte compiler has processed the file, so your test setup has to route test files through that compiler. This guide covers the current, correct Vitest configuration and the specific patterns for testing runes and components in Svelte 5.


## Key Takeaways


- In Svelte 5 the` new Component({ target })` constructor and` $set` /` $on` /` $destroy` were removed; mount with` mount()` from` svelte` or a` render()` helper, and read props via` $props()` .
- To test runes directly, put them in a file whose name includes` .svelte` (for example` counter.svelte.test.ts` ) so the compiler processes the runes before Vitest runs the assertions.
- Effects don’t run synchronously — wrap` $effect` -using code in` $effect.root()` and call` flushSync()` to flush pending effects before you assert.
- For` @testing-library/svelte` on Svelte 5, add the` svelteTesting` plugin from` @testing-library/svelte/vite` ; it sets the browser resolve condition and cleans up the DOM after each test automatically.
- ` vitest-browser-svelte` runs your component in a real browser via Playwright and requires Vitest 4; always` await render(...)` , query with locators, and assert with` await expect.element(...)` .


## What changed for testing in Svelte 5?


Most Svelte testing tutorials online are Svelte 4-era and use APIs that no longer exist. If a guide instantiates a component with` new` , calls` component.$set` , or reads` $$props` , it is outdated. Here is the migration map:


Svelte 4 (removed) Svelte 5 (current)


` new Component({ target })`` mount(Component, { target })` or` render(Component)`


` component.$set(props)` pass props to` render` /` rerender`


` component.$on` /` component.$destroy` callback props /` unmount(component)`


` $$props`` $props()`


` fireEvent` -first` userEvent` or browser-mode locators


` svelte-jester` setup` svelteTesting` plugin (Vitest)


Two setups are current, and both are valid. The first is` @testing-library/svelte` running on jsdom — higher-level, familiar, and it supports Svelte versions 3, 4, and 5. The second is` vitest-browser-svelte` , which renders components in a real browser through Playwright using[Vitest’s stable Browser Mode](https://vitest.dev/blog/vitest-4) . Browser Mode dropped its experimental tag in Vitest 4, so ignore any tutorial that still calls it experimental.


## How do you configure Vitest for Svelte testing?


Every Svelte 5 test setup shares one requirement: Vitest must resolve the` browser` entry points of your packages even though it runs in Node. The Svelte docs do this with` resolve.conditions` . Start with a base config and build on it.


For component tests on jsdom, install` jsdom` and add the environment plus the` svelteTesting` plugin:


```text
// vite.config.js
import   {   defineConfig   }   from   '  vitest/config  '  ;
import   {   svelte   }   from   '  @sveltejs/vite-plugin-svelte  '  ;
import   {   svelteTesting   }   from   '  @testing-library/svelte/vite  '  ;


export default   defineConfig  ({
plugins  :   [  svelte  (),   svelteTesting  ()],
test  :   {
environment  :   '  jsdom  '
}
});
```


The` svelteTesting` plugin sets the browser resolve condition for you and, in Vitest, automatically sets up and cleans up the DOM before and after each test — so you write no manual` afterEach(cleanup)` . Do not also hand-write` resolve.conditions` ; the plugin covers it.


For real-browser tests, swap in Vitest’s Browser Mode. As of Vitest 4, provider packages install separately, and the config imports` playwright()` from` @vitest/browser-playwright` with an` instances` array — the older` provider: 'playwright', name: 'chromium'` form is deprecated:


```text
// vite.config.js
import   {   defineConfig   }   from   '  vitest/config  '  ;
import   {   svelte   }   from   '  @sveltejs/vite-plugin-svelte  '  ;
import   {   playwright   }   from   '  @vitest/browser-playwright  '  ;


export default   defineConfig  ({
plugins  :   [  svelte  ()],
test  :   {
browser  :   {
enabled  :   true  ,
provider  :   playwright  (),
instances  :   [{   browser  :   '  chromium  '   }]
}
}
});
```


## Write a component test


A Svelte 5 component reads its inputs with` $props()` and holds local state in` $state` . Here is the component both approaches will test:


```text
<!-- Counter.svelte -->
<  script  >
let   {   initial   =   0   }   =   $  props  ();
let   count   =   $  state  (  initial  );
</  script  >


<  button   onclick  =  {  ()   =>   count  ++  }>{count}</  button  >
```


With` @testing-library/svelte` , call` render` , query by role, drive interaction with` userEvent` , and` await` the click:


```text
import   {   render  ,   screen   }   from   '  @testing-library/svelte  '  ;
import   userEvent   from   '  @testing-library/user-event  '  ;
import   {   expect  ,   test   }   from   '  vitest  '  ;
import   Counter   from   '  ./Counter.svelte  '  ;


test  (  '  increments on click  '  ,   async   ()   =>   {
const   user   =   userEvent  .  setup  ();
render  (  Counter  , {   initial  :   0   });
const   button   =   screen  .  getByRole  (  '  button  '  );
expect  (  button  ).  toHaveTextContent  (  '  0  '  );
await   user  .  click  (  button  );
expect  (  button  ).  toHaveTextContent  (  '  1  '  );
});
```


Svelte’s own` mount()` /` unmount()` is the low-level API underneath these helpers. The docs note the raw` mount()` approach is “low level and somewhat brittle” because it asserts against exact` innerHTML` , so prefer a render helper for component tests.


With` vitest-browser-svelte` , always` await render(...)` , query with locators, and assert with` expect.element` , which auto-retries until the assertion passes:


```text
import   {   render   }   from   '  vitest-browser-svelte  '  ;
import   {   expect  ,   test   }   from   '  vitest  '  ;
import   Counter   from   '  ./Counter.svelte  '  ;


test  (  '  increments on click  '  ,   async   ()   =>   {
const   screen   =   await   render  (  Counter  , {   initial  :   0   });
const   button   =   screen  .  getByRole  (  '  button  '  );
await   button  .  click  ();
await   expect  .  element  (  button  ).  toHaveTextContent  (  '  1  '  );
});
```


## Test runes and reactive logic


Before mounting anything, ask whether you actually need a component test. The Svelte docs advise extracting reactive logic into a` .svelte.js` module and testing it in isolation, without the overhead of a component. That module can use runes because its filename includes` .svelte` :


```text
// counter.svelte.js
export   function   createCounter  (  initial   =   0  ) {
let   count   =   $state  (  initial  );
const   doubled   =   $derived  (  count   *   2  );
return   {
get   count  ()   {   return   count  ;   },
get   doubled  ()   {   return   doubled  ;   },
increment  ()   {   count  ++  ;   }
};
}
```


Test it directly — the test file itself must also include` .svelte` in its name, e.g.` counter.svelte.test.js` , so the compiler processes the runes:


```text
import   {   expect  ,   test   }   from   '  vitest  '  ;
import   {   createCounter   }   from   '  ./counter.svelte.js  '  ;


test  (  '  derives doubled from count  '  , ()   =>   {
const   counter   =   createCounter  (  2  );
expect  (  counter  .  doubled  ).  toBe  (  4  );
counter  .  increment  ();
expect  (  counter  .  doubled  ).  toBe  (  6  );
});
```


Effects are the exception: they don’t run synchronously. When the code under test uses` $effect` , wrap it in` $effect.root()` and call` flushSync()` to execute pending effects before you assert, exactly as the Svelte testing docs show:


```text
import   {   flushSync   }   from   '  svelte  '  ;
import   {   expect  ,   test   }   from   '  vitest  '  ;
import   {   logger   }   from   '  ./logger.svelte.js  '  ;


test  (  '  logs each update  '  , ()   =>   {
const   cleanup   =   $effect  .  root  (()   =>   {
let   count   =   $state  (  0  );
const   log   =   logger  (()   =>   count  );
flushSync  ();
expect  (  log  ).  toEqual  ([  0  ]);
count   =   1  ;
flushSync  ();
expect  (  log  ).  toEqual  ([  0  ,   1  ]);
});
cleanup  ();
});
```


## Test snippets and props


Snippets are Svelte 5’s replacement for slots, rendered with` {@render}` and received through` $props()` . For a component that renders a` children` snippet, the simplest test is a small wrapper component with a` data-testid` , then query for it. For snippets whose arguments you want to inspect, the` vitest-browser-svelte` docs use Svelte’s` createRawSnippet` API to pass a snippet directly and check what it received:


```text
<!-- Greeting.svelte -->
<  script  >
let   {   name  ,   message   }   =   $  props  ();
const   greeting   =   $  derived  (  `  Hello,   ${  name  }  !  `  );
</  script  >


<  p  >  {@  render   message  ?.(  greeting  )}  </  p  >
```


```text
import   {   render   }   from   '  vitest-browser-svelte  '  ;
import   {   createRawSnippet   }   from   '  svelte  '  ;
import   {   expect  ,   test   }   from   '  vitest  '  ;
import   Greeting   from   '  ./Greeting.svelte  '  ;


test  (  '  passes the greeting into the snippet  '  ,   async   ()   =>   {
const   screen   =   await   render  (  Greeting  , {
name  :   '  Alice  '  ,
message  :   createRawSnippet  ((  greeting  )   =>   ({
render  :   ()   =>   `  <span data-testid="message">  ${  greeting  ()  }  </span>  `
}))
});
await   expect  .  element  (  screen  .  getByTestId  (  '  message  '  ))
.  toHaveTextContent  (  '  Hello, Alice!  '  );
});
```


## jsdom vs browser mode: which to choose


Choose jsdom +` @testing-library/svelte` for fast, browserless tests of markup and logic; choose` vitest-browser-svelte` when you need real browser APIs — layout, focus,` IntersectionObserver` — without mocking them.


jsdom + testing-library vitest-browser-svelte


Environment Simulated DOM (jsdom) Real browser via Playwright


Speed / setup Fast, no browser download Heavier per test; needs a browser


Browser APIs Shimmed / mocked Native, no mocking


Sync flushing Often needs` flushSync` Locators auto-retry; rarely need it


Requires Svelte 3/4/5 support Vitest 4


Because browser-mode locators and` expect.element` retry until the assertion succeeds, you rarely reach for` flushSync` in component tests there — though a few edge cases still need it. Keep pure reactive logic in jsdom` .svelte.test` files for speed, and reserve browser mode for behavior that depends on a genuine rendering engine.


Start by extracting logic into` .svelte.js` modules and testing it in isolation, add jsdom component tests through the` svelteTesting` plugin, and reach for` vitest-browser-svelte` when a test genuinely needs a real browser. Wire up the config once, pin to the current APIs above, and your Svelte 5 suite will stay clear of the removed Svelte 4 patterns that break most older tutorials.


## FAQs


Why is my $effect not running in a Vitest test?


Effects don't run synchronously in tests, so an assertion placed right after a state change sees stale values. Wrap the effect-using code in $effect.root() and call flushSync() from svelte to flush pending effects before you assert. Call the cleanup function returned by $effect.root() when the test finishes. In browser-mode tests you rarely need this because locators and expect.element auto-retry, though a few edge cases still require flushSync.


Do I still need svelte-jester to test Svelte 5 components?


No — svelte-jester is the Jest-only path and is unnecessary with Vitest. For Vitest, add the svelteTesting plugin from @testing-library/svelte/vite, which sets the browser resolve condition and auto-cleans the DOM after each test. svelte-jester still appears in the testing-library setup docs as the Jest fallback, but if you are on Vitest you should ignore it. Many older tutorials copy the Jest path, which causes needless setup failures.


Can I test Svelte 5 runes in a regular .test.js file?


No. Runes only run after the Svelte compiler processes the file, and the compiler only processes files whose name includes .svelte. To test runes directly, name the file with .svelte in it, for example counter.svelte.test.js, so the compiler transforms the runes before Vitest runs the assertions. The same rule applies to plain modules that use runes: name them with .svelte, such as counter.svelte.js, and import them normally into your tests.


What version of Vitest does vitest-browser-svelte require?


vitest-browser-svelte requires Vitest 4.0.0 or higher; installing it against Vitest 3 or earlier fails. Browser Mode became stable in Vitest 4, which also moved provider packages into separate installs — you import playwright() from @vitest/browser-playwright and configure an instances array. The older provider: 'playwright', name: 'chromium' shape from Vitest 2 is deprecated and no longer correct. The package lives under the vitest-community org on GitHub.


DevTools for the frontend


## Gain Debugging Superpowers


Unleash the power of session replay to reproduce bugs, track slowdowns and uncover frustrations in your app. Get complete visibility into your frontend with **OpenReplay** — the most advanced open-source session replay tool for developers.


[Star on GitHub 12k](https://github.com/openreplay/openreplay)
