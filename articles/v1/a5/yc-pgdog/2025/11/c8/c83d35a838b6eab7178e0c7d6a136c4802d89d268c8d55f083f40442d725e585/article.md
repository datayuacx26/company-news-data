---
schema_version: "1.0.0"
document_id: "c83d35a838b6eab7178e0c7d6a136c4802d89d268c8d55f083f40442d725e585"
company_key: "yc-pgdog"
company: "PgDog"
source_id: "yc-pgdog-rss-662afb283f74"
canonical_url: "https://pgdog.dev/blog/shard-your-database"
published_at: "2025-11-13T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:49.478310+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:b53777ceccd0e27c017d4e4f17122e998866c65dd2ec4adef3ae3da4aa255dc9"
---

# You should shard your database

Managing large databases sucks. If you had this thought before, this post might sound familiar. If not, I’m going to give you a sneak peek into the good, the bad and the ugly of running Postgres at-scale. Somewhere along the way, I’ll try to convince you of my main thesis:


> The best time to shard your Postgres database was last year. The next best time might be this quarter.


Let’s start at the beginning.


### We are down, again


The day-to-day of a software engineer is normally pretty boring. We are not firefighters or fighter pilots and breaking the land speed record isn’t something we are *that* interested in doing after our lunch break.


When the headphones are on, we go into our safe space composed of logic puzzles, decoding computer instructions and, these days, wrangling sloppy AI bots into writing maintainable Svelte components.


This is all to say, when our mid-afternoon ritual is interrupted by a call from someone named “PagerDuty” who, in a robotic voice, informs us that our “production database has exceeded 90% CPU utilization”, we are not amused.


Slack pings from our least favorite channel start coming in and someone says the magic words that tell us that our day is officially ruined: “Can we all get on a Zoom call?”


After about twenty minutes of scanning GitHub for any suspicious commits and finding nothing, someone decides to pull up database stats from` pg_stat_activity` , only to find out that most connections are stuck executing the same, relatively simple,` SELECT` statement.


With a couple of joins and a few innocuous-looking filters, nothing really stands out until someone runs an` EXPLAIN` and discovers that our carefully optimized query decided, out of the blue and for no good reason, to skip all indexes and sequentially scan the entire 300 GB table.


This discovery is made about 30 minutes into the incident, and prod is still down. With senior leadership sitting on the same call, our bodies are now fully saturated with cortisol and other fun hormones that are supposed to “help” us deal with stressful situations.


The engineer with the most experience and muscle memory pulls up an admin` psql` console, runs` ANALYZE` on the table, switches their screenshare to the CPU graph in Datadog and starts hitting the refresh button. After about a minute, the upside-down hockey stick shows up, CPU usage is back down to 30% and the incident is over.


Due to the abrupt nature of the problem and the somewhat mysterious resolution, the first question on everyone’s minds isn’t all that surprising: what the hell happened and how can we prevent it from happening again?


### Planning is hard


The Postgres query planner is the brain of the database. Its job is to take a query, like a` SELECT` , and map it to as few CPU instructions as possible.


To make this work, the planner collects statistics about the data stored in all database tables. Each table is sampled to create a representative estimate of what kind of data is stored inside each column.


The size of each sample is controlled by the` default_statistics_target` parameter. It can be configured globally for the whole database, or on a per-table basis. The larger its value, the more values will be stored in the column-specific histograms every time the table is sampled by the autovacuum process.


Whenever Postgres is about to execute a query, it reads the relevant statistics and decides which indexes, if any, to search for the values in the` WHERE` clause. However, if the stats are old because the data in the table has significantly changed, the planner could make the wrong choice.


In our case, this choice was to skip using indexes entirely. It decided that they were very unlikely to contain any of the values it’s looking for and it would be faster to just read the whole table.


This is good to know (and knowledge is power), but the question remains: how do we prevent this from happening again?


### Tuning is also hard


Since we root-caused the problem to be a bad query plan caused by out-of-date statistics, Occam’s razor tells us that the solution should be simple: calculate better statistics.


The stats are collected in the background by the autovacuum process. The process is triggered periodically, and collects enough samples to fill the histogram buckets for each column. How often the process runs and how much data is sampled is controlled by these four parameters:


Parameter Description


` default_statistics_target` How many entries to store in the column-specific histograms.


` autovacuum_analyze_threshold` Trigger new sample collection if this many rows changed since the last sample.


` autovacuum_analyze_scale_factor` Trigger new sample collection if the percentage of rows relative to the *estimated* number of rows in the table changed since the last autovacuum run.


` autovacuum_naptime` How often to wake up the autovacuum process to check these conditions.


Changing either one could help, so we started experimenting. The first parameter seemed like the right call, so we bumped up the statistics target to 500 (that’s 5x from the default 100). At first, nothing happened, but as soon as we hit` ANALYZE` , we noticed something different about our query plans:


```text
Planning Time: 1.075 ms
Execution Time: 0.025 ms


```


The time it takes to come up with a query plan has increased quite a bit. Not a huge amount, since we are still talking about milliseconds here, but enough that it noticeably impacted the overall latency of the system. This made sense: the larger the histogram, the more time it takes to read it.


However, if we add *0.8ms* to every single query (it took only *0.2ms* before the config change), and we execute 100,000 of them per second, that’s *80s* of additional CPU time we now need to serve the same requests.


This is something one would certainly notice on the API latency dashboard and on the database CPU utilization graph. And the worst part is, since we can’t really prove a negative, we were not sure this actually solved our bad statistics problem.


After reverting the` default_statistics_target` change, we moved on to the other autovacuum parameters. If we tweaked how often it refreshed the stats it should, in theory, react to data changes faster.


Tweaking either parameter produced some noticeable activity in the autovacuum process. As our app inserted and updated rows, autovacuum woke up more readily and re-analyzed the table. This seemed like the solution, except now we were seeing a noticeable increase in disk activity and IOPS usage.


On an already overextended database, additional maintenance work wasn’t a welcome sight. Like before, we couldn’t prove a negative and be sure that this solved the issue. The whole affair took over a week, and our budget for “infra stuff” ran out. We kicked the can down the road and crossed our fingers that it wouldn’t happen again.


### Smaller is easier


You’re probably wondering: what does the planner or the autovacuum have to do with sharding? The answer lies again with Occam’s razor: if our problem is a large and overutilized database, let’s make it smaller and use it less.


This means splitting it into several smaller databases, also known as *sharding* . If we sharded our 300 GB table, dividing it into 12 pieces, for example, would have the following effects:


- Table writes are reduced by a factor of 12
- Autovacuum has to maintain 12 times less data
- Queries have to search 12 times fewer rows


And our biggest table is now only 25 GB. We can even` pg_dump` it to a staging box if we wanted to, something we previously thought was a pipe dream.


And most importantly, we know we can grow our company 12x before we have to deal with this problem again. That’s quite a bit of runway.


Features and procedures we thought were history are now possible again. Take the following migration as an example:


```text
ALTER    TABLE    users    ALTER    COLUMN    id    TYPE    BIGINT  ;


```


On a 300 GB table, it would have taken over 5 minutes and brought down the application with a very loud bang. On a 25 GB table? It would take about 20 seconds, way below our typical 30 seconds` Rack::Timeout` setting.


Don’t get me wrong, I’m not advocating we should abandon all good sense and engineering principles and let Claude take the wheel. But, the cost of mistakes becomes considerably lower.


We added the wrong column? We can just drop it. There’s 12 times less traffic on the table, so we’ll be able to acquire an exclusive lock without any problems. We backfilled it with wrong data? We can just do it again. After all, it only took a few minutes the first time.


Our Postgres shards are using only five percent of their provisioned capacity and we have plenty of room to build more.


### Big data is a choice


The year is 2025 and the times when we *had* to do things a certain way are, increasingly, in the past. The state of the art in database technology is constantly evolving. Things are getting better. We don’t *have* to operate large databases anymore.


Looking back at my time at Instacart in 2020, I thought managing 16 TB in a single Postgres instance was cool. Knowing how to tune it while performing middle of the night ninja-like migrations on fragile schemas was something I was proud of.


Today, I know better. Nights are for sleeping and binge-watching For All Mankind, knowing my databases are overprovisioned and scaled horizontally with PgDog.
