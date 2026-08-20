---
schema_version: "1.0.0"
document_id: "41e5753e8e32244d064aadcbf70591982689483f14f85c8737bd1dcf61680a56"
company_key: "yc-same"
company: "Same"
source_id: "yc-same-news-import-393e713dbea1"
canonical_url: "https://million.dev/blog/million-3.en-US"
published_at: "2024-02-02T00:00:00+00:00"
first_seen_at: "2026-08-10T02:52:25.226434+00:00"
fetched_at: "2026-08-10T02:52:27.075037+00:00"
content_hash: "sha256:52b7c62750a6b3fd01258267946bff7018abddb014eb8eae59eaf0bb45e61649"
---

# Announcing Million 3

[Blog](https://old.million.dev/blog)


Announcing Million.js 3.0


# Announcing Million 3


[AIDEN BAI (opens in a new tab)](https://twitter.com/aidenybai) ,[NISARG PATEL (opens in a new tab)](https://twitter.com/nisargptel) ,[JOHN YANG (opens in a new tab)](https://twitter.com/fiveseveny) – FEB 2 2024


---


After many months of development ( *and one[soft launch (opens in a new tab)](https://twitter.com/aidenybai/status/1732812329434423647) later* ), we are **so excited** to finally release Million 3. Thousands of hours have gone into this release by the Million community, including the core team, contributors, and many Discord members who have helped us test and provide feedback.


## What is Million?


Million is an optimizing compiler for React. The React virtual DOM represents the user interface (UI) as a tree. Every time a component (node) renders, React traverses the virtual DOM tree to update the UI, resulting in` O(n)` time complexity. As your website grows, this can lead to sluggish user experiences.


Million takes a fundamentally different approach. It still represents a UI as a tree, but it discriminates between nodes. In an application, some nodes will never change (static text, images, etc.), while others will change frequently (user input, dynamic data, etc.). Instead of traversing every node, Million uses a compiler to directly update dynamic nodes, resulting in` O(1)` time complexity.


Since launching Million 1 & 2, we have found that this approach works exceptionally well for data-heavy UIs, like dashboard with real-time information. However, there were certain caveats – some common libraries were not compatible, non-deterministic returns could not be optimized, and the compiler was not as stable as we would like.


Million 3 is a signficant update that solves these issues. We believe that Million 3 is the best way to build React applications, and we are excited to share it with you.


## What's new?


### Performance


The biggest challenge with Million 2 was hydration. In React, hydration is the process of attaching event listeners to the server-rendered HTML – making the page interactive. Just like how the virtual DOM needs to be traversed, hydration is also` O(n)` time complexity. This means that as your application grows, hydration can become a bottleneck.


```text
// Normally, React SSR will traverse every node in the component (👎 ❌)
<  div  >
<  h1  >Hello, world!</  h1  >
<  button     onClick  =  {handleClick}>{count}</  button  >
</  div  >
```


In Million 3, we have introduced a new hydration system that only traverses the parts of the component that are dynamic, resulting in` O(d)` time complexity (where` d` is the number of dynamic nodes,` d` ≤` n` ).


```text
// Million 3 only hydrates `handleClick` and `count` (✨ ✅)
<  div  >
<  h1  >Hello, world!</  h1  >
<  button     onClick  =  {handleClick}  >  {count}  </  button  >
</  div  >
```


We are also currently working on removing` <slot>` elements. In Million 2, this was necessary to mount blocks and portals properly, but often resulted in extra memory overhead and issues with parent-dependent styling (such as` flex-box` or` grid` ). We expect to ship a full revamp of this system in the next minor release.


### Stability


One of the major focuses of this release was to improve the stability of the developer experience.


Million 3 is a complete rewrite of the compiler. We have refactored based on correctness, to cover significantly more edge cases. These include: better TypeScript support, multiple returns, conditionals support, and handling of nested React components. This means that you can expect a more seamless and stable experience when using Million 3.


In the next few minor releases, we will be focusing on improving the performance of the compiler. This will make Million 3.x even faster and more reliable.


### Docs & i18n upgrade


The official[million.dev (opens in a new tab)](https://million.dev/) site has gotten an overhaul. We have added a new i18n system, so the site is now available in multiple languages! In addition, the documentation has been reorganized to show automatic mode as the default and manual mode as an advanced feature.


## Upgrading to version 3


Your current 2.x.x code should work with 3.x.x with no changes. To upgrade, simply run:


```text
npx     million@latest
```


That's all!


## The road ahead


In the coming weeks, the Million team will ship a suite of developer tools to redefine how frontend engineering teams approach debugging


, fixing, and maintaining web performance. Our mission is to enable developers to deliver fast software effortlessly: with any system, on any platform.


Today, dealing with performance issues is a nightmare. The status quo is navigating through React Devtools,` Profiler` , Chrome Devtools,` why-did-you-render` , Forget, and the lot. This is a fundamentally broken experience, leading to some developers spending more time debugging rerenders than shipping features.


Even within the React ecosystem, there is a basic lack of understanding on how to enable developers to build and maintain fast applications. Frameworks like Next.js and Gatsby have made it easier to build performant websites, but they can't optimize inefficiently implemented code. Hosting services scale as your application grows, but they can't fix a poorly designed architecture.


We need to build tools that make it easy to deliver fast applications, regardless of the framework, platform, or the size of your engineering team.


At Million, we have a simple thesis for software performance – we can build tools that make *existing* tooling fast and easy to use. Developers should think only of shipping features and fixing bugs – not keeping their apps fast. We plan to start with React, then extend to the roader frontend, backend, and other platforms.


I invite you to join Million on this journey. We are looking for talented frontend (dev tools) and pl/ml engineers to join us in the Bay Area. If you are interested, please email[\[email protected\]](https://old.million.dev/blog/aiden@million.dev) with your resume and something you built using Million.


Let's make the future of software fast, together.


## Acknowledgements


Thank you to the many contributors who made this release possible. Special thanks to[@lxsmnsyc (opens in a new tab)](https://twitter.com/lxsmnsyc) ,[@nisargptel (opens in a new tab)](https://twitter.com/nisargptel) ,[@fiveseveny (opens in a new tab)](https://twitter.com/fiveseveny) ,[@melindachang (opens in a new tab)](https://github.com/melindachang) for their contributions to this release.


Feel free to ask questions and reach out to us on[Twitter (opens in a new tab)](https://twitter.com/milliondotjs) or[GitHub (opens in a new tab)](https://github.com/aidenybai/million) .
