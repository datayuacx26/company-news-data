---
schema_version: "1.0.0"
document_id: "43a807c361ce168f2f70cdb27590e3a0f797f508fa99a82052018f1d4289d52b"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/Data-Replication-Mechanisms"
published_at: "2025-01-31T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:ae14ca48bb03c9134d243478f555b9cfbd180dc4748eecc566e454d2553e91de"
---

# A Deep Dive into Data Replication Mechanisms

In today's tech world, it's crucial for businesses to avoid data loss and system slowdowns. Data replication is one method that can help.


Data replication means having the same data available in multiple places at once. It's not just about backups — it is about making sure your data is always accessible, reliable, and performs well. However, data replication alone is not enough, verifying backups, including ensuring data is


restorable and retrievable


is essential for replication failure and accidental deletion of data. This is key for disaster recovery and load balancing, forming the backbone of high-availability systems.


Data replication is crucial for any organization. At Target, it helps ensure guests have great and consistent experiences during times of high volume.


Whether you're managing large enterprise systems or starting a cloud application, replication is essential for building robust, scalable, and reliable systems.


The benefits of data migration include


:


- Backup:


If a server crashes or something goes wrong, then you have a backup copy.


- Speed:


H


aving a copy of your data closer to your app improves speed. The closer the data, the faster the performance.


- Reliability:


If one copy of the data fails, there are verified backups available.


Replication Scenarios: Primary-Replica Architecture


In the above diagram, the company relies on a single database, creating a single point of failure. If the database fails, all clients must wait for a restart, leading to poor user experience. A better approach is to create a replica of the database and synchronize it with the original. This way, write requests go to the primary database, and updates are copied to the replica.


The advantage is that you can distribute read requests between the primary and replica databases. This improves performance, ensures better availability, and is beneficial for read-intensive operations.


Here is a quick look at the benefits of and drawbacks to replication.


Benefits


- Fault Tolerance:


If the primary database fails, a replica provides a failover option, eliminating a single point of failure.


- Improved Read Speeds:


Read requests can be distributed between the primary and replica databases, improving read speed.


- Relative Simplicity:


Creating a replica is relatively simple compared to other solutions.


Potential Drawbacks


- Consistency Issues:


There can be a lag in data synchronization between the primary and replica databases, leading to consistency issues.


- Slightly Slower Write Speeds:


Synchronizing data with the replica can slightly slow down write speeds.


- Increased Latency:


Synchronizing write data between the primary and replica databases can increase latency.


How does primary-replica architecture work?


Both the primary and replica databases start in the same state. Complex operations are initially copied onto both databases to ensure they are synchronized


.


When a new write operation occurs on the primary database, it is also sent to the replica for synchronization. This process uses a Write Ahead Log (WAL) to record and apply changes sequentially, ensuring data consistency and allowing for rollbacks if needed.


It’s like how transaction logs work, providing a mechanism to maintain data consistency between the primary and replica databases.


Concepts of WAL


Write Ahead Logging (WAL) involves logging changes before they are applied to the database, ensuring that all changes are recorded first. This pre-write logging helps maintain durability, allowing the database to recover and stay consistent after a crash by reapplying logged changes. WAL also guarantees atomicity, meaning transactions are all-or-nothing, and consistency, ensuring that changes are either fully completed or fully rolled back to prevent partial updates.


The benefits of WAL include efficient transfer of only necessary operations, making the process streamlined. However, there are potential drawbacks, such as consistency issues if the replica can't keep up with the primary database, and confusion due to different timestamps between the primary and replica databases.


Solution: Change Data Capture (CDC)


Change Data Capture (CDC) is a strategy for monitoring and recording changes in a database. By capturing these changes, CDC ensures system consistency by updating all relevant systems or services with any new data. This helps keep everything uniform across distributed systems.


CDC works through observing a database’s transaction logs. Anytime there is a modification of data such as an insert, update or delete it is written in this log file. The change entries are then captured by CDC and sent to subscribers as change events which may be other databases, data lakes or microservices. It may be done either in real-time or near-real time depending on what the system expects.


CDC is especially useful for databases optimized for writing or reading. It also supports effective data transformation and has built-in libraries for connecting with several types of databases, making data replication easier.


Benefit of primary-replica architecture vs. two primary databases


Having two primary databases can cause inconsistencies, known as "split brain." This happens when two servers update the same value differently. For example, if one server sets a value to 10 and another sets it to 30 at the same time, it is unclear which value is correct (see diagram below).


To avoid split brain, strategies like "last write wins" and manual reconciliation can be used, but they come with risks and complexities. Another solution is "quorum," where an odd number of nodes agree on one value.


Challenges with Multiple Read Replicas


Some might think using multiple-read replicas instead of multiple primary databases might solve the split-brain problem, but it does not.


Using multiple read replicas can improve your system's read performance, but it comes with challenges like "write amplification" and increased latency. Write amplification is when updates need to be applied to multiple read replicas, slowing the system. Potential latency problems can come from synchronizing multiple read replicas, which can slow the system.


In the above figure, we are trying to update a value of X in all three replicas. If any replica fails, we will face challenges and need to make a decision:


1. Accept the operation.


Accepting the operation when one replica fails can lead to consistency issues across replicas.


2. Hold the client request.


Holding the client request until all replicas are operational may increase latency and reduce fault tolerance.


The challenges include increased load on the primary database due to multiple change requests across replicas, and the need to choose between potential inconsistency and increased latency.


Recommended Solution


To solve the split-brain problem, you can implement a consensus mechanism like Paxos or Raft to ensure all replicas agree on values during replication. This approach ensures strong consistency, as all nodes must agree on a value before performing a write operation. In the figure below, for example, if a write request comes to server S1, it first sends the request to all databases to log the operation.


The Paxos Algorithm


Locking in a distributed system is challenging, especially when dealing with failures and potential timeouts. Databases often use a two-phase commit protocol to lock rows when committing changes, but deciding which rows to lock can be difficult. The Paxos algorithm addresses this by making decisions once a majority of nodes in the cluster agree on one node, rather than requiring agreement from all nodes. This enables efficient distributed consensus.


See the above figure and consider a scenario with five servers and three databases handling write requests:


- Server s2 gets a write request to add 10 to variable X (X += 10). It logs this instruction and informs all three databases to persist the log.


- Meanwhile, another request comes to server s4 to subtract 10 from variable Y (Y -= 10), and this request is also sent to all databases.


- A third request arrives at server s5 to add 10 to variable Y (Y += 10), with the request being sent to all databases.


The challenge lies in the order of these operations. Even if the final value of Y is consistent (net effect is zero), the order of operations can be critical in scenarios like insufficient balance.


What is Consensus in the Paxos algorithm


Consensus is the process by which distributed systems agree on a single value. For example, to achieve consensus, at least two out of three databases must accept the write request received by server s1 in the diagram below.


When an incoming request arrives, such as


x = x + 10


to Server S1, the server first agrees on the log line (or position) where this data should be recorded in the database before actually writing the data.


In the diagram below, all databases must agree on a specific log line before the value is inserted into the database. Only after the log line is agreed upon and locked can the value be persisted in the database, and then the process moves to the next log line.


S2 asked


: Hey! What is the current log line for this transaction?


DB replied:


We are at log line 0 currently. We have not received any write requests yet. Yours will be the first.


S2 replies


: Great! Since no one has written to your database yet, I can write to log line 0.


If server s2 receives enough responses agreeing on the value it proposes, log line 0 will be locked, and no other write request can lock it. If more than half of the servers respond positively to a specific log line (in this case, log line 0), that log line will be locked, and the write operation will proceed based on it.


In the example, if two out of three servers agree, there is consensus, allowing the write to proceed at that log line. This ensures consistency if the majority of servers are operational. Even if one database goes down, consistency is maintained if the remaining servers (two in this case) are up and have agreed on the log line.


Conclusion


In any distributed system, data replication techniques like CDC, WAL, and Paxos play a crucial role in ensuring consistency, availability, and resiliency. These methods contribute to reliable data synchronization and fault tolerance, which are essential for supporting complex and scalable architectures.


References


Paxos Algo:


[https://medium.com/coccoc-engineering-blog/distributed-computing-paxos-algorithm-part-2-4d684b2c504d](https://medium.com/coccoc-engineering-blog/distributed-computing-paxos-algorithm-part-2-4d684b2c504d)


Quorum:


[https://medium.com/@sunny_81705/quorum-in-distributed-systems-37cbe17aae88](https://medium.com/@sunny_81705/quorum-in-distributed-systems-37cbe17aae88)
