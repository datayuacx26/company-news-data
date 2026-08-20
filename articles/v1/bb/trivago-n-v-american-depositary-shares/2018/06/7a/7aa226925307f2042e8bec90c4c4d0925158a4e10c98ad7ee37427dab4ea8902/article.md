---
schema_version: "1.0.0"
document_id: "7aa226925307f2042e8bec90c4c4d0925158a4e10c98ad7ee37427dab4ea8902"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/introducing-melody/"
published_at: "2018-06-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:50df6fa634b9c2c764c6304ed67cbfe15a7e6d64c06145fa713730cdc24b89c2"
---

# Melody - the sound of JavaScript for our Hotel Search

For a long time, we’ve mentioned that we’re running our own JavaScript framework:[Melody](https://melody.js.org/) . Today, we are happy to share the framework we’ve been using since 2016 as an Open Source project with you. We designed Melody to be fast, memory efficient and to be flexible enough for the future.


## Where we came from


Before introducing Melody, we were using a custom[Backbone](http://backbonejs.org/) fork with minor enhancements. On the Backend,[Symfony](https://symfony.com/) - a PHP framework - used[Twig](https://twig.symfony.com/) templates to perform server side rendering. In the browser, we used[Swig](https://github.com/paularmstrong/swig) , a Twig-like template language, to build up HTML strings. Those were injected into the DOM using` innerHTML` .


This combination of libraries allowed us to render the same templates on the client and the server. It also helped us to reduce accidental bugs from mismatches between the two.


Having only one set of templates gave us a huge increase in productivity and possibilities. Features that had been hard or impossible before became easy. All the bugs from mismatches between client and server disappeared from our backlogs.


Yet, it soon became obvious that we weren’t doing right by our users. The` innerHTML` updates and handling child views and event handlers, were too slow for them. Thus, we replaced it with direct DOM manipulation for some cases. That fixed most of the performance issues but reintroduced the out-of-sync problem. Of course it also increased the complexity of our code base again.


## A new solution - Requirements


We started to think about a new, ideal solution and came up with some requirements.


- Memory efficiency
- Rendering performance
- Compatible with Twig
- High productivity for our developers
- Small size
- Incremental adoption possible


Our product should be accessible regardless of internet connection or quality of device. Thus, performance, a low memory usage and a small size for the framework was critical to us.


We also wanted to be able to switch to it without a full rewrite and gain benefits right from the very first feature. Furthermore, it had to be compatible with the Twig templates we were using on the server.


## Introducing Melody


Melody is our solution for Client Side Rendering. It fulfils all the requirements mentioned above and has been in use at trivago since 2016.


It consists of three parts: a Twig compiler, a runtime, and a component API.


The compiler targets the runtime to produce the rendering code. Our component API leverages higher order components which are familiar to React developers.


We have an incremental-dom runtime as well as a[JSX compiler target](https://melody.js.org/documentation/advanced/jsx) for using[React](https://reactjs.org/) and[Preact](https://preactjs.com/) . For trivago, we are using` melody-idom` due to better performance in our experiments. Melody’s architecture allows replacing every part with something else with low effort.


` melody-idom` consists of simple instructions, such as “open an element here”. Those instructions compare the expected shape to the real DOM on the fly, while traversing it. This avoids re-creating a virtual DOM on each render and a separate diff is also not needed. The low memory overhead pays back in good performance.


Over the past 2 years, we have converted most of our frontend to use Melody. Every time we ported a new feature, our users appreciated it by using the feature more often. For example, when we ported our Gallery to use Melody, we noticed a 7% increase in Gallery usage.


Many types of bugs that we used to fix on a daily basis have completely disappeared. Memory usage is at an all time low and our product works on most devices without significant issues.


Today, most of our frontend uses Melody, except for a few parts where plain JavaScript is enough.


## What’s next?


The Web is a fast moving environment, filled with innovation and possibilities. Melody’s architecture enables us to experiment and to stay up to date with the current state of the art.


We are researching a new component API around[Custom Elements](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements) and[RxJS](https://github.com/ReactiveX/rxjs) . Our goal for this new API is to make asynchronous loading of components much easier. But also to produce a more consistent code base by removing higher order components. We expect a positive impact on our productivity.


Of course we are also watching the innovative work from the[Ember community](https://www.emberjs.com/) . Their[Glimmer VM](https://glimmerjs.com/) is a remarkable piece of engineering and has the potential to set new standards. We plan to experiment with bringing similar optimisations to Melody as well. Removing the code that the Compiler produces for our templates, will allow us to trim down our JavaScript bundles even more.


A Node.js renderer for Melody is also planned but we didn’t need it so far.


## Who is it for?


Melody is a framework made for trivago. It is a fantastic choice if you have similar requirements as we do. If you want to render the same templates on client and server without Node.js. If you need low memory usage but great performance. These requirements make Melody a good fit for us and maybe also for you.


At the moment, using Melody is an adventure. It’s not easy to set up and you might run into bugs that we’ve not identified yet. If you fancy an adventure though, Melody has a lot to offer. Its expressive nature helped us to reduce our code by 40% on average for the features we converted.


You can find out more about Melody on its website:[https://melody.js.org](https://melody.js.org/)
