---
schema_version: "1.0.0"
document_id: "621e6a57e9e0624c749b85179db8c0c52f7a218f00e2736ec24b33a2ef206c5a"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/melodic-filters-article/"
published_at: "2017-12-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:24.699163+00:00"
content_hash: "sha256:b6b8f591c4e54c9da92cdf54b5507742f8f924ac9e8e7a188cc7d6e8f492f0ac"
---

# trivago just made filtering faster and more accessible, but why and how?

As we all adventure around this space that we call the Internet, consuming content is often on our minds. Naturally with the vast amount of data, filtering out what’s not interesting is a huge time saver. In order to help you[find your ideal hotel at the best price](https://www.trivago.com/) , trivago’s filters are one of the best ways to do so. Sadly, some visitors couldn’t even access them due to poor accessibility and performance.


At trivago over the past year,[we’ve had an accessibility revolution](https://tech.trivago.com/2017/09/26/accessibility-at-trivago/) . We worked to modify our existing components to become keyboard accessible, but, what happens when one of these components is based on the largest part of our legacy codebase? Not only does that seem like a maintainer’s Hell but also a waste of time and resources. Furthermore, that same legacy code is well, legacy and therefore has a tendency to be slow and inefficient.


---


> ## “Slow websites harm accessibility and user-experience.“


---


The component I’m referring to is the filter sidebar, which had been patiently waiting for the day to move away from trivago’s previous internal JavaScript framework; Harmony,[a fork of Marionette](https://marionettejs.com/) . After the birth of our new[JavaScript framework named Melody JS](https://melody.js.org/) in 2015, we started to move away from a fully-featured, render-blocking framework and towards one which simply assists with component rendering to avoid the same rendering issue; utilising Redux to handle global state.


Having rewritten several components to utilise Melody in the past, we knew to expect a significant performance increase however due to this framework difference, it required us to rewrite our filters implementation from the ground up in order to reduce maintenance overhead in the future and increase the feature’s accessibility.


Due to the slow performance of our legacy Harmony framework, many mobile devices cried when they were initially rendered with script evaluation and rendering time taking over 120ms. This was due to several factors but namely the huge 176KB bundle size which was loaded instantly, despite some functionality requiring several steps of interaction.


A team of enthusiastic developers and quality assurance engineers formed together and as their colleagues encouraged them with words of “finally!” and “it’s about time!” they worked together on the rebuild.


---


---


306 commits and 5 weeks later it was ready to merge to our Git master branch. After being prepared for release, it was followed by a few popped beer caps after the realisation of the effort set in. The result was a set of filters that whilst not vastly visually different, finally became interactive for more of our users as the loaded bundle was reduced to 53KB; a 70% reduction. Once gzipped that reduced further to 13KB.


This has been achieved by a congregration of different factors however part of the reduction came from simply deleting legacy code. We had earlier[dropped support for IE8](https://tech.trivago.com/2016/06/01/dropping-ie8-support-consequences-for-javascript/) and consequently we were able to drop an animation library named Velocity and rely solely on CSS. Additionally, we modified our “More filters” tab to be loaded on demand as it was previously loading 35kb immediately, irrespective of that the majority of users don’t interact with them that quickly. Overall the script evaluation and rendering time decreased to 16ms; an 88% reduction. Moreover, our filters are now completely keyboard accessible. In doing so, we have learned[a lot about accessibility](http://heydonworks.com/practical_aria_examples/#tab-interface) as the combination of tabs and accordions had us scratching our heads until doing some[research](http://www.oaa-accessibility.org/examplep/accordian1/) .


Your browser does not support HTML5 video.


[Melody JS](https://melody.js.org/) continues to show the benefits it brings to not only our Time To Interactive (TTI) metrics, but Time To First Meaningful Paint as another component had been successfully rewritten to utilise it. Our goal is to continue to port all our components to use it, so that we can continue giving you the best user-experience that we can provide.


You can now find out what drives trivago’s front-end as we recently open-sourced Melody JS ( *though it’s currently in beta, so documentation and dependecies are a work in progress* )! You can[check out the Melody JS website to get started](https://melody.js.org/) or alternatively get stuck right in by[visiting Github](https://github.com/trivago/melody) .


*Credits and thanks:[Malte Wessel](https://twitter.com/drbreak) ,[Radovan Janjic](https://twitter.com/radovanjanjic) ,[Adriano Ferracuti](https://twitter.com/adriferracuti) ,[Tobias Baldauf](https://twitter.com/tbaldauf)*
