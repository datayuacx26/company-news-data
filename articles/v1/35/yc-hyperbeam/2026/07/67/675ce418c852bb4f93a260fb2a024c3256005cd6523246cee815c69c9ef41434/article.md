---
schema_version: "1.0.0"
document_id: "675ce418c852bb4f93a260fb2a024c3256005cd6523246cee815c69c9ef41434"
company_key: "yc-hyperbeam"
company: "Hyperbeam"
source_id: "yc-hyperbeam-news-import-52ebed8f4b04"
canonical_url: "https://hyperbeam.com/blog/universal-rendering-bad-idea/"
published_at: null
first_seen_at: "2026-07-21T23:25:50.594051+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:c3b47c83699a41eba9ad8a3e8837046092d8361f02e4fec7a9f0a6aaf9c5fbdd"
---

# Why Universal Rendering / SSR is (almost) always a bad idea

You are having a great time using frameworks like React and Vue to build your app, but you have that one friend how keeps telling you about Universal Rendering and how it cured his depression, or you read this article once about how client-side rendering causes poor SEO. Now you are considering or have already started using a universal rendering framework like Next.js or Nuxt.js. But, whether or not you like it, Universal Rendering is (almost) always a bad idea, at least in our humble opinion.


Universal Rendering, sometimes called Isomorphic Rendering, is the process of running a client-side framework on the server to generate the initial HTML representation of the page, which is sent to the client, which analyzes the rendered DOM to build up state for the framework (rehydration), and proceed to render updates on the client side. Universal Rendering is also sometimes confusingly called "SSR" or server-side rendering, not to be mistaken with exclusively server-side frameworks which render static HTML on server side through string templating rather than running a client-side JavaScript framework on the server, with also no rehydration or subsequent client-side rendering steps. We'll just use the term "Universal Rendering" from here on to avoid any confusion, and to remind ourselves that rendering is done on both the client and the server, hence the term "universal".


In essence, Universal Rendering seeks to address some of the problems faced when having a completely client-side rendered application. However it's very important to realize that Universal Rendering is a **compromise** , and a bad one at that. It serves to keep all the goodness of client-side rendering while shoehorning extra complexity from server-side rendering to mitigate some of the shortcomings, while simultaneously creating new problems.


## "Universal Rendering is fast"


You often get told that client-side rendering is slow, especially on older mobile devices and on slower connections, because downloading and running the JavaScript that puts stuff on the page takes too long to execute. Let's say we use Universal Rendering to help with this — since the user now gets a preview of the initial render while the JavaScript loads and the framework boots up, they avoid having to sit through a loading wheel and be more likely to bounce. Savvy. But in exchange, now it takes **even longer** for the device to load the page completely since it has to do the extra work of downloading and rendering the initial content, slowing down the execution of the JavaScript which actually does all the subsequent work. But the worst part is the user cannot interact with the page until all the JavaScript is loaded and the event handlers are set, so whether or not the initial preview is better than a loading screen is subjective. To put it in numbers, the[Time to Interactive](https://web.dev/interactive/) (TTI) takes a big hit.


## "Universal Rendering improves SEO"


Gone are the days when client-rendered apps are a black box to web crawlers. Most web crawlers these days are capable of at least some degree of JavaScript rendering, and Google's crawler Googlebot is based on Chromium, which means it's actually **better** at handling JavaScript than your average browser (no Safari to drag things down). That being said, there are[things to be aware of](https://developers.google.com/search/docs/advanced/javascript/fix-search-javascript) , and you need to be wary of the fact that JavaScript crawling is a lot slower than regular crawling, so changes on the site could take longer to reflect in search results. Hence, in theory, Universal Rendering is superior when it comes to SEO, but by how much, and does that matter?


The answer is, in most cases, it's not better enough to be worth it if SEO is the only factor driving your decision. First, you need to determine which pages of the site need to be crawled — usually, this is the lander and the links on the lander, like the terms of service, privacy policy, contact, FAQ, etc. You can ensure that just these pages render fast enough using Lighthouse. The main issue pointed out by Lighthouse would likely be "unused JavaScript", since you would not be using any code related to the actual application. You can leverage code-splitting to remove unused application code and libraries. Optimizing these page would also benefit users visiting your site to boot.


## "We at <big company> use it, and so should you"


Just because they do doesn't mean you have to. The processes of a big company are very refined and when they choose to do something, it's because they have very specific needs and are willing to compromise so that they can achieve a bigger goal, like increased developer productivity. Sometimes thought, things do backfire,[like it did with Netflix](https://medium.com/dev-channel/a-netflix-web-performance-case-study-c0bcde26a9d9) , where universally rendered React affected conversions on their lander.


## Do you like being joined at the hip with Node.js?


Universal Rendering needs a server-side JavaScript context to function, which in most cases is Node.js, and its single-threaded event loop model means you would eventually run into scaling problems. Hence, you should not treat this Node.js instance as your "backend server", but as a stateless server that would hit your actual backend with all the state, so that it's easy to scale. Or instead, you could go with a hosted solution like Vercel for Next.js. The third option is to rely on prerendering, also known as static site generation or SSG, which involves saving the server-side rendered HTML to static files and then statically hosting the generated files for cheap.


## Why Universal Rendering didn't work for us


We tried our hand at Universal Rendering, and yes, it was very fun to use and very addictive. The main motivation was productivity features like hot reloading which come standard in most JavaScript frameworks these days. We first used Nuxt.js, since I (cofounder and CTO of Hyperbeam) had used it before and liked it. We used it for the then-new version of our lander. We started off with full Universal Rendering, and decided to move to prerendering when we started getting hit with a lot of traffic for more stable response times. But the most troubling thing we saw was the high TTI value on Lighthouse for mobile.


Although marked in green, the troubling matter was the 1.2 seconds between the First Contentful Paint (FCP) and the TTI, during which time the page won't respond to the user. This non-responsiveness is measured by[Total Blocking Time](https://web.dev/tbt/) (TBT), which is the amount of time for the main thread is blocked between FCP and TTI. If we see the breakdown of time spent loading the page, the burden of JavaScript is evident:


While we knew this was an inherent issue with Universal Rendering, we didn't expect it to be this bad — there were more mobile users hitting our lander than desktop users, but the bounce rate was higher, and as a result we saw terrible conversions on mobile.


## What we did instead for our new lander


It was clear that we needed to make the lander fast and lean to ensure mobile users aren't having an inferior experience. We realized that we weren't making full use of Universal Rendering — we weren't doing anything dynamic on the server side, which made prerendering an easy switch for us, and we weren't using enough Vue on the frontend to make it worthwhile to keep using a frontend framework in the first place. Inspired by Netflix's experience, we decided to go plain HTML with as little JavaScript as possible, while retaining the developer experience we had come to love with Nuxt.


### Good old server-side rendering


There are a lot of static site generators out there, like Hugo, as previously mentioned, but those are more blog-oriented and we just needed a simple HTML page with nothing fancy. We took a page out of how Universal Rendering does SSG by just running the server and crawling the pages at build time — we built a simple server-side rendered application, by which I mean OG server-side rendering with HTML templating, with a build mode which builds all the HTML and writes the output to disk. Since we are talking about a frontend project build system, we just chose to use Node.js, since it has a lot of packages for this kind of stuff already. We picked EJS as the templating engine since it's simple and easy to use and is based on plain JavaScript.


### "Hot" reloading


Another consideration was we needed some kind of hot reloading to see changes as they are applied. This meant that we need some kind of dev server that listens serves the development version of the site which updates on the fly as changes are made. Since we don't have any JavaScript state to worry about, the only things that needed updates were the HTML and CSS, and a simple page refresh would update the HTML and CSS. Wait, that's cheating, refreshing the page is not hot reloading! I know, but it gets the job done and I'm too lazy to figure out actual hot reloading.


```text
<  %    if    (  hot )    {    %  >
<  script >
const   evtSource  =    new    EventSource  (  "/hotreload"  )
const   scroll  =   sessionStorage .  getItem  (  "scroll"  )
if    (  scroll )    {
sessionStorage .  removeItem  (  "scroll"  )
const    [  x ,   y ]    =   scroll .  split  (  ","  )  .  map  (  Number )
const    {  body }    =   document
body .  scrollLeft  =   x
body .  scrollTop  =   y
}
evtSource .  onmessage    =    (  )    =>    {
const    {  body }    =   document
sessionStorage .  setItem  (
"scroll"  ,
`  ${  body .  scrollLeft }   ,  ${  body .  scrollTop }   `   ,
)
location .  reload  (  )
}
<  /  script >
<  %    }    %  >
```


*Cold* reloading


The only state we persist between reloads is the page scroll state, so we can snap back to the same scroll point after refreshing. When building for production,` hot` is set to` false` so that this script tag is not included in the generated HTML. Also, if you haven't seen` EventSource` before, it's part of[Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) (SSE).


### The CSS


We have a single CSS file built from a bunch of SCSS files. We don't need to have individual CSS files for each page since a vast majority of the styling is shared between all the pages. During dev mode, we inline the CSS in a` <style>` tag, and during build mode, we use a CSS file which can be cached between page visits.


### But are we any faster?


Yes, a lot faster! Here's how the new Lighthouse mobile performance scores look like:


We see a huge improvement across the board, with a one second speedup in FCP and a two second speedup in TTI. There is a regression in Largest Contentful Paid, but it's not a cause for concern since it's dominated by the previously absent below-the-fold content which is hidden from the user on page load. Since we don't have all that JavaScript to run anymore, there is no time wasted scripting, resulting in a big improvement in TBT as well. Here is the current breakdown of time spent:


### Where this brings us now


We ditched frameworks for a plain HTML approach since we don't have a lot of DOM manipulation happening on our lander, and we do simple client-side rendering using Vue for our webapp located at[hyperbeam.com/app/](https://watch.hyperbeam.com/app/) since we want it to be fully functional as fast as possible. So it's clear that Universal Rendering is not a good fit for us, and if you are in a similar situation as us, it's likely not a good fit for you either.


## Not yet convinced?


You might still have some reservations about reconsidering Universal Rendering, which is understandable, since it's incredible addictive. And of course, since Universal Rendering is only *almost* always a bad idea, there are good use cases for it, such as a blog or a news site — users visits your site to read, so getting content on the screen ASAP is the highest priority, and they won't be impacted by the long TTI as they are not going to interact with the page right away.


But compare that with a site like YouTube, which is very interaction-heavy. Considering they use Polymer, which is very coupled to browser APIs and hence can't easily be run on the server side, they clearly rejected the idea of full-blown Universal Rendering early on. That, plus considering how horrendously slow and bloated the new YouTube client is (a rant for later), they can't just show nothing while the app is loading, so they make use of a lightweight skeleton loading screen in the meantime. Additionally, YouTube also optimizes the most important part of the watch page, the video player, to load first, while the rest of the app loads in later.


YouTube skeleton screen


You can do the same with your client-side rendered app


```text
<  body >
<  div id =  "app"  >
...
<  div  class  =  "skeleton"  >  Lorem Ipsum <  /  div >
...
<  /  div >
<  /  body >
```


Once the framework is done booting up, it will replace the` #app` entry point with the full app UI, and in the meantime, the user gets to see your skeleton UI, giving them feedback that the app is loading. But don't get too excited about stealing more ideas from YouTube since it otherwise sets a very low bar for loading performance.


## Conclusion


We believe that in most use cases, Universal Rendering would lead to a sub-optimal user experience, on top of being a maintenance burden in the form of extra complexity, one more thing that could go wrong. With all the time you'll save from avoiding Universal Rendering, you could maybe give[Hyperbeam](https://watch.hyperbeam.com/) a try!


[← Go back](https://watch.hyperbeam.com/blog/)
