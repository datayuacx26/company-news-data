---
schema_version: "1.0.0"
document_id: "8eb3c8599c953f87f8be429ebe44ad247a6df14aed1d9bff2a3378f6cac104b4"
company_key: "payoneer-global-inc-common-stock"
company: "Payoneer Global Inc."
source_id: "payoneer-global-inc-common-stock-rss-9fa92a296e72"
canonical_url: "https://engineering.payoneer.com/concurrency-in-c-implementation-of-producer-consumer-pattern-using-blockingcollection-cfee28a9d5c8"
published_at: "2023-12-21T07:01:42+00:00"
first_seen_at: "2026-07-20T23:18:22.924920+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:acb6870aa161126b68e07b67ba59dee4d072fd3b689992695b67cbc512689b6f"
---

# Concurrency in C# : Implementation of producer-consumer pattern using BlockingCollection

# Concurrency in C# : Implementation of producer-consumer pattern using BlockingCollection


[Talalmog](https://medium.com/@talalmog8?source=post_page---byline--cfee28a9d5c8---------------------------------------)


6 min read


·


Dec 21, 2023


--


Press enter or click to view image in full size


## **Introduction**


In the fast-paced realm of modern software development, concurrency stands as a cornerstone concept. Concurrency enables the execution of multiple tasks concurrently, optimizing program performance and responsiveness.


With the ability to distribute work across threads or processes, we can significantly reduce processing time compared to sequential execution. Whether achieved through[processor time slicing](https://www.geeksforgeeks.org/time-slicing-in-cpu-scheduling) **** in a single environment or deploying code across **distributed systems** like containers and servers, effective concurrency maximizes system resource utilization and overall throughput.


At the heart of this intricate landscape lies the **producer-consumer pattern** , a paradigm that resonates with real-world scenarios and encapsulates the essence of concurrent programming.


Having embarked on a journey into this dynamic domain, this article delves into the realm of concurrency, specifically focusing on my personal exploration and implementation of the producer-consumer pattern using C#’s powerful tool, the[BlockingCollection](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.blockingcollection-1) class. By exploring this pattern and the capabilities it offers, we uncover the potential to transform software systems into resilient, efficient, and scalable entities that cater to the ever-evolving demands of modern computing.


## Producer-consumer pattern


Often, in real time scenarios, we could design our concurrent code to follow the producer-consumer pattern, as explained by[Edsger W. Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra) in 1965:


> “We consider two processes, which are called the ‘producer’ and the ‘consumer’ respectively. The producer is a cyclic process and each time it goes through its cycle it produces a certain portion of information, that has to be processed by the consumer. The consumer is also a cyclic process and each time it goes through its cycle, it can process the next portion of information, as has been produced by the producer …”


This idea can be broadened to multiple concurrent producers, and multiple concurrent consumers.


However, this straightforward pattern can produce the most challenging problems concurrent processes can make, (as explained further[here](https://en.wikipedia.org/wiki/Producer%E2%80%93consumer_problem) ).


## ` BlockingCollection`


Fortunately for us, C# has a built in implementation for a collection that can be used for implementing this pattern.


The`[BlockingCollection](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.blockingcollection-1)` class in C# is a part of the` System.Collections.Concurrent` namespace and provides a thread-safe collection that is designed for scenarios where one or more producers generate data or tasks, and one or more consumers consume or process them.


The` BlockingCollection` class combines the functionality of a concurrent collection (such as a`[ConcurrentQueue](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentqueue-1)` or`[ConcurrentStack](https://learn.microsoft.com/en-us/dotnet/api/system.collections.concurrent.concurrentstack-1)` ) with synchronization and blocking capabilities.


## **Demo**


Let’s take a look athow to use the` BlockingCollection` class:


```text
public class Demo  {      private readonly int _publishTimeout;      private readonly BlockingCollection<int> blockingCollection;       public Demo(int bound, int publishTimeout)      {          _publishTimeout = publishTimeout;          blockingCollection = new BlockingCollection<int>(new ConcurrentQueue<int>(), boundedCapacity: bound);      }       // Try produce one item into blockingCollection within specified timeout      public async Task Produce(int item, CancellationToken cancellationToken)      {          await Task.Delay(millisecondsDelay: 10, cancellationToken: cancellationToken); // Simulating some work          bool published = blockingCollection.TryAdd(++item, millisecondsTimeout: _publishTimeout, cancellationToken);           Console.WriteLine(published ? $"Produced item {item}" : $"SKipped item {item}, Collection Is Full");      }       // Consumes one item at a time from the blockingCollection      // while blockingCollection is empty, consumer thread goes to sleep inside blockingCollection.GetConsumingEnumerable()      public async Task Consume(int consumerId, CancellationToken cancellationToken)      {                foreach (int item in blockingCollection.GetConsumingEnumerable())          {              Console.WriteLine($"Consumer {consumerId} processed item {item}");              await Task.Delay(millisecondsDelay: 10, cancellationToken: cancellationToken); // Simulating some work          }      }  }
```


In the example above you can see a basic example of how to implement producer and consumer functions for a` BlockingCollection` using a` ConcurrentQueue` behind the scenes.


## Get Talalmog’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


As you can see, the` BlockingCollection` has **bounding** capabilities that can be activated by specifying the *boundedCapacity* variable in the` BlockingCollection` constructor.


Bounding means you can set the maximum capacity of the collection. This is important in certain scenarios because it enables you to control the maximum size of the collection in memory, and it prevents the producing threads from moving too far ahead of the consuming threads.


Multiple threads or tasks can add items to the collection concurrently, and if the collection reaches its specified maximum capacity, the producing threads will **sleep** until an item is removed.


If you don’t wish to for the producer thread to sleep until there’s space in the collection, use the ‘ *TryAdd* ’ function, specifying a timeout for producing the message and handle cases where the queue is full yourself.


In addition, multiple consumers can remove items concurrently, and if the collection becomes empty, the consuming threads will **sleep** until a producer adds an item.


It’s important to note that when the producer sleeps due to a full collection or the consumer sleeps because of an empty collection, there is no occurrence of busy waiting. This approach minimizes CPU consumption within these threads.


Let's use our demo class:


```text
private static void Main(string[] args)  {      const int concurrentProducers = 20;      const int concurrentConsumers = 20;      const int bound = 10;      const int publishTimeout = 100;       var demo = new Demo(bound, publishTimeout);          // start multiple concurrent producers      var producers = Enumerable.Range(0, concurrentProducers).Select(x => demo.Produce(x, CancellationToken.None));            // start multiple concurrent consumers      var consumers = Enumerable.Range(0, concurrentConsumers).Select(x => demo.Consume(x, CancellationToken.None));       var allTasks = producers.Union(consumers).ToArray();      await Task.WhenAll(allTasks);  } // the program won't reach here since a consumer task doesn't end when it is empty, it sleeps until more messages arrive
```


As you can see, you can control the concurrency level of the producers and consumers, as well as the queue bound, which can give you memory consumption control.


## **My experience with BlockingCollection**


I recently leveraged this class to create an asynchronous logging mechanism. The project demanded the recording of request and response data from two distinct API calls. These datasets handle the[3D Secure](https://en.wikipedia.org/wiki/3-D_Secure) flow of the Payoneer Card product. Our specific requirement was to persist certain data from these API calls in a relational database, for analytical research regarding the[3D Secure](https://en.wikipedia.org/wiki/3-D_Secure) flow.


Considering the nature of the API calls — both being pivotal to complete financial transactions (part of the[3D Secure](https://en.wikipedia.org/wiki/3-D_Secure) flow)—I sought to minimize response time. It was paramount that the client's experience remained seamless, and they wouldn't be left waiting for data to be logged in the database. To address this, I engineered a solution wherein the API call logs were directed to a` BlockingCollection` . This collection was subsequently managed by dedicated consumers responsible for persisting the messages in the database. It’s worth mentioning that we were comfortable with the idea of losing some logs during a process crash or restart.


To augment this architecture, I also employed the mediator pattern. Each message type was systematically consumed by an associated handler, thereby ensuring specialized processing. This mechanism allowed us to funnel diverse messages through the same *BlockingCollection* , paving the way for their eventual storage in different database tables. For the implementation of the mediator pattern, I integrated the[MediatR](https://github.com/jbogard/MediatR) NuGet package.


This is an example for a blocking collection in combination with the mediator to message handlers:


```text
public class LoggerQueue : ILoggerQueue  {      private static readonly ILogger _logger = LogManager.GetCurrentClassLogger();       private readonly IMediator _mediator;       public BlockingCollection<IRequest> Queue { get; }      public int Bound { get; }      public int AddTimeoutMS { get; }       public LoggerQueue(IMediator mediator, IConfiguration configuration)      {          AddTimeoutMS = configuration.GetSection("AppSettings").GetValue<int>("LoggerQueue_AddTimeoutMS");          Bound = configuration.GetSection("AppSettings").GetValue<int>("LoggerQueue_Bound");          Queue = new BlockingCollection<IRequest>(new ConcurrentQueue<IRequest>(), Bound);          _mediator = mediator;      }       public void Add(IRequest message)      {          if(!Queue.TryAdd(message, AddTimeoutMS))              _logger.Warn("Queue flooded, skipping message");      }       public async Task Consume(CancellationToken cancellationToken)      {          foreach (IRequest item in Queue.GetConsumingEnumerable(cancellationToken: cancellationToken))          {              try              {                  // foward each message to it's associated message handler                  await _mediator.Send(item, cancellationToken);               }              catch (Exception ex)              {                  _logger.Error(ex, "Unhandled exception in logger consumer");              }          }      }  }
```


## Choosing between in-memory BlockingCollection and persisted queuing systems


When making the decision between a straightforward in-memory` BlockingCollection` and more complex persisted queuing systems like[Apache Kafka](https://kafka.apache.org/) or[RabbitMQ](https://www.rabbitmq.com/) , certain considerations come into play. Opting for a basic` BlockingCollection` provides a swift and efficient setup for rapid in-memory message routing. Due to its lack of data persistence on disk, a` BlockingCollection` runs the risk of message loss if they remain unconsumed before the application encounters an exit or crash event. In addition, a simple *BlockingCollection* falls short in comparison to advanced features such as **dead letter queuing** , **message expiry/TTL** **(time-to-live)** , **delayed messaging** , etc. These are available in more sophisticated queuing platforms like Apache Kafka or RabbitMQ.


Mastering concurrency is pivotal in modern software development, enabling efficient utilization of system resources and enhancing overall system performance. The producer-consumer pattern, as exemplified by the *BlockingCollection* class in C#, offers a powerful tool to manage concurrent operations seamlessly. By synchronizing producers and consumers within a bounded collection, developers can achieve a delicate balance between high-throughput processing and controlled memory usage.


My personal experience with *BlockingCollection* in implementing a logging mechanism demonstrates its practicality in real-world scenarios, particularly when responsiveness and resource management are critical. Leveraging concurrency constructs such as *BlockingCollection* not only streamlines program execution but also empowers developers to design robust and scalable systems, ultimately elevating the software development process to new heights of efficiency and reliability.
