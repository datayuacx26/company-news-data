---
schema_version: "1.0.0"
document_id: "a7b3f71515b48b4323b21c9745bab54404ce70f17b9235d2666de688c9176b1c"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2016-08-19-js-startup-improvements/"
published_at: "2016-08-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:d29a35f14d353b9aab47e89e28c4a62de2814094dfa5623ee6ca34985a6eaf10"
---

# Prioritizing resources for a faster app startup

### Up to 4.5 times faster startup achieved on trivago.com


We’ve prioritized the resources that our users need to load and gained an impressive boost on our JavaScript application startup. In this article, I will explain how we’ve used Webpack, adjusted our handling of SVG icons and prioritized user needs to achieve up to 4.5 times faster startup.


Earlier this year, we ran a test on our website. We wanted to find out what would happen if we left the screen white until our JavaScript application had been fully loaded and was ready to interact with. This essentially meant undoing the optimizations we had made to improve our time to first render and something that we all expected to be extremely negative.


We were wrong. Our users preferred to look at a blank screen until the application was ready to be used over being able to interact with it while it was not completely started.


After some careful analysis, we noticed three problems:


1. There was a bug in our search field that caused user input to be removed upon application initialisation.
2. We were loading a lot of lower priority resources before actually starting to load our application.
3. The size of our assets was too big and took a while to load on a 2G connection.


The first problem was easy enough to solve. We located the bug and are currently testing the fix. However, the other problems required a plan, and we needed every team to contribute to an improvement of the situation.


Before going into the plan itself, let me share the result with you:


After the changes we have done so far, our application loads faster on a 2G connection than it did before on a 3G connection. Specifically, by “Application Startup”, we mean the time between receiving the first few bytes of HTML and the start of JavaScript execution.


As you can imagine, I am quite excited about this result, especially because we still have some improvements in the queue.


So, how did we achieve this?


## Step 1: Reduce asset size by loading features on demand


One of the reasons why we invested into[adopting Webpack](https://tech.trivago.com/2015/05/28/introduce-webpack/) as our asset bundler is because of its integrated ability to load parts of our application on demand. We have been using this for quite some time, however, this time we actually implemented it for some of our more prominent, and complex, features like Slideouts, Gallery, Filters, Maps and others.


We also created separate build targets for the two versions of our search field that we were testing at the time and rewrote some simple but expensive code parts.


As the picture above shows, we managed to reduce the amount of JavaScript initially delivered to our users by almost 50%. Of course we also used Webpack to load the CSS for these features on demand, leading to a significant reduction of the initially loaded CSS as well.


Our users loved this improvement and we saw an almost 2% increase in revenue on mobile devices.


## Step 2: Asset caching


One way to deliver large assets faster is to just allow the user to cache them locally. In the past, every single one of our releases invalidated the user’s cache due to changing file names. Again, just by using the tools that Webpack provides for us, we were able to implement long term caching for our assets so that users only need to download them again if they were affected by the current release.


As a result of this change, our users have been able to load our base libraries from cache for the last couple of months.


## Step 3: Inline SVG icons


Introducing SVG icons instead of CSS sprites was a big win for our developers, designers, and a key element of our famous[Project Ironman](https://tech.trivago.com/2016/02/02/large-scale-css-refactoring-at-trivago/) .


While this had already been an improvement, we decided to take it one step further and instead of loading the SVG icons as part of a CSS file, we decided to[implement inline SVG icons](https://tech.trivago.com/2016/09/05/inline-svg-icon-system/) with a fallback solution for browsers that don’t support SVGs. My colleague Radovan Janjic will soon offer a follow up article, describing the challenges we faced during this project and also explaining the benefits we’ll gain from it in the future.


For now, let me just summarize by mentioning that we got rid of another ~14% in asset size and gained 10% in time to first render.


## Step 4: Prioritizing users over collecting data


As any other web app out there, we need to do some user tracking for our analytics to work. Without the data, we can’t improve the website for our users.


All of our trackers were being loaded async and should not have blocked anything. However on a slow connection, there is only so much data that can be downloaded at any given time. Especially on 2G connections that meant that the trackers were practically blocking the load of the application.


The real question here is not whether we really need to do it. In reality, the question boils down to priorities. Does it matter more to collect the necessary data or is it more important to offer a pleasing user experience?


At trivago, there was no discussion. As soon as we proposed to prioritize the application startup over loading our trackers, we got the green light.


As shown in the image above, this change reduced the time for loading 3rd Party resources quite a lot.


The result, once again, was pleasing. We were able to save a lot of time during startup on slow connections just like we expected.


## Step 5: Delay loading of images


When accessing[trivago.com](https://www.trivago.com/) you might have noticed that you can scroll down some more. And once you do so, you’ll find an area with inspirational images of top destinations. These images also blocked the loading of our JavaScript application. So we did what we had to and started loading them once the application had been loaded. The first results for this test look very promising and could also lead to a significant increase in revenue, once again highlighting that our users are happy with these changes and interact more with the page as a result.


## Are we done yet?


Well, of course not. But these steps have already lead to a massive improvement and we managed to do all of this with a coordinated effort from all of our teams without impacting their usual feature development work. We also managed to get all obstacles out of the way so that the browsers can start loading our assets as soon as possible even if they are on a slower connection like 2G or 3G.


In the upcoming months, we will start using the new possibilities offered through using inline SVG icons and we will continue to reduce the size of our JavaScript while making it faster, more efficient and more powerful at the same time.


Don’t know about you but I will have an Espresso now.
