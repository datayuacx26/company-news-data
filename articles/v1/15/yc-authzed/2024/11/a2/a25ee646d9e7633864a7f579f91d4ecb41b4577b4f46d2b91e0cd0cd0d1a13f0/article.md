---
schema_version: "1.0.0"
document_id: "a25ee646d9e7633864a7f579f91d4ecb41b4577b4f46d2b91e0cd0cd0d1a13f0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/the-day-benjamin-franklin-broke-our-ci"
published_at: "2024-11-07T20:20:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:10783d7b281bad1b22d13e0b9acee73ebce33a32c50cc8ce48465453c3ceadb5"
---

# The Day Benjamin Franklin Broke our CI

Growing up in Philadelphia and attending the University of Pennsylvania, I was immersed in stories about Benjamin Franklin and his influence on modern society. Some, like his famous experiment proving lightning was electricity, are mostly accurate—though perhaps a bit embellished. Others, like his supposed recommendation for Daylight Saving Time, are more myth than history.


Daylight Saving Time and its twice-yearly changes remain controversial to this day. The origin story begins with Franklin, who "recommended" it in a satirical letter to a French newspaper's editor. In this tongue-in-cheek piece, he extolled the virtues of waking earlier to save on candles and lamp oil.


Ironically, despite Franklin's satirical intent, DST was eventually adopted worldwide—not merely for saving candle wax, but for conserving energy during World War I. While history still debates whether DST achieves its energy-saving goals, I can say with certainty that this past Monday it didn't save me any energy. Here's the tale of how Franklin's satirical "recommendation" ended up breaking our CI tests in an unexpected and amusing way!


## A timely mystery


As part of my job as CTO of AuthZed, I often work on improvements to[SpiceDB](https://spicedb.io/) , our open source implementation of the[Google Zanzibar](https://zanzibar.tech/) paper. On Monday morning, after gaining a delightful extra hour of sleep due to the “fall back” of Daylight Savings, I created[a PR](https://github.com/authzed/spicedb/pull/2115) to make some very small changes to how SpiceDB handles its[recently added transaction metadata feature](https://github.com/authzed/spicedb/releases/tag/v1.38.0) .


Unfortunately, despite my changes not impacting the computation of revisions at all, a particular test (` CheckRevisionsTest` ) in the MySQL datastore test suite immediately[started to fail](https://github.com/authzed/spicedb/actions/runs/11634776970/job/32501001600?pr=2114) .


At first, I thought an unrelated change in the datastore might be the cause, but then our team started to notice this failure on other changes, including when we reran code merged on` main` last week; this was especially odd because our merge process runs the *full* test suite before merging… and the test was now *consistently* failing on all runs!


Sensing a mystery afoot, I began to debug the underlying issue that was occurring.


## Background


SpiceDB is a permissions database based on the ReBAC or Relationships-based access control paradigm. A permission is granted if and only if there exists a path of relationships connecting the resource+permission (e.g.` document#view` ) and the subject (e.g.` user:tom` ) in a graph formed by the relationship data and any expressions defined in the schema. The relationships used to compute permissions are stored in an underlying database such as Postgres, CRDB or MySQL, which SpiceDB calls a` datastore` .


One of the most important aspects of a ReBAC permissions system is that of *consistency* : in order to guarantee reasonable performance while *also* ensuring that permissions do not grow too stale, all permissions queries made to SpiceDB (like` CheckPermission` ) specify a` consistency` block that allows the caller to specify whether they want better performance (with some staleness) or less staleness (at the cost of not being able to use the cache). This is practically implemented in SpiceDB by having every API call select a` revision` or point-in-time at which the *entire* set of required relationships is loaded when computing a result.


Each` revision` selected has to be validated against the underlying datastore to ensure that the datastore can return the relationships found at that specific point-in-time: afterall, if a` revision` of, say, 3 weeks ago was requested, the datastore would be unable to return the state of the relationships **at that time** , and thus has to return an error.


To do so, each datastore implements the[CheckRevision](https://github.com/authzed/spicedb/blob/1e03edd52d1229514203fd2b9b00dfa251ee9fdd/pkg/datastore/datastore.go#L572) method, which takes in a specific revision (point-in-time) and checks that the revision is still **valid** , i.e. it has not moved beyond the configured window of validity.


The test that was failing due to` CheckRevision` returning the revision as invalid… despite having been created by the same test no more than a second ago. It was time to put on my bifocals and begin an investigation!


## Starting the investigation


The very first step I performed was running the test locally to see if I could reproduce the failure.


Running the test, however, produced no failure whatsoever:


1


2


3


4


```text
$ cd internal/datastore/mysql
$ go test ./... -tags ci,docker -count=1 -run TestMySQL8Datastore/TestCheckRevisions -v


ok


```


1


2


3


4


```text
$ cd internal/datastore/mysql


ok


```


This was, to say the least, quite strange.


Thinking the problem might be related to running tests in parallel, I reran the *full* test suite for MySQL:


1


2


3


```text
$ go test ./... -run TestMySQL8Datastore/ -tags ci,docker -count=1 -run TestMySQL8Datastore/ -v


FAIL: TestMySQL8Datastore/TestCheckRevisions


```


1


2


3


```text


FAIL: TestMySQL8Datastore/TestCheckRevisions


```


Which immediately failed. So something amongst the various tests in the suite was causing a failure. However, removing` t.Parallel` did not fix the problem.


This led me to believe that something was broken in the test or the MySQL datastore implementation itself… Yet no one in the SpiceDB project had made **any** changes related to the datastores in SpiceDB since Friday… and the test failure was **consistent** .


What was the key to this mystery?


## Debugging CheckRevision in MySQL


For the MySQL datastore, revisions are stored in a` relation_tuple_transaction` table, with the time used stored in a` timestamp` column:


1


2


3


4


5


```text
CREATE TABLE relation_tuple_transaction (
id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
timestamp DATETIME(6) DEFAULT NOW(6) NOT NULL,
PRIMARY KEY (id),
INDEX ix_relation_tuple_transaction_by_timestamp (timestamp)) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


```


1


2


3


4


5


```text
CREATE TABLE relation_tuple_transaction (
id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
timestamp DATETIME(6) DEFAULT NOW(6) NOT NULL,
PRIMARY KEY (id),


```


When a revision is checked,[the following query](https://github.com/authzed/spicedb/blob/1e03edd52d1229514203fd2b9b00dfa251ee9fdd/internal/datastore/mysql/revisions.go#L54) is executed, returning whether the revision’s timestamp is within the required window:


1


2


3


4


5


6


7


8


9


10


11


12


```text
SELECT $revision_id >= COALESCE((
SELECT MIN(id)
FROM   relation_tuple_transaction
WHERE  timestamp >= TIMESTAMPADD(SECOND, -$window_in_seconds, UTC_TIMESTAMP(6))
),(
SELECT MAX(id)
FROM relation_tuple_transaction
LIMIT 1
)) as fresh, $revision_id > (
SELECT MAX(id)
FROM   timestamp
) as unknown;`


```


1


2


3


4


5


6


7


8


9


10


11


12


```text
SELECT $revision_id >= COALESCE((
SELECT MIN(id)
FROM   relation_tuple_transaction
WHERE  timestamp >= TIMESTAMPADD(SECOND, -$window_in_seconds, UTC_TIMESTAMP(6))
),(
SELECT MAX(id)
FROM relation_tuple_transaction
LIMIT 1
)) as fresh, $revision_id > (
SELECT MAX(id)
FROM   timestamp
) as unknown;`


```


As we can see above, the SQL query returns two boolean values, with the first boolean being the important one here: the first boolean is` true` if and only if the` revision_id` is greater or equal to the least recently “alive” revision *or* is equal to the latest revision.


In practical terms, this query ensures that the revision is still alive by ensuring that it is within the “window” revisions considered alive by **` $window_in_seconds`** .


Despite the revision having been created by the **same** test no more than 1 second ago, this query was failing to return` true` .


Looking at[the test itself](https://github.com/authzed/spicedb/blob/1e03edd52d1229514203fd2b9b00dfa251ee9fdd/pkg/datastore/test/revisions.go#L179) , we can see that the` window_in_seconds` was set to` 300*time.Minute` , or 5 hours, which should be more than enough time for the test to run:


1


```text
ds, err := tester.New(0, 1000*time.Second, 300*time.Minute, 1)


```


1


```text
ds, err := tester.New(0, 1000*time.Second, 300*time.Minute, 1)


```


Wanting to know **why** the call was failing, I added code to the test to select the timestamp and adjustment for all revisions:


1


```text
select id, timestamp, TIMESTAMPADD(SECOND, -18000.000000, UTC_TIMESTAMP(6)) from relation_tuple_transaction


```


1


```text


```


However, running this query returned **truly bizarre results** :


id timestamp computed


1` 1969-12-31 19:00:01 +0000 UTC`` 2024-11-04 17:21:46.926416 +0000 UTC`


2` 2024-11-04 17:21:46.914328 +0000 UTC`` 2024-11-04 17:21:46.926416 +0000 UTC`


3` 2024-11-04 17:21:46.922334 +0000 UTC`` 2024-11-04 17:21:46.926416 +0000 UTC`


Somehow, the computed result of` TIMESTAMPADD` was **later** than the` timestamp` for the most recent revision, which was written only a few milliseconds ago.


Further, changing the delta from` -18000` seconds to` -122` seconds (chosen at[random](https://xkcd.com/221/) ) showed the time **moving forward**


My reaction, to say the least, was one of befuddlement:


(actual Slack messages I sent when I first saw this output)


Was time just… broken in MySQL all of sudden? I had not changed the version of MySQL used, so what could have caused this change?


Trying to isolate the time issue on MySQL, I next ran` now` as part of the test, both as part of the unified test suite and the single test.


Needless to say, this gave me my first confirmation that time was indeed the issue:


(yes, I wrote “this is going to be a fun blog post” at the time)


Now the question was to find **how** the time zone was changing in the test suite. A cursory search immediately landed me[on a test which does, in fact, change the timezone](https://github.com/authzed/spicedb/blob/1e03edd52d1229514203fd2b9b00dfa251ee9fdd/internal/datastore/mysql/datastore_test.go#L628) , ironically to ensure **timezones don’t impact revision calculations!**


1


2


```text
_, err := db.ExecContext(ctx, "SET GLOBAL time_zone = 'America/New_York';")
req.NoError(err)


```


1


2


```text
_, err := db.ExecContext(ctx, "SET GLOBAL time_zone = 'America/New_York';")
req.NoError(err)


```


With the source now figured out, the solution to the test suite failure was straightforward: simply move this one test to run against an isolated instance of MySQL, rather than the same instance being used by the remainder of the test suite, ensuring the global timezone change did not impact any other tests running.


However, this still left a bit of a mystery remaining: why did the test only start failing after the daylight savings time changed this past weekend?


## Concluding the mystery


Recall the code used for testing a revision being valid:


1


```text
ds, err := tester.New(0, 1000*time.Second, 300*time.Minute, 1)


```


1


```text
ds, err := tester.New(0, 1000*time.Second, 300*time.Minute, 1)


```


The window chosen for the test is 300 minutes or… **5 hours** , the exact same delta that the` America/New_York` timezone *now* had as per Eastern Standard Time.


Before the fallback from Eastern Daylight Time, the timezone delta from UTC had been **4 hours** , thus giving the test an hour-long “window” accidentally. When daylight savings ended and we moved back to Standard Time, the delta became 5 hours, **exactly matching** the test’s configured window **by accident** , ensuring that the revision would be considered out of date… by **milliseconds** .


The test had previously succeeded by accident, only functioning because Franklin had published a letter many decades ago.


## Takeaways


Daylight savings time continues to be an everlasting source of fun and bugs for us software engineers. However, we can also glean another important lesson that Franklin taught: taking a scientific and standardized approach to learning new information.


By using a consistent approach to debugging the test, we were able to take an otherwise confusing and somewhat bizarre failure and turn it into a manageable series of steps:


- Try to get a consistent repro of the bug, ideally on your local machine
- Narrow the conditions for achieving the repro as much as possible
- Change one condition at a time and observe changes in behavior
- Once a suspicious condition has been identified, try to find the root cause


When I was studying software engineering at Penn, never did I dream that my university’s founder would one day “directly” cause an impacting issue in software that I wrote. Fortunately, the impact was minimal, and we now have another great story of our own on the adventure of building SpiceDB!


Want to learn more about SpiceDB or just discuss cool Franklin facts?
Check out[our progress](https://spicedb.io/) and join us[in our Discord](https://authzed.com/discord)


On this page


- A timely mystery
- Background
- Starting the investigation
- Debugging CheckRevision in MySQL
- Concluding the mystery
- Takeaways


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
