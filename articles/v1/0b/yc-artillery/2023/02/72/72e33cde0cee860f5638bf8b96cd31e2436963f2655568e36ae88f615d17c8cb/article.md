---
schema_version: "1.0.0"
document_id: "72e33cde0cee860f5638bf8b96cd31e2436963f2655568e36ae88f615d17c8cb"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/load-testing-in-production"
published_at: "2023-02-06T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:1043bd2fc3811844772b246b164bb7202a299ad9823fb7326820887ecf37098f"
---

# What’s stopping you from load testing in production?

February 6th, 2023[How to](https://www.artillery.io/blog/tag/howto)


# What’s stopping you from load testing in production?


Hassy Veldstra


**At Artillery we believe that you should load test in production.**


We’ve helped many teams with their production load tests here at Artillery, and we speak from experience when we say:


- production load testing is not rocket science
- its dangers are obvious and can be mitigated in straightforward ways
- the benefits are worth a bit of extra work


Of course, there’s more nuance to it than “just load test in production!”:


- Load tests in production don’t have to be the only kind of load tests you run. There’s a lot of value in load testing pre-production systems and deployments.
- It does not mean that you must test the entire system in production. There’s a lot to be gained by testing sub-systems, sub-components, or individual APIs.
- It does not mean that you have to push production beyond its capacity. Load testing is not an all-or-nothing thing, and extra load can be added gradually.
- It does not mean you need to just accept the risk of affecting real users, or the risk of bringing the system down either. We’re engineers, managing risk is what we do, and there are plenty of tools at our disposal to help us do that.


This article aims to be a **real-world production load testing guide** , aimed at SQA and SRE teams that want to add an extra layer of reliability and production quality to their systems. It’s delivered in two parts:


1. In **Part I** we will look at the whys and some theory behind load testing in production
2. **Part II** will be a hands-on guide for going from zero to a production load test


Along the way there are two main ideas we want to get across:


1. Production load testing is not rocket science
2. Production load testing offers some of the best opportunities for learning and making your systems more resilient


## Contents


Below is a table of contents for what we’ll cover in Part I of this guide:


- **The Why** of load testing in production


- Production Load Testing = Confidence
- Production Load Testing = A Fitness Function


- Mitigating Risk (It Does Not Have To Be Dangerous)


- Build up in small chunks (to minimize blast radius)
- Take time
- Instrument for load testing


- Common Concerns


- Junk in the database and other side effects
- Irreversible and destructive operations
- Breaking stuff for real users
- Can’t do 100% coverage
- Cost
- Affecting third parties


- Why Artillery Is Great For Production Load Testing (hey, this *is* our blog!)


Part II will follow next week and will cover the roadmap of how we’d go from zero to a production load scale. Follow us on Twitter at[@artilleryio](https://twitter.com/artilleryio) or[LinkedIn](https://www.linkedin.com/company/artillery-io/) to hear when it’s published.


## Production Load Testing = Confidence


Let’s step back and remind ourselves why we test our code and systems in the first place. **Testing helps us increase confidence that things are working, and that they will continue working.** On the spectrum of` 0%` confidence to` 100%` confidence (an unnatainable ideal), our goal as engineers is to move as far to the right as possible.


The confidence that production load testing provides **cannot be provided by any other kinds of testing** .


A production system is composed of a myriad of components beside APIs and services that implement business logic. CDNs, databases, caches, load balancers, application servers, autoscaling configurations, circuit breakers, monitoring, logging - with a whole lot of emergent behaviors and properties that arise from their interactions. The only way to have confidence that a complex system can handle extra stress is to test **that very system** .


Every grizzled SRE has a story or two of a completely innocuous code or config change that got deployed, did not cause any issues for weeks or months, until there was a load spike… and then things turned bad very quickly. Things tend to break under stress, and exposing things to stress is a sure way to make them more resilient.


## Production Load Testing = A Fitness Function


Production load testing will help you identify all sorts of unknown unknowns in your system. It’s a great fitness function (an[evolutionary architecture](https://www.thoughtworks.com/insights/books/building-evolutionary-architectures) term), for proactively and very visibly enforcing certain performance related properties of your system.


It’s a great guard rail for helping you build safety margins around your SLOs — just because your system could handle its usual daily traffic peak yesterday, it won’t necessarily still be able to do it today. And again, if you’re expecting a high-traffic event, the only way to be able to say that “yes, we can handle it” with a high degree of confidence is to test it. You don’t want real users to do it for you, and you don’t want to “hope” that things will be OK.


The ability to load test in production is a very good signal of a high overall quality bar.


## Mitigating Risk


Adding extra load on a production *is* risky, but **it does not have to be dangerous** , if we take steps to mitigate those risks. Let’s look at what we can do.


### Build up in small chunks


The first production load test you run does not need to triple peak production traffic on the entire system on its first run. There are several ways to start small and build up over time:


1. Add a small amount of synthetic load to one API endpoint or user flow, which is not on the critical path. Run it manually with human oversight, and be ready to stop the test instantly if you need to.
2. Increase the load gradually across several test runs (which can be distributed over several days or weeks), until you’re adding a significant amount of synthetic load (say +50%).
3. Increase coverage by adding more endpoints and user flows to the load test.
4. Increase the frequency of these load tests, for example going from a run every two weeks, to running them weekly.
5. Start automating some of these tests by adding them to your CICD pipeline.


You’re very likely to see benefits very early on, even working through step 1 above. Teams often find important but overlooked gaps in monitoring & observability coverage, or lack of important information in service runbooks.


### Take time


It will take time to put everything together and to build up capacity to run these tests safely and address everyone’s concerns. Plan at least a couple of months for conversations to be had, teams to allocate time to gather data you may need, put extra monitoring in place, make changes to some of the services, and just get comfortable with the idea. It doesn’t mean that nothing will happen for a couple of months - no, you should be able to start adding some extra traffic to production within a couple of weeks, but you won’t run a Big Friendly Production Load Test for a while. It cannot be a last-minute effort. It takes time.


### Instrument for load testing


You will need to instrument your code to make it “load testable” in production. This isn’t as weird as it may sound: we add code to our apps for all sorts of non-core-business-logic things all the time, such as:


- monitoring and observability instrumentation
- logging
- security controls
- A/B testing
- feature flags


Adding code to aid load testing is just one more of those.


## Common Concerns


### What about junk in our database? or other side effects?


Synthetic traffic in production *is* going to cause a number of side effects:


- There may be new database records in your production database
- There will be extra log and metric output
- Your traffic dashboards will show that extra traffic
- User analytics may be affected, which may affect all sorts of reports assembled by other teams, such as marketing or product


**This is where extra instrumentation comes in.**


You will need a way to **distinguish real traffic from synthetic traffic** , as it propagates through the system. For example - if` service A` is the entry point into the system, and it calls` service B` , then` service B` will need to able to know when a request was triggered by a load test. One common way of doing this is via an extra HTTP header, which is then propagated across every subsequent request inside the system, similar to how distributed request tracing works.


You will also need a way to distinguish any persistent objects or database records created by load testing traffic. You might want to clear those out periodically, or you will want to be able to ignore them in reports created by your data or analytics team for example. One way to do it for example is to use a convention for usernames or email addresses associated with accounts in your system, whether that’s the domain used, part of the username or a combination of both. You can get creative, there’s no one-size-fits-all approach here.


### What about irreversible or destructive operations?


There may also be some operations that you don’t want to trigger from synthetic tests - attempting to charge a credit card or send out an email being two common examples. These will need to be a **no op** operation when the request comes from a virtual user created by the load test.


It will take time to discover and identify those places in a system, and then to implement that branching, and you will need to allow for plenty of time for it.


### Breaking stuff for real users


The other common concern is of course affecting real users by increasing tail latencies with extra load for example, or causing an outright outage, and bringing everything down.


The way you work around this is two-fold:


1. One, you always ramp up load slowly and over time so that negative effects of extra traffic can be spotted early, and the test run can be paused or stopped. You never go the whole hog from the start. It’s important to exercise caution, especially early on, and also **to be seen to exercise caution** .
2. The second part of this is to have your **production SLOs and KPIs monitored** and for those metrics to be available - for people running tests when they’re run manually, and programmatically later on for automation. As an operator of a production load test, you need to have a dashboard which tracks conformance to SLOs, defined in terms of real user experience, and it shows you how close you are to breaching those SLOs. You need to be able to have them right there.


### We can’t do 100% coverage


This concern is usually a variation of “are we going to load test our payment processing or physical stock allocation too?” Yes it’s true, there will be parts of the system you will not want to exercise, pretty much always. But - think back to that confidence spectrum. You won’t exercise 100% of all paths in a production system, or all of the components, but there is value in every shift to the right towards that 100%. Even if you never reach it. Every bit of extra confidence gained is valuable.


### What do we do about third parties?


That “100% coverage” point is often related to load testing third-party services and dependencies. This has to be decided on a case by case basis, but again: you can always add a load testing specific branch in your code and no-op some operations, or call a mock. It’s OK to do that if you have a 10 step process and you’re not really testing the last one - you’ve still tested the other nine.


### What about cost?


Adding extra production traffic will use additional resources, both to generate that traffic and to serve it. The call of whether it’s worth has to be made on a case-by-case basis but it ultimately boils down to whether generating and serving extra traffic is going to be more expensive than suffering downtime or degraded response times affecting UX. The arguments against this could probably be used against other kinds of testing, like spending time to write unit tests, or paying for non-prod environments, and you probably have both of those.


## Artillery does production load testing well


Not to go all sales on you, but this is our blog after all, so we’re going to briefly mention a few things we built into Artillery that make it a pretty good choice for production load testing. It’s all open source too,[give it a try](https://github.com/artilleryio/artillery/tree/main/packages/artillery) !


#### Production-scale workloads


Artillery does **distributed load testing out-of-the box** , it runs from your own infra (your own AWS account), and it’s completely serverless. This makes running production-scale workloads pretty straightforward:


- You can generate high load (whether it’s 100k RPS or 10M RPS) by **scaling out horizontally** , on[AWS Lambda](https://aws.amazon.com/lambda/) or[AWS Fargate](https://aws.amazon.com/fargate/) . There is no long-lived infra to set up or manage.
- You can generate load from multiple regions,[16 of them in fact](https://www.artillery.io/docs/resources/pro/limits#supported-regions) .


#### Real-time reporting and integrations


Monitoring & observability are **critical** for running load tests in production. Artillery comes with integrations with several popular systems (Datadog, CloudWatch, Honeycomb, Lightstep and more) out of the box.


#### Ease of customization


Every aspect of an Artillery load test can be customized - through[ready-made integrations](https://www.artillery.io/docs/resources/integrations) , or by writing custom logic in Javascript, with the option to use existing` npm` modules.


#### Support for modeling complex workflows and workloads


Artillery was designed to emulate complex transactional workflows with dependent steps, request chaining, parameterization & randomization, and weighting. It also offers a variety of ways to shape the traffic, with ramp up and ramp down phases for example.


#### It’s ridiculously cost-efficient


Production systems usually deal with high load, and you need to be able to generate enough synthetic load, and do it without breaking the bank. Artillery runs in your own AWS VPC, using AWS Lambda or Fargate, to provide the most cost-efficient way of running load tests at scale. It has built-in cost estimation too.


## Part II


In Part II of this guide we will look at a roadmap of how we’d go from zero to a production load scale:[Load Testing In Production, Part II](https://www.artillery.io/blog/load-testing-in-production-part-2) .
