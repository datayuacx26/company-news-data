---
schema_version: "1.0.0"
document_id: "15dd16d4ba01a5437a6cd453a283a5ce7654b9cfce2bf33bded53b9d947bb73f"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/cheering-on-coworkers-building-culture-with-datadog-dashboards/"
published_at: "2016-10-27T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:29.470006+00:00"
content_hash: "sha256:5f6a7b610cf222aa53b5e91d1476c64c57d2f9dce14560b74c33af799cbb836a"
---

# Cheering on coworkers: Building culture with Datadog dashboards

Nadir Kadem


Martin Fejoz


Benjamin Fernandes


One of our colleagues,[Christian](http://twitter.com/ufootorg) , is participating in a tremendous[6-day-run challenge](http://www.6jours-de-france.fr/) . Yes, you read that right, he will run around 850km (528 miles) over 6 days.


As we like to graph everything, we thought it would be fun to cheer him on remotely and follow his progress in this crazy race via a Datadog dashboard.


## Extracting the data


We discovered that Christian’s stats and the race’s progress are regularly updated on the event website.


Since the data is available in plain HTML on the website, scraping his current ranking, total distance run and other data was easy. It involved:


-


A simple crawler to scrape the html code of the webpage via the popular Python library[Requests](http://docs.python-requests.org/en/master/)


-


A basic HTML parser with the Python library[BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/)


## Feeding the metrics to Datadog


Now that we had the data, we began emitting metrics using[StatsD](https://docs.datadoghq.com/developers/dogstatsd/) and the Datadog agent.


```text
1   from   datadog   import   statsd   as   dog    2
3   ci = course_info()    4
5   for   runner   in   ci:    6              dog.gauge(  "runner.distance"  , ci[runner][  'distance'  ],   tags  =[  "name:  %s  "   %runner])    7              dog.gauge(  "runner.ranking"  , ci[runner][  'ranking'  ],   tags  =[  "name:  %s  "   %runner])    8              dog.gauge(  "runner.elapsed_time"  , ci[runner][  'time'  ],   tags  =[  "name:  %s  "   %runner])
```


With all metrics now available in Datadog, we built a dashboard to support our champion!


## The result


We crunched the metrics in order to get a nice dashboard including a live video, a few gifs for fun and some meaningful metrics, and there we are![You can check the dashboard](https://p.datadoghq.com/sb/O-FBIq-4e3365c3e6) .


We are displaying this around our NY and Paris offices, so we can cheer Christian on throughout the day.


At publishing time, Christian is at the head of the pack, with a lead of over 47km (29 miles) and another 44h to run. Good luck Christian!


-
-
-
