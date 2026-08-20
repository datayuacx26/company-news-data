---
schema_version: "1.0.0"
document_id: "980ef575a7c821bd4916bef84ae7670684f6c7c2919ff0f852d192edc4d43601"
company_key: "yc-photoroom"
company: "Photoroom"
source_id: "yc-photoroom-rss-f545fb403576"
canonical_url: "https://www.photoroom.com/inside-photoroom/picking-a-state-management-library-why-we-went-with-mobx-"
published_at: null
first_seen_at: "2026-07-23T22:00:21.557706+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:e986fd5d91169400d5e0b7ed8fb815ae8e07fc7ce82fd893e08d438796558484"
---

# Picking a state management library for a React app used by millions (and why we went with MobX)

Photoroom is an image editor used by tens of millions of users around the world. As our web app team was crossing 5-developers mark, we found ourselves running into more and more of the same issues: the whole app was rendering too often and we were frequently dealing with an endless list of` useEffect` dependencies.


What’s more, we were paving the way to introduce real-time editing: this implies a reliable shared state across iOS, Android, and the web.


We needed to rationalize state management in the app and migrate from the extensive use of` useContext` (with[Constate](https://github.com/diegohaz/constate) ). This article covers why we picked MobX, the transition and why we’re not looking back.


## Zustand vs MobX vs Redux


When it came time to take a decision that was probably going to impact the life of our app for the next few years, we used the tried and tested “measure twice, cut once” strategy


Members of the team pushed for[Zustand](https://github.com/pmndrs/zustand) but while we loved its simplicity, it also meant that it was less opinionated. We also considered the good ol’ Redux. The name triggered PTSD from copying boilerplate state code around 10 years ago (although I’ve been told the Redux Toolkit make it much less verbose these days)


What seduced us with MobX was its philosophy:[anything that can be derived from the application state, should be derived. Automatically](https://mobx.js.org/getting-started) . Which is great when you’re creating a photo-editing app with sliders, UI boxes, a lot of server calls to AI models. MobX works by setting native JavaScript **Proxies** on objects. If an object gets updated, it’ll know about it and only re-render your component if necessary.


## The transition


As a proof of concept, we started by using MobX in the most complex part of the app: the content sync (syncing user designs with the server). A lot of the logic around the state was already pure TypeScript, making it a good candidate. Converting this part of the codebase was painless (shout-out to Jimmy!), convincing us that MobX was a great match for our needs.


We then proclaimed a 'no new state shall be created without MobX' law inside the web team, ensuring no new feature was to be built with useContext. We created a little` Quick start and footguns to avoid` document to share our learnings and we had a few workshops to learn how to use our new state library.


The most common mistake is to forget to wrap your component / subcomponent / sub-sub-component in MobX’s` observer` helper. If you don't... nothing happens when the state changes. About 13` <p> state debug {store.myVariable} </p>` and a` git reset` later, you’ve learned your lesson and are now an official MobXer 🎉.


We then migrated existing state from` useContext` to MobX. It went smoothly but it required someone very thorough to ensure we were not missing any edge case (shout-out to Jonathan!).


The only road bump was the inability to use the` useQuery` hook from the amazing[TanStack Query](https://tanstack.com/query/latest) . Indeed, since your state is basically a class that does not live in the React world, you can’t use hooks. There are a[few](https://github.com/js2me/mobx-tanstack-query)[packages](https://github.com/mtsewrs/mobx-query)[available](https://github.com/zapolnoch/mobx-with-query) , but we went with our own wrapper. Here’s how you use it inside a MobX Store:


*Example of checking if a new version of the app is available inside a MobX store*


The same issue occurred with other libraries like Tanstack Router or the React i18next, but thankfully they offer interfaces to query their state outside of React


## What we gained from the migration


Now that the migration is almost complete, we’re clearly seeing the difference compared to the previous state of things (pun intended).


First **, testing became much easier.** With MobX, your state is basically a class with properties and methods. This makes testing incredibly easy, especially as you don’t need a DOM around to run your tests.


Second, **developers are much more comfortable** updating the state without having to grasp the re-render chain a one-line change will trigger. No more endless fiddling with the React developer tools to hunt for whole-page re-renders. No more circular` useEffect` dependencies.


Finally, **the state is updated in a few select methods** and we can enforce it. With a` useContext` state, any developer can decide to update the state in any location. Here's how we do it:


I’ll follow up soon with a blog post on how we to integrate our cross-platform internal SDK with the existing MobX store.


## The downsides


Finding developers with extensive MobX experience has proven harder than anticipated. Although MobX is about 10 years old, finding people who (1) worked with it, (2) on apps with complex state and (3) are based in Europe proved more complex than anticipated.


I’ve been told that the best way for that was to write a praising blog post. So here we are: if you’re matching all 3 criteria: please[apply here](https://jobs.ashbyhq.com/photoroom/81de4c1e-f4ee-4c14-a196-6e869fa6b320) . We’re nice people, work in English and we have amazing challenges to tackle. Add the word “banana” to your application so we know you’ve been patient enough to read until the end of this blog post.
