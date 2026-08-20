---
schema_version: "1.0.0"
document_id: "a269fb248fd039fd5be08d35e952d65ef45c1ae3ec43f20dcd9daebc53d86c5c"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/introducing-kafka-kit-tools-for-scaling-kafka/"
published_at: "2018-08-13T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:a03fae9ff160c358e6bc16434df2ac89d70100f1edf1976c16bcef4112ec7060"
---

# Introducing Kafka-Kit: Tools for scaling Kafka

Jamie Alquiza


For a company with data in the name, it’s no surprise that we ingest large amounts of it. Kafka is our messaging persistence layer of choice underlying many of our high-traffic services. Consequently, our Kafka usage is quite high: the intake of trillions of data points per day yields double-digit gigabytes per second bandwidth and the need for petabytes of NVMe storage, even for relatively short retention windows.


Scaling large systems can be as challenging as it is fun. A common pattern in most datastores under constant growth is the need to relocate large amounts of data, whether for routine recoveries or scaling. As systems grow, data movement occurs in increasingly large sizes and frequency. Kafka is designed in a way that focuses on doing a few things incredibly well and provides tooling that does not impose strong opinions on how it should be used or operated, leaving this flexibility to the user.


Over time, we’ve developed scaling patterns along with tools customized to how we do operations. Today, we’re opening them to the world and giving a brief overview of how we use them at Datadog.


## Introducing Kafka-Kit


Kafka-Kit is a collection of tools that handle partition to broker mappings, failed broker replacements, storage based partition rebalancing, and replication auto-throttling. The two primary tools are` topicmappr` and` autothrottle` .


These tools cover two categories of our Kafka operations: data placement and replication auto-throttling.


## Kafka Data Placement and Capacity Planning


` topicmappr` is a drop-in replacement for the Kafka` kafka-reassign-partitions.sh` script` --generate` function with a few additional features:


-


**Deterministic Output** . Repeat runs with a given input will always generate the same partition map.


-


**Minimal movement broker replacements** . If a broker is being replaced, a complete map can be regenerated with replacement brokers only filling the holes left by the previously failed broker(s). Partitions with complete ISRs (in-sync replicas) by default are untouched.


-


**Configurable, rack-aware partition placement** .` topicmappr` uses Kafka’s built-in locality tags and ZooKeeper metadata to ensure safe partition placement by partition count or size, the latter allowing storage bin-packing and storage rebalancing.


-


**Replication factor updates** . Topic replication factor can be easily increased or decreased on the fly.


-


**Clear action summaries** .` topicmappr` clearly explains what will happen before a partition map is executed by a user.


Example using` topicmappr` to rebuild a topic with a failed broker, replacing ID` 1002` with` 1003` and` 1004` :


```text
1   $ topicmappr --rebuild-topics "test_topic" --brokers=1001,1003,1004 --force-rebuild    2
3   Topics:    4        test_topic    5
6   Broker change summary:    7        Broker 1002 marked for removal    8        Replacing 1, added 2, missing 0, total count changed by 1    9
10   Action:    11        Rebuild topic with 1 broker(s) marked for replacement    12
13   WARN:    14        [none]    15
16   Partition map changes:    17        test_topic p0: [1001 1002] -> [1001 1003] replaced broker    18        test_topic p1: [1002 1001] -> [1003 1004] replaced broker    19        test_topic p2: [1001 1002] -> [1004 1001] replaced broker    20        test_topic p3: [1002 1001] -> [1001 1004] replaced broker    21
22   Broker distribution:    23        degree [min/max/avg]: 1/1/1.00 -> 2/2/2.00    24        -    25        Broker 1001 - leader: 2, follower: 1, total: 3    26        Broker 1003 - leader: 1, follower: 1, total: 2    27        Broker 1004 - leader: 1, follower: 2, total: 3    28
29   New partition maps:    30        test_topic.json
```


` topicmappr` is written in Go, so it’s easy to run binaries from anywhere that has access to the ZooKeeper cluster managing Kafka. It minimally requires two inputs: a list of topic names that you want to generate partition maps for and a list of broker IDs.` topicmappr` then builds a` kafka-reassign-partitions.sh` compatible input file, additionally using Kafka metadata stored in ZooKeeper to ensure that:


-


Replicas are dispersed over discrete localities (via the Kafka` broker.rack` parameter)


-


That all specified brokers are live in ZooKeeper


-


That a sufficient number of brokers has been provided to build a map


Along with further criteria beyond this list,` topicmappr` works to ensure optimal and safe partition placement.


### Deciding Where Topic Data Is Placed


` topicmappr` exposes a selection of partition replica placement strategies, each with interesting properties designed for different workloads. This includes the` count` strategy and the tunable` storage` strategy.


#### ‘Count’ Placement Strategy


When` topicmappr` is used to build an ideal map, the` count` strategy ensures that leadership is maximized among brokers and that the number of partitions held per broker is as even as possible. This is the default placement strategy and is useful when even data flow is expected across all partitions in a topic. Mappings are simple and can be quickly produced without metrics data.


A topic with six partitions could be mapped over two brokers as follows:


Another subtle but important feature of the` count` strategy is that it will attempt to maximize the number of broker-to-broker relationships. In other words, we want each broker to share replicas for every partition held with as many other distinct brokers as possible. Our initial algorithm ensured global leadership balance and even partition ownership per broker, but subsets of brokers tended to cluster together in terms of partition ownership. While partitions for a given topic were maximally distributed, our links among brokers were not. For instance, given 1,000 partitions to place over 50 brokers residing in 3 racks, any given broker would show ownership for roughly 200 partitions. If those particular 200 partitions were examined, we found that all ISRs were composed of the same 3 brokers. This was a result of a deterministic but naive selection design: each time an ideal broker needed to be chosen, brokers were sorted by number of partitions held, then by ID. The lowest ID, least-used broker not in any previously used rack was always selected. When generating a fresh map from scratch, usage counts would increment evenly across all brokers. Given enough partitions, this would result in a repeating grain pattern with a small offset that unintentionally yielded clustering.


The problem that this poses is that the number of broker-to-broker relationships defines an upper limit of replication sources; replacing a broker with more relationships is likely to replicate from more unique sources than one with fewer relationships. Not only is having more replication sources faster, but the strain of recovery is distributed as widely as possible over healthy nodes, minimizing impact. In order to solve this, at each selection step we now sort brokers by usage then shuffle each tier of brokers with equal usage scores. This is a pseudo-random shuffle with a seed value that’s incremented predictably. The result is that we have a “randomized” least-utilized selection that maximizes broker-to-broker replica sharing while retaining deterministic input/output.


To further inform users on how data is distributed among brokers,` topicmappr` reports before and after broker-to-broker relationships in the form of degrees ( *vertex degree* in graph theory) in addition to the leader, follower and total replica slots filled. Snippet from the previous example output:


```text
1   Broker distribution:    2        degree [min/max/avg]: 1/1/1.00 -> 2/2/2.00    3        -    4        Broker 1001 - leader: 2, follower: 1, total: 3    5        Broker 1003 - leader: 1, follower: 1, total: 2    6        Broker 1004 - leader: 1, follower: 2, total: 3
```


#### ‘Storage’ Placement Strategy


The` storage` placement strategy works similarly to a first-fit descending bin packing algorithm and is designed to prioritize even storage utilization across brokers while still satisfying locality (` rack.id` ) constraints. In order to drive decisions in this strategy,` topicmappr` uses partition size and broker storage metrics data that’s loaded into ZooKeeper beforehand. To get started, the Kafka-Kit repo includes a tool called` metricsfetcher` that sources data from the Datadog API, but other metrics sources could be integrated as well.


In contrast to the` count` strategy, a balance of partition size is favored over partition quantity:


This strategy is useful for topics with high levels of size imbalance across partitions. We’re most often using this to perform in-place rebalances of topics that have grown unevenly, usually due to routing requirements particular to services using those topics. Most of the time, these topics were initially mapped using the` count` strategy and later remapped using` storage` strategy as usage changed.


When the` storage` placement strategy is used, an additional output that estimates changes in free storage available is printed in the` topicmappr` results. This includes the free storage available range spread and standard deviation to help quantify what level of improvement is to be expected:


```text
1   Storage free change estimations:    2        range spread: 14.85% -> 0.41%    3        std. deviation: 54.08GB -> 1.48GB    4        -    5        Broker 1002: 1055.13 -> 993.30 (-61.83GB, -5.86%)    6        Broker 1003: 925.77 -> 995.47 (+69.70GB, 7.53%)    7        Broker 1004: 942.71 -> 993.46 (+50.75GB, 5.38%)    8        Broker 1005: 1020.07 -> 997.41 (-22.66GB, -2.22%)    9        Broker 1006: 961.55 -> 993.53 (+31.98GB, 3.33%)    10        Broker 1007: 1063.25 -> 995.30 (-67.95GB, -6.39%)    11
12   New partition maps:    13        test_topic.json
```


Recalling the previous mention of degree distribution, the` storage` placement strategy has an additional` optimize` parameter that allows users to bias the` storage` placement algorithm towards more even storage utilization or more broker-to-broker relationships. The default optimization choice is` distribution` and is suitable for most cases. In scenarios where one or very few partitions are significantly larger than others, the` storage` optimization choice may yield more desirable results. A user can easily construct two mapping models and determine which is most suitable using` topicmappr` node degree, storage range spread and standard deviation indicators.


*Storage utilization is optimized across brokers after applying a* *` topicmappr`* *assignment:*


While this feature is new and relatively simple at the moment, it delivered immediate payback the week it entered production. We had a single topic that consumed over 100 expensive machines. Because of the number of partitions, the need for replica isolation by locality, and uneven partition growth over time, it was not feasible for a human to manually wrangle optimal maps. Instead, the broker sets for this topic were scaled according to the usage of the most utilized machines (which when viewed on a graph, showed a cluttered gradient of utilization with a large range spread). Having software that quickly computes safe, optimal maps allowed a roughly 50% reduction in instance count from the out of the box Kafka partition placement.


### From Data Mapping to Capacity Planning


Now that our partition mapping tools have been explained, our methods of capacity planning can be put into context. The primary inputs of` topicmappr` is a topic (or list of topics) and a broker list– we do this precisely because a given set of topics and brokers are coupled as a single logical unit of capacity in our operations.


A common pattern of running Kafka, especially on hardware, is a large pool of homogenous machines composing a single cluster. Topics are dispersed over the cluster where a single broker may hold partitions for many topics. Loads are then realized and observed on a per-broker basis depending on what the broker is holding. One difficulty that this brings about is that redistributing workloads from hot brokers requires granular visibility into current *and* potential loads for every broker. *Which partitions on broker x are replicas? How much bandwidth will this broker take on when the leaders for those partitions fail? How does the load on this broker break down by topic?* This list goes on, and once all known variables are accounted for, you’re presented with some difficult combinatorial optimization problems. Enough so that LinkedIn’s *excellent* Kafka[Cruise Control](https://www.slideshare.net/JiangjieQin/introduction-to-kafka-cruise-control-68180931) project uses a z3 theorem prover to drive complex decisions in this scenario.


Since Datadog runs on public cloud providers, we have shaped our capacity planning around the assumption that machines of arbitrary counts and sizes could be consumed on demand. This combined with our particular usage patterns allows us to reason about topics as a first-class citizen of capacity, where broker resource allotments follow the topic demands. Topics are individually determined to have a cost value derived from various forms of workload dimensions, such as data points per second, anticipated network demand from consumers, and storage required for the full retention period. A ceiling function of broker counts required by the largest determinant among these dimensions is allotted to the topic. As topics grow, their broker counts grow, and we constantly shape broker set capacities to topic workloads using` topicmappr` . A topic (or list of topics) and set of brokers experience a lifecycle as a single capacity unit.


An interesting side-effect of reasoning about resource allotments this way is that instance types can be better determined. At one point, we built an internal calculator that showed changes in instance counts for hypothetical workloads. Spin one dimension too far out of proportion, such as retention vs throughput, and you’ll find that another instance class would be most economical. Visibility in this scenario also becomes simplified: our brokers are tagged (using Datadog host tags) according to what topics they hold, allowing a narrowly scoped juxtaposition of topic throughput vs broker utilization. This materializes our concept of capacity units all the way back to our monitoring dashboards and alerting pipeline, both of which only need to be concerned with the generalized view of what capacity each unit is at. The utilization of brokers *1,2,3* has no relationship with utilization of brokers *4,5,6* if they belong to different topic sets (as they are then recognized as distinct capacity units). If a unit is projected to exhaust its capacity, we add brokers and perform a rebalance (in` topicmappr` this is usually an in-place scaling operation with an expanded broker list).


A single cluster can be composed of a dozen or more capacity units (topic and broker set combinations) that are scaled and recovered *individually* . For smaller topics, we aggregate several together over a shared “pool” of brokers. Some of our clusters are configured with multiple pools of brokers in fixed quantities and topics are then packed into them based on topic load and pool capacity. We store these as configurations which eventually become` topicmappr` inputs in automatic configuration management runs. In configuration pseudo-code, this may look like:


```text
1   // topicmappr supports regex topic matching.    2   // Map all topics prefixed with "test_topic" to    3   // pool1 brokers.    4   pool1: "test_topic.*" => [1001,1002,1003,1004,1005]    5
6   // Isolate these two specific    7   // perf testing topics to pool2.    8   pool2: "load_testing|latency_testing" => [1006,1007,1008,1009,1010]
```


New topics can be assigned to the most fitting pools or existing topics can be moved across pools as needed. Topics often outgrow shared pools and are moved to dedicated pools. For each pool, partitions for the referenced topics are maximally dispersed according to the selected` topicmappr` placement strategy.


In the above config, if broker` 1010` failed, we’d replace the broker ID in the list with a replacement ID. Our automated` topicmappr` runs are configured to assume any broker list changes are repairs, which will result in all the holes` 1010` left behind to be filled with the new broker ID (this is done using the` count` placement strategy combined with a feature called *substitution affinity* - see the Kafka-Kit wiki for details). This results in a minimally invasive repair while retaining any previous storage balance performed by system operators. This also works with many machines at once; at one point, we rebuilt partition maps for every topic in a large cluster that suffered a total AZ loss with a` topicmappr` one-liner.


The long term goal of our capacity planning is to continue refining the coordination between scaling resource pools and mapping capacity to the right place at the right time. Our philosophy so far has been to hang on to simplified fundamentals for as long as possible, get them humming as one large system, then step back and focus on global optimality.


## Making Data Movement and Recovery Hands-Off


Each time that we build a map with` topicmappr` , whether to scale pool capacity or replace failed brokers, Kafka undergoes heavy data replication. In Kafka 0.10.1, the replication throttle was added which allows users to limit the network throughput of a replication event in order to reduce impact on clients. This throttle is applied manually when executing a partition reassignment.


As clusters continue to grow, you inevitably approach the point of constant failures and data migrations. The need to observe workloads and determine throttles became a point of operational risk and toil: run it too quickly and you might starve out consumers, run it too slowly and you increase exposure to failures beyond the allowable fault tolerance. You must constantly observe changes in workloads and adjust the throttle accordingly.


` autothrottle` was created to do this job for us. It runs on an interval and looks out for topics undergoing replication (particularly events in the` /admin/reassign_partitions` znode). It uses the Datadog metrics API and dynamically updates throttles based on the observed loads, known throttle rates, and configurable parameters such as upper limits on throughput. It’s optimized for network bound clusters and designed to adapt replication throughput to changing workloads.


*` autothrottle`* *dynamically tuning the outbound network utilization during a rebalance:*


` autothrottle` distinguishes source vs destination brokers in a replication and accounts for capacity by instance type. A throttle is determined by using a configurable portion of free network capacity that’s not already used for replication.` autothrottle` observes, calculates, and applies the throttle in a continuous cycle. And of course, it writes Datadog events that detail which topics are being replicated, involved brokers, and changes in throttle rates. Once a replication is done,` autothrottle` unsets all throttles and writes a final event.


*Watching* *` autothrottle`* *handling a recovery through a Datadog dashboard:*


` autothrottle` has additional parameters, such as failure modes in the face of losing metrics visibility or being unable to make safe determinations. Like` topicmappr` ,` autothrottle` should make it clear for the user what it’s doing:


```text
1   2018/03/16 18:27:11 Autothrottle Running    2   2018/03/16 18:27:12 Admin API: localhost:8080    3   2018/03/16 18:27:13 No topics undergoing reassignment    4   <...>    5   2018/03/16 18:31:21 Topics with ongoing reassignments: [test_topic]    6   2018/03/16 18:31:21 Source brokers participating in replication: [1002 1003 1004 1005 1006 1007]    7   2018/03/16 18:31:21 Destination brokers participating in replication: [1002 1003 1004 1005 1006 1007]    8   2018/03/16 18:31:21 Most utilized source broker: [1005] net tx of 116.43MB/s (over 60s) with an existing throttle rate of 0.00MB/s    9   2018/03/16 18:31:21 Replication capacity (based on a 90% max free capacity utilization): 120.21MB/s    10   2018/03/16 18:31:21 Updated throttle to 120.21MB/s on broker 1005    11   2018/03/16 18:31:22 Updated throttle to 120.21MB/s on broker 1004    12   2018/03/16 18:31:22 Updated throttle to 120.21MB/s on broker 1003    13   2018/03/16 18:31:23 Updated throttle to 120.21MB/s on broker 1006    14   2018/03/16 18:31:23 Updated throttle to 120.21MB/s on broker 1002    15   2018/03/16 18:31:24 Updated throttle to 120.21MB/s on broker 1007    16   2018/03/16 18:32:24 Topics done reassigning: [test_topic]    17   2018/03/16 18:32:24 No topics undergoing reassignment    18   2018/03/16 18:32:29 Throttle removed on broker 1003    19   2018/03/16 18:32:30 Throttle removed on broker 1004    20   2018/03/16 18:32:30 Throttle removed on broker 1002    21   2018/03/16 18:32:31 Throttle removed on broker 1007    22   2018/03/16 18:32:31 Throttle removed on broker 1005    23   2018/03/16 18:32:32 Throttle removed on broker 1006    24   2018/03/16 18:33:32 No topics undergoing reassignment
```


## Looking Forward


The entire Kafka-Kit toolset is just part of a continued evolution of Kafka scaling at Datadog. Emphasis on operational methods that are easy to reason about is essential to paving way for what’s next. Having simple capacity planning means that we can eventually build software to perform autonomous load mapping and capacity growth. Having machines safely pace recoveries means we can more easily build completely autonomous failure recovery.


We hope that insight into our operations is a helpful reference point to develop and improve your own. Kafka-Kit is available from the[Github project page](https://github.com/DataDog/kafka-kit) and is suitable for all users of Kafka, and if you’re a current Datadog user,` autothrottle` is ready today to carry you beyond just observability.


-
-
-
