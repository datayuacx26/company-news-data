---
schema_version: "1.0.0"
document_id: "9240258f0812edfebcd52306291c538bde8600fb9dce5b02f1f9c54bae674b45"
company_key: "yc-jitsu"
company: "Jitsu"
source_id: "yc-jitsu-news-import-65f0e2b767a6"
canonical_url: "https://jitsu.com/blog/jitsu-next"
published_at: "2023-08-11T00:00:00+00:00"
first_seen_at: "2026-07-22T00:57:29.784311+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:d6a416d00d66daccab919ba6886d6913c2f0269111d8047bd3874a3214f24279"
---

# Introducing Jitsu Next

We're incredibly excited to announce **Jitsu Next** , the next generation of Jitsu Platform. Jitsu Next is a new product that is built on top of same core technology as Jitsu 1.x, which is now[Jitsu Classic](https://classic.jitsu.com/docs/)


We started the project almost a year ago. It's been in private beta for the last 3 months and we're now ready to open it to the public. Let's dive in into what we have built!


## # Unlimited incoming events on all plans


Starting from now, all incoming events are completely free on all[plans of Jitsu Cloud](https://jitsu.com/pricing) . No matter how many events you send to Jitsu, you will not be charged for them. We will only charge for events going out to your data warehouse or other destinations.


Send as many events as you need and control your costs with[functions](https://jitsu.com/features/functions) by filtering out events that you don't need.


## # New core


Jitsu Next is based on a new core called[🚚 Bulker](https://github.com/jitsucom/bulker) . Bulker is a standalone open source project that powers Jitsu Next. It's a high performance, low latency, horizontally scalable event processing engine. It's written in Go and uses Apache Kafka as a transport layer.


⭐️ ⭐️ Like Bulker? Give us a star[on GitHub!](https://github.com/jitsucom/bulker) ⭐️ ⭐️


Bulker can be used separately from Jitsu Next as a standalone event processing engine. See configuration and deployment docs on[on github](https://https//github.com/jitsucom/bulker/blob/main/.docs/server-config.md)


## # Clickhouse, included


[ClickHouse](https://clickhouse.com/) is an analytics database for a big data processing. It is available both as a open-source project and[cloud service](https://clickhouse.com/) . At Jitsu we are big fans of ClickHouse, and we added ClickHouse support at the very beginning.


**We decided to include ClickHouse as a default data warehouse into all Jitsu Cloud plans.**[Create an account](https://use.jitsu.com/) and claim your free ClickHouse instance and query your data from UI directly


## # A brand new UI


Jitsu got a brand new UI. It's much more intuitive and easy to use


- **New Live Events debugger** . It's now much easier to debug incoming events. You can see all incoming events in real-time along with destination logs and functions execution results.
- **New data model** . It's now much easier to debug incoming events. You can see all incoming events in real-time along with destination logs and functions execution results.
- **Embedded SQL editor** . It's now much easier to debug incoming events. You can see all incoming events in real-time along with destination logs and functions execution results.


## # New functions engine


We completely redesigned functions engine. It's still based on V8, but now it's much more powerful and flexible. We also added a lot of new features:


- **[Persistent storage](https://jitsu.com/docs/features/functions/#persistent-storage)** . Now functions can persist data between the calls on each event. It opens an endless opportunities for customization: custom sessionization, caching and etc
- **[Command-line SDK](https://jitsu.com/docs/features/functions/sdk)** . Turn your functions into full-stack Node.js project. Use typescript, unit-test, linter and other tools available for Node.js ecosystem


## # Segment compatible API


Jitsu Next[JavaScript API](https://jitsu.com/docs/sending-data/npm) is fully compatible with Segment. It also can be used as a drop-in replacement.


Our JavaScript client is based on open-source[analytics.js](https://getanalytics.io/) library. Unfortunately, Jitsu Next doesn't support[previous versions of API](https://jitsu.com/docs/sending-data/js-sdk) . You'll need to migrate your code to a new library. Fortunately, it's very easy to do. The average migration project won't take more than few hours.


## # Migration FAQ


### # I'm using Jitsu already. Is Jitsu Next compatible with previous version?


Unfortunately, no. While being based on a same core, **Jitsu Next** not compatible **Jitsu Classic** . Please see your migration options below.


### # How long Jitsu Classic will be supported?


We're committed to support and develop Jitsu Classic for at least until December 2024. It applies both to open source and hosted versions of Jitsu Classic.


### # How can I access Jitsu Classic?


- **Cloud version** : if you already has an account, Jitsu Classic at[cloud.jitsu.com](https://cloud.jitsu.com/) . We are closing new accounts registration for Jitsu Classic. Please use[use.jitsu.com](https://use.jitsu.com/) to create a new account. However, if you need a new account in Jitsu Classic, please contact us atsupport@jitsu.com
- **Self-hosted** : Jitsu Classic is available on github at[master](https://github.com/jitsucom/jitsu/tree/master/) branch
- **Documentation** . Documentation is available at[classic.jitsu.com](https://classic.jitsu.com/)


### # What's the best way to migrate to Jitsu Next?


Configuration of Jitsu Next is very similar to Jitsu Classic. Our average user has 4 destinations and 9 sources. So the configuration migration shouldn't take more than one hour.


- **Cloud version** : if you're using a paid version of Jitsu Cloud, please contact us atsupport@jitsu.com so we can work together on migration plan. We will provide you with a free migration assistance, and a free subscription to Jitsu Next for the period of migration.
- **Open-source** : please contact ussupport@jitsu.com if you're interested in a paid migration assistance.


### # Can Jitsu Next pull data from sources?


Pulling data from sources in a closed beta now. . We're planning to port[all sources](https://classic.jitsu.com/docs/sources) from Jitsu Classic to Jitsu Next by the end of July 2022.


Get on[the wait-list](https://jitsu.com/integrations/connectors) to be notified when it's available.
