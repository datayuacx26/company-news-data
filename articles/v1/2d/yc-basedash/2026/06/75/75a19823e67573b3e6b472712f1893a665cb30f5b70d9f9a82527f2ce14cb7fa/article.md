---
schema_version: "1.0.0"
document_id: "75a19823e67573b3e6b472712f1893a665cb30f5b70d9f9a82527f2ce14cb7fa"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-we-cut-our-client-bundle-from-26-mib-to-18-mib/"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:6979bd5bdfa1acb14d56f949bb617821b4f56170bd492d04f452d4d83508d58a"
---

# How we cut our client bundle from 26 MB to 18 MB

Performance is really important to us at Basedash, but being a BI web app, most performance optimizations are tied to complex data fetching & rendering once the app has already been loaded. We hadn’t looked at our production client bundle in a while.


Bundle size is the kind of metric that constantly drifts upward, one dependency at a time, until someone finally notices that it’s an issue. A couple weeks ago someone noticed, so we looked. The client bundle was 25.67 MiB across 552 chunks. After a bit of digging, we got it down to 18.34 MiB across 453 chunks, a 28.6% drop, without removing a single feature.


Here’s what we found in there.


## Looking at the bundle


The first step was just turning on[rollup-plugin-visualizer](https://github.com/btd/rollup-plugin-visualizer) and reading the treemap. Our build is React Router v7 framework mode on top of Rolldown-Vite, so we already had the plugin wired up behind a` VITE_ANALYZE` env var:


```text
VITE_ANALYZE  =  treemap   pnpm   build
open   build/client/stats.html
```


The treemap is the most useful 30 seconds of work in this whole project. You stare at the rectangles and immediately have opinions about every one of them.


Our top offenders, in rough order of “what is this even doing here”:


1. **` vendor-shiki`** , 5.1 MiB
2. **` AgentMessageHistory`** , 2.41 MiB
3. **` html2pdf`** vendor chunk, 1.85 MiB
4. **` root` chunk** , 983 KiB
5. **` entry.client`** , 974 KiB


Some of those make sense (Shiki is a real syntax highlighter doing real work). Some absolutely do not.` AgentMessageHistory` is a domain class that holds AI chat messages, and there’s no universe where it should be 2.41 MiB.


## Surprise #1: react-scan in production


The` root` chunk had a 484 KiB blob labeled` react-scan` .[react-scan](https://github.com/aidenybai/react-scan) is the runtime profiler that draws colored borders around components when they re-render. Genuinely useful in dev, no business being on a customer’s machine.


We had it gated behind a feature flag, but the gate only controlled whether` scan()` actually started. The entire library was still in the root bundle on every page load, ready to scan in case the flag ever flipped.


```text
// app/components/.client/ReactScan.tsx (deleted)
import   { scan }   from   'react-scan'  ;


export   function   ReactScanMonitoring  ({   scanEnabled   }  :   {   scanEnabled  :   boolean   }) {
useEffect  (()   =>   {
scan  ({ enabled: scanEnabled, dangerouslyForceRunInProduction: scanEnabled });
}, [scanEnabled]);
return   null  ;
}
```


Two deleted files and one removed` package.json` entry later, the root chunk lost half a megabyte. If we ever want react-scan back, we can lazy-load it behind the same flag. There’s no reason for it to live at the top of the dependency tree.


## Surprise #2: gpt-tokenizer in the client bundle


This was the bigger one, and it’s the kind of bug that’s almost invisible without a treemap.


` gpt-tokenizer` is 2.3 MiB of BPE token tables, used for counting OpenAI tokens before we send a chat history to the model. Token counting is a server thing. We never count tokens in the browser. And yet there it was, sitting inside a chunk called` AgentMessageHistory` .


` AgentMessageHistory.ts` is a shared file. It defines the` AgentMessage` shape, plus a helper called` bufferHistoryToTokenLimit` that trims an array of messages to fit under a context window. The helper is the only thing that imports` gpt-tokenizer` , but importing it from a shared file means the bundler can’t tell that only the server needs it. Both the client and server pull in the whole file, so both get the tokenizer.


Fix: split the file. The pure type definitions stay in the shared file, and the token-counting helper moves to a` .server.ts` suffix, which Vite/React Router treats as server-only and tree-shakes from the client.


```text
// app/features/ai/domain/agentMessageTokenBuffer.server.ts
import   { encode }   from   'gpt-tokenizer'  ;


export   function   bufferHistoryToTokenLimit  (  /* ... */  ) {
// token counting that only ever runs on the server
}
```


```text
// app/features/ai/core/agent.ts
import   { bufferHistoryToTokenLimit }   from   '~/features/ai/domain/agentMessageTokenBuffer.server'  ;
```


The` AgentMessageHistory` chunk went from 2.41 MiB to 47 KiB. That’s a 50x reduction from moving 111 lines of code into a different file.


## Shiki: from a bundle of everything to the languages we use


` shiki/bundle/web` is the convenience entrypoint that ships every web language Shiki supports (around 53 of them) plus the Oniguruma WASM regex engine. It’s a great default if you don’t know which languages you need. We know which languages we need, and it’s not 53.


```text
// Before
import   { createHighlighter }   from   'shiki/bundle/web'  ;


const   highlighter   =   await   createHighlighter  ({
themes: [BasedashDark, BasedashLight],
langs: [  'sql'  ,   'javascript'  ,   'typescript'  ,   /* ... 8 more */  ],
});
```


We replaced that with` shiki/core` plus fine-grained imports. Only SQL and Markdown load at init (those are by far the most common in our app, since the chat agent generates a lot of SQL inside Markdown fences). Everything else is dynamically imported the first time we see it.


```text
// After
import   { createHighlighterCore }   from   'shiki/core'  ;
import   { createJavaScriptRegexEngine }   from   'shiki/engine/javascript'  ;


const   highlighter   =   await   createHighlighterCore  ({
engine:   createJavaScriptRegexEngine  (),
themes: [BasedashDark, BasedashLight],
langs: [
CustomSqlTextmate,
import  (  '@shikijs/langs/sql'  ),
import  (  '@shikijs/langs/markdown'  ),
],
});


const   LANGUAGE_IMPORTS  :   Record  <  string  , ()   =>   Promise  <  unknown  >>   =   {
javascript  : ()   =>   import  (  '@shikijs/langs/javascript'  ),
typescript  : ()   =>   import  (  '@shikijs/langs/typescript'  ),
python  : ()   =>   import  (  '@shikijs/langs/python'  ),
// ... 7 more
};
```


Swapping Oniguruma WASM for the JavaScript regex engine is another 608 KiB. Shiki ships an engine specifically for the languages it supports, and the trade-off (slightly slower regex matching on a one-time init) is fine for our usage.


` vendor-shiki` went from 5.10 MiB to 1.02 MiB, an 80% cut on its own.


## Lazy-loading the things you only sometimes use


There’s a class of dependencies that are huge but only needed in specific situations: PDF export, SQL formatting, emoji rendering for Slack messages, Sentry session replay. We were eagerly importing all of them, so every dashboard load paid for features the user might never trigger.


**html2pdf** (1.85 MiB) only runs when someone clicks “Export to PDF” in the dashboard header. We split it into its own chunk and dynamically imported it at the click site. The library now sits as a deferred chunk that the browser fetches in the time between “user clicks the button” and “the PDF actually generates” (which takes seconds anyway because rasterizing a dashboard is not cheap).


**sql-formatter** (285 KiB) is interesting because it’s used in a lot of places, just not on first paint. The chart agent shows formatted SQL after running a query. The SQL editor formats on demand. None of that needs to block the initial bundle.


The trick was that several React components called` formatSqlQuery` synchronously and just rendered the result. Making the function async would force all of those into a loading state. Instead, we wrote a small hook that shows raw SQL immediately and swaps in the formatted version once the module loads:


```text
// app/hooks/useFormattedSql.ts
let   formatterModule  :   typeof   import  (  'sql-formatter'  )   |   null   =   null  ;
let   formatterPromise  :   Promise  <  typeof   import  (  'sql-formatter'  )>   |   null   =   null  ;


export   function   useFormattedSql  (  sql  :   string  ,   dialect  :   Dialect  ) {
const   [  formatted  ,   setFormatted  ]   =   useState  (sql);


useEffect  (()   =>   {
if   (formatterModule) {
setFormatted  (formatterModule.  format  (sql, { language: dialect }));
return  ;
}
formatterPromise   ??=   import  (  'sql-formatter'  );
formatterPromise.  then  ((  mod  )   =>   {
formatterModule   =   mod;
setFormatted  (mod.  format  (sql, { language: dialect }));
});
}, [sql, dialect]);


return   formatted;
}
```


The module is cached at module scope, so the dynamic import only fetches once per session. After that, the hook is synchronous in practice. The` formatSqlQuery` chunk went from 301 KiB to 2.9 KiB.


**node-emoji** (343 KiB, mostly its` emojilib` data) gets the same treatment, with a slightly different fallback. Emoji shortcodes like` :wave:` just pass through as text until the module is ready, then get replaced once it loads. Nobody notices the half second it takes to convert` :white_check_mark:` into a checkmark on the second render.


**Sentry Replay** (231 KiB) had a built-in escape hatch. Sentry’s SDK has a` lazyLoadIntegration` helper that fetches the integration code from the Sentry CDN after init, instead of bundling it. One line change:


```text
Sentry.  lazyLoadIntegration  (  'replayIntegration'  ).  then  ((  replayIntegration  )   =>   {
Sentry.  addIntegration  (  replayIntegration  ({
maskAllText:   true  ,
blockAllMedia:   true  ,
}));
});
```


Replay still captures everything it captured before, just on a brief delay after page load. For session replay specifically, missing the first 500ms is a fine trade.


## The Docker-specific Babel weirdness


This last one isn’t a bundle-size win, but it cost us 14% of build CPU time and was a weird enough bug to write down.


Our Vite config runs Babel with the React Compiler plugin over our source files. The include glob looked like this:


```text
babel  ({
include:   '/app/**/*'  ,
exclude:   /  app  \/  emails  \/  templates  /  ,
filter:   /  \.  [jt]  sx  ?$  /  ,
}),
```


That works fine on a developer laptop where the project lives at` /Users/derek/code/charts/app/...` . The glob` /app/**/*` doesn’t match anything outside the project.


In Docker,` WORKDIR=/app` , so` node_modules` lives at` /app/node_modules/` . The same glob now eagerly matches every vendor file. Babel happily started processing thousands of dependencies (` react-dom` ,` html2pdf` , the works), triggering V8 deoptimization on a few of the larger files.


Fix is one line:


```text
babel  ({
include:   '/app/**/*'  ,
exclude: [  /  node_modules  /  ,   /  app  \/  generated  \/  /  ,   /  emails  \/  templates  /  ],
filter:   /  \.  tsx  $  /  ,
}),
```


We also narrowed the file filter from` .js/.ts/.jsx/.tsx` to` .tsx` only. React Compiler only does useful work on TSX files (it’s about React components), so processing non-component code was pure waste.


## Final numbers


Metric Before After Change


Total client bundle 25.67 MiB 18.34 MiB −7.33 MiB (−29%)


` entry.client` 974 KiB 741 KiB −233 KiB (−24%)


` root` chunk 983 KiB 438 KiB −545 KiB (−55%)


Chunks 552 453 −99


Build wall-clock 30.2s 30.4s ~same


Build CPU time (user) 42.6s 36.6s −14%


The chunks-emitted count dropping is mostly the inverse of all the lazy loading. We have fewer small chunks because we don’t pre-split things that nobody asked for, and the things we do lazy-load are larger, more deliberate chunks.


Build wall-clock barely moved. That was the boring outcome we were hoping for: smaller output, same build speed.


## What we’d actually do next


There’s more to find. We didn’t touch Monaco (the SQL editor’s underlying engine), which is around 2 MiB and could probably be lazy-loaded behind the SQL editor route. There’s a long tail of vendor chunks in the 50 to 150 KiB range that could be coalesced or trimmed.


We also haven’t measured the impact on real-world TTI (time-to-interactive) for our customers. A smaller bundle is necessary but not sufficient; what matters is whether the user can do something a second sooner. We’re going to instrument that next, with the Sentry Replay we just made smaller.


The boring lessons, if you want to walk them away:


1. **Run the bundle analyzer.** Just look at it. We had a 484 KiB profiler shipping to production because nobody had looked.
2. **A` .server.ts` suffix is the cheapest fix you’ll ever ship.** Splitting` AgentMessageHistory.ts` was 110 lines moved. It deleted 2.36 MiB of client code.
3. **Convenience bundles are convenient until they’re not.**` shiki/bundle/web` is a great default. We needed to graduate to` shiki/core` and load what we actually use.
4. **Lazy-load anything that’s both heavy and conditional.** PDF export, syntax formatting, emoji rendering, session replay. None of these need to block first paint.
5. **Re-check your glob patterns in Docker.** Anything anchored at` /app` is going to behave differently when` WORKDIR=/app` .


If you want a tour of how we think about reliability and frontend perf more broadly, we have a writeup on[how 97 Replicache subscriptions stalled our dashboard editor](https://www.basedash.com/blog/how-97-replicache-subscriptions-stalled-our-dashboard-editor) and another on[what was killing our healthy Kubernetes pods](https://www.basedash.com/blog/what-was-killing-our-healthy-kubernetes-pods) . Both are the same shape as this one: measure, find the surprise, fix the surprise, measure again.


And if you want to see what’s running on the smaller bundle,[Basedash](https://www.basedash.com/) is an AI-native BI platform that lets your team chat with their data instead of building dashboards by hand.
