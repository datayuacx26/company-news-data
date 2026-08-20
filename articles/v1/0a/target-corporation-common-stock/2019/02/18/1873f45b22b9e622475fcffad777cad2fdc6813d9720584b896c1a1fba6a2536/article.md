---
schema_version: "1.0.0"
document_id: "1873f45b22b9e622475fcffad777cad2fdc6813d9720584b896c1a1fba6a2536"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/goalert-your-future-open-source-on-call-notification-product"
published_at: "2019-02-25T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:48246eca15919c67c1e70c7cbe9a6d7fb162563091b2ab8b0362b8b9aa69e193"
---

# GoAlert - Your Future Open Source, On-Call Notification Product

A few years ago,


[Target started a journey](https://dojo.target.com/journey) to move into a product-based organization with dedicated, durable, full-stack teams. One core belief we rallied behind was that product teams were accountable for building, running and supporting their products. Gone were the days of siloed development and operations teams. When a team introduces any change into production, it is accountable for supporting that change for as long as it lives.


Evolving as the Customer Base Changed


Within Measurement (the domain commonly referred to in the industry as Monitoring & Telemetry, Observability or Visibility), our customer base (users of a Measurement product) changed dramatically, very quickly.


Previously, our customers were a few hundred support engineers incentivized by restoring alerts. These customers cared about tickets being generated in their work queue when things were broken or about to break. All support work required a ticket, so we optimized monitoring products for efficient and accurate ticket delivery. These support engineers’ responsibilities were focused on break/fix work. As a result, our customers wanted full-service offerings where the monitoring instrumentation work was done for them. In addition, these support engineers were located in three countries spread across the globe so that people could watch for incoming tickets all day, every day.


Our on-call system during this time period was pretty simple. If support team members needed to get in touch with an engineering team, they would look at the on-call calendar, find the person’s phone number and give that person a call. If the engineer could not be reached within an acceptable timeframe, the manager would be called. Overall, this process worked well at the time...


Then, So Much Changed, So Quickly...


With the move to the product model, we suddenly had thousands of engineers as Measurement customers who cared about many different things. These customers wanted data ... measurements ... delivered in near real time to give them a contextualized understanding of whether their product was meeting our guests’ needs (a guest is a person shopping in one of our stores or online). When experiments were run, product teams needed immediate feedback on the results. In addition, when key business metrics indicated that an engineer needed to promptly take action to prevent a negative guest experience, we needed to quickly engage the right engineer on the right product team.


How We Responded


This customer shift meant that we needed to pivot all of our Measurement products. Focusing on our on-call notification capability in this blog post, what we had previously was not going to work for us any longer. We obviously would not ask thousands of engineers to watch ticket queues all day, every day.


Instead, we needed an on-call notification product that would:


- Automatically


inform the right person, the right way, at the right time


- Be simple and intuitive for customers to use


- Be easy to enhance/maintain/support


- Be even more reliable than our customer’s products (After all, what good is an on-call product if it isn’t available in a customer’s time of need?)


- Minimize shared dependencies between the on-call product and our customer’s products


- Be extremely scalable and available to all product teams at Target. We need to be able to process thousands of alerts per second and support thousands of users located in multiple countries.


- Adhere to open standards and recommended security practices to ensure our customers’ data is secure


- Be built from the beginning to be an open-source product


GoAlert Is Born


After looking at different options, we decided to build GoAlert. We quickly formed a small, dedicated product team composed of people with diverse backgrounds in software engineering, infrastructure and support. This full-stack team would be accountable to build, maintain and support this new product.


With our customers' needs top of mind, we made the following technical decisions:


Programming Language: Go


Why:


- Very stable language (e.g., compatibility promise) that has great tooling for avoiding common bugs


- Simple deployment model (no dependencies once built)


- Easy to learn


- Already used in Measurement’s tech stack (for example:


[Filebeat](https://github.com/elastic/beats) ,


[Telegraf](https://github.com/influxdata/telegraf) ,


[Influx](https://github.com/influxdata/influxdb) )


- Performant and can handle concurrent workloads with strong guarantees and manageable code


- A focus on clarity as "clear is better than clever;" we need to ensure that when things go wrong, we can get to the bottom of it quickly


Compute Platform: Kubernetes


Why:


- Single deployment model that can be used across multiple cloud providers


- Low cost of deployments and scaling


- Zero downtime deployments are obtainable


Data Store: PostgreSQL


Why:


- Available as a managed offering from multiple cloud providers


- Large range of features (e.g., advisory locks, notification channels) to help simplify the deployment model


Deployment Location: Multiple public cloud providers and in regions not used by Target’s other products


Why:


- Reduce shared dependencies with the systems and products being monitored


- Geographic redundancy is important for self-monitoring (if one region, or provider is down, another will sound the alarm)


With the initial launch of GoAlert, our customers could receive alert notifications via SMS and/or Voice that were triggered via alerts defined in


[Grafana](https://github.com/grafana/grafana) . Very quickly, our initial users realized value and were able to prevent issues before they impacted the in-store guest experience. Then, feature requests started rolling in and the GoAlert team has been hard at work adding new features and improvements to make the lives of on-call engineers easier.


Here’s a look at the alerts page viewed on a mobile device:


One belief we have really rallied around is the benefit of automated testing. As an on-call notification product, we need to ensure that notification delivery is extremely reliable. Measurement customers trust GoAlert to notify them of any issue that could impact our guests. To maintain this trust, we have a robust suite of automated tests built into our continuous integration process. Here are a few examples:


- Full suite of black-box integration/behavioral "smoke tests" to ensure every version maintains core functionality, even through database migrations


- Full up/down migration tests to ensure every database change has a straightforward rollback mechanism


- Full suite of user interface integration tests run in the browser using


[Cypress](https://github.com/cypress-io/cypress)
- Deployment tests that validate the transfer of control when rolling out new versions under load to ensure a database or pipeline change will not cause issues during the deployment


With the critical nature of GoAlert, proper visibility into the product was important from the beginning. We leverage other Measurement offerings for operational visibility (we use the same products we offer to our customers) and also experiment with alternative products we do not yet make available to the enterprise. This experimentation allows us to learn and potentially offer new products or features to our customers in the future. Currently, we are experimenting with


[OpenCensus](https://opencensus.io/) to trace, measure, monitor and debug transactions.


What’s Next


GoAlert has two primary customer personas: engineers around the world who occasionally go on call and members of the


[Target Operations Center](https://www.youtube.com/watch?v=zvfFWvxHkkQ) who need to rapidly engage engineers on call to restore service for critical interruptions. To improve the lives of both personas, we plan to work on the following features:


- Notifications to customers to inform them that they will be going on call soon


- The ability to favorite items in GoAlert to make it easier for customers to find what they are looking for


- A mobile application


Open Source


Target believes deeply in


[open source](https://tech.target.com/opensource) . We love both using


AND


contributing to open source. GoAlert is a project we are passionate about and seriously considering open sourcing.


If you have any questions, ideas to make GoAlert more valuable to the open-source community, thoughts on how you would use GoAlert, or even to simply say that you would like to see GoAlert open-sourced... we’d love to hear from you!


Please contact us at


WeAreListening@GoAlert.me


The People Behind GoAlert


- [Nathaniel Caza](https://github.com/mastercactapus)
- [Mitch Cimenski](https://github.com/m17ch)
- [Arundhati Rao](https://github.com/arurao)
- [Nate Cook](https://github.com/auchindoun)
- Donna Roisen


- [Adam Westman](https://github.com/AdamLWestman)


## RELATED POSTS


### GoAlert - Now Available in Open Source


By Adam Westman, June 11, 2019


Open Source GoAlert Is Now Available At https://github.com/target/goalert
