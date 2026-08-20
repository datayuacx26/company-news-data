---
schema_version: "1.0.0"
document_id: "5b2f3a8f39dacd508fbfbffc412e15e5c741461b7621b8623ef98d9e098f0de4"
company_key: "yc-questdb"
company: "QuestDB"
source_id: "yc-questdb-news-import-d0368e5a3210"
canonical_url: "https://questdb.com/blog/2022/04/12/demo-live-crypto-data-streamed-with-questdb-and-grafana/"
published_at: "2022-04-12T00:00:00+00:00"
first_seen_at: "2026-07-23T21:47:49.914954+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:68bea5ea103b1fb413c3c167b1505919d84cc6b8e823142b4eecd410487087a9"
---

# Demo of live crypto data streamed with QuestDB and Grafana

QuestDB is a next-generation database for[market data](https://questdb.com/capital-markets/) . It offers premium ingestion throughput, enhanced SQL analytics that can power through analysis, and cost-saving hardware efficiency. It's[open source](https://github.com/questdb/questdb) , applies open formats, and is ideal for[tick data](https://questdb.com/glossary/tick-data/) .


---


At QuestDB we are all about performance. To showcase querying capabilities of the database we have been running a live demo of historical taxi rides in NYC with 1.6 billion rows[1](https://news.ycombinator.com/item?id=23616878) and a geospatial dataset that contains the locations of 250k unique ships[2](https://www.reddit.com/r/programming/comments/q1vnfi/demo_geospatial_and_timeseries_queries_on_250k/) moving over time. You can analyze this dataset with SQL on our[live instance](https://demo.questdb.io/) and see how fast each query is processed. Today, we introduce a new dataset on the same demo instance: crypto market data ingested in real-time from the Coinbase Exchange. For ingestion, we use a convenient Python library[Cryptofeed](https://github.com/bmoscon/cryptofeed) , a cryptocurrency exchange feed handler that supports QuestDB. And for visualization, we use[Grafana](https://questdb.com/docs/integrations/visualization/grafana/) to create interactive live charts, which refresh every 5 seconds.


We ingest the following columns into QuestDB in real-time for each BTC-USDT and ETH-USDT trades coming through the Coinbase Exchange:


- price
- side (buy/sell)
- amount
- timestamp


To get you started, we added a set of example queries in the live demo of QuestDB[Web Console](https://questdb.com/docs/getting-started/web-console/overview/) . These pre-written queries leverage the standard SQL syntax and time-series SQL extensions in QuestDB. When clicking on a query, it's automatically added to the SQL editor. Then, click the


Run button or press F9 to execute the query. Despite the large amount of data stored on the demo instance, the queries should come back in milliseconds!


Let's go through these sample queries one by one.


## Last prices of BTC and ETH


To find out the latest prices of BTC and ETH in USD. We use the[LATEST ON](https://questdb.com/docs/query/sql/latest-on/) syntax, which is native to QuestDB's SQL Engine:


Latest BTC and ETH prices


[Demo this query](https://demo.questdb.io/?query=SELECT%20*%20FROM%20trades%0AWHERE%20symbol%20in%20(%27BTC-USDT%27%2C%20%27ETH-USDT%27)%0ALATEST%20ON%20timestamp%20PARTITION%20BY%20symbol%3B%0A&executeQuery=true)


```text
SELECT * FROM trades   WHERE symbol in ('BTC-USDT', 'ETH-USDT')   LATEST ON timestamp PARTITION BY symbol;
```


Below is a real-time chart for Bitcoin and Ethereum prices with a time sample of 10 seconds.


## Candle chart sampled by time


This query returns open, close, minimal and maximal prices as well as cumulated volumes with 15-minute intervals. We use the[SAMPLE BY](https://questdb.com/docs/query/sql/sample-by/) syntax, which aggregates time series data into homogeneous time chunks:


Candle chart with 15-minute intervals


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20%20%20timestamp%2C%0A%20%20%20%20first(price)%20AS%20open%2C%0A%20%20%20%20last(price)%20AS%20close%2C%0A%20%20%20%20min(price)%20AS%20low%2C%0A%20%20%20%20max(price)%20AS%20high%2C%0A%20%20%20%20sum(amount)%20AS%20volume%0AFROM%20trades%0AWHERE%20symbol%20%3D%20%27BTC-USDT%27%20AND%20timestamp%20%3E%20dateadd(%27d%27%2C%20-1%2C%20now())%0ASAMPLE%20BY%2015m%20ALIGN%20TO%20CALENDAR%3B%0A&executeQuery=true)


```text
SELECT       timestamp,       first(price) AS open,       last(price) AS close,       min(price) AS low,       max(price) AS high,       sum(amount) AS volume   FROM trades   WHERE symbol = 'BTC-USDT' AND timestamp > dateadd('d', -1, now())   SAMPLE BY 15m ALIGN TO CALENDAR;
```


This real-time chart on Grafana plots the candle chart with a time sample of 10 seconds. We also show the volume traded on a secondary axis.


## VWAP Bitcoin price sampled by time


For each 15 minutes interval, we calculate the average price of BTC-USDT adjusted for the volume of trades during that period. This query includes the[WHERE](https://questdb.com/docs/query/sql/where/) clause that is accelerated by our new[JIT Compiler](https://questdb.com/docs/concepts/deep-dive/jit-compiler/) (see the *lightning* in the logs). And once again, we[downsample](https://questdb.com/glossary/downsampling/) the dataset using` SAMPLE BY` .


Volume-weighted average price


[Demo this query](https://demo.questdb.io/?query=SELECT%0A%20%20%20%20timestamp%2C%0A%20%20%20%20sum(price%20*%20amount)%20%2F%20sum(amount)%20AS%20vwap_price%2C%0A%20%20%20%20sum(amount)%20AS%20volume%0AFROM%20trades%0AWHERE%20symbol%20%3D%20%27BTC-USDT%27%20AND%20timestamp%20%3E%20dateadd(%27d%27%2C%20-1%2C%20now())%0ASAMPLE%20BY%2015m%20ALIGN%20TO%20CALENDAR%3B%0A&executeQuery=true)


```text
SELECT       timestamp,       sum(price * amount) / sum(amount) AS vwap_price,       sum(amount) AS volume   FROM trades   WHERE symbol = 'BTC-USDT' AND timestamp > dateadd('d', -1, now())   SAMPLE BY 15m ALIGN TO CALENDAR;
```


The following real-time chart displays the distribution of trades based on their size and paints a more granular picture of volume traded.


## Implied BTC-ETH exchange rate


The two series of prices for BTC-USDT and ETH-USDT have different unique timestamps. In order to join these two series where timestamps do not exactly match, use the[ASOF JOIN](https://questdb.com/docs/query/sql/asof-join/) syntax and then divide the price of BTC-USDT with the price of ETH-USDT to get the implied BTC/ETH rate.


Implied BTC-ETH exchange rate


[Demo this query](https://demo.questdb.io/?query=WITH%20btc%20AS%20(%0A%20%20SELECT%20timestamp%20AS%20x%2C%20price%20AS%20btcusd%0A%20%20FROM%20trades%0A%20%20WHERE%20symbol%20%3D%20%27BTC-USDT%27%20AND%20timestamp%20%3E%20dateadd(%27m%27%2C%20-30%2C%20now())%0A)%2C%0Aeth%20AS%20(%0A%20%20SELECT%20timestamp%20AS%20y%2C%20price%20AS%20ethusd%0A%20%20FROM%20trades%0A%20%20WHERE%20symbol%20%3D%20%27ETH-USDT%27%20AND%20timestamp%20%3E%20dateadd(%27m%27%2C%20-30%2C%20now())%0A)%0ASELECT%0A%20%20x%20AS%20time%2C%0A%20%20btcusd%20%2F%20ethusd%20AS%20crossPrice%2C%0A%20%20btcusd%2C%0A%20%20ethusd%0AFROM%20btc%0AASOF%20JOIN%20eth%3B%0A&executeQuery=true)


```text
WITH btc AS (     SELECT timestamp AS x, price AS btcusd     FROM trades     WHERE symbol = 'BTC-USDT' AND timestamp > dateadd('m', -30, now())   ),   eth AS (     SELECT timestamp AS y, price AS ethusd     FROM trades     WHERE symbol = 'ETH-USDT' AND timestamp > dateadd('m', -30, now())   )   SELECT     x AS time,     btcusd / ethusd AS crossPrice,     btcusd,     ethusd   FROM btc   ASOF JOIN eth;
```


The following real-time chart plots three series: BTC-USDT, ETH-USDT and the implied BTC-ETH cross price.


## Conclusion


We hope you find these example queries and charts useful to get started. Let us know about other useful queries you build on our demo server! To follow along, with your local instance of QuestDB and Grafana, follow[our tutorial](https://questdb.com/blog/time-series-monitoring-dashboard-grafana-questdb/) or visit the[Grafana docs](https://questdb.com/docs/integrations/visualization/grafana/) for more information. If you're interested in setting up something similar within your organizations or for personal projects, you can get started on[GitHub](https://github.com/questdb/questdb#try-questdb) or[join our community forums](https://community.questdb.com/) .
