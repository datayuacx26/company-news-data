---
schema_version: "1.0.0"
document_id: "c5f99a59ab8b4855e29fe20ea04dcb31920c0503bd4d20caafcb6541a47420f9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/introducing-alerts-in-apollo-engine-e32eeb03b7fa"
published_at: "2018-04-10T20:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:d4f6664923d54bbf11336d9aab8fd4f08a65445cf42b2160f250ba3de683658b"
---

# Proactive alerts for your GraphQL API

GraphQL makes it incredibly easy for teams to request any data that they need to build a feature. This flexibility is great for shipping fast, but when you get into production, you need to be able to respond in a flexible way, however your schema is being used. Wouldn’t it be great if you had a tool that would proactively tell you when your service was slowing down or was running into errors? With[Apollo Engine](https://www.apollographql.com/engine/) ’s new Alerts feature, now you do!


*If you haven’t added Engine to your API yet, read the*[setup directions here](https://www.apollographql.com/docs/engine/setup-node.html) *or learn more about what it does*[on the website](https://www.apollographql.com/engine/) *.*


Stop those pesky bugs as soon as they happen!


## How alerts work


Apollo Engine gives you insight into how your GraphQL API is performing. You can get a service-wide birds-eye view, or look into each GraphQL operation in detail. Engine alerts allow you to set thresholds on this expansive set of timing data, and send notifications to Slack or PagerDuty when problems arise.


### Flexible Configuration


Alerts can be configured on a number of different thresholds:


- Request rate — requests per minute
- Request duration — p50/p95/p99 service time
- Error rate — errors per minute
- Error percentage — the number of requests with errors divided by total requests


Engine lets you configure these thresholds on the entire service, or on individual operations. That pesky front page feed query will never be slow again with Engine on guard.


Selecting alert triggers in Apollo Engine


### Proactive monitoring


The triggers you select are evaluated on a rolling five minute window. This means that if you have an alert configured to trigger when the` HomeFeed` operation’s error rate exceeds 5%, and 6 out of the 100 requests that were sent through Engine in the last five minutes resulted in an error, then the alert will trigger with an error rate of 6%. Once you have put out the fire with the information provided by Engine, the alert will resolve after the five minute rate falls back into acceptable levels.


## How to set up alerts


Setting up alerts in your Engine account couldn’t be easier! There is a new **ALERTS** tab when you look at your service. I know you have been dying to click on it, now is your time to shine!


It may not be a big red button, but c’mon, you know you want to click it


After turning on alerts, you will be greeted by a friendly prompt to setup your first alert channel. Choose either Slack or PagerDuty and put in your connection information.


Say hello to your new favorite slack bot 🤖


Now Engine will suggest a couple handy alerts as a way to get started. By default it includes a performance alert for your entire service, and a critically helpful error rate trigger. You don’t want to be in production without Engine by your side!


## Monitor what matters


The default alerts are great starting point, but the real value of this feature is to keep an eye on what matters most to your users. Using Engine’s performance data and error tracking, you should pick trigger values that matter for your entire service and for key operations. That` HomeFeed` query from earlier should always be fast, but the` SignUp` mutation should never fail! So let Engine help keep those goals in check without you having to watch dashboards all day long.


Engine has your back for what matters the most


Setting up per operation triggers in Engine is just as easy as service wide ones. You can either set them up on the service level by clicking the operation name in the list, or go to the operation you want to alert on and setup the alert there.


Often times, you expect a certain operation to be slower than the rest or be more prone to errors. Other times, you want to have a more stringent alert on a critical operation. In these cases, you can configure more appropriate alerts for these operations individually. The service level triggers are great safety nets to make sure schema wide things are running smoothly.


### Alert wisely


No one wants to be woken up at 3 am because a developer in a different timezone forgot a closing bracket on an operation in GraphiQL. In the same vein, if error rates spike suddenly, a slack mesage when everyone is out to lunch won’t do much good. With Engine you can select the channel of notification that you want for each alert. This means its easy to warn the product team that things are looking slow in Slack but also super simple to wake up the on call team when things are on fire


## Try it out today


Alerts are part of the new paid Engine Standard and Pro plans. You can easily try things out on any of your Engine services with a free trial, without putting in a credit card. Turn them on and rest easy knowing that Engine is watching your app for you!


*Apollo Engine is a GraphQL gateway that helps you operate and understand your GraphQL API in production with features like performance tracing, error tracking, and result caching. If you haven’t added Engine to your API yet, read the*[setup directions here](https://www.apollographql.com/docs/engine/setup-node.html) *or learn more about what it does*[on the website](https://www.apollographql.com/engine/) *.*


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
