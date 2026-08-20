---
schema_version: "1.0.0"
document_id: "c5d475978648a897742525806f039a33f1be05e0ac7eb0aa129fa41e9c9ab6b5"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/handling-distributed-transactions-in-c-with-sql-server-and-mongodb-2e948d9b1853"
published_at: "2025-05-14T09:02:35+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:3dff4fe67f3a6452b573433c5ae1d8e52f043b4b31531bce7dac0eb57e5fd971"
---

# Handling distributed transactions in C# with SQL Server and MongoDB

## Introduction


Transactions play a crucial role in ensuring data integrity and consistency when working with databases. In some scenarios, an application might need to insert data into both SQL Server and MongoDB as part of a single transaction. However, since SQL Server and MongoDB do not support distributed transactions together natively, alternative approaches must be considered to maintain data consistency.


This article provides a practical guide on implementing distributed transactions in Win OS and C#. It explores alternative solutions such as eventual consistency, compensating transactions, and event-driven architectures (e.g., Outbox Pattern).


## **The problem**


In many real-world enterprise systems, different components rely on different types of databases. For example:


- A **relational database** like **SQL Server** may handle structured financial records.
- A **NoSQL document database** like **MongoDB** may manage flexible metadata, logs, or analytics data.


The fundamental challenge is ensuring **data consistency** when **multiple data stores** are involved. Ideally, we want the operation to behave **transactionally** , meaning:


- **All-or-nothing behavior** : If one part of the operation fails, the other must be rolled back.


There are scenarios where a single **business operation** requires writing to **both** of these databases. For example: A new user signs up → their core identity is stored in SQL Server, while preferences and profile data are saved in MongoDB.


## Implementing a c *ombined t* ransaction


One approach to handling transactions across SQL Server and MongoDB is using MongoDB sessions and SQL Server transactions together. However, this solution has limitations and does not guarantee full atomicity across both databases.


*Example code for a combined transaction*


```text
using System;  using System.Data.SqlClient;  using MongoDB.Driver;  using MongoDB.Bson;   class Program  {      static void Main()      {          string sqlConnectionString = "your_sql_connection_string";          string mongoConnectionString = "your_mongodb_connection_string";                    var mongoClient = new MongoClient(mongoConnectionString);          var mongoDatabase = mongoClient.GetDatabase("your_database_name");          var mongoCollection = mongoDatabase.GetCollection<BsonDocument>("Users");                    using (var sqlConnection = new SqlConnection(sqlConnectionString))          {              sqlConnection.Open();              using (var sqlTransaction = sqlConnection.BeginTransaction())              using (var mongoSession = mongoClient.StartSession())              {                  mongoSession.StartTransaction();                  try                  {                      // Insert into SQL Server                      var sqlCommand = new SqlCommand("INSERT INTO Users (Name, Age) VALUES (@Name, @Age); SELECT SCOPE_IDENTITY();", sqlConnection, sqlTransaction);                      sqlCommand.Parameters.AddWithValue("@Name", "John Doe");                      sqlCommand.Parameters.AddWithValue("@Age", 30);                      var userId = Convert.ToInt32(sqlCommand.ExecuteScalar());                                            // Insert into MongoDB                      var mongoUser = new BsonDocument { { "UserId", userId }, { "Name", "John Doe" }, { "Age", 30 } };                      mongoCollection.InsertOne(mongoSession, mongoUser);                                            // Commit both transactions                      sqlTransaction.Commit();                      mongoSession.CommitTransaction();                                            Console.WriteLine("Transaction committed successfully.");                  }                  catch (Exception ex)                  {                      // Rollback transactions if any error occurs                      sqlTransaction.Rollback();                      mongoSession.AbortTransaction();                      Console.WriteLine("Transaction rolled back: " + ex.Message);                  }              }          }      }  }
```


### Why TransactionScope can’t be used


` TransactionScope` in C# is designed to work with databases and resources that support *System.Transactions* , such as SQL Server, MSMQ, and some other transactional resource managers. However, **MongoDB does not support *System.Transactions*** , making` TransactionScope` inapplicable for managing transactions across SQL Server and MongoDB.


Additionally,` TransactionScope` relies on the Distributed Transaction Coordinator (DTC) for multi-resource transactions, but MongoDB does not integrate with DTC, which makes it impossible to achieve a fully atomic distributed transaction using` TransactionScope` alone.


*Example for using*` TransactionScope`


***TL;DR: This is for demonstration purposes only. The SQL part will work. MongoDB will not be enlisted in the scope.***


```text
using System;  using System.Transactions;  using System.Data.SqlClient;  using MongoDB.Driver;  using MongoDB.Bson;   class Program  {      static void Main()      {          var sqlConnectionString = "your_sql_connection_string";          var mongoConnectionString = "your_mongo_connection_string";           var mongoClient = new MongoClient(mongoConnectionString);          var mongoDatabase = mongoClient.GetDatabase("YourMongoDbName");          var mongoCollection = mongoDatabase.GetCollection<BsonDocument>("Users");           using (var scope = new TransactionScope(TransactionScopeOption.Required))          {              try              {                  // SQL Operation                  using (var sqlConnection = new SqlConnection(sqlConnectionString))                  {                      sqlConnection.Open();                       var sqlCommand = new SqlCommand("INSERT INTO Users (Name, Age) VALUES (@Name, @Age)", sqlConnection);                      sqlCommand.Parameters.AddWithValue("@Name", "John Doe");                      sqlCommand.Parameters.AddWithValue("@Age", 30);                      sqlCommand.ExecuteNonQuery();                  }                   // MongoDB Operation (not truly part of TransactionScope)                  var userDoc = new BsonDocument                  {                      { "Name", "John Doe" },                      { "Age", 30 }                  };                  mongoCollection.InsertOne(userDoc); // this operation is NOT enlisted                   // Attempt to complete transaction                  scope.Complete();                  Console.WriteLine("Transaction completed.");              }              catch (Exception ex)              {                  Console.WriteLine("Transaction failed: " + ex.Message);                  // No need to call scope.Complete(), it will roll back automatically              }          }      }
```


- MongoDB is not a “resource manager” that supports` System.Transactions` or DTC
- MongoDB doesn’t **enlist** in the ambient transaction created by` TransactionScope` .
- It **does not integrate with DTC** and can’t participate in 2-phase commit protocols that coordinate distributed transactions.
- As a result, even if` TransactionScope` succeeds on the SQL side, **MongoDB operations will execute independently and will not be rolled back** if something goes wrong later.


### When should you use` TransactionScope (distributed transaction)` ?


Only when **all involved resources** support it:


- SQL Server
- MSMQ
- Durable services
- Other DTC-compatible data stores


## Alternative approaches


### 1. Eventual consistency


Eventual consistency ensures that data across multiple databases will become consistent over time, even if temporary inconsistencies exist. Instead of enforcing strict transactional integrity, updates are performed asynchronously, ensuring system availability.


## Get Barak Mordechay’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


**Example:**


- Insert record into SQL Server.
- Use a background job (e.g., Hangfire, Quartz.NET) or an event-driven system to sync data to MongoDB.
- If MongoDB fails, retry until consistency is achieved.


### 2. Compensating transactions


Compensating transactions involve performing an additional corrective action when an operation fails, ensuring that the system returns to a consistent state.


**Example:**


- Insert a record in SQL Server.
- Attempt to insert the corresponding record in MongoDB.
- If MongoDB insert fails, instead of rolling back SQL, mark the SQL record as “Pending Sync” and retry later.


### 3. Event-driven architecture with the outbox pattern


Event-driven architectures rely on messaging systems like Kafka, RabbitMQ, or Azure Service Bus to handle distributed transactions asynchronously. One of the best practices to ensure reliability in event-driven architectures is the **Outbox Pattern** .


### Outbox Pattern overview


Instead of publishing an event immediately after a database operation, the event is first stored in a dedicated **Outbox table** in SQL Server within the same transaction. A separate background process then reads the events from the Outbox and publishes them to the event bus, ensuring **eventual consistency** .


HL Design Outbox pattern


*C# Implementation of Outbox Pattern*


**a. Insert into SQL Server** (including an entry in the Outbox table).


In this step, you store both the primary business data (e.g., a new` User` ) and a corresponding outbox event **in the same SQL transaction** .


```text
using (var connection = new SqlConnection(sqlConnectionString))  {      await connection.OpenAsync();      using (var transaction = connection.BeginTransaction())      {          try          {              // 1. Insert user              var insertUserCmd = new SqlCommand(@"                  INSERT INTO Users (Name, Email)                  VALUES (@Name, @Email);                  SELECT SCOPE_IDENTITY();", connection, transaction);              insertUserCmd.Parameters.AddWithValue("@Name", "John Doe");              insertUserCmd.Parameters.AddWithValue("@Email", "john@example.com");              var userId = Convert.ToInt32(await insertUserCmd.ExecuteScalarAsync());               // 2. Insert event into Outbox              var outboxEvent = new              {                  EventType = "UserCreated",                  Payload = JsonConvert.SerializeObject(new { UserId = userId, Name = "John Doe", Email = "john@example.com" })              };               var insertOutboxCmd = new SqlCommand(@"                  INSERT INTO Outbox (EventType, Payload, CreatedAt, IsProcessed)                  VALUES (@EventType, @Payload, @CreatedAt, 0);", connection, transaction);              insertOutboxCmd.Parameters.AddWithValue("@EventType", outboxEvent.EventType);              insertOutboxCmd.Parameters.AddWithValue("@Payload", outboxEvent.Payload);              insertOutboxCmd.Parameters.AddWithValue("@CreatedAt", DateTime.UtcNow);              await insertOutboxCmd.ExecuteNonQueryAsync();               // 3. Commit transaction              transaction.Commit();          }          catch          {              transaction.Rollback();              throw;          }      }  }
```


**b. Background job reads the Outbox table and publishes the event**


A scheduled job (e.g., using` Hangfire` ,` Quartz.NET` , or a hosted service) reads unprocessed events and sends them to a message broker or directly to MongoDB handler.


```text
var unprocessedEvents = new List<OutboxEvent>();   using (var connection = new SqlConnection(sqlConnectionString))  {      await connection.OpenAsync();       var getEventsCmd = new SqlCommand(@"          SELECT Id, EventType, Payload          FROM Outbox          WHERE IsProcessed = 0;", connection);       using (var reader = await getEventsCmd.ExecuteReaderAsync())      {          while (await reader.ReadAsync())          {              unprocessedEvents.Add(new OutboxEvent              {                  Id = reader.GetInt32(0),                  EventType = reader.GetString(1),                  Payload = reader.GetString(2)              });          }      }  }   // Publish each event  foreach (var evt in unprocessedEvents)  {      try      {          // Send to queue or directly call handler          await eventPublisher.PublishAsync(evt.EventType, evt.Payload);           // Mark as processed          using var markConn = new SqlConnection(sqlConnectionString);          await markConn.OpenAsync();          var markCmd = new SqlCommand("UPDATE Outbox SET IsProcessed = 1 WHERE Id = @Id", markConn);          markCmd.Parameters.AddWithValue("@Id", evt.Id);          await markCmd.ExecuteNonQueryAsync();      }      catch (Exception ex)      {          // Log and skip, to retry later          Console.WriteLine($"Failed to publish event {evt.Id}: {ex.Message}");      }  }
```


c. **A separate microservice or worker listens for this event and inserts data into MongoDB**


Imagine a consumer listening on a message queue (or HTTP endpoint if directly pushed). Here’s a mock of that handler:


```text
public async Task HandleUserCreatedAsync(string payloadJson)  {      var user = JsonConvert.DeserializeObject<UserCreatedEvent>(payloadJson);       var mongoClient = new MongoClient(mongoConnectionString);      var database = mongoClient.GetDatabase("YourDatabase");      var collection = database.GetCollection<BsonDocument>("UserProfiles");       var profileDoc = new BsonDocument      {          { "UserId", user.UserId },          { "Name", user.Name },          { "Email", user.Email },          { "CreatedAt", DateTime.UtcNow }      };       await collection.InsertOneAsync(profileDoc);  }
```


d. **If MongoDB fails, the event remains in the queue for retry**


This behavior is built-in if:


- You **don’t mark the Outbox record as processed** unless MongoDB insert is successful.
- Or if you use a **message broker** (like RabbitMQ, Kafka), they support **retry, dead-lettering** , etc.


Optionally, keep retry logic in your worker:


```text
try  {      await HandleUserCreatedAsync(eventPayload);      // success, mark processed  }  catch (Exception ex)  {      Console.WriteLine($"MongoDB insert failed: {ex.Message}");      // don't mark outbox row as processed -> will retry on next run  }
```


## Summary of flow


1. SQL Insert and Outbox log in a **single transaction** .
2. Background service polls and processes Outbox.
3. Sends to MongoDB (or other consumers).
4. Retries failed ones automatically.


## Conclusion


While implementing a single distributed transaction between SQL Server and MongoDB is challenging due to the lack of native support, alternative strategies like eventual consistency, compensating transactions, and event-driven architectures (with the Outbox Pattern) provide practical solutions.


Choosing the right approach depends on your application’s tolerance for temporary inconsistencies and the need for high availability.
