---
schema_version: "1.0.0"
document_id: "5e74c05ffcbfb4440fd6b9ab6378c512a24d968a38577dc819969992791760fb"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/prevent-newenemy-cockroachdb"
published_at: "2021-10-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:e45945b17e6f3ad5508b4b160360bd67603bee5fb58cdefc824f342ac0c62fe0"
---

# The One Crucial Difference Between Spanner and CockroachDB

[SpiceDB](https://github.com/authzed/spicedb) is the open-source Zanzibar implementation that drives Authzed, and it's important that it provide all the guarantees that are described by the Zanzibar paper - one such property (oft overlooked by other descriptions or implementations of Zanzibar) is protection against the *New Enemy problem* .


"New Enemy" is the name[Google gave](https://research.google/pubs/pub48190/) to two undesirable properties we wish to prevent in a distributed authorization system:


1. Neglecting ACL update order
2. Misapplying old ACLs to new content


In other words, the "New Enemy" occurs whenever you can issue an ACL check that returns results different from what you reasonably expected based on the order of your previous actions. Don't worry if this feels a little too nebulous - we'll look at some concrete examples later (or you can take a detour now and read our in-depth introduction to the[New Enemy problem](https://authzed.com/blog/new-enemies/) ).


## Zanzibar Requires Spanner


The system described by the[Zanzibar paper](https://research.google/pubs/pub48190/) relies on[TrueTime](https://research.google/pubs/pub45855/) and Spanner's snapshot reads to prevent the New Enemy problem.


TrueTime is Google's internal time API described in the[Spanner paper](https://research.google/pubs/pub39966/) . Instead of returning a single value for "now" when requesting the current time, it returns an interval` \[earliest, latest\]` such that "now" must lie within the interval. TrueTime can provide this guarantee (as well as return a usefully small interval) thanks to use of GPS and atomic clocks.


TrueTime is used in Spanner to provide[external consistency](https://research.google/pubs/pub45855/) :


> if T2 starts to commit after T1 finishes committing, then the timestamp for T2 is greater than the timestamp for T1


This is true for any two transactions, hitting any node, anywhere in the world.


If this is the first description you've seen of Spanner and TrueTime, this property might seem magical and potentially CAP-theorem-breaking.


But if we have an API like TrueTime available, we can write down exactly how to create external consistency:


1. T1 finishes committing at some time
2. T2 starts to commit
3. T2 can ask TrueTime for "now" and get the range` \[earliest, latest\]`
4. The node handling T2 ensures that the transaction timestamp it assigns is provably in the past by waiting the entire uncertainty window before committing.


When all timestamps are selected this way, they become totally ordered according to "real" time. But this method will force writes to wait, so it's important that the interval be as small as possible - this is where atomic clocks and GPS are crucial (Google reports typical intervals of less than 10ms).


Spanner also supports snapshot reads at specific timestamps, which allows[Zookies](https://github.com/authzed/spicedb/blob/e264e9c0cdef35a9084674d0311a7956617d02a3/docs/zedtokens-and-zookies.md) to be issued to ensure ACL checks are evaluated at a specific point in time. Together, external consistency and snapshot reads prevent the new enemy problem:


- **External consistency** : ACL changes are applied and persisted in the order you observed writing them
- **Snapshot reads** : ACL checks are evaluated at a consistent point in time to avoid mixing old and new rules, and at a minimum time after which you know the changes you care about have been applied.


## SpiceDB Without Spanner


SpiceDB today supports[several backends](https://docs.authzed.com/spicedb/selecting-a-datastore) (and will likely support more in the future): In-Memory, Postgres, and CockroachDB.


Note that none of these backends are Spanner, and none have a TrueTime-like API available. Nor is such a thing readily available: various cloud providers tout "TrueTime-like" time precision for their NTP sources, but none provide the uncertainty interval and guarantees described by TrueTime as required for this application. So what do we do?


For the **[In-Memory Driver](https://github.com/authzed/spicedb/tree/f82f5c4a74c0d072a01033691a508a35ae9ead07/internal/datastore/memdb)** , all communication happens through the same process, so can easily ensure total ordering. The memory backend is for testing and demo purposes only.


With the **[Postgres Driver](https://github.com/authzed/spicedb/tree/f82f5c4a74c0d072a01033691a508a35ae9ead07/internal/datastore/postgres)** , external consistency is not a concern - we get it for free from postgres. But postgres doesn't directly support snapshot reads - we layer them in via an MVCC-like transactions table, which is worthy of its own separate post.


Postgres, while excellent, does not scale horizontally and can't provide the global distribution and availability we require from a permissions service.


For this, we turn to the **[CockroachDB Driver](https://github.com/authzed/spicedb/tree/f82f5c4a74c0d072a01033691a508a35ae9ead07/internal/datastore/crdb)** . CockroachDB often draws comparisons to Spanner, but it is different in some important ways.


## CockroachDB vs. Spanner


CockroachDB doesn't have a TrueTime-like time source, but still provides many of the same properties that Spanner does (including snapshot reads).


Cockroach has written extensively about their[consistency model](https://www.cockroachlabs.com/blog/consistency-model/) and how they get by[without atomic clocks](https://www.cockroachlabs.com/blog/living-without-atomic-clocks/) . There's a lot of great information in these Cockroach resources (and they're worth a read), but to quickly summarize the parts that impact the New Enemy problem:


- Cockroach does not provide external consistency for transactions involving *non-overlapping keys* .
- Cockroach *does* provide external consistency for transactions with *overlapping keys* , sometimes termed having a "causal relationship".
- Cockroach timestamps can only1 be off of cluster consensus time by up to the` max_offset` for the cluster - a clock skew beyond which a node is removed from the cluster for being too far off.


This means that SpiceDB with CockroachDB could display the New Enemy problem if mitigations are not put into place. To see how this could happen, we need to take a closer look at how CockroachDB stores data and synchronizes clocks.


## Is a New Enemy Possible?


Without any mitigations in place, is there a series of writes to a CockroachDB-backed SpiceDB that could result in a "new enemy"?


As stated above, we're not guaranteed external consistency for non-overlapping keys. It helps to understand how Cockroach[translates relational data into its underlying KV store](https://www.cockroachlabs.com/blog/distributed-sql-key-value-store/) , but for the purposes of this discussion it is enough to know that for the transactions to definitively overlap, they must touch the same row in the same (relational) table.


Relationships in SpiceDB are stored in one table, and are roughly:


1


```text
<resource type> <resource id> <relation> <subject type> <subject id> (optional <relation>)


```


1


```text


```


Let's cut to the chase and look at an example of a schema and some relationships that could result in the New Enemy Problem:


zed


1


2


3


4


5


6


```text
definition    user    {  }
definition    resource    {
relation    direct  :  user
relation    excluded  :  user
permission   allowed  =   direct  -   excluded
}


```


zed


1


2


3


4


5


6


```text
definition    user    {  }
definition    resource    {
relation    direct  :  user
relation    excluded  :  user
permission   allowed  =   direct  -   excluded
}


```


This defines two resource types:` user` and` resource` .


A` user` is` allowed` to access a` resource` if:


- the` resource` has the relation` direct` to the` user`
- and the` resource` does not have the relation` excluded` to the` resource` .


This is not an unusual schema: perhaps a document is viewable by all employees except external contractors.


To display the New Enemy problem, we'll perform two writes and a check:


1. T1 = Write` resource:thegoods#excluded@user:me`
2. T2 = Write` resource:thegoods#direct@user:me`
3. Check` resource:thegoods#allowed@user:me` *at snapshot T2*


Each of these calls happens in sequence (not parallel). If everything goes as intended, the Check will fail, because the snapshot T2 will contain both relationships (since T1 < T2).


However, the rows written to in T1 and T2 do not overlap. This means that cockroach *could* assign timestamps such that T2 < T1. In this case, the check in 3 will succeed - despite observing the writes in the proper order externally. This is exactly the "new enemy" problem: access was granted to a resource that should have been protected.


This doesn't mean we can't use CockroachDB, it just means that we need to find a way to prevent this particular scenario from happening.


## Properties of CockroachDB


Before we look at solutions to the problem, it would be nice if we had a test that could demonstrate this in the real world, so that we can demonstrate the efficacy of our mitigation.


We know that CockroachDB *can* assign transactions inconsistent timestamps for non-overlapping keys, but how and when does it actually happen?


We started by running the three steps above (Write, Write, Check) against a CockroachDB cluster in a loop. If this is an event that can occur, we should be able to see it with enough tries, right?


But after hundreds of thousands of tries, we couldn't reproduce the issue.


There are a few reasons why:


### Cockroach Cloud Provides Highly-Synchronized Nodes


We started by running against a staging cluster in[CockroachCloud](https://cockroachlabs.cloud/) , but CockroachCloud provides nodes that are very highly synchronized.


Here's a snapshot of our Clock Offset:


The scale here is microseconds, and the` max_offset` for the cluster is 500 **milliseconds** . With these numbers, it would be incredibly difficult to see a timestamp reversal.


### Writes to the Same Range Are Synchronized


Cockroach translates relational data into key-value range writes. Each range is a raft cluster with a leader coordinating the writes. A transaction's[closed timestamps](https://www.cockroachlabs.com/docs/v21.1/architecture/transaction-layer#closed-timestamps) ensure that any writes into the same range will have properly ordered timestamps.


Ranges are split and merged as needed by Cockroach, and you can't tell Cockroach which range a particular write should hit. Even with heavy write traffic, we'd need to get lucky and have the two specific writes land in separate ranges.


### Timestamp Cache


Any write into a range propagates changes to the[timestamp cache](https://www.cockroachlabs.com/docs/stable/architecture/transaction-layer.html#timestamp-cache) to nodes holding followers for the range.


This means that not only do our two writes need to land in different ranges, they need to land in ranges with leaders on nodes that don't contain followers (in the raft sense) of the other range.


By default, ranges for relational data in CockroachDB have a replica count of 3. This means that in a Cockroach cluster with 3 or fewer nodes, the timestamp caches will be synchronized after every write, and it won't be possible to see out of order timestamps.


## Intentionally Breaking CockroachDB


Luckily, Cockroach has parameters that we can tune that will help us get writes with "reversed" timestamps.


1. We can make the ranges very small. With a small maximum range size, we'll be more likely to have data written into different ranges.
2. We can lower the replica count. By setting the default replica count to 1, we can ensure no other nodes contain followers, and therefore have a better chance of having nodes assign out-of-order timestamps.


This is the SQL required to configure Cockroach with these (bad!) settings:


sql


1


```text
ALTER   DATABASE spicedb CONFIGURE ZONE  USING   range_min_bytes  =    0  , range_max_bytes  =    65536  , num_replicas  =    1  ;


```


sql


1


```text


```


But this was still not enough to see the problem in practice.


### Modifying Time


Time sources in GKE are still generally good enough that even a poorly configured Cockroach cluster couldn't demonstrate the problem.


We experimented with several methods to lie about time to a cockroach node:


1. [libfaketime](https://github.com/wolfcw/libfaketime) can intercept time-related syscalls and return fake answers. CockroachDB is written in golang, though, which[doesn't directly call time-related syscalls](https://go-review.googlesource.com/c/go/+/69390/) whenever possible, so this doesn't work for Cockroach.
2. [linux time namespaces](https://man7.org/linux/man-pages/man7/time_namespaces.7.html) are relatively new and seemed promising at first. However, they do not allow modifying` CLOCK_REALTIME` and therefore don't help in this instance.


Ultimately, we turned to[chaos-mesh](https://chaos-mesh.org/) , a CNCF sandbox project that happens to provide support for[faking vDSO time calls](https://chaos-mesh.org/docs/simulate-time-chaos-on-kubernetes/) . The subproject[watchmaker](https://github.com/chaos-mesh/chaos-mesh/tree/master/cmd/watchmaker) allows us to quickly adjust the time in a running cockroach instance.


Another advantage of this approach is that because we're not relying on separate machines for time skew, we can run multiple Cockroach nodes on the same machine and still produce the problem.


*Note: watchmaker does not yet support vDSO swapping on arm, and` ptrace` currently is not supported in Docker's qemu x86 emulation. This means there's not a good way to run watchmaker against cockroachdb on an m1 mac.*


### Network Delay


Cockroach nodes, even without transactions forcing the timestamp caches into sync, are always heartbeating. Even with time delay, it could sometimes take a long time to see the issue in practice.


This was simpler to achieve than skewing time; though we again turn to chaos-mesh for[chaosd](https://github.com/chaos-mesh/chaosd) to easily adjust delay at runtime.


## Putting It All Together


With these tools in our toolkit, the test is coming together:


1. Start a cockroach cluster with 3 nodes
2. Set a time delay on one node
3. Set a network delay on the same node
4. Configure the cluster to have small ranges and a replica count of 1
5. Prime the ranges by writing a large number of relationships


Then, in a loop:


1. T1 = Write` resource:thegoods#excluded@user:me`
2. T2 = Write` resource:thegoods#direct@user:me`
3. Check` resource:thegoods#allowed@user:me` *at snapshot T2*


Most of the time, this setup can get T2 < T1 within the first few tries, and the Check (that should fail) succeeds instead.


## Preventing New Enemy


The relevant properties of CockroachDB suggest two approaches to preventing the New Enemy problem:


1. Sleeping for` max_offset` after a write before returning to a client (and preventing further writes during that time).
2. Ensuring that any writes that could result in the new enemy problem produce overlapping transactions.


(1) is similar to the approach spanner takes, but without highly accurate time sources, the time we have to wait will unacceptably slow down writes. For SpiceDB, we started exploring option (2).


### What Is an Overlapping Key?


From the CockroachDB docs, it appears that all we would need to do to force the transactions to overlap is read a common key.


However, through testing (now that we have a test!), we determined that we needed both transactions to write to the same key in order for Cockroach to assign the timestamps properly.


In SpiceDB, you can pick two strategies for writing these keys. The simplest (and the default)` static` strategy is that all relationship writes touch the same row. This does add a small amount of latency to the write path, but it leaves the read paths performant, which matches the expected usage of a permission system.


The second strategy is` prefix` . If you are careful with how you design and interact with permissions in SpiceDB, you can choose to have multiple overlap domains by prefixing the definitions in your schema.


For example:


1


2


3


4


5


6


7


8


```text
definition sys1/user {}
definition sys1/resource {
relation direct: sys1/user
relation excluded: sys1/user
permission allowed = direct - excluded
}


definition sys2/user {}


```


1


2


3


4


5


6


7


8


```text
definition sys1/user {}
definition sys1/resource {
relation direct: sys1/user
relation excluded: sys1/user
permission allowed = direct - excluded
}


definition sys2/user {}


```


Writes to` sys1` -prefixed definitions will overlap with other writes to` sys1` -prefixed definitions, but not with` sys2` -prefixed definitions.


This allows advanced users to optimize overlap strategy, but we don't think it's something you'll need to worry about in general. The default is safe and fast.


## Testing a Negative


The tricky thing about our test for the New Enemy problem is that there are still enough variables outside our control that we can't guarantee it will happen in one try.


Usually it will happen within a few tries, but sometimes it may take dozens or even hundreds of tries.


How then can we be confident that the problem has been fixed?


We take a few tricks from statistics to increase our confidence:


1. Run the test (without the mitigations enabled) multiple times, and track how many tries it took to see the problem each time.
2. Use those values to determine a number of iterations to attempt to reproduce the problem, but with the mitigations enabled. This number is selected such we fall three standard deviations away from the mean from the unprotected case.


In the best case scenarios, the iterations we pick in (2) are very small (because we successfully showed the problem very quickly in (1)). But occasionally the number picked in (2) is very high, and the test runs for a bit longer to increase confidence.


## Looking Forward


To recap, we have now:


1. Understood what the New Enemy problem is
2. Understood how the consistency properties of Cockroach DB mean that a New Enemy problem is theoretically possible
3. Written a test that can demonstrate the New Enemy problem
4. Learned about how CockroachDB handles transactions in order to increase the likelihood of reproducing the New Enemy problem
5. Added mitigations to the CockroachDB driver for SpiceDB, and written CI tests to convince ourselves of their efficacy


You can check out the result of this[work](https://github.com/authzed/spicedb/pull/75) in the[SpiceDB tests](https://github.com/authzed/spicedb/tree/main/e2e/newenemy) .


SpiceDB is always improving, and we follow the work that CockroachDB does closely. We're excited to try out new approaches to this problem as CockroachDB evolves, and now we have a test framework to quickly try new solutions.


If you have an idea for how we can improve this, don't be shy![Let us know](https://authzed.com/discord) what you think.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


## Footnotes


1.


if clocks get very quickly out of sync, this may not hold↩


On this page


- Zanzibar Requires Spanner
- SpiceDB Without Spanner
- CockroachDB vs. Spanner
- Is a New Enemy Possible?
- Properties of CockroachDB
- Cockroach Cloud Provides Highly-Synchronized Nodes
- Writes to the Same Range Are Synchronized
- Timestamp Cache
- Intentionally Breaking CockroachDB
- Modifying Time
- Network Delay
- Putting It All Together
- Preventing New Enemy
- What Is an Overlapping Key?
- Testing a Negative
- Looking Forward
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
