---
schema_version: "1.0.0"
document_id: "384f48e729561a313af384de4003f4c8544e413d643bbea287df890547ba1e3f"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/the-dual-write-problem-in-spicedb-a-deep-dive-from-google-and-canva-experience"
published_at: "2025-09-16T08:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:4986cfcc49468319651e21fa8c25171b4dad6b65d502b4518a2419b483db0579"
---

# The Dual-Write Problem in SpiceDB: A Deep Dive from Google and Canva Experience

*This talk was part of the Authorization Infrastructure event hosted by AuthZed on August 20, 2025.*


## How We Are Solving the Dual Write Problem at Canva


In this technical deep-dive, Canva software engineer Artie Shevchenko draws on five years of experience with centralized authorization systems—first with Google's Zanzibar and now with SpiceDB—to tackle one of the most challenging aspects of authorization system implementation: the dual-write problem.


The dual-write problem emerges when data must be replicated between your main database (like Postgres or Spanner) and SpiceDB, creating potential inconsistencies due to network failures, race conditions, and system bugs. These inconsistencies can lead to false negatives (blocking legitimate access) or false positives (security vulnerabilities).


However, as Shevchenko explains, "the good news is centralized authorization systems, they actually do simplify things quite a bit." Unlike traditional event-driven architectures where teams publish events hoping others interpret them correctly, "with SpiceDB, you're fully in control" of the entire replication process.


SpiceDB offers several key advantages: "you're not replicating aggregates. Most often, it's simple booleans or relationships," making inconsistencies easier to reason about. Additionally, "the volume of replication is also much smaller" since authorization data can live primarily in SpiceDB, and you're "replicating just to SpiceDB, not to 10 other services."


The talk explores four solution approaches—from cron sync jobs to transactional outboxes—with real-world examples from Google and Canva. Shevchenko's key insight: "dual write is not a SpiceDB problem. It's a data replication problem," but "SpiceDB makes the dual write problem, and ultimately the data integrity problem, much more manageable."


### On Ownership and Control


> "First of all, as a team now, you own the whole replication process. Because you own both copies of the data. Which makes a huge difference. You're not just publishing an event that other teams would hopefully correctly interpret and apply to their data stores."


**Takeaway:** SpiceDB gives you complete control over your authorization data replication, eliminating dependencies on other teams and reducing coordination overhead.


### On Proven Scale


> "And then feed it as an input to our MapReduce style sync job, which would sync data for 100 millions of users in just a couple of hours."


**Takeaway:** SpiceDB's approach has been battle-tested at Google scale, handling hundreds of millions of users efficiently.


### On Technical Advantages


> "But, the first three approaches without Zanzibar or SpiceDB would be really tricky, if not impossible. Not only because of the data ownership problem, but also because of aggregates. With event-driven replication, you're probably not replicating simple atomic facts."


**Takeaway:** SpiceDB's simple data model (booleans and relationships) makes dual-write problems significantly more manageable compared to traditional event-driven architectures that deal with complex aggregates.


---


## Full Transcript


*Talk by Artie Shevchenko, Software Engineer at Canva*


### Introduction


All right, let's talk about the dual-write problem. My name is Artie Shevchenko, and I'm a software engineer at Canva. My first experience with systems like SpiceDB was actually with Zanzibar at Google in 2017. And now I'm working on SpiceDB integration at Canva. So, yeah, almost five years working with this piece of tech.


### Why SpiceDB Simplifies Authorization


And from my experience, there are two hard things in centralized authorization systems. It's dual-writes and data backfills. But neither of them is unique to Zanzibar or SpiceDB. In fact, dual-write is a fairly standard problem. And when we're talking about replication to another database, it is always challenging. Whether it's a permanent replication of some data to another microservice, or migration to a new database with zero downtime, or even replication to SpiceDB.


The good news is centralized authorization systems, they actually do simplify things quite a bit. First of all, as a team now, you own the whole replication process. Because you own both copies of the data. Which makes a huge difference. You're not just publishing an event that other teams would hopefully correctly interpret and apply to their data stores. With SpiceDB, you're fully in control.


Secondly, with SpiceDB, you're not replicating aggregates. Most often, it's simple booleans or relationships. Which makes it much easier to reason about the possible inconsistencies.


And finally, the volume of replication is also much smaller. For two reasons. First, most of the authorization data you can store in SpiceDB only, once the migration is done. And second, with SpiceDB, you need to replicate just to SpiceDB, not to 10 other services. Well, there are also search indexes, but they're very special for multiple reasons. And the good news is search indexes, you don't need to solve them on the client side. Mostly, you can just delegate this to tools that materialize.


But that said, even with replication to SpiceDB, there is a lot of essential complexity there that first, you need to understand. And second, you need to decide which approach you're going to use to solve the dual-write problem.


### Talk Structure and Definitions


The structure of this talk, unlike the topic itself, is super simple. I don't have any ambition to make the dual-write problem look simple. It's not. But I do hope to make it clear. So, the goal of this talk is to make the problems and the underlying causes clear. And we're going to spend quite a lot of time unpacking what are the practical problems we're solving. And then, talking about the solution space, the goal is to make it clear what works and what doesn't. And, of course, the pros and cons of the different alternatives.


But let's start with a couple of definitions. Almost obvious definitions aside, let's take a look at the left side of the slide, at the diagrams. Throughout the talk, we'll be looking into storing the same piece of data in two databases. Of course, ideally, you would store it in exactly one of them. But in practice, unfortunately, it's not always possible, even with SpiceDB.


So, when information in one database does not match the information in another database, we'll call it a discrepancy or inconsistency. Or I'll simply say that databases are out of sync.


When talking about the dual-write problem in general, I'll be using the term "source of truth" for the database that is kind of primary in the replication process. And the second database I'll call the second database. I was thinking about calling them primary and replica or maybe master and slave. But the problem is, these terms are typically used to describe replication within the same system. But I want to emphasize that these are different databases. And also, the same piece of knowledge may take very different forms in them. So, I'll stick to the terms "source of truth" and just some other second database. That's when I talk about the dual-write problem in general.


But not to be too abstract, we'll be mostly looking at the dual-write problem in the context of data replication to SpiceDB, not just to some other abstract second database. And in this case, instead of using the term "source of truth," I'll be using the term "main database," referring to the traditional transactional database where you store most of your data, like Postgres, Dynamo, or Spanner. Because for the purposes of this talk, we'll assume that the main database is a source of truth for any replicated piece of data. Yes, theoretically, replicating in the other direction is also an option, but we won't consider that. We're replicating from the main database to SpiceDB.


So, in different contexts, I'll refer to the database on the left side of this giant white replication arrow as either "source of truth" or "main database" or, even more specifically, Postgres or Spanner. Please keep this in mind.


And finally, don't get confused when I call SpiceDB a database. Maybe I can blame the name. Of course, it's more than just a database. It is a centralized authorization system. But in this talk, we actually care about the underlying database only. So, hopefully, that doesn't cause any confusion.


### Defining the Dual-Write Problem


All right. We're done with these primitive definitions. Now, let's define what the dual-write problem is. And let's start with an oversimplified but real example from home automation.


Let's say there are two types of resources, homes and devices. Users can be members of multiple homes, and they have access to all the devices in their homes. So, whether a device is in one home or another, that information obviously has to be stored both in the main database, in this case, Spanner, and in SpiceDB.


And if you want to move a device from one home to another, now you need to update the device's home in both databases. If you get a task to implement that, you would probably start with these two lines of code. You first write to the source of truth, which is Spanner, and then write to the second database, which is SpiceDB. The problem is you cannot write to both data stores in the same transaction, because these are literally different systems.


So, a bunch of things can go wrong. If the first write fails, it's easy. You just let the error propagate to the client, and they can retry. But what about the second write? What if that one fails? Do you try to revert the first write and return an error to the client? But what if reverting the first one fails? It's getting complicated.


Another idea. Maybe open a Spanner transaction and write to SpiceDB with the Spanner transaction open. I won't spend time on exploring this option, but it also doesn't solve anything, and in fact, just makes things worse. The truth is, none of the obvious workarounds actually make things better.


So, we'll use these two simple lines of code as a starting point, and just acknowledge that there is a problem for us to solve there. The second write may fail for different reasons. It's either because of a network problem, or a problem with SpiceDB, or even the machine itself terminating after the first line. In all of these scenarios, the two databases become out of sync with each other. One of them will think that the device is in Home 1, and another will think that it is in Home 2.


### Data Integrity Problems: False Negatives and False Positives


The second write failing can create two types of data integrity problems. It's either SpiceDB is too restrictive. It doesn't allow access to someone who should have access, which is called a false negative on the slides. Or the opposite. SpiceDB can be too permissive, allowing access to someone who shouldn't have access. False negatives are more visible. It's more likely you would get a bug report for it from a customer. But false positives are actually more dangerous, because that's potentially a security issue.


We've already tried several obvious workarounds, and none of them worked. But let's give it one last shot, given that it is false positives that are the main issue here. Maybe there is a simple way to get rid of those. Let's try a special write operations ordering. Namely, let's do SpiceDB deletes first. Then, in the same transaction, make all the changes to the main database. And then, do SpiceDB upserts.


So, in our example, the device is first removed from home 1 in SpiceDB. And then, after the Spanner write, the device is added to home 2 in SpiceDB. And it actually does the trick. And it's easy to prove that it works not only in this example, but in general. If there are no negations in the schema, such an ordering of writes ensures no false positives from SpiceDB. So, now the dual write problem looks like this. Much better, isn't it? No security issues anymore.


Let me play devil's advocate here. If the second or the third write fails, let's say, 100 times per month, we would probably hear from nobody. Or maybe one user. But for one user, can you fix it manually? But aren't we missing something here?


### The Race Condition Problem


The problem is, there is a whole class of issues we've ignored so far. It's race conditions. In this scenario from the slide, we're doing writes in the order that was supposed to totally eliminate the false positives. But as a result of these two requests from Alice and Bob, we get a false positive for Tom. That's because we're no longer talking about failing writes. None of the writes failed in this scenario. It is race conditions that caused the data integrity problem here.


So, we have identified two causes or two sources of discrepancies between the two databases. The first is failing writes. And the second is race conditions. So, unfortunately, yet another workaround doesn't really make much difference. Back to our initial simple starting point. Two consecutive writes. First write to the main database. And then write to SpiceDB. Probably in a try-catch like here.


And one last note looking at this diagram. Often people think about the dual write problem very simplistically. They think if they can make all the writes eventually succeed, that would solve the problem for them. So, all they need is a transactional outbox or a CDC, change data capture, or something like this. But that's not exactly the case. Because at the very least, there are also race conditions. And as we'll see very soon, it's even more than that.


### Adding Backfill Complexity


And now, let's add backfill to the picture. If you're introducing a new field, a new type of information that you want to be present in multiple databases, you just make the schema changes, implement the dual write logic, and that's it. You can immediately start reading from the new field or a new column in all the databases. But if it's not a new type of information, if there is pre-existing data, then the data needs to be backfilled.


Then the new column, field, or relation goes through these three phases. You can say there is a lifecycle. First, the schema definition changes. New column is created or something like this. Then, dual write is enabled. And finally, we do a backfill, which iterates through all of the existing data and writes it to the second database. And once the backfill is done, the data in the second database is ready to use. It's ready for reads and ready for access checks if we're talking about SpiceDB.


And as it's easy to see from the backfill pseudocode, backfill also contributes to race conditions. Simply because the data may change between the read and write operations. And again, welcome false positives.


Okay. So far, we've done two things. We've defined the problem. And we've examined multiple tempting workarounds just to find that they don't really solve anything. Now, let's take a look at several approaches used at Google and Canva that actually do work. And, of course, discuss their trade-offs.


### Solution Approaches


#### Approach 1: Cron Sync Jobs (Google's Solution)


First of all, doing nothing about it is probably not a good idea in most cases. Because authorization data integrity is really important. It's not only false negatives. It is false positives as well, which, as you remember, can be a security issue. The good news is there are multiple options to choose from if you want to solve the dual-write problem.


And let's start with a solution we used in our team at Google, which is pretty simple. We just had a cron sync job. That job would run several times per day and fix all the discrepancies between our Spanner instance and Zanzibar. Looking at the code on the right side, because of the sync job, we can keep the dual-write code itself very, very simple. It's basically the two lines of code we started with.


Sync jobs at Google are super common. And what made it even easier for us here is consistent snapshots. We could literally have a snapshot of both Spanner and Zanzibar for exactly the same instant. And then feed it as an input to our MapReduce style sync job, which would sync data for 100 millions of users in just a couple of hours.


And interestingly, sync jobs are the only solution that truly guarantees eventual consistency, no matter what. Because in addition to write failures and races, there is also a third problem here. It is bugs in the data replication logic.


Now, the most interesting part is how did it perform in practice? And thanks to our sync job, we actually know for sure how did it go. Visibility into the data integrity is a huge, huge benefit. We not only knew that all the discrepancies get fixed within several hours, but we also knew how many of them we actually had. And interestingly, the number of discrepancies was really high only when we had bugs in our replication logic. Race conditions and failed writes, they did cause some inconsistencies too. But even at our scale, there were a small number of them, typically tens or hundreds per day.


Now, talking about the downsides of this approach, there are two main downsides. The first one is there are always some transient discrepancies, which can be there for several hours. Because we're not trying to address race conditions or failing writes in real time. And the second problem is infra costs. Running a sync job for a large database almost continuously is really, really expensive.


#### Transactional Outbox Pattern Foundation


All right. We're done with the sync jobs. Now, all the other approaches we'll be looking at, they leverage the transactional outbox pattern. For some of those approaches, you could achieve similar results with CDC, change data capture, instead of the outbox. But outbox is more flexible, so we'll stick to it.


And at its core, the transactional outbox pattern is really, really simple. When writing changes to the main database, in the same transaction, we also store a message saying, "please write something to SpiceDB." And unlike traditional message queues outside of the main database, such an approach truly guarantees for us at-least-once delivery. And then there is a worker running continuously that pulls the messages from the outbox and acts upon them, makes the SpiceDB writes. Note that I mentioned a Zedtoken here in the code, but these are orthogonal to our topics, so I'll just skip them on the next slides.


As I already mentioned, the problem the transactional outbox solves for us is reliable message delivery. Once SpiceDB and the network are in a healthy state, all the valid SpiceDB writes will eventually succeed. One less problem for us to worry about. But similar to CDC, it doesn't solve any of the other problems. It obviously doesn't provide any safety nets for the bugs in the data replication logic. And as it's easy to see from these examples, the transactional outbox is also subject to race conditions. Unless there are some extra properties guaranteed, which we'll talk very, very soon about.


Okay. Now that we've set the stage with transactional outboxes, let's take a look at several solutions. The second approach to solving the dual-write problem is what I would call micro-syncs. Not sure if there's a proper term for it, but let me explain what I mean. In many ways, it's very similar to the first approach, cron sync jobs. But instead of doing a sync for the whole databases, we would be doing targeted syncs for specific relationships only.


For example, if Bob's role in Team X changed, we would completely resync Bob's membership in that team, including all his roles. So in the worker, we would pull the message from the outbox, then read the data from both databases, and fix it in SpiceDB if there are any discrepancies.


To make it scale, instead of writing it to SpiceDB from the worker directly, we can pull those messages in batches and just put them into another durable queue, for example, into Amazon SQS. And then we can have as many workers as we need to process those messages.


But aren't these micro-syncs subject to races themselves? They are. Here on this diagram, you can see an example of such a race condition creating a discrepancy. But adding just a several-seconds delay makes such races highly unlikely. And for our own peace of mind, we can even process the same message again, let's say in one hour. Then races become practically impossible. I mean, yes, in theory, the internet is a weird thing that doesn't make any guarantees. But in practice, even TCP retransmissions, they won't take an hour.


So the race conditions are solved with significantly delayed micro-syncs. And you can even do multiple syncs for the same message with different delays.


Now, what about bugs in the data replication logic? And in practice, that's the only difference with the first approach, is that micro-syncs, they do not cover some types of bugs. Specifically, let's say you're introducing a new flow that modifies the source of truth, but then you simply forget to update SpiceDB in that particular flow. Obviously, if there is no message sent, there is no micro-sync, and there would be a discrepancy. But apart from that, there are no other substantial downsides in micro-syncs. They provide you with almost the same set of benefits as normal sync jobs, and even fix discrepancies on average much, much faster, which is pretty exciting.


And finally, let's take a look at a couple of options that do not rely on syncs between the databases. Let's introduce a version field for each replicated field. In our home automation example, it would be a home version column in the devices table, and a corresponding home version relation in the SpiceDB device definition. And then we must ensure that each write to the home ID field in Spanner increments the device home version value. And then in the message itself, we also provide this new version value so that when the worker writes to SpiceDB, it can do a conditional write to make sure it doesn't override a newer home value with an older one.


And there are different options for how to implement this. But none of them are really simple. So introducing a bug in the replication logic, honestly, is pretty easy. And the worst thing is, unlike sync jobs or even micro-syncs, this approach doesn't provide you with any safety nets. When you introduce a bug, it won't even make it visible. So yeah, that's the three downsides of this approach. It's complexity, no visibility into the replication consistency, and no safety nets. And the main benefit is, it does guarantee there would be no inconsistencies from race conditions or failed writes.


And the last option is here more for completeness. To explore the idea that lies on the surface and, in fact, almost works, but there are a lot of nuances, limitations, and pitfalls to avoid there. And that's the only option where we solve the dual write problem by actually abandoning the dual write logic. So let's say we have a transactional outbox. And the only thing the service code does, it writes to the main database and the transactional outbox. No SpiceDB writes there. So there is no dual write.


And there is just a single worker that processes a single message at a time, the oldest message available in the transactional outbox, and then it attempts to make a SpiceDB write until it succeeds. So the transactional outbox is basically a queue. And that by itself guarantees eventual consistency. I'll give you some time to digest this statement.


You can prove that as long as there are no bugs, the transactional outbox is a queue, and there is a single consumer, eventual consistency between the main database and SpiceDB is guaranteed. Because it's FIFO, first in, first out, and there are no SpiceDB writes from service code.


However, a single worker processing one message at a time from a queue wouldn't provide us with a high throughput. So you might be tempted to, instead of writing to SpiceDB directly from the worker, to put it into another durable queue. But I'm sure you can see the problem with this change, right? We've lost the FIFO property. So now it's subject to races. Unless that second queue is FIFO as well, of course. But if it's FIFO, guess what? We're not increasing throughput.


So yeah, if we're relying on the FIFO property to address race conditions, there is literally no reason to transfer messages into another durable queue. If you want to increase the throughput, just use bulk SpiceDB writes\]. But you would need to preprocess them to make sure there are no conflicts within the same batch. Yes, there is no horizontal scalability, but maybe that's not a problem for you.


Yet, what would probably be a problem for most use cases is that a single problematic write can stop the whole replication process. And once we actually experienced exactly this issue, a single malformed SpiceDB write halting the whole replication process for us. And that's pretty annoying, as it requires manual intervention and is pretty urgent.


And yet another class of race conditions is introduced by backfills. Because FIFO is a property of the transactional outbox. But backfill writes, fundamentally, they do not go through the outbox. So, yeah. While it's possible to introduce a delay to the transactional outbox specifically for the backfill phase, to address it, I would say the overall amount of problems with this approach is already pretty catastrophic.


So, let's do a quick summary. We've explored four different approaches to solving the dual write problem. And here is a trade-off table with the pros and cons of each of them. The obvious loser is the last FIFO transactional outbox option. And probably conditional writes with the version field are not the most attractive solution as well. Mostly because of their complexity and lack of visibility into the replication consistency.


So, the two options we're probably choosing from are the first and the second one. It's two types of syncs. Either a classic cron sync job or micro syncs. And, yeah. You can totally combine most of these approaches with each other if you want.


We're almost done. I just wanted to reiterate the fact that dual write is not a SpiceDB problem. It's a data replication problem. So, let's say you're doing event-driven replication. Strictly speaking, there are no dual writes, same as in the last FIFO option. But, ultimately, there are two writes to two different systems, to two different databases. So, we're facing exactly the same set of problems.


Adding a transactional outbox can kind of ensure that all the valid writes will eventually succeed. But, probably only if you own the other end of the replication process. Then, you can also add the FIFO property to address race conditions, which is option four. But, the first three approaches without Zanzibar or SpiceDB would be really tricky, if not impossible. Not only because of the data ownership problem, but also because of aggregates. With event-driven replication, you're probably not replicating simple atomic facts.


So, yeah. SpiceDB makes the dual write problem, and ultimately the data integrity problem, much more manageable.


And that's it. Hopefully, this presentation brought some clarity into the highly complex dual write problem.
