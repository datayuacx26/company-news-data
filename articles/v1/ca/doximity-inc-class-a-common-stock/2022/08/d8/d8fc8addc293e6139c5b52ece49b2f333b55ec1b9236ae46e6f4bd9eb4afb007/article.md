---
schema_version: "1.0.0"
document_id: "d8fc8addc293e6139c5b52ece49b2f333b55ec1b9236ae46e6f4bd9eb4afb007"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/integrating-neo4j-into-your-stack"
published_at: "2022-08-29T10:10:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:5bc9e65662dcdeeb6e1da9f27b198b8930484acd308be81966482f1569958d8e"
---

# Integrating Neo4j Into Your Stack

If you’ve decided to incorporate a graph database into your application stack then this article is for you. Graph databases can be difficult to integrate without some strong foresight. I’m here to help! Having just implemented the latest[Neo4j](https://neo4j.com/product/neo4j-graph-database/) and a revised architecture, I’ve compiled all of my notes and I want to share the decisions we have made. This article will be about how to bring Neo4j into a Rails application in an organized, fault-tolerant, and asynchronous way.


## Some Background


Before we get into the how - let's briefly talk about what kinds of things we do with Neo4j. We are a medical professional network, and as a network of users, it is helpful to model your relational data in this way. We can take any two users in the system and determine if and how they are connected. Or calculate if these two users are a first or second-degree connection to each other. Or identify which colleagues they have in common.


This is what Neo4j excels at. You take any two nodes (in our case two User nodes) and ask the graph how to connect them. Graph-type questions are sometimes answerable in relational SQL but not in a performant way, if even possible at all. With Neo4j you don’t have to know the "join tables" in advance - you can give it a wildcard and it will tell you how they are connected, or even connected via other connections (like the[Kevin Bacon game](https://en.wikipedia.org/wiki/Six_Degrees_of_Kevin_Bacon) !)


## The Workflow


We have two categories of[Cypher query operations](https://neo4j.com/developer/cypher/) - READs and WRITEs. Each category has its own workflow. For READ operations we wanted a performant, low latency interface for clients to use so they query the database directly. For WRITE operations we wanted these to be asynchronous and retriable. These categories will be explained in more detail below:


### READs


We use a gem to house our queries and abstract a bit about the Neo4j database. A read request would look like this:


```text
client queries data ->
calls Neo4j HTTP API ->
Neo4j runs query ->
returns results ->
client interprets results


```


1. client queries data - this is the client issuing a READ Cypher query through a convenience gem
2. calls Neo4j HTTP API - the gem handles the authentication and issues an auto-commit transaction statement
3. Neo4j runs query - the READ query is run on Neo4j
4. Results are collected in JSON format and parsed by the client
5. Client interprets results


We have some convenient methods for calling the Neo4j server, and parsing the results, handling any errors like timeouts. Note again that the clients talk directly to Neo4j via the HTTP API. These are synchronous calls with minimal overhead.


### WRITEs


The WRITE flow is a bit more complex because we do our WRITEs through a message-based architecture using[Kafka](https://www.confluent.io/what-is-apache-kafka) . Previously we've had issues with Neo4j being unable to upsert a node, failing, and then rolling back a web-based transaction. We now handle this async so we completely isolate the web transaction from a user, with the graph database upsert transaction. The latter could fail and the user would not be affected. A WRITE request would look like this:


```text
client makes a change ->
ActiveRecord lifecycle hook publishes Kafka message ->
graph database service consumes message ->
calls Neo4j HTTP API ->
upserts structure


```


1. client makes a change - This can be the result of a user submitting a form, or anything that would insert, or update data
2. ActiveRecord lifecycle hook publishes Kafka message - we use an` after_commit: publish_kafka_message` in the ActiveRecord model
3. graph database service consumes message - a consumer group is run from a separate service that reads these messages and runs the appropriate graph command
4. Calls Neo4j HTTP API - we use the same HTTP API as we do for READ but with a WRITE access mode header
5. Upserts structure - this is a crucial design I will talk more in more detail shortly. The data is inserted or updated in Neo4j.


Because all we do from the client side is publish a message to a Kafka topic, Neo4j can be completely offline without any impact (other than delayed consistency). The message will stay on the topic until the consumer is able to successfully upsert the structure and advance the offset - more on upserts below.


### The Neo4j Server


Neo4j offers several ways to connect to its database. We decided to go with the[HTTP API](https://neo4j.com/docs/http-api/current/) which allows us to interact in a RESTful way. You can in fact do transactions that span multiple requests but for our purposes, we auto-commit our transactions per request. This sidesteps some issues with connection pooling, and low-level[Bolt adapters for Ruby](https://neo4j.com/docs/operations-manual/current/configuration/connectors/) . We keep the high interaction level at the cost of a little more latency.


The server setup itself is a cluster of Neo4j servers. These will load balance themselves but we can give it hints with routing headers. We recommended a minimum of 3 - one leader (for WRITE), and two followers (for READ).


We have also limited the value of the[global config option neo4j.vm.thread.count](https://neo4j.com/developer/kb/limiting-bolt-threads-vs-connections/) to prevent a large number of write operations from overwhelming the leader server.


#### Client side routing


The HTTP API accepts a special header called the[access mode](https://neo4j.com/docs/http-api/current/actions/transaction-configuration/) that allows you to specify if this is a READ or a WRITE operation. READ operations can go to any server in the cluster, but WRITE operations always go to the leader. This means that the WRITE server is special and can be a bottleneck with heavy traffic. Due to the nature of Kafka, you will get eventual consistency, and eventually, the writes will make it as fast as the consumers can deliver them.


We've opted to include this header in our gem that handles our HTTP API calls. Because our WRITEs are always done via Kafka message publishing we know that our clients will be using READ mode. If a Cypher query that attempts to update a structure is issued, Neo4j will actually respond with a Read-only error. This enforces our workflow in that any client can READ, but WRITE only happens asynchronously through the graph database service (which includes a WRITE access mode header)


## Upserts


We always want our operations to be idempotent. It is very possible that we will reset a Kafka consumer group offset and replay an entire topic. We don't want to fail if a given structure already exists so we make use of an[upsert pattern](https://neo4j.com/docs/cypher-manual/current/clauses/merge/#merge-merge-with-on-create-and-on-match) in Cypher via` MERGE` :


Our operations look something like:


```text
MERGE (n:User {name: 'Ben Simpson'})
ON CREATE
SET n.city = $city
ON MATCH
SET n.city = $city
RETURN u


```


This allows us to create a new node, or upsert an existing node transparently handling either scenario in a single Cypher statement. We do the same thing to upsert relationships:


```text
MERGE (from:User {uuid: $from.uuid})
MERGE (to:Publication {uuid: $to.uuid})
MERGE (from) - [relationship:AUTHOR_OF {uuid: $relationship.author_of_uuid}] - (to)
ON CREATE SET relationship += $relationship_attributes
ON MATCH SET relationship += $relationship_attributes
RETURN from, to, relationship


```


### Stubbing Nodes


Every structure should have a unique identifier. The identifier will be what we upsert on to guarantee eventual consistency.


Because we are dealing with asynchronous writes it is possible that we have an out-of-order scenario where we get relationship data before we get data about both nodes that the relationship connects to. This is solved by "stubbing" the nodes with an identifier when they are not found.


For example, one of our relationships is authorship. Our users can be the author of a publication and we want that data to go into the graph as` (n:User)-\[r:AUTHOR_OF\]-(p:Publication)` . What happens if we get the` AUTHOR_OF` message first, and do not yet have a matching` User` or` Publication` node? The message for the authorship relationship needs to include a unique identifier for all three pieces like the following JSON payload example:


```text
{
"author_of_uuid": "838828fa-58e3-49df-b587-e910396b93b9,
"user_uuid": "784f3f68-5fe0-4198-a38d-9b71cbcc9be8",
"publication_uuid": "a4d55d00-f78e-4511-bb56-1443130c4730"
}


```


When we upsert this relationship, we know we are looking for a` User` node with uuid "784f3f68-5fe0-4198-a38d-9b71cbcc9be8". If we don't find it, we upsert a new` User` node with just this property. Same for the` Publication` node. And finally, we upsert the` AUTHOR_OF` relationship with a uuid property between these two nodes.


Eventually, we should see a complete user-created message, and a complete publication-created message come through on different topics in Kafka. When this happens we upsert on the same` User` and` Publication` uuids and set additional properties on our nodes.


This allows us to handle out-of-order processing between nodes and relationships. If you abort the relationship when both nodes are not found you are at risk of missing this relationship unless you can guarantee the node's order is first.


## Additional Technologies


### Kafka


Kafka has been mentioned a few times already. To speak more to this setup we have multiple topics and multiple messages on those topics. An example topic name is` doximity.user_updates.v2` . A message name is` publication_added` with the payload containing information about a publication and importantly its uuid.


We've chosen to use uuids when we cross application boundaries as the data can be moved around without reliance on an internal auto-incrementing identifier like an` INT AUTO_INCREMENT` flag in a relational database. This allows us to delete a publication, insert it later, and keep the uuid value the same, despite a different id.


We also make use of[Avro](https://avro.apache.org/docs/1.11.1/) to strongly type our messages. It has good integration with Kafka too. This helps when producing and consuming the messages to understand what the contract is, and raise an exception when it has been violated. Because the schema registry is accessible to both producer and consumer the contract is agreed upon. This isn't strictly necessary but it does help when changing message payloads over time to understand what is backward and forward compatible.


#### Compacted topics


Kafka has several topic cleanup strategies. The default is a retention time period. Messages will be cleaned out of the topic after two weeks, or some other time interval. A better topic cleanup strategy for our use case is[compaction](https://kafka.apache.org/documentation/#compaction) . A compacted topic uses the key name of a message and only keeps the latest message with that key. If a user makes multiple updates, and we produce a message after each update, only the most recent message for that user is ultimately kept. Use a unique identifier as the key. We use the` uuid` from the database record.


Using Compacted topics means that the contents of the messages should not only be the diffs; but rather a complete representation of the state of the subject. Nothing in Kafka will enforce this but this is a best practice, as the latest message with a given key overwrites the previous messages with that key. This is not a merge but a replacement of that key's value.


The magic of the compacted topic is that it does not delete all messages after an interval of time. The latest message stays on the topic and will be available as new consumer groups are added. This prevents the dreaded backfill of topics and the spamming of repeat messages to all consumer groups on a topic when older data is needed.


A[tombstone message](https://kafka.apache.org/documentation/#design_compactionbasics) is what will mitigate the topic from filling up over time as records are deleted from the database. If a user is created, updated, and then deleted, this would take up zero bytes in a compacted topic (after compaction) because the last message of the user is that it was removed. A tombstone message signals the user is deleted and this key can be removed from the topic.


### Neo4j Schemas


Neo4j does offer constraints that can be applied to node and relationship properties. We have placed a unique constraint on our uuid properties for every structure so that we can be assured of data integrity. The upsert strategy should handle this, but this is a safeguard. Unique constraints may also be a query execution hint that there are no other structures once the first one is found.


An example of a constraint on a uuid property looks like this:


```text
CREATE CONSTRAINT IF NOT EXISTS FOR (n:User) REQUIRE n.uuid IS UNIQUE


```


This can be packaged up into a rake task or similar that you can call in your different environments.


### Gem for querying


In order for all applications at Doximity to be able to talk to the graph database in a consistent way, we have packaged up our Cypher queries into a gem. This gem can be included in any project and with a simple initializer, can be set up to issue queries. An example of calling one of the queries from the gem looks like this:


```text
Dox::Graphdb::Client::Repository.how_are_two_users_related($user_uuid, $other_user_uuid)


```


Anytime we change the definition of how a user is related (maybe with more relationships, or with changing business logic) we can update the gem, and then update the gem version in the clients.


The gem also houses all of our unit tests for these queries. We host this within the same service that runs all of our consumers and uses the same Neo4j test database instance for unit test setup and teardown.


## Conclusion


Graph databases are a great way to model data and answer questions that relational databases would struggle with. However, integrating the graph into your tech stack can be a challenging experience when it comes to stability and data integrity. By relying on a few other technologies like Kafka and Avro the connection points are easier to implement. A solid workflow is also critical for building the data pipeline into Neo4j. If you are not confident in your data integrity it makes the queries tough to trust.


I hope that the technologies and the workflows outlined in this article make setting up Neo4j easier!


---


Be sure to follow @doximity_tech if you'd like to be notified about new blog posts.
