---
schema_version: "1.0.0"
document_id: "3a9f2c5b1975b2ab52ff5e84568a719a612eee72d5c5c40327c11871aa29a65c"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-06-22-gollum/"
published_at: "2015-06-22T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:37.879348+00:00"
content_hash: "sha256:6796a16bd8fd60eaf3d25689fefb75a6e385b335f38a5b4de7c77ccaa2bce46d"
---

# Introducing Gollum: A NxM message multiplexer written in Go

Here at trivago we write a huge number of log messages every day that need to be stored and monitored. To handle all these messages we created Gollum, a tool that enables us to conveniently send messages from multiple sources to different services. While initially only covering log messages Gollum quickly evolved to a routing framework for all kinds of data. This blogpost is a short introduction to Gollum and how we use it at trivago.


## Once upon a time


Back in the days log events at trivago were sent to a[Scribe](https://github.com/facebookarchive/scribe) server by using a custom tool called Serelog. Serelog was written in C++, collected messages by reading from StdIn or a unix domain socket and was basically not touched during the last couple of years. Over time problems of this approach became apparent. The log server showed more and more i/o load and processing logs evolved to some kind of arcane magic that involved a lot of *grep* , *sed* and *awk* . Solving these problems by scaling Serelog/[Scribe](https://github.com/facebookarchive/scribe) proved to be hard and it soon became obvious that we needed to switch to a different system.


## Taking the log files to Isengard


We started to draw a new architecture to fit our new demands while not breaking the old way of doing things during the transition period. We came up with four major points:


- Logs are to be written to a[Kafka](https://kafka.apache.org/) cluster
- The[Kafka](https://kafka.apache.org/) messages are to be pushed to e.g.[Elasticsearch](https://www.elastic.co/products/elasticsearch) by a background worker
- Other services, like writing to a log file, should follow the same background worker approach
- It must be possible to run the original pipeline in parallel to backup the transition period


We decided to use a new tool for the background worker job. Extending Serelog would have required a full rewrite and we only had a couple of C++ developers at hand. After having a look at tools like[Logstash](http://logstash.net/) we discovered that these were either too heavy weight or too slow for our needs. So we decided to create a new tool and we also decided to write this in Go. There were two main reasons to choose Go over C++:


1. We found Go to be a lot easier to implement and (thus) easier to maintain.
2. Our first tests showed that tools written in Go were able to perform similar to C/C++ based implementations. This was backed up by the possibility to easily fall back to C/C++ for performance critical paths if needed.


## Gollum


Gollum’s source is hosted on[GitHub](https://github.com/trivago/gollum) .
You can find the documentation on[Godoc](https://godoc.org/github.com/trivago/gollum) and at[read the docs](http://gollum.readthedocs.org/en/latest/) .
We decided on the name “Gollum” because of several reasons.


- It is a **MUL** tiplexer for **LOG** s. Just read this backwards.
- It has the word “Go” in it which is of course extremely important
- It has multiple personalities (plugins)
- It talks a lot …
- … and sometimes even to itself (via socket or loopback)
- We even planned to use a ring buffer (which sadly didn’t make it)


## Building Minas Tirith


Gollum’s architecture is plugin based. The main framework covers basic runtime control as well as the initialization and connection of all configured plugins. It also provides many utility functions to implement common tasks like parsing message streams or buffering data in a thread-safe way.


As of this architecture, all “worker” components are extremely modular, completely exchangeable and easy to write. By simply creating a new config file, it is possible to connect all these components for a specific use-case. We like to call this “designing a data stream”.


As Go does not support dynamic libraries, all plugins have to be compiled directly into Gollum. We decided to not work around this by using an external scripting language. Go’s compile times are so fast that improving turn-around times by reloading a script is not an argument. Reloading plugins during runtime is also not required (or even wanted) for most of Gollum’s use-cases. Last but not least native code performs a lot better and API conflicts caused by old plugins are easier to fix.


The Gollum framework gives you a couple of guarantees but also has some important rules:


- Data is passed as bytes, allowing both text and binary messages
- The configuration has to make sure that data is passed in the correct format or converted where necessary
- Runtime is a top priority, i.e. default values prefer the fast lane
- No data is lost while Gollum is running unless the configuration explicitly allows this


## The three towers


There are three major plugin types used by Gollum:


- “Consumers” read data from other services
- ”Producers” write data to other services
- ”Streams” route data between consumers and producers


In addition to that there are three main sub-components:


- A “message” is a set of data passed between consumers and producers
- ”Formatters” can transform the content of messages
- ”Filters” can block/pass messages based on their content


All these components are configured in a single YAML file that is passed as a command line parameter. For example: let’s say you want to read messages from the console (StdIn) and to write them into a file if the word “gollum” appears in the message. Additionally, every message should be encapsulated into XML tags. A configuration for this use-case would look like this:


```text
-   'consumer.Console'  :
Stream  :   'console'


-   'stream.Broadcast'  :
Filter  :   'filter.RegExp'
FilterExpression  :   'gollum'
Formatter  :   'filter.Envelope'
Prefix  :   '<message>'
Postfix  :   '</message>'
Stream  :   'console'


-   'producer.File'  :
Filename  :   'my.log'
Stream  :   'console'
```


First we configure a consumer that reads from StdIn. This is the console consumer. Next we define a stream configuration for the “console” stream the consumer is writing to and set a filter to block all messages not matching a given regular expression. This is followed by an envelope formatter, which adds a start tag and an end tag to all messages passing the filter. Finally we define a file producer, which writes the filtered and modified messages from the console stream to a file called “my.log”.


## One does not simply walk into Mordor


Gollum has a strong emphasis on runtime performance. We wrote a special profiling consumer and used Go’s internal profiling tools to identify and eliminate bottlenecks. To make sure we’re on track, we also compared Gollum’s performance to[Logstash 1.5.0](http://logstash.net/) and[Heka 0.9.0](https://github.com/mozilla-services/heka) . We did not try to get the maximum performance out of every tool but used mostly default values. So please keep in mind that the following results can likely be improved by modifying the configuration of each tool.


```text
Name    OS              CPU                        RAM    HDD
--------------------------------------------------------------------------------------
iMac    MacOSX 10.10.3  1x  Core i5-4570 @3.2 Ghz  16 GB  APPLE SSD SD0128F 120GB
MacPro  MacOSX 10.9.5   1x  Xeon E5-1680 @3.0 Ghz  32 GB  APPLE SSD 1TB
Server  Debian 7.7      32x Xeon E5-2690 @2.9 GHz  64 GB  Raid 1+0: 8xSAS 6G DP 900GB
```


```text
Tool      Worker  Host      msgs/sec  Description
Gollum    * (4)   iMac    ~2.200.000  Theoretic peak performance (memory)
Gollum    * (8)   MacPro  ~2.000.000  [1]
Gollum    * (32)  Server    ~730.000
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Gollum    * (4)   iMac    ~1.100.000  Theoretic peak performance (file)
Gollum    * (8)   MacPro  ~1.200.000  [2]
Gollum    * (32)  Server    ~450.000
Gollum    * (32)  Server     ~22.000  Kafka to File (network-local, parsed)
Logstash  32      Server      ~8.500  [3,5,7]
Heka      32      Server      ~2.000
Gollum    8       Server     ~15.000  Kafka to ElasticSearch (network-network, parsed)
Logstash  8       Server      ~3.200  [4,6]
```


\[1\][Gollum memory](https://gist.github.com/arnecls/e9beaebf40e467f88ce5)


\[2\][Gollum file](https://gist.github.com/arnecls/b0f7cbba01243e2f65dd)


\[3\][Gollum Kafka to file](https://gist.github.com/arnecls/84a7aa67088488a23cdc)


\[4\][Gollum Kafka to Elasticsearch](https://gist.github.com/arnecls/22f687e78e160c1a8cce)


\[5\][Logstash Kafka to file](https://gist.github.com/arnecls/cd8bebfda5e7c2dcc8ab)


\[6\][Logstash Kafka to Elasticsearch](https://gist.github.com/arnecls/80767eb87043a9a68158)


\[7\][Heka Kafka to file](https://gist.github.com/arnecls/080462feae74b4720147)


These tests seem to indicate that Gollum is depending on raw CPU speed more than on the number of Cores. If you take a closer look though, the above tests use only a small number of Go routines and those will probably run on only 1 or 2 threads. For more complex scenarios Gollum will utilize more threads and thus perform better on systems with many cores.


As you can see from the above tests, too, harddisk speed is (of course) an issue for scenarios that need to write to disk. If you don’t do any message processing, and if network speed is not already an issue, the harddisk speed will be the main bottleneck. As of this, Gollum’s file and socket producers already try to mitigate these problems by batching messages into larger, consecutive writes.


## What about a catapult: Logs -> Kafka -> ElasticSearch


Our first use-case shows how to read logs from[Kafka](https://kafka.apache.org/) and how to transfer them to[Elasticsearch](https://www.elastic.co/products/elasticsearch) . The log messages stored in Kafka have been written as plain text and thus have to be converted to JSON before they are sent to[Elasticsearch](https://www.elastic.co/products/elasticsearch) . For debugging reasons we optionally configure a second stream to write log messages to disk without any modifications. This scenario correlates to the profiling results presented above.


```text
-   'consumer.Kafka'  :
Topic  :   'logs'
DefaultOffset  :   'Oldest'
Stream  :
-   'toElastic'
-   'toFile'
Servers  :
-   '10.1.1.2:9092'
-   '10.1.1.3:9092'
-   '10.1.1.4:9092'


-   'stream.Broadcast'  :
Stream  :   'toElastic'
Formatter  :   'format.JSON'
JSONDirectives  :
-   'serverIP       : :    serverName     ::esc'
-   'serverName     : :    forwardedFor   ::esc'
-   'forwardedFor   :, :   forwardedFor   ::arr+esc'
-   'forwardedFor   : :    remoteIP       ::esc+end'
-   'forwardedFor   :- :   remoteIP       ::'
# ... and several more ...


-   'producer.File'  :
Enable  :   true
Formatter  :   'format.Envelope'
File  :   'access.log'
Rotate  :   true
RotateAt  :   '00:00'
Compress  :   true
Stream  :   'toFile'


-   'producer.ElasticSearch'  :
Port  :   9200
DayBasedIndex  :   true
Stream  :   'toElastic'
Servers  :   '10.1.3.225'
Index  :
'toElastic'  :   'acceslog'
```


One interesting detail about this configuration is the JSONDirectives setting. This setting configures a custom, state-machine based parser included in the Gollum framework. We use this parser as a more heap friendly alternative to regular expressions. The parser looks for a token in a string, executes a function on the string read since the last match and moves on to the next state. If you are interested in how this parser works internally, please have a look at the[documentation](http://gollum.readthedocs.org/en/latest/) .


## What about a catapult: Collectd -> Kafka -> InfluxDB


We collect server and application metrics via[collectd](https://collectd.org/) /[statsd](https://github.com/etsy/statsd) . As we are running multiple datacenters, this data is written to a per-datacenter[Kafka](https://kafka.apache.org/) cluster to have fast response times. In a second step each[Kafka](https://kafka.apache.org/) cluster is queried by Gollum to write all metrics to a central InfluxDB server located in our main datacenter.


```text
-   'consumer.Kafka'  :
Topic  :   'stats'
DefaultOffset  :   'Newest'
Stream  :   'stats'
Servers  :
-   '10.1.1.2:9092'
-   '10.1.1.3:9092'


-   'consumer.Kafka'  :
Stream  :   'stats'
# ... same but different servers ...


-   'consumer.Kafka'  :
Stream  :   'stats'
# ... same but different servers ...


-   'trivago.InfluxDBProducer'  :
Host  :   '10.1.1.4:8086'
Database  :   'stats'
Stream  :   'stats'
```


As you might have noticed the InfluxDBProducer does not use the “producer.*” notation. This plugin is currently under development and will be made “official” once the InfluxDB API is marked as stable. As of this, we placed this producer in the “contrib/trivago” folder, which is intended to be used for plugins that are either in development or completely company specific.


## What about a catapult: Traffic duplication


There is a new project at trivago that aims at duplicating traffic from one webserver to one or multiple other servers. We call it “windkanal” (which is German for wind tunnel). This helps us to test, debug or profile our applications using live traffic while not actually being live. In a first step we use a hardware based, one-way mirror port to duplicate traffic from an “A” to a “B” server. Gollum runs a[libpcap](http://www.tcpdump.org/) based listener on the “B” server, reassembles all TCP packets from the A server and forwards the resulting HTTP requests to an arbitrary number of testing servers. As Gollum supports multiple producers of arbitrary type, we can easily write all HTTP requests to a file or even a database, too. To replay the traffic we simply read from such storage and feed the requests directly into the same pipeline. Because of Gollum’s filtering capabilities we can even decide which requests are relevant for the replay.


```text
-   'trivago.PcapHttp'  :
Stream  :   'windkanal'
Interface  :   'eth0'
Port  :   80


-   'producer.HttpRequest'  :
Stream  :   'windkanal'
Address  :   '10.1.1.2:8080'


-   'producer.HttpRequest'  :
Stream  :   'windkanal'
Address  :   '10.1.1.3:8080'


-   'producer.HttpRequest'  :
Stream  :   'windkanal'
Address  :   '10.1.1.4:8080'


-   'producer.File'  :
Enable  :   false
Stream  :   'windkanal'
File  :   'http.dump'
```


## Conclusion


Gollum can be used for all kinds of tasks that involve a proxy and/or multiplexer pattern. This does not only apply to log management but basically to any kind of forwarding task. Two-way communication is possible, too, but concrete implementations are still under development. The overall runtime performance shows to be either matching or to be above similar tools like[Logstash](http://logstash.net/) or[Heka](https://github.com/mozilla-services/heka) . In addition to that Gollum is keeping a relatively low footprint. We found Gollum to be easy to deploy, easy to configure and easy to extend.


## Has Go been a good choice?


Our initial guess on Go being a good alternative to C++ proved to be right. Development speed was fast and we had new developers joining the project without any problems, even if they had never written a single line of Go before. While we had the possibility to fall back to C, we never took use of it as we managed to keep runtime performance high at any time.


Garbage collection showed to be no major problem unless Gollum was under really heavy load. We managed to work around these situations, too, by switching to stack based allocations whenever possible. Even though Go’s escape analysis should do this automatically it makes some seemingly “weird” decisions in some cases. Luckily these problems can be spotted and fixed manually by utilizing Go’s built-in profiler.


Channels and Go routines were a great help to make the whole project scalable. While this could have been done in C++ too, it would have required a lot more time and testing. Deploying Gollum is also a breeze. Compile times are extremely fast and as Go applications have no dependencies, deployment is as easy as copying a file.


On the downside Go still lacks good debugging support. While you can of course use[GDB](http://www.gnu.org/software/gdb/) or[Delve](https://github.com/derekparker/delve) , proper IDEs are either not existing or hard to configure. This made us go back to printf style debugging for a lot of cases. As Go is a rather young language, this will probably improve over the next couple of years and first attempts in this direction look promising.


On the language side there were only a few things that we found to be annoying albeit they probably help keeping compiler/linker times low. Functions cannot be overloaded which means you have to pre- or postfix methods to allow different types. For example:[Math.Min](http://golang.org/pkg/math/#Min) only accepts float64. An integer-based version of this function requires you to manually create a “MinInt” function. This leads to several “almost-duplicate-code” style functions. Directly related to this problem is the lack of generics. While you can work around this by using external tools it kills the otherwise noticed great productivity (for certain situations) and introduces a lot of runtime casting.


The last point of critique is Go’s dependency management. The *go get* command always fetches the latest version, which makes it very prone to API changes. We chose to use[gopkg.in](http://labix.org/gopkg.in) to have at least major version stickiness and[godep](https://github.com/tools/godep) for saving snapshots of all dependencies. With these snapshots we are able to rollback to older tags even if a dependency ceases to exist.


All these problems are well known and heavily discussed inside the Go community. Because of these discussions chances are high that these problems will either be resolved or improved in future versions of Go. All in all we’re pretty happy with our decision and are already planning other Go based projects.
