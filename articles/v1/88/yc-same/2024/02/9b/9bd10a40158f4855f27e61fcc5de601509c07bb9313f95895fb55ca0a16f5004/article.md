---
schema_version: "1.0.0"
document_id: "9bd10a40158f4855f27e61fcc5de601509c07bb9313f95895fb55ca0a16f5004"
company_key: "yc-same"
company: "Same"
source_id: "yc-same-news-import-393e713dbea1"
canonical_url: "https://million.dev/blog/lint.en-US"
published_at: "2024-02-29T00:00:00+00:00"
first_seen_at: "2026-08-10T02:52:25.226434+00:00"
fetched_at: "2026-08-10T02:52:27.075037+00:00"
content_hash: "sha256:487fb472e961ffbc6071314725fa8d62f36ffa4aa15068f1b7ca31d66c03d62a"
---

# Million Lint is in public beta

[Blog](https://old.million.dev/blog)


Million Lint is in public beta


# Million Lint is in public beta


[Aiden Bai (opens in a new tab)](https://twitter.com/aidenybai) ,[John Yang (opens in a new tab)](https://twitter.com/fiveseveny) ,[Nisarg Patel (opens in a new tab)](https://twitter.com/nisargptel) – February 29, 2024


---


It’s launch time. After three months and hundreds of commits, we invite you to try out **Million Lint** . The experience is not finished – there are a few known bugs and several missing features – but we are really happy with how it's shaping up and couldn't wait to share it with you.


Get started in one command by running this in any React app:


```text
npx     @million/lint@latest
```


Set up failed? Click here to follow the guide.


### Install NPM package


```text
npm     install     @million/lint@latest
```


### Install VSCode extension


Go to the[VSCode Marketplace (opens in a new tab)](https://marketplace.visualstudio.com/items?itemName=million.million-lint) and install the Million Lint extension.


### Add the compiler to your application


Million.js is supported within the` /app` ("use client" components only) and` /pages`


next.config.mjs


```text
import   MillionCompiler   from     '@million/lint'  ;


/**   @type     {import('next').NextConfig}   */
const     nextConfig     =   {
reactStrictMode  :     true  ,
};


export     default     MillionCompiler  .next  ({
rsc  :     true     // if used in the app router mode
})(nextConfig);
```


### Run your app!


Let us know if you have any[questions or feedback (opens in a new tab)](https://million.dev/chat) !


## What is Million Lint?


Million Lint is a VSCode extension that keeps your React website fast. We identify slow code and provide suggestions to fix it. It’s like ESLint, but for performance!


Many developers try to use tools like React Devtools to find unnecessary renders. Unfortunately, most lack the knowledge to properly manage the complexity.


See the difference between React Devtools and Million Lint:


*Feeling lost? Yeah, us too (+99% of all devs)*


Complicated tools means developers give up. Every developer knows this experience: we insert` console.log` everywhere, catch some promising leads, but nothing happens before "time runs out." Eventually, the slow/buggy code never gets fixed, problems pile up on a backlog, and our end users are hurt.


Are there no better ways to surface performance issues?


### You vs. profilers


Existing instrumentation tools (Sentry, Datadog, etc.) show you the “entire universe” of web data. This can include every function call, network request, user interaction, core web vital, screenshot, memory … a dizzying amount of information to process. As such, these tools have interfaces that are overwhelming.


As for React & Chrome Devtools, they are simply unusable if you don't know what you are doing. And even if you do, it is hard to tell which part of the component re-rendered, or which hook or prop caused this change. Unless you are an expert, the current experience of performance debugging is hostile to most developers.


### Compiler or runtime?


JavaScript compilers enable us to perform static analysis on source code. For performance optimization, static analysis is great for breadth – by writing rules, developers can surface problems across the entire code base. This is why ESLint is so great. The tradeoff is that we can't predict how slow an operation will be without actually running it, making it impossible to implement Million Lint solely as a compiler.


Another approach is thus instrumenting at runtime. The advantage here is we can directly run the app and get rendering information (via the[React Fiber (opens in a new tab)](https://blog.logrocket.com/deep-dive-react-fiber/) ), so many of the complexity challenges with static analysis are instantly solved. However, the tradeoff with runtime is that you don’t have the original source (unless you use source maps, which are painful and slow). "Which hook or prop caused this change?" "Which part of the component re-rendered?" We couldn’t answer these questions without the source code.


### In search of a better way


So we went back to the drawing board. For Million Lint, we asked ourselves: how can we have the best of both worlds? We needed to create an profiling experience that didn’t hog build time or runtime; then, support a debugging flow that helped developers find performance problems fast.


Our answer is dynamic analysis – using both static analysis and runtime analysis where it makes sense. Instead of filtering top-down, we follow the necessary data flow to understand each component render and build up from there. This resulted in our custom instrumentation library, designed specifically for React apps.


## *import MillionCompiler from '@million/lint';*


The Million Lint experience starts with a compiler running dynamic analysis on individual React components.


First, the compiler inject handlers:


```text
function     App  ({ start }) {


Million  .capture   ({ start });    // ✨ inject
const   [  count  ,     setCount  ]   =     Million  .capture   (useState)(start);    // ✨ inject


useEffect  (
()   =>   {
console  .log  (  "double: "  ,   count   *     2  );
}  ,
Million  .capture   ([count])  ,      // ✨ inject
);


return     Million  .capture   (    // ✨ inject
<  Button     onClick  =  {()   =>     setCount  (count   +     1  )}>{count}</  Button  >  ,
);
}
```


During runtime, these inject handlers capture render, timing, and metadata information as you interact with your web app in dev mode. With this approach, we can get runtime profiling data from running the app, while keeping access to the source code thanks to the JavaScript compiler!


```text
// psuedo code of collected data:
[
{
kind  :     'state'  ,
count  :     7  ,
time  :     0.1  ,
changes  :   [{ prev  :     0  ,   next  :     8   }  ,     ...  ]  ,
}  ,
// and so on...
];
```


After capturing the renders, we extend the compiler to asynchronously collect bundle, network, and state manager information without build time overhead. These real-time insights are then fed into the VSCode extension, where you can see the collected information and suggestions to fix it.


Lastly, we made **Lint++** , which feeds the collected information into a language model to discover optimization opportunities. So even if you don’t know what you’re doing – Million Lint will show you how!


## How good is Lint++?


Measuring the objective quality of **Lint++** requires a non-trivial dataset, so we are working on an open source React web app optimization benchmark for code-gen models (coming soon!). For now, we have tested **Lint++** on a dozen in-production products between our friends. For a public example, **Lint++** was able to identify and suggest the correct fix to every problem in[a slow, educational React app (opens in a new tab)](https://github.com/3perf/react-workshop-ra) our friend[Ivan Akulov (opens in a new tab)](https://twitter.com/iamakulov) built.


After running **Lint++** on Ivan's` notes` app, it labeled the following problems across 6 components:


- Unstable reference as prop optimization
- Memoizing expensive components
- Caching inline functions
- Suggesting virtualization
- Suggesting` use-context-selector` >` useContext`
- Moving context providers up the tree


We invited an engineer who's never seen the code base before to try speed up the` notes` app. Using Million Lint, he 3x'd the performance of the main page after ~15 minutes of work.


Of course,` notes` is a small app, and Million Lint wouldn't always find the best fix for every problem. We are working hard to improve its quality – try and please let us know about your experience!


## How will we make money from this?


In the next few weeks, we will open source the Million Lint compiler and the VSCode extension. Both the compiler and in-editor annotations are free to use forever. Our focus is to build a great developer tool, and we believe that the best way to build a great developer tool is to build it in the open.


To earn a living, we will charge the **Lint++** service at $20 per month for 100 lints. For more frequent users, we are still working on the details, but the idea is to charge based on the number of lints you translate to code. We believe this aligns our incentives with yours: we only make money when we make your app faster.


## The road to Million Lint 1.0


We are still in the very early days of experimentation! Million Lints focuses on solving unnecessary re-renders right now (but we understand this is[not a problem (opens in a new tab)](https://react.dev/blog/2024/02/15/react-labs-what-we-have-been-working-on-february-2024) for long 🫡) and will move on to handling slow-downs arising from the React ecosystem: state managers, animations, bundle sizes, waterfalls, etc. Our eventual goal is to create a toolchain which keeps your whole web infrastructure fast, automatically - frontend to backend.


We would like to[invite you on this journey (opens in a new tab)](https://million.dev/chat) with us to make the best possible web performance developer tool. Million Lint is our very first step. Try it out and let us know what pieces are missing!


## How can I help?


We are looking for talented frontend and pl/ml engineers to join us in the Bay Area.


At Million, we have a simple thesis for software performance – we can build tools that make *anyone* a performance expert. Developers should think only of shipping features and fixing bugs – not keeping their code fast. We plan to start with React, then extend to the broader frontend, backend, and other platforms.


If you feel like you missed the beginning of Million.js, get in on this. The fun of the beginning is how much you get to shape 🔜[My email](https://old.million.dev/cdn-cgi/l/email-protection#e889818c8d86a885818484818786c68c8d9e)


*the Million team +[Ivan Akulov (opens in a new tab)](https://twitter.com/iamakulov) fixing React performance one step at a time. Join us!*
