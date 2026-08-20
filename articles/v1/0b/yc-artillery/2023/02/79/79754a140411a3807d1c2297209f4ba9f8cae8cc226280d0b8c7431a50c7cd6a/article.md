---
schema_version: "1.0.0"
document_id: "79754a140411a3807d1c2297209f4ba9f8cae8cc226280d0b8c7431a50c7cd6a"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/load-testing-in-production-part-2"
published_at: "2023-02-17T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:d95812e76d79d3005a0567b63962f747ac324b38b933cf6a59951792f1823b15"
---

# Load testing in production, part II

February 17th, 2023[How to](https://www.artillery.io/blog/tag/howto)


# Load testing in production, part II


Hassy Veldstra


Welcome to Load Testing In Production, Part II! We’re continuing from[Part I](https://www.artillery.io/blog/load-testing-in-production) in which we looked at **why** you may want to load test in production, and common objections and dangers and ways to mitigate them.


Before we continue and look at how we’d go from zero to a production load test, let’s take a quick look at different ways of generating load in production. There’s more than one, and the 3 most commonly used methods are:


## Types of production load tests


1. Traffic replay
2. Adding synthetic traffic
3. Dark traffic


Artillery’s main use-case is **adding synthetic traffic** , but it’s useful to know what other options exist.


### Traffic replay


Traffic replay is a method where you capture production traffic (or a subset of it) and replay it (sometimes with an amplification factor), back onto production.


This method can work extremely well, but it’s not versatile. The system under test should satisfy the following requirements:


- All requests must be idempotent, i.e. sending the same request over and over again should yield the same result. This usually means no state modifications of any kind on the backend.
- All requests should ideally be commutative, i.e. the exact order of requests is not important, and there are no dependencies between them.


For websites, a classic example would be a news website without much personalisation, or a classifieds website. For an API, a good example might be an analytics event ingestion API which receives JSON payloads describing some discrete events.


There is one obvious danger here: creating a self-feeding loop that blows everything up. The way to avoid that is by being able to distinguish real traffic from replayed traffic at the capture point, and having some intelligence in the traffic replay layer to make sure it does not exceed a certain threshold of extra traffic above real traffic.


### Synthetic traffic


This method works best for systems which expose a transactional interface, i.e. where later requests can depend on earlier requests. An example is an API with an authentication step, where the first request may request a token which is then included with every subsequent request, or something more complex like a headless CMS system or an e-commerce system, where a client searches for keywords, browses results returned based on those searches, adds products to basket, and eventually goes through a checkout process. Essentially anything with a session of some sort.


For these systems, what you would do is write definitions of common user journeys and scenarios, parameterized and randomized to send realistic requests to the system, and run them against production. This is the use-case that Artillery was designed for.


### Dark traffic


Dark traffic is a technique that can work really well for certain types of new components in an existing production system. The idea here is that a new service or endpoint is deployed to production alongside everything else, with real traffic going to it, but without it being exposed to users or being on any kind of a critical path for existing functionality. An example would be a new event logging endpoint which is hit on every page load on your website, but which is completely decoupled from everything otherwise, so that if it goes down - users don’t even notice. You’re effectively load testing with real traffic in this way.


## Goals for production load testing


Usually, there are two main motivations for running production load tests:


1. A big traffic event coming up, eg. Cyber Week in e-commerce, Election week for a publisher, fashion week for Vogue etc. You have projections of traffic and you want to make sure you can meet it. A launch of a new service would fall into this category as well.
2. Building a margin of confidence and safety, e.g. run 25% extra traffic in production so that you know there’s always that safety margin. This can be sliced and implemented in a variety of different ways, for example:


- Run on a schedule, e.g. weekly, with extra load added to go above weekly natural peak
- Run extra load continuously as a percentage of real load or a fixed margin above weekly peak


These production load tests can also serve as a guardrail around code and config changes deployed to production.


Teams often find that they shift between these over time, e.g. you can start by running load tests before a busy period with a lot of human oversight, and then continue running them regularly afterwards. You can start with weekly tests, and as you improve automation and failsafe mechanisms, you start running them daily, or maybe even on certain releases to prod automatically and with no human oversight.


## Production load testing is only 50% about technology


The other 50% is the social side of it. You will need to talk to a lot of people. Publicise your production load testing initiative across the company. Talk to teams that may want to know or be affected by this extra production traffic, teams outside of core engineering like ad and analytics teams, data teams, marketing, front line customer support and so on.


### Pre-requisites and requirements


Let’s look at what we need to have in place *before* we consider production load testing.


As mentioned earlier, we need the ability to monitor our production SLOs and KPIs in real time, and be able to see at a glance when we’re getting close to breaching one of them. We also need to decide on what our tolerances are, or what our abort condition is. If for example your response time SLO is p95 of 500ms calculated over a sliding 5m window, are you OK to breach that SLO for 5m or 10m while a load test runs, perhaps to give autoscaling time to kick in? If the typical value is 300, you might say stop the test once it’s exceeded by 10%. You can define similar stop conditions for other SLOs, such as the ratio of 5xx responses to total requests for example. The key here is that the operator — whether that’s a human to start with, or an automated system — can continuously and unambiguously check that the load test is not causing issues.


**If you don’t have this, load testing in production is too dangerous.**


In a similar vein, you should have monitoring across all layers of your system, from CDN down to EC2 instances that make up your Kubernetes worker pool and everything in between. You’re running a production system, at scale, so you probably have that in place already.


**Request tracing and metadata propagation across the system** - again, this will help us distinguish synthetic traffic from real traffic where needed. Without this capability the surface area of the system you will be able to load test in production is going to shrink. It’s a worthwhile investment to make if you don’t have that in place already.


Don’t forget your monitoring system here. Let’s say you use Datadog. You want to be able to plot a timeseries which shows all requests to production, and then split it by real traffic vs load testing traffic. The way to do that is through metric tagging, the exact mechanics may depend on how exactly you’re tracking metrics, but it should be possible.


This goes without saying, but you should be able to stop your load test almost instantly when needed. This is some hard-won experience right there. We were running tests one day, and we did have the ability to stop them, just not instantly it turned out, and that five minutes can feel like a loooooong time when you’re seeing error rate on prod start climbing.


## Roadmap - getting to your first production load test


So, what will this look like in practical terms? You starting point is that you are not running any load tests in production. Let’s say you don’t run any load tests whatsoever.


**The overall idea is to iterate, and communicate your progress to teams & people who may be interested.**


The first thing you want to do is pick your load testing tooling. We have covered the evaluation criteria you can use in[Part I](https://www.artillery.io/blog/load-testing-in-production)


In parallel, start talking to relevant people and teams. Usually a project like this is championed by either the SRE or the SQA team (or their equivalent) in your organization. Obviously, we want to include dev teams in charge of any services and components which are going to be hit with extra load. We want to let data and analytics teams know well in advance too and get as much feedback from them as possible - their data analysis and reporting pipelines are likely to be affected. Someone from marketing and product should be included too - if for no other reason than to let them know how seriously you’re taking reliability. In my experience, almost no one does not want to know that you’re working towards being able to show that` prod` can handle extra traffic spikes at any moment. Everyone regardless of their role tends to understand the importance of reliability, and production load testing sounds cool. This will help you to start mapping out different components of the system that may be affected by initial load tests, and make a list of actions that may need to be branched for synthetic traffic, such as sending an actual email, and talking to teams responsible for those components and getting them involved.


You will also want to set up a situation room or an initiative room in Slack or your real time communication platform of choice. You will use this for updates on the project and when load tests are being run.


The next step is to gather data. You need to understand your load patterns, and your usage patterns. What are the most common user journeys through the system? What does traffic look like - are there daily or weekly peaks for example?


This will inform testing scenarios that you will be writing. To start with, it’s best to pick something simple and expand your coverage with time. It will also inform when you may want to run your first few tests. The general idea being that to stay on the safe side, you first few tests will run during off-peak hours.


It will also help you make sure that synthetic traffic is realistic and exercises the system in a similar manner. You hit the same endpoints, in the same general sequence, and synthetic traffic ramps up and down with the same shape as real traffic.


This is where you start putting together a new set of dashboards which a human operator can use when running the first few production load tests. These need to display top-level SLOs and KPIs, with the ability to tell at a glance when they start degrading.


As this information is being put together, start writing a run book for running these load tests. This is how you prepare the test, do a dry run, run a full run; this is where you watch key metrics, these are the thresholds for stopping the test early, this is how you prepare test data if needed, this is how you clean up etc - start with this. It will serve as basis for automation later.


Finally, you have your scenarios ready to go, you have a time window for your first test, you have your monitoring set up… pick a modest amount of extra traffic to start with. Announce the test run ahead of time, ideally the day before at least and then again on the day using a distribution list or a relevant Slack channel to give someone who might have a reason for you to hold off plenty of time to be able to tell you… and then, just run your test. At this point, it should not feel dangerous at all because if nothing else you won’t be adding much extra load.


If everything goes well, and you’ve confirmed that for example you can tell synthetic traffic apart, request headers are being propagated correctly, no-op operations are firing as expected - dial up that level of load and run again. And then again. Something might go wrong, but **your blast radius will be minimal at every step** . And eventually, you will arrive at a point where you’re running with 20% extra traffic, in production, and things just work.


At this point - **celebrate!** This is hugely important. Spread the good news. **A production load test is something to be proud of.**


## Try it


Our goal with this write up is to share what we learned implementing production load testing over the years and to provide some clarity on what it takes to run load tests against` prod` , and to show that it’s actually not that hard. It takes time, it takes team work, it takes a perspective shift, but it’s very doable.


We’d love to hear from you. Are you thinking of giving it a go, and getting to experience how fun it is? Let us know! Have you done it before, and have suggestions for something we didn’t cover here? Please let us know!
