---
schema_version: "1.0.0"
document_id: "97d613318b59aae57d7e336f2993e3ef34cf692a2f0c20b9e308493cf0b2624b"
company_key: "yc-movinglake"
company: "MovingLake"
source_id: "yc-movinglake-news-import-d738cd23891a"
canonical_url: "https://www.movinglake.com/blog/brand-launch"
published_at: null
first_seen_at: "2026-07-22T04:57:21.843550+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:e802155589e23737f0ce54140ea6b667ac015e260a72b3ac6862a16a9dd0097f"
---

# MovingLake Launches Backed by YCombinator!

We are happy to announce we are officially launching MovingLake backed by[YCombinator.](http://ycombinator.com/) We are launching as well our first version of our[web application.](https://app.movinglake.com/) Anyone can now use our proprietary, realtime, event-driven, replayable connectors to sync their data from[supported APIs](https://www.movinglake.com/realtime-connectors) into your preferred[destinations](https://www.movinglake.com/destinations) .


## What's so important about realtime?


We created MovingLake because of what we had seen in our previous jobs. We detected at least three important use cases which we couldn't solve with existing tools and which we thought were of super importance:


### ML for ML


MovingLake for Machine Learning is one of the critical use cases. During my time at Google we spent inhuman amounts of time training, testing and calibrating search ranking models. Once we had a satisfactory version of a model, moving it to production was quite difficult. Thankfully the serving infrastructure was already there, however the big problem was matching the offline data with the online data. Because part of our training data came from batch pipelines, ingesting the data in realtime for online inferences became a huge hurdle.


MovingLake solves this issue exactly because it's Realtime By Default. You can ingest data and store the full historical version of it in your data base or data warehouse of your preference. You can then create a model based on this data and once you put it in production you can be sure that the same data you trained on will be available for inferences as well. Not only do we guarantee you'll have data for realtime inferences, but we also guarantee it's the same exact source as the one you used for training. No more ensuring different data sources for the same features match perfectly.


### State Skipping


The next big hurdle we have seen with batch pipelines is that object states can be skipped. Imagine you run a car rental business and operate through a marketplace. If you run a daily batch pipeline which downloads all the day's reservations then you will probably have missing data. If a client comes in and books a car, but then decides to cancel an hour later, that data is lost forever because your batch pipeline takes too long to resync data. In this case your database would show only a cancelled reservation.


Using MovingLake's connectors you can be sure you will never lose any intermediary states for your data. Because we sync as often as possible, in the case of the car rental business, the data warehouse would be filled up with both the created reservation and the cancelled reservation. This can lead to meaningful business insights on why people cancel and how long they take to cancel a reservation.


### Automation, alarms and realtime dashboards


One last topic which comes up again and again is the need to create realtime workflows, alarms and dashboards from data that is stored in your data warehouse. Sure you can do reverse ETLs, use tools like data visualization tool notifications, or some other workarounds, but you will never have the flexibility of running a dedicated service to run a custom workflow.


Using MovingLake ensures that even if you started out with just syncing the data to your data warehouse, you will always have the flexibility of piping the data to realtime systems and create automations, alarms, realtime dashboards and even run full-fledged systems integrations with it.


## Disconnect between Data and Transactional systems


MovingLake aims to provide the bridge between Data and Transactional (a.k.a Backend) teams. You should not have to purchase a data connector and then have to also purchase an iPaaS solution or build your own integration to an API you are already extracting data from!


The event-driven nature of MovingLake ensures that one connector can serve the whole company's needs. By creating any number of destinations microservices, data warehouses, data bases and data lakes can all consume data from a single connector source.


‍


MovingLake's Data Engine


‍


Thanks for reading through. Please subscribe to our newsletter below. Our[documentation is here](https://www.movinglake.com/docs/) , the[web application](https://app.movinglake.com/) is here and let us know any comments[using this form!](https://www.movinglake.com/contact-us)
